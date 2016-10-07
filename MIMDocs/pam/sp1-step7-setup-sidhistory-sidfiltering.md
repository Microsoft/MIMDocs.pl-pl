---
title: "Krok 7 — Konfigurowanie historii/filtrowania identyfikatorów SID"
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
ms.openlocfilehash: 4c5cfa92f3111a6d298f586ba547a1eca2502853


---

# Konfigurowanie historii/filtrowania identyfikatorów SID

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



<!--HONumber=Sep16_HO4-->


