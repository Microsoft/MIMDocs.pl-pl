---
title: Użyj usługi Azure Serwer Multi-Factor Authentication, aby aktywować scenariusze PAM lub SSPR | Microsoft Docs
description: Skonfiguruj Serwer Multi-Factor Authentication platformy Azure jako drugą warstwę zabezpieczeń, gdy użytkownicy aktywują role w Privileged Access Management i samoobsługowego resetowania hasła.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 1dd87db01a5c1100c8206d82bedf96a8a5e702ad
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044314"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Korzystanie z usługi Azure Serwer Multi-Factor Authentication w celu aktywowania PAM lub SSPR
W poniższym dokumencie opisano sposób konfigurowania serwera usługi Azure MFA jako drugiej warstwy zabezpieczeń, gdy użytkownicy aktywują role w Privileged Access Management lub samoobsługowego resetowania hasła.

> [!IMPORTANT]
> Ze względu na powiadomienie o zaniechaniu korzystania z usługi Azure Multi-Factor Authentication Software Development Kit, zestaw SDK usługi Azure MFA będzie obsługiwany dla istniejących klientów aż do daty wycofania 14 listopada 2018. Nowi klienci i obecni klienci nie będą mogli pobrać zestawu SDK już za pośrednictwem klasycznego portalu Azure. Aby pobrać pakiet poświadczeń usługi MFA, należy skontaktować się z działem pomocy technicznej platformy Azure.

W poniższym artykule opisano aktualizację konfiguracji i kroki umożliwiające przejście z zestawu Azure MFA SDK do usługi Azure Serwer Multi-Factor Authentication.

## <a name="prerequisites"></a>Wymagania wstępne

Aby można było korzystać z usługi Azure Serwer Multi-Factor Authentication z programem MIM, potrzebne są:

- Dostęp do Internetu z każdej usługi MIM lub serwera MFA dostarczającego moduły PAM i SSPR, aby skontaktować się z usługą Azure MFA
- Subskrypcja platformy Azure
- Instalacja już korzysta z zestawu Azure MFA SDK
- Licencje usługi Azure Active Directory Premium dla użytkowników kandydujących lub alternatywne metody licencjonowania usługi Azure MFA
- Numery telefonów wszystkich użytkowników kandydujących
- Poprawka programu MIM 4,5. lub więcej zobacz [historię wersji](./reference/version-history.md) dla anonsów

## <a name="azure-multi-factor-authentication-server-configuration"></a>Konfiguracja usługi Azure Serwer Multi-Factor Authentication 
> [!NOTE] 
> W konfiguracji jest wymagany prawidłowy certyfikat SSL zainstalowany dla zestawu SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Krok 1. Pobieranie Serwer Multi-Factor Authentication platformy Azure z poziomu witryny Azure Portal 
Zaloguj się do [Azure Portal](https://portal.azure.com/) i Pobierz serwer usługi Azure MFA.
![Praca z mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Krok 2. Generowanie poświadczeń aktywacji
Użyj linku **Generuj poświadczenia aktywacji, aby zainicjować korzystanie** z poświadczeń aktywacji. Po wygenerowaniu Zapisz do późniejszego użycia.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Krok 3. Instalowanie usługi Azure Serwer Multi-Factor Authentication
Po pobraniu serwera [Zainstaluj](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) go.  Twoje poświadczenia aktywacji będą wymagane. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Krok 4. Tworzenie aplikacji sieci Web usług IIS, która będzie hostować zestaw SDK
1. Otwórz Menedżera ![usług IIS pracuję z mfaserver-for-mim_iis. Format](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Utwórz nowe wywołanie witryny sieci Web "MIM MFASDK", połącz je z pustym katalogiem ![działającym z-mfaserver-for-mim_sdkweb. Format](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Otwórz konsolę Multi-Factor Authentication i kliknij pozycję zestaw SDK ![usługi sieci Web działające w systemie-mfaserver-for-mim_sdkinstall. Format](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Po kliknięciu kreatora w obszarze Konfiguracja wybierz pozycję "MIM MFASDK" i pulę aplikacji

> [!NOTE] 
> Kreator będzie wymagał utworzenia grupy administratorów. Więcej informacji można znaleźć w dokumentacji usługi Azure > > MFA na platformie Azure Serwer Multi-Factor Authentication.

5. Następnie musimy zaimportować konto usługi MIM otwarte Multi-Factor Authentication konsoli wybierz "Użytkownicy" a. Kliknij pozycję "Importuj z Active Directory" b. Przejdź do konta usługi alias "contoso\mimservice" c. Kliknij pozycję "Importuj" i "Zamknij ![" działa-WITH-mfaserver-for-mim_importmimserviceaccount. Format](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Edytuj konto usługi programu MIM, aby włączyć w konsoli ![Multi-Factor Authentication działającej z mfaserver-for-mim_enableserviceaccount. Format](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Zaktualizuj uwierzytelnianie usług IIS w witrynie sieci Web "MIM MFASDK". Najpierw wyłączymy "Uwierzytelnianie anonimowe", a następnie Włącz uwierzytelnianie ![systemu Windows "działa-WITH-mfaserver-for-mim_iisconfig. Format](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Ostatni krok: Dodaj konto usługi programu MIM do "PhoneFactor administratorów" ![działające z-mfaserver-for-mim_addservicetomfaadmin. Format](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Konfigurowanie usługi MIM dla platformy Azure Serwer Multi-Factor Authentication 

### <a name="step-1-patch-server-to-452020"></a>Krok 1. serwer poprawek do 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Krok 2. Tworzenie kopii zapasowej i otwieranie pliku pliku mfasettings. XML znajdującego się w folderze "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Krok 3. aktualizowanie następujących wierszy
1. Usuń/Wyczyść następujące wiersze pozycji konfiguracji <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Zaktualizuj lub Dodaj następujące wiersze do poniższego polecenia, aby pliku mfasettings. XML <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Uruchom ponownie usługę MIM i funkcję testowania za pomocą usługi Azure Serwer Multi-Factor Authentication.

> [!NOTE] 
> Aby przywrócić ustawienie Zamień pliku mfasettings. XML na plik kopii zapasowej w kroku 2


## <a name="see-also"></a>Zobacz także

-    [Wprowadzenie do serwera Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Co to jest platforma Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Korzystanie z niestandardowego interfejsu API Multi-Factor Authentication w celu aktywowania usługi PAM lub SSPR](Working-with-custommfaserver-for-mim.md)
- [Historia wersji programu MIM](./reference/version-history.md)
