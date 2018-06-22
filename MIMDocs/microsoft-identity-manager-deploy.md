---
title: Kroki wdrażania programu Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft
description: Pobierz pełną listę kroków związanych z wdrażaniem programu Microsoft Identity Manager 2016 od przygotowania środowiska do konfigurowania portali.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3460436682054acf5e9e1b186c3fa39faaa40a43
ms.sourcegitcommit: 8316fa41f06f137dba0739a8700910116b5575d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/04/2018
ms.locfileid: "33079010"
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>Wdrażanie programu Microsoft Identity Manager 2016 z dodatkiem SP1
Artykuły w tej sekcji zawierają instrukcje krok po kroku dotyczące wdrażania programu Microsoft Identity Manager (MIM) 2016 w scenariuszach samoobsługi użytkownika końcowego na serwerze, na którym nie wdrożono jeszcze usługi FIM ani programu MIM.

> [!NOTE]
> Topologia wdrożenia opisana w tej sekcji służy tylko do rozpoczęcia pracy i poznawania programu MIM.  W [przewodniku planowania pojemności](capacity-planning-guide.md) zamieszczono więcej informacji dotyczących topologii wdrożeń produkcyjnych.  Firma Microsoft zaleca przejrzenie tej dokumentacji przed wdrożeniem programu MIM na skalę produkcyjną lub do użytku produkcyjnego.

Scenariusz zarządzania dostępem uprzywilejowanym jest wdrażany inaczej niż inne scenariusze z programem MIM, ponieważ wymaga dedykowanego środowiska lasu bastionu.  Jeśli chcesz dowiedzieć się więcej na temat wdrażania programu MIM dla usługi Privileged Identity Management, zobacz [Konfigurowanie środowiska programu MIM na potrzeby usługi Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

Proces wdrażania MIM jest bardzo podobny do procesu jego poprzednika, programu FIM 2010 R2. Jeśli chcesz zapoznać się z dokumentacją programu FIM, zobacz [Forefront Identity Manager 2010 R2 Deployment Guide](https://technet.microsoft.com/library/jj134310) (Podręcznik wdrażania programu Forefront Identity Manager 2010 R2).

## <a name="first-prepare-a-domain"></a>Po pierwsze: przygotowanie domeny
Program MIM współpracuje z usługą Active Directory (AD), dlatego należy wykonać następujące kroki, aby skonfigurować kontroler domeny usługi AD.
- [Konfiguracja domeny](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>Następnie: Tożsamość Przygotowanie serwerów zarządzania
Po utworzeniu i skonfigurowaniu domeny należy przygotować firmowy serwer zarządzania tożsamością. Obejmuje to konfigurowanie następujących składników:
- [Windows Server 2016](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [Program SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcjonalnie)

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>Zakończenie: Instalowanie programu Microsoft Identity Manager 2016 z dodatkiem SP1 składników
Po skonfigurowaniu domeny i serwera można zainstalować składniki programu MIM i skonfigurować je do synchronizacji z usługą AD.
- [Usługa synchronizacji programu MIM](install-mim-sync.md)
- [Usługa i portal MIM](install-mim-service-portal.md)
- [Synchronizowanie baz danych usług Active Directory i MIM](install-mim-sync-ad-service.md)
- [Platformy obsługiwane przez program MIM 2016 lub nowszej](microsoft-identity-manager-2016-supported-platforms.md)
