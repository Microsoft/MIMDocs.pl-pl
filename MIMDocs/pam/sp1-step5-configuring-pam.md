---
title: "Krok 5 — Instalowanie/konfigurowanie usługi PAM"
description: "Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager, z użyciem skryptów"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a5d86991f1579f292d7d303148422cef746d008a


---
# Krok 5 — Instalowanie/konfigurowanie usługi PAM

W przypadku serwera PAMServer przyłączonego do domeny należy zalogować się jako MIMAdmin; w przypadku innego serwera należy zalogować się jako administrator lokalny.
1. Uruchom program PowerShell jako administrator.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wybierz opcję menu 5 (MIM PAM Setup (Konfiguracja usługi PAM programu MIM)).

>[!NOTE] Jeśli komputer nie jest już przyłączony do domeny PRIV, zostanie wyświetlony monit o poświadczenia. Po przyłączeniu do domeny nastąpi ponowne uruchomienie komputera.

Po ponownym uruchomieniu serwera PAMServer ponownie zaloguj się do komputera przy użyciu konta MIMAdmin.

1. Uruchom program PowerShell jako administrator.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wybierz opcję menu 5 (MIM PAM Setup (Konfiguracja usługi PAM programu MIM)).

  Po wyświetleniu monitu wprowadź hasło do konta monitora programu MIM, konta składnika MIM, konta agenta zarządzania (MA) programu MIM, konta usługi MIM, konta administratora MIM i programu SharePoint.
  Po zakończeniu instalacji nastąpi ponowne uruchomienie komputera.



<!--HONumber=Sep16_HO4-->


