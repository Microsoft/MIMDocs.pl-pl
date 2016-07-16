---
title: "Krok 3 — Przygotowanie serwera PAM | Microsoft Identity Manager"
description: 
keywords: 
author: 
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: ec65078cea33b73aa9482e831a1870df477c6581


---

# Krok 3 — Przygotowanie serwera PAM

>[!div class="step-by-step"]
[« Krok 2](step-2-prepare-priv-domain-controller.md)
[Krok 4 »](step-4-install-mim-components-on-pam-server.md)

## Instalowanie systemu Windows Server 2012 R2
Na trzeciej maszynie wirtualnej zainstaluj system Windows Server 2012 R2, konkretnie system Windows Server 2012 R2 Standard (serwer z graficznym interfejsem użytkownika) x64, aby utworzyć serwer *PAMSRV*. Ponieważ programy SQL Server i SharePoint 2013 zostaną zainstalowane na tym komputerze, komputer musi mieć co najmniej 8 GB pamięci RAM.

1. Wybierz opcję **Windows Server 2012 R2 Standard (serwer z graficznym interfejsem użytkownika) x64**.

    ![Wybieranie systemu Windows Server Standard z graficznym interfejsem użytkownika — zrzut ekranu](media/PAM_GS_Select_WS2012.png)

2. Przeczytaj i zaakceptuj postanowienia licencyjne.

3.  Ponieważ dysk będzie pusty, wybierz opcję **Niestandardowa: tylko zainstaluj system Windows** i użyj **niezainicjowanego miejsca na dysku**.

4.  Zaloguj się na nowym komputerze jako administrator.  Przy użyciu Panelu sterowania nadaj komputerowi statyczny adres IP w sieci wirtualnej, skonfiguruj ten interfejs sieciowy do wysyłania zapytań DNS do adresu IP PRIVDC i ustaw nazwę tego komputera na *PAMSRV*.  Wymaga to ponownego uruchomienia serwera.

5.  Jeśli sieć wirtualna nie zapewnia łączności z Internetem, dodaj do komputera dodatkowy interfejs sieciowy pozwalający na łączenie się z Internetem.  Będzie to potrzebne podczas instalacji programu SharePoint. Po zakończeniu tego kroku można wyłączyć tę funkcję.

6.  Po ponownym uruchomieniu serwera zaloguj się jako administrator. Za pomocą Panelu sterowania skonfiguruj komputer tak, aby sprawdzał dostępność aktualizacji, a następnie zainstaluj wszystkie wymagane aktualizacje.  Może to wymagać ponownego uruchomienia serwera.

7.  Po ponownym uruchomieniu serwera zaloguj się jako administrator, otwórz Panel sterowania i dołącz serwer PAMSRV do domeny PRIV (priv.contoso.local).  Wymaga to też podania nazwy użytkownika i poświadczeń administratora domeny PRIV (PRIV\Administrator). Gdy zostanie wyświetlony komunikat powitalny, zamknij okno dialogowe i ponownie uruchom serwer.


### Dodawanie serwera sieci Web (IIS) i ról serwera aplikacji
Dodaj role Serwer sieci Web (IIS) i Serwer aplikacji, funkcje platformy .NET Framework 3.5 oraz moduł usługi Active Directory dla środowiska Windows PowerShell, a także inne funkcje wymagane przez program SharePoint

1.  Zaloguj się jako administrator domeny PRIV (PRIV\Administrator) i uruchom program PowerShell.

2.  Wpisz następujące polecenia. Może być konieczne określenie innej lokalizacji plików źródłowych funkcji platformy .NET Framework 3.5. Te funkcje nie są zwykle dostępne w ramach instalacji systemu Windows Server, ale są dostępne w folderze równoległym (SxS) w folderze źródłowym na dysku instalacyjnym systemu operacyjnego, np. d:\Sources\SxS\.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### Konfigurowanie zasad zabezpieczeń serwera
Skonfiguruj zasady zabezpieczeń serwera w celu zezwalania na uruchamianie nowo utworzonych kont jako usług.

1.  Uruchom program **Zasady zabezpieczeń lokalnych**.   
2.  Przejdź do lokalizacji **Zasady lokalne** > **Przypisywanie praw użytkownika**.  
3.  W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Logowanie w trybie usługi** i wybierz polecenie **Właściwości**.  
4.  Kliknij przycisk **Dodaj użytkownika lub grupę** i w nazwach użytkowników i grup wpisz *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*. Kliknij opcję **Sprawdź nazwy** i kliknij przycisk **OK**.  

5.  Kliknij przycisk **OK**, aby zamknąć okno Właściwości.  
6.  W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Odmowa dostępu do tego komputera z sieci** i wybierz polecenie **Właściwości**.  
7.  Kliknij przycisk **Dodaj użytkownika lub grupę** i w nazwach użytkowników i grup wpisz *priv\mimmonitor; priv\MIMService; priv\mimcomponent*, a następnie kliknij przycisk **OK**.  
8.  Kliknij przycisk **OK**, aby zamknąć okno właściwości.  

9. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Odmowa logowania lokalnego** i wybierz polecenie **Właściwości**.  
10. Kliknij przycisk **Dodaj użytkownika lub grupę** i w nazwach użytkowników i grup wpisz *priv\mimmonitor; priv\MIMService; priv\mimcomponent*, a następnie kliknij przycisk **OK**.  
11. Kliknij przycisk **OK**, aby zamknąć okno właściwości.  
12. Zamknij okno programu Zasady zabezpieczeń lokalnych.  

13. Otwórz Panel sterowania i przejdź do **kont użytkowników**.  
14. Kliknij przycisk **Zezwól na dostęp do tego komputera**.  
15. Kliknij przycisk **Dodaj**, wprowadź użytkownika *MIMADMIN* w domenie *PRIV*, a następnie kliknij przycisk **Dodaj tego użytkownika jako administratora** na następnym ekranie kreatora.  
16. Kliknij przycisk **Dodaj**, wprowadź użytkownika *SharePoint* w domenie *PRIV*, a następnie kliknij przycisk **Dodaj tego użytkownika jako administratora** na następnym ekranie kreatora.  
17. Zamknij Panel sterowania.  

### Zmiana konfiguracji IIS
Istnieją dwa sposoby zmiany konfiguracji usług IIS, aby umożliwić aplikacjom wykorzystywanie trybu uwierzytelniania systemu Windows. Upewnij się, że logujesz się przy użyciu konta MIMAdmin, a następnie wykonaj jedną z następujących czynności.

