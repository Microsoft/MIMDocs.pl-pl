---
title: Obsługiwane łączniki | Dokumentacja firmy Microsoft
description: Łączniki umożliwiają zarządzanie przesyłaniem danych między usługą MIM i sieci połączonych źródeł danych.
keywords: ''
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 1/24/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 7b685ffb6f2a52bd2782395e4c1f26501ffe3101
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479352"
---
# <a name="connect-to-your-directories"></a>Łączenie z katalogami

Łączniki łączą określone połączonych źródeł danych do programu Microsoft Identity Manager z dodatkiem SP1 (MIM). Łącznik przenosi dane z połączonego źródła danych do usługi MIM. Jeśli dane w usłudze MIM zostaną zmodyfikowane, łącznik może także wyeksportować dane do połączonego źródła danych, aby zapewnić jego synchronizację z usługą MIM. Ogólnie dla każdego połączonego katalogu istnieje co najmniej jeden łącznik.

W programie Forefront Identity Manager łączniki były znane jako agenci zarządzania. Ta nazwa jest nadal używana w niektórych artykułach i częściach produktu, lecz obie nazwy odnoszą się do tego samego pojęcia.

W tym artykule omówiono łączniki, które są włączone i obsługiwane w programie MIM, lecz łącznik Extensible Connectivity 2.0 umożliwia łączenie z jeszcze większą liczbą źródeł danych. Niektórzy partnerzy utworzyli własne łączniki w ten sposób. Pełna lista jest dostępna w wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (Program FIM 2010: agenci zarządzania parterów).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Obsługiwane łączniki w programie MIM 2016 SP1

| Nazwa | Obsługiwane wersje połączonego źródła danych & łączy techniczne |
| ---- | ----------------------------------------------- |
| Usługi domenowe Active Directory | Active Directory, 2012 2016 |
| Usługi LDS Active Directory (ADLDS) | Usługi LDS Active Directory (ADLDS) |
| Globalna lista adresów usługi Active Directory | Usługi Active Directory globalnej liście adresowej (GAL) — programu Exchange 2013 2016 |
| Extensible Connectivity 2.0 | Wszystkie źródła danych oparte na wywołaniu lub pliku |
| Usługa FIM Service | Agent zarządzania usługi FIM (usługa synchronizacji) musi znajdować się w tej samej wersji "Usługa programu Forefront Identity Manager" zainstalowany |
| IBM DB2 Universal Database | Program IBM DB2 wersja 9.5 lub 9.7; IBM DB2 OLEDB 9.5 FP5 lub 9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Program Novell eDirectory wersje 8.7.3, 8.8.5 i 8.8.6 |
| Oracle Database | Program Oracle Database 10g lub 11g, klient 64-bitowy |
| Microsoft SQL Server | SQL Server 2012, 2014 r. 2016 |
| Serwery katalogowe firmy Oracle (wcześniej Sun i Netscape) | Programy Sun Directory Server 6.x, 7.x i Oracle 11 |
| [Windows PowerShell Connector for FIM 2010 R2](https://msdn.microsoft.com/library/dn640417.aspx) | Program Windows PowerShell 2.0 lub nowszy |
| [Microsoft Azure Active Directory Connector for FIM 2010 R2](https://msdn.microsoft.com/library/dn511001.aspx) | Usługi Active Directory systemu Microsoft Azure |
| [Ogólny łącznik LDAP dla programu FIM 2010 R2](https://msdn.microsoft.com/library/dn510997.aspx) | [Serwer LDAP w wersji 3 (maksymalnie ze specyfikacją RFC 4510)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Łącznik usług SQL ogólnego dla programu FIM 2010 R2 / MIM](https://msdn.microsoft.com/library/dn510997.aspx) | [Łącznik jest obsługiwana przez wszystkie 64-bitowe sterowniki ODBC](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql) |
| [Łącznik programu Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Program Lotus Notes 8.5.x zlecenia |
| [Łącznik usług SharePoint UPA](https://msdn.microsoft.com/library/dn511003.aspx) | Serwer programu SharePoint 2013 lub 2016 z aplikacją usługi profilu użytkownika |
| [Łącznik usług sieci Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 lub 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Plik tekstowy z parami atrybut-wartość](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Pliki tekstowe z parami atrybut-wartość |
| [Rozdzielany plik tekstowy](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Rozdzielane pliki tekstowe |
| [Język narzut usług katalogowych (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Plik tekstowy stałej szerokości](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Pliki tekstowe stałej szerokości |
| [Format wymiany danych LDAP (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | Format wymiany danych LDAP (LDIF) |

## <a name="related-topics"></a>Tematy pokrewne

[Agenci zarządzania w programie FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
