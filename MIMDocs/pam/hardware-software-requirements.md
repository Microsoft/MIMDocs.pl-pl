---
title: "Wymagania programowe usługi PAM | Dokumentacja firmy Microsoft"
description: "Określ wymagania sprzętowe i programowe, aby pomyślnie wdrożyć usługę Privileged Access Management"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 2985215821db843d2f90d8a34250a8ca6a84b592
ms.contentlocale: pl-pl
ms.lasthandoff: 07/10/2017


---

# Wymagania dotyczące sprzętu i oprogramowania
<a id="hardware-and-software-requirements" class="xliff"></a>

Usługa Privileged Access Management nie ma wymagań sprzętowych poza wymaganiami jej bazowych platform oprogramowania. Po prostu upewnij się, że masz wystarczającą ilość pamięci lub miejsca na dysku i łączność sieciową.

Ten artykuł zawiera minimalne wymagania dla wdrożenia podstawowego. Nie ma na celu prezentowania wydajności, skalowalności ani wysokiej dostępności oraz nie przedstawia zalecanej topologii wdrożenia dla dużych firm i środowisk produkcyjnych.

## Instalowanie z pakietów oprogramowania
<a id="installing-from-software-packages" class="xliff"></a>

Następujące oprogramowanie można pobrać z witryny TechNet Evaluation Center lub MSDN:  
- Microsoft Identity Manager 2016
  - Portal i usługa: zawiera instalator usługi MIM i portalu MIM oraz scenariusza PAM
  - Dodatki i rozszerzenia: zawiera instalator poleceń cmdlet środowiska PowerShell obiektu żądającego

Następujące oprogramowanie można pobrać z witryny GitHub:  
- PAMSamplePortal: zawiera przykładową aplikację sieci Web dla interfejsu API REST

## Wymagane oprogramowanie
<a id="required-software" class="xliff"></a>

- Windows Server 2012 R2  
- Windows 8.1 Enterprise lub Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 lub SQL Server 2014  

## Oprogramowanie próbne
<a id="evaluation-software" class="xliff"></a>

Jeśli nie masz licencji dla systemu Windows, programu SQL Server lub systemu Windows Server, możesz pobrać wersje próbne.

### TechNet Evaluation Center
<a id="technet-evaluation-center" class="xliff"></a>

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft Download Center (Centrum pobierania Microsoft)
<a id="microsoft-download-center" class="xliff"></a>

- [Program SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 i jego wymagania wstępne](https://www.microsoft.com/download/details.aspx?id=42039)

## Wymagania sprzętowe
<a id="hardware-requirements" class="xliff"></a>

W przypadku każdego składnika PAM należy zapoznać się z wymaganiami systemowymi produktów.

Dla kontrolera domeny CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) lub starszy

Dla stacji roboczej CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

Dla kontrolera domeny PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Dla serwera PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) lub [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)

