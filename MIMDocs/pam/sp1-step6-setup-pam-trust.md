---
title: Krok 6 — Konfigurowanie zaufania usługi PAM
description: Krok 6 konfigurowania usługi PAM za pomocą skryptów. W tej sekcji omówiono sposób konfigurowania niezbędnej relacji zaufania między domenami CORP i PRIV
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: ad1482718693c9ae7004a71334013de68f7c20da
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379383"
---
# <a name="step-6-set-up-the-pam-trust"></a>Krok 6 — Konfigurowanie zaufania usługi PAM

> [!div class="step-by-step"]
> [« Krok 5](sp1-step5-configuring-pam.md)
> [Krok 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Ten krok nie jest wymagany dla środowiska PRIVOnly**. Zaloguj się na serwerze PAMServer przy użyciu konta MIMAdmin.

1. Zaloguj się na serwerze PAMServer przy użyciu konta MIMAdmin.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Wybierz opcję menu 6 (PAM trust setup (Konfigurowanie zaufania usługi PAM)).

   Po wyświetleniu monitu wprowadź poświadczenia dla konta administratora domeny CORP. Po wprowadzeniu poświadczeń nastąpi ustanowienie zaufania, a konfiguracja zostanie ukończona.

> [!div class="step-by-step"]
> [« Krok 5](sp1-step5-configuring-pam.md)
> [Krok 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)
