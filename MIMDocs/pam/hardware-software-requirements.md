---
title: Wymagania programowe usługi PAM | Dokumentacja firmy Microsoft
description: Określ wymagania sprzętowe i programowe, aby pomyślnie wdrożyć usługę Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8d3c266c78df21c6ddb24f62618621c55820bd6f
ms.sourcegitcommit: 0e2b4b47a8050737c78e3b0ad088358e5de7e929
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395456"
---
# <a name="hardware-and-software-requirements"></a>Wymagania dotyczące sprzętu i oprogramowania

Privileged Access Management nie ma wymagań sprzętowych wykraczających poza te wymagania dotyczące podstawowych platform oprogramowania. Po prostu upewnij się, że masz wystarczającą ilość pamięci lub miejsca na dysku i łączność sieciową.

> [!IMPORTANT]
> Ten artykuł zawiera minimalne wymagania dotyczące podstawowego wdrożenia w sieci izolowanej. Nie ma na celu prezentowania wydajności, skalowalności ani wysokiej dostępności oraz nie przedstawia zalecanej topologii wdrożenia dla dużych firm i środowisk produkcyjnych.  Jeśli Active Directory jest częścią środowiska połączonego z Internetem, zapoznaj się z tematem " [Zabezpieczanie uprzywilejowanych](/security/compass/overview) wskazówek dotyczących dostępu", aby uzyskać więcej informacji na temat lokalizacji do uruchomienia.

## <a name="installing-from-software-packages"></a>Instalowanie z pakietów oprogramowania

Następujące oprogramowanie można pobrać z witryny TechNet Evaluation Center lub MSDN:

- Microsoft Identity Manager 2016
  - Portal i usługa: zawiera instalator usługi MIM i portalu MIM oraz scenariusza PAM
  - Dodatki i rozszerzenia: zawiera instalator poleceń cmdlet środowiska PowerShell obiektu żądającego

Następujące opcjonalne oprogramowanie można pobrać z witryny GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): zawiera przykładową aplikację sieci Web dla interfejsu API REST

## <a name="required-software"></a>Wymagane oprogramowanie

- Windows Server 2016
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 lub SQL Server 2014

## <a name="hardware-requirements"></a>Wymagania sprzętowe

W przypadku każdego składnika PAM należy zapoznać się z wymaganiami systemowymi produktów.

Dla kontrolera domeny CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) lub starszy

Dla stacji roboczej CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Dla kontrolera domeny PRIVDC:

- Windows Server 2016

Dla serwera PAMSRV:

- Windows Server 2016
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) lub [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
