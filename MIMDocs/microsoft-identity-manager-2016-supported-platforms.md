---
title: Obsługiwane platformy oprogramowania | Dokumentacja firmy Microsoft
description: Produkty i wersje zgodne z poszczególnymi składnikami programu MIM 2016
keywords: ''
author: fimguy
ms.author: davidste
manager: davidste
ms.date: 04/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 1054d611ae0b230005a0f79be69f5c6c2bba7af2
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2018
---
# <a name="supported-platforms-for-mim-2016"></a>Platformy obsługiwane przez program MIM 2016

W tej tabeli podano obsługiwane platformy i wersje dla każdego składnika programu Microsoft Identity Manager 2016. Wersje oznaczonych * są obsługiwane tylko w MIM 2016 z dodatkiem Service pack 1 z najnowszych poprawek.


| **Składnik programu MIM** | **Platforma** | **Wersja** |
|-------------------|--------------|--------------|
| **Usługa synchronizacji programu MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Poziom funkcjonalności Active Directory dla aprowizacji użytkowników, PCNS i synchronizacja usługi GAL | Windows 2000 <br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Baza danych usługi synchronizacji programu MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Usługi Active Directory dla Inicjowanie obsługi użytkowników, PCNS i synchronizacja usługi GAL (opcjonalnie)|Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange na potrzeby aprowizacji skrzynek pocztowych i synchronizacji usługi GAL (opcjonalnie)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Środowisko programistyczne (opcjonalnie) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Dodatkowy system połączony (opcjonalnie) | Usługi domenowe Active Directory<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 lub nowsza wersja<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Produkty innych firm |
| **Usługa i portal MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Scenariusz PAM: Systemu Windows Server | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Scenariusz PAM: Active Directory dla lasu PAM środowiska bastionu | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Scenariusz PAM: Active Directory dla scenariusza PAM istniejących lasów (CORP) | Windows Server 2008 <br/> Windows Server 2008 R2* <br/> Windows Server 2012* <br/> Windows Server 2012 R2* <br/> Windows Server 2016 * |
| | Baza danych usługi programu MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | Program SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Serwer poczty dla wiadomości e-mail zatwierdzania i zarządzania grupami usługi programu MIM (opcjonalnie) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016* <br/> Exchange Online * (tylko powiadomienie przed kompilacji [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490) |
| | Przeglądarka | Wszystkie główne obsługiwane przeglądarki * (tylko urządzeń przenośnych)|
| **Raportowanie usługi programu MIM** | Windows Server |  Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012 <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Magazyn danych | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager* (z wersją 4.4.1459)<br/> [Zgodność wersji programu SQL Server z programem System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016)
 |
| **Portale resetowania hasła i rejestracji w programie MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Przeglądarka sieci Web | Wszystkie główne obsługiwanych przeglądarek |
| **Dodatki i rozszerzenia programu MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integracja z programem Outlook (opcjonalnie) | Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (w systemie Windows 10) * |
| | Polecenia cmdlet obiektu żądającego programu PowerShell PAM (opcjonalnie) | Windows 8.1<br/>Windows 10 |
| **Zarządzanie certyfikatami programu MIM** (integracja serwera i urzędu certyfikacji) | Serwer systemu Windows | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Urząd certyfikacji | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Baza danych zarządzania certyfikatami programu MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **Zarządzanie certyfikatami programu MIM** (aplikacja) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Zarządzanie certyfikatami programu MIM** (zbiorcze klienta) | Windows | Windows 7 |
| **Zarządzanie certyfikatami programu MIM** (Kontrolka ActiveX klienta na podstawie karty inteligentnej) | Windows | Windows 7 </br> Windows 8 </br> Windows 8.1 </br> Windows 10 |
| **Pakiet BHOLD MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Baza danych BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | Serwer poczty (opcjonalnie) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016* |
| | Przeglądarka sieci Web | Program Internet Explorer obsługiwane przeglądarki z dodatkiem Silverlight |
