---
title: Przewodnik planowania pojemności | Dokumentacja firmy Microsoft
description: Ten przewodnik ułatwia zrozumienie zmiennych, które należy wziąć pod uwagę przed wdrożeniem programu MIM 2016, w tym poziomów obciążeń i decyzji dotyczących zasad.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b14066543c036eb4ec8a350843743b87902a13a1
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "73636977"
---
# <a name="capacity-planning-guide"></a>Przewodnik planowania pojemności

Program Microsoft Identity Manager (MIM) umożliwia tworzenie, aktualizowanie i usuwanie kont użytkowników w całej organizacji. Daje również użytkownikom możliwość zarządzania funkcjami samoobsługi własnych kont. Nawet w niewielkim środowisku wszystkie te działania mogą się szybko skumulować.

Przed rozpoczęciem pracy z programem MIM skorzystaj z tego przewodnika wraz ze środowiskami testowymi, aby zrozumieć odpowiedni zakres wdrożenia. W tym artykule przedstawiono wiele typowych czynników, które należy wziąć pod uwagę. Każde wdrożenie jest wyjątkowe, w związku z czym przetestowanie scenariuszy w laboratorium to nadal najlepsza metoda dopasowania serwerów, urządzeń i topologii do potrzeb.

Jeśli nie znasz jeszcze programu MIM 2016 i jego składników, przed kontynuowaniem zapoznaj się z dodatkowymi informacjami na temat programu [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).

## <a name="overview"></a>Overview

Istnieje wiele czynników, które mogą mieć wpływ na ogólną pojemność i wydajność wdrożenia Microsoft Identity Manager:

- Sposoby, w których można fizycznie wdrożyć składniki programu MIM (topologia).
- Sprzęt, na którym działają te składniki.
- Liczba i złożoność obiektów konfiguracji zasad programu MIM to znaczące czynniki, które należy wziąć pod uwagę podczas planowania pojemności.
- Oczekiwana Skala wdrożenia i oczekiwane obciążenie są zazwyczaj bardziej oczywistymi czynnikami wpływającymi na wydajność i pojemność.

Główne czynniki wpływające na pojemność i wydajność wdrożenia programu MIM 2016 zostały omówione w poniższej tabeli:

| Współczynnik projektu | Kwestie do rozważenia |
| ------------- | -------------- |
| Topologia | Dystrybucja usług programu MIM między komputerami w sieci. |
| Sprzęt | Sprzęt fizyczny (fizyczny lub wirtualny) dla każdego składnika programu MIM, w tym Konfiguracja procesora CPU, pamięci, karty sieciowej i dysku twardego. |
| Obiekty konfiguracji zasad programu MIM | Liczba i typ obiektów konfiguracji zasad programu MIM, w tym zestawów, reguł zasad zarządzania (MPR) i przepływów pracy. |
| Skala | Użytkowników, grup, grup obliczeniowych i typów obiektów niestandardowych, które mają być zarządzane przez program MIM 2016. Ponadto należy wziąć pod uwagę złożoność grup dynamicznych oraz uwzględnić zagnieżdżanie grup. |
| Obciążenie | Częstotliwość używania. Operacje, takie jak nowe tworzenie grup lub użytkowników, resetowanie haseł lub wizyty w portalu na minutę lub godzinę. Należy pamiętać, że obciążenie może się różnić zależnie od godziny, dnia, tygodnia lub roku. Poszczególne składniki można projektować pod kątem obciążenia szczytowego lub średniego. |

## <a name="hosting-microsoft-identity-manager-components"></a>Hosting składników programu Microsoft Identity Manager

Składniki programu Microsoft Identity Manager nie muszą znajdować się na tym samym komputerze. Ważnym elementem planowania pojemności jest analiza tych składników oraz maszyn fizycznych i wirtualnych, które będą je hostować.

Czynniki sprzętowe mogą wpływać na wydajność środowiska programu MIM. Przykład:

- Jaka jest konfiguracja dysków fizycznych komputera, na którym uruchomiono bazę danych SQL usługi programu MIM 2016? Liczba jednostek wchodzących w skład konfiguracji dysków oraz rozkład plików dzienników i danych mogą wpłynąć w znaczący sposób na wydajność systemu.

Ponadto w konfiguracji należy wziąć pod uwagę czynniki zewnętrzne. Przykład:

- Jeśli używasz sieci SAN jako konfiguracji bazy danych usługi programu MIM 2016, jakie inne aplikacje korzystają z sieci SAN? Te aplikacje mogą wpływać na wydajność bazy danych, jeśli konkurują o współużytkowane zasoby dyskowe w sieci SAN.

