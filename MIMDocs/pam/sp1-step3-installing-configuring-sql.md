---
title: Krok 3 — Konfigurowanie serwera SQL
description: Ten artykuł stanowi krok 3 w serii artykułów obejmujących konfigurowanie programu Privileged Identity Manager za pomocą skryptów. Omówiono w nim kroki konfiguracji serwera SQL.
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
ms.openlocfilehash: e9bc0345358a634adb0d7c0bdf9bd0f22ccea27e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043821"
---
# <a name="step-3-configuring-sql"></a>Krok 3 — Konfigurowanie serwera SQL

> [!div class="step-by-step"]
> [«Krok 2](sp1-step2-configuring-corp-domain.md)
> [krok 4»](sp1-step4-configuring-sharepoint.md)

Przed przejściem do kolejnych czynności upewnij się, że używasz programu SQL Server 2012 z dodatkiem SP1 lub nowszym lub programu SQL Server 2014. W przypadku serwerów przyłączonych do domeny należy zalogować się przy użyciu danych konta MIMAdmin; w przypadku innych serwerów należy zalogować się przy użyciu konta administratora lokalnego.
1. Uruchom program PowerShell jako administrator.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wybierz opcję menu 3 (SQL Server Setup (Konfiguracja serwera SQL)).

   Jeśli serwer nie został jeszcze przyłączony do domeny PRIV, zostanie wyświetlony monit o podanie poświadczeń i przyłączenie serwera do domeny.
   Po przyłączeniu do domeny nastąpi ponowne uruchomienie komputera. Po pomyślnym ponownym uruchomieniu należy zalogować się na serwer przy użyciu konta MIMAdmin.

5. Uruchom program PowerShell jako administrator.
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Wybierz opcję menu 3 (SQL Server Setup (Konfiguracja serwera SQL)).

Po wyświetleniu monitu podaj hasło dla konta usługi MIMAdmin i zezwól na kontynuowanie instalacji. Po wykonaniu tych czynności przejdź do kroku 4.

> [!div class="step-by-step"]
> [«Krok 2](sp1-step2-configuring-corp-domain.md)
> [krok 4»](sp1-step4-configuring-sharepoint.md)
