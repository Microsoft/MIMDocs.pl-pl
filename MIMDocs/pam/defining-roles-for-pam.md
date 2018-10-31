---
title: Definiowanie ról uprzywilejowanych w usłudze PAM | Dokumentacja firmy Microsoft
description: Istnieje możliwość określenia, które role uprzywilejowane mają być zarządzane, i zdefiniowania zasad zarządzania dla każdej z nich.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 38a9fc174c037e5d7c3ea17b46dcf9f6ea924822
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50380021"
---
# <a name="define-roles-for-privileged-access-management"></a>Definiowanie ról na potrzeby funkcji Privileged Access Management

Za pomocą funkcji Privileged Access Management (PAM) można przypisać użytkowników do uprzywilejowanych ról, które mogą oni aktywować w razie potrzeby dla dostępu just in time. Te role są definiowane ręcznie i ustanawiane w środowisku bastionu. W tym artykule przedstawiono proces podejmowania decyzji o tym, które role mają być zarządzane za pomocą funkcji Privileged Access Management, oraz sposób definiowania ich przy użyciu odpowiednich uprawnień i ograniczeń.

Najprostszym sposobem definiowania ról funkcji Privileged Access Management jest skompilowanie wszystkich informacji w arkuszu kalkulacyjnym. Utwórz listę ról i użyj kolumn do określenia wymagań i uprawnień ładu.

Wymagania ładu różnią się w zależności od istniejącej tożsamości i zasad dostępu lub wymagań dotyczących zgodności. Parametry, aby zidentyfikować dla każdej roli mogą być następujące:

- Właściciel roli.
- Użytkowników kandydujących, którzy mogą znajdować się w tej roli
- Formanty uwierzytelniania, zatwierdzania lub powiadamiania, które powinny być skojarzone z użyciem roli.

Uprawnienia roli są zależne od zarządzanych aplikacji. W tym artykule jako przykładowa aplikacja jest używana usługa Active Directory, w której uprawnienia dzielą się na dwie kategorie:

- Potrzebne do zarządzania samą usługą Active Directory (np. do konfigurowania topologii replikacji)

- Potrzebne do zarządzania danymi przechowywanymi w usłudze Active Directory (np. do tworzenia użytkowników i grup)

## <a name="identify-roles"></a>Identyfikowanie ról

Rozpocznij od zidentyfikowania wszystkich ról, które mają być zarządzane za pomocą funkcji PAM. W arkuszu kalkulacyjnym każda potencjalna rola będzie miała swój własny wiersz.

Aby znaleźć odpowiednie role, rozważ każdą aplikację w zakresie zarządzania:

- Aplikacja w [warstwy 0, 1 czy 2](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)?
- Jakie uprawnienia wpływają na poufność, integralność lub dostępność aplikacji?
- Czy aplikacja ma zależności od innych składników systemu Na przykład czy ma zależności od bazy danych, sieci, zabezpieczeń infrastruktury wirtualizacji lub hostingu platformy?

Określ sposób grupowania tych zagadnień dotyczących aplikacji. Wymagane są role, które mają wyraźne granice i dają tylko wystarczające uprawnienia do wykonywania typowych zadań administracyjnych w aplikacji.

Należy zawsze projektować role dla przypisania z najmniejszymi uprawnieniami. Może to być oparte na bieżących (lub planowanych) obowiązkach organizacyjnych dla użytkowników i obejmować uprawnienia wymagane ze względu na obowiązki użytkownika. Może to również obejmować uprawnienia, które upraszczają operacje bez zwiększania ryzyka.

Inne zagadnienia z zakresu uprawnień, które ma zawierać rola, są następujące:

- Jak wiele osób pracuje z określoną rolą? Jeśli jest to tylko jedna osoba, rola może być zbyt ściśle zdefiniowana, aby była przydatna, lub zdefiniowano określone obowiązki danej osoby.

- Jak wiele ról ma dana osoba? Czy użytkownicy będą mogli wybrać odpowiednią rolę dla swojego zadania?

- Czy populacja użytkowników i sposób ich interakcji z aplikacjami będzie zgodny z funkcją Privileged Access Management?

- Czy jest możliwe oddzielenie administracji i inspekcji, aby użytkownik z rolą administracyjną nie mógł wymazać rekordów inspekcji swoich działań?

## <a name="establish-role-governance-requirements"></a>Ustanawianie wymagań roli dotyczących ładu

