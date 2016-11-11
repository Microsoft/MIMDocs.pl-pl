---
title: "Krok 3 — Konfigurowanie serwera SQL"
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
ms.openlocfilehash: 375a34e5255c90559fc0ffb3a80fc7c92ebd27a2


---
# <a name="step-3-configuring-sql"></a>Krok 3 — Konfigurowanie serwera SQL

>[!div class="step-by-step"]
[« Krok 2](sp1-step2-configuring-corp-domain.md)
[Krok 4 »](sp1-step4-configuring-sharepoint.md)

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

>[!div class="step-by-step"]
[« Krok 2](sp1-step2-configuring-corp-domain.md)
[Krok 4 »](sp1-step4-configuring-sharepoint.md)



<!--HONumber=Nov16_HO2-->


