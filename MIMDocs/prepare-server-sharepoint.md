---
title: Konfigurowanie programu SharePoint pod kątem programu Microsoft Identity Manager 2016| Dokumentacja firmy Microsoft
description: Instalowanie i konfigurowanie programu SharePoint Foundation w celu hostowania strony portalu programu MIM.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62ef8796717dbcaea18d21bc3d28248efdeef92e
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "73568104"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Konfigurowanie serwera zarządzania tożsamościami: SharePoint

> [!div class="step-by-step"]
> [«SQL Server](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
> 

> [!NOTE]
> Procedura instalacji programu SharePoint Server 2019 nie różni się od procedury instalacji programu SharePoint Server 2016, **z wyjątkiem** jednego dodatkowego kroku, który należy podjąć w celu odblokowania plików ASHX używanych przez portal programu MIM.

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **corpdc**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi programu MIM — **corpservice**
> - Nazwa serwera synchronizacji programu MIM — **corpsync**
> - Nazwa SQL Server — **corpsql**
> - Hasło — <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Zainstaluj **program SharePoint 2016**

> [!NOTE]
> Instalator wymaga połączenia internetowego w celu pobrania jego wymagań wstępnych. Jeśli komputer znajduje się w sieci wirtualnej, która nie zapewnia łączności z Internetem, dodaj do komputera dodatkowy interfejs sieciowy pozwalający na łączenie się z Internetem. Interfejs ten można wyłączyć po ukończeniu instalacji.

Wykonaj następujące kroki, aby zainstalować program SharePoint 2016. Po zakończeniu instalacji serwer zostanie uruchomiony ponownie.

1.  Uruchom program **PowerShell** jako konto domeny z uprawnieniami administratora lokalnego na **corpservice** i **sysadmin** na serwerze bazy danych SQL, użyjemy **contoso\miminstall**.

    -   Przejdź do katalogu, do którego rozpakowano program SharePoint.

    -   Wpisz następujące polecenie.
    ```
    .\prerequisiteinstaller.exe
    ```

2.  Po zainstalowaniu wymagań wstępnych **programu SharePoint** zainstaluj **program SharePoint 2016** , wpisując następujące polecenie:

    ```
    .\setup.exe
    ```

3.  Wybierz typ kompletnego serwera.

4.  Po zakończeniu instalacji uruchom kreatora.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Uruchom kreatora, aby skonfigurować program SharePoint

Wykonaj kroki określone w **Kreatorze konfiguracji produktów SharePoint**, aby skonfigurować program SharePoint do pracy z programem MIM.

1. Na karcie **Połącz z farmą serwerów** zmień ustawienia, aby utworzyć nową farmę serwerów.

2. Określ ten serwer jako serwer bazy danych, taki jak **corpsql** dla bazy danych konfiguracji, i *Contoso\SharePoint* jako konto dostępu do bazy danych używane przez program SharePoint.
3. Utwórz hasło zabezpieczeń farmy.

4. W Kreatorze konfiguracji zalecamy wybranie [MinRole](/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server) typu **frontonu**

5. Gdy Kreator konfiguracji ukończy zadanie konfiguracji o wartości 10 z 10, kliknij przycisk Zakończ. zostanie otwarta przeglądarka sieci Web...

6. Jeśli zostanie wyświetlony monit z menu podręcznego programu Internet Explorer, należy przeprowadzić uwierzytelnianie jako *Contoso\miminstall* (lub równoważne konto administratora).

7. W Kreatorze sieci Web (w aplikacji sieci Web) kliknij przycisk **Anuluj/Pomiń**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Przygotowywanie programu SharePoint do hostowania portalu programu MIM

> [!NOTE]
> Początkowo protokół SSL nie zostanie skonfigurowany. Należy pamiętać o skonfigurowaniu protokołu SSL lub równoważnego przed włączeniem dostępu do tego portalu.

1. Uruchom **powłokę zarządzania programu sharepoint 2016** i uruchom następujący skrypt programu PowerShell, aby utworzyć **aplikację sieci Web programu SharePoint 2016**.

    ```PowerShell
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Zostanie wyświetlony komunikat ostrzegawczy z informacją, że jest używana metoda uwierzytelniania Windows Classic i powrót z polecenia końcowego może potrwać kilka minut. Po ukończeniu dane wyjściowe będą wskazywać adres URL nowego portalu. Pozostaw otwarte okno **powłoki zarządzania programu SharePoint 2016** do późniejszego odwoływania się do niego.

2. Uruchom powłokę zarządzania programu SharePoint 2016 i uruchom następujący skrypt programu PowerShell, aby utworzyć **kolekcję witryn programu SharePoint** skojarzoną z daną aplikacją sieci Web.
    ```PowerShell
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
    ```
    > [!NOTE]
    > Sprawdź, czy wynik zmiennej *CompatibilityLevel* to "15". Jeśli wynik jest inny niż "15", kolekcja witryn nie została utworzona w odpowiedniej wersji środowiska; Usuń kolekcję witryn i utwórz ją ponownie.

    > [!IMPORTANT]
    > Program SharePoint Server 2019 używa innej właściwości aplikacji sieci Web, aby zachować listę zablokowanych rozszerzeń plików. Dlatego w celu odblokowania. Pliki ASHX używane przez portal programu MIM trzy dodatkowe polecenia muszą zostać wykonane ręcznie z poziomu powłoki zarządzania programu SharePoint.
    <br/>
    **Wykonaj kolejne trzy polecenia tylko dla programu SharePoint 2019:**

    ```PowerShell
    $w.BlockedASPNetExtensions.Remove("ashx")
    $w.Update()
    $w.BlockedASPNetExtensions
    ```
   > [!NOTE]
   > Upewnij się, że lista *BlockedASPNetExtensions* nie zawiera rozszerzenia ASHX, w przeciwnym razie kilka stron portalu programu MIM nie zostanie prawidłowo renderowane.


3. Wyłącz **stan wyświetlania po stronie serwera SharePoint** i zadanie programu SharePoint "zadanie analizy kondycji (godzinowo, czasomierz Microsoft SharePoint Foundation, wszystkie serwery)", uruchamiając następujące polecenia programu PowerShell w **powłoce zarządzania programu SharePoint 2016**:

   ```PowerShell
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. Na serwerze zarządzania tożsamościami Otwórz nową kartę przeglądarki sieci Web, przejdź do http://mim.contoso.com/ i zaloguj się jako *contoso\miminstall*.  Zostanie wyświetlona pusta witryna programu SharePoint o nazwie *MIM Portal*.

    ![Portal programu MIM na http://mim.contoso.com/ obrazie](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Skopiuj adres URL, a następnie w przeglądarce Internet Explorer otwórz **Opcje internetowe**, przejdź do **karty Zabezpieczenia**, wybierz opcję **Lokalny intranet** i kliknij opcję **Witryny**.

    ![Obraz opcji internetowych](media/MIM-DeploySP2.png)

6. W oknie **Lokalny intranet** kliknij pozycję **Zaawansowane** i wklej skopiowany adres URL w polu tekstowym **Dodaj tę witrynę do strefy**. Kliknij przycisk **Dodaj**, a następnie zamknij okna.

7. Otwórz program **Narzędzia administracyjne**, przejdź do karty **Usługi**, odszukaj usługę administracji programu SharePoint i uruchom ją, jeśli nie jest jeszcze uruchomiona.

> [!div class="step-by-step"]  
> [«SQL Server](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
