---
title: Konfigurowanie SQL Server dla Microsoft Identity Manager 2016 SP2 | Microsoft Docs
description: Zainstaluj SQL Server 2016 lub 2017 w przygotowaniu do instalacji programu MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2ca9131a6c4f38ed559618d662848b74e1bffe66
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043498"
---
# <a name="set-up-an-identity-management-server-sql-server-2016-or-2017"></a>Konfigurowanie serwera zarządzania tożsamościami: SQL Server 2016 lub 2017

> [!div class="step-by-step"]
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
 
> [!NOTE] 
> Procedura instalacji programu SQL Server 2017 nie różni się od procedury instalacji programu SQL Server 2016.

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **corpdc**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi programu MIM — **corpservice**
> - Nazwa serwera synchronizacji programu MIM — **corpsync**
> - Nazwa SQL Server — **corpsql**
> - Hasło<strong>Pass@word1</strong>

> [!IMPORTANT]
> Program MIM 2016 SP2 obsługuje odbiorniki grupy dostępności funkcji SQL AlwaysOn (AoAG) z opcją *RegisterAllProvidersIP* ustawioną na 0, co oznacza, że SQL Server tryb failover między podsieciami nie jest obecnie obsługiwany.

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Zainstaluj **SQL Server 2016 Standard/Enterprise Edition**

1. Uruchom program **PowerShell** jako administrator domeny.

2. Przejdź do katalogu, w którym znajduje się plik instalacyjny programu SQL Server.

3. Wpisz następujące polecenia.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
Więcej informacji na temat kont i usług wdrożenia SQL można znaleźć [tutaj](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)

> [!NOTE]
> Program SSMS nie jest już uwzględniony w programie SQL 2016. Szczegóły pobierania można znaleźć [tutaj](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)

> [!div class="step-by-step"]  
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
