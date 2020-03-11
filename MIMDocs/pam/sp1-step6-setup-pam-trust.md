---
title: Krok 6 — Konfigurowanie zaufania usługi PAM
description: Krok 6 konfigurowania usługi PAM za pomocą skryptów. W tej sekcji omówiono sposób konfigurowania niezbędnej relacji zaufania między domenami CORP i PRIV
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
ms.openlocfilehash: baab111f1b8f0f290611b9b63bb6981d8ae9538a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043770"
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
