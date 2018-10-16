---
title: Konfigurowanie programu MIM 2016 pod kątem usługi Privileged Access Management | Dokumentacja firmy Microsoft
description: Przewodnik instalacji programu MIM i konfigurowania go pod kątem usługi Privileged Access Management.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ade3557c99dc2e0623000f47df0e9519178c0641
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334061"
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Konfigurowanie środowiska programu MIM na potrzeby usługi Privileged Access Management

Proces konfigurowania środowiska pod kątem dostępu między lasami, instalowania i konfigurowania usługi Active Directory i programu Microsoft Identity Manager oraz demonstrowania żądania dostępu just in time obejmuje siedem kroków.

Te kroki zostały przedstawione, aby można było zacząć od zera i utworzyć środowisko testowe. W przypadku stosowania funkcji PAM w istniejącym środowisku można użyć własnych kontrolerów domeny lub kont użytkowników zamiast tworzyć nowe zgodne z przykładami.

1. Przygotowanie serwera *CORPDC* do działania jako kontroler domeny i stacji roboczej *CORPWKSTN* do działania jako członkowskiej stacji roboczej.

2. Przygotowanie serwera *PRIVDC* do działania jako kontroler domeny.

3.  Przygotowanie serwera *PAMSRV* w lesie *PRIV*.

4.  Zainstalowanie składników programu MIM na serwerze *PAMSRV* i poleceń cmdlet na członkowskiej stacji roboczej lasu *CONTOSO* oraz przygotowanie tych elementów na potrzeby funkcji Privileged Access Management.

5.  Ustanowienie relacji zaufania między lasami *PRIV* i *CONTOSO*.

6.  Przygotowanie uprzywilejowanych grup zabezpieczeń z dostępem do chronionych zasobów i kont członków na potrzeby funkcji Privileged Access Management just in time.

7.  Zaprezentowanie żądania, odbierania i używania uprzywilejowanego dostępu z podwyższonym poziomem uprawnień do chronionych zasobów.

> [!div class="step-by-step"]
> [Początek »](step-1-prepare-corp-domain.md)
