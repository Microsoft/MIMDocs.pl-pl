---
title: Dodatek
description: Jest to dodatek do dokumentów dotyczących wdrażania usługi PAM inicjowanego przez skrypty. Obejmuje konfigurowanie domen PRIV i CORP, jak również konfigurowanie klienta w celu sprawdzania poprawności oraz informacje o tym, jak poprosić o pomoc.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 79b3547564fd5dd7874ffc53a7df50cb50ad3d49
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010680"
---
# <a name="pam-deployment-scripts-addendum"></a>Dodatek do skryptów wdrażania usługi PAM:

## <a name="addendum-1-setting-up-the-priv-domain"></a>Dodatek 1 — Konfigurowanie domeny PRIV

Po rozpakowaniu skompresowanego pliku do folderu $env:SYSTEMDRIVE\PAM dokonaj edycji pliku PAMDeploymentConfig.xml, aby podać szczegóły lasu PRIV. Zaktualizuj DNSName, NetBiosName, nazwę kontrolera domeny, ścieżkę bazy danych/dziennika & ścieżka folderu SYSVOL. Zaktualizuj także wartości DomainMode i ForestMode. Jeśli używasz systemu Windows Server 2016 lub nowszego, ustaw wartość DomainMode & ForestMode na system Windows Server 2016 (WinThreshold).

1. Zaloguj się do kontrolera domeny PRIV jako administrator
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Wybierz opcję menu 9 (Konfiguracja lasu PRIV)


Kontroler domeny zostanie automatycznie ponownie uruchomiony po zakończeniu. Hasło administratora trybu przywracania usług katalogu (DSRM) musi spełniać następujące kryteria:

  * Długość hasła wynosi co najmniej 15 znaków.
  * Hasło zawiera co najmniej jedną małą literę.
  * Hasło zawiera co najmniej jedną WIELKĄ literę.
  * Hasło zawiera co najmniej jedną cyfrę lub znak specjalny.

## <a name="addendum-2-setting-up-the-corp-domain"></a>Dodatek 2 — Konfigurowanie domeny CORP

Jeśli dopiero zaczynasz pracę z usługą PAM i chcesz skonfigurować środowisko testowe, skrypt umożliwia również konfigurację domeny CORP. Po rozpakowaniu skompresowanego pliku do folderu $env:SYSTEMDRIVE\PAM dokonaj edycji pliku PAMDeploymentConfig.xml, aby dodać szczegóły lasu CORP. Zaktualizuj nazwę DNSName, NetBiosName, DC, Database/log i Path folderu SYSVOL. Wymagany jest co najmniej poziom funkcjonalny systemu Windows Server 2012 R2.

1. Zaloguj się do kontrolera domeny CORP jako administrator
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Wybierz opcję menu 10 (CORP forest setup (Konfigurowanie lasu CORP)).

Kontroler domeny zostanie automatycznie ponownie uruchomiony po zakończeniu.

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>Dodatek 3 — Konfigurowanie klienta CORP pod kątem sprawdzenia poprawności

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

## <a name="addendum-4-if-something-goes-wrong"></a>Dodatek 4 — Postępowanie w razie wystąpienia błędu

Wszystkie dzienniki skryptu są zapisywane w folderze %AppData%\MIMPAMInstall. W razie konieczności, Kompresuj folder do pliku zip wraz ze szczegółami operacji i błędu.
