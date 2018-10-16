---
title: Instalacja dodatku SP1 dla pakietu BHOLD | Dokumentacja firmy Microsoft
description: Dokumentacja instalacji dodatku SP1 dla pakietu BHOLD
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: e73596ea1b07814a46d638ac705edf5fdada76a2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334350"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Podręcznik instalacji programu Microsoft BHOLD Suite z dodatkiem SP1 (6.0)

Dodatku Service Pack 1 (SP1) dla systemu Microsoft® pakietu BHOLD Suite jest zbiorem aplikacji, w przypadku użycia za pomocą programu Microsoft Identity Manager 2016 z dodatkiem SP1 (MIM), dodaje zarządzania roli, analizy i zaświadczania, do programu MIM. Dodatek SP1 dla pakietu BHOLD Microsoft składa się z następujących modułów:

- Podstawowe pakietu BHOLD
- Dostęp do łącznika zarządzania
- Integracja programu FIM/MIM BHOLD
- Generator modeli pakietu BHOLD
- Analizy BHOLD
- Raportowania pakietu BHOLD
- Zaświadczania pakietu BHOLD


> [!NOTE]
> **Dotyczy**: programu Microsoft Identity Manager 2016 z dodatkiem SP1

## <a name="what-this-document-covers"></a>W tym dokumencie opisano

W tym dokumencie opisano sposób planowania wdrożenia pakietu BHOLD do potrzeb Twojej firmy i zainstaluj moduł każdego pakietu BHOLD. Dla każdego modułu, odpowiednie, infrastruktury, wymagania sprzętowe i programowe, przed instalacją konfiguracją sieci informacje są wymagane podczas instalacji, a o krokach, są szczegółowo opisane.

## <a name="pre-requisite-knowledge"></a>Wstępnie wymagane wiedzy

