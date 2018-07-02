---
title: Konwersja określone usługi MIM na gMSA | Dokumentacja firmy Microsoft
description: Temat opisujący podstawowe kroki, aby skonfigurować usługi zarządzane przez grupę.
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.openlocfilehash: 090e82cac6c734beb9767e2d2e6230320e44c26f
ms.sourcegitcommit: c6cb2556bb9f2256b959a3c95db7ca5bbfc2b437
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37065146"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Konwersja określone usługi MIM na grupę

W tym przewodniku będzie krokowo podstawowe kroki, aby skonfigurować grupę usług obsługiwanych. Proces do przekonwertowania na grupę jest proste, gdy wstępne skonfigurowanie środowiska.

Poprawki wymagane: \<łącze do najnowszej KB\>

Obsługiwane:

-   Service(FIMSynchronizationService) synchronizacji programu MIM
-   MIM Service(FIMService)
-   Rejestracji haseł programu MIM
-   Resetowanie hasła MIM
-   Service(PamMonitoringService) monitorowania PAM
-   Usługa składnika PAM (PrivilegeManagementComponentService)

Nie jest obsługiwana:

-   Portal programu MIM nie jest obsługiwana jest częścią środowiska programu sharepoint i będzie potrzebny do wdrożenia w trybie farmy i [skonfigurować automatyczne zmienianie hasła w SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Wszyscy agenci zarządzania
-   Zarządzanie certyfikatami firmy Microsoft
-   BHOLD

<a name="general-information"></a>Informacje ogólne 
--------------------

Odczytywanie wymagana do zakończenia instalacji i zrozumieć

-   [Omówienie kont usług zarządzanych przez grupę](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/en-us/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/en-us/library/jj128430(v=ws.11).aspx>

Krok fist na kontrolerze domeny systemu windows

1.  Jeśli to konieczne, należy utworzyć klucz główny Services(KDS) klucza dystrybucji (tylko raz w każdej domenie). Klucz główny jest używany przez usługę KDS na kontrolery domeny (wraz z innymi informacjami) do generowania haseł.

    -   Dodaj KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately" oznacza poczekaj maksymalnie \~10 godzin / musi zostać zreplikowana na wszystkie kontrolery domeny. To około 1 godzina na dwóch kontrolerach domeny.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Usługa synchronizacji
-----------------------

1.  Krok fist tworzy wywołanie grupy "MIMSync_Servers" i dodać wszystkie serwery synchronizacji do tej grupy.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  W środowisku windows PowerShell, wykonujących poniżej polecenia jako administrator domeny konta na komputerze już przyłączony do domeny

    -   New-ADServiceAccount-Name - DNSHostName MIMSyncGMSAsvc MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Uzyskiwanie szczegółowych informacji o GSMA synchronizacji:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Jeśli usługi PCNS, należy zaktualizować delegowanie

    -   Set-ADServiceAccount-Identity MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Obok na synchronizacji usługi należy utworzyć kopię zapasową klucza szyfrowania, jak będzie wymagane podczas instalacji w trybie zmiany

    -   Na serwerze zainstalowano usługi synchronizacji na zlokalizuj narzędzia synchronizacji usługi Key Management

    -   Domyślnie ** eksportu klucza zestawu ** jest już wybrana

    -   Polecenie **dalej**

    -   Wprowadź informacje dotyczące istniejącego konta synchronizacji zostanie być monitowani

    -   Wpisz i Zweryfikuj informacje o koncie synchronizacji FIM

        -   Nazwa konta — Nazwa konta usługi synchronizacji używanego podczas początkowej instalacji

        -   Hasło — hasło do konta usługi synchronizacji

        -   Domena - domeny, która jest związane z konta usługi synchronizacji

    -   Polecenie **dalej**

    -   Jeśli wprowadzono coś nieprawidłowo, zostanie wyświetlony następujący błąd

    -   Po pomyślnym wprowadzeniu informacji o koncie, zostaną wyświetlone z opcją Zmienianie miejsca docelowego (lokalizacja pliku eksportu) klucz szyfrowania kopii zapasowych

        -   Domyślna lokalizacja pliku eksportu jest **C:\\Windows\\system32**\\miiskeys 1.bin.

4. Instalacja Microsoft Identity Manager z dodatkiem SP1 usługi synchronizacji kompilacji 4.4.1302.0. Możesz można znaleźć w Centrum pobierania licencji woluminu lub pobrać witrynę MSDN. Po zakończeniu instalacji upewnij się, Zapisz miiskeys.bin kluczy.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Zainstaluj najnowsze [4.5.x.x poprawkę](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/version-history) lub nowszym.

- Raz poprawiono synchronizacji FIM zatrzymania usługi.
- Programy w Panelu sterowania i funkcji programu Microsoft Identity Manager
- Usługa synchronizacji zmiany -\> dalej -\> Konfiguruj -\> dalej

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Wyczyść pole wyboru Nazwa konta
-  Nazwa konta usługi typu **MIMSyncGMSA** z \$ symboli na dzień
- Zrzut ekranu. Hasło może pozostać puste.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Next —\> dalej -\> instalacji
- Przywracanie kluczy z zapisany plik bin.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> Uprawnienia SQL dodawane jest konto jest tworzone dla nazwy logowania w związku z tym musisz zezwolić na użytkownika stosowania zmiany trybu uprawnienia do dodawania konta "i" dbo w bazie danych usługi synchronizacji

## <a name="mim-service"></a>Usługa MIM
-----------

>[!IMPORTANT]
>Podczas konwertowania najpierw usługę MIM dotyczących konta jako konta gMSA, należy użyć następującego procesu. Polecenia cmdlet programu PowerShell w dodatku tylko pozwala zmienić informacje o koncie done.* Po początkowej konfiguracji

1.  Tworzenie grupy zarządzane konta usługi programu MIM, Rest PAM interfejsu API, Usługa monitorowania PAM, usługa składnika PAM, Portal rejestracji SSPR, SSPR Portal resetowania.

    -   Upewnij się, że należy zaktualizować delegowanie przez grupę i nazw SPN
        -   Set-ADServiceAccount-Identity \<konta\> - ServicePrincipalNames \@{Dodaj = "\<SPN\>"}
        -   Delegowanie
            -   Set-ADServiceAccount-Identity \<gsmaaccount\> - TrustedForDelegation \$true
        -   Delegowanie ograniczone
            -   \$delspns = "http/mim", "http/mim.contoso.com"
            -   New-ADServiceAccount-Name \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<grupy\> - ServicePrincipalNames \$SPN - OtherAttributes \@{"msDS-AllowedToDelegateTo" =\$delspns}

2.  Dodaj konto usługi MIM w grupach synchronizacji. Konieczne jest dla usługi SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **UWAGA**.  Znany problem, który usług korzystających z kontem zarządzanym zawieszenie po ponownym uruchomieniu serwera z powodu usługi dystrybucji kluczy firmy Microsoft nie została uruchomiona po ponownym uruchomieniu systemu Windows. Nie można uruchomić usługi i systemu Windows nie można ponownie uruchomić zbyt. Problem jest odtworzenia co najmniej systemu Windows Server 2012 R2. Obejście tego problemu jest Uruchom polecenie 

-   **kdssvc triggerinfo sc start/networkon**

    można uruchomić Usługa dystrybucji kluczy firmy Microsoft, gdy znajduje się w sieci (zazwyczaj wczesne w cyklu rozruchu).

    Zobacz Omówienie podobnego problemu: <https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Uruchom z podwyższonym poziomem uprawnień MSI usługi MIM i wybierz zmiany.

5.  Na "Strony połączenia serwera głównego konfiguracji" wyboru "Użyj innego konta dla programu Exchange (dla kont zarządzanych)". W tym miejscu należy opcję, aby użyć stare konto, które ma skrzynkę pocztową lub skrzynki pocztowej w chmurze.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Na koncie usługi typu "Konfigurowanie usługi MIM konto" strony z \$ symbol na końcu. Również wpisz hasło do konta usługi poczty E-mail. Hasło konta usługi powinny być wyłączone.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Jak funkcja funkcji LogonUser nie działa w przypadku zarządzanych kont, Następna strona zostanie ostrzeżenie "Sprawdź, czy konto usługi jest zabezpieczone w bieżącej konfiguracji".

![CID:image007.PNG\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Na stronie "Konfigurowanie uprzywilejowanego dostępu interfejs API REST zarządzania", wpisz nazwę konta puli aplikacji z \$ symboli na końcu i pozostaw puste pole hasła.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Na stronie "Skonfiguruj usługę składnika usługi PAM" wpisz nazwę konta usługi z \$ symboli na końcu i pozostaw puste pole hasła.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.PNG\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Na stronie "Konfigurowanie uprzywilejowanego dostępu do zarządzania usługi monitorowania" wpisz nazwę konta usługi z \$ symboli na końcu i pozostaw puste pole hasła.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Na stronie "Portalu rejestracji haseł programu skonfigurować MIM" wpisz nazwę konta z \$ symboli na końcu i pozostaw puste pole hasła.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Na stronie "Konfigurowanie portalu resetowania haseł MIM" wpisz nazwę konta z \$ symboli na końcu i pozostaw puste pole hasła.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Zakończenie instalacji.

Uwaga:

-  Po zakończeniu instalacji dwa nowe klucze są tworzone w rejestrze za pomocą ścieżki
    - "HKEY_LOCAL_MACHINE\\oprogramowania\\Microsoft\\Forefront Identity
    - Menedżer\\2010\\usługi "do przechowywania zaszyfrowane hasło programu Exchange. Jeden dla
    - Exchange Online i drugi dla programu Exchange lokalnie (jeden z nich powinien być
    - pusta).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Aby zaktualizować hasło, podaliśmy skryptu [Pobierz tutaj](microsoft-identity-manager-2016-gmsascript.md) tak klienta nie będzie musiał uruchomić Zmień tryb

- Do szyfrowania Exchange hasła Instalator tworzy dodatkowych usług i
    - uruchamia go przy użyciu zarządzanego konta. Następujące komunikaty zostaną dodane w
    - Dziennik zdarzeń aplikacji, podczas instalacji.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
