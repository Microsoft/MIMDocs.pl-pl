---
title: "Wymagania sprzętowe i programowe | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 77e7174e94ea8032c4e57155db489f493ce18177


---

Nie ma żadnych dodatkowych wymagań sprzętowych poza wymaganiami podstawowych platform oprogramowania oraz wymaganiami dotyczącymi wystarczającej ilości pamięci, miejsca na dysku oraz łączności sieciowej. Ten artykuł zawiera minimalne wymagania dla wdrożenia podstawowego. Nie ma na celu prezentowania wydajności, skalowalności ani wysokiej dostępności oraz nie przedstawia zalecanej topologii wdrożenia dla dużych firm i środowisk produkcyjnych.

## Instalowanie z pakietów oprogramowania

Następujące oprogramowanie można pobrać z witryny TechNet Evaluation Center lub MSDN:  
- Microsoft Identity Manager 2016
  - Portal i usługa: zawiera instalator usługi MIM i portalu MIM oraz scenariusza PAM
  - Dodatki i rozszerzenia: zawiera instalator poleceń cmdlet środowiska PowerShell obiektu żądającego

Następujące oprogramowanie można pobrać z witryny GitHub:  
- PAMSamplePortal: zawiera przykładową aplikację sieci Web dla interfejsu API REST

## Wymagane oprogramowanie

- Windows Server 2012 R2  
- Windows 8.1 Enterprise lub Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 lub SQL Server 2014  

## Oprogramowanie próbne

Jeśli nie masz licencji dla systemu Windows, programu SQL Server lub systemu Windows Server, możesz pobrać wersje próbne.

### TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft Download Center (Centrum pobierania Microsoft)

- [Serwer SQL](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 i jego wymagania wstępne](https://www.microsoft.com/download/details.aspx?id=42039)

## Wymagania sprzętowe

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



<!--HONumber=Jun16_HO3-->


