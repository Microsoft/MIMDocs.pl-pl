---
title: Aktywacji funkcji PAM lub scenariuszy funkcji samoobsługowego resetowania HASEŁ za pomocą zestawu SDK usługi Azure Multi-Factor Authentication Server | Dokumentacja firmy Microsoft
description: Konfigurowanie zestawu SDK usługi Azure Multi-Factor Authentication Server jako drugą warstwę zabezpieczeń, gdy użytkownicy aktywują role w Privileged Access Management i samoobsługowego resetowania hasła.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 69b7f8f4b94f9f94b2aef6afd9573ad8173e148e
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379798"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Aktywacji funkcji PAM lub funkcji samoobsługowego resetowania HASEŁ za pomocą usługi Azure Multi-Factor Authentication
Ten dokument zawiera opis sposobu konfiguracji serwera Azure MFA jako drugiej warstwy zabezpieczeń, gdy użytkownicy aktywują role w uprawnienia dostępu do zarządzania lub samoobsługowego resetowania hasła.

> [!IMPORTANT]
> Ze względu na ogłoszenie wycofywania z usługi Azure Multi-Factor Authentication uwierzytelnianie Software Development Kit. Zestaw SDK usługi Azure MFA będą obsługiwane w przypadku istniejących klientów do dnia wycofania 14 listopada 2018 r. Nowych klientów i obecni klienci nie będzie można już pobrać zestaw SDK, za pośrednictwem klasycznego portalu Azure. Możesz pobrać będzie konieczne skontaktowanie się z obsługą klienta platformy Azure do odbierania wygenerowanego pakietu poświadczenia usługi MFA. <br> Zespół projektowy Microsoft pracuje nad zmiany MFA dzięki integracji z zestawem SDK usługi Azure Multi-Factor Authentication Server.

Poniższy artykuł opisują aktualizację konfiguracji i czynności, aby umożliwić prostego przełącznika z usługi Azure MFA z zestawu SDK do usługi Azure Multi-Factor Authentication uwierzytelnianie serwera SDK wydana jako to zostaną uwzględnione w nadchodzącej poprawce zobacz [Historia wersji ](./reference/version-history.md) dla anonsów. 

## <a name="prerequisites"></a>Wymagania wstępne

Aby można było używać usługi Azure Multi-Factor Authentication z programem MIM, potrzebne są:

- Dostęp do Internetu z każdej usługi MIM lub serwer usługi MFA, zapewniając PAM i samoobsługowego resetowania HASEŁ, skontaktuj się z usługą Azure MFA
- Subskrypcja platformy Azure
- Instalacja jest już przy użyciu zestawu SDK usługi Azure MFA
- Licencje usługi Azure Active Directory Premium dla użytkowników kandydujących lub alternatywne metody licencjonowania usługi Azure MFA
- Numery telefonów wszystkich użytkowników kandydujących
- Poprawka programu MIM 4.5. lub zobacz większy [historię wersji](./reference/version-history.md) zapowiedzi

## <a name="azure-multi-factor-authentication-server-configuration"></a>Konfiguracja serwera usługi Azure Multi-Factor Authentication 
> [!NOTE] 
> W konfiguracji należy ważnego certyfikatu SSL zainstalowany zestaw SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Krok 1: Pobierz serwer Azure Multi-Factor Authentication w witrynie azure portal 
Zaloguj się do [witryny Azure portal](https://portal.azure.com/) i pobrać serwer Azure MFA.
![pracy — z mfaserver do mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Krok 2: Generuj poświadczenia aktywacji
Użyj **Generuj poświadczenia aktywacji, aby zainicjować użycie** łącze do Generuj poświadczenia aktywacji. Po wygenerowaniu Zapisz do późniejszego użycia.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Krok 3: Instalowanie serwera Azure Multi-Factor Authentication
Po pobraniu serwera, [zainstalować](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) go.  Wymagane będzie swoje poświadczenia aktywacji. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Krok 4: Tworzenie aplikacji sieci Web usług IIS, który będzie hostował zestawu SDK
1. Otwórz Menedżera usług IIS ![pracy — z mfaserver do mim_iis. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Utwórz nowe wywołanie z witryny sieci Web "MIM MFASDK", połączyć pusty katalog ![pracy — z mfaserver do mim_sdkweb. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Otwórz konsolę uwierzytelniania wieloskładnikowego, a następnie wybierz polecenie Web Service SDK ![pracy — z mfaserver do mim_sdkinstall. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Raz kreatorów klikaj elementy konfiguracji, wybierz opcję "MIM MFASDK" i puli aplikacji

> [!NOTE] 
> Kreator będzie wymagać grupy administratorów, ma zostać utworzony. Więcej informacji znajduje się na platformie Azure >> dokumentacji serwera MFA Azure Multi-Factor Authentication.

5. Następnie musimy importowanie usługi MIM konta Otwórz konsolę uwierzytelniania wieloskładnikowego wybierz "Użytkownicy". Kliknij pozycję "Importuj z usługi Active Directory" b. Przejdź do konta usługi alias c "contoso\mimservice". Kliknij pozycję "Importuj" i "Zamknąć" ![pracy — z mfaserver do mim_importmimserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Edytuj konto usługi programu MIM, aby włączyć w konsoli uwierzytelniania wieloskładnikowego ![pracy — z mfaserver do mim_enableserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Zaktualizuj uwierzytelnianie usług IIS w witrynie sieci Web "MIM MFASDK". Najpierw zostanie wyłączona "Uwierzytelnianie anonimowe", a następnie Włącz uwierzytelnianie Windows" ![pracy — z mfaserver do mim_iisconfig. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Ostatni krok: Dodaj konto usługi programu MIM do "PhoneFactor Admins" ![pracy — z mfaserver do mim_addservicetomfaadmin. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Konfigurowanie usługi MIM na potrzeby serwera Azure Multi-Factor Authentication 

### <a name="step-1-patch-server-to-452020"></a>Krok 1: Poprawki serwerowi 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Krok 2: Kopia zapasowa i otwórz MfaSettings.xml znajduje się w "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Krok 3: Zaktualizuj następujące wiersze
1. Usuń/Clear następujące wiersze pozycji konfiguracji <br>
&LT; KLUCZ_LICENCJI &GT;&LT; / KLUCZ_LICENCJI &GT;<br>
&LT; GROUP_KEY &GT;&LT; / GROUP_KEY &GT;<br>
&LT; CERT_PASSWORD &GT;&LT; / CERT_PASSWORD &GT;<br>
<CertFilePath></CertFilePath><br>

2. Zaktualizuj lub Dodaj następujące wiersze do następującego MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Uruchom ponownie usługę programu MIM i przetestować funkcje przy użyciu serwera Azure Multi-Factor Authentication.

> [!NOTE] 
> Aby przywrócić ustawienia Zamień MfaSettings.xml pliku kopii zapasowej utworzonego w kroku 2


## <a name="next-steps"></a>Następne kroki

-    [Wprowadzenie do usługi Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Co to jest uwierzytelnianie wieloskładnikowe systemu Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Aktywacji funkcji PAM lub funkcji samoobsługowego resetowania HASEŁ za pomocą interfejsu API usługi Custom Multi-Factor Authentication](Working-with-custommfaserver-for-mim.md)
- [Historia wersji programu MIM](./reference/version-history.md)
