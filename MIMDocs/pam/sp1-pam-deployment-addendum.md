---
title: Dodatek
description: "Jest to dodatek do dokumentów dotyczących wdrażania usługi PAM inicjowanego przez skrypty. Obejmuje konfigurowanie domen PRIV i CORP, jak również konfigurowanie klienta w celu sprawdzania poprawności oraz informacje o tym, jak poprosić o pomoc."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: f69fe68dc63323c0945a4902e34ea8153f938c02
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# Dodatek do skryptów wdrażania usługi PAM:
<a id="pam-deployment-scripts-addendum" class="xliff"></a>

## Dodatek 1 — Konfigurowanie domeny PRIV
<a id="addendum-1-setting-up-the-priv-domain" class="xliff"></a>

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
<a id="addendum-2-setting-up-the-corp-domain" class="xliff"></a>

Jeśli dopiero rozpoczynasz korzystanie z programu PAM i chcesz skonfigurować środowisko testowe, korzystając ze skryptu możesz także skonfigurować domenę CORP. Po rozpakowaniu skompresowanego pliku do folderu $env:SYSTEMDRIVE\PAM dokonaj edycji pliku PAMDeploymentConfig.xml, aby dodać szczegóły lasu CORP. Zaktualizuj wartości DNSName, NetbiosName, nazwę kontrolera domeny DC, ścieżkę bazy danych/dziennika i ścieżkę SYSVOL. Wymagany jest co najmniej poziom funkcjonalny systemu Windows Server 2012 R2.

1. Zaloguj się do kontrolera domeny CORP jako administrator.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Wybierz opcję menu 10 (CORP forest setup (Konfigurowanie lasu CORP)).

Kontroler domeny zostanie automatycznie ponownie uruchomiony po zakończeniu.

## Dodatek 3 — Konfigurowanie klienta CORP pod kątem sprawdzenia poprawności
<a id="addendum-3-setting-up-a-corp-client-to-do-the-validation" class="xliff"></a>

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
<a id="addendum-4-if-something-goes-wrong" class="xliff"></a>

Wszystkie dzienniki skryptu są zapisywane w folderze %AppData%\MIMPAMInstall. Skompresuj ten folder do postaci pliku ZIP i wyślij go za pośrednictwem poczty e-mail na adres [mim2016@microsoft.com](mailto:mim2016@microsoft.com) wraz ze szczegółami operacji i błędu.