Po zidentyfikowaniu ról kandydujących rozpocznij wypełnianie arkusza kalkulacyjnego. Utwórz kolumny dla wymagań istotnych dla Twojej organizacji. Niektóre wymagania, które należy wziąć pod uwagę, to:

- Kto jest właścicielem roli, który będzie odpowiedzialny za dalsze definiowanie roli, wybieranie uprawnień i obsługę ustawień ładu roli?

- Kim są posiadacze roli (użytkownicy), którzy będą wykonywać obowiązki lub zadania związane z rolą?

- Jaka metoda dostępu (omówiona w następnej sekcji) będzie odpowiednia dla posiadaczy roli?

- Czy jest wymagane ręczne zatwierdzanie przez właściciela roli, gdy użytkownik aktywuje swoją rolę?

- Czy jest wymagane powiadomienie, gdy użytkownik aktywuje swoją rolę?

- Czy użycie tej roli powinno spowodować wygenerowanie alertu lub powiadomienia w systemie SIEM na potrzeby śledzenia?

- Czy jest konieczne ograniczanie możliwości logowania użytkowników aktywujących rolę tylko do komputerów, na których jest wymagany dostęp w celu wykonywania obowiązków związanych z rolą oraz weryfikacja hosta jest wystarczająca do zabezpieczenia uprawnień lub poświadczeń przed ich nieprawidłowym użyciem?

- Czy wymagane jest zapewnienie osobom pełniącym rolę dedykowanej administracyjnej stacji roboczej?

- Które uprawnienia aplikacji (zobacz przykładową listę dla usługi AD poniżej) są skojarzone z tą rolą?

## <a name="select-an-access-method"></a>Wybieranie metody dostępu

Może istnieć wiele ról w systemie zarządzania dostępem uprzywilejowanym przy użyciu tych samych uprawnień przypisanych do nich. Może to nastąpić, jeśli różne społeczności użytkowników mają wymagania ładu dotyczące dostępu distinct. Na przykład organizacja może zastosować inne zasady dla swoich pełnoetatowych pracowników, a inne dla zewnętrznych pracowników IT z innej organizacji.

W niektórych przypadkach użytkownik może zostać trwale przypisany do roli. W takim przypadku nie ma potrzeby żądać lub aktywować przypisania roli. Przykłady scenariuszy stałych przypisań obejmują:

- Zarządzane konto usługi w istniejącym lesie.

- Konto użytkownika w istniejącym lesie z poświadczeniami zarządzanymi poza funkcją PAM. Może to być konto "break szkła". Konto awaryjne, że wymagane rolę takich jak "domeny / kontrolera domeny konserwacji" do rozwiązywania problemów, takich jak zaufania i kontroler domeny kondycji problemów. Jako konto awaryjne miałby roli trwale przypisane z fizycznie zabezpieczonym hasłem)

- Konto użytkownika w lesie administracyjnym, który jest uwierzytelniany przy użyciu hasła. Może to być, użytkownik musi mieć uprawnienia administracyjne w 24 x 7 trwałe i loguje się z urządzenia, które nie obsługuje silnego uwierzytelniania.

- Konto użytkownika w lesie administracyjnym z kartą inteligentną lub wirtualną kartą inteligentną (na przykład konto z kartą inteligentną w trybie offline potrzebną do rzadkich zadań konserwacji).

W przypadku organizacji niepokojących się o potencjalną kradzież poświadczeń lub ich nieuprawnione użycie, przewodnik [Using Azure MFA for activation](use-azure-mfa-for-activation.md) (Używanie usługi Azure MFA do aktywacji) zawiera instrukcje dotyczące sposobu konfigurowania programu MIM w taki sposób, aby wymagane było dodatkowe sprawdzanie poza pasmem w momencie aktywowania roli.

## <a name="delegate-active-directory-permissions"></a>Delegowanie uprawnień usługi Active Directory

System Windows Server podczas tworzenia nowych domen automatycznie tworzy grupy domyślne, takie jak „Administratorzy domeny”. Te grupy ułatwiają rozpoczęcie pracy i mogą być przydatne dla mniejszych organizacji. Większe organizacje lub organizacje, które wymagają większej izolacji uprawnień administracyjnych, powinny opróżnić grupy, te i zastąpić je grupami, które zapewniają szczegółowe uprawnienia.

Jednym z ograniczeń grupy Administratorzy domeny jest to, że jej członkami nie mogą być osoby z domeny zewnętrznej. Innym ograniczeniem jest to, że przyznaje ona uprawnienia do trzech osobnych funkcji:

- Zarządzanie samą usługą Active Directory
- Zarządzanie danymi przechowywanymi w usłudze Active Directory
- Umożliwianie zdalnego logowania do komputerów przyłączonych do domeny

Zamiast domyślnych grup, takich jak Administratorzy domeny należy utworzyć nowe grupy zabezpieczeń, które zapewniają tylko wymagane uprawnienia. Następnie należy użyć programu MIM do dynamicznego udostępniania kont administratorów z tym członkostwem w grupach.

### <a name="service-management-permissions"></a>Uprawnienia zarządzania usługami

W poniższej tabeli przedstawiono przykłady uprawnień, które warto dodać do ról służących do zarządzania usługą AD.

| Rola | Opis |
| ---- | ---- |
| Konserwacja domeny/kontrolera domeny | Członkostwo w grupie DOMENA\Administratorzy umożliwia rozwiązywania problemów i zmieniania systemu operacyjnego kontrolera domeny. Operacje, takie jak podwyższania poziomu nowego kontrolera domeny do istniejącej domeny w lesie oraz delegowania ról usługi AD.
|Zarządzanie wirtualnymi kontrolerami domeny | Zarządzanie maszynami wirtualnymi kontrolerów domeny za pomocą oprogramowania do zarządzania wirtualizacją. To uprawnienie można przyznać za pośrednictwem pełnej kontroli nad wszystkimi maszynami wirtualnymi w narzędziu do zarządzania lub funkcji kontroli dostępu na podstawie ról. |
| Rozszerzanie schematu | Zarządzanie schematem, w tym dodawanie nowych definicji obiektów, zmienianie uprawnień do obiektów schematu i zmienianie domyślnych uprawnień schematu dla typów obiektów. |
| Tworzenie kopii zapasowej bazy danych usługi Active Directory | Wykonywanie kopii zapasowej całej bazy danych usługi Active Directory, w tym wszystkich kluczy tajnych powierzonych kontrolerowi domeny i domenie. |
| Zarządzanie relacjami zaufania i poziomami funkcjonalności | Tworzenie i usuwanie relacji zaufania z zewnętrznymi lasami i domenami. |
| Zarządzanie lokacjami, podsieciami i replikacją | Zarządzanie obiektami topologii replikacji usługi Active Directory, w tym modyfikowanie lokacji i podsieci, oraz obiektami linków lokacji, a także inicjowanie operacji replikacji. |
| Zarządzanie obiektami zasad grupy | Tworzenie, usuwanie i modyfikowanie obiektów zasad grupy w domenie. |
| Zarządzanie strefami | Tworzenie, usuwanie i modyfikowanie stref DNS oraz obiektów w usłudze Active Directory. |
| Modyfikowanie jednostek organizacyjnych warstwy 0 | Modyfikowanie jednostek organizacyjnych warstwy 0 i obiektów znajdujących się w usłudze Active Directory. |

### <a name="data-management-permissions"></a>Uprawnienia zarządzania danymi

Poniższej tabeli przedstawiono przykłady uprawnień, które warto dodać do ról służących do zarządzania lub korzystanie z danych przechowywanych w usłudze AD.

| Rola | Opis |
| ---- | ---- |
| Modyfikowanie administracyjnej jednostki organizacyjnej warstwy 1                 | Modyfikowanie jednostek organizacyjnych zawierających obiekty administracyjne warstwy 1 w usłudze Active Directory. |
| Modyfikowanie administracyjnej jednostki organizacyjnej warstwy 2                 | Modyfikowanie jednostek organizacyjnych zawierających obiekty administracyjne warstwy 2 w usłudze Active Directory. |
| Zarządzanie kontami: tworzenie/usuwanie/przenoszenie | Modyfikowanie kont użytkowników standardowych.                                      |
| Zarządzanie kontami: resetowanie/odblokowywanie       | Resetowanie haseł i odblokowywanie kont.                                  |
| Grupa zabezpieczeń: tworzenie/modyfikowanie          | Tworzenie i modyfikowanie grup zabezpieczeń w usłudze Active Directory.              |
| Grupa zabezpieczeń: usuwanie                 | Usuwanie grup zabezpieczeń w usłudze Active Directory.                         |
| Zarządzanie obiektami zasad grupy                         | Zarządzanie wszystkimi obiektami zasad grupy w domenie lub lesie, które nie mają wpływu na serwery warstwy 0.             |
| Przyłączony komputer/administrator lokalny                    | Lokalne uprawnienia administracyjne do wszystkich stacji roboczych.                               |
| Przyłączony serwer/administrator lokalny                   | Lokalne uprawnienia administracyjne do wszystkich serwerów.                                    |

