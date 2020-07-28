---
title: Konwertowanie usług specyficznych dla Microsoft Identity Manager na gMSA | Microsoft Docs
description: W tym artykule przedstawiono wymagania wstępne i podstawowe kroki konfigurowania konta usługi zarządzanego przez grupę (gMSA).
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 03/10/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 5985ded45a53a804728572404fb0db43e988ac1d
ms.sourcegitcommit: f87be3d09cee6a8880b3a6babf32e0d064fde36b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87176765"
---
# <a name="convert-microsoft-identity-manager-specific-services-to-use-group-managed-service-accounts"></a>Konwertowanie usług specyficznych dla Microsoft Identity Manager na korzystanie z kont usług zarządzanych przez grupę

Ten artykuł zawiera Przewodnik konfigurowania obsługiwanych Microsoft Identity Manager usług w celu korzystania z kont usług zarządzanych przez grupę (gMSA). Po wstępnej konfiguracji środowiska proces konwersji do gMSA jest łatwy.

## <a name="prerequisites"></a>Wymagania wstępne

- Pobierz i zainstaluj następującą wymaganą poprawkę: [Microsoft Identity Manager 4.5.26.0 lub nowszą](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history).

    Obsługiwane usługi:

    -   Usługa synchronizacji Microsoft Identity Manager (FIMSynchronizationService)
    -   Usługa Microsoft Identity Manager (usługi FIMService)
    -   Rejestracja hasła Microsoft Identity Manager
    -   Microsoft Identity Manager resetowania hasła
    -   Usługa monitorowania Privileged Access Management (PAM) (PamMonitoringService)
    -   Usługa składnika PAM (PrivilegeManagementComponentService)

    Nieobsługiwane usługi:

    -   Portal Microsoft Identity Manager nie jest obsługiwany. Jest częścią środowiska programu SharePoint i należy wdrożyć ją w trybie farmy i [skonfigurować automatyczną zmianę hasła w programie SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
    -   Wszyscy agenci zarządzania z wyjątkiem Microsoft Identity Manager agenta zarządzania usługami
    -   Zarządzanie certyfikatami firmy Microsoft
    -   BHOLD

- Aby uzyskać ogólne informacje dotyczące konfigurowania środowiska, zobacz: 

    -   [Omówienie kont usług zarządzanych przez grupę](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)  
    -   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)  

