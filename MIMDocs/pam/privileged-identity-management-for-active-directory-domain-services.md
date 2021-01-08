---
title: Usługa Privileged Access Management dla usług domenowych Active Directory | Dokumentacja firmy Microsoft
description: Poznaj usługę Privileged Access Management i dowiedz się, jak możesz zarządzać środowiskiem usługi Active Directory i chronić je.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
experimental: true
experiment_id: kgremban_images
ms.openlocfilehash: 351a516ccb6a529ca27b157508b06af46f3d243a
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010731"
---
# <a name="privileged-access-management-for-active-directory-domain-services"></a>Usługa Privileged Access Management dla usług domenowych Active Directory

MIM Privileged Access Management (PAM) to rozwiązanie, które ułatwia organizacjom ograniczenie dostępu uprzywilejowanego w istniejącym i izolowanym środowisku Active Directory.

Usługa Privileged Access Management umożliwia zrealizowanie dwóch celów:

- Odzyskanie kontroli nad środowiskiem usługi Active Directory z naruszonymi zabezpieczeniami dzięki utrzymywaniu oddzielnego środowiska bastionu, o którym wiadomo, że jest poza zasięgiem złośliwych ataków.
- Odizolowanie uprzywilejowanych kont w celu ograniczenia ryzyka kradzieży poświadczeń.

> [!NOTE]
> Moduł PAM programu MIM jest różny od [Azure Active Directory Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM). Moduł PAM programu MIM jest przeznaczony dla izolowanych lokalnych środowisk usługi AD. Azure AD PIM to usługa w usłudze Azure AD, która umożliwia zarządzanie, kontrolowanie i monitorowanie dostępu do zasobów w usłudze Azure AD, na platformie Azure oraz w innych usługach online firmy Microsoft, takich jak Microsoft 365 lub Microsoft Intune. Aby uzyskać wskazówki dotyczące lokalnych środowisk podłączonych do Internetu i środowisk hybrydowych, zobacz [Zabezpieczanie uprzywilejowanego dostępu](/security/compass/overview) Aby uzyskać więcej informacji.

## <a name="what-problems-does-mim-pam-help-solve"></a>Jakie problemy rozwiązuje usługa PAM programu MIM?

Obecnie, osoby atakujące mogą uzyskać poświadczenia konta administratorów domeny i nie mogą wykryć tych ataków po tym fakcie. Celem usługi PAM jest zmniejszenie możliwości uzyskania dostępu przez złośliwych użytkowników przy jednoczesnym zapewnieniu administratorom lepszego rozeznania i większej kontroli nad środowiskiem.

Usługa PAM utrudnia osobom atakującym spenetrowanie sieci i uzyskanie dostępu do uprzywilejowanych kont. Zapewnia ona ochronę uprzywilejowanym grupom, które kontrolują dostęp do komputerów dołączonych do domeny i aplikacji zainstalowanych na tych komputerach. Dodatkowo zwiększa to monitorowanie, większą widoczność i bardziej precyzyjną kontrolę. Dzięki temu organizacje mogą zobaczyć, którzy Administratorzy uprzywilejowani są i co robią. Usługa PAM pozwala organizacjom uzyskać lepszy wgląd w używanie kont administracyjnych w środowisku.

Podejście PAM udostępnione przez program MIM jest przeznaczone do użycia w niestandardowej architekturze dla izolowanych środowisk, w których dostęp do Internetu nie jest dostępny, gdy ta konfiguracja jest wymagana przez regulację lub w środowiskach izolowanych o dużym wpływie, takich jak laboratoria badawcze w trybie offline i rozłączona technologia kontroli i środowiska pozyskiwania danych. Jeśli Active Directory jest częścią środowiska połączonego z Internetem, zapoznaj się z tematem [Zabezpieczanie uprzywilejowanego dostępu](/security/compass/overview) , aby uzyskać więcej informacji na temat lokalizacji do uruchomienia.

## <a name="setting-up-mim-pam"></a>Konfigurowanie modułu PAM programu MIM

Usługa PAM korzysta z zasady administrowania w miarę potrzeb, która jest powiązana z zasadą [wystarczających uprawnień administracyjnych (Just Enough Administration, JEA)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). JEA to zestaw narzędzi środowiska Windows PowerShell, który definiuje zestaw poleceń do wykonywania uprzywilejowanych działań. Jest punktem końcowym, w którym Administratorzy mogą uzyskać autoryzację do uruchamiania poleceń. W ramach zasad JEA to administrator decyduje, jakie uprawnienia są wymagane do wykonywania danego zadania. Za każdym razem, gdy zachodzi potrzeba wykonania tego zadania przez kwalifikującego się użytkownika, jest włączane odpowiednie uprawnienie. Uprawnienia wygasają po upływie określonego czasu. Uniemożliwia to złośliwym użytkownikom przechwycenie uprawnień dostępu.

Konfiguracja i działanie usługi PAM obejmuje cztery kroki.

