---
title: Konfigurowanie systemu Windows Server 2016 z dodatkiem SP1 MIM 2016 | Dokumentacja firmy Microsoft
description: Pobierz kroki i minimalne wymagania w celu przygotowania systemu Windows Server 2016 do pracy z dodatkiem SP1 programu MIM 2016.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7c77ed0ceb541b9b00ebb9954ce65a53f0f44442
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-servers-windows-server-2016"></a>Konfigurowanie serwerów zarządzania tożsamościami: Windows Server 2016

>[!div class="step-by-step"]
[«Przygotowywanie domeny](preparing-domain.md)
[programu SQL Server 2016»](prepare-server-sql2016.md)

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **corpdc**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi MIM — **corpservice**
> - Nazwa serwera synchronizacji MIM — **corpsync**
> - Nazwa programu SQL Server — **corpsql**
> - Hasło — **Pass@word1**

## <a name="join-windows-server-2016-to-your-domain"></a>Dołącz do domeny systemu Windows Server 2016

Użyj komputera z systemem Windows Server 2016, z co najmniej 8 12GB pamięci RAM. Podczas instalacji Określ wersję "Windows Server 2016 Standard/centrum danych (serwer z graficznym interfejsem użytkownika) x 64".

1. Zaloguj się na nowym komputerze jako administrator.

2. Za pomocą Panelu sterowania nadaj komputerowi statyczny adres IP w sieci. Skonfiguruj ten interfejs sieciowy w celu wysyłania zapytań DNS na adres IP kontrolera domeny w poprzednim kroku, a następnie ustaw nazwę komputera **CORPSERVICE**.  Wymaga to ponownego uruchomienia serwera.

3. Otwórz Panel sterowania i przyłącz komputer do domeny, skonfigurowanej w ostatnim kroku *contoso.com*.  Wymaga to też podania nazwy użytkownika i poświadczeń administratora domeny, takich jak *Contoso\Administrator*.  Gdy zostanie wyświetlony komunikat powitalny, zamknij okno dialogowe i ponownie uruchom serwer.

4. Zaloguj się do komputera *CORPSERVICE* jako konto domeny z komputera lokalnego administratora, takich jak *Contoso\MIMINSTALL*.


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
> W zależności od konfiguracji pojedynczego server(all-in-one) lub serwerów rozproszonych, które należy dodać oparty na roli komputera członkowskiego, takich jak serwer synchronizacji. 

1. Uruchom program Zasady zabezpieczeń lokalnych.

2. Przejdź do lokalizacji **Zasady lokalne > Przypisywanie praw użytkownika**.

3. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Logowanie w trybie usługi** i wybierz polecenie **Właściwości**.

    ![Obraz programu Zasady zabezpieczeń lokalnych](media/MIM-DeployWS3.png)

4. Kliknij przycisk **Dodaj użytkownika lub grupę**, a w polu tekstowym następującego typu na podstawie roli `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, kliknij przycisk **Sprawdź nazwy**i kliknij przycisk **OK**.

5. Kliknij przycisk **OK**, aby zamknąć okno **Logowanie w trybie usługi: właściwości**.

6.  W okienku szczegółów kliknij prawym przyciskiem myszy **odmowa dostępu do tego komputera z sieci**i wybierz **właściwości**. >

[!NOTE] Jeśli rola oddzielne serwery ten krok spowoduje przerwanie niektórych funkcjonalności, takich jak funkcja SSPR.

7. Kliknij pozycję **Dodaj użytkownika lub grupę**, a następnie w polu tekstowym wpisz `contoso\MIMSync; contoso\MIMService` i kliknij przycisk **OK**.

8. Kliknij przycisk **OK**, aby zamknąć okno **Odmowa dostępu do tego komputera z sieci: właściwości**.

9. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Odmowa logowania lokalnego** i wybierz polecenie **Właściwości**.

10. Kliknij pozycję **Dodaj użytkownika lub grupę**, a następnie w polu tekstowym wpisz `contoso\MIMSync; contoso\MIMService` i kliknij przycisk **OK**.

11. Kliknij przycisk **OK**, aby zamknąć okno **Odmowa logowania lokalnego: właściwości**.

12. Zamknij okno programu Zasady zabezpieczeń lokalnych.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Zmień tryb uwierzytelniania systemu Windows usług IIS, w razie potrzeby

1.  Otwórz okno programu PowerShell.

2.  Zatrzymaj usługi IIS za pomocą polecenia *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[«Przygotowywanie domeny](preparing-domain.md)
[programu SQL Server 2016»](prepare-server-sql2016.md)