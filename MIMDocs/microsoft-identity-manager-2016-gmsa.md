---
title: Konwersja określonych usług programu MIM na gMSA | Dokumentacja firmy Microsoft
description: Temat opisujący podstawowe kroki, aby skonfigurować gMSA.
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.openlocfilehash: d719b771e8db514d45b15c21b60460cc821e872e
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333228"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Konwersja określonych usług programu MIM na gMSA

Ten przewodnik będzie przejść przez podstawowe kroki, aby skonfigurować konta gMSA dla obsługiwanych usług. Po wstępnym skonfigurowaniu środowiska, proces, aby przekonwertować gMSA jest proste.

Poprawki wymagane: \<link, aby najnowsze KB\>

Obsługiwane:

-   Service(FIMSynchronizationService) synchronizacji programu MIM
-   Service(FIMService) programu MIM
-   Rejestracji haseł programu MIM
-   Resetowanie hasła programu MIM
-   Service(PamMonitoringService) monitorowania usługi PAM
-   Usługa składnika PAM (PrivilegeManagementComponentService)

Nieobsługiwane:

-   Portal programu MIM nie jest obsługiwana jest częścią środowiska programu sharepoint i konieczne będzie wdrożenie w trybie farmy i [skonfigurować automatyczną zmianę hasła w SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Wszyscy agenci zarządzania
-   Zarządzanie certyfikatami firmy Microsoft
-   BHOLD

<a name="general-information"></a>Informacje ogólne 
--------------------

Odczytywanie potrzebne do ukończenia instalacji i zrozumienia

-   [Omówienie kont usług zarządzanych przez grupę](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Pierwszym krokiem na kontrolerze domeny systemu windows

1.  Jeśli to konieczne, należy utworzyć klucz główny Services(KDS) klucza dystrybucji (tylko raz w każdej domenie). Klucz główny jest używany przez usługę KDS na kontrolerach domeny (wraz z innymi informacjami) do wygenerowania hasła.

    -   Dodaj KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately" oznacza, że oczekiwania maksymalnie \~10 godzin / musi zostać zreplikowana na wszystkie kontrolery domeny. Jest to około 1 godziny dla dwóch kontrolerów domeny.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Usługa synchronizacji
-----------------------

1.  Utwórz grupę o nazwie "MIMSync_Servers" i Dodaj wszystkie serwery synchronizacji do tej grupy.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Z systemu windows PowerShell, następnie wykonaj poniższe polecenie jako administrator domeny przy użyciu konta komputera już przyłączone do domeny

    -   New-ADServiceAccount — nazwa MIMSyncGMSAsvc - DNSHostName MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Pobierz szczegóły GSMA do celów synchronizacji:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Jeśli uruchomiona usługa PCNS, musisz zaktualizować delegowanie

    -   Set-ADServiceAccount-Identity MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Następnie podczas synchronizacji usługi upewnij się utworzyć kopię zapasową klucza szyfrowania, jak go będzie wymagane podczas zmiany trybu instalacji

    -   Na serwerze, który usługa synchronizacji jest zainstalowana na zlokalizuj narzędzia Zarządzanie kluczami usługi synchronizacji

    -   Domyślnie ** eksportu klucza zestawu ** jest już wybrany

    -   Kliknij pozycję **dalej**

    -   Użytkownik będzie teraz monit o wprowadzenie istniejące informacje o koncie synchronizacji

    -   Wprowadź i sprawdź informacje o koncie synchronizacji FIM

        -   Nazwa konta - Nazwa konta usługi synchronizacji używanego podczas początkowej instalacji

        -   Hasło — hasło dla konta usługi synchronizacji

        -   Domena - domeny od siebie z konta usługi synchronizacji

    -   Kliknij pozycję **dalej**

    -   Jeśli coś niepoprawnie, zostanie wyświetlony następujący błąd

    -   Teraz zostały pomyślnie wprowadzone informacje o koncie, zostaną wyświetlone z opcją Zmień lokalizację docelową (lokalizacja pliku eksportu), klucza szyfrowania kopii zapasowych

        -   Domyślnie jest lokalizacja pliku eksportu **C:\\Windows\\system32**\\miiskeys 1.bin.

4. Zainstaluj usługi synchronizacji programu Microsoft Identity Manager z dodatkiem SP1, kompilację 4.4.1302.0. Możesz z nich można znaleźć w Centrum pobierania licencji woluminu lub Centrum pobierania MSDN. Po zakończeniu instalacji upewnij się, Zapisz miiskeys.bin zestawu kluczy.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Zainstaluj najnowsze [4.5.x.x poprawkę](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) lub nowszej.

- Raz poprawek Zatrzymaj synchronizacji FIM service.
- Programy w Panelu sterowania i funkcji programu Microsoft Identity Manager
- Usługa synchronizacji zmiana -\> dalej -\> Konfiguruj -\> dalej

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Wyczyść nazwę konta
-  Nazwa konta usługi typu **MIMSyncGMSA** z \$ symboli na dzień
- Zrzut ekranu. Hasło należy pozostawić pusty.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Dalej -\> dalej -\> instalacji
- Przywracanie kluczy z zapisać plik bin.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> Uprawnienia SQL dodawane jest konto zostanie utworzone dla nazwy logowania w związku z tym musisz zezwolić na użytkownika, stosując Zmień tryb uprawnienie do dodawania konta i dbo w bazie danych usługi synchronizacji

## <a name="mim-service"></a>Usługa MIM
-----------

>[!IMPORTANT]
>Podczas konwertowania najpierw usługę MIM powiązane konta jako konta gMSA, należy użyć następującego procesu. Polecenia cmdlet programu PowerShell w dodatku należy używać tylko zmienić informacje o koncie po początkowej konfiguracji done.*

1.  Tworzenie zarządzanych przez grupę Portal resetowania konta usługi programu MIM, interfejsu API Rest usługi PAM, Usługa monitorowania PAM, usługa składnika PAM, Portal rejestracji samoobsługowego resetowania HASEŁ, Samoobsługowe Resetowanie HASEŁ.

    -   Upewnij się, że nazwy SPN i delegowania gMSA
        -   Set-ADServiceAccount-Identity \<konta\> - ServicePrincipalNames \@{Dodaj = "\<SPN\>"}
        -   Delegowanie
            -   Set-ADServiceAccount-Identity \<gsmaaccount\> - TrustedForDelegation \$true
        -   Delegowanie ograniczone
            -   \$delspns = "http/mim", "http/mim.contoso.com"
            -   New-ADServiceAccount — nazwa \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<grupy\> - ServicePrincipalNames \$nazwy SPN - OtherAttributes \@{"msDS-AllowedToDelegateTo" =\$delspns}

2.  Dodaj konto usługi programu MIM w grupy synchronizacji. Jest to niezbędne do samoobsługowego resetowania HASEŁ.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **UWAGA**.  Znany problem, który usług korzystających z kontem zarządzanym zawieszanie się po ponownym uruchomieniu serwera z powodu usługi dystrybucji kluczy firmy Microsoft nie jest uruchomiona po ponownym uruchomieniu Windows. Nie można uruchomić usługi, a Windows mogła nie zostać ponownie uruchomiona zbyt. Problem polega powtarzalne, co najmniej w systemie Windows Server 2012 R2. Obejście tego problemu jest uruchomienie polecenia 

-   **kdssvc triggerinfo sc start/networkon**

    uruchomienie usługi dystrybucji kluczy firmy Microsoft znajduje się w sieci (zazwyczaj wcześniej cykl rozruchu).

    Zobacz Omówienie podobny problem: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Uruchom z podwyższonym poziomem uprawnień tożsamości usługi Zarządzanej usługi MIM i wybierz polecenie Zmień.

5.  Na "Konfiguruj serwer główny połączenia page" zaznacz wyboru "Użyj innego konta dla programu Exchange (dla kont zarządzanych)". W tym miejscu masz opcję, aby użyć starego konta, który ma skrzynkę pocztową lub skrzynki pocztowej w chmurze.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Na koncie usługi typu "Konta usługi programu MIM Konfiguruj" strony z \$ symbolu na końcu. Również wpisać hasło do konta usługi poczty E-mail. Hasło do konta usługi powinny być wyłączone.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Jak funkcja funkcji LogonUser nie działa dla kont zarządzanych, Następna strona będzie ostrzeżenie ". Sprawdź, czy konto usługi jest zabezpieczone w bieżącej konfiguracji".

![CID:image007.PNG\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Na stronie "Konfigurowanie uprzywilejowanego dostępu do interfejsu API REST zarządzania" wpisz nazwę konta puli aplikacji za pomocą \$ symbolu na końcu i pozostaw puste pole hasła.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Na stronie "Konfigurowanie usługi PAM składnik usługi" wpisz nazwę konta usługi z \$ symbolu na końcu i pozostaw puste pole hasła.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.PNG\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Na stronie "Konfigurowanie uprzywilejowanego dostępu do zarządzania usługi monitorowania", wpisz nazwę konta usługi z \$ symbolu na końcu i pozostaw puste pole hasła.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Na stronie "Portalu rejestracji haseł ze konfigurowania programu MIM" wpisz nazwę konta z \$ symbolu na końcu i pozostaw puste pole hasła.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Na stronie "Konfigurowanie programu MIM Portal resetowania haseł" wpisz nazwę konta z \$ symbolu na końcu i pozostaw puste pole hasła.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Ukończenie instalacji.

Uwaga:

-  Po zakończeniu instalacji dwa nowe klucze są tworzone w rejestrze przy użyciu ścieżki
    - "HKEY_LOCAL_MACHINE\\oprogramowania\\Microsoft\\Forefront Identity
    - Menedżer\\2010\\usługi "do przechowywania zaszyfrowane hasła programu Exchange. Jeden dla
    - Exchange Online i inny wpis dla programu Exchange (jeden z nich powinien być lokalne
    - pusty).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Aby zaktualizować hasło, zamieszczone skrypt [pobrać tutaj](microsoft-identity-manager-2016-gmsascript.md) więc klienta nie będzie musiał uruchomić Zmień tryb

- Do szyfrowania Exchange hasło Instalator tworzy dodatkowe usługi i
    - działa na koncie zarządzanym. Następujące komunikaty zostaną dodane w
    - Dziennik zdarzeń aplikacji podczas instalacji.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