- Przed rozpoczęciem [Utwórz klucz główny usługi dystrybucji kluczy](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx) na kontrolerze domeny systemu Windows. Weź pod uwagę następujące informacje:  

    - Klucze główne są używane przez usługę dystrybucji kluczy (KDS) do generowania haseł i innych informacji na kontrolerach domeny.
    - Utwórz klucz główny tylko raz dla każdej domeny, jeśli jest to konieczne.  
    - Uwzględnij `Add-KDSRootKey –EffectiveImmediately` . "– EffectiveImmediately" oznacza, że replikacja klucza głównego do wszystkich kontrolerów domeny może potrwać do 10 godzin. Replikacja do dwóch kontrolerów domeny może potrwać około 1 godzinę. 
    ![Ciąg "– EffectiveImmediately"](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="actions-to-run-on-the-active-directory-domain-controller"></a>Akcje do uruchomienia na kontrolerze domena usługi Active Directory

1.  Utwórz grupę o nazwie *MIMSync_Servers*i Dodaj do niej wszystkie serwery synchronizacji.

    ![Utwórz grupę MIMSync_Servers](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

1.  Zaloguj się do programu Windows PowerShell jako *administrator domeny* przy użyciu konta, które jest już przyłączone do domeny, a następnie uruchom następujące polecenie: 

    `New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"`

    ![Polecenie w programie PowerShell](media/f6deb0664553e01bcc6b746314a11388.png)

    - Wyświetl szczegóły gMSA do synchronizacji:  
     ![Szczegóły gMSA do synchronizacji](media/c80b0a7ed11588b3fb93e6977b384be4.png)

    - Jeśli używasz usługi powiadamiania o zmianie hasła (PCNS), Zaktualizuj delegowanie, uruchamiając następujące polecenie:

        `Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames
        @{Add="PCNSCLNT/mimsync.contoso.com"}`

## <a name="actions-to-run-on-the-microsoft-identity-manager-synchronization-server"></a>Akcje do uruchomienia na serwerze synchronizacji Microsoft Identity Manager

1. W Synchronization Service Manager wykonaj kopię zapasową klucza szyfrowania. Zostanie ona zażądana z instalacją trybu zmiany. Wykonaj następujące czynności:

    a. Na serwerze, na którym zainstalowano Synchronization Service Manager, należy poszukać narzędzia do zarządzania kluczami usługi synchronizacji. **Zestaw kluczy eksportu**   jest już zaznaczony domyślnie.

    b. Wybierz pozycję **Dalej**. 
    
    c. W wierszu polecenia wprowadź i sprawdź informacje o koncie usługi synchronizacji Microsoft Identity Manager lub Forefront Identity Manager (FIM):

    -   **Nazwa konta**: nazwa konta usługi synchronizacji, która jest używana podczas początkowej instalacji.  
    -   **Password (hasło**): hasło konta usługi synchronizacji.  
    -   **Domena**: domena, do której należy konto usługi synchronizacji.

    d. Wybierz pozycję **Dalej**.

    W przypadku pomyślnego wprowadzenia informacji o koncie można zmienić miejsce docelowe lub wyeksportować lokalizację pliku kopii zapasowej klucza szyfrowania. Domyślnie lokalizacją pliku eksportu jest *C:\Windows\system32\miiskeys-1.bin*.

1. Zainstaluj program Microsoft Identity Manager SP1, który można znaleźć w witrynie Volume Licensing Service Center lub witryny pliki do pobrania w witrynie MSDN. Po zakończeniu instalacji Zapisz zestaw kluczy *miiskeys. bin*.

   ![Okno postępu instalacji usługi synchronizacji Microsoft Identity Manager](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)

1. Zainstaluj [poprawkę 4.5.2.6.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) lub nowszą.

1. Po zainstalowaniu poprawki Zatrzymaj usługę synchronizacji programu FIM, wykonując następujące czynności:

   a. W panelu sterowania wybierz pozycję **programy i funkcje**  >  **Microsoft Identity Manager**.  
   b. Na stronie **usługa synchronizacji** wybierz pozycję **Zmień**  >  **dalej**.  
   c. W oknie **Opcje konserwacji** wybierz pozycję **Konfiguruj**.

   ![Okno Opcje konserwacji](media/dc98c011bec13a33b229a0e792b78404.png)

   d. W oknie **Konfigurowanie usługi synchronizacji Microsoft Identity Manager** Wyczyść wartość domyślną w polu **konto usługi** , a następnie wprowadź **MIMSyncGMSA $**. Pamiętaj, aby dołączyć symbol znaku dolara ($), jak pokazano na poniższej ilustracji. Pozostaw puste pole **hasła** .

   ![Okno Konfigurowanie usługi synchronizacji Microsoft Identity Manager](media/38df9369bf13e1c3066a49ed20e09041.png)

   e. Wybierz kolejno pozycje **dalej**  >  **dalej**  >  **Zainstaluj**.  
   f. Przywróć zestaw kluczy z pliku *miiskeys. bin* , który został zapisany wcześniej.

   ![Opcja przywracania konfiguracji zestawu kluczy](media/44cd474323584feb6d8b48b80cfceb9b.png)

   ![Lista agentów zarządzania w Synchronization Service Manager](media/03e7762f34750365e963f0b90e43717c.png)

## <a name="microsoft-identity-manager-service"></a>Usługa Microsoft Identity Manager

>[!IMPORTANT]
>Należy uważnie postępować zgodnie z instrukcjami w tej sekcji w przypadku konwertowania Microsoft Identity Manager kont związanych z usługą na konta gMSA.

1. Utwórz konta zarządzane przez grupę dla usług Microsoft Identity Manager, interfejs API REST usługi PAM, usługa monitorowania PAM, usługa składnika PAM, Portal rejestracji funkcji samoobsługowego resetowania haseł (SSPR) i Portal resetowania SSPR.

    -   Aktualizowanie delegowania gMSA i głównej nazwy usługi (SPN):

        - `Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames
            @{Add="\<SPN\>"}`
    -   Wierz

        - `Set-ADServiceAccount -Identity \<gmsaaccount\>
                -TrustedForDelegation $true`
    -   Delegowanie ograniczone:
        -   `$delspns = 'http/mim', 'http/mim.contoso.com'`
        -   `New-ADServiceAccount -Name \<gmsaaccount\> -DNSHostName
                \<gmsaaccount\>.contoso.com
                -PrincipalsAllowedToRetrieveManagedPassword \<group\>
                -ServicePrincipalNames $spns -OtherAttributes
                @{'msDS-AllowedToDelegateTo'=$delspns }`

1. Dodaj konto dla usługi Microsoft Identity Manager w grupach synchronizacji. Ten krok jest niezbędny w przypadku SSPR.

    ![Okno Active Directory Użytkownicy i komputery](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

    > [!NOTE]  
    > Znany problem w systemie Windows Server 2012 R2 polega na tym, że usługi korzystające z zarządzanego konta przestają odpowiadać po ponownym uruchomieniu serwera, ponieważ usługa dystrybucji kluczy firmy Microsoft nie jest uruchomiona po uruchomieniu systemu Windows. Aby obejść ten problem, należy uruchomić następujące polecenie: 
    >
    > `sc triggerinfo kdssvc start/networkon`
    >
    > Polecenie uruchamia usługę dystrybucji kluczy firmy Microsoft, gdy sieć jest włączona (zazwyczaj na wczesnym etapie cyklu rozruchu).
    >
    > Aby zapoznać się z omówieniem podobnego problemu, zobacz [AD FS Windows 2012 R2: adfssrv zawiesza się w trybie uruchamiania](https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS).

1.  Uruchom plik MSI z podwyższonym poziomem uprawnień usługi Microsoft Identity Manager, a następnie wybierz pozycję **Zmień**.

1.  W oknie **Konfigurowanie połączenia z serwerem poczty** zaznacz pole wyboru **Użyj innego użytkownika dla programu Exchange (dla kont zarządzanych)** . Masz możliwość użycia bieżącego konta programu Exchange lub skrzynki pocztowej w chmurze.

    >[!NOTE]
    >W przypadku wybrania opcji **Użyj usługi Exchange Online** , aby umożliwić usłudze Microsoft Identity Manager przetwarzać odpowiedzi zatwierdzenia z dodatku Microsoft Identity Manager Outlook, ustaw wartość klucza rejestru **HKLM\SYSTEM\CurrentControlSet\Services\FIMService** *PollExchangeEnabled* na **1** po instalacji.
    
    ![Okno "Konfigurowanie połączenia z serwerem poczty"](media/0cd8ce521ed7945c43bef6100f8eb222.png)

1.  W oknie dialogowym **Konfigurowanie konta usługi programu MIM** wpisz nazwę w polu **nazwa konta usługi** . Pamiętaj, aby dołączyć symbol znaku dolara ($). Wprowadź również hasło w polu **hasło do konta usługi poczty e-mail** . Pole **hasła konta usługi** powinno być niedostępne.

    ![Okno "Konfigurowanie konta usługi MIM"](media/db0d543df6e1b0174a47135617c23fcb.png)

    Ponieważ funkcja funkcji LogonUser nie działa dla kont zarządzanych, Następna strona wyświetla ostrzeżenie "Sprawdź, czy konto usługi jest bezpieczne w bieżącej konfiguracji".

    ![Okno ostrzeżenia o zabezpieczeniach konta](media/d350bc13751b2d0a884620db072ed019.png)

1.  W oknie **konfigurowanie Privileged Access Management interfejsu API REST** w polu **nazwa konta puli aplikacji** wprowadź nazwę konta. Pamiętaj, aby dołączyć symbol znaku dolara ($). Pozostaw puste pole **hasła konta puli aplikacji** .

    ![Okno Konfigurowanie interfejsu API REST usługi Privileged Access Management](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

1.  W oknie **Konfigurowanie usługi składnika PAM** w polu **nazwa konta usługi** wprowadź nazwę konta. Pamiętaj, aby dołączyć symbol znaku dolara ($). Pozostaw puste pole **hasło do konta usługi** .

    ![Okno Konfigurowanie usługi składnika PAM](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

    ![Okno ostrzeżenia o zabezpieczeniach konta](media/9d2b52f6faed10601e7e2166a339fb47.png)

1.  W oknie **konfigurowanie Privileged Access Management monitorowania usługi** w polu **nazwa konta usługi** wpisz nazwę konta usługi. Pamiętaj, aby dołączyć symbol znaku dolara ($). Pozostaw puste pole **hasło do konta usługi** .

    ![Okno Konfigurowanie usługi monitorowania Privileged Access Management](media/d1e824248edf12a77fc9ffb011475164.png)

1.  W oknie dialogowym **Konfigurowanie portalu rejestracji haseł programu MIM** w polu **nazwa konta** wprowadź nazwę konta. Pamiętaj, aby dołączyć symbol znaku dolara ($). Pozostaw puste pole **hasła** .

    ![Okno Konfigurowanie portalu rejestracji haseł programu MIM](media/601e935cdfda298b61ae753a2a152996.png)

1.  W oknie dialogowym **Konfigurowanie portalu resetowania haseł programu MIM** w polu **nazwa konta** wprowadź nazwę konta. Pamiętaj, aby dołączyć symbol znaku dolara ($). Pozostaw puste pole **hasła** .

    ![Okno Konfigurowanie portalu resetowania haseł programu MIM](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

1.  Ukończ instalację.

    > [!NOTE]
    > Podczas instalacji w ścieżce rejestru tworzone są dwa nowe klucze **HKEY_LOCAL_MACHINE \Software\microsoft\forefront Identity Manager\2010\Service** do przechowywania szyfrowanego hasła programu Exchange. Jeden wpis jest przeznaczony dla *ExchangeOnline*, a drugi to *ExchangeOnPremise*. W przypadku jednego z wpisów wartość w kolumnie **dane** powinna być pusta.

    > ![Edytor rejestru](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

Aby zaktualizować hasło do przechowywanych kont bez konieczności uruchamiania trybu zmiany, [Pobierz ten skrypt programu PowerShell](microsoft-identity-manager-2016-gmsascript.md).

W celu zaszyfrowania hasła programu Exchange Instalator tworzy dodatkową usługę i uruchamia ją na koncie zarządzanym. Podczas instalacji w dzienniku zdarzeń **aplikacji** dodawane są następujące komunikaty:

![Okno Podgląd zdarzeń](media/95b315454705cd4d939b55ac5ad910f5.jpg)
