---
title: Krok 5 — Instalowanie/konfigurowanie usługi PAM
description: Jest to krok 5 konfigurowania Microsoft Identity Manager przy użyciu skryptów i obejmuje kroki wdrażania na serwerze PAM.
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
ms.openlocfilehash: 01fe9cd7704674f408e0b9b5a27673989d0eaecf
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010594"
---
# <a name="step-5-installingconfiguring-pam"></a>Krok 5 — Instalowanie/konfigurowanie usługi PAM

> [!div class="step-by-step"]
> [«Krok 4](sp1-step4-configuring-sharepoint.md) 
>  [Krok 6»](sp1-step6-setup-pam-trust.md)

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
>  [Krok 6»](sp1-step6-setup-pam-trust.md)
