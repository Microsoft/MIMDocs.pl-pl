---
title: "Krok 3 — Konfigurowanie serwera SQL"
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
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# Krok 3 — Konfigurowanie serwera SQL

Przed przejściem do kolejnych czynności upewnij się, że używasz programu SQL Server 2012 z dodatkiem SP1 lub nowszym lub programu SQL Server 2014. W przypadku serwerów przyłączonych do domeny należy zalogować się przy użyciu danych konta MIMAdmin; w przypadku innych serwerów należy zalogować się przy użyciu konta administratora lokalnego.
1. Uruchom program PowerShell jako administrator.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wybierz opcję menu 3 (SQL Server Setup (Konfiguracja serwera SQL)).

  Jeśli serwer nie został jeszcze przyłączony do domeny PRIV, zostanie wyświetlony monit o podanie poświadczeń i przyłączenie serwera do domeny.
  Po przyłączeniu do domeny nastąpi ponowne uruchomienie komputera. Po pomyślnym ponownym uruchomieniu należy zalogować się na serwer przy użyciu konta MIMAdmin.

1. Uruchom program PowerShell jako administrator.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wybierz opcję menu 3 (SQL Server Setup (Konfiguracja serwera SQL)).

Po wyświetleniu monitu podaj hasło dla konta usługi MIMAdmin i zezwól na kontynuowanie instalacji. Po wykonaniu tych czynności przejdź do kroku 4.



<!--HONumber=Sep16_HO4-->


