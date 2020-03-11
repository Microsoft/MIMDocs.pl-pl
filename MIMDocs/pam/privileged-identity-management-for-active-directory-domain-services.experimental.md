---
title: Co to jest usługa PAM dla usług ADDS? | Dokumentacja firmy Microsoft
description: Usługa Privileged Access Management (PAM) ułatwia organizacjom ograniczenie dostępu uprzywilejowanego w istniejącym środowisku usługi Active Directory.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 03/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
experiment_id: kgremban_images
ms.openlocfilehash: acec45a2843febd3821d9045336098cdeb4ddcf7
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043940"
---
# <a name="privileged-access-management-for-active-directory-domain-services"></a>Usługa Privileged Access Management dla usług Active Directory Domain Services

Usługa Privileged Access Management (PAM) ułatwia organizacjom ograniczenie dostępu uprzywilejowanego w istniejącym środowisku usługi Active Directory.

![Diagram przedstawiający funkcjonowanie usługi PAM: przygotowywanie, ochrona, działanie, monitorowanie](media/MIM_PIM_SetupProcess.png)

W usłudze Privileged Access Management skoncentrowano się na cyklu przygotowywania, ochrony i monitorowania środowiska, dzięki czemu umożliwia ona zrealizowanie dwóch celów:

- Odzyskanie kontroli nad środowiskiem usługi Active Directory z naruszonymi zabezpieczeniami dzięki utrzymywaniu oddzielnego środowiska bastionu, o którym wiadomo, że jest poza zasięgiem złośliwych ataków.  
- Odizolowanie uprzywilejowanych kont w celu ograniczenia ryzyka kradzieży poświadczeń.

> [!NOTE]
> Usługa PAM jest wystąpieniem usługi [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) zaimplementowanym przy użyciu programu Microsoft Identity Manager (MIM).

## <a name="what-problems-does-pam-help-solve"></a>Jakie problemy rozwiązuje usługa PAM?

Obecnie jednym z priorytetów przedsiębiorstw jest dostęp do zasobów w środowisku usługi Active Directory. Szczególnie niepokojące są informacje na temat luk w zabezpieczeniach oraz nieautoryzowanych podwyższeń poziomu uprawnień, a także innych rodzajów nieautoryzowanego dostępu, na przykład atakach typu Pass-the-Hash, Pass-the-Ticket i spear phishing oraz naruszeniach zabezpieczeń protokołu Kerberos.

Niestety uzyskanie przez hakerów poświadczeń dla konta administratora domeny jest obecnie bardzo łatwe, a wykrycie takiego ataku — trudne. Celem usługi PAM jest zmniejszenie możliwości uzyskania dostępu przez złośliwych użytkowników przy jednoczesnym zapewnieniu administratorom lepszego rozeznania i większej kontroli nad środowiskiem.

Usługa PAM utrudnia osobom atakującym spenetrowanie sieci i uzyskanie dostępu do uprzywilejowanych kont. Zapewnia ona ochronę uprzywilejowanym grupom, które kontrolują dostęp do komputerów dołączonych do domeny i aplikacji zainstalowanych na tych komputerach. Zwiększono również zakres monitorowania i widoczności oraz dodano bardziej szczegółowe mechanizmy kontroli. Dzięki temu organizacje wiedzą, kim są uprzywilejowani administratorzy i jakie czynności wykonują. Usługa PAM pozwala organizacjom uzyskać lepszy wgląd w używanie kont administracyjnych w środowisku.

## <a name="how-is-pam-set-up"></a>Konfiguracja usługi PAM

Usługa PAM korzysta z zasady administrowania w miarę potrzeb, która jest powiązana z zasadą [wystarczających uprawnień administracyjnych (Just Enough Administration, JEA)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). Technologia JEA jest zawarta w zestawie narzędzi programu Windows PowerShell, który udostępnia zestaw poleceń służących do wykonywania uprzywilejowanych działań oraz punkt końcowy, w którym administratorzy uzyskują autoryzację do uruchamiania tych poleceń. W ramach zasad JEA to administrator decyduje, jakie uprawnienia są wymagane do wykonywania danego zadania. Za każdym razem, gdy zachodzi potrzeba wykonania tego zadania przez kwalifikującego się użytkownika, jest włączane odpowiednie uprawnienie. Uprawnienia wygasają po upływie określonego czasu. Uniemożliwia to złośliwym użytkownikom przechwycenie uprawnień dostępu.

