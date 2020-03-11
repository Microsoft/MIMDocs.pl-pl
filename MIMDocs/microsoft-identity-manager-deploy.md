---
title: Kroki wdrażania programu Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft
description: Pobierz pełną listę kroków związanych z wdrażaniem programu Microsoft Identity Manager 2016 od przygotowania środowiska do konfigurowania portali.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 5a80718b038a10bb8d746a86856b87d016783fd6
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043192"
---
# <a name="deploy-microsoft-identity-manager-2016-sp2"></a>Wdrażanie Microsoft Identity Manager 2016 z dodatkiem SP2
Artykuły w tej sekcji zawierają instrukcje krok po kroku dotyczące wdrażania programu Microsoft Identity Manager (MIM) 2016 w scenariuszach samoobsługi użytkownika końcowego na serwerze, na którym nie wdrożono jeszcze usługi FIM ani programu MIM.

> [!NOTE]
> Topologia wdrożenia opisana w tej sekcji służy tylko do rozpoczęcia pracy i poznawania programu MIM.  W [przewodniku planowania pojemności](capacity-planning-guide.md) zamieszczono więcej informacji dotyczących topologii wdrożeń produkcyjnych.  Firma Microsoft zaleca przejrzenie tej dokumentacji przed wdrożeniem programu MIM na skalę produkcyjną lub do użytku produkcyjnego.

Scenariusz zarządzania dostępem uprzywilejowanym jest wdrażany inaczej niż inne scenariusze z programem MIM, ponieważ wymaga dedykowanego środowiska lasu bastionu.  Jeśli chcesz dowiedzieć się więcej na temat wdrażania programu MIM dla usługi Privileged Identity Management, zobacz [Konfigurowanie środowiska programu MIM na potrzeby usługi Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

Proces wdrażania programu MIM jest bardzo podobny do procesu jego poprzednika, FIM 2010 R2. Jeśli chcesz zapoznać się z dokumentacją programu FIM, zobacz [Forefront Identity Manager 2010 R2 Deployment Guide](https://technet.microsoft.com/library/jj134310) (Podręcznik wdrażania programu Forefront Identity Manager 2010 R2).

## <a name="first-prepare-a-domain"></a>Po pierwsze: przygotowanie domeny
Program MIM współpracuje z usługą Active Directory (AD), dlatego należy wykonać następujące kroki, aby skonfigurować kontroler domeny usługi AD.
- [Konfiguracja domeny](preparing-domain.md) lub
- [Konfiguracja domeny dla kont usług zarządzanych przez grupę](preparing-domain-gmsa.md)


## <a name="next-prepare-an-identity-management-servers"></a>Dalej: przygotowywanie serwerów zarządzania tożsamościami
Po utworzeniu i skonfigurowaniu domeny należy przygotować firmowy serwer zarządzania tożsamością.

Aby uzyskać więcej informacji na temat obsługiwanych platform, zobacz [obsługiwane platformy dla programu MIM 2016 lub nowszego](microsoft-identity-manager-2016-supported-platforms.md)

 Obejmuje to konfigurowanie następujących składników:
- [System Windows Server](prepare-server-ws2016.md)
- [Program SQL Server](prepare-server-sql2016.md)
- [Serwer programu SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcjonalnie)

## <a name="finally-install-microsoft-identity-manager-2016-sp2-components"></a>Na koniec: Zainstaluj składniki Microsoft Identity Manager 2016 SP2
Po skonfigurowaniu domeny i serwera można zainstalować składniki programu MIM i skonfigurować je do synchronizacji z usługą AD.
- [Usługa synchronizacji programu MIM](install-mim-sync.md)
- [Usługa i portal MIM](install-mim-service-portal.md)
- [Synchronizowanie baz danych usług Active Directory i MIM](install-mim-sync-ad-service.md)
