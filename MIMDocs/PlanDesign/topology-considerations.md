---
title: "Kwestie dotyczące topologii wdrożeń programu MIM | Microsoft Identity Manager"
description: "W tym artykule opisano składniki programu MIM 2016 i przedstawiono sugestie dotyczące wdrażania ich w środowisku docelowym."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c023d147d0fcc1525fefbe866c952e217f7bee6b
ms.openlocfilehash: e33a08d77a0b5c422cdbc8c19516b55df980a2c6


---


# Kwestie dotyczące topologii
Składniki programu Microsoft Identity Manager (MIM) można wdrożyć na tym samym serwerze lub na wielu serwerach w wielu konfiguracjach. Wybór topologii wdrożenia wpływa na wydajność programu MIM. W tym artykule przedstawiono różne topologie wdrożenia, które można zastosować.

## Składniki programu MIM
Podczas projektowania topologii wdrożenia musisz znać funkcje wszystkich składników i interakcje między nimi.

- **Portal programu MIM** — interfejs umożliwiający resetowanie haseł, zarządzanie grupami i wykonywanie operacji administracyjnych.
    -
- **Usługa MIM** — usługa sieci Web, która implementuje funkcje zarządzania tożsamościami programu MIM 2016.
- **Usługa synchronizacji programu MIM** — synchronizuje dane z innymi systemami zarządzania tożsamościami.
- **Microsoft SQL Server** — dane usługi MIM i usługi synchronizacji programu MIM są przechowywane w bazach danych SQL.

W poniższej tabeli przedstawiono opcje hostingu dla poszczególnych składników programu MIM. Składniki mogą być hostowane na tym samym komputerze lub mogą zostać rozproszone między wiele serwerów i klastrów.

| | Portal programu MIM | Usługa MIM | Usługa synchronizacji programu MIM | Serwer SQL |
| --- | --- | --- | --- | --- |
| Ten sam komputer | Tak | Tak | Tak | Tak |
| Osobny serwer | Tak | Tak | Tak | Tak |
| Klaster równoważenia obciążenia sieciowego | Tak | Tak | | |
| Klaster serwerów | | | | Tak |


## Topologia wielowarstwowa
Topologia wielowarstwowa jest najczęściej używaną topologią. Zapewnia ona największą elastyczność. Portal programu MIM, usługa MIM oraz bazy danych są podzielone na warstwy i wdrażane na wielu komputerach. Ta topologia zwiększa elastyczność skalowania poszczególnych składników programu MIM. Można na przykład skalować portal programu MIM w poziomie przez dodanie serwerów w klastrze równoważenia obciążenia sieciowego. Można też skalować usługę MIM przy użyciu klastra równoważenia obciążenia sieciowego oraz przez odpowiednie zwiększenie liczby komputerów (węzłów) w klastrze.

W przypadku topologii wielowarstwowej jest przydzielany dedykowany komputer hostujący każdą bazę danych SQL (jeden komputer dla usługi MIM i jeden komputer dla usługi synchronizacji programu MIM). Skalowalność wydajności komputerów hostujących bazy danych SQL można zwiększyć przez dodanie lub zmodernizowanie sprzętu, na przykład zmodernizowanie procesorów CPU, dodanie procesorów CPU, zwiększenie ilości pamięci RAM lub zmodernizowanie pamięci RAM albo zmodernizowanie konfiguracji dysków twardych w celu przyspieszenia odczytu i zapisu oraz zmniejszenia opóźnienia.

![Diagram topologii wielowarstwowej programu MIM](media/MIM-topo-multitier.png)

W tej konfiguracji usługa synchronizacji programu MIM i jej baza danych są hostowane na tym samym komputerze. Można jednak uzyskać podobną wydajność, gdy istnieje 1-gigabitowe dedykowane połączenie sieciowe między usługą synchronizacji programu MIM i jej bazą danych oraz jeśli są one hostowane na osobnych komputerach.


## Topologia wielowarstwowa z wieloma usługami MIM
Synchronizowanie danych z systemami zewnętrznymi może być czasochłonne i znacząco zwiększać obciążenie systemu w tym czasie. Jeśli konfiguracja synchronizacji powoduje wyzwalanie zasad z przepływami pracy, te zasady będą konkurować o zasoby z przepływami pracy użytkowników końcowych. Takie problemy mogą występować w przypadku przepływów pracy uwierzytelniania, takich jak resetowanie haseł, które są wykonywane w czasie rzeczywistym, gdy użytkownik końcowy oczekuje na ukończenie procesu. Korzystanie z jednego wystąpienia usługi MIM dla operacji użytkowników końcowych i z osobnego portalu na potrzeby synchronizacji danych administracyjnych zapewnia krótszy czas odpowiedzi dla operacji użytkowników końcowych.

![Diagram topologii wielowarstwowej z wieloma usługami MIM](media/MIM-topo-multitier-multiservice.png)

Tak jak w przypadku standardowej topologii wielowarstwowej można zwiększyć wydajność portalu programu MIM przy użyciu klastra równoważenia obciążenia sieciowego i przez odpowiednie zwiększenie liczby węzłów w klastrze.

Komputery z zainstalowanym programem SQL Server hostujące usługę synchronizacji programu MIM i bazę danych usługi MIM mają znaczący wpływ na ogólną wydajność wdrażania programu MIM. Dlatego należy postępować zgodnie z zaleceniami w dokumentacji programu SQL Server dotyczącymi optymalizacji wydajności bazy danych. Więcej informacji można znaleźć w następujących dokumentach:

## Zobacz też
- Dostępny do pobrania podręcznik [Forefront Identity Manager (FIM) 2010 Capactity Planning Guide](http://go.microsoft.com/fwlink/?LinkId=200180) (Podręcznik planowania pojemności programu Forefront Identity Manager [FIM] 2010) zawiera dodatkowe informacje na temat kompilacji testowych i wyników testowania wydajności.



<!--HONumber=Jun16_HO4-->


