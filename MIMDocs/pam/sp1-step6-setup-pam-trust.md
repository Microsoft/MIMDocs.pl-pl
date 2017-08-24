---
title: "Krok 6 — Konfigurowanie zaufania usługi PAM"
description: "Krok 6 konfigurowania usługi PAM za pomocą skryptów. W tej sekcji omówiono sposób konfigurowania niezbędnej relacji zaufania między domenami CORP i PRIV"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: a24b2a5a196e633379b696ee82e9428dd67454ca
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/19/2017
---
# <a name="step-6-set-up-the-pam-trust"></a>Krok 6 — Konfigurowanie zaufania usługi PAM

>[!div class="step-by-step"]
[« Krok 5](sp1-step5-configuring-pam.md)
[Krok 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Ten krok nie jest wymagany dla środowiska PRIVOnly**. Zaloguj się na serwerze PAMServer przy użyciu konta MIMAdmin.

1. Zaloguj się na serwerze PAMServer przy użyciu konta MIMAdmin.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 6 (PAM trust setup (Konfigurowanie zaufania usługi PAM)).

  Po wyświetleniu monitu wprowadź poświadczenia dla konta administratora domeny CORP. Po wprowadzeniu poświadczeń nastąpi ustanowienie zaufania, a konfiguracja zostanie ukończona.

>[!div class="step-by-step"]
[« Krok 5](sp1-step5-configuring-pam.md)
[Krok 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)
