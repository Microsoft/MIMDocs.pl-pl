---
title: "Konfigurowanie programu SQL Server pod kątem instalacji programu Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft"
description: "Zainstaluj program SQL Server 2014 w ramach przygotowań do instalacji programu MIM 2016."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 105d2320ed5a0d610e8e6c5f459366680e3f8a77
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="set-up-an-identity-management-server-sql-server-2014"></a>Konfigurowanie serwera zarządzania tożsamościami: SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Na przykład:
> - Nazwa kontrolera domeny — **nazwa_serwera_mim**
> - Nazwa domeny — **contoso**
> - Hasło — **Pass@word1**

## <a name="install-sql-server-2014-standard-edition"></a>Instalowanie programu **SQL Server 2014 Standard Edition**

1. Uruchom program **PowerShell** jako administrator domeny.

2. Przejdź do katalogu, w którym znajduje się plik instalacyjny programu SQL Server.

3. Wpisz następujące polecenia.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)
