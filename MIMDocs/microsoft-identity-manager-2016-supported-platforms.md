---
title: Obsługiwane platformy oprogramowania | Dokumentacja firmy Microsoft
description: Produkty i wersje zgodne z poszczególnymi składnikami programu MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/19/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 40a79d234e702ca8f98349b13687a7262ffc4a27
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044076"
---
# <a name="supported-platforms-for-mim-2016"></a>Platformy obsługiwane przez program MIM 2016

W tej tabeli podano obsługiwane platformy i wersje dla każdego składnika programu Microsoft Identity Manager 2016. Wersje oznaczone gwiazdką * są obsługiwane tylko w wersji programu MIM 2016 z dodatkiem Service Pack 1. Wersje oznaczone gwiazdką * * są obsługiwane tylko w programie MIM 2016 z dodatkiem Service Pack 2 lub nowszym. Wersje oznaczone jako "NR" niezalecane są obsługiwane, ale nie są zalecane w przypadku uruchamiania nowego wdrożenia tej platformy dla programu MIM.


| **Składnik programu MIM** | **Platforma** | **Wersja** |
|-------------------|--------------|--------------|
| **Usługa synchronizacji programu MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 *<br/>Windows Server 2019 * * |
| | Active Directory poziom funkcjonalności dla aprowizacji użytkowników, PCNS i synchronizacji | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Baza danych usługi synchronizacji programu MIM | SQL Server 2008 R2 z dodatkiem SP3 (NR)<br/>SQL Server 2012 z dodatkiem SP4 (NR)<br/>SQL Server 2014 z dodatkiem SP3 (NR) <br/>SQL Server 2016 z dodatkiem SP2 *<br/>SQL Server 2017 * * |
| | Active Directory do obsługi administracyjnej użytkowników, PCNS i synchronizacji z użytkownikami (opcjonalnie)|Windows Server 2008 R2 z dodatkiem SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Exchange na potrzeby aprowizacji skrzynek pocztowych i synchronizacji usługi GAL (opcjonalnie)|Exchange Server 2010 z dodatkiem SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016*<br/>Program Exchange Server 2019 * * |
| | Środowisko programistyczne (opcjonalnie) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Dodatkowy system połączony (opcjonalnie) | Usługi domenowe Active Directory<br/>Usługi domenowe<br/>Lightweight Directory Services<br/>SQL Server 2008 lub nowsza wersja<br/>SharePoint Server 2013<br/> SharePoint Server 2016 *<br/> SharePoint Server 2019 * * <br/> Inne produkty innych firm |
| **Usługa i portal MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| |Scenariusz PAM: system Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |Scenariusz usługi PAM: Active Directory lasu PAM środowiska bastionu | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |Scenariusz PAM: Active Directory dla scenariusza usługi PAM istniejące lasy (CORP) | Windows Server 2008 <br/> Windows Server 2008 R2* <br/> Windows Server 2012* <br/> Windows Server 2012 R2* <br/> Windows Server 2016 * |
| | Baza danych usługi programu MIM | SQL Server 2008 R2 z dodatkiem SP3 (NR)<br/>SQL Server 2012 z dodatkiem SP4 (NR)<br/>SQL Server 2014 z dodatkiem SP3 (NR) <br/> SQL Server 2016 z dodatkiem SP2 *<br/> SQL Server 2017 * * |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 z dodatkiem SP1 (NR) <br/> SharePoint 2016 *<br/> SharePoint 2019 * * |
| | Serwer poczty dla wiadomości e-mail zatwierdzania i zarządzania grupami usługi programu MIM (opcjonalnie) | Exchange Server 2010 z dodatkiem SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016*<br/> Program Exchange Server 2019 * * <br/> Exchange Online * (powiadomienie przed kompilacją [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Przeglądarka | Wszystkie główne obsługiwane przeglądarki * (ograniczone urządzenia przenośne)|
| **Raportowanie usługi programu MIM** | Windows Server |  Windows Server 2008 R2 z dodatkiem SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Magazyn danych | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager* (z wersją 4.4.1459)<br/> System Center 2019 Service Manager * * |
| **Portale resetowania hasła i rejestracji w programie MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Przeglądarka sieci Web | Wszystkie główne obsługiwane przeglądarki |
| **Dodatki i rozszerzenia programu MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integracja z programem Outlook (opcjonalnie) | Outlook 2010 (w systemie Windows, z wyjątkiem kliknięcia do uruchomienia)<br/>Outlook 2013 (w systemie Windows, z wyjątkiem kliknięcia do uruchomienia) <br/> Outlook 2016 (w systemie Windows 10, z wyjątkiem kliknięcia do uruchomienia) *<br/>Office 365 Outlook (w systemie Windows 10, w tym kliknij przycisk-Uruchom) * * |
| | Polecenia cmdlet obiektu żądającego programu PowerShell PAM (opcjonalnie) | Windows 8.1<br/>Windows 10 |
| **Zarządzanie certyfikatami programu MIM** (integracja serwera i urzędu certyfikacji) | Serwer systemu Windows | Windows Server 2008 R2 z dodatkiem SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Urząd certyfikacji | Windows Server 2008 R2 z dodatkiem SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Baza danych zarządzania certyfikatami programu MIM | SQL Server 2008 R2 z dodatkiem SP3 (NR)<br/>SQL Server 2012 z dodatkiem SP4 (NR)<br/>SQL Server 2014 z dodatkiem SP3 (NR) <br/> SQL Server 2016 z dodatkiem SP2 *<br/> SQL Server 2017 * * |
| **Zarządzanie certyfikatami programu MIM** (aplikacja) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Zarządzanie certyfikatami programu MIM** (klient zbiorczy) | Windows | Windows 7 |
| **Zarządzanie certyfikatami programu MIM** (karta inteligentna oparta na kontrolce ActiveX klienta) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **Pakiet BHOLD MIM** | Windows Server | Windows Server 2008 R2 z dodatkiem SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Baza danych BHOLD | SQL Server 2008 R2 z dodatkiem SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 z dodatkiem SP2 * |
| | Serwer poczty (opcjonalnie) | Exchange Server 2010 z dodatkiem SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016* |
| | Przeglądarka sieci Web | Przeglądarki obsługiwane przez program Internet Explorer z technologią Silverlight |
