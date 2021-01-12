---
title: Użyj usługi Azure Serwer Multi-Factor Authentication, aby aktywować scenariusze PAM lub SSPR | Microsoft Docs
description: Skonfiguruj Serwer Multi-Factor Authentication platformy Azure jako drugą warstwę zabezpieczeń, gdy użytkownicy aktywują role w Privileged Access Management i samoobsługowego resetowania hasła.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 1/5/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 68f8ae0f64e136add96b3abb5083331912a0efe7
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104109"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Korzystanie z usługi Azure Serwer Multi-Factor Authentication w celu aktywowania PAM lub SSPR
W poniższym dokumencie opisano sposób konfigurowania serwera usługi Azure MFA jako drugiej warstwy zabezpieczeń, gdy użytkownicy aktywują role w Privileged Access Management lub Self-Service resetowania hasła.

> [!IMPORTANT]
> Z powodu wycofania zestawu SDK (Software Development Kit) usługi Azure Multi-Factor Authentication (MFA) klienci nie będą mogli już pobrać zestawu SDK usługi Azure MFA.

W poniższym artykule opisano aktualizację konfiguracji i kroki umożliwiające przejście z zestawu Azure MFA SDK na serwer usługi Azure MFA.

## <a name="prerequisites"></a>Wymagania wstępne

Aby można było korzystać z usługi Azure Serwer Multi-Factor Authentication z programem MIM, potrzebne są:

- Dostęp do Internetu z każdej usługi MIM lub serwera MFA dostarczającego moduły PAM i SSPR, aby skontaktować się z usługą Azure MFA
- Subskrypcja platformy Azure
- Instalacja już korzysta z zestawu Azure MFA SDK
- Azure Active Directory — wersja Premium licencji dla użytkowników kandydujących
- Numery telefonów wszystkich użytkowników kandydujących
- Poprawka programu MIM 4,5. lub więcej zobacz [historię wersji](./reference/version-history.md) dla anonsów

## <a name="azure-multi-factor-authentication-server-configuration"></a>Konfiguracja usługi Azure Serwer Multi-Factor Authentication 
> [!NOTE] 
> W konfiguracji jest wymagany prawidłowy certyfikat SSL zainstalowany dla zestawu SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Krok 1. Pobieranie Serwer Multi-Factor Authentication platformy Azure z Azure Portal 
Zaloguj się do [Azure Portal](https://portal.azure.com/) i Pobierz serwer usługi Azure MFA.
![Praca z mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Krok 2. Generowanie poświadczeń aktywacji
Użyj linku **Generuj poświadczenia aktywacji, aby zainicjować korzystanie** z poświadczeń aktywacji. Po wygenerowaniu Zapisz go do późniejszego użycia.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Krok 3. Instalowanie usługi Azure Serwer Multi-Factor Authentication
Po pobraniu serwera [Zainstaluj](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) go.  Twoje poświadczenia aktywacji będą wymagane. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Krok 4. Tworzenie aplikacji sieci Web usług IIS, która będzie hostować zestaw SDK
1. Otwórzworking-with-mfaserver-for-mim_iis.PNGMenedżera usług IIS ![](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Utwórz nowe wywołanie witryny sieci Web "MIM MFASDK", połącz je z pustym katalogiem ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Otwórz konsolę Multi-Factor Authentication i kliknij pozycję zestaw SDK usługi sieci Web ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Po kliknięciu kreatora w obszarze Konfiguracja wybierz pozycję "MIM MFASDK" i pulę aplikacji

> [!NOTE] 
> Kreator będzie wymagał utworzenia grupy administratorów. Więcej informacji można znaleźć w dokumentacji usługi Azure > > MFA na platformie Azure Serwer Multi-Factor Authentication.

5. Następnie musimy zaimportować konto usługi MIM otwarte Multi-Factor Authentication konsoli wybierz "Użytkownicy" a. Kliknij pozycję "Importuj z Active Directory" b. Przejdź do konta usługi, na przykład "contoso\mimservice" c. Kliknij przycisk "Importuj" i "Zamknij" ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Edytuj konto usługi programu MIM, aby włączyć w konsoli Multi-Factor Authentication ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Zaktualizuj uwierzytelnianie usług IIS w witrynie sieci Web "MIM MFASDK". Najpierw wyłącz "Uwierzytelnianie anonimowe", a następnie Włącz uwierzytelnianie systemu Windows " ![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Ostatni krok: Dodaj konto usługi programu MIM doworking-with-mfaserver-for-mim_addservicetomfaadmin.PNG"Administratorzy PhoneFactor" ![](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Konfigurowanie usługi MIM dla platformy Azure Serwer Multi-Factor Authentication 

### <a name="step-1-patch-server-to-452020"></a>Krok 1. serwer poprawek do 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Krok 2. Tworzenie kopii zapasowej i otwieranie MfaSettings.xml znajdującej się w folderze "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Krok 3. aktualizowanie następujących wierszy
1. Usuń/Wyczyść następujące wiersze pozycji konfiguracji <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Zaktualizuj lub Dodaj następujące wiersze do poniższego, aby MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Uruchom ponownie usługę MIM i funkcję testowania za pomocą usługi Azure Serwer Multi-Factor Authentication.

> [!NOTE] 
> Aby przywrócić ustawienie Zamień MfaSettings.xml na plik kopii zapasowej w kroku 2


## <a name="see-also"></a>Zobacz też

-    [Wprowadzenie do serwera Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Co to jest platforma Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Korzystanie z niestandardowego interfejsu API Multi-Factor Authentication w celu aktywowania usługi PAM lub SSPR](Working-with-custommfaserver-for-mim.md)
- [Historia wersji programu MIM](./reference/version-history.md)
