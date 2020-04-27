---
title: Obsługiwane łączniki | Dokumentacja firmy Microsoft
description: Używanie łączników do zarządzania przesyłaniem danych między programem MIM i połączonymi źródłami danych.
keywords: ''
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
author: EugeneSergeev
manager: daveba
editor: ''
reviewer: markwahl-msft
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/23/2019
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f6e43abea8b58ccff7fa376b266a91cb138f5aa9
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044393"
---
# <a name="connect-to-your-directories"></a>Łączenie z katalogami

Łączniki łączą określone połączone źródła danych z programem Microsoft Identity Manager SP1 (MIM). Łącznik przenosi dane z połączonego źródła danych do usługi MIM. Jeśli dane w usłudze MIM zostaną zmodyfikowane, łącznik może także wyeksportować dane do połączonego źródła danych, aby zapewnić jego synchronizację z usługą MIM. Ogólnie dla każdego połączonego katalogu istnieje co najmniej jeden łącznik.

W programie Forefront Identity Manager łączniki były znane jako agenci zarządzania. Ta nazwa jest nadal używana w niektórych artykułach i częściach produktu, lecz obie nazwy odnoszą się do tego samego pojęcia.

W tym artykule opisano łączniki, które są uwzględnione & obsługiwane w programie MIM, ale łącznik rozszerzalnej łączności 2,0 umożliwia łączenie się z jeszcze większą liczbą źródeł danych. Niektórzy partnerzy utworzyli własne łączniki w ten sposób. Pełna lista jest dostępna w wiki [FIM 2010: Management Agents from Partners](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (Program FIM 2010: agenci zarządzania parterów).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Obsługiwane łączniki w programie MIM 2016 z dodatkiem SP1

| Nazwa | Obsługiwane wersje połączonych źródeł danych & linki techniczne |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Active Directory w systemie Windows Server 2012 — 2019 |
| Usługi LDS Active Directory (ADLDS) | Usługi LDS Active Directory (ADLDS) |
| Globalna lista adresów usługi Active Directory | Active Directory listy adresów globalnych — Exchange 2013 — 2019 |
| Extensible Connectivity 2.0 | Wszystkie źródła danych oparte na wywołaniu lub pliku |
| Usługa FIM Service | Usługa MIM. Należy pamiętać, że usługa synchronizacji programu MIM i usługa MIM muszą być w tej samej wersji. |
| IBM DB2 Universal Database | IBM DB2 w wersji 9,5 lub 9,7; IBM DB2 OLEDB v 9,5 FP5 lub 9,7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Program Novell eDirectory wersje 8.7.3, 8.8.5 i 8.8.6 |
| Baza danych Oracle | Program Oracle Database 10g lub 11g, klient 64-bitowy |
| Microsoft SQL Server | SQL Server 2012 – 2017 |
| Serwery katalogowe firmy Oracle (wcześniej Sun i Netscape) | Programy Sun Directory Server 6.x, 7.x i Oracle 11 |
| [Łącznik programu Windows PowerShell](https://msdn.microsoft.com/library/dn640417.aspx) | Program Windows PowerShell 2.0 lub nowszy |
| [Łącznik Microsoft Azure Active Directory](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (niezalecane w przypadku nowych wdrożeń) |
| [Ogólny łącznik LDAP](https://msdn.microsoft.com/library/dn510997.aspx) | [Serwer LDAP w wersji 3 (zgodny ze specyfikacją RFC 4510)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Ogólny łącznik SQL](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Łącznik jest obsługiwany ze wszystkimi 64-bitowymi sterownikami ODBC](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Łącznik programu Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Program Lotus Notes w wersji 8,5. x, wersja 9.0. x |
| [Łącznik usług SharePoint UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013-2019 z aplikacją usługi profilu użytkownika (UPA) |
| [Łącznik usług sieci Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5,0 lub 6,0; Oracle PeopleSoft 9,1; Oracle eBusiness 12,1 i inne interfejsy API protokołu SOAP i REST](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Plik tekstowy z parami atrybut-wartość](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Pliki tekstowe z parami atrybut-wartość |
| [Rozdzielany plik tekstowy](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Rozdzielane pliki tekstowe |
| [Directory Services Mark-up Language (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Plik tekstowy stałej szerokości](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Pliki tekstowe stałej szerokości |
| [Format wymiany danych LDAP (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | Format wymiany danych LDAP (LDIF) |
| [Łącznik Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Powiązane tematy

[Agenci zarządzania w programie FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
