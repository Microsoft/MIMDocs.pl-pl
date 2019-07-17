---
title: Instalacja programu pakietu BHOLD SP1 | Microsoft Docs
description: Dokumentacja instalacji programu pakietu BHOLD SP1
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 05eb2afc0ddbf6104e27a5c24e121a55bd805292
ms.sourcegitcommit: 4c4bc7aa42cd5984c838abdd302490355ddcb4ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68238902"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Przewodnik instalacji programu Microsoft pakietu BHOLD Suite SP1 (6,0)

Microsoft® pakietu BHOLD Suite Service Pack 1 (SP1) to zbiór aplikacji, które są używane z programem Microsoft Identity Manager 2016 z dodatkiem SP1 (MIM), które umożliwiają zarządzanie rolami, analizowanie i zaświadczanie w programie MIM. Pakiet Microsoft pakietu BHOLD Suite SP1 składa się z następujących modułów:

- PAKIETU BHOLD rdzeń
- Łącznik zarządzania dostępem
- PAKIETU BHOLD z integracją FIM/MIM
- Generator modelu pakietu BHOLD
- Analiza pakietu BHOLD
- Raportowanie pakietu BHOLD
- Zaświadczanie pakietu BHOLD


> [!NOTE]
> **Dotyczy**: Microsoft Identity Manager 2016 z dodatkiem SP1

## <a name="what-this-document-covers"></a>Co obejmuje ten dokument

W tym dokumencie wyjaśniono, jak zaplanować wdrożenie pakietu BHOLD w celu spełnienia wymagań firmy i zainstalować każdy moduł pakietu BHOLD. Dla każdego modułu, odpowiedniego sprzętu, infrastruktury i wymagań dotyczących oprogramowania, wstępnej instalacji konfiguracji sieci, informacji wymaganych podczas instalacji oraz postinstallation kroki, jeśli istnieją.

## <a name="pre-requisite-knowledge"></a>Wiedza o wymaganiach wstępnych

