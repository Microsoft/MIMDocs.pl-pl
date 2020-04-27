---
title: Omówienie środowiska usługi PAM | Dokumentacja firmy Microsoft
description: Określenie wymaganej liczby maszyn wirtualnych i ich konfiguracji, dzięki czemu można pomyślnie wdrożyć usługę Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c758bd924c6f7272a44567c2e9dc919215cdd273
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044008"
---
# <a name="environment-overview"></a>Omówienie środowiska

Usługa Privileged Access Management działa na maszynach wirtualnych z oddzielnymi dyskami, które są ze sobą połączone za pomocą współużytkowanej sieci. Na tych maszynach wirtualnych może działać system Windows 8.1, Windows Server 2012 R2 lub inne platformy systemu operacyjnego.

![Serwery PAM: relacje i obsługiwane platformy — diagram](media/pam-test-lab-architecture.png)

Wymagana jest co najmniej trzy maszyny wirtualne.  Jeśli nie masz jeszcze domeny usługi AD do zarządzania usługą PAM, musisz mieć jedną dodatkową maszynę wirtualną, która będzie działać jako kontroler domeny CORP.  Jeśli chcesz skonfigurować oprogramowanie PRIV pod kątem wysokiej dostępności, potrzebujesz dwóch dodatkowych maszyn wirtualnych.

Dyski, na których będą przechowywane obrazy dysków maszyny wirtualnej, wymagają co najmniej 120 GB wolnego miejsca na dysku.  Jeśli planujesz przeprowadzić wdrożenie w celu uzyskania wysokiej dostępności, upewnij się, że podsystem dysku spełnia wymagania udostępnionego magazynu SQL.  Magazyn udostępniony mogą stanowić dyski klastra trybu failover systemu Windows Server, dyski w sieci SAN lub udziały plików na serwerze SMB.

> [!IMPORTANT]
> Magazyn musi być dedykowany dla środowiska bastionu. Nie zaleca się udostępniania magazynu z innymi obciążeniami poza środowiskiem bastionu, ponieważ może to spowodować zagrożenie integralności środowiska bastionu.

## <a name="next-steps"></a>Następne kroki

- [Privileged Access Management dla Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) to Omówienie usługi Pam i sposobu jej działania.
- [Zrozumienie składników usługi PAM](principles-of-operation.md) to omówienie różnych składników usługi PAM.
