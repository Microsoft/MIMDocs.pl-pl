---
title: Przestarzałe funkcje programu MIM i planowanie w przyszłości | Microsoft Docs
description: W tym artykule udokumentowano przestarzałe funkcje programu MIM Identity Manager 2016 SP2.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 7/28/2020
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: cb0159ec70e161dc47140712bd2ac039786e034d
ms.sourcegitcommit: 50cee02a7146445bd6fa361349099c7b294792d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2020
ms.locfileid: "87443554"
---
# <a name="deprecated-features"></a>Przestarzałe funkcje

W tym artykule opisano przestarzałe funkcje programu Microsoft Identity Manager 2016 SP2. Gdy funkcja jest nadal obecna w Microsoft Identity Manager, jest nadal obsługiwana. W przypadku nowych wdrożeń funkcje nie są zalecane, ponieważ mogą zostać usunięte w przyszłej wersji poprawki lub dodatku Service Pack.  W przypadku deweloperów nie zaleca się używania przestarzałych funkcji w nowych aplikacjach i rozwiązaniach.

> [!NOTE]
>
> Aby uzyskać więcej informacji na temat nowych opcji pomocy technicznej dla programu MIM, zobacz [Opcje pomocy technicznej dla klientów Azure AD — wersja Premium](support-update-for-azure-active-directory-premium-customers.md).

## <a name="bhold"></a>BHOLD

Firma Microsoft nie zaleca klientom uruchamiania nowych wdrożeń składników Microsoft pakietu BHOLD Suite. Istniejące wdrożenia programu pakietu BHOLD będą nadal obsługiwane. Usługa Azure AD zapewnia teraz [przeglądy dostępu](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), które zastępują funkcje kampanii zaświadczania pakietu BHOLD oraz zarządzanie prawami, które zastępują funkcje przypisywania dostępu.

## <a name="service-and-portal"></a>Usługa i portal

| **Kategoria**                | **Przestarzała funkcja**              | **Komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programowa Konfiguracja synchronizacji | Interfejs konfiguracji usługi sieci Web (dane o danych i MV) | Możliwość konfigurowania usługi synchronizacji programu MIM, za pomocą usługi sieci Web programu MIM, może zostać usunięta w przyszłej poprawce lub dodatku Service Pack.
|

## <a name="synchronization-service"></a>Usługa synchronizacji 

Następujące MAs zostały usunięte w programie MIM 2016: </br> 1. MA na celu zarządzanie certyfikatami programu FIM </br>2. MA dla programu Lotus Notes</br> 3. MA dla SAP R/3 </br> Programy Lotus Notes i SAP R/3 MAs zostały zastąpione nowymi łącznikami. Aby uzyskać więcej informacji, zobacz [Najnowsza wersja łącznika historia wersji & pobierania](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).

Struktura rozszerzalności ECMA1/XMA została zastąpiona przez ECMA 2,0. Wymagane jest zaktualizowanie istniejących agentów zarządzania ECMA1 przy użyciu łączników ECMA 2.0.

| **Kategoria**                | **Przestarzała funkcja**              | **Komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agenci zarządzania           | Uruchomione łączniki poza procesem      | Usługa synchronizacji zawsze wywoła łącznik w tym samym procesie. Jest odpowiedzialny za łącznik do uruchamiania i zarządzania innym procesem. |
| Agenci zarządzania           | Konfiguruj nazwę wyświetlaną partycji    | Ta opcja była używana tylko w celu zapewnienia alternatywnej nazwy partycji w interfejsach usługi WMI.                                                                                                                                                                       |
| Profile uruchamiania                | Połączone profile                   | Mogą zostać usunięte przyrostowe Importowanie/synchronizowanie/synchronizacja, pełna Import/Synchronizacja różnicowa oraz pełny Import/Synchronizacja. W zamian użyj profilów uruchamiania z dwoma krokami.

> [!NOTE]
> Profile przebiegów połączonych należy przechowywać tylko w środowiskach, w których wydajność może być ograniczona przez dużą liczbę istniejących odłączeń.

| **Kategoria**                | **Przestarzała funkcja**              | **Komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Pierwszeństwo atrybutów | Wiele wzorców/równe pierwszeństwo                       | Może zostać usunięty równy priorytet. Zamiast tego należy skonfigurować pierwszeństwo ręczne. Można nadal używać tej funkcji, jeśli środowisko ma wdrożony agent zarządzania usługą FIM Service. Ten agent zarządzania nie zapewnia ręcznego pierwszeństwa, aby uniknąć eksportowania niepoprzedników dla aprowizacji deklaracyjnej. |
| Reguły dołączania           | Dołącz do typu obiektu "any"                             | Wszystkie reguły sprzężenia powinny jawnie definiować typ obiektu metaverse, do którego próbują się dołączyć.       |
| Przepływy atrybutów      | Usuń zaznaczenie opcji "Zezwalaj na wartości null" dla eksportowanych wartości            | Ustawienie "Zezwalaj na wartości null" będzie zawsze zaznaczone, więc upewnij się, że w bieżącym środowisku wybrano opcję "Zezwalaj na wartości null".  |
| Przepływy atrybutów      | "Nie odwołują atrybutów"                            | Atrybuty zawsze będą odwoływane, co jest najlepszym rozwiązaniem.  |
| Rozszerzenie reguł      | Uruchamianie funkcji Metaverse i ma rozszerzenie reguł | Reguły Metaverse i przepływu atrybutów będą uruchamiane w tym samym procesie co aparat synchronizacji.       |
| Rozszerzenie reguł      | Właściwości transakcji                                | Należy unikać przekazywania danych między przychodzącymi, aprowizacji i synchronizacją wychodzącą przy użyciu tej klasy narzędzi.  |
| Rozszerzenie reguł      | ExchangeUtils: Create55 \* metody                     | Metody tworzenia obiektów dla serwerów z programem Exchange 5,5 mogą zostać usunięte.        |
| Interfejs            | Mms_Metaverse                                        | Wszystkie elementy członkowskie klasy ClmUtils mogą zostać usunięte w przyszłej poprawce lub dodatku Service Pack.   |

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej:

- Program Microsoft Identity Manager jest nadal ściśle związany ze swoim poprzednikiem, programem Forefront Identity Manager. Jeśli nadal korzystasz z programu FIM lub chcesz zapoznać się z dodatkową dokumentacją, zobacz [plan dokumentacji programu FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Zagadnienia dotyczące topologii dotyczące wdrażania programu MIM](topology-considerations.md) W tym artykule wprowadzono wiele topologii wdrożenia, które można rozważyć w celu zaimplementowania.
- [Przewodnik planowania pojemności](capacity-planning-guide.md) W tym przewodniku wraz ze środowiskami testowymi można zapoznać się z odpowiednim zakresem wdrożenia.