W tym dokumencie przyjęto założenie, że masz podstawową wiedzę na temat sposobu instalowania oprogramowania na komputerach serwerów. Założono również, że masz podstawową wiedzę na temat Active Directory® usługi domenowe, Microsoft Identity Manager z dodatkiem SP1 (FIM) i Microsoft SQL Server 2012. Opis sposobu konfigurowania i konfigurowania technologii zależnych, takich jak AD DS i FIM, znajduje się poza zakresem tej dokumentacji. Informacje o funkcjach wykonywanych przez moduły Microsoft pakietu BHOLD można znaleźć [w podręczniku Microsoft pakietu BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Odbiorcy

Ten dokument jest przeznaczony dla planistów IT, architektów systemów, decyzji technologicznych, konsultantów, planisty infrastruktury i personelu IT, którzy planują wdrożenie pakietu Microsoft pakietu BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>Zagadnienia dotyczące infrastruktury pakietu BHOLD

Najczęściej pakietu BHOLD i FIM są używane w dużym środowisku infrastruktury. Możesz dostosować architekturę pakietu BHOLD i FIM, aby spełniała konkretne potrzeby biznesowe. W poniższych sekcjach przedstawiono niektóre możliwe rozwiązania architektury. Ten przegląd nie jest kompleksową listą wszystkich możliwych opcji, ale sugeruje sposoby wdrażania pakietu BHOLD w sieci.
 
W tej sekcji omówiono następujące tematy:

- Architektura jednego serwera
- Architektura dwóch serwerów
- Architektura dwuwarstwowa
- Zalecenia dotyczące programu SQL Server

### <a name="single-server-architecture"></a>Architektura jednego serwera

W przypadku wdrażania w małych organizacjach lub do celów deweloperskich można zainstalować pakietu BHOLD i program FIM na tym samym serwerze co SQL Server i AD DS, jak pokazano na poniższej ilustracji.
 
![Architektura jednego serwera](media/bhold-installation-guide/single.png)

Gdy program pakietu BHOLD Suite SP1 i Portal programu FIM są instalowane razem na jednym serwerze, należy utworzyć różne Aliasy hostów (CNAME lub rekordy A) w systemie DNS dla pakietu BHOLD i dla programu FIM. Pozwala to na Tworzenie oddzielnych głównych nazw usług (SPN) dla usług pakietu BHOLD i FIM. Aby uzyskać więcej informacji, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Aby uzyskać wskazówki dotyczące instalowania programu FIM w konfiguracji pojedynczego serwera, zobacz artykuł [Common Configuration for wprowadzenie przewodniki](https://technet.microsoft.com/library/ff575965.aspx) w bibliotece Microsoft TechNet.

### <a name="dual-server-architecture"></a>Architektura dwóch serwerów

Zainstalowanie pakietu BHOLD Core i programu FIM na oddzielnych serwerach zapewnia lepszą wydajność i elastyczność dla organizacji średniego rozmiaru, które nie wymagają bardziej złożonego wdrożenia, takiego jak zapewniane przez architektury wielowarstwowe. Na poniższej ilustracji przedstawiono pakietu BHOLD i FIM zainstalowane na własnych serwerach; serwer programu FIM jest również uruchomiony SQL Server, aby zapewnić usługi bazy danych pakietu BHOLD i FIM. Usługa synchronizacji FIM uruchomiona na serwerze programu FIM synchronizuje zmiany między bazami danych programu FIM i pakietu BHOLD. Należy pamiętać, że jeśli użytkownik końcowy nie jest wymagany, moduł integracji programu pakietu BHOLD FIM musi być zainstalowany na tym samym serwerze, na którym znajduje się usługa FIM i Portal programu FIM. Moduł integracji z programem FIM pakietu BHOLD wymaga, aby usługa FIM i moduł integracji programu FIM pakietu BHOLD zostały zainstalowane na tym samym serwerze.

![Architektura Dual Server](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> Funkcja raportowania modułu integracji z programem FIM pakietu BHOLD wymaga, aby bazy danych pakietu BHOLD i FIM były zainstalowane w tym samym wystąpieniu SQL Server, a konto usługi pakietu BHOLD musi mieć prawa dostępu do bazy danych usługi FIM.

### <a name="two-tier-architecture"></a>Architektura dwuwarstwowa

W większości środowisk, szczególnie w przypadku, gdy wydajność jest ważna, należy uruchomić pakiet pakietu BHOLD Suite z dodatkiem SP1, FIM i SQL Server na oddzielnych serwerach (architektura dwuwarstwowa). W przypadku architektury dwuwarstwowej zasoby pamięci i procesora CPU są dedykowane dla każdej warstwy. Na poniższej ilustracji przedstawiono jeden z możliwych metod konfigurowania architektury dwuwarstwowej. Usługa synchronizacji FIM uruchomiona na serwerze programu FIM synchronizuje zmiany między bazami danych programu FIM i pakietu BHOLD. Należy pamiętać, że jeśli użytkownik końcowy nie jest wymagany, moduł integracji programu pakietu BHOLD FIM musi być zainstalowany na tym samym serwerze, na którym znajduje się usługa i Portal programu FIM.

![Architektura dwuwarstwowa](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Zalecenia dotyczące programu SQL Server

Jeśli wdrażasz pakietu BHOLD w dużych organizacjach, zdecydowanie zalecamy przestrzeganie następujących wytycznych dotyczących konfigurowania Microsoft SQL Server bazy danych:

- Wdróż SQL Server na serwerze oddzielonym od dowolnych usług FIM lub pakietu BHOLD.
- Wyodrębnij plik dziennika z pliku danych na poziomie dysku fizycznego.
- Jeśli używasz macierzy RAID do zapewnienia nadmiarowości magazynu, użyj poziomu RAID 10 (1 + 0). Nie należy używać poziomu 5 RAID.
- Pamiętaj, aby skonfigurować poprawne ustawienia w przypadku używania więcej niż 2 GB pamięci fizycznej na serwerze z programem SQL Server.

Aby uzyskać więcej informacji o SQL Server najlepszych rozwiązaniach, zobacz [10 najlepszych](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) rozwiązań dotyczących magazynu w bibliotece Microsoft TechNet.

### <a name="trusted-certificates-list-update"></a>Aktualizacja listy zaufanych certyfikatów

System Windows można skonfigurować pod kątem weryfikowania łańcuchów certyfikatów przed rozpoczęciem usługi. W takich systemach nie można uruchomić usługi, jeśli kod wykonywalny usługi został podpisany przy użyciu certyfikatu, który nie znajduje się na liście zaufanych certyfikatów (TCL) serwera. Oprogramowanie Microsoft pakietu BHOLD Suite z dodatkiem SP1 to kod podpisany przy użyciu łańcucha certyfikatów podpisywania kodu, który pochodzi z certyfikatu głównego urzędu certyfikacji firmy Microsoft 2010.
System Windows można skonfigurować do pobierania certyfikatów głównych od firmy Microsoft przez połączenie internetowe. W odłączonym systemie system Windows Server zawiera jednak tylko te certyfikaty, które były obecne w programie głównym w danym momencie przed zwolnieniem systemu Windows. W wersjach systemu Windows Server starszych niż Windows Server 2010 te certyfikaty nie będą obejmować certyfikatu głównego wymaganego do sprawdzania poprawności łańcucha certyfikatów podpisywania kodu pakietu BHOLD Suite SP1. Jeśli zamierzasz zainstalować jeden lub więcej modułów Microsoft pakietu BHOLD Suite SP1 w systemie, który może nie mieć aktualnych TCL, musisz pobrać i zainstalować pakiet root-Update lub użyć zasady grupy, aby zainstalować pakiet root-Update, przed zainstalowaniem pakietu pakietu BHOLD Suite SP1 elementu. Aby uzyskać więcej informacji, zobacz [Członkowie programu certyfikatów głównych systemu Windows](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Instalowanie programu pakietu BHOLD Suite SP1 w systemie Windows Server 2012/2016 — wymagany krok 

![PAKIETU BHOLD instalacji usług IIS](media/bhold-installation-guide/iis-install-bhold.png)

W przypadku instalowania programu pakietu BHOLD Suite SP1 w systemie Windows Server 2012 lub 2016 strony sieci Web pakietu BHOLD nie będą dostępne do czasu modyfikacji pliku applicationHost. config znajdującego się ```C:\Windows\System32\inetsrv\config```w temacie. W sekcji Dodaj ```preCondition="bitness64``` do wpisu, który rozpoczyna się ```<add name="SPNativeRequestModule"``` w następujący sposób: ```<globalModules>```

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Po edytowaniu i zapisaniu pliku Uruchom polecenie iisreset, aby zresetować serwer usług IIS.


## <a name="upgrading-bhold-suite"></a>Uaktualnianie pakietu pakietu BHOLD

Nie można uaktualnić istniejącej instalacji pakietu pakietu BHOLD. Zamiast tego należy odinstalować istniejącą instalację zestawu pakietu BHOLD Suite, aby można było zaktualizować moduły pakietu BHOLD. Jeśli masz istniejący model roli pakietu BHOLD, możesz uaktualnić bazę danych pakietu BHOLD i użyć jej podczas instalowania zaktualizowanego modułu pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz temat [wymiana pakietu pakietu BHOLD z pakietem pakietu BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Następne kroki

- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
