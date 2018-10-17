---
title: Program MIM przestarzałe funkcje i planowania w przyszłości | Dokumentacja firmy Microsoft
description: Ten artykuł dokumentów przestarzałe funkcje programu MIM Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fcf9ec8387761b6f154a95d6100ef54a12d4caf8
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358443"
---
# <a name="deprecated-features"></a>Przestarzałe funkcje

W tym artykule opisano przestarzałe funkcje programu Microsoft Identity Manager 2016 z dodatkiem SP1. W przypadku, gdy ta funkcja jest nadal znajdują się w programie Microsoft Identity Manager, są nadal obsługiwane. Funkcje nie są zalecane w przypadku nowych wdrożeń, ponieważ mogą zostać usunięte w wersji funkcji.  Dla deweloperów zaleca się nie przy użyciu przestarzałe funkcje w nowych aplikacji lub rozwiązania.

> [!NOTE]
> Narzędzia i funkcje usunięte w dodatku SP1 dla programu Microsoft Identity Manager są oznaczone symbolem **. <br>
> Aby uzyskać więcej informacji na temat obsługi [cyklu życia programu Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Firma Microsoft zaleca klientom rozpocząć nowych wdrożeń składników pakietu BHOLD firmy Microsoft. Istniejące wdrożenia pakietu BHOLD będą w dalszym ciągu być obsługiwane. Teraz usługa Azure AD zapewnia [przeglądów dostępu](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) które zastępują niektóre funkcje kampanii zaświadczania pakietu BHOLD.

## <a name="certificate-management"></a>Zarządzanie certyfikatami 

| **Kategoria**                | **Przestarzałych funkcji**              | **Zastąpienie i komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agenci zarządzania | ** Zarządzanie certyfikatami programu FIM | Agent zarządzania usługi FIM certyfikat został usunięty w programie MIM 2016.                                                             |

## <a name="service-and-portal"></a>Usługa i portal

| **Kategoria**                | **Przestarzałych funkcji**              | **Zastąpienie i komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Konfigurację programistyczną | Interfejs konfiguracji usługi sieci Web (danych ma i mv-data) | Możliwość konfigurowania FIM synchronization service za pomocą programu FIM Usługa sieci web zostanie usunięta w następnej wersji.
|

## <a name="synchronization-service"></a>Usługa synchronizacji 

| **Kategoria**                | **Przestarzałych funkcji**              | **Zastąpienie i komentarz**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Konfigurację programistyczną | Interfejs konfiguracji usługi sieci Web | Możliwość konfigurowania FIM synchronization service za pośrednictwem usługi FIM service zostanie usunięta w następnej wersji.                                                          |
| Agenci zarządzania           | Wbudowane MAs                        | Następujące MAs zostały usunięte w programie MIM 2016: </br> 1. ** MA dla zarządzania certyfikatami programu FIM </br>2. ** agenta zarządzania dla programu Lotus Notes</br> 3. ** MA dla SAP R/3 </br> Program Lotus Notes i SAP R/3 MAs zostały zastąpione do nowych wersji. Aby uzyskać więcej informacji, zobacz [najnowsze Historia wersji łącznika i Pobierz](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Agenci zarządzania           | ECMA1                               | Struktura rozszerzalności ECMA1/XMA została zastąpiona przez ECMA w wersji 2.0. Aktualizowanie istniejących agentów zarządzania ECMA1 za pomocą łączników ECMA2.0 jest wymagana.                                                                                                                                          |
| Agenci zarządzania           | Uruchamianie łączników poza poza procesem      | Ta funkcja nie zostanie zastąpiona. Usługa synchronizacji zawsze spowoduje wywołanie łącznika w tym samym procesie. Jest odpowiedzialny za łącznik uruchomieniu i zarządzaniu inny proces. |
| Agenci zarządzania           | Skonfiguruj nazwę wyświetlaną partycji    | Ta funkcja nie zostanie zastąpiona. Ta opcja był używany tylko do dostarczania alternatywna nazwa podmiotu dla partycji za pomocą interfejsów WMI.                                                                                                                                                                       |
| Profile przebiegu                | Profile połączone                   | Importowanie/synchronizacja różnicowa połączone profilów, synchronizacja pełny import/różnicowa i pełny import/synchronizacja zostaną usunięte. Zamiast tego należy użyć profilów uruchamiania przy użyciu dwóch krokach. 

> [!NOTE]
> Tylko w środowiskach, gdzie będzie mieć wpływ na wydajność przez dużą liczbę istniejących disconnectors, należy zachować połączone profile uruchamiania.


| **Kategoria**                | **Przestarzałych funkcji**              | **Zastąpienie i komentarz**           |
|--------|-------|---|    
| Pierwszeństwo atrybutów | Pierwszeństwo Multi-nadzoru nad/równości                       | Równe pierwszeństwo zostaną usunięte. Nie ma zastępstwa dla tej funkcji. Zamiast tego należy skonfigurować ręcznie priorytet. Można nadal korzystać z tej funkcji w środowisku zawierającym agenta zarządzania usługi FIM Service wdrożone. Ten agent zarządzania nie zapewnia pierwszeństwo ręcznej w celu uniknięcia eksportu nie anulują deklaratywne aprowizacji. |
| Dołącz do reguły           | Dołącz do "Dowolna" typu obiektu                             | Ta funkcja nie zostanie zastąpiona. Wszystkie reguły dołączania należy jawnie zdefiniować typ obiektu metaverse, który chce dołączyć do.       |
| Przepływy atrybutów      | Usuń zaznaczenie "dopuszcza wartości null" dla wartości eksportowanych            | Ta funkcja nie zostanie zastąpiona. "Dopuszcza wartości null" zawsze zostanie wybrana. Należy upewnić się, że masz "Zezwalaj na wartości null" wybranego w bieżącym środowisku.  |
| Przepływy atrybutów      | "Nie przypominasz sobie atrybuty"                            | Ta funkcja nie zostanie zastąpiona. Atrybuty zawsze należy przypomnieć, co jest najlepszym rozwiązaniem.  |
| Rozszerzenia reguł      | Uruchamianie metaverse i ma reguły rozszerzenia-procesem | Ta funkcja nie zostanie zastąpiona. Reguły przepływu magazynu metaverse i atrybutów jest uruchamiana w tym samym procesie co aparat synchronizacji.       |
| Rozszerzenia reguł      | Właściwości transakcji                                | Ta funkcja nie zostanie zastąpiona. Należy unikać przekazywania danych między synchronizacji ruchu przychodzącego, inicjowania obsługi administracyjnej i wychodzących za pomocą tej klasy narzędzia.  |
| Rozszerzenia reguł      | ExchangeUtils: Create55\* metody                     | Metody służące do tworzenia obiektów dla serwerów programu Exchange 5.5 zostaną usunięte.        |
| Interfejs            | Mms_Metaverse                                        | Wszystkie elementy członkowskie klasy ClmUtils zostanie usunięta w następnej wersji.   |

## <a name="next-steps"></a>Kolejne kroki
Dowiedz się więcej na następujące tematy:

- Program Microsoft Identity Manager jest nadal ściśle związany ze swoim poprzednikiem, programem Forefront Identity Manager. Jeśli nadal korzystasz z programu FIM lub chcesz zapoznać się z dodatkową dokumentacją, zobacz [plan dokumentacji programu FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Zagadnienia dotyczące wdrażania programu MIM topologii](topology-considerations.md) w tym artykule przedstawiono różne topologie wdrożenia, które można rozważyć wykonania.
- [Przewodnik planowania pojemności](capacity-planning-guide.md) można użyć tego przewodnika oraz środowisk testowych, aby zrozumieć odpowiedni zakres dla danego wdrożenia.
