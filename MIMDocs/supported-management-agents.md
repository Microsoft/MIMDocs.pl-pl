---
title: Obsługiwane łączniki | Dokumentacja firmy Microsoft
description: Łączniki umożliwiają zarządzanie przesyłaniem danych między usługą MIM i z połączonych źródeł danych.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 1/24/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 1f7d4150ce7012cd4726126ba50b1ab0f94f474c
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690683"
---
# <a name="connect-to-your-directories"></a>Łączenie z katalogami

Łączniki łączą określone połączone źródła danych do programu Microsoft Identity Manager z dodatkiem SP1 (MIM). Łącznik przenosi dane z połączonego źródła danych do usługi MIM. Jeśli dane w usłudze MIM zostaną zmodyfikowane, łącznik może także wyeksportować dane do połączonego źródła danych, aby zapewnić jego synchronizację z usługą MIM. Ogólnie dla każdego połączonego katalogu istnieje co najmniej jeden łącznik.

W programie Forefront Identity Manager łączniki były znane jako agenci zarządzania. Ta nazwa jest nadal używana w niektórych artykułach i częściach produktu, lecz obie nazwy odnoszą się do tego samego pojęcia.

W tym artykule omówiono łączniki, które są dołączone i obsługiwane w programie MIM, lecz łącznik Extensible Connectivity 2.0 umożliwia łączenie z jeszcze większą liczbą źródeł danych. Niektórzy partnerzy utworzyli własne łączniki w ten sposób i Pełna lista jest dostępna w witrynie Wiki dotyczącej [programu FIM 2010: Agenci zarządzania oferowanych przez partnerów](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Łączniki obsługiwane przez program MIM 2016 z dodatkiem SP1

| Name (Nazwa) | Obsługiwane wersje połączonego źródła danych & linki Technical Preview |
| ---- | ----------------------------------------------- |
| Usługi domenowe Active Directory | Usługi Active Directory w systemie Windows Server 2012 i Windows Server 2016 |
| Usługi LDS Active Directory (ADLDS) | Usługi LDS Active Directory (ADLDS) |
| Globalna lista adresów usługi Active Directory | Usługi Active Directory globalnej liście adresowej (GAL) — programu Exchange 2013, Exchange 2016 |
| Extensible Connectivity 2.0 | Wszystkie źródła danych oparte na wywołaniu lub pliku |
| Usługa FIM Service | Usługa programu MIM. Należy pamiętać, że usługa synchronizacji programu MIM i usługi programu MIM musi być w tej samej wersji. |
| IBM DB2 Universal Database | Program IBM DB2 wersja 9.5 lub zbierając 9,7; IBM DB2 OLEDB 9.5 FP5 lub 9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Program Novell eDirectory wersje 8.7.3, 8.8.5 i 8.8.6 |
| Oracle Database | Program Oracle Database 10g lub 11g, klient 64-bitowy |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Serwery katalogowe firmy Oracle (wcześniej Sun i Netscape) | Programy Sun Directory Server 6.x, 7.x i Oracle 11 |
| [Łącznik programu Windows PowerShell](https://msdn.microsoft.com/library/dn640417.aspx) | Program Windows PowerShell 2.0 lub nowszy |
| [Microsoft Azure Active Directory Connector](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (niezalecane w przypadku nowych wdrożeń) |
| [Ogólny łącznik LDAP](https://msdn.microsoft.com/library/dn510997.aspx) | [Serwer v3 LDAP (maksymalnie ze specyfikacją RFC 4510)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Ogólny łącznik SQL](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Łącznik jest obsługiwany przy użyciu wszystkich 64-bitowe sterowniki ODBC](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Łącznik programu Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes 8.5.x wydania |
| [Łącznik usług SharePoint UPA](https://msdn.microsoft.com/library/dn511003.aspx) | Serwer programu SharePoint 2013 lub 2016 z aplikacją usługi profilu użytkownika |
| [Łącznik usług sieci Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 lub 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 i innych protokołu SOAP i interfejsy API REST](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Plik tekstowy z parami atrybut-wartość](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Pliki tekstowe z parami atrybut-wartość |
| [Rozdzielany plik tekstowy](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Rozdzielane pliki tekstowe |
| [Język narzut usług katalogowych (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Plik tekstowy stałej szerokości](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Pliki tekstowe stałej szerokości |
| [LDAP Data Interchange Format (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | Format wymiany danych LDAP (LDIF) |
| [Łącznik programu Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Tematy pokrewne

[Agenci zarządzania w programie FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
