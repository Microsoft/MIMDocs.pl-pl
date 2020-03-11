---
title: Wymagania programowe usługi PAM | Dokumentacja firmy Microsoft
description: Określ wymagania sprzętowe i programowe, aby pomyślnie wdrożyć usługę Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 79f9eeabdd9ac9206c4232217c7cbcdf870c3a3a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043991"
---
# <a name="hardware-and-software-requirements"></a>Wymagania dotyczące sprzętu i oprogramowania

Usługa Privileged Access Management nie ma wymagań sprzętowych poza wymaganiami jej bazowych platform oprogramowania. Po prostu upewnij się, że masz wystarczającą ilość pamięci lub miejsca na dysku i łączność sieciową.

> [!IMPORTANT]
> Ten artykuł zawiera minimalne wymagania dla wdrożenia podstawowego. Nie jest przeznaczona do zademonstrowania wydajności, skalowalności ani wysokiej dostępności. Nie reprezentuje ona zalecanej topologii wdrażania dla dużych przedsiębiorstw lub środowisk produkcyjnych.

## <a name="installing-from-software-packages"></a>Instalowanie z pakietów oprogramowania

Następujące oprogramowanie można pobrać z witryny TechNet Evaluation Center lub MSDN:

- Microsoft Identity Manager 2016
  - Portal i usługa: zawiera instalator usługi MIM i portalu MIM oraz scenariusza PAM
  - Dodatki i rozszerzenia: zawiera instalator poleceń cmdlet środowiska PowerShell obiektu żądającego

Następujące oprogramowanie można pobrać z witryny GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): zawiera przykładową aplikację sieci Web dla interfejsu API REST

## <a name="required-software"></a>Wymagane oprogramowanie

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 lub SQL Server 2014

## <a name="evaluation-software"></a>Oprogramowanie próbne

Jeśli nie masz licencji dla systemu Windows, programu SQL Server lub systemu Windows Server, możesz pobrać wersje próbne.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Microsoft Download Center (Centrum pobierania Microsoft)

- [Program SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 i jego wymagania wstępne](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Wymagania sprzętowe

W przypadku każdego składnika PAM należy zapoznać się z wymaganiami systemowymi produktów.

Dla kontrolera domeny CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) lub starszy

Dla stacji roboczej CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Dla kontrolera domeny PRIVDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Dla serwera PAMSRV:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) lub [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
