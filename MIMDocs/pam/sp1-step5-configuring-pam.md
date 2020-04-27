---
title: Krok 5 — Instalowanie/konfigurowanie usługi PAM
description: Jest to krok 5 konfigurowania programu Privileged Identity Manager za pomocą skryptów. Obejmuje on kroki wdrażania na serwerze usługi PAM.
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
ms.openlocfilehash: 58a70336af4f79d87d6175aa99dc79fc81aa62dd
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043787"
---
# <a name="step-5-installingconfiguring-pam"></a>Krok 5 — Instalowanie/konfigurowanie usługi PAM

> [!div class="step-by-step"]
> [«Krok 4](sp1-step4-configuring-sharepoint.md)
> [krok 6»](sp1-step6-setup-pam-trust.md)

W przypadku serwera PAMServer przyłączonego do domeny należy zalogować się jako MIMAdmin; w przypadku innego serwera należy zalogować się jako administrator lokalny.
1. Uruchom program PowerShell jako administrator.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wybierz opcję menu 5 (MIM PAM Setup (Konfiguracja usługi PAM programu MIM)).

>[!NOTE]
>Jeśli komputer nie jest już przyłączony do domeny PRIV, zostanie wyświetlony monit o poświadczenia. Po przyłączeniu do domeny nastąpi ponowne uruchomienie komputera.

Po ponownym uruchomieniu serwera PAMServer ponownie zaloguj się do komputera przy użyciu konta MIMAdmin.

1. Uruchom program PowerShell jako administrator.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wybierz opcję menu 5 (MIM PAM Setup (Konfiguracja usługi PAM programu MIM)).

   Po wyświetleniu monitu wprowadź hasło do konta monitora programu MIM, konta składnika MIM, konta agenta zarządzania (MA) programu MIM, konta usługi MIM, konta administratora MIM i programu SharePoint.
   Po zakończeniu instalacji nastąpi ponowne uruchomienie komputera.

> [!div class="step-by-step"]
> [«Krok 4](sp1-step4-configuring-sharepoint.md)
> [krok 6»](sp1-step6-setup-pam-trust.md)
