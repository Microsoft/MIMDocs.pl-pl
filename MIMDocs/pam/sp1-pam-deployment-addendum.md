---
title: Dodatek
description: "Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager, z użyciem skryptów"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 482cfbbac3ea668ca4bf9d8a4a45469e61634f98


---
# Dodatek:

## Dodatek 1 — Konfigurowanie domeny PRIV

Po rozpakowaniu skompresowanego pliku do folderu $env:SYSTEMDRIVE\PAM dokonaj edycji pliku PAMDeploymentConfig.xml, aby podać szczegóły lasu PRIV. Zaktualizuj wartości DNSName, NetbiosName, nazwę kontrolera domeny, ścieżkę bazy danych/dziennika i ścieżkę SYSVOL. Zaktualizuj także wartości DomainMode i ForestMode. Jeśli testujesz system Windows Server Technical Preview 5, ustaw dla opcji DomainMode i ForestMode wartość WinThreshold.

1. Zaloguj się do kontrolera domeny PRIV jako administrator.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Wybierz opcję menu 9 (Priv Forest setup (Konfigurowanie lasu Priv)).


Kontroler domeny zostanie automatycznie ponownie uruchomiony po zakończeniu. Hasło administratora trybu przywracania usług katalogu (DSRM) musi spełniać następujące kryteria:

  * Długość hasła wynosi co najmniej 15 znaków.
  * Hasło zawiera co najmniej jedną małą literę.
  * Hasło zawiera co najmniej jedną WIELKĄ literę.
  * Hasło zawiera co najmniej jedną cyfrę lub znak specjalny.

## Dodatek 2 — Konfigurowanie domeny CORP

Jeśli dopiero rozpoczynasz korzystanie z programu PAM i chcesz skonfigurować środowisko testowe, korzystając ze skryptu możesz także skonfigurować domenę CORP. Po rozpakowaniu skompresowanego pliku do folderu $env:SYSTEMDRIVE\PAM dokonaj edycji pliku PAMDeploymentConfig.xml, aby dodać szczegóły lasu CORP. Zaktualizuj wartości DNSName, NetbiosName, nazwę kontrolera domeny DC, ścieżkę bazy danych/dziennika i ścieżkę SYSVOL. Wymagany jest co najmniej poziom funkcjonalny systemu Windows Server 2012 R2.

1. Zaloguj się do kontrolera domeny CORP jako administrator.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Wybierz opcję menu 10 (CORP forest setup (Konfigurowanie lasu CORP)).

Kontroler domeny zostanie automatycznie ponownie uruchomiony po zakończeniu.

## Dodatek 3 — Konfigurowanie klienta CORP pod kątem sprawdzenia poprawności

Wartość ClientBinaryLocation w pliku konfiguracyjnym musi wskazywać lokalizację pliku setup.exe.
Zaloguj się do klienta jako administrator lokalny i uruchom następujące polecenia w oknie programu PowerShell z podwyższonym poziomem uprawnień:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Wybierz opcję menu 7 (MIM PAM Client Setup (Konfiguracja klienta usługi PAM programu MIM)).


Jeśli komputer nie jest przyłączony do domeny, wyświetlony zostanie monit o poświadczenia administratora domeny CORP w celu przeprowadzenia przyłączenia do domeny. Po przyłączeniu do domeny wymagane jest ponowne uruchomienie komputera. Ponownie zaloguj się do klienta jako administrator lokalny i uruchom następujące polecenia w oknie programu PowerShell z podwyższonym poziomem uprawnień:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Wybierz opcję menu 7 (MIM PAM Client Setup (Konfiguracja klienta usługi PAM programu MIM)).

Przejdź do kroku 8 opisanego powyżej.

## Dodatek 4 — Postępowanie w razie wystąpienia błędu

Wszystkie dzienniki skryptu są zapisywane w folderze %AppData%\MIMPAMInstall. Skompresuj ten folder do postaci pliku ZIP i wyślij go za pośrednictwem poczty e-mail na adres [mim2016@microsoft.com](mim2016@microsoft.com) wraz ze szczegółami operacji i błędu.



<!--HONumber=Sep16_HO4-->

