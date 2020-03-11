---
title: Uaktualnienie z programu FIM 2010 R2 i programu MIM 2016 SP2 do Microsoft Identity Manager 2016 z dodatkiem Service Pack 2 | Microsoft Docs
description: Dowiedz się, jak uaktualnić składniki programu FIM 2010 R2 lub MIM 2016 SP2, a następnie zainstalować składniki, które są nowe w programie MIM 2016.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: bfe604795d44cdea61fe0d10bc2943f9b8433784
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044155"
---
# <a name="mim-2016-sp2-upgrade--from-forefront-identity--or-microsoft-identity-manager"></a>Uaktualnienie programu MIM 2016 SP2 z tożsamości Forefront lub Microsoft Identity Manager

Organizacje mogą przeprowadzić uaktualnienie do Microsoft Identity Manager 2016 SP2 ze starszych wersji Microsoft Identity Manager lub programu Forefront Identity Manager.  Każda sekcja w tym artykule obejmuje obsługiwaną ścieżkę uaktualnienia.

Dostępnych jest kilka opcji uaktualniania. Jeśli korzystasz już z programu MIM 2016 i nie musisz odświeżać podstawowej platformy (Windows Server, SQL, SharePoint, SCSM DW) ani uruchamiać usług programu MIM przy użyciu kont usług zarządzanych przez grupę, a nie używasz pakietów językowych programu MIM, najprostszą opcją będzie instalacja uaktualnienia/poprawek w miejscu (msp). W przeciwnym razie zalecana jest pełna instalacja.