Jeśli chcesz użyć programu PowerShell:
1.  Kliknij prawym przyciskiem myszy program PowerShell i wybierz opcję **Uruchom jako administrator**.  
2.  Zatrzymaj usługi IIS i odblokuj ustawienia hosta aplikacji przy użyciu tych poleceń  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```  

Jeśli chcesz użyć edytora tekstu, np. programu Notatnik:   
1. Otwórz plik **C:\Windows\System32\inetsrv\config\applicationHost.config**   
2. Przewiń w dół do wiersza 82 tego pliku. Tag **overrideModeDefault** powinien mieć wartość **<section name="windowsAuthentication" overrideModeDefault="Deny" />**  
3. Zmień wartość tagu **overrideModeDefault** na *Zezwalaj*  
4. Zapisz plik i ponownie uruchom usługi IIS za pomocą polecenia programu PowerShell `iisreset /START`

## Instalacja programu SQL Server
Jeśli program SQL Server nie znajduje się jeszcze w środowisku bastionu, zainstaluj program SQL Server 2012 (dodatek Service Pack 1 lub nowszy) lub SQL Server 2014. W poniższych krokach założono użycie programu SQL 2014.

1. Upewnij się, że logujesz się jako MIMAdmin.
2. Kliknij prawym przyciskiem myszy program PowerShell i wybierz opcję **Uruchom jako administrator**.   
3. Przejdź do katalogu, w którym znajduje się plik instalacyjny programu SQL Server.  
4. Wpisz następujące polecenie.  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## Zainstaluj program SharePoint Foundation 2013

Za pomocą programu SharePoint Foundation 2013 z Instalatorem z dodatkiem SP1 zainstaluj wymagania wstępne dotyczące oprogramowania SharePoint na serwerze PAMSRV.

> [!NOTE] 
> Ten Instalator wymaga połączenia internetowego w celu pobrania jego wymagań wstępnych. Po ich zainstalowaniu serwer zostanie ponownie uruchomiony.

1. Kliknij prawym przyciskiem myszy program PowerShell i wybierz opcję **Uruchom jako administrator**.  
2. Przejdź do katalogu, do którego rozpakowano program SharePoint.  
3. Wpisz polecenie `.\prerequisiteinstaller.exe`.

Po zainstalowaniu wymagań wstępnych programu SharePoint zainstaluj program SharePoint Foundation 2013 z dodatkiem SP1.

1.  Kliknij prawym przyciskiem myszy program PowerShell i wybierz opcję **Uruchom jako administrator**.  
2.  Przejdź do katalogu, do którego rozpakowano program SharePoint.  
3.  Wpisz polecenie `.\setup.exe`.  
4.  Wybierz typ **kompletnego serwera**.  
5.  Po zakończeniu instalacji uruchom kreatora.  

### Konfiguracja programu SharePoint
Uruchom kreatora konfiguracji produktów SharePoint, aby skonfigurować program SharePoint.

1.  Na karcie Połącz z farmą serwerów zmień ustawienia, aby **utworzyć nową farmę serwerów**.  
2.  Określ serwer **PAMSRV** jako serwer bazy danych dla bazy danych konfiguracji oraz **PRIV\SharePoint** jako konto dostępu do bazy danych używane przez program SharePoint.  
3.  Określ hasło jako hasło zabezpieczeń farmy (nie będzie już używane w dalszej części tego przewodnika).  
4.  Teraz zaakceptuj pozostałe domyślne ustawienia kreatora konfiguracji programu SharePoint, aby utworzyć farmę z jednym serwerem.    
5.  Po zakończeniu przez kreatora konfiguracji zadania konfiguracji o numerze 10 z 10 kliknij przycisk **Zakończ**. Zostanie otwarta przeglądarka sieci Web.  
6.  W menu podręcznym przeglądarki Internet Explorer przeprowadź uwierzytelnianie jako administrator domeny (PRIV\MIMAdmin), aby kontynuować.  
7.  Uruchom kreator w aplikacji sieci Web, aby skonfigurować farmę programu SharePoint.  
8.  Wybierz opcję użycia istniejącego konta zarządzanego (PRIV\SharePoint), usuń zaznaczenie pola wyboru, aby wyłączyć wszelkie dodatkowe usługi, a następnie kliknij przycisk **Dalej**.  
9. Po wyświetleniu okna Tworzenie kolekcji witryn kliknij przycisk **Pomiń**, a następnie **Zakończ**.  

## Tworzenie aplikacji sieci Web programu SharePoint Foundation 2013
Po zakończeniu pracy kreatora użyj programu PowerShell, aby utworzyć aplikację sieci Web programu SharePoint Foundation 2013 w celu hostowania portalu MIM. Jako że niniejszy przewodnik służy w celach demonstracyjnych, protokół SSL nie będzie włączony.

1.  Kliknij prawym przyciskiem myszy powłokę zarządzania programu SharePoint 2013, wybierz opcję **Uruchom jako administrator**, a następnie uruchom następujący skrypt programu PowerShell:

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Zostanie wyświetlony komunikat ostrzegawczy z informacją, że jest używana metoda uwierzytelniania Windows Classic i powrót z polecenia końcowego może potrwać kilka minut.  Po ukończeniu dane wyjściowe będą wskazywać adres URL nowego portalu.

> [!NOTE] 
> Nie zamykaj okna powłoki zarządzania programu SharePoint 2013, aby użyć go w kolejnym kroku.

## Tworzenie kolekcji witryn programu SharePoint
Następnie utwórz kolekcję witryn programu SharePoint skojarzoną z tą aplikacją sieci Web, aby hostować portal MIM.

1.  Uruchom **powłokę zarządzania programu SharePoint 2013**, jeśli nie jest jeszcze uruchomiona, a następnie uruchom poniższy skrypt programu PowerShell

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Upewnij się, że zmienna **CompatibilityLevel** jest ustawiona na wartość *14*. Jeśli zmienna zwraca wartość *15*, kolekcja witryn nie została utworzona dla wersji 2010 środowiska. Usuń kolekcję witryn i utwórz ją ponownie.

2.  Uruchom następujące polecenia programu PowerShell w **powłoce zarządzania programu SharePoint 2013**. Operacja wyłączy stan wyświetlania po stronie serwera SharePoint i zadanie programu SharePoint **Zadanie analizy kondycji (godzinowo, czasomierz Microsoft SharePoint Foundation, wszystkie serwery)**.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## Zmiana ustawień aktualizacji

1. Otwórz Panel sterowania, przejdź do **Usługi Windows Update** i kliknij, aby **zmienić ustawienia**.  
2. Zmień ustawienia, aby otrzymywać aktualizacje z usługi Windows Update i innych produktów w usługach Microsoft Update.  
3. Sprawdź dostępność nowych aktualizacji i upewnij się, że wszystkie oczekujące ważne aktualizacje są zainstalowane przed kontynuowaniem.

## Ustawianie witryny jako lokalnego intranetu

1. Uruchom przeglądarkę Internet Explorer i otwórz nową kartę przeglądarki sieci Web
2. Przejdź do adresu http://pamsrv.priv.contoso.local:82/ i zaloguj się jako PRIV\MIMAdmin.  Zostanie wyświetlona pusta witryna programu SharePoint o nazwie „MIM Portal”.  
3. W przeglądarce Internet Explorer otwórz **Opcje internetowe**, przejdź do karty **Zabezpieczenia**, wybierz opcję **Lokalny intranet** i dodaj adres URL `http://pamsrv.priv.contoso.local:82/`.

Jeśli logowanie nie powiedzie się, nazwy SPN Kerberos utworzone wcześniej w [Kroku 2](step-2-prepare-priv-domain-controller.md) mogą wymagać aktualizacji.

## Uruchomienie usługi administracji programu SharePoint

Przy użyciu opcji **Usługi** (znajdującej się w narzędziach administracyjnych) uruchom usługę **Administracja programu SharePoint**, jeśli nie jest jeszcze uruchomiona.

W Kroku 4 rozpoczniesz instalowanie składników programu MIM na serwerze PAM.

>[!div class="step-by-step"]
[« Krok 2](step-2-prepare-priv-domain-controller.md)
[Krok 4 »](step-4-install-mim-components-on-pam-server.md)



<!--HONumber=Jun16_HO5-->


