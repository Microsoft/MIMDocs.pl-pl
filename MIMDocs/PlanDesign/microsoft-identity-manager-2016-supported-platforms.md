---
title: "Obsługiwane platformy oprogramowania | Microsoft Identity Manager"
description: "Produkty i wersje zgodne z poszczególnymi składnikami programu MIM 2016"
keywords: 
author: kgremban
manager: femila
ms.date: 09/29/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 69e2c327cf897dea450c43618a9b4ce3ab555cc0
ms.openlocfilehash: 522e9321d7709a7967cfea3eb1cea809dfe8202e


---

# Platformy obsługiwane przez program MIM 2016

W tej tabeli podano obsługiwane platformy i wersje dla każdego składnika programu Microsoft Identity Manager 2016. Wersje oznaczone gwiazdką * są obsługiwane tylko w wersji programu MIM 2016 z dodatkiem Service Pack 1.


| **Składnik programu MIM** | **Platforma** | **Wersja** |
|-------------------|--------------|-------------|
| **Usługa synchronizacji programu MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
|| | Baza danych usługi synchronizacji programu MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | Active Directory na potrzeby aprowizacji użytkowników, PCNS i synchronizacja usługi GAL (opcjonalnie)|Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Exchange na potrzeby aprowizacji skrzynek pocztowych i synchronizacji usługi GAL (opcjonalnie)|Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| | Środowisko programistyczne (opcjonalnie) | Visual Studio 2012<br/>Visual Studio 2013 |
|| | Dodatkowy system połączony (opcjonalnie) | Usługi domenowe Active Directory<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 lub nowszy<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Inne produkty podmiotów trzecich |
| **Usługa programu MIM** (za wyjątkiem scenariusza PAM) | Windows Server | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Baza danych usługi programu MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | Exchange na potrzeby wiadomości e-mail zatwierdzania i zarządzania grupami usługi MIM (opcjonalnie) | Exchange Server 2007 SP3 (z zainstalowaną konsolą zarządzania usługi Exchange)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
| **Portal i usługa programu MIM** (tylko scenariusz PAM)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Active Directory dla lasu PAM środowiska bastionu | Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Active Directory dla istniejących lasów | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
|| | Baza danych usługi programu MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | Program SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
|| | Serwer poczty dla wiadomości e-mail zatwierdzania i zarządzania grupami usługi programu MIM (opcjonalnie) | Exchange Server 2007 SP3 (z zainstalowaną konsolą zarządzania usługi Exchange)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
|| | Przeglądarka | Wszystkie popularne przeglądarki |
| **Raportowanie usługi programu MIM** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
|| | Magazyn danych | System Center 2012 Service Manager SP1 |
|| | Baza danych magazynu danych | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 |
| **Portale resetowanie hasła i rejestracji w programie MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Przeglądarka sieci Web | Wszystkie popularne przeglądarki |
| **Dodatki i rozszerzenia programu MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
|| | Integracja z programem Outlook (opcjonalnie) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (w systemie Windows 10) * |
|| | Polecenia cmdlet obiektu żądającego programu PowerShell PAM (opcjonalnie) | Windows 8.1<br/>Windows 10 |
| **Zarządzanie certyfikatami programu MIM** (integracja serwera i urzędu certyfikacji) | Serwer systemu Windows | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012 R2 |
|| | Urząd certyfikacji | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
|| | Baza danych zarządzania certyfikatami programu MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
| **Zarządzanie certyfikatami programu MIM** (aplikacja) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Zarządzanie certyfikatami programu MIM** (klient i klient zbiorczy) | Windows | Windows 7 |
| **Pakiet BHOLD MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012 R2 |
|| | Baza danych BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * |
|| | Serwer poczty (opcjonalnie) | Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| | Przeglądarka sieci Web | Internet Explorer 7, 8, 9, 10 lub 11 z dodatkiem Silverlight |



<!--HONumber=Sep16_HO5-->


