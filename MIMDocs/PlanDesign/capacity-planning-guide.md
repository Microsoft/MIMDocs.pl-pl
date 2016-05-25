---
# required metadata

title: Przewodnik planowania pojemności | Microsoft Identity Manager
description: Ten przewodnik ułatwia zrozumienie zmiennych, które należy wziąć pod uwagę przed wdrożeniem programu MIM 2016, w tym poziomów obciążeń i decyzji dotyczących zasad.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Przewodnik planowania pojemności

Po przygotowaniu do wdrożenia programu Microsoft Identity Manager (MIM) użyj tego przewodnika oraz środowisk testowych do zaprojektowania wdrożenia. W tym artykule przedstawiono wiele typowych czynników, które należy wziąć pod uwagę. Każde wdrożenie jest wyjątkowe, w związku z czym przetestowanie scenariuszy w laboratorium to nadal najlepsza metoda dopasowania serwerów, urządzeń i topologii do potrzeb.

Jeśli nie znasz jeszcze programu MIM 2016 i jego składników, przed kontynuowaniem zapoznaj się z dodatkowymi informacjami na temat programu [Microsoft Identity Manager 2016](/microsoft-identity-manager/understand-explore/microsoft-identity-manager-2016).

## Przegląd
Istnieją różne zmienne, które mogą wpłynąć na ogólną pojemność i wydajność wdrożenia programu Microsoft Identity Manager. Metody fizycznego wdrożenia składników (topologii) programu MIM oraz urządzeń, na których uruchomiono te składniki, to ważne czynniki wpływające na wydajność i pojemność zapewnianą przez wdrożenie programu MIM. Liczba i złożoność obiektów konfiguracji zasad programu MIM może być mniej oczywista, ale podczas planowania pojemności nadal należy uwzględnić pewne istotne czynniki. Zazwyczaj bardziej oczywistymi czynnikami wpływającymi na wydajność i pojemność są oczekiwana skala wdrożenia oraz planowane obciążenie wdrożenia.

Główne czynniki wpływające na pojemność i wydajność, których można oczekiwać od wdrożenia programu MIM 2016, omówiono w tabeli poniżej.

| Współczynnik projektu | Kwestie do rozważenia |
| ------------- | -------------- |
| Topologia | Dystrybucja usług programu MIM między komputerami w sieci. |
| Sprzęt | Dane techniczne urządzeń fizycznych i zwirtualizowanych związanych z poszczególnymi składnikami programu MIM. Jest to między innymi konfiguracja procesora CPU, pamięci, karty sieciowej i dysku twardego. |
| Obiekty konfiguracji zasad programu MIM | Liczba i typ obiektów konfiguracji zasad programu MIM, w tym zestawów, reguł zasad zarządzania (MPR) i przepływów pracy. |
| Skala | Liczba użytkowników, grup, grup obliczeniowych i typów obiektów niestandardowych, takich jak komputery do zarządzania przez program MIM 2016. Ponadto należy wziąć pod uwagę złożoność grup dynamicznych oraz uwzględnić zagnieżdżanie grup. |
| Obciążenie | Częstotliwość używania. Jest to na przykład częstotliwość tworzenia nowych grup lub użytkowników, resetowania haseł lub odwiedzin portalu w danym okresie. Należy pamiętać, że obciążenie może się różnić zależnie od godziny, dnia, tygodnia lub roku. Poszczególne składniki można projektować pod kątem obciążenia szczytowego lub średniego. |


## Hosting składników programu Microsoft Identity Manager

Składniki programu Microsoft Identity Manager nie muszą znajdować się na tym samym komputerze. Ważnym elementem planowania pojemności jest analiza tych składników oraz maszyn fizycznych i wirtualnych, które będą je hostować.

Czynniki sprzętowe mogą wpływać na wydajność środowiska programu MIM. Przykład:
- Jaka jest konfiguracja dysków fizycznych komputera, na którym uruchomiono bazę danych SQL usługi programu MIM 2016? Liczba jednostek wchodzących w skład konfiguracji dysków oraz rozkład plików dzienników i danych mogą wpłynąć w znaczący sposób na wydajność systemu.

Ponadto w konfiguracji należy wziąć pod uwagę czynniki zewnętrzne. Przykład:
- Jeśli używasz sieci SAN jako konfiguracji bazy danych usługi programu MIM 2016, jakie inne aplikacje korzystają z sieci SAN? Te aplikacje mogą wpływać na wydajność bazy danych, jeśli konkurują o współużytkowane zasoby dyskowe w sieci SAN.


