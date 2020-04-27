---
title: Przestarzałe funkcje programu MIM i planowanie w przyszłości | Microsoft Docs
description: Ten artykuł zawiera dokumenty przestarzałe programu MIM Identity Manager 2016 z dodatkiem SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 009d0e99e2da445d4df35dc9de81b297a65fe2a3
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044229"
---
# <a name="deprecated-features"></a>Przestarzałe funkcje

W tym artykule opisano przestarzałe funkcje programu Microsoft Identity Manager 2016 z dodatkiem SP1. Gdy funkcja jest nadal obecna w Microsoft Identity Manager, jest nadal obsługiwana. W przypadku nowych wdrożeń funkcje nie są zalecane, ponieważ mogą zostać usunięte w wersji funkcji.  W przypadku deweloperów nie zaleca się używania przestarzałych funkcji w nowych aplikacjach i rozwiązaniach.

> [!NOTE]
> Funkcja i funkcje usunięte w Microsoft Identity Manager SP1 są identyfikowane za pomocą * *. <br>
> Aby uzyskać więcej informacji na temat [cyklu pomocy technicznej dla Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Firma Microsoft nie zaleca klientom uruchamiania nowych wdrożeń składników Microsoft pakietu BHOLD Suite. Istniejące wdrożenia programu pakietu BHOLD będą nadal obsługiwane. Usługa Azure AD zapewnia teraz [przeglądy dostępu](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) , które zastępują niektóre funkcje kampanii zaświadczania pakietu BHOLD.

## <a name="certificate-management"></a>Zarządzanie certyfikatami 

| **Kategoria**                | **Przestarzała funkcja**              | **Zastąpienie i komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agenci zarządzania | * * Zarządzanie certyfikatami usługi FIM | Agent zarządzania certyfikatami usługi FIM został usunięty w programie MIM 2016.                                                             |

## <a name="service-and-portal"></a>Usługa i portal

| **Kategoria**                | **Przestarzała funkcja**              | **Zastąpienie i komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Konfiguracja programowa | Interfejs konfiguracji usługi sieci Web (dane o danych i MV) | Możliwość skonfigurowania usługi synchronizacji FIM za pomocą usługi sieci Web usługi FIM Service zostanie usunięta w następnej wersji.
|

## <a name="synchronization-service"></a>Usługa synchronizacji 

| **Kategoria**                | **Przestarzała funkcja**              | **Zastąpienie i komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Konfiguracja programowa | Interfejs konfiguracji usługi sieci Web | Możliwość skonfigurowania usługi synchronizacji FIM za pomocą usługi FIM zostanie usunięta w następnej wersji.                                                          |
| Agenci zarządzania           | Wbudowana MAs                        | Następujące MAs zostały usunięte w programie MIM 2016: </br> 1. * * MA dla zarządzania certyfikatami programu FIM </br>2. * * MA dla programu Lotus Notes</br> 3. * * MA dla SAP R/3 </br> Programy Lotus Notes i SAP R/3 MAs zostały zastąpione nowymi wersjami. Aby uzyskać więcej informacji, zobacz [Najnowsza wersja łącznika historia wersji & pobieranie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Agenci zarządzania           | ECMA1                               | Struktura rozszerzalności ECMA1/XMA została zastąpiona przez ECMA 2,0. Wymagane jest zaktualizowanie istniejących agentów zarządzania ECMA1 przy użyciu łączników ECMA 2.0.                                                                                                                                          |
| Agenci zarządzania           | Uruchomione łączniki poza procesem      | Ta funkcja nie zostanie zastąpiona. Usługa synchronizacji zawsze wywoła łącznik w tym samym procesie. Jest odpowiedzialny za łącznik do uruchamiania i zarządzania innym procesem. |
| Agenci zarządzania           | Konfiguruj nazwę wyświetlaną partycji    | Ta funkcja nie zostanie zastąpiona. Ta opcja była używana tylko w celu zapewnienia alternatywnej nazwy partycji w interfejsach usługi WMI.                                                                                                                                                                       |
| Profile uruchamiania                | Połączone profile                   | Zostaną usunięte przyrostowe Importowanie/Synchronizacja zmian/synchronizacji profilów, pełna synchronizacja/synchronizacje różnicowe i pełna Import/Synchronizacja. Zamiast tego należy użyć profili uruchamiania z dwoma krokami. 

> [!NOTE]
> Profile przebiegów połączonych należy przechowywać tylko w środowiskach, w których wydajność może być ograniczona przez dużą liczbę istniejących odłączeń.


| **Kategoria**                | **Przestarzała funkcja**              | **Zastąpienie i komentarz**           |
|--------|-------|---|    
| Pierwszeństwo atrybutów | Wiele wzorców/równe pierwszeństwo                       | Równy priorytet zostanie usunięty. Nie ma zamiennika dla tej funkcji. Zamiast tego należy skonfigurować pierwszeństwo ręczne. Można nadal używać tej funkcji, jeśli środowisko ma wdrożony agent zarządzania usługą FIM Service. Ten agent zarządzania nie zapewnia ręcznego pierwszeństwa, aby uniknąć eksportowania niepoprzedników dla aprowizacji deklaracyjnej. |
| Reguły dołączania           | Dołącz do typu obiektu "any"                             | Ta funkcja nie zostanie zastąpiona. Wszystkie reguły sprzężenia powinny jawnie definiować typ obiektu metaverse, do którego próbują się dołączyć.       |
| Przepływy atrybutów      | Usuń zaznaczenie opcji "Zezwalaj na wartości null" dla eksportowanych wartości            | Ta funkcja nie zostanie zastąpiona. Ustawienie "Zezwalaj na wartości null" będzie zawsze zaznaczone. Upewnij się, że w bieżącym środowisku wybrano opcję "Zezwalaj na wartości null".  |
| Przepływy atrybutów      | "Nie odwołują atrybutów"                            | Ta funkcja nie zostanie zastąpiona. Atrybuty zawsze będą odwoływane, co jest najlepszym rozwiązaniem.  |
| Rozszerzenie reguł      | Uruchamianie funkcji Metaverse i ma rozszerzenie reguł | Ta funkcja nie zostanie zastąpiona. Reguły Metaverse i przepływu atrybutów będą uruchamiane w tym samym procesie co aparat synchronizacji.       |
| Rozszerzenie reguł      | Właściwości transakcji                                | Ta funkcja nie zostanie zastąpiona. Należy unikać przekazywania danych między przychodzącymi, aprowizacji i synchronizacją wychodzącą przy użyciu tej klasy narzędzi.  |
| Rozszerzenie reguł      | ExchangeUtils: Create55\* metody                     | Metody tworzenia obiektów dla serwerów z programem Exchange 5,5 zostaną usunięte.        |
| Interface            | Mms_Metaverse                                        | Wszyscy członkowie klasy ClmUtils zostaną usunięci w następnej wersji.   |

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o usługach:

- Program Microsoft Identity Manager jest nadal ściśle związany ze swoim poprzednikiem, programem Forefront Identity Manager. Jeśli nadal korzystasz z programu FIM lub chcesz zapoznać się z dodatkową dokumentacją, zobacz [plan dokumentacji programu FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Zagadnienia dotyczące topologii dotyczące wdrażania programu MIM](topology-considerations.md) W tym artykule wprowadzono wiele topologii wdrożenia, które można rozważyć w celu zaimplementowania.
- [Przewodnik planowania pojemności](capacity-planning-guide.md) W tym przewodniku wraz ze środowiskami testowymi można zapoznać się z odpowiednim zakresem wdrożenia.
