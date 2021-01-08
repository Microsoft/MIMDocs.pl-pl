---
title: Model warstwowy środowiska usługi PAM | Dokumentacja firmy Microsoft
description: Poznaj model warstwowy, który dzieli system na podstawie podatności na ryzyko.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 25bf28731f14331da386ae9f59b041c8c9ee33a1
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010579"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>Model warstwy partycjonowania uprawnień administracyjnych

W tym artykule opisano model zabezpieczeń zapewniający ochronę przed podniesieniem uprawnień przez oddzielenie działań wymagających wysokich uprawnień od stref wysokiego ryzyka.

> [!IMPORTANT]
> Model w tym artykule jest przeznaczony tylko dla izolowanych środowisk Active Directory przy użyciu usługi PAM programu MIM.  W przypadku środowisk hybrydowych zapoznaj się ze wskazówkami w [modelu dostępu przedsiębiorstwa](/security/compass/privileged-access-access-model).

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Podniesienie uprawnień w lasach usługi Active Directory

Konta użytkowników, usług lub aplikacji, które mają przyznane stałe uprawnienia administracyjne do lasów usługi Active Directory (AD) systemu Windows Server, znacznie zwiększają poziom ryzyka dla misji i celów biznesowych organizacji. Te konta są często wskazywane przez osoby atakujące, ponieważ w przypadku naruszenia zabezpieczeń osoba atakująca ma uprawnienia do łączenia się z innymi serwerami lub aplikacjami w domenie.

Model warstwy pozwala podzielić uprawnienia administracyjne na podstawie zarządzanych zasobów. Administratorzy sprawujący kontrolę nad stacjami roboczymi użytkowników zostają oddzieleni od osób, które zajmują się aplikacjami lub zarządzają tożsamościami w przedsiębiorstwie.

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Zmniejszanie widoczności poświadczeń za pomocą ograniczeń logowania

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

## <a name="next-steps"></a>Następne kroki

- W artykule [Planning a bastion environment](planning-bastion-environment.md) (Planowanie środowiska bastionu) opisano dodawanie dedykowanego lasu administracyjnego dla programu Microsoft Identity Manager w celu ustanowienia kont administracyjnych.
- [Zabezpieczanie urządzeń](/security/compass/concept-azure-managed-workstation) zapewnia dedykowany system operacyjny dla poufnych zadań, które są chronione przed atakami internetowymi i nosicielami zagrożeń.
