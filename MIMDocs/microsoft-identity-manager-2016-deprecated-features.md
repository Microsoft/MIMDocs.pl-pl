---
title: MIM przestarzałe funkcje i planowania w przyszłości | Dokumentacja firmy Microsoft
description: Ten artykuł dokumenty przestarzałe funkcje programu MIM Identity Manager 2016 SP1.
keywords: ''
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 2/28/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 50f7b135ce0d5a46ea08068a7658b229759d2b50
ms.sourcegitcommit: 24bb3e82f55971696bdefa6c240f1a27f856e110
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/17/2018
---
# <a name="deprecated-features"></a>Przestarzałe funkcje

W tym artykule opisano przestarzałe funkcje programu Microsoft Identity Manager 2016 z dodatkiem SP1. W przypadku, gdy funkcja jest nadal obecna programu Microsoft Identity Manager, nadal jest obsługiwany. Funkcje nie są zalecane w przypadku nowych wdrożeń, ponieważ mogą zostać usunięte w wersji funkcji.  Dla deweloperów zaleca się nie przy użyciu przestarzałe funkcje w nowych aplikacji lub rozwiązania.

>[!NOTE]
Narzędzia i funkcje usunięte w dodatku SP1 dla programu Microsoft Identity Manager są oznaczone symbolem **. <br>
Aby uzyskać więcej informacji na temat obsługi [cyklu życia dla programu Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Nie zaleca się, że klientów rozpoczyna się nowych wdrożeń składników pakietu BHOLD firmy Microsoft. Istniejące wdrożenia BHOLD będzie w dalszym ciągu obsługiwana. Obecnie usługa Azure AD zapewnia [dostępu przeglądami](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) która zastępuje niektóre funkcje BHOLD zaświadczania kampanii.

## <a name="certificate-management"></a>Zarządzanie certyfikatami 
| **Kategoria**                | **Funkcja przestarzała**              | **Zastąpienie i komentarza**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agenci zarządzania | ** Zarządzanie certyfikatami FIM | Agent zarządzania certyfikatu FIM została usunięta w programie MIM 2016.                                                             |

## <a name="service-and-portal"></a>Usługa i portal

| **Kategoria**                | **Funkcja przestarzała**              | **Zastąpienie i komentarza**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programowe konfiguracji | Interfejs konfiguracji usługi sieci Web (danych ma i mv danych) | Możliwość konfigurowania usługi synchronizacji FIM za pośrednictwem usługi sieci web usługi FIM zostanie usunięta w następnej wersji.
|

## <a name="synchronization-service"></a>Usługa synchronizacji 

| **Kategoria**                | **Funkcja przestarzała**              | **Zastąpienie i komentarza**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programowe konfiguracji | Interfejs konfiguracji usługi sieci Web | Możliwość konfigurowania usługi synchronizacji FIM przez usługę FIM zostanie usunięta w następnej wersji.                                                          |
| Agenci zarządzania           | Wbudowane MAs                        | Następujące MAs zostały usunięte w programie MIM 2016: </br> 1. ** agenta zarządzania usługą FIM certyfikatu zarządzania </br>2. ** MA dla programu Lotus Notes</br> 3. ** MA dla SAP R/3 </br> Program Lotus Notes i SAP R/3 MAs zostały zastąpione nowe wersje. Aby uzyskać więcej informacji, zobacz [najnowsze Historia wersji łącznika i pobierania](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Agenci zarządzania           | ECMA1                               | Struktura rozszerzalności ECMA1/XMA została zastąpiona ECMA 2.0. Uaktualnianie istniejącego agentów zarządzania ECMA1 ECMA2.0 łączników jest wymagana.                                                                                                                                          |
| Agenci zarządzania           | Uruchomiona poza procesem łączników      | Ta funkcja nie zostanie zastąpiony. Usługa synchronizacji zawsze spowoduje wywołanie łącznika w tym samym procesie. Jest odpowiedzialny za łącznik, aby uruchomić i zarządzanie nimi innego procesu. |
| Agenci zarządzania           | Konfigurowanie partycji, nazwa wyświetlana    | Ta funkcja nie zostanie zastąpiony. Ta opcja została użyta tylko do zapewnienia alternatywnych nazw partycji w interfejsy usługi WMI.                                                                                                                                                                       |
| Profile uruchamiania                | Łączna profilów                   | Import/synchronizacja różnicowa Scalonej profile, synchronizacja pełna import/różnicowa i pełny import/synchronizacji zostaną usunięte. Zamiast tego należy użyć profilów uruchamiania z dwóch kroków. 

>[!NOTE]
Tylko w środowiskach gdy wydajność może mieć wpływ na dużą liczbę istniejących disconnectors Zachowaj Scalonej profile uruchamiania.


| **Kategoria**                | **Funkcja przestarzała**              | **Zastąpienie i komentarza**           |
|--------|-------|---|    
| Pierwszeństwo atrybutów | Pierwszeństwo Multi-nadzoru nad/równości                       | Taki sam priorytet zostaną usunięte. Nie zastępuje dla tej funkcji. Zamiast tego należy skonfigurować priorytet ręczne. Można nadal używać tej funkcji, jeśli środowiska pod kątem wdrożyć agenta zarządzania usługą FIM. Ten agent zarządzania nie ma pierwszeństwo ręczne w celu uniknięcia eksportu nie uprzednie deklaratywne udostępniania. |
| Dołącz zasady           | Dołącz do "" typ obiektu                             | Ta funkcja nie zostanie zastąpiony. Wszystkie reguły sprzężenia powinien jawnie definiować typ obiektu metaverse, który chce dołączyć do.       |
| Przepływów atrybutów      | Usuń zaznaczenie "dopuszcza wartości null" dla wartości wyeksportowanej            | Ta funkcja nie zostanie zastąpiony. "Dopuszcza wartości null" zawsze będzie zaznaczone. Należy upewnić się, że masz "Zezwalaj na wartości null" wybranego w bieżącym środowisku.  |
| Przepływów atrybutów      | "Odwołuje atrybuty"                            | Ta funkcja nie zostanie zastąpiony. Atrybuty będą zawsze przypomnieć, która jest najlepszym rozwiązaniem.  |
| Rozszerzenia reguł      | Uruchamianie metaverse i ma zasady poza procesem rozszerzenia | Ta funkcja nie zostanie zastąpiony. Reguły przepływu metaverse i atrybut zostaną uruchomione w tym samym procesie jako aparatu synchronizacji.       |
| Rozszerzenia reguł      | Właściwości transakcji                                | Ta funkcja nie zostanie zastąpiony. Należy unikać przekazywania danych między synchronizacji ruchu przychodzącego, inicjowania obsługi administracyjnej i wychodzącego za pomocą tej klasy narzędzia.  |
| Rozszerzenia reguł      | ExchangeUtils: Create55\* metody                     | Metody służące do tworzenia obiektów dla serwerów programu Exchange 5.5 zostaną usunięte.        |
| Interfejs            | Mms_Metaverse                                        | Wszystkie elementy członkowskie klasy ClmUtils zostanie usunięta w następnej wersji.   |

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na następujące tematy:

- Program Microsoft Identity Manager jest nadal ściśle związany ze swoim poprzednikiem, programem Forefront Identity Manager. Jeśli nadal korzystasz z programu FIM lub chcesz zapoznać się z dodatkową dokumentacją, zobacz [plan dokumentacji programu FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Zagadnienia dotyczące topologii wdrożeń MIM](topology-considerations.md) w tym artykule przedstawiono różne topologie wdrożenia, które można zastosować.
- [Przewodnik planowania pojemności](capacity-planning-guide.md) można użyć tego przewodnika oraz środowisk testowych, aby zrozumieć odpowiedni zakres wdrożenia.