![Diagram przedstawiający funkcjonowanie usługi PAM: przygotowywanie, ochrona, działanie, monitorowanie](media/MIM_PIM_SetupProcess.png)

1. **Przygotowywanie**: określenie, które grupy w istniejącym lesie mają znaczące uprawnienia. Grupy te są ponownie tworzone w lesie bastionu, ale bez członków.
2. **Ochrona**: skonfiguruj Cykl życia i ochronę przed uwierzytelnianiem w przypadku, gdy użytkownicy żądają administracji just-in-Time. 
3. **Działanie**: gdy wymagania dotyczące uwierzytelniania zostaną spełnione, a żądanie zatwierdzone, konto użytkownika zostanie tymczasowo dodane do uprzywilejowanej grupy w lesie bastionu. Administrator może wtedy korzystać ze wszystkich uprawnień przypisanych do tej grupy, które są ważne przez wstępnie określony czas. Po upływie tego czasu konto zostanie usunięte z grupy.
4. **Monitorowanie**: usługa PAM udostępnia inspekcje, alerty i raporty dotyczące żądań uprzywilejowanego dostępu. Można wyświetlić historię uprzywilejowanego dostępu i sprawdzić, kto wykonał dane działanie, co pozwala określić, czy jest ono prawidłowe. Można też łatwo zidentyfikować nieautoryzowane działania, na przykład próbę bezpośredniego dodania użytkownika do uprzywilejowanej grupy w oryginalnym lesie. Ten krok umożliwia nie tylko identyfikację złośliwego oprogramowania, ale także śledzenie ataków „od wewnątrz”.

## <a name="how-does-mim-pam-work"></a>Jak działa usługa PAM programu MIM?

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

Dowiedz się więcej na temat [poleceń cmdlet funkcji Privileged Access Management](https://docs.microsoft.com/powershell/identitymanager/mimpam/vlatest/mimpam).

## <a name="what-workflows-and-monitoring-options-are-available"></a>Jakie przepływy pracy i opcje monitorowania są dostępne?

Załóżmy na przykład, że użytkownik był członkiem grupy administracyjnej przed skonfigurowaniem modułu PAM. W ramach konfiguracji usługi PAM użytkownik jest usuwany z grupy administracyjnej, a zasady są tworzone w programie MIM. Zasady te określają, że jeśli ten użytkownik zażąda uprawnień administracyjnych, żądanie zostanie zatwierdzone, a osobne konto użytkownika zostanie dodane do uprzywilejowanej grupy w lesie bastionu.

Po zatwierdzeniu żądania przepływ pracy akcji komunikuje się bezpośrednio z usługą Active Directory w lesie bastionu w celu dodania użytkownika do grupy. Na przykład gdy użytkownik Jen wyśle żądanie dotyczące administrowania bazy danych działu HR, w ciągu kilku sekund zostanie dodane konto administracyjne dla tego użytkownika do uprzywilejowanej grupy w lesie bastionu. Członkostwo konta administratora w tej grupie wygaśnie po upływie limitu czasu. W systemie Windows Server 2016 lub nowszym członkostwo jest skojarzone w Active Directory z limitem czasu.

> [!NOTE]
> Po dodaniu nowego członka do grupy zmiana musi zostać zreplikowana na innych kontrolerach domen (DC) w lesie bastionu. Opóźnienie replikacji może wpływać na dostęp użytkowników do zasobów. Więcej informacji o opóźnieniu replikacji można znaleźć w artykule [How Active Directory Replication Topology Works](https://technet.microsoft.com/library/cc755994.aspx) (Jak działa topologia replikacji usługi Active Directory).
>
> Z kolei wygasłe łącza są oceniane w czasie rzeczywistym przez Menedżera kont zabezpieczeń (Security Accounts Manager, SAM). Mimo że dodanie członka do grupy musi zostać zreplikowane na kontrolerze domeny, który odebrał żądanie dostępu, usunięcie członka z grupy jest oceniane natychmiast na dowolnym kontrolerze domeny.

Taki przepływ pracy jest przeznaczony specjalnie dla kont administracyjnych. Administratorzy, którzy rzadko potrzebują dostępu do uprzywilejowanych grup, mogą wysyłać precyzyjne żądania dostępu. To samo dotyczy skryptów. Program MIM rejestruje żądanie i zmiany w usłudze Active Directory. Można je wyświetlić w Podglądzie zdarzeń lub wysłać dane do rozwiązań służących do monitorowania infrastruktury przedsiębiorstwa, na przykład usług Audit Collection Services (ACS) programu System Center 2012 — Operations Manager lub narzędzi innych firm.

## <a name="next-steps"></a>Następne kroki

- [Strategia uprzywilejowanego dostępu](https://docs.microsoft.com/security/compass/privileged-access-strategy)
- [Polecenia cmdlet usługi Privileged Access Management](https://docs.microsoft.com/powershell/identitymanager/mimpam/vlatest/mimpam)
