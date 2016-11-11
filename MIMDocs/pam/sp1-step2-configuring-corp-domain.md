---
title: "Krok 2 — Konfigurowanie domeny CORP"
description: "Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager, z użyciem skryptów"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


---

# <a name="step-2-configuring-the-corp-domain"></a>Krok 2 — Konfigurowanie domeny CORP

>[!div class="step-by-step"]
[« Krok 1](sp1-step1-configuring-priv-domain.md)
[Krok 3 »](sp1-step3-installing-configuring-sql.md)

Po skopiowaniu pliku SIDs.txt do kontrolera domeny CORPDC **te czynności nie są wymagane w przypadku wdrożeń PRIVOnly**.

1. Zaloguj się do kontrolera domeny CORPDC jako administrator.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 2 (CORP Forest Configuration (Konfiguracja lasu CORP)).

>[!div class="step-by-step"]
[« Krok 1](sp1-step1-configuring-priv-domain.md)
[Krok 3 »](sp1-step3-installing-configuring-sql.md)



<!--HONumber=Nov16_HO2-->


