---
title: Omówienie środowiska usługi PAM | Dokumentacja firmy Microsoft
description: Określenie wymaganej liczby maszyn wirtualnych i ich konfiguracji, dzięki czemu można pomyślnie wdrożyć usługę Privileged Access Management
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4e3a3fb2b23deced2202994463645521beef81e8
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332922"
---
# <a name="environment-overview"></a>Omówienie środowiska

Usługa Privileged Access Management działa na maszynach wirtualnych z oddzielnymi dyskami, które są ze sobą połączone za pomocą współużytkowanej sieci. Na tych maszynach wirtualnych może działać system Windows 8.1, Windows Server 2012 R2 lub inne platformy systemu operacyjnego.

![Serwery PAM: relacje i obsługiwane platformy — diagram](media/pam-test-lab-architecture.png)

Należy co najmniej trzech maszyn wirtualnych.  Jeśli nie masz jeszcze domeny usługi AD dla funkcji PAM służący do zarządzania, konieczne będzie jedna dodatkowa maszyna wirtualna działa jako kontroler domeny CORP.  Jeśli chcesz skonfigurować oprogramowanie PRIV w celu zapewnienia wysokiej dostępności, należy się dwie dodatkowe maszyny wirtualne.

Dysków, w którym będą przechowywane obrazy dysków maszyny Wirtualnej należy co najmniej 120 GB wolnego miejsca na dysku.  Jeśli planujesz przeprowadzić wdrożenie w celu uzyskania wysokiej dostępności, upewnij się, że podsystem dysku spełnia wymagania udostępnionego magazynu SQL.  Magazyn udostępniony mogą stanowić dyski klastra trybu failover systemu Windows Server, dyski w sieci SAN lub udziały plików na serwerze SMB.

> [!IMPORTANT]
> Magazyn musi zostać przeznaczona dla środowiska bastionu. Udostępnianie magazynu z innymi obciążeniami poza środowisko bastionu nie jest zalecane, ponieważ może zagrozić integralności środowiska bastionu.

## <a name="next-steps"></a>Kolejne kroki

- [Privileged Access Management dla usług domenowych w usłudze Active Directory](privileged-identity-management-for-active-directory-domain-services.md) zawiera omówienie usługi PAM i sposób jej działania.
- [Opis składników funkcji PAM](principles-of-operation.md) zawiera omówienie różnych składników funkcji PAM.