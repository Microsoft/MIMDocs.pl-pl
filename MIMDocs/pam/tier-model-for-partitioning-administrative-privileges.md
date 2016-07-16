---
title: "Model warstwy partycjonowania uprawnień administracyjnych | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 509c05bbda5f0a0b936518fb023000771c45d4f7


---

# Model warstwy partycjonowania uprawnień administracyjnych

W dobie obecnych zagrożeń należy zakładać, że osoba atakująca w końcu uzyska dostęp do systemu organizacji. Oznacza to, że zabezpieczenia wewnętrzne są równie ważne co ochrona zapewniana przez sieć obwodową. W tym artykule opisano model zabezpieczeń zapewniający ochronę przed podniesieniem uprawnień przez oddzielenie działań wymagających wysokich uprawnień od stref wysokiego ryzyka. Użycie tego modelu pozwala uzyskać komfortowe środowisko użytkownika przy zapewnieniu zgodności z zasadami zabezpieczeń i najlepszymi rozwiązaniami.

## Podniesienie uprawnień w lasach usługi Active Directory

Konta użytkowników, usług lub aplikacji, które mają przyznane stałe uprawnienia administracyjne do lasów usługi Active Directory (AD) systemu Windows Server, znacznie zwiększają poziom ryzyka dla misji i celów biznesowych organizacji. Konta te są często obiektem ataków, ponieważ w przypadku naruszenia ich zabezpieczeń osoba atakująca uzyska uprawnienia umożliwiające nawiązanie połączenia z innymi serwerami lub aplikacjami w domenie.

Model warstwy pozwala podzielić uprawnienia administracyjne na podstawie zarządzanych zasobów. Administratorzy sprawujący kontrolę nad stacjami roboczymi użytkowników zostają oddzieleni od osób, które zajmują się aplikacjami lub zarządzają tożsamościami w przedsiębiorstwie. Więcej informacji na temat tego modelu zawiera artykuł [Securing privileged access reference material](http://aka.ms/tiermodel) (Materiały referencyjne dotyczące zabezpieczania uprzywilejowanego dostępu).

## Zmniejszanie widoczności poświadczeń za pomocą ograniczeń logowania

Zmniejszenie ryzyka kradzieży poświadczeń dla kont z uprawnieniami administracyjnymi zwykle wymaga zmodyfikowania praktyk administracyjnych w celu ograniczenia narażenia na ataki. W pierwszej kolejności zalecane jest wykonanie następujących działań w organizacji:

- Ograniczenie liczby hostów, na których widoczne są poświadczenia administracyjne.
- Ograniczenie uprawnień roli do wymaganego minimum.
- Upewnienie się, że zadania administracyjne nie są wykonywane na hostach używanych przez użytkowników do wykonywania standardowych czynności (na przykład odbierania poczty e-mail czy przeglądania Internetu).

Następny krok obejmuje zaimplementowanie ograniczeń logowania oraz wdrożenie procesów i praktyk, które spełniają wymagania modelu warstwy. Najlepszym rozwiązaniem jest ograniczenie poświadczeń do najniższych uprawnień wymaganych dla danej roli w każdej warstwie.

Należy wymusić ograniczenia logowania, tak aby konta z wysokim poziomem uprawnień nie miały dostępu do mniej bezpiecznych zasobów. Na przykład:

- Administratorzy domeny (warstwa 0) nie mogą logować się do serwerów przedsiębiorstwa (warstwa 1) i stacji roboczych standardowych użytkowników (warstwa 2).
- Administratorzy serwerów (warstwa 1) nie mogą logować się do stacji roboczych standardowych użytkowników (warstwa 2).

>[!NOTE] 
> Administratorzy serwerów nie powinni należeć do grupy administratorów domeny. Pracownicy odpowiedzialni za zarządzanie zarówno kontrolerami domeny, jak i serwerami przedsiębiorstwa powinni mieć osobne konta.

Aby wymusić ograniczenia logowania, można użyć następujących rozwiązań:

- Ograniczenia uprawnień do logowania w ramach zasad grupy, w tym:  
    - Odmowa dostępu do tego komputera z sieci  
    - Odmowa logowania w trybie wsadowym  
    - Odmowa logowania w trybie usługi  
    - Odmowa logowania lokalnego  
    - Odmowa logowania za pomocą ustawień pulpitu zdalnego  
- Zasady uwierzytelniania i silosy, jeśli jest używany system Windows Server 2012 lub nowszy
- Uwierzytelnianie selektywne, jeśli konto należy do dedykowanego lasu administracyjnego

W artykule [Planning a bastion environment](planning-bastion-environment.md) (Planowanie środowiska bastionu) opisano dodawanie dedykowanego lasu administracyjnego dla programu Microsoft Identity Manager w celu ustanowienia kont administracyjnych.



<!--HONumber=Jun16_HO5-->