> [!IMPORTANT]
> Zapoznaj się z sekcją [wymagania wstępne dotyczące oprogramowania](prepare-server-ws2016.md#software-prerequisites) przed zainstalowaniem programu MIM 2016 SP2

## <a name="upgrade-from-fim-2010-r2-sp1-or-later-fim-builds"></a>Uaktualnianie z programu FIM 2010 R2 z dodatkiem SP1 lub nowsze kompilacje FIM

> [!NOTE]
> Minimalna obsługiwana wersja programu Forefront Identity Manager, która może zostać uaktualniona bezpośrednio do programu MIM 2016 SP2, to FIM 2010 R2 SP1 (kompilacja 4.1.3419.0). Bezpośrednie uaktualnienie do programu MIM 2016 SP2 z wcześniejszych wersji programu FIM nie jest obsługiwane. Jeśli korzystasz z kompilacji programu FIM wcześniej niż 4.1.3419.0, musisz przeprowadzić uaktualnienie do wersji FIM 2010 R2 SP1 przed uaktualnieniem do programu MIM 2016 SP2.

1. **Opcja 1: Pełna instalacja przy użyciu istniejących baz danych**
    1. Utwórz kopię zapasową baz danych FIMSynchronizationService i usługi FIMService.
    1. Eksportuj wszystkie obiekty usługi FIM Service RCDC i ciągi zasobów RCDC, do których wprowadzono zmiany.
    1. Eksportuj klucze szyfrowania usługi synchronizacji.
    1. Backup MIISServer. exe. config, folder "Extensions" serwera synchronizacji, Microsoft. ResourceManagement. Service. exe. config, ponieważ Instalator programu MIM może zastępować niestandardowe zmiany wprowadzone w plikach konfiguracji.
    1. Odinstaluj **wszystkie** składniki programu FIM, w tym pakiety językowe (najpierw do odinstalowania).
    1. *Opcjonalne:* Przenieś bazy danych programu FIM na inny serwer SQL Server. Zaleca się utworzenie aliasów SQL na serwerach MIM i użycie tych aliasów zamiast rzeczywistej nazwy programu SQL Server, aby ułatwić migrację programu MIM baz danych w przyszłości.
    1. Zainstaluj program MIM 2016 z dodatkiem SP2 z nośnika instalacyjnego ISO na tym samym lub na innym serwerze, który znajduje się w [przewodniku wdrażania programu MIM](microsoft-identity-manager-deploy.md), wybierz opcję Użyj istniejących baz danych po wyświetleniu monitu, podaj wcześniej wyeksportowane klucze szyfrowania usługi synchronizacji.
    1. Przejrzyj pliki MIISServer. exe. config i Microsoft. ResourceManagement. Service. exe. config dla brakujących przekierowań .NET lub dodanych niestandardowych sekcji.
    1. W razie konieczności Zainstaluj pakiety językowe programu MIM 2016 SP2.
    1. W razie potrzeby należy ponownie przesłać dostosowania do obiektów RCDC i RCDC i lokalizacji.
    1. Uaktualnij Dodatki FIM i klientów resetowania hasła, podaj nową nazwę serwera usługi programu MIM, Jeśli zmieniono nazwę serwera usługi programu MIM.
    
## <a name="upgrade-from-previous-mim-2016-builds"></a>Uaktualnianie z poprzednich kompilacji programu MIM 2016
1. Utwórz kopię zapasową baz danych FIMSynchronizationService i usługi FIMService.
1. Eksportuj wszystkie niestandardowe lokalizacje usługi FIM do obiektów RCDC i ciągów zasobów RCDC.
1. Eksportuj klucze szyfrowania usługi synchronizacji.
1. Backup MIISServer. exe. config, folder "Extensions" serwera synchronizacji, Microsoft. ResourceManagement. Service. exe. config, ponieważ Instalator programu MIM może zastępować niestandardowe zmiany wprowadzone w plikach konfiguracji.
1. Odinstaluj pakiety językowe programu MIM, jeśli są używane.
1. Zatrzymaj usługi programu MIM.
1. **Opcja 1: uaktualnianie w miejscu — Instalacja poprawek**
    1. Zastosuj [poprawkę](https://www.microsoft.com/download/details.aspx?id=100412) usługi synchronizacji programu MIM 2016 SP2
    1. Zastosuj [poprawkę](https://www.microsoft.com/download/details.aspx?id=100412) usługi programu MIM 2016 SP2
    1. Zapoznaj się z plikami MIISServer. exe. config i Microsoft. ResourceManagement. Service. exe. config brakujących przekierowań .NET lub dowolnych sekcji niestandardowych, które należy dodać.
    1. W razie konieczności Zainstaluj pakiety językowe programu MIM 2016 SP2.
    1. W razie potrzeby należy ponownie przesłać dostosowania do obiektów RCDC i RCDC i lokalizacji.
    1. Uaktualnij Dodatki programu MIM 2016 i klientów resetowania haseł.
1. **Opcja 2: Pełna instalacja przy użyciu istniejących baz danych**
    1. Odinstaluj **wszystkie** składniki programu MIM.
    1. *Opcjonalne:* Przenieś bazy danych programu FIM na inny serwer SQL Server. Zaleca się utworzenie aliasów SQL na serwerach MIM i użycie tych aliasów zamiast rzeczywistej nazwy programu SQL Server, aby ułatwić migrację programu MIM baz danych w przyszłości.
    1. Zainstaluj program MIM 2016 z dodatkiem SP2 z nośnika instalacyjnego ISO na tym samym lub na innym serwerze, który znajduje się w [przewodniku wdrażania programu MIM](microsoft-identity-manager-deploy.md), wybierz opcję Użyj istniejących baz danych po wyświetleniu monitu, podaj wcześniej wyeksportowane klucze szyfrowania usługi synchronizacji.
    1. Zapoznaj się z plikami MIISServer. exe. config i Microsoft. ResourceManagement. Service. exe. config brakujących przekierowań .NET lub dowolnych sekcji niestandardowych, które należy dodać.
    1. W razie konieczności Zainstaluj pakiety językowe programu MIM 2016 SP2.
    1. W razie potrzeby należy ponownie przesłać dostosowania do obiektów RCDC i RCDC i lokalizacji.
    1. Uaktualnij Dodatki programu MIM 2016 i klientów resetowania hasła, podaj nową nazwę serwera usługi MIM, Jeśli zmieniono nazwę serwera usługi programu MIM.

> [!NOTE]
> Aktualizacje pakietów językowych po zainstalowaniu programu MIM 2016 SP2 będą dystrybuowane jako poprawki (pliki msp), eliminując konieczność odinstalowywania/ponownego instalowania pakietów językowych.

Więcej szczegółowych informacji na temat procedur uaktualniania i tworzenia baz danych można znaleźć w artykule [uaktualnienie do programu fim 2010 R2](https://docs.microsoft.com/previous-versions/mim/jj134291%28v%3dws.10%29) , który ma zastosowanie do dowolnego procesu uaktualniania usługi FIM lub MIM.
