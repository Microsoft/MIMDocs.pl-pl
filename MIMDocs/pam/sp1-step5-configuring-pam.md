---
title: "Krok 5 — Instalowanie/konfigurowanie usługi PAM"
description: "Jest to krok 5 konfigurowania programu Privileged Identity Manager za pomocą skryptów. Obejmuje on kroki wdrażania na serwerze usługi PAM."
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
ms.translationtype: MT
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 862f62ab9bac87bcee31c35e249db34740e9fb14
ms.contentlocale: pl-pl
ms.lasthandoff: 07/10/2017


---
# Krok 5 — Instalowanie/konfigurowanie usługi PAM
<a id="step-5-installingconfiguring-pam" class="xliff"></a>

>[!div class="step-by-step"]
[« Krok 4](sp1-step4-configuring-sharepoint.md)
[Krok 6 »](sp1-step6-setup-pam-trust.md)

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

>[!div class="step-by-step"]
[« Krok 4](sp1-step4-configuring-sharepoint.md)
[Krok 6 »](sp1-step6-setup-pam-trust.md)

