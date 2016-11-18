---
title: "Wdrażanie programu MIM 2016| Dokumentacja firmy Microsoft"
description: "Pobierz pełną listę kroków związanych z wdrażaniem programu Microsoft Identity Manager 2016 od przygotowania środowiska do konfigurowania portali."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 09/07/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 7fdad09650af245157264778d2732ac4327f25b5


---

# <a name="deploy-mim-2016"></a>Wdrażanie programu MIM 2016
Artykuły w tej sekcji zawierają instrukcje krok po kroku dotyczące wdrażania programu Microsoft Identity Manager (MIM) 2016 w scenariuszach samoobsługi użytkownika końcowego na serwerze, na którym nie wdrożono jeszcze usługi FIM ani programu MIM.

> [!NOTE]
> Topologia wdrożenia opisana w tej sekcji służy tylko do rozpoczęcia pracy i poznawania programu MIM.  W [przewodniku planowania pojemności](/microsoft-identity-manager/plan-design/capacity-planning-guide) zamieszczono więcej informacji dotyczących topologii wdrożeń produkcyjnych.  Firma Microsoft zaleca przejrzenie tej dokumentacji przed wdrożeniem programu MIM na skalę produkcyjną lub do użytku produkcyjnego.

Scenariusz zarządzania dostępem uprzywilejowanym jest wdrażany inaczej niż inne scenariusze z programem MIM, ponieważ wymaga dedykowanego środowiska lasu bastionu.  Jeśli chcesz dowiedzieć się więcej na temat wdrażania programu MIM dla usługi Privileged Identity Management, zobacz [Konfigurowanie środowiska programu MIM na potrzeby usługi Privileged Access Management](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam).

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



<!--HONumber=Nov16_HO2-->


