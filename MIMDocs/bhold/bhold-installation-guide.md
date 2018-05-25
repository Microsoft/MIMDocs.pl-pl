---
title: Instalacja dodatku SP1 BHOLD | Dokumentacja firmy Microsoft
description: BHOLD instalacji z dodatkiem SP1
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: e0514530c9bceef18cc8eea7ec8b7060110811c2
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2018
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Przewodnik instalacji dodatku SP1 dla (6.0) firmy Microsoft BHOLD

Dodatku Service Pack 1 (SP1) dla systemu Microsoft® BHOLD Suite to zbiór aplikacji, gdy jest używany z dodatku SP1 dla programu Microsoft Identity Manager 2016 (MIM), dodaje zarządzania roli, analizy i zaświadczania do programu MIM. Pakiet BHOLD Microsoft SP1 składa się z następujących modułów:

- Podstawowe BHOLD
- Łącznik zarządzania dostępu
- BHOLD FIM/MIM integracji
- Generator modeli BHOLD
- Analizy BHOLD
- Raportowanie BHOLD
- Poświadczenie BHOLD


>[!NOTE]
**Dotyczy**: dodatek SP1 dla programu Microsoft Identity Manager 2016

## <a name="what-this-document-covers"></a>W tym dokumencie opisano

Tym dokumencie opisano sposób planowania wdrożenia BHOLD spełnić potrzeby biznesowe i instalowania poszczególnych modułów BHOLD. Dla każdego modułu, istotne, infrastruktury, wymagania sprzętowe i programowe, preinstalacji konfiguracji sieci wymaganych informacji podczas instalacji, a o krokach, są szczegółowe.

## <a name="pre-requisite-knowledge"></a>Wstępnych wiedzy