## Użytkownicy i grupy
Liczba użytkowników i grup w danym środowisku to typowa kwestia do uwzględnienia podczas analizy skali wdrożenia. Jednak występuje także kilka innych powiązanych kwestii, które należy uwzględnić podczas planowania.

- Czy użytkownicy mogą tworzyć grupy? W takim przypadku warto oszacować, w jaki sposób tworzenie nowych grup użytkowników wpłynie na przyrost liczby grup w środowisku.

- Czy zostaną wdrożone grupy dynamiczne? Ustal liczbę i typy grup dynamicznych, które mogą pojawić się w danym środowisku.


## Spodziewane poziomy obciążenia
Należy również rozważyć typ obciążenia, którego doświadczą składniki programu MIM. Informacje te można prawdopodobnie oszacować, analizując aplikacje znajdujące się obecnie w środowisku. Niektóre istotne pytania, które należy zadać, przedstawiono poniżej:

- Jak często będą pojawiać się żądania dołączenia do grupy lub opuszczenia jej?

- Jak często użytkownicy będą tworzyć grupy statyczne lub dynamiczne?

- Ile operacji niezależnych od użytkowników, takich jak synchronizacja zmian z systemów zewnętrznych, powinno występować? Upewnij się, że zostało uwzględnione obciążenie wygenerowane przez synchronizację danych tożsamości z systemami zewnętrznymi.

- Jakiego rodzaju scenariusze planujesz wdrożyć? Różne scenariusze przyczyniają się do różnych wzorców obciążenia. Na przykład komputery klienckie, na których zainstalowano klienta programu MIM 2016, okresowo sprawdzają, czy logowanie wymaga rejestracji, co zwiększa obciążenie systemu.

- Czy spodziewasz się dużej zmienności poziomów obciążeń między obciążeniem normalnym i szczytowym? Na przykład po okresach świątecznych zwiększa się częstotliwość resetowania haseł. Upewnij się, że harmonogramy konserwacji i synchronizacji systemu uwzględniają przewidywane obciążenia szczytowe. Przy planowaniu pojemności należy pamiętać o uwzględnieniu okresów obciążenia szczytowego.


## Obiekty konfiguracji zasad

Obiekty konfiguracji zasad programu Microsoft Identity Manager obejmują reguły MPR, zestawy, przepływy pracy i reguły synchronizacji dla konkretnego wdrożenia. Wdrożenia programu MIM różnią się pomiędzy klientami, ponieważ konfiguracja zasad zmienia się zgodnie z potrzebami poszczególnych wdrożeń. Kluczowe zagadnienia dotyczące wydajności związane z obiektami konfiguracji zasad programu MIM obejmują następujące kwestie:

- **Zestawy** Każda operacja w systemie musi zostać oceniona pod kątem istniejących członkostw zestawów i aktualizacji, które powodują zmiany w członkostwach zestawów. Na przykład prosta zmiana, taka jak zmiana numeru budynku dla biura danej osoby, może nie wywierać wyraźnego wpływu. Jednak inne zmiany mogą mieć wpływ bardzo poważny, na przykład zmiana menedżera, która może wpływać na wiele obiektów na różnych poziomach.

- **Reguły zasad zarządzania (MPR)** Reguły MPR zarządzają regułami kontroli dostępu i wyzwalają przepływy pracy. Podczas tworzenia reguł MPR może zajść konieczność zwiększenia liczby zestawów w celu zarejestrowania różnych stanów przejścia obiektów. Te dodatkowe zestawy mogą wyzwalać dodatkowe przepływy pracy. Każdy przepływ pracy odpowiada unikatowym żądaniom w systemie. W związku z tym jest to kolejny element do uwzględnienia podczas planowania pojemności.

Konfiguracja zasad programu MIM obejmuje także decyzje dotyczące inicjowania obsługi w danym środowisku. Pamiętaj o uwzględnieniu następujących kwestii:

- Czy obce zasady zabezpieczeń będą aprowizowane w wielu lasach Usług domenowych Active Directory (AD DS)? Działania takie generują więcej przepływów pracy i żądań, co prowadzi do dodatkowego obciążenia systemu.

- Czy będziesz używać aprowizacji bez kodu? Taka sytuacja wpływa na liczbę oczekiwanych wpisów reguł, jak również na skojarzone żądania i przepływy pracy w systemie.


## Zobacz też
- [Zagadnienia dotyczące topologii wdrożeń programu MIM](topology-considerations.md)
- Dostępny do pobrania podręcznik [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) (Podręcznik planowania pojemności programu Forefront Identity Manager [FIM] 2010) zawiera dodatkowe informacje na temat kompilacji testowych i wyników testowania wydajności.


<!--HONumber=Apr16_HO4-->


