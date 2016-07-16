---
title: "Konfigurowanie środowiska programu MIM na potrzeby funkcji Privileged Access Management | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 9cf126d898c93faf89d7119136cce4e4963bb63d
ms.openlocfilehash: c9f2cf2ba1f42ea1513ae38d8089839d85ae5553


---

# Konfigurowanie środowiska programu MIM na potrzeby funkcji Privileged Access Management
Proces konfigurowania środowiska pod kątem dostępu między lasami, instalowania i konfigurowania usługi Active Directory i programu Microsoft Identity Manager oraz demonstrowania żądania dostępu just in time obejmuje siedem kroków.

Te kroki zostały przedstawione, aby można było zacząć od zera i utworzyć środowisko testowe. W przypadku stosowania funkcji PAM w istniejącym środowisku można użyć własnych kontrolerów domeny lub kont użytkowników zamiast tworzyć nowe zgodne z przykładami.

1.  Przygotowanie serwera *CORPDC* do działania jako kontroler domeny i stacji roboczej *CORPWKSTN* do działania jako członkowskiej stacji roboczej.

2.  Przygotowanie serwera *PRIVDC* do działania jako kontroler domeny.

3.  Przygotowanie serwera *PAMSRV* w lesie *PRIV*.

4.  Zainstalowanie składników programu MIM na serwerze *PAMSRV* i poleceń cmdlet na członkowskiej stacji roboczej lasu *CONTOSO* oraz przygotowanie tych elementów na potrzeby funkcji Privileged Access Management.

5.  Ustanowienie relacji zaufania między lasami *PRIV* i *CONTOSO*.

6.  Przygotowanie uprzywilejowanych grup zabezpieczeń z dostępem do chronionych zasobów i kont członków na potrzeby funkcji Privileged Access Management just in time.

7.  Zaprezentowanie żądania, odbierania i używania uprzywilejowanego dostępu z podwyższonym poziomem uprawnień do chronionych zasobów.

>[!div class="step-by-step"] [Początek »](step-1-prepare-corp-domain.md)



<!--HONumber=Jun16_HO3-->


