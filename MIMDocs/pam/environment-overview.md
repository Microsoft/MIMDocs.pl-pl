---
title: "Omówienie środowiska | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: b8af77d2354428da19d91d5f02b490012835f544
ms.openlocfilehash: a01cb2e1df52f3157b3d84a4eab837cececfbe1b


---

# Omówienie środowiska

Funkcja PAM pracuje z maszynami wirtualnymi z oddzielnymi dyskami, które są połączone ze sobą w sieci udostępnionej. Na tych maszynach wirtualnych może działać system Windows 8.1, Windows Server 2012 R2 lub inne platformy systemu operacyjnego.

![Serwery PAM: relacje i obsługiwane platformy — diagram](media/pam-test-lab-architecture.png)

Potrzebne będą co najmniej trzy maszyny wirtualne.  Jeśli nie masz jeszcze domeny usługi AD do zarządzania przez funkcję PAM, wymagana będzie jedna dodatkowa maszyna wirtualna działająca jako kontroler domeny CORP.  Jeśli chcesz skonfigurować oprogramowanie PRIV w celu uzyskania wysokiej dostępności, wymagane będą również dwie dodatkowe maszyny wirtualne.

Na dyskach, na których będą przechowywane obrazy dysków maszyny wirtualnej, musi być co najmniej 120 GB wolnego miejsca do przechowywania wszystkich maszyn wirtualnych.  Jeśli planujesz przeprowadzić wdrożenie w celu uzyskania wysokiej dostępności, upewnij się, że podsystem dysku spełnia wymagania udostępnionego magazynu SQL.  Magazyn udostępniony mogą stanowić dyski klastra trybu failover systemu Windows Server, dyski w sieci SAN lub udziały plików na serwerze SMB. Należy pamiętać, że elementy te muszą być przeznaczone wyłącznie dla środowiska bastionu. Udostępnianie magazynu z innymi obciążeniami poza środowisko bastionu nie jest zalecane, ponieważ może zagrozić integralności środowiska bastionu.

> [!NOTE]
> Bieżąca wersja Customer Technical Preview (CTP) programu MIM nie jest zgodna z bazą danych ani zawartością katalogu z poprzedniej wersji CTP. Jeśli wcześniej miała miejsce ocena scenariusza programu MIM dla funkcji PAM lub innych scenariuszy, wykonaj i zarchiwizuj kopię zapasową maszyn wirtualnych używanych na potrzeby tego testu, a następnie rozpocznij wdrażanie przy użyciu nowych obrazów maszyny wirtualnej, które nie były wcześniej używane w scenariuszach dotyczących programu MIM.



<!--HONumber=Jun16_HO3-->


