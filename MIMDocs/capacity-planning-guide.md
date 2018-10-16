---
title: Przewodnik planowania pojemności | Dokumentacja firmy Microsoft
description: Ten przewodnik ułatwia zrozumienie zmiennych, które należy wziąć pod uwagę przed wdrożeniem programu MIM 2016, w tym poziomów obciążeń i decyzji dotyczących zasad.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5a54cd60f8ccee51d3bdb9cdb4770ff659afe854
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333806"
---
# <a name="capacity-planning-guide"></a>Przewodnik planowania pojemności

Program Microsoft Identity Manager (MIM) umożliwia tworzenie, aktualizowanie i usuwanie kont użytkowników w całej organizacji. Daje również użytkownikom możliwość zarządzania funkcjami samoobsługi własnych kont. Nawet w niewielkim środowisku wszystkie te działania mogą się szybko skumulować.

Przed rozpoczęciem pracy z programem MIM skorzystaj z tego przewodnika wraz ze środowiskami testowymi, aby zrozumieć odpowiedni zakres wdrożenia. W tym artykule przedstawiono wiele typowych czynników, które należy wziąć pod uwagę. Każde wdrożenie jest wyjątkowe, w związku z czym przetestowanie scenariuszy w laboratorium to nadal najlepsza metoda dopasowania serwerów, urządzeń i topologii do potrzeb.

Jeśli nie znasz jeszcze programu MIM 2016 i jego składników, przed kontynuowaniem zapoznaj się z dodatkowymi informacjami na temat programu [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).

## <a name="overview"></a>Przegląd

Istnieje wiele czynników, które mogą wpłynąć na ogólną pojemność i wydajność wdrożenia programu Microsoft Identity Manager:

- Sposoby, w którym fizycznego wdrożenia składników (topologii) programu MIM.
- Sprzęt, na którym te składniki są uruchomione.
- Liczba i złożoność obiektów konfiguracji zasad programu MIM są pewne istotne czynniki, które należy uwzględnić podczas planowania pojemności.
- Oczekiwana Skala wdrożenia oraz oczekiwane obciążenia są zazwyczaj bardziej oczywistymi czynnikami, które mają wpływ na wydajność i pojemność.

Główne czynniki wpływające na pojemność i wydajność wdrożenia programu MIM 2016 zostały omówione w poniższej tabeli:

| Współczynnik projektu | Kwestie do rozważenia |
| ------------- | -------------- |
| Topologia | Dystrybucja usług programu MIM między komputerami w sieci. |
| Sprzęt | Sprzęt fizyczny (fizyczny lub wirtualny) dla każdego składnika programu MIM, w tym procesora CPU, pamięć, karty sieciowej i konfiguracją dysku twardego. |
| Obiekty konfiguracji zasad programu MIM | Liczba i typ obiektów konfiguracji zasad programu MIM, w tym zestawów, reguł zasad zarządzania (MPR) i przepływów pracy. |
| Skala | Użytkowników, grup, grup obliczeniowych i typów obiektów niestandardowych mają być zarządzane przez program MIM 2016. Ponadto należy wziąć pod uwagę złożoność grup dynamicznych oraz uwzględnić zagnieżdżanie grup. |
| Obciążenie | Częstotliwość używania. Resetuje hasło operacji, takich jak nowej grupy lub tworzenie użytkownika, lub portalu odwiedzin minuta lub godzina. Należy pamiętać, że obciążenie może się różnić zależnie od godziny, dnia, tygodnia lub roku. Poszczególne składniki można projektować pod kątem obciążenia szczytowego lub średniego. |

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

Należy również rozważyć typ obciążenia, którego doświadczą składniki programu MIM. Będzie trzeba oszacować obciążenia, analizując bieżące aplikacje w danym środowisku. Niektóre istotne pytania, które należy zadać, przedstawiono poniżej:

- Jak często będą pojawiać się żądania dołączenia do grupy lub opuszczenia jej?

- Jak często użytkownicy będą tworzyć grupy statyczne lub dynamiczne?

- Ile operacji niezależnych od użytkowników, takich jak synchronizacja zmian z systemów zewnętrznych, powinno występować? Upewnij się, że zostało uwzględnione obciążenie wygenerowane przez synchronizację danych tożsamości z systemami zewnętrznymi.

- Jakiego rodzaju scenariusze planujesz wdrożyć? Różne scenariusze przyczyniają się do różnych wzorców obciążenia. Na przykład komputery klienckie, które okresowo zainstalowanego klienta programu MIM 2016 Sprawdź, czy rejestracja jest wymagana podczas logowania.

- Czy spodziewasz się dużej zmienności poziomów obciążeń między obciążeniem normalnym i szczytowym? Na przykład świątecznych zwiększa się wiele operacji resetowania hasła po okresach. Upewnij się, że harmonogramy konserwacji i synchronizacji systemu uwzględniają przewidywane obciążenia szczytowe. Przy planowaniu pojemności należy pamiętać o uwzględnieniu okresów obciążenia szczytowego.

## <a name="policy-configuration-objects"></a>Obiekty konfiguracji zasad

Obiekty konfiguracji zasad programu MIM obejmują reguły MPR, zestawy, przepływy pracy i reguły synchronizacji do wdrożenia. Wdrożenia programu MIM różnią się pomiędzy klientami, ponieważ konfiguracja zasad zmienia się zgodnie z potrzebami poszczególnych wdrożeń. Kluczowe zagadnienia dotyczące wydajności obejmują poniższe obiekty konfiguracji zasad programu MIM:

- **Zestawy** Każda operacja w systemie musi zostać oceniona pod kątem istniejących członkostw zestawów i aktualizacji, które powodują zmiany w członkostwach zestawów. Na przykład zmiana numeru budynku dla biura danej osoby nie mogą mieć duży wpływ. Jednak inne zmiany mogą mieć wpływ bardzo poważny, na przykład zmiana menedżera, która może wpływać na wiele obiektów na różnych poziomach.

- **Reguły zasad zarządzania (MPR)** Reguły MPR zarządzają regułami kontroli dostępu i wyzwalają przepływy pracy. Tworzenie reguł MPR może spowodować konieczność zwiększenia liczby zestawów, dzięki czemu można przechwycić różnych stanów przejścia obiektów. Te dodatkowe zestawy mogą wyzwalać dodatkowe przepływy pracy. Każdy przepływ pracy odpowiada unikatowym żądaniom w systemie. W związku z tym jest to kolejny element do uwzględnienia podczas planowania pojemności.

Konfiguracja zasad programu MIM obejmuje także decyzje dotyczące inicjowania obsługi w danym środowisku. Pamiętaj o uwzględnieniu następujących kwestii:

- Czy obce zasady zabezpieczeń będą aprowizowane w wielu lasach Usług domenowych Active Directory (AD DS)? Działania takie generują więcej przepływów pracy i żądań, co prowadzi do dodatkowego obciążenia systemu.

- Czy będziesz używać aprowizacji bez kodu? Taka sytuacja wpływa na liczbę oczekiwanych wpisów reguł, jak również na skojarzone żądania i przepływy pracy w systemie.

## <a name="next-steps"></a>Kolejne kroki

- [Zagadnienia dotyczące topologii wdrożeń programu MIM](topology-considerations.md)
- Dostępny do pobrania [przewodnika planowania pojemności programu Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) przechodzi w stan więcej szczegółów na temat kompilacji testowych i wyników testowania wydajności.
