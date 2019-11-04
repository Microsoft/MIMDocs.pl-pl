---
title: Planowanie Microsoft Identity Manager 2016 w środowisku TLS 1,2 | Microsoft Docs
description: Planowanie Microsoft Identity Manager 2016 w środowisku TLS 1,2
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b574a9c8536a460cdddf57131a821775672c082a
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2019
ms.locfileid: "73383954"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>Planowanie programu MIM 2016 z dodatkiem SP2 w środowiskach TLS 1,2 lub w trybie FIPS


> [!IMPORTANT]
> Ten artykuł ma zastosowanie tylko do programu MIM 2016 SP2

W przypadku instalowania programu MIM 2016 z dodatkiem SP2 w środowisku zablokowanym, które ma wszystkie protokoły szyfrowania, ale protokół TLS 1,2 jest wyłączony, obowiązują następujące wymagania:
- Aby ustanowić bezpieczne składniki MIM protokołu TLS 1,2, wymagaj najnowszych aktualizacji dla systemu Windows Server i .NET Framework, które umożliwiają zainstalowanie obsługi protokołu TLS 1,2 w programie .NET 3,5 Framework. W zależności od konfiguracji serwera *może* być konieczne [włączenie SystemDefaultTlsVersions dla .NET Framework](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) i/lub [wyłączenie wszystkich protokołów SCHANNEL z wyjątkiem protokołu TLS 1,2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) w rejestrze w podkluczach *klienta* i *serwera* .

## <a name="mim-synchronization-service-sql-ma"></a>Usługa synchronizacji programu MIM, SQL MA

- Aby ustanowić bezpieczne połączenie TLS 1,2 z programem SQL Server, usługą synchronizacji programu MIM i wbudowanym agentem zarządzania SQL wymaga [SQL Native Client 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) lub nowszego.

## <a name="mim-service"></a>Usługa MIM
- Certyfikaty z podpisem własnym nie mogą być używane przez usługę MIM tylko w środowisku TLS 1,2. W przypadku instalowania usługi MIM wybierz certyfikat zgodny ze standardem szyfrowania wystawiony przez zaufany urząd certyfikacji.
- Instalator usługi programu MIM dodatkowo wymaga [sterownika OLE DB dla SQL Server w wersji 18,2](https://www.microsoft.com/download/details.aspx?id=56730) lub nowszej.

## <a name="fips-mode-considerations"></a>Zagadnienia dotyczące trybu FIPS

W przypadku instalowania usługi MIM na serwerze z włączoną obsługą trybu FIPS należy wyłączyć weryfikację zasad FIPS, aby zezwolić na wykonywanie przepływów pracy usługi MIM. W tym celu należy dodać *enforceFIPSPolicy Enabled = false* do sekcji *środowiska uruchomieniowego* pliku *Microsoft. ResourceManagement. Service. exe. config* między sekcjami *środowiska uruchomieniowego* i *zestawubinding* , jak przedstawiono poniżej:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    