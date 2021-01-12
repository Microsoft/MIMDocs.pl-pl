---
title: Omówienie środowiska usługi PAM | Dokumentacja firmy Microsoft
description: Określenie wymaganej liczby maszyn wirtualnych i ich konfiguracji, dzięki czemu można pomyślnie wdrożyć usługę Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fb38ee26d829acd5c54bf690f7a80f0659baaa85
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104075"
---
# <a name="mim-pam-test-lab-environment-overview"></a>Omówienie środowiska laboratorium testowego PAM programu MIM

> [!NOTE]
> Podejście PAM udostępnione przez program MIM jest przeznaczone do użycia w niestandardowej architekturze dla izolowanych środowisk, w których dostęp do Internetu nie jest dostępny, gdy ta konfiguracja jest wymagana przez regulację lub w środowiskach izolowanych o dużym wpływie, takich jak laboratoria badawcze w trybie offline i rozłączona technologia kontroli i środowiska pozyskiwania danych. Jeśli Active Directory jest częścią środowiska połączonego z Internetem, zobacz zamiast [zabezpieczania dostępu uprzywilejowanego](/security/compass/overview) , aby uzyskać więcej informacji na temat miejsca, w którym należy zacząć.

Aby skonfigurować laboratorium testowe modułu PAM programu MIM, można zainstalować oprogramowanie na maszynach wirtualnych.
Usługa Privileged Access Management działa na maszynach wirtualnych z oddzielnymi dyskami, które są ze sobą połączone za pomocą współużytkowanej sieci. Na tych maszynach wirtualnych może działać system Windows 8.1, Windows Server 2012 R2 lub inne platformy systemu operacyjnego.

![Serwery PAM: relacje i obsługiwane platformy — diagram](media/pam-test-lab-architecture.png)

Wymagana jest co najmniej trzy maszyny wirtualne.  Jeśli nie masz jeszcze domeny usługi AD do zarządzania usługą PAM, musisz mieć jedną dodatkową maszynę wirtualną, która będzie działać jako kontroler domeny CORP.  Jeśli chcesz skonfigurować oprogramowanie PRIV pod kątem wysokiej dostępności, potrzebujesz dwóch dodatkowych maszyn wirtualnych.

Dyski, na których będą przechowywane obrazy dysków maszyny wirtualnej, wymagają co najmniej 120 GB wolnego miejsca na dysku.  Jeśli planujesz przeprowadzić wdrożenie w celu uzyskania wysokiej dostępności, upewnij się, że podsystem dysku spełnia wymagania udostępnionego magazynu SQL.  Magazyn udostępniony mogą stanowić dyski klastra trybu failover systemu Windows Server, dyski w sieci SAN lub udziały plików na serwerze SMB.

> [!IMPORTANT]
> Magazyn musi być dedykowany dla środowiska bastionu. Nie zaleca się udostępniania magazynu z innymi obciążeniami poza środowiskiem bastionu, ponieważ może to spowodować zagrożenie integralności środowiska bastionu.

## <a name="next-steps"></a>Następne kroki

- [Privileged Access Management dla Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) to Omówienie usługi Pam i sposobu jej działania.
- [Zrozumienie składników usługi PAM](principles-of-operation.md) to omówienie różnych składników usługi PAM.
