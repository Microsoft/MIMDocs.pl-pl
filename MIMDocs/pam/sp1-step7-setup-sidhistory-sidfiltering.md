---
title: "Krok 7 — Konfigurowanie historii/filtrowania identyfikatorów SID"
description: "To jest krok 7 konfigurowania programu Privileged Identity Manager za pomocą skryptów. Ten krok obejmuje konfigurowanie historii/filtrowania identyfikatorów SID."
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
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: e608593f40759e3bc995daa56c4575510a71e987


---

# <a name="step-7-set-up-sid-historysid-filtering"></a>Krok 7 — Konfigurowanie historii/filtrowania identyfikatorów SID

>[!div class="step-by-step"]
[« Krok 6](sp1-step6-setup-pam-trust.md)
[Krok 8 »](sp1-step8-pam-deployment-verification.md)

**Ten krok nie jest wymagany dla środowiska PRIVOnly**. Zaloguj się na serwerze PAMServer przy użyciu konta MIMAdmin.

1. Zaloguj się do kontrolera domeny CORP jako administrator.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 8 (Setup SID history/SID filtering (Konfigurowanie historii/filtrowania identyfikatorów SID)).

Po pomyślnym wykonaniu pojawią się następujące komunikaty:<br/></br>
Filtrowanie identyfikatorów SID: <br/></br>
„Ustawianie braku filtrowania identyfikatorów SID w zaufaniu” lub „Filtrowanie identyfikatorów SID nie jest włączone dla tego zaufania”. </br></br>
Historia SID: </br></br>
„Włączanie historii SID dla tego zaufania” lub „Historia identyfikatorów SID jest już włączona dla tego zaufania”.

>[!div class="step-by-step"]
[« Krok 6](sp1-step6-setup-pam-trust.md)
[Krok 8 »](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Jan17_HO2-->


