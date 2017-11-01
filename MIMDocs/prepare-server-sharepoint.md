---
title: "Konfigurowanie programu SharePoint pod kątem programu Microsoft Identity Manager 2016| Dokumentacja firmy Microsoft"
description: Instalowanie i konfigurowanie programu SharePoint Foundation w celu hostowania strony portalu programu MIM.
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8646620f8f0ea8e1bdea3705e3db8593aa5a464e
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Konfigurowanie serwera zarządzania tożsamościami: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Na przykład:
> - Nazwa kontrolera domeny — **nazwa_serwera_mim**
> - Nazwa domeny — **contoso**
> - Hasło — **Pass@word1**


## <a name="install-sharepoint-foundation-2013-with-sp1"></a>Zainstaluj program **SharePoint Foundation 2013 z dodatkiem SP1**

> [!NOTE]
> Instalator wymaga połączenia internetowego w celu pobrania jego wymagań wstępnych. Jeśli komputer znajduje się w sieci wirtualnej, która nie zapewnia łączności z Internetem, dodaj do komputera dodatkowy interfejs sieciowy pozwalający na łączenie się z Internetem. Interfejs ten można wyłączyć po ukończeniu instalacji.

Wykonaj następujące kroki, aby zainstalować program SharePoint Foundation 2013 SP1. Po zakończeniu instalacji serwer zostanie uruchomiony ponownie.

1.  Uruchom program **PowerShell** jako administrator domeny.

    -   Przejdź do katalogu, do którego rozpakowano program SharePoint.

    -   Wpisz następujące polecenie.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Po zainstalowaniu wymagań wstępnych programu **SharePoint** zainstaluj program **SharePoint Foundation 2013 z dodatkiem SP1**, wpisując następujące polecenie:

    ```
    .\setup.exe
    ```

3.  Wybierz typ kompletnego serwera.

4.  Po zakończeniu instalacji uruchom kreatora.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Uruchom kreatora, aby skonfigurować program SharePoint

Wykonaj kroki określone w **Kreatorze konfiguracji produktów SharePoint**, aby skonfigurować program SharePoint do pracy z programem MIM.

1. Na karcie **Połącz z farmą serwerów** zmień ustawienia, aby utworzyć nową farmę serwerów.

2. Określ ten serwer jako serwer bazy danych dla bazy danych konfiguracji oraz *Contoso\SharePoint* jako konto dostępu do bazy danych używane przez program SharePoint.

3. Utwórz hasło zabezpieczeń farmy.

4. Po zakończeniu przez kreatora konfiguracji zadania konfiguracji o numerze 10 z 10 kliknij przycisk Zakończ. Zostanie otwarta przeglądarka sieci Web.

5. W oknie wyskakującym przeglądarki Internet Explorer zaloguj się na konto *Contoso\Administrator* (lub równoważnym koncie administratora domeny), aby kontynuować.

6. Uruchom kreatora (w aplikacji sieci Web), aby skonfigurować farmę programu SharePoint.

7. Wybierz opcję wykorzystania istniejącego konta zarządzanego (*Contoso\SharePoint*) i kliknij przycisk **Dalej**.

8. W oknie **Tworzenie kolekcji witryn** kliknij przycisk **Pomiń**.  Następnie kliknij przycisk **Zakończ**.

## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Przygotowywanie programu SharePoint do hostowania portalu programu MIM

> [!NOTE]
> Początkowo protokół SSL nie zostanie skonfigurowany. Należy pamiętać o skonfigurowaniu protokołu SSL lub równoważnego przed włączeniem dostępu do tego portalu.

1. Uruchom **powłokę zarządzania programu SharePoint 2013** i uruchom następujący skrypt programu PowerShell, aby utworzyć **aplikację sieci Web programu SharePoint Foundation 2013**.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > Zostanie wyświetlony komunikat ostrzegawczy z informacją, że jest używana metoda uwierzytelniania Windows Classic i powrót z polecenia końcowego może potrwać kilka minut. Po ukończeniu dane wyjściowe będą wskazywać adres URL nowego portalu. Nie zamykaj okna **powłoki zarządzania programu SharePoint 2013**, aby móc odnieść się do niego w przyszłości.

2. Uruchom powłokę zarządzania programu SharePoint 2013 i uruchom następujący skrypt programu PowerShell, aby utworzyć **kolekcję witryn programu SharePoint** skojarzoną z daną aplikacją sieci Web.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Sprawdź, czy wynik zmiennej *CompatibilityLevel* to „14”. Jeśli wynik wynosi „15”, kolekcja witryn nie została utworzona dla wersji 2010 środowiska. Usuń kolekcję witryn i utwórz ją ponownie.

3. Wyłącz **stan wyświetlania po stronie serwera SharePoint** i zadanie programu SharePoint „Zadanie analizy kondycji (godzinowo, czasomierz Microsoft SharePoint Foundation, wszystkie serwery)”, uruchamiając następujące polecenia programu PowerShell w **powłoce zarządzania programu SharePoint 2013**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Na serwerze zarządzania tożsamością otwórz nową kartę przeglądarki sieci Web, przejdź do adresu http://localhost:82/ i zaloguj się na konto *contoso\Administrator*.  Zostanie wyświetlona pusta witryna programu SharePoint o nazwie *MIM Portal*.

    ![Obraz portalu MIM po adresem http://localhost:82/](media/MIM-DeploySP1.png)

5. Skopiuj adres URL, a następnie w przeglądarce Internet Explorer otwórz **Opcje internetowe**, przejdź do **karty Zabezpieczenia**, wybierz opcję **Lokalny intranet** i kliknij opcję **Witryny**.

    ![Obraz opcji internetowych](media/MIM-DeploySP2.png)

6. W oknie **Lokalny intranet** kliknij pozycję **Zaawansowane** i wklej skopiowany adres URL w polu tekstowym **Dodaj tę witrynę do strefy**. Kliknij przycisk **Dodaj**, a następnie zamknij okna.

7. Otwórz program **Narzędzia administracyjne**, przejdź do karty **Usługi**, odszukaj usługę administracji programu SharePoint i uruchom ją, jeśli nie jest jeszcze uruchomiona.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)
