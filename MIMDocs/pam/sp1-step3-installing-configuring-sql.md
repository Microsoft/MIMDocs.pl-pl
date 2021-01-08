---
title: Krok 3 — Konfigurowanie serwera SQL
description: W tym artykule przedstawiono krok 3 serii artykułów obejmujących sposób konfigurowania Microsoft Identity Manager przy użyciu skryptów i omówiono kroki konfiguracji programu SQL Server.
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
ms.openlocfilehash: e4317363084da53e5683cd1a9b3d7bd0ce3ebcda
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010527"
---
# <a name="step-3-configuring-sql"></a>Krok 3 — Konfigurowanie serwera SQL

> [!div class="step-by-step"]
> [«Krok 2](sp1-step2-configuring-corp-domain.md) 
>  [Krok 4»](sp1-step4-configuring-sharepoint.md)

Przed przejściem do kolejnych czynności upewnij się, że używasz programu SQL Server 2012 z dodatkiem SP1 lub nowszym lub programu SQL Server 2014. W przypadku serwerów przyłączonych do domeny Zaloguj się przy użyciu konta MIMAdmin, w przeciwnym razie zaloguj się jako administrator lokalny.
1. Uruchom program PowerShell jako administrator.
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Wybierz opcję menu 3 (SQL Server Setup (Konfiguracja serwera SQL)).

   Jeśli serwer nie został jeszcze przyłączony do domeny PRIV, zostanie wyświetlony monit o podanie poświadczeń i przyłączenie serwera do domeny.
   Po przyłączeniu do domeny nastąpi ponowne uruchomienie komputera. Po pomyślnym ponownym uruchomieniu zaloguj się na serwerze przy użyciu konta MIMAdmin.

5. Uruchom program PowerShell jako administrator.
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Wybierz opcję menu 3 (SQL Server Setup (Konfiguracja serwera SQL)).

Po wyświetleniu monitu podaj hasło dla konta usługi MIMAdmin i zezwól na kontynuowanie instalacji. Po wykonaniu tych czynności przejdź do kroku 4.

> [!div class="step-by-step"]
> [«Krok 2](sp1-step2-configuring-corp-domain.md) 
>  [Krok 4»](sp1-step4-configuring-sharepoint.md)
