---
title: "Konfigurowanie serwera zarządzania tożsamościami&#58; SQL Server 2014 | Microsoft Identity Manager"
description: "Zainstaluj program SQL Server 2014 w ramach przygotowań do instalacji programu MIM 2016."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c023d147d0fcc1525fefbe866c952e217f7bee6b
ms.openlocfilehash: 2c0ff0bdbba4bcf979def8d5c7aa381947d6cc87


---

# Konfigurowanie serwera zarządzania tożsamościami: SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **nazwa_serwera_mim**
> - Nazwa domeny — **contoso**
> - Hasło — **Has@lo1**

## Instalowanie programu **SQL Server 2014 Standard Edition**

1. Uruchom program **PowerShell** jako administrator domeny.

2. Przejdź do katalogu, w którym znajduje się plik instalacyjny programu SQL Server.

3. Wpisz następujące polecenia.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)



<!--HONumber=Jun16_HO4-->


