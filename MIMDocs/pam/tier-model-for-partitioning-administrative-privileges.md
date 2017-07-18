---
title: "Model warstwowy środowiska usługi PAM | Dokumentacja firmy Microsoft"
description: "Poznaj model warstwowy, który dzieli system na podstawie podatności na ryzyko."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4c3b43e50403890572e77773191a821cf247269c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# Model warstwy partycjonowania uprawnień administracyjnych
<a id="tier-model-for-partitioning-administrative-privileges" class="xliff"></a>

W dobie obecnych zagrożeń należy zakładać, że osoba atakująca w końcu uzyska dostęp do systemu organizacji. Oznacza to, że zabezpieczenia wewnętrzne są równie ważne co ochrona zapewniana przez sieć obwodową. W tym artykule opisano model zabezpieczeń zapewniający ochronę przed podniesieniem uprawnień przez oddzielenie działań wymagających wysokich uprawnień od stref wysokiego ryzyka. Użycie tego modelu pozwala uzyskać komfortowe środowisko użytkownika przy zapewnieniu zgodności z zasadami zabezpieczeń i najlepszymi rozwiązaniami.

## Podniesienie uprawnień w lasach usługi Active Directory
<a id="elevation-of-privilege-in-active-directory-forests" class="xliff"></a>

Konta użytkowników, usług lub aplikacji, które mają przyznane stałe uprawnienia administracyjne do lasów usługi Active Directory (AD) systemu Windows Server, znacznie zwiększają poziom ryzyka dla misji i celów biznesowych organizacji. Konta te są często obiektem ataków, ponieważ w przypadku naruszenia ich zabezpieczeń osoba atakująca uzyska uprawnienia umożliwiające nawiązanie połączenia z innymi serwerami lub aplikacjami w domenie.

Model warstwy pozwala podzielić uprawnienia administracyjne na podstawie zarządzanych zasobów. Administratorzy sprawujący kontrolę nad stacjami roboczymi użytkowników zostają oddzieleni od osób, które zajmują się aplikacjami lub zarządzają tożsamościami w przedsiębiorstwie. Więcej informacji na temat tego modelu zawiera artykuł [Securing privileged access reference material](http://aka.ms/tiermodel) (Materiały referencyjne dotyczące zabezpieczania uprzywilejowanego dostępu).

## Zmniejszanie widoczności poświadczeń za pomocą ograniczeń logowania
<a id="restricting-credential-exposure-with-logon-restrictions" class="xliff"></a>

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
