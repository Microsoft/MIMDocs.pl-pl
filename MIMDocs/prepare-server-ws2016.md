---
title: Konfigurowanie systemu Windows Server 2016 dla programu MIM 2016 z dodatkiem SP1 | Microsoft Docs
description: Zapoznaj się z krokami i minimalnymi wymaganiami dotyczącymi przygotowania systemu Windows Server 2016 do pracy z programem MIM 2016 z dodatkiem SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7348507593426ba112feef9d68686ee493a6391d
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701408"
---
# <a name="set-up-an-identity-management-server-windows-server-2016"></a>Konfigurowanie serwera zarządzania tożsamościami: Windows Server 2016

> [!div class="step-by-step"]
> [«Przygotowywanie domeny](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
> 
> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Na przykład:
> - Nazwa kontrolera domeny — **corpdc**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi programu MIM — **corpservice**
> - Nazwa serwera synchronizacji programu MIM — **corpsync**
> - Nazwa SQL Server — **corpsql**
> - Hasło — <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Przyłączanie do domeny systemu Windows Server 2016

Zacznij od maszyny z systemem Windows Server 2016 z co najmniej 8 12GB pamięci RAM. Podczas instalacji Określ wartość "Windows Server 2016 Standard/Datacenter (serwer z graficznym interfejsem użytkownika) x64".

1. Zaloguj się na nowym komputerze jako administrator.

2. Za pomocą Panelu sterowania nadaj komputerowi statyczny adres IP w sieci. Skonfiguruj ten interfejs sieciowy do wysyłania zapytań DNS na adres IP kontrolera domeny w poprzednim kroku, a następnie ustaw nazwę komputera na **CORPSERVICE**.  Ta operacja wymaga ponownego uruchomienia serwera.

3. Otwórz Panel sterowania i Przyłącz komputer do domeny skonfigurowanej w ostatnim kroku *contoso.com*.  Ta operacja obejmuje podanie nazwy użytkownika i poświadczeń administratora domeny, takich jak *contoso\administrator*.  Gdy zostanie wyświetlony komunikat powitalny, zamknij okno dialogowe i ponownie uruchom serwer.

4. Zaloguj się do komputera *CORPSERVICE* jako konto domeny przy użyciu administratora komputera lokalnego, takiego jak *Contoso\MIMINSTALL*.


5. Uruchom okno programu PowerShell jako administrator i wpisz poniższe polecenie, aby zaktualizować komputer przy użyciu ustawień zasad grupy.

    ```
    gpupdate /force /target:computer
    ```

    Po upływie maksymalnie minuty proces zostanie zakończony i zostanie wyświetlony komunikat „Pomyślnie ukończono aktualizowanie zasad komputera”.

6. Dodaj role **Serwer sieci Web (IIS)** i **Serwer aplikacji**, funkcje platformy **.NET Framework** 3.5, 4.0 i 4.5 oraz **moduł usługi Active Directory dla środowiska Windows PowerShell**.

    ![Obraz funkcji programu PowerShell](media/MIM-DeployWS2.png)

7. W programie PowerShell wpisz poniższe polecenia. Może być konieczne określenie innej lokalizacji plików źródłowych funkcji platformy **.NET Framework** 3.5. Te funkcje nie są zwykle dostępne w ramach instalacji systemu Windows Server, ale są dostępne w folderze równoległym (SxS) w folderze źródłowym na dysku instalacyjnym systemu operacyjnego, np. „*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Konfigurowanie zasad zabezpieczeń serwera

Skonfiguruj zasady zabezpieczeń serwera w celu zezwalania na uruchamianie nowo utworzonych kont jako usług.
> [!NOTE] 
> W zależności od konfiguracji, jednego serwera (wszystkie w jednym) lub serwerów rozproszonych, które należy dodać tylko na podstawie roli komputera członkowskiego, na przykład serwera synchronizacji. 

1. Uruchom program Zasady zabezpieczeń lokalnych.

2. Przejdź do lokalizacji **Zasady lokalne > Przypisywanie praw użytkownika**.

3. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Logowanie jako usługa**, a następnie wybierz pozycję **Właściwości**.

    ![Obraz programu Zasady zabezpieczeń lokalnych](media/MIM-DeployWS3.png)

4. Kliknij pozycję **Dodaj użytkownika lub grupę**, a następnie w polu tekstowym wpisz następujące `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`polecenie, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.

5. Kliknij przycisk **OK**, aby zamknąć okno **Logowanie w trybie usługi: właściwości**.

6.  W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Odmów dostępu do tego komputera z sieci**, a następnie wybierz polecenie **Właściwości**. >

[!NOTE] Rozdzielenie serwerów ról spowoduje przerwanie niektórych funkcji, takich jak SSPR.

7. Kliknij pozycję **Dodaj użytkownika lub grupę**, a następnie w polu tekstowym wpisz `contoso\MIMSync; contoso\MIMService` i kliknij przycisk **OK**.

8. Kliknij przycisk **OK**, aby zamknąć okno **Odmowa dostępu do tego komputera z sieci: właściwości**.

9. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **odmowa logowania lokalnego**i wybierz polecenie **Właściwości**.

10. Kliknij pozycję **Dodaj użytkownika lub grupę**, a następnie w polu tekstowym wpisz `contoso\MIMSync; contoso\MIMService` i kliknij przycisk **OK**.

11. Kliknij przycisk **OK**, aby zamknąć okno **Odmowa logowania lokalnego: właściwości**.

12. Zamknij okno programu Zasady zabezpieczeń lokalnych.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>W razie konieczności zmień tryb uwierzytelniania systemu Windows usług IIS

1.  Otwórz okno programu PowerShell.

2.  Zatrzymaj usługi IIS za pomocą polecenia *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Przygotowywanie domeny](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