Konfiguracja i działanie usługi PAM obejmuje cztery kroki.

1. **Przygotowywanie**: określenie, które grupy w istniejącym lesie mają znaczące uprawnienia. Grupy te są ponownie tworzone w lesie bastionu, ale bez członków.

2. **Ochrona**: skonfigurowanie ochrony cyklu życia i uwierzytelniania, na przykład uwierzytelniania wieloskładnikowego (Multi-Factor Authentication, MFA) na potrzeby obsługi żądań użytkowników dotyczących administrowania w miarę potrzeb. Usługa MFA pomaga zapobiegać programowym atakom ze strony złośliwego oprogramowania lub będącym efektem kradzieży poświadczeń.

3. **Działanie**: gdy wymagania dotyczące uwierzytelniania zostaną spełnione, a żądanie zatwierdzone, konto użytkownika zostanie tymczasowo dodane do uprzywilejowanej grupy w lesie bastionu. Administrator może wtedy korzystać ze wszystkich uprawnień przypisanych do tej grupy, które są ważne przez wstępnie określony czas. Po upływie tego czasu konto zostanie usunięte z grupy.

4. **Monitorowanie**: usługa PAM udostępnia inspekcje, alerty i raporty dotyczące żądań uprzywilejowanego dostępu. Można wyświetlić historię uprzywilejowanego dostępu i sprawdzić, kto wykonał dane działanie, co pozwala określić, czy jest ono prawidłowe. Można też łatwo zidentyfikować nieautoryzowane działania, na przykład próbę bezpośredniego dodania użytkownika do uprzywilejowanej grupy w oryginalnym lesie. Ten krok umożliwia nie tylko identyfikację złośliwego oprogramowania, ale także śledzenie ataków „od wewnątrz”.

## <a name="how-does-pam-work"></a>Jak działa usługa PAM

Usługa PAM korzysta z nowych funkcji usług AD DS, szczególnie tych dotyczących uwierzytelniania i autoryzacji kont domen, oraz nowych możliwości oferowanych przez program Microsoft Identity Manager. Uprzywilejowane konta są oddzielane od istniejącego środowiska usługi Active Directory. W razie potrzeby użycia uprzywilejowanego należy wysłać żądanie, które musi zostać zatwierdzone. Po zatwierdzeniu żądania uprzywilejowane konto uzyskuje za pośrednictwem grupy obcego podmiotu zabezpieczeń uprawnienia w nowym lesie bastionu, a nie w bieżącym lesie użytkownika lub aplikacji. Użycie lasu bastionu zapewnia organizacji lepszą kontrolę i pozwala na przykład określić metodę uwierzytelniania użytkownika czy warunki jego przynależności do uprzywilejowanej grupy.

Poszczególne elementy tego rozwiązania — takie jak usługa Active Directory czy MIM — można również wdrożyć w konfiguracji o wysokiej dostępności.

W poniższym przykładzie bardziej szczegółowo pokazano działanie usługi PIM.

![Diagram przedstawiający procesy i uczestników usługi PIM](media/MIM_PIM_howitworks.png)

W lesie bastionu są tworzone ograniczone czasowo członkostwa grup, które z kolei umożliwiają utworzenie ograniczonych czasowo biletów uprawniających do przyznania biletów (ticket-granting tickets, TGT). Aplikacje i usługi korzystające z protokołu Kerberos i należące do lasów, które ufają lasowi bastionu, mogą uznawać te bilety TGT i wymuszać ich stosowanie.

Typowych kont użytkowników nie trzeba przenosić do nowego lasu. To samo dotyczy komputerów, aplikacji i ich grup. Pozostają one w lasach, do których aktualnie należą. Rozważmy przykładową organizację, której dotyczą obecne zagrożenia bezpieczeństwa cyfrowego, ale nie planuje ona w najbliższej przyszłości uaktualnienia infrastruktury do następnej wersji systemu Windows Server. Ta organizacja może użyć programu MIM i nowego lasu bastionu w ramach korzystania z połączonego rozwiązania. Pozwoli to lepiej kontrolować dostęp do istniejących zasobów.

Użycie usługi PAM zapewnia następujące korzyści:

- **Izolacja/określanie zakresu uprawnień**: uprawnienia nie są przechowywane na kontach, które są używane również do wykonywania zadań niewymagających uprzywilejowanego dostępu, takich jak sprawdzanie poczty e-mail czy przeglądanie Internetu. Użytkownicy muszą zażądać uprawnień. Żądania są zatwierdzane lub odrzucane na podstawie zasad programu MIM zdefiniowanych przez administratora usługi PAM. Dopóki żądanie nie zostanie zatwierdzone, uprzywilejowany dostęp nie jest możliwy.

