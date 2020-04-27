---
title: Krok 1 — Konfigurowanie domeny PRIV
description: Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager, z użyciem skryptów
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
ms.openlocfilehash: 4e24ac1b3f3f3d46aa5f67175dc4c4b8778bdb21
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043855"
---
# <a name="step-1-configuring-the-priv-domain"></a>Krok 1 — Konfigurowanie domeny PRIV

> [!div class="step-by-step"]
> [Krok 2»](sp1-step2-configuring-corp-domain.md)

1. Zaloguj się do kontrolera domeny PRIVDC jako administrator.
   * W przypadku środowiska PRIVOnly należy zalogować się do kontrolera domeny CORPDC.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 1 (PRIV Forest Configuration (Konfiguracja lasu PRIV)).


Konta usługi wymagane do zarządzania programami SQL/SharePoint i MIM są tworzone automatycznie, jeśli nie są jeszcze obecne w domenie. Podczas wykonywania skryptu będą wyświetlane monity o podanie haseł w celu utworzenia tych kont usługi.
W przypadku domeny PRIV w systemie Windows Server 2016 z poziomem funkcjonalności systemu Windows Server 2016 Technical Preview 5 skrypt wyświetli monit o włączenie opcjonalnej funkcji zarządzania uprzywilejowanym dostępem usługi Active Directory, która jest wymagana przez usługę PAM. Wybierz opcję „Tak”, aby kontynuować.
Dla poziomów funkcjonalności poniżej systemu Windows Server 2016 należy odrzucić ostrzeżenie informujące o tym, że dodatkowa konfiguracja nie zostanie przeprowadzona. Gdy administrator podniesie poziom funkcjonalności do systemu Windows Server 2016, konieczne będzie ponowne uruchomienie pliku PAMDeployment.ps1 i konfiguracja lasu PAM.

>[!NOTE]
>Poniższe kroki nie są wymagane dla konfiguracji środowiska PRIVOnly.

Skopiuj plik SIDs.txt, który został wygenerowany w folderze $env:SYSTEMDRIVE\PAM do podobnego folderu w kontrolerze domeny CORPDC. Kontroler domeny CORPDC wymaga przeprowadzenia tej czynności w celu ustawienia uprawnień dla użytkowników domeny PRIV w celu umożliwienia im odczytu właściwości użytkownika domeny CORP.
Po zakończeniu działania skryptu zostanie wyświetlony monit o ponowne uruchomienie maszyny, dzięki czemu zmiany zaczną obowiązywać.

> [!div class="step-by-step"]
> [Krok 2»](sp1-step2-configuring-corp-domain.md)
