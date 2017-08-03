---
title: "Kroki wdrażania programu Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft"
description: "Pobierz pełną listę kroków związanych z wdrażaniem programu Microsoft Identity Manager 2016 od przygotowania środowiska do konfigurowania portali."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fa200bb18871387420743af64ca196565397e5d5
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="deploy-mim-2016"></a>Wdrażanie programu MIM 2016
Artykuły w tej sekcji zawierają instrukcje krok po kroku dotyczące wdrażania programu Microsoft Identity Manager (MIM) 2016 w scenariuszach samoobsługi użytkownika końcowego na serwerze, na którym nie wdrożono jeszcze usługi FIM ani programu MIM.

> [!NOTE]
> Topologia wdrożenia opisana w tej sekcji służy tylko do rozpoczęcia pracy i poznawania programu MIM.  W [przewodniku planowania pojemności](capacity-planning-guide.md) zamieszczono więcej informacji dotyczących topologii wdrożeń produkcyjnych.  Firma Microsoft zaleca przejrzenie tej dokumentacji przed wdrożeniem programu MIM na skalę produkcyjną lub do użytku produkcyjnego.

Scenariusz zarządzania dostępem uprzywilejowanym jest wdrażany inaczej niż inne scenariusze z programem MIM, ponieważ wymaga dedykowanego środowiska lasu bastionu.  Jeśli chcesz dowiedzieć się więcej na temat wdrażania programu MIM dla usługi Privileged Identity Management, zobacz [Konfigurowanie środowiska programu MIM na potrzeby usługi Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

Proces wdrażania programu MIM 2016 jest bardzo podobny do procesu wdrażania jego poprzednika, programu FIM 2010 R2. Jeśli chcesz zapoznać się z dokumentacją programu FIM, zobacz [Forefront Identity Manager 2010 R2 Deployment Guide](https://technet.microsoft.com/library/jj134310) (Podręcznik wdrażania programu Forefront Identity Manager 2010 R2).

## <a name="first-prepare-a-domain"></a>Po pierwsze: przygotowanie domeny
Program MIM współpracuje z usługą Active Directory (AD), dlatego należy wykonać następujące kroki, aby skonfigurować kontroler domeny usługi AD.
- [Konfiguracja domeny](preparing-domain.md)

## <a name="next-prepare-an-identity-management-server"></a>Następnie: przygotowanie serwera zarządzania tożsamością
Po utworzeniu i skonfigurowaniu domeny należy przygotować firmowy serwer zarządzania tożsamością. Obejmuje to konfigurowanie następujących składników:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcjonalnie)

## <a name="finally-install-microsoft-identity-manager-2016-components"></a>Na zakończenie: instalowanie składników programu Microsoft Identity Manager 2016
Po skonfigurowaniu domeny i serwera można zainstalować składniki programu MIM i skonfigurować je do synchronizacji z usługą AD.
- [Usługa synchronizacji programu MIM](install-mim-sync.md)
- [Usługa i portal MIM](install-mim-service-portal.md)
- [Synchronizowanie baz danych usług Active Directory i MIM](install-mim-sync-ad-service.md)
