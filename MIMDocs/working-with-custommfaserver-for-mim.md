---
title: Użyj alternatywnego dostawcy usługi Multi-Factor Authentication za pośrednictwem interfejsu API do aktywacji usługi PAM lub w scenariuszu samoobsługowego resetowania HASEŁ | Dokumentacja firmy Microsoft
description: Ustawienie interfejsu API usługi Custom uwierzytelnianie wieloskładnikowe jako drugą warstwę zabezpieczeń, gdy użytkownicy aktywują role w ramach Privileged Access Management i używać samoobsługowego resetowania hasła.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 7fb111520f94541672fc56d0fd2ee95bfcd3a49e
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690743"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Użyj niestandardowego dostawcę uwierzytelniania Multi-Factor Authentication za pośrednictwem interfejsu API podczas aktywacji roli funkcji PAM lub funkcji samoobsługowego resetowania haseł

Klienci usługi Azure AD Premium lub usługi Azure MFA można zintegrować usługę Azure MFA w przypadku dwóch scenariuszy programu MIM — Privileged Access Management (PAM) aktywacji roli i funkcji samoobsługowego resetowania haseł (SSPR).

Klienci programu MIM są dwie opcje dodatkowe:

 - Użyj niestandardowego jeden haseł dostawcę, który ma zastosowanie tylko w scenariuszu samoobsługowego resetowania HASEŁ programu MIM i udokumentowane w przewodniku, aby [Konfigurowanie samoobsługowego resetowania z hasła, za pomocą brama SMS portalu OTP](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10))
 - Użyj dostawcy telefonii niestandardowe uwierzytelnianie wieloskładnikowe. Ta opcja ma zastosowanie w scenariuszach samoobsługowego resetowania HASEŁ programu MIM i PAM, opisane w tym artykule

W tym artykule opisano sposób używania programu MIM z dostawcą uwierzytelnianie wieloskładnikowe niestandardowe, za pośrednictwem interfejsu API i integracji zestawu SDK opracowanych przez klienta.  

## <a name="prerequisites"></a>Wymagania wstępne

Aby można było używać niestandardowego interfejsu API dostawcy usługi Multi-Factor Authentication z programem MIM, potrzebne są:

- Numery telefonów wszystkich użytkowników kandydujących
- Poprawka programu MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) lub nowsze wersje — zobacz [historię wersji](reference/version-history.md) zapowiedzi
- Usługa MIM skonfigurowane dla funkcji samoobsługowego resetowania HASEŁ lub usługi PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Podejście przy użyciu kodu niestandardowe uwierzytelnianie wieloskładnikowe

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Krok 1: Upewnij się, usługa MIM jest w wersji 4.5.202.0 lub nowszej

Pobierz i zainstaluj poprawkę programu MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) lub nowszej.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Krok 2: Tworzy bibliotekę DLL, która implementuje interfejs IPhoneServiceProvider

Biblioteka DLL musi zawierać klasę, która implementuje trzy metody:

- `InitiateCall`: Usługa MIM spowoduje wywołanie tej metody. Usługa przekazuje identyfikator telefonu numer i żądania jako parametry.  Metoda musi zwracać `PhoneCallStatus` wartość `Pending`, `Success` lub `Failed`.
- `GetCallStatus`: Jeśli podczas wcześniejszego wywołania `initiateCall` zwrócił `Pending`, usługa MIM spowoduje wywołanie tej metody. Ta metoda zwraca też wartość `PhoneCallStatus` wartość `Pending`, `Success` lub `Failed`.
- `GetFailureMessage`: Jeśli poprzednie wywołanie `InitiateCall` lub `GetCallStatus` zwrócił `Failed`, usługa MIM spowoduje wywołanie tej metody. Ta metoda zwraca komunikat diagnostyczny.

Implementacje tych metod musi zapewniać bezpieczeństwo wątkowe, a ponadto wykonania `GetCallStatus` i `GetFailureMessage` nie należy zakładać, będzie można wywołać w tym samym wątku, jak podczas wcześniejszego wywołania `InitiateCall`.

Biblioteki DLL w Store `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` katalogu.

Przykładowy kod, który może być skompilowany przy użyciu programu Visual Studio 2010 lub nowszej.

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
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Krok 3: Kopia zapasowa MfaSettings.xml znajduje się w "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Krok 4. Edytuj plik MfaSettings.xml

Zaktualizuj lub usuń następujące wiersze:

- Usuń/wyczyść wszystkie wiersze z pozycji konfiguracji 

- Zaktualizuj lub Dodaj następujące wiersze do następującego MfaSettings.xml z dostawcą telefon niestandardowy <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Krok 5. Uruchom ponownie usługę programu MIM

Po ponownym uruchomieniu usługi użyj funkcji samoobsługowego resetowania HASEŁ i/lub PAM do weryfikacji funkcjonalności za pomocą niestandardowego dostawcy tożsamości.

> [!NOTE] 
> Aby przywrócić ustawienia Zamień MfaSettings.xml pliku kopii zapasowej utworzonego w kroku 3


## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do usługi Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Co to jest uwierzytelnianie wieloskładnikowe systemu Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Historia wersji programu MIM](./reference/version-history.md)