## <a name="example-role-definitions"></a>Przykładowe definicje ról

Wybór definicji ról zależy od warstwy serwerów zarządzanych. To również zależy od wyboru zarządzanych aplikacji. Aplikacje takie jak Exchange lub strona trzecia produktami w przedsiębiorstwie, takich jak SAP, często będą miały własne dodatkowe definicje ról na potrzeby administracji delegowanej.

W poniższych sekcjach znajdują się przykłady dla typowych scenariuszy przedsiębiorstwa.

### <a name="tier-0---administrative-forest"></a>Warstwa 0 — las administracyjny

Role odpowiednie dla kont w środowisku bastionu mogą być następujące:

- Dostęp awaryjny do lasu administracyjnego
- Administratorzy z czerwonymi kartami: użytkownicy, którzy są administratorami lasu administracyjnego
- Użytkownicy, którzy są administratorami lasu produkcyjnego
- Użytkownicy, którym zostały delegowane ograniczone prawa administracyjne do aplikacji w lesie produkcyjnym

### <a name="tier-0---enterprise-production-forest"></a>Warstwa 0 — las produkcyjny przedsiębiorstwa

Role odpowiednie do zarządzania kontami i zasobami lasu produkcyjnego warstwy 0 mogą być następujące:

- Dostęp awaryjny do lasu produkcyjnego
- Administratorzy zasad grupy
- Administratorzy serwera DNS
- Administratorzy infrastruktury PKI
- Administratorzy topologii i replikacji usługi AD
- Administratorzy wirtualizacji serwerów warstwy 0
- Administratorzy magazynu
- Administratorzy ochrony przed złośliwym oprogramowaniem dla serwerów warstwy 0
- Administratorzy programu SCCM dla warstwy 0
- Administratorzy programu SCOM dla warstwy 0
- Administratorzy kopii zapasowych dla warstwy 0
- Użytkownicy kontrolerów poza pasmem i kontrolerów zarządzania płytą główną (w przypadku maszyny KVM lub zarządzania lights-out) połączonych z hostami warstwy 0

### <a name="tier-1"></a>Warstwa 1

Role na potrzeby zarządzania serwerami i tworzenia ich kopii zapasowych w warstwie 1 mogą być następujące:

- Konserwacja serwera
- Administratorzy wirtualizacji dla serwerów warstwy 1
- Konto skanera zabezpieczeń
- Administratorzy ochrony przed złośliwym oprogramowaniem dla serwerów warstwy 1
- Administratorzy programu SCCM dla warstwy 1
- Administratorzy programu SCOM dla warstwy 1
- Administratorzy kopii zapasowych dla serwerów warstwy 1
- Użytkownicy kontrolerów poza pasmem i kontrolerów zarządzania płytą główną (w przypadku maszyny KVM lub zarządzania lights-out) połączonych z hostami warstwy 1

Ponadto role służące do zarządzania aplikacjami przedsiębiorstwa w warstwie 1 mogą być następujące:

- Administratorzy DHCP
- Administratorzy programu Exchange
- Administratorzy programu Skype dla firm
- Administratorzy farmy programu SharePoint
- Administratorzy usługi w chmurze, np. witryny sieci Web firmy lub publicznego serwera DNS
- Administratorzy systemów HCM, finansowych lub prawnych

### <a name="tier-2"></a>Warstwa 2

Role dla użytkowników innych niż użytkownicy administracyjni i role na potrzeby zarządzania komputerami mogą być następujące:

- Administratorzy kont
- Pomoc techniczna
- Administratorzy grup zabezpieczeń
- Pomoc techniczna dla stacji roboczych na miejscu

## <a name="next-steps"></a>Kolejne kroki

- [Zabezpieczanie dostępu uprzywilejowanego — materiały referencyjne](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Używanie usługi Azure MFA do aktywacji](use-azure-mfa-for-activation.md)
