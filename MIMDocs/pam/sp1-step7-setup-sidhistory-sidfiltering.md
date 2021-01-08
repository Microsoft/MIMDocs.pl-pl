---
title: Krok 7 — Konfigurowanie historii/filtrowania identyfikatorów SID
description: Krok 7 konfigurowania Microsoft Identity Manager przy użyciu skryptów. Ten krok obejmuje konfigurowanie historii/filtrowania identyfikatorów SID.
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
ms.openlocfilehash: c09649bbecfb4608391fef1cda3d8da87ded888b
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010646"
---
# <a name="step-7-setup-sid-historysid-filtering"></a>Krok 7 — Konfigurowanie historii/filtrowania identyfikatorów SID

> [!div class="step-by-step"]
> [«Krok 6](sp1-step6-setup-pam-trust.md) 
>  [Krok 8»](sp1-step8-pam-deployment-verification.md)

**Następujące polecenia nie są wymagane w przypadku środowiska tylko priv** Zaloguj się do serwerze pamserver przy użyciu konta MIMAdmin.

1. Zaloguj się do kontrolera domeny CORP jako administrator
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 8 (Setup SID history/SID filtering (Konfigurowanie historii/filtrowania identyfikatorów SID)).

Po pomyślnym wykonaniu pojawią się następujące komunikaty:<br/></br>
Filtrowanie identyfikatorów SID: <br/></br>
„Ustawianie braku filtrowania identyfikatorów SID w zaufaniu” lub „Filtrowanie identyfikatorów SID nie jest włączone dla tego zaufania”. </br></br>
Historia SID: </br></br>
„Włączanie historii SID dla tego zaufania” lub „Historia identyfikatorów SID jest już włączona dla tego zaufania”.

> [!div class="step-by-step"]
> [«Krok 6](sp1-step6-setup-pam-trust.md) 
>  [Krok 8»](sp1-step8-pam-deployment-verification.md)