## <a name="users-and-groups"></a>Użytkownicy i grupy

Liczba użytkowników i grup w danym środowisku to typowa kwestia do uwzględnienia podczas analizy skali wdrożenia. Jednak występuje także kilka innych powiązanych kwestii, które należy uwzględnić podczas planowania.

- Czy użytkownicy mogą tworzyć grupy? W takim przypadku warto oszacować, w jaki sposób tworzenie nowych grup użytkowników wpłynie na przyrost liczby grup w środowisku.

- Czy zostaną wdrożone grupy dynamiczne? Ustal liczbę i typy grup dynamicznych, które mogą pojawić się w danym środowisku.

## <a name="expected-load-levels"></a>Spodziewane poziomy obciążenia

Należy również rozważyć typ obciążenia, którego doświadczą składniki programu MIM. Należy oszacować obciążenie, przeglądając bieżące aplikacje w danym środowisku. Niektóre istotne pytania, które należy zadać, przedstawiono poniżej:

- Jak często będą pojawiać się żądania dołączenia do grupy lub opuszczenia jej?

- Jak często użytkownicy będą tworzyć grupy statyczne lub dynamiczne?

- Ile operacji niezależnych od użytkowników, takich jak synchronizacja zmian z systemów zewnętrznych, powinno występować? Upewnij się, że zostało uwzględnione obciążenie wygenerowane przez synchronizację danych tożsamości z systemami zewnętrznymi.

- Jakiego rodzaju scenariusze planujesz wdrożyć? Różne scenariusze przyczyniają się do różnych wzorców obciążenia. Na przykład komputery klienckie, na których zainstalowano klienta programu MIM 2016, okresowo weryfikują, czy rejestracja jest wymagana podczas logowania.

- Czy spodziewasz się dużej zmienności poziomów obciążeń między obciążeniem normalnym i szczytowym? Na przykład podczas okresów świątecznych może być resetowanych wiele haseł. Upewnij się, że harmonogramy konserwacji i synchronizacji systemu uwzględniają przewidywane obciążenia szczytowe. Przy planowaniu pojemności należy pamiętać o uwzględnieniu okresów obciążenia szczytowego.

## <a name="policy-configuration-objects"></a>Obiekty konfiguracji zasad

Obiekty konfiguracji zasad programu MIM obejmują reguł MPR, zestawy, przepływy pracy i reguły synchronizacji dla wdrożenia. Wdrożenia programu MIM różnią się pomiędzy klientami, ponieważ konfiguracja zasad zmienia się zgodnie z potrzebami poszczególnych wdrożeń. Kluczowe zagadnienia dotyczące wydajności obejmują następujące obiekty konfiguracji zasad programu MIM:

- **Zestawy** Każda operacja w systemie musi zostać oceniona pod kątem istniejących członkostw zestawów i aktualizacji, które powodują zmiany w członkostwach zestawów. Na przykład zmiana numeru budynku biura osoby może nie mieć dużego wpływu. Jednak inne zmiany mogą mieć wpływ bardzo poważny, na przykład zmiana menedżera, która może wpływać na wiele obiektów na różnych poziomach.

- **Reguły zasad zarządzania (MPR)** Reguły MPR zarządzają regułami kontroli dostępu i wyzwalają przepływy pracy. Tworzenie reguł MPR może stworzyć potrzebę zwiększenia liczby zestawów, aby można było przechwycić różne stany przejścia obiektów. Te dodatkowe zestawy mogą wyzwalać dodatkowe przepływy pracy. Każdy przepływ pracy odpowiada unikatowym żądaniom w systemie. W związku z tym jest to kolejny element do uwzględnienia podczas planowania pojemności.

Konfiguracja zasad programu MIM obejmuje także decyzje dotyczące inicjowania obsługi w danym środowisku. Pamiętaj o uwzględnieniu następujących kwestii:

- Czy obce zasady zabezpieczeń będą aprowizowane w wielu lasach Usług domenowych Active Directory (AD DS)? Działania takie generują więcej przepływów pracy i żądań, co prowadzi do dodatkowego obciążenia systemu.

- Czy będziesz używać aprowizacji bez kodu? Taka sytuacja wpływa na liczbę oczekiwanych wpisów reguł, jak również na skojarzone żądania i przepływy pracy w systemie.

## <a name="next-steps"></a>Następne kroki

- [Zagadnienia dotyczące topologii wdrożeń programu MIM](topology-considerations.md)
- [Przewodnik planowania pojemności programu Forefront Identity Manager (FIM) 2010](https://www.microsoft.com/en-us/download/details.aspx?id=7437) zawiera bardziej szczegółowe informacje na temat wyników testów kompilacji i wydajności.