- **Podniesienie i weryfikacja uprawnień**: nowe funkcje uwierzytelniania i autoryzacji ułatwiają zarządzanie cyklem życia oddzielnych kont administracyjnych. Żądanie użytkownika, który chce podnieść uprawnienia dla konta administracyjnego, przechodzi przez przepływy pracy programu MIM.

- **Dodatkowe rejestrowanie**: oprócz wbudowanych przepływów pracy programu MIM usługa PAM udostępnia dodatkowe opcje rejestrowania obejmujące identyfikowanie i autoryzację żądań oraz obsługę zdarzeń, które wystąpiły po zatwierdzeniu żądania.

- **Możliwość dostosowania przepływu pracy**: można używać wielu przepływów pracy programu MIM i konfigurować je pod kątem różnych scenariuszy na podstawie parametrów użytkowników wysyłających żądania albo żądanych ról.

## <a name="how-do-users-request-privileged-access"></a>Sposoby wysyłania żądań uprzywilejowanego dostępu

Poniżej podano kilka sposobów umożliwiających wysłanie takiego żądania przez użytkownika:  

- Interfejs API usług sieci Web dla usług programu MIM  
- Punkt końcowy REST  
- Program Windows PowerShell (`New-PAMRequest`)

## <a name="what-workflows-and-monitoring-options-are-available"></a>Jakie przepływy pracy i opcje monitorowania są dostępne?

Załóżmy na przykład, że przed skonfigurowaniem usługi PIM dany użytkownik był członkiem grupy administratorów. W ramach konfiguracji usługi PIM ten użytkownik zostaje usunięty z grupy administratorów, a w programie MIM zostają utworzone zasady. Zasady określają, że jeśli ten użytkownik zażąda uprawnień administracyjnych i zostanie uwierzytelniony przez usługę MFA, żądanie zostanie zatwierdzone i do uprzywilejowanej grupy w lesie bastionu zostanie dodane oddzielne konto tego użytkownika.

Po zatwierdzeniu żądania przepływ pracy akcji komunikuje się bezpośrednio z usługą Active Directory w lesie bastionu w celu dodania użytkownika do grupy. Na przykład gdy użytkownik Jen wyśle żądanie dotyczące administrowania bazy danych działu HR, w ciągu kilku sekund zostanie dodane konto administracyjne dla tego użytkownika do uprzywilejowanej grupy w lesie bastionu. Członkostwo tego konta w grupie wygaśnie po określonym czasie. W systemie Windows Server Technical Preview członkostwo to jest skojarzone z limitem czasu w usłudze Active Directory. W systemie Windows Server 2012 R2 jest ono powiązane z lasem bastionu, a limit czasu jest wymuszany przez program MIM.

> [!NOTE]
> Po dodaniu nowego członka do grupy zmiana musi zostać zreplikowana na innych kontrolerach domen (DC) w lesie bastionu. Opóźnienie replikacji może wpływać na dostęp użytkowników do zasobów. Więcej informacji o opóźnieniu replikacji można znaleźć w artykule [How Active Directory Replication Topology Works](https://technet.microsoft.com/library/cc755994.aspx) (Jak działa topologia replikacji usługi Active Directory).
> Z kolei wygasłe łącza są oceniane w czasie rzeczywistym przez Menedżera kont zabezpieczeń (Security Accounts Manager, SAM). Mimo że dodanie członka do grupy musi zostać zreplikowane na kontrolerze domeny, który odebrał żądanie dostępu, usunięcie członka z grupy jest oceniane natychmiast na dowolnym kontrolerze domeny.

Taki przepływ pracy jest przeznaczony specjalnie dla kont administracyjnych. Administratorzy, którzy rzadko potrzebują dostępu do uprzywilejowanych grup, mogą wysyłać precyzyjne żądania dostępu. To samo dotyczy skryptów. Program MIM rejestruje żądanie i zmiany w usłudze Active Directory. Można je wyświetlić w Podglądzie zdarzeń lub wysłać dane do rozwiązań służących do monitorowania infrastruktury przedsiębiorstwa, na przykład usług Audit Collection Services (ACS) programu System Center 2012 — Operations Manager lub narzędzi innych firm.
