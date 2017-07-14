---
title: "Konfigurowanie systemu Windows Server 2012 R2 do współdziałania z programem MIM 2016| Dokumentacja firmy Microsoft"
description: "Zapoznaj się z procedurą przygotowywania systemu Windows Server 2012 R2 do współdziałania z programem MIM 2016 i minimalnymi wymaganiami dotyczącymi tego procesu."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 7f16c3a054f0a2c59f118ba33bf64fca10034690
ms.openlocfilehash: a0241964edb21ca4bf938ae84693b9947f6e2efb
ms.contentlocale: pl-pl
ms.lasthandoff: 07/10/2017


---

# Konfigurowanie serwera zarządzania tożsamościami: Windows Server 2012 R2
<a id="set-up-an-identity-management-server-windows-server-2012-r2" class="xliff"></a>

>[!div class="step-by-step"]
[« Przygotowywanie domeny](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Na przykład:
> - Nazwa kontrolera domeny — **nazwa_serwera_mim**
> - Nazwa domeny — **contoso**
> - Hasło — **Pass@word1**

## Przyłączanie komputera z systemem Windows Server 2012 R2 do domeny
<a id="join-windows-server-2012-r2-to-your-domain" class="xliff"></a>

Użyj komputera z systemem Windows Server 2012 R2 i co najmniej 8 GB pamięci RAM. Podczas instalacji określ wersję „Windows Server 2012 R2 Standard (serwer z graficznym interfejsem użytkownika) x64”.

1. Zaloguj się na nowym komputerze jako administrator.

2. Za pomocą Panelu sterowania nadaj komputerowi statyczny adres IP w sieci. Skonfiguruj ten interfejs sieciowy w celu wysyłania zapytań DNS na adres IP kontrolera domeny z poprzedniego kroku, a następnie ustaw nazwę komputera **CORPIDM**.  Wymaga to ponownego uruchomienia serwera.

3. Otwórz Panel sterowania i przyłącz komputer do domeny *contoso.local*, skonfigurowanej w ostatnim kroku.  Wymaga to też podania nazwy użytkownika i poświadczeń administratora domeny, takich jak *Contoso\Administrator*.  Gdy zostanie wyświetlony komunikat powitalny, zamknij okno dialogowe i ponownie uruchom serwer.

4. Zaloguj się na komputerze *CorpIDM* jako administrator domeny, na przykład *Contoso\Administrator*.

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

## Konfigurowanie zasad zabezpieczeń serwera
<a id="configure-the-server-security-policy" class="xliff"></a>

Skonfiguruj zasady zabezpieczeń serwera w celu zezwalania na uruchamianie nowo utworzonych kont jako usług.

1. Uruchom program Zasady zabezpieczeń lokalnych.

2. Przejdź do lokalizacji **Zasady lokalne > Przypisywanie praw użytkownika**.

3. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Logowanie w trybie usługi** i wybierz polecenie **Właściwości**.

    ![Obraz programu Zasady zabezpieczeń lokalnych](media/MIM-DeployWS3.png)

4. Kliknij pozycję **Dodaj użytkownika lub grupę**, a następnie w polu tekstowym wpisz `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, kliknij pozycję **Sprawdź nazwy** i kliknij przycisk **OK**.

5. Kliknij przycisk **OK**, aby zamknąć okno **Logowanie w trybie usługi: właściwości**.

6.  W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Odmowa dostępu do tego komputera z sieci** i wybierz polecenie **Właściwości**.

7. Kliknij pozycję **Dodaj użytkownika lub grupę**, a następnie w polu tekstowym wpisz `contoso\MIMSync; contoso\MIMService` i kliknij przycisk **OK**.

8. Kliknij przycisk **OK**, aby zamknąć okno **Odmowa dostępu do tego komputera z sieci: właściwości**.

9. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Odmowa logowania lokalnego** i wybierz polecenie **Właściwości**.

10. Kliknij pozycję **Dodaj użytkownika lub grupę**, a następnie w polu tekstowym wpisz `contoso\MIMSync; contoso\MIMService` i kliknij przycisk **OK**.

11. Kliknij przycisk **OK**, aby zamknąć okno **Odmowa logowania lokalnego: właściwości**.

12. Zamknij okno programu Zasady zabezpieczeń lokalnych.


## Zmienianie trybu uwierzytelniania systemu Windows w usługach IIS
<a id="change-the-iis-windows-authentication-mode" class="xliff"></a>

1.  Otwórz okno programu PowerShell.

2.  Zatrzymaj usługi IIS za pomocą polecenia *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[« Przygotowywanie domeny](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

