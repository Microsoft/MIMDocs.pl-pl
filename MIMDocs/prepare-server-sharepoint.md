---
title: Konfigurowanie programu SharePoint pod kątem programu Microsoft Identity Manager 2016| Dokumentacja firmy Microsoft
description: Instalowanie i konfigurowanie programu SharePoint Foundation w celu hostowania strony portalu programu MIM.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: eceb1ed31b0212970d5cf0eae0bc8d96aa087ff5
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Konfigurowanie serwera zarządzania tożsamościami: SharePoint

>[!div class="step-by-step"]
[«Programu SQL Server 2016](prepare-server-sql2016.md)
[Exchange Server»](prepare-server-exchange.md)

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **corpdc**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi MIM — **corpservice**
> - Nazwa serwera synchronizacji MIM — **corpsync**
> - Nazwa programu SQL Server — **corpsql**
> - Hasło — **Pass@word1**


## <a name="install-sharepoint-2016"></a>Zainstaluj **programu SharePoint 2016**

> [!NOTE]
> Instalator wymaga połączenia internetowego w celu pobrania jego wymagań wstępnych. Jeśli komputer znajduje się w sieci wirtualnej, która nie zapewnia łączności z Internetem, dodaj do komputera dodatkowy interfejs sieciowy pozwalający na łączenie się z Internetem. Interfejs ten można wyłączyć po ukończeniu instalacji.

Wykonaj następujące kroki, aby zainstalować program SharePoint 2016. Po zakończeniu instalacji serwer zostanie uruchomiony ponownie.

1.  Uruchom **PowerShell** jako konto domeny z administratora lokalnego na **corpservice** i **sysadmin** na serwerze bazy danych SQL, będziemy używać limit **contoso\ miminstall**.

    -   Przejdź do katalogu, do którego rozpakowano program SharePoint.

    -   Wpisz następujące polecenie.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Po **SharePoint** wymagania wstępne są zainstalowane, należy zainstalować **programu SharePoint 2016** , wpisując następujące polecenie:

    ```
    .\setup.exe
    ```

3.  Wybierz typ kompletnego serwera.

4.  Po zakończeniu instalacji uruchom kreatora.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Uruchom kreatora, aby skonfigurować program SharePoint

Wykonaj kroki określone w **Kreatorze konfiguracji produktów SharePoint**, aby skonfigurować program SharePoint do pracy z programem MIM.

1. Na karcie **Połącz z farmą serwerów** zmień ustawienia, aby utworzyć nową farmę serwerów.

2. Określ ten serwer jako serwer bazy danych, takich jak **corpsql** bazy danych konfiguracji i *Contoso\SharePoint* jako konta dostępu do bazy danych programu SharePoint do użycia.
    a. W Kreatorze konfiguracji zalecane jest wybranie opcji [MinRole](https://docs.microsoft.com/en-us/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) typu **frontonu**
3. Utwórz hasło zabezpieczeń farmy.

4. Po zakończeniu przez kreatora konfiguracji zadania konfiguracji o numerze 10 z 10 kliknij przycisk Zakończ. Zostanie otwarta przeglądarka sieci Web.

5. W menu podręcznym programu Internet Explorer, Uwierzytelnij się jako *Contoso\miminstall* (lub równoważne administratora), aby kontynuować.

6. W Kreatorze sieci web (w aplikacji sieci web) kliknij **Anuluj/Skip**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Przygotowywanie programu SharePoint do hostowania portalu programu MIM

> [!NOTE]
> Początkowo protokół SSL nie zostanie skonfigurowany. Należy pamiętać o skonfigurowaniu protokołu SSL lub równoważnego przed włączeniem dostępu do tego portalu.

1. Uruchom **Powłoka zarządzania programu SharePoint 2016** i uruchom następujący skrypt programu PowerShell, aby utworzyć **aplikacji sieci Web programu SharePoint 2016**.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Zostanie wyświetlony komunikat ostrzegawczy z informacją, że jest używana metoda uwierzytelniania Windows Classic i powrót z polecenia końcowego może potrwać kilka minut. Po ukończeniu dane wyjściowe będą wskazywać adres URL nowego portalu. Zachowaj **Powłoka zarządzania programu SharePoint 2016** okna otwarte dla odwołania później.

2. Uruchom powłokę zarządzania programu SharePoint 2013 i uruchom następujący skrypt programu PowerShell, aby utworzyć **kolekcję witryn programu SharePoint** skojarzoną z daną aplikacją sieci Web.

  ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Sprawdź, czy wynik *CompatibilityLevel* zmiennej wynosi "15". Jeśli wynik jest inne niż "15", następnie kolekcja witryn nie utworzono wersji poprawne środowisko; Usuń kolekcję witryn i utwórz ją ponownie.

3. Wyłącz **stan wyświetlania po stronie serwera SharePoint** i zadanie programu SharePoint "Zadanie analizy kondycji (godzinowo, czasomierz Microsoft SharePoint Foundation, wszystkie serwery)" za pomocą programu PowerShell następujące polecenia w  **Powłoka zarządzania programu SharePoint 2016**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Na serwerze zarządzania tożsamością Otwórz nową kartę przeglądarki sieci web, przejdź do http://mim.contoso.com/ i zaloguj się jako *contoso\miminstall*.  Zostanie wyświetlona pusta witryna programu SharePoint o nazwie *MIM Portal*.

    ![Portalu MIM po adresem http://mim.contoso.com/ obrazu](media/MIM-DeploySP1.png)

5. Skopiuj adres URL, a następnie w przeglądarce Internet Explorer otwórz **Opcje internetowe**, przejdź do **karty Zabezpieczenia**, wybierz opcję **Lokalny intranet** i kliknij opcję **Witryny**.

    ![Obraz opcji internetowych](media/MIM-DeploySP2.png)

6. W oknie **Lokalny intranet** kliknij pozycję **Zaawansowane** i wklej skopiowany adres URL w polu tekstowym **Dodaj tę witrynę do strefy**. Kliknij przycisk **Dodaj**, a następnie zamknij okna.

7. Otwórz program **Narzędzia administracyjne**, przejdź do karty **Usługi**, odszukaj usługę administracji programu SharePoint i uruchom ją, jeśli nie jest jeszcze uruchomiona.

>[!div class="step-by-step"]  
[«Programu SQL Server 2016](prepare-server-sql2016.md)
[Exchange Server»](prepare-server-exchange.md)
