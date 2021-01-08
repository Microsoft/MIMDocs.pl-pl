---
title: Krok 1 — Konfigurowanie domeny PRIV
description: Przygotuj domenę PRIV z istniejącymi lub nowymi tożsamościami, które mają być zarządzane przez Microsoft Identity Manager przy użyciu skryptów
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
ms.openlocfilehash: a4d7c8ed4f5d7a9d6b4653caf03de69f1d18e509
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010629"
---
# <a name="step-1-configuring-the-priv-domain"></a>Krok 1 — Konfigurowanie domeny PRIV

> [!div class="step-by-step"]
> [Krok 2»](sp1-step2-configuring-corp-domain.md)

1. Zaloguj się do PRIVDC jako administrator
   * Jeśli to wdrożenie PAM jest środowiskiem PRIV-Only, zaloguj się do CORPDC
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 1 (PRIV Forest Configuration (Konfiguracja lasu PRIV)).


Konta usług wymagane do zarządzania SQL, SharePoint i MIM są tworzone automatycznie, jeśli nie znajdują się one jeszcze w domenie. Podczas wykonywania skryptu będą wyświetlane monity o podanie haseł w celu utworzenia tych kont usługi.

Jeśli domena PRIV jest w systemie Windows Server 2016 z poziomem funkcjonalności ustawionym na Windows Server 2016, skrypt wyświetli monit o włączenie opcjonalnej Active Directory "Privileged Access Management funkcji" wymagane przez funkcję PAM. Wybierz opcję „Tak”, aby kontynuować. Dla poziomów funkcjonalności poniżej systemu Windows Server 2016 należy odrzucić ostrzeżenie informujące o tym, że dodatkowa konfiguracja nie zostanie przeprowadzona. Należy ponownie uruchomić konfigurację PAMDeployment.ps1 i lasu PAM, gdy administrator programu ustawi poziom funkcjonalności systemu Windows Server 2016.

>[!NOTE]
>Następujące kroki nie są wymagane w przypadku konfiguracji PRIVOnly

7. Skopiuj plik SIDs.txt, który został wygenerowany w folderze $env:SYSTEMDRIVE\PAM do podobnego folderu w kontrolerze domeny CORPDC.
   * Ta lista identyfikatorów SID będzie potrzebna w CORPDC, aby skonfigurować uprawnienia w kolejnym kroku dla użytkowników PRIV, aby mogli odczytywać właściwości użytkownika CORP.
8. Po zakończeniu działania skryptu zostanie wyświetlony monit o ponowne uruchomienie maszyny, dzięki czemu zmiany zaczną obowiązywać.

> [!div class="step-by-step"]
> [Krok 2»](sp1-step2-configuring-corp-domain.md)
