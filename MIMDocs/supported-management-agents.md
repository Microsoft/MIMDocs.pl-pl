---
title: "Obsługiwane łączniki | Dokumentacja firmy Microsoft"
description: "Łączniki umożliwiają zarządzanie przesyłaniem danych między usługą MIM i katalogami."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b26fe7bc56ab8229054afb1409c3652e81464a3d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# Łączenie z katalogami
<a id="connect-to-your-directories" class="xliff"></a>

Łączniki łączą określone połączone źródła danych z usługą Microsoft Identity Manager (MIM). Łącznik przenosi dane z połączonego źródła danych do usługi MIM. Jeśli dane w usłudze MIM zostaną zmodyfikowane, łącznik może także wyeksportować dane do połączonego źródła danych, aby zapewnić jego synchronizację z usługą MIM. Ogólnie dla każdego połączonego katalogu istnieje co najmniej jeden łącznik.

W programie Forefront Identity Manager łączniki były znane jako agenci zarządzania. Ta nazwa jest nadal używana w niektórych artykułach i częściach produktu, lecz obie nazwy odnoszą się do tego samego pojęcia.

W tym artykule omówiono łączniki dołączone do usługi MIM, lecz łącznik Extensible Connectivity 2.0 umożliwia łączenie z jeszcze większą liczbą źródeł danych. Niektórzy partnerzy utworzyli własne łączniki w ten sposób. Pełna lista jest dostępna w wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (Program FIM 2010: agenci zarządzania parterów).

## Łączniki obsługiwane przez usługę MIM 2016
<a id="supported-connectors-in-mim-2016" class="xliff"></a>

| Nazwa | Obsługiwane wersje połączonego źródła danych |
| ---- | ----------------------------------------------- |
| Usługi domenowe Active Directory | Usługi Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Usługi LDS Active Directory (ADLDS) | Usługi LDS Active Directory (ADLDS) |
| Globalna lista adresów usługi Active Directory | Globalna lista adresów usługi Active Directory — programy Exchange 2000, 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Wszystkie źródła danych oparte na wywołaniu lub pliku |
| Usługa MIM | Dokumentacja firmy Microsoft 2016 |
| IBM DB2 Universal Database | Program IBM DB2 wersja 9.1, 9.5 lub 9.7, program IBM DB2 OLEDB 9.5 FP5 lub 9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Program Novell eDirectory wersje 8.7.3, 8.8.5 i 8.8.6 |
| Oracle Database | Program Oracle Database 10g lub 11g, klient 64-bitowy |
| Microsoft SQL Server | Programy SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Serwery katalogowe firmy Oracle (wcześniej Sun i Netscape) | Programy Sun Directory Server 6.x, 7.x i Oracle 11 |
| [Windows PowerShell Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Program Windows PowerShell 2.0 lub nowszy |
| [Microsoft Azure Active Directory Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Usługi Active Directory systemu Microsoft Azure |
| [Ogólny łącznik LDAP dla programu FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | Serwer LDAP w wersji 3 (zgodny ze specyfikacją RFC 4510) |
| [Łącznik programu Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Program Lotus Notes 8.0.x lub 8.5.x |
| [SharePoint Services Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | Serwer programu SharePoint 2013 lub 2016 z aplikacją usługi profilu użytkownika |
| [Łącznik usług sieci Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | Programy SAP ECC 5.0 lub 6.0, Oracle PeopleSoft 9.1, Oracle eBusiness 12.1 |
| Plik tekstowy z parami atrybut-wartość | Pliki tekstowe z parami atrybut-wartość |
| Rozdzielany plik tekstowy | Rozdzielane pliki tekstowe |
| Directory Services Mark-up Language (DSML) | Directory Services Markup Language (DSML) 2.0 |
| Plik tekstowy stałej szerokości | Pliki tekstowe stałej szerokości |
| Format wymiany danych LDAP (LDIF) | Format wymiany danych LDAP (LDIF) |

## Tematy pokrewne
<a id="related-topics" class="xliff"></a>

[Agenci zarządzania w programie FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