W tym dokumencie przyjęto założenie, że masz podstawową wiedzę na temat sposobu instalowania oprogramowania na komputerach serwera. Przyjęto również założenie, że masz podstawową wiedzę na temat oprogramowania bazy danych w usługach domenowych Active Directory®, Microsoft Identity Manager z dodatkiem SP1 (FIM) i Microsoft SQL Server 2008. Opis sposobu instalowania i konfigurowania technologie zależne, takie jak usługi AD DS i FIM jest poza zakresem niniejszej dokumentacji. Aby uzyskać informacje na temat funkcji, które wykonują modułów BHOLD firmy Microsoft, zobacz [przewodnik pojęcia pakietu Microsoft BHOLD](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Odbiorcy

Ten dokument jest przeznaczony dla planistów IT, architektów systemów, technologia osoby podejmujące decyzje, konsultantów, oceniający infrastruktury i personel działu informatycznego, którzy planują wdrożenie pakietu BHOLD firmy Microsoft.

## <a name="bhold-infrastructure-considerations"></a>Zagadnienia związane z infrastrukturą pakietu BHOLD

W większości przypadków BHOLD i FIM są używane w środowisku z dużą infrastrukturą. Można dostosować do własnych potrzeb biznesowych konkretnego architektury BHOLD i FIM. Poniższe sekcje zawierają kilka możliwych rozwiązań architektonicznych. W tym omówieniu nie jest uzyskać pełną listę wszystkich możliwych opcji, ale podpowiada sposoby BHOLD można wdrożyć w sieci.
 
W tej sekcji omówiono następujące tematy:

- Architektura pojedynczego serwera
- Architektura podwójny serwer
- Architektura dwuwarstwowa
- Zalecenia dotyczące programu SQL Server

### <a name="single-server-architecture"></a>Architektura pojedynczego serwera

Do wdrożenia w małych organizacjach lub do celów programistycznych można zainstalować pakietu BHOLD i FIM na tym samym serwerze co program SQL Server i usług AD DS, jak pokazano na poniższej ilustracji.
 
![Architektura pojedynczego serwera](media/bhold-installation-guide/single.png)

Gdy pakietu BHOLD z dodatkiem SP1 i portalu programu FIM są zainstalowane jednocześnie na jednym serwerze, należy utworzyć aliasy innego hosta (rekordy CNAME i A) w systemie DNS dla pakietu BHOLD i programu FIM. Dzięki temu nazwy główne oddzielnej usługi (SPN), który ma zostać utworzony dla usług pakietu BHOLD i programu FIM. Aby uzyskać więcej informacji, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Aby uzyskać wskazówki na temat instalowania programu FIM w konfiguracji pojedynczego serwera, zobacz [Typowa konfiguracja przewodnikach wprowadzających](https://technet.microsoft.com/library/ff575965.aspx) w bibliotece Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Architektura podwójny serwer

Zainstalowanie pakietu BHOLD Core i usługi FIM na oddzielnych serwerach zapewnia lepszą wydajność i elastyczność dla średniej wielkości organizacji, które nie wymagają bardziej złożonym wdrożeniu, takim jak udostępniany przez architektury wielowarstwowej. Na poniższej ilustracji przedstawiono BHOLD i FIM instalowane na własnych serwerach; Serwer programu FIM również jest uruchomiony program SQL Server do świadczenia usług baza danych BHOLD i FIM. FIM Synchronization Service uruchomiona na serwerze programu FIM synchronizuje zmiany między bazami danych usługi FIM i BHOLD. Należy pamiętać, że jeśli wymagana jest samoobsługi użytkowników końcowych, moduł BHOLD FIM integracji musi zainstalowani na tym samym serwerze co usługi FIM Service i portalu programu FIM. Moduł integracji usługi FIM BHOLD wymaga usługi FIM Service i moduł BHOLD FIM integracji są zainstalowane na tym samym serwerze.

![Architektura podwójny serwer](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> Funkcji raportowania modułu BHOLD FIM integracji wymaga bazy danych BHOLD i FIM, należy zainstalować na tym samym wystąpieniu programu SQL Server i konta usługi BHOLD musi mieć prawa dostępu do bazy danych usługi FIM Service.

### <a name="two-tier-architecture"></a>Architektura dwuwarstwowa

W większości środowisk, zwłaszcza w przypadku, gdy wydajność jest ważne, należy uruchomić SP1 pakietu BHOLD FIM i programu SQL Server na oddzielnych serwerach (architektura dwuwarstwowa). Architektura dwuwarstwowa pamięci i zasobów procesora CPU są przeznaczone dla każdej warstwy. Poniższa ilustracja przedstawia możliwy sposób konfigurowania architektura dwuwarstwowa. FIM Synchronization Service uruchomiona na serwerze programu FIM synchronizuje zmiany między bazami danych usługi FIM i BHOLD. Należy pamiętać o tym, jeśli wymagana jest samoobsługi użytkowników końcowych, moduł BHOLD FIM integracji musi zainstalowania na tym samym serwerze, portalu i usługi programu FIM.

![Architektura dwuwarstwowa](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Zalecenia dotyczące programu SQL Server

Jeśli wdrażasz BHOLD w dużych organizacjach, zalecane jest zgodna z tymi wytycznymi, do konfigurowania bazy danych programu Microsoft SQL Server:

- Wdrażanie programu SQL Server na serwerze oddzielnie od usług FIM lub BHOLD.
- Izolowanie dziennika od plików danych na poziomie dysku fizycznego.
- Jeśli używasz RAID w celu zapewnienia nadmiarowości magazynu, należy użyć poziom macierzy RAID 10 (1 + 0). Nie należy używać poziom macierzy RAID 5.
- Pamiętaj skonfigurować prawidłowe ustawienia, w przypadku używania ponad 2 GB pamięci fizycznej dla serwera z uruchomionym programem SQL Server.
- Aby uzyskać optymalną wydajność BHOLD, należy użyć programu Microsoft SQL Server 2008 R2 lub nowszej.

Aby uzyskać więcej informacji na temat najlepszych rozwiązań programu SQL Server, zobacz [Storage Top 10 najlepszych rozwiązań](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) w bibliotece Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Aktualizacja listy zaufanych certyfikatów

Windows można skonfigurować do sprawdzania poprawności łańcucha certyfikatów przed rozpoczęciem usługi. Takie systemy nie można uruchomić usługi, jeśli kod wykonywalny usługi została podpisana przy użyciu certyfikatu, który nie ma na liście zaufanych certyfikatów (TCL) serwera. Oprogramowanie Microsoft BHOLD Suite z dodatkiem SP1 jest kod jest podpisywany przy użyciu łańcucha certyfikatów, który pochodzi z certyfikatem Microsoft główny certyfikat urzędu 2010 do podpisywania kodu.
Windows można skonfigurować do pobrania certyfikatów głównych firmy Microsoft za pośrednictwem połączenia internetowego. O odłączony system Windows Server zawiera jednak tylko te certyfikaty, które znajdowały się w programie głównym w czasie przed Windows został wydany. W wersjach systemu Windows Server przed systemu Windows Server 2010 te certyfikaty nie zostaną uwzględnione służące do sprawdzania poprawności łańcucha certyfikatu podpisywania kodu SP1 pakietu BHOLD certyfikatu głównego. Jeśli zamierzasz zainstalować przynajmniej jeden moduł Microsoft BHOLD Suite z dodatkiem SP1 na komputerze, który może nie mieć aktualne TCL należy pobrać i zainstalować pakiet aktualizacji katalogu głównego lub użyj zasad grupy, aby zainstalować pakiet aktualizacji katalogu głównego, przed zainstalowaniem pakietu BHOLD z dodatkiem SP1 Moduł. Aby uzyskać więcej informacji, zobacz [Windows członkowie programu głównych certyfikatów](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Instalowanie pakietu BHOLD Suite z dodatkiem SP1 w systemie Windows Server 2012/2016 wymaganych kroku 

![Usługi IIS instalacji pakietu BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Po zainstalowaniu pakietu BHOLD Suite z dodatkiem SP1 w systemie Windows Server 2012 lub 2016 na stronach sieci web pakietu BHOLD nie będą dostępne dopiero po zmodyfikowaniu pliku applicationHost.config znajdujący się w ```C:\Windows\System32\inetsrv\config```. W ```<globalModules>``` Dodaj ```preCondition="bitness64``` do wpisu, który rozpoczyna się ```<add name="SPNativeRequestModule"``` tak, aby wyglądało następująco:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Po zakończeniu edycji i zapisać go, uruchom polecenie iisreset, aby zresetować na serwerze usług IIS.


## <a name="upgrading-bhold-suite"></a>Uaktualnianie pakietu BHOLD

Nie można uaktualnić istniejącą instalację pakietu BHOLD. Zamiast tego należy odinstalować istniejącą instalację pakietu BHOLD, aby można było zaktualizować moduły BHOLD. W przypadku istniejącego pakietu BHOLD modelu roli można uaktualnić bazy danych pakietu BHOLD i użyć go podczas instalowania zaktualizowany moduł Core pakietu BHOLD. Aby uzyskać więcej informacji, zobacz [zastąpienie pakietu BHOLD z dodatkiem SP1 pakietu BHOLD](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Kolejne kroki

- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
