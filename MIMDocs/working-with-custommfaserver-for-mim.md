---
title: Użyj alternatywnego dostawcy Multi-Factor Authentication za pośrednictwem interfejsu API w celu aktywowania usługi PAM lub w scenariuszu SSPR | Microsoft Docs
description: Skonfiguruj niestandardowy interfejs API usługi MFA jako drugą warstwę zabezpieczeń, gdy użytkownicy aktywują role w Privileged Access Management i korzystają z funkcji samoobsługowego resetowania hasła.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: b157b2a8716d20ce3b472d5655d393e64f2baa6b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044365"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Używanie niestandardowego dostawcy Multi-Factor Authentication za pośrednictwem interfejsu API podczas aktywacji roli PAM lub w SSPR

Klienci Azure AD — wersja Premium lub Azure MFA mogą zintegrować usługę Azure MFA w dwóch scenariuszach programu MIM — aktywacja roli Privileged Access Management (PAM) i samoobsługowego resetowania hasła (SSPR).

Klienci programu MIM mają dwie dodatkowe opcje:

 - Użyj niestandardowego dostawcy dostarczania jednorazowych haseł, który jest stosowany tylko w scenariuszu programu MIM SSPR i udokumentowany w przewodniku [konfigurowania funkcji samoobsługowego resetowania haseł za pomocą bramy SMS OTP](https://docs.microsoft.com/previous-versions/mim/hh824692(v=ws.10))
 - Użyj niestandardowego dostawcy usługi telefonii usługi uwierzytelniania wieloskładnikowego. Dotyczy to zarówno scenariuszy SSPR i PAM programu MIM, opisanych w tym artykule.

W tym artykule opisano sposób korzystania z programu MIM z niestandardowym dostawcą usługi uwierzytelniania wieloskładnikowego za pośrednictwem interfejsu API i zestawu integracyjnego integracji opracowanego przez klienta.  

## <a name="prerequisites"></a>Wymagania wstępne

Aby można było używać niestandardowego interfejsu API dostawcy Multi-Factor Authentication z programem MIM, potrzebne są:

- Numery telefonów wszystkich użytkowników kandydujących
- Poprawka programu MIM 4.5.202.0 lub nowsza — zobacz [historię wersji](reference/version-history.md) dla anonsów
- Usługa MIM skonfigurowana dla SSPR lub PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Podejście przy użyciu niestandardowego kodu uwierzytelniania wieloskładnikowego

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Krok 1. upewnienie się, że usługa MIM jest w wersji 4.5.202.0 lub nowszej

Pobierz i zainstaluj poprawkę programu MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) lub nowszą wersję.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Krok 2. Tworzenie biblioteki DLL implementującej interfejs IPhoneServiceProvider

Biblioteka DLL musi zawierać klasę, która implementuje trzy metody:

- `InitiateCall`: usługa MIM wywoła tę metodę. Usługa przekazuje numer telefonu i identyfikator żądania jako parametry.  Metoda musi zwracać `PhoneCallStatus` wartość `Pending`, `Success` lub `Failed`.
- `GetCallStatus`: Jeśli starsze wywołanie `initiateCall` zwróciło `Pending`, usługa MIM wywoła tę metodę. Ta metoda zwraca również wartość `PhoneCallStatus` `Pending`, `Success` lub `Failed`.
- `GetFailureMessage`: Jeśli poprzednie wywołanie `InitiateCall` lub `GetCallStatus` zwróciło `Failed`, usługa MIM wywoła tę metodę. Ta metoda zwraca komunikat diagnostyczny.

Implementacje tych metod muszą być bezpieczne wątkowo, a ponadto implementacja `GetCallStatus` i `GetFailureMessage` nie może przyjąć, że zostaną wywołane przez ten sam wątek, który jest wcześniejszym wywołaniem do `InitiateCall`.

Zapisz bibliotekę DLL w katalogu `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\`.

Przykładowy kod, który można skompilować przy użyciu programu Visual Studio 2010 lub nowszego.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Krok 3. Tworzenie kopii zapasowej pliku pliku mfasettings. XML znajdującego się w folderze "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Krok 4: edytowanie pliku pliku mfasettings. XML

Zaktualizuj lub wyczyść następujące wiersze:

- Usuń/Wyczyść wszystkie wiersze wpisów konfiguracji 

- Zaktualizuj lub Dodaj następujące wiersze do pliku pliku mfasettings. XML z niestandardowym dostawcą telefonu <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Krok 5. ponowne uruchomienie usługi MIM

Po ponownym uruchomieniu usługi należy użyć SSPR i/lub PAM do zweryfikowania funkcjonalności z niestandardowym dostawcą tożsamości.

> [!NOTE] 
> Aby przywrócić ustawienie Zamień pliku mfasettings. XML na plik kopii zapasowej w kroku 3


## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do usługi Azure Serwer Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Co to jest platforma Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Historia wersji programu MIM](./reference/version-history.md)