Tym dokumencie przyjęto założenie, że masz podstawową wiedzę na temat sposobu instalowania oprogramowania na komputerach serwera. Przyjęto założenie, że masz podstawową wiedzę na temat oprogramowania bazy danych usług domenowych Active Directory®, Microsoft Identity Manager z dodatkiem SP1 (FIM) i Microsoft SQL Server 2008. Opis sposobu instalowania i konfigurowania technologie zależne, takie jak usługi AD DS i FIM jest poza zakresem niniejszej dokumentacji. Uzyskać informacje na temat funkcji, wykonujących modułów BHOLD firmy Microsoft, zobacz [przewodnik koncepcje pakietu Microsoft BHOLD](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Odbiorcy

Ten dokument jest przeznaczony dla projektanci systemów IT, architektów systemów, decydentów technologii, konsultantów, projektanci systemów infrastruktury i personelu IT, którzy planują wdrożenie pakietu BHOLD firmy Microsoft.

## <a name="bhold-infrastructure-considerations"></a>Zagadnienia dotyczące infrastruktury BHOLD

W większości przypadków BHOLD i FIM są używane w środowisku z dużą infrastrukturą. Można dostosować do potrzeb biznesowych konkretnego architektury BHOLD i FIM. Poniższe sekcje zawierają kilka możliwych rozwiązań architektury. W tym omówieniu nie jest jest kompleksowa lista wszystkich możliwości, ale sugeruje metody BHOLD można wdrożyć w sieci.
 
W tej sekcji omówiono następujące tematy:

- Architektura jednego serwera
- Architektura podwójnego serwera
- Architektura dwuwarstwowa
- Zalecenia dotyczące programu SQL Server

### <a name="single-server-architecture"></a>Architektura jednego serwera

Do wdrożenia w małych firmach lub do celów programistycznych można zainstalować BHOLD i FIM na tym samym serwerze co program SQL Server i usług AD DS, jak pokazano na poniższej ilustracji.
 
![Architektura pojedynczego serwera](media/bhold-installation-guide/single.png)

Gdy pakiet BHOLD z dodatkiem SP1 i portalu programu FIM są zainstalowane jednocześnie na jednym serwerze, należy utworzyć inny host aliasy (rekordy CNAME i A) w systemie DNS BHOLD i FIM. Dzięki temu głównych nazw oddzielne usługi (SPN), który ma zostać utworzony dla usług BHOLD i programu FIM. Aby uzyskać więcej informacji, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Aby uzyskać instrukcje dotyczące instalowania programu FIM w konfiguracji pojedynczego serwera, zobacz [typowe konfiguracje dla pobierania przewodniki dotyczące](https://technet.microsoft.com/library/ff575965.aspx) w Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Architektura podwójnego serwera

Instalowanie i BHOLD Core FIM na osobnych serwerach zapewnia większą wydajność i elastyczność średniej wielkości organizacji, które nie wymagają bardziej złożone wdrożenia, takim jak udostępniany przez wielowarstwowa architektury. Na poniższej ilustracji przedstawiono BHOLD i FIM zainstalowanego na serwerach; serwera FIM działa także program SQL Server do udostępniania usługi baza danych BHOLD i FIM. Usługa synchronizacji programu FIM uruchomionych na serwerze programu FIM synchronizuje zmiany między bazami danych FIM i BHOLD. Należy pamiętać, że jeśli samoobsługi użytkownika końcowego jest wymagana, moduł BHOLD FIM integracji musi zainstalowany na tym samym serwerze co usługi FIM i portalu programu FIM. Moduł BHOLD FIM integracji wymaga usługi FIM i moduł BHOLD FIM integracji są zainstalowane na tym samym serwerze.

![Architektura dwóch serwerów](media/bhold-installation-guide/dual.png)

>[!IMPORTANT]
Funkcja raportowania modułu BHOLD FIM integracji wymaga bazy danych BHOLD i FIM do zainstalowania na tym samym wystąpieniu programu SQL Server i BHOLD konto usługi musi mieć prawa dostępu do bazy danych usługi FIM.

### <a name="two-tier-architecture"></a>Architektura dwuwarstwowa

W większości środowisk, zwłaszcza w przypadku, gdy wydajności odgrywa ważną rolę, na różnych serwerach (architektura dwuwarstwowa) należy uruchomić z dodatkiem SP1 pakietu BHOLD FIM i programu SQL Server. Architektura dwuwarstwowa pamięć i zasoby Procesora są przeznaczone wyłącznie dla każdej warstwy. Na poniższej ilustracji przedstawiono możliwy sposób konfigurowania architektura dwuwarstwowa. Usługa synchronizacji programu FIM uruchomionych na serwerze programu FIM synchronizuje zmiany między bazami danych FIM i BHOLD. Należy pamiętać, że jeśli samoobsługi użytkownika końcowego jest wymagana, moduł BHOLD FIM integracji musi zainstalowany na tym samym serwerze co usługi FIM i portalu.

![Architektura dwuwarstwowa](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Zalecenia dotyczące programu SQL Server

Jeśli wdrażasz BHOLD dużej organizacji, zdecydowanie zaleca się należy wykonać te wskazówki dotyczące konfigurowania bazy danych programu Microsoft SQL Server:

- Wdrażanie programu SQL Server na serwerze, niezależnie od wszelkich usługi FIM lub BHOLD.
- Określ plik dziennika z pliku danych na poziomie dysku fizycznego.
- Jeśli używasz RAID, aby zapewnić nadmiarowość magazynu, użyj poziomu RAID 10 (1 + 0). Nie należy używać poziom RAID 5.
- Pamiętaj skonfigurować poprawne ustawienia, w przypadku używania ponad 2 GB pamięci fizycznej dla serwera z uruchomionym programem SQL Server.
- Dla uzyskania optymalnej wydajności BHOLD, należy użyć programu Microsoft SQL Server 2008 R2 lub nowszej.

Aby uzyskać więcej informacji na temat najlepszych rozwiązań programu SQL Server, zobacz [magazynu pierwszych 10 najlepszych rozwiązań](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) w Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Aktualizacja listy zaufanych certyfikatów

Systemu Windows można skonfigurować do sprawdzania poprawności łańcuchów certyfikatów przed rozpoczęciem usługi. W takich systemach nie można uruchomić usługi, jeśli kod wykonywalny usługi został podpisany za pomocą certyfikatu, który nie jest na liście zaufanych certyfikatów (TCL) na serwerze. Oprogramowanie Microsoft pakietu BHOLD z dodatkiem SP1 jest kod jest podpisywany przy użyciu łańcucha certyfikatów, która pochodzi z certyfikatem Microsoft główny certyfikat urzędu 2010 do podpisywania kodu.
Można skonfigurować systemu Windows można pobrać certyfikatów głównych firmy Microsoft za pośrednictwem połączenia internetowego. Na komputerze bez połączenia jednak system Windows Server zawiera tylko te certyfikaty, które były obecne w programie głównych w czasie przed wydania systemu Windows. W wersjach systemu Windows Server przed systemu Windows Server 2010 te certyfikaty nie będzie zawierał certyfikatu głównego, wymagane do weryfikacji łańcucha certyfikatu podpisywania kodu pakietu BHOLD z dodatkiem SP1. Jeśli zamierzasz zainstalować jednego lub większej liczby modułów SP1 pakietu BHOLD firmy Microsoft w systemie, który może nie mieć aktualne TCL musi pobrać i zainstalować pakiet aktualizacji katalogu głównego lub przy użyciu zasad grupy do zainstalowania pakietu aktualizacji katalogu głównego, przed zainstalowaniem SP1 pakietu BHOLD Moduł. Aby uzyskać więcej informacji, zobacz [Windows root członkowie programu certyfikatów](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Instalowanie BHOLD dodatku SP1 dla systemu Windows Server 2012/2016 wymaganych kroku 

![Usługi IIS Zainstaluj BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Jeśli użytkownik zainstaluje BHOLD dodatku SP1 dla systemu Windows Server 2012 lub 2016, BHOLD stron sieci web nie będą dostępne dopiero po zmodyfikowaniu plik applicationHost.config znajdujący się w ```C:\Windows\System32\inetsrv\config```. W ```<globalModules>``` Dodaj ```preCondition="bitness64``` do wpisu, który rozpoczyna się ```<add name="SPNativeRequestModule"``` tak, aby o następującej treści:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Po zakończeniu edycji i zapisać plik, uruchom polecenie iisreset, aby zresetować serwer usług IIS.


## <a name="upgrading-bhold-suite"></a>Uaktualnianie pakietu BHOLD

Nie można uaktualnić istniejącą instalację pakietu BHOLD. Zamiast tego należy odinstalować istniejącą instalację pakietu BHOLD, aby można było zaktualizować modułów BHOLD. Jeśli masz istniejący model roli BHOLD można uaktualnić baza danych BHOLD i użyć go po zainstalowaniu zaktualizowanej moduł BHOLD Core. Aby uzyskać więcej informacji, zobacz [zastępowanie pakietu BHOLD z dodatkiem SP1 pakietu BHOLD](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Następne kroki

- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
