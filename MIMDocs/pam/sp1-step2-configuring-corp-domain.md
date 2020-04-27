---
title: Krok 2 — Konfigurowanie domeny CORP
description: W tym artykule opisano drugi krok wymagany do skonfigurowania domeny CORP, który polega na uruchomieniu skryptu po skopiowaniu pliku sids.txt do domeny CORPDC
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
ms.openlocfilehash: 1215e0be4d978e879ebc09ecdd99223fb3667a85
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043838"
---
# <a name="step-2-configuring-the-corp-domain"></a>Krok 2 — Konfigurowanie domeny CORP

> [!div class="step-by-step"]
> [«Krok 1](sp1-step1-configuring-priv-domain.md)
> [krok 3»](sp1-step3-installing-configuring-sql.md)

Po skopiowaniu pliku SIDs.txt do kontrolera domeny CORPDC **te czynności nie są wymagane w przypadku wdrożeń PRIVOnly**.

1. Zaloguj się do kontrolera domeny CORPDC jako administrator.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 2 (CORP Forest Configuration (Konfiguracja lasu CORP)).

> [!div class="step-by-step"]
> [«Krok 1](sp1-step1-configuring-priv-domain.md)
> [krok 3»](sp1-step3-installing-configuring-sql.md)
