---
title: Przewodnik dotyczący pojęć związanych z pakietem Microsoft pakietu BHOLD Suite | Microsoft Docs
description: Rozpoczęcie pracy ze składnikami programu MIM 2016 przez instalację i konfigurację usługi synchronizacji.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: f120709e517d82d4f94e72f4d0a44361f5552a1c
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042308"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Przewodnik dotyczący pojęć związanych z pakietem Microsoft pakietu BHOLD Suite

Microsoft Identity Manager 2016 (MIM) umożliwia organizacjom zarządzanie całym cyklem życia tożsamości użytkowników i ich skojarzonymi poświadczeniami. Można ją skonfigurować tak, aby synchronizować tożsamości, centralnie zarządzać certyfikatami i hasłami oraz dostarczać użytkowników w systemach heterogenicznych. Dzięki programowi MIM organizacje IT mogą definiować i automatyzować procesy służące do zarządzania tożsamościami od tworzenia do wycofania.

Pakiet Microsoft pakietu BHOLD Suite rozszerza te możliwości programu MIM przez dodanie kontroli dostępu opartej na rolach. PAKIETU BHOLD umożliwia organizacjom definiowanie ról użytkowników oraz kontrolowanie dostępu do poufnych danych i aplikacji. Dostęp jest oparty na tym, co jest odpowiednie dla tych ról. Pakiet pakietu BHOLD zawiera usługi i narzędzia, które upraszczają modelowanie relacji roli w organizacji. PAKIETU BHOLD mapuje te role na prawa i aby sprawdzić, czy definicje ról i skojarzone prawa są poprawnie stosowane do użytkowników. Te funkcje są w pełni zintegrowane z programem MIM, zapewniając bezproblemowe środowisko dla użytkowników końcowych i INFORMATYKów.

Ten przewodnik pomaga zrozumieć, w jaki sposób pakiet pakietu BHOLD współpracuje z programem MIM i obejmuje następujące tematy:

- Kontrolę dostępu opartą na rolach.
- Zaświadczanie
- Analiza
- Raportowanie
- Łącznik zarządzania dostępem
- Integracja z programem MIM

## <a name="role-based-access-control"></a>Kontrolę dostępu opartą na rolach.

Najbardziej Typowa metoda kontrolowania dostępu użytkowników do danych i aplikacji odbywa się za pomocą poufnej kontroli dostępu (DAC). W większości typowych implementacji każdy znaczący obiekt ma zidentyfikowany właściciel. Właściciel ma możliwość udzielenia lub odmowy dostępu do obiektu innym osobom opartym na poszczególnych tożsamościach lub członkostwie w grupie. W tym przypadku DAC zwykle mnóstwo grupy zabezpieczeń, niektóre odzwierciedlające strukturę organizacyjną, inne reprezentujące funkcjonalne grupowania (takie jak typy zadań lub przypisania projektu), a także inne, które składają się z kolekcji Makeshift użytkowników i urządzenia, które są połączone na potrzeby bardziej tymczasowych celów. W miarę rozwoju organizacji członkostwo w tych grupach stają się coraz trudniejsze do zarządzania. Na przykład, jeśli pracownik jest przenoszony z jednego projektu do innego, grupy, które są używane do kontrolowania dostępu do zasobów projektów, muszą być aktualizowane ręcznie. W takich przypadkach nie jest zdarza się występowania błędów, co może utrudnić bezpieczeństwo projektu lub produktywność.

Program MIM zawiera funkcje, które pomagają ograniczyć ten problem, zapewniając automatyczne sterowanie członkostwem w grupie i liście dystrybucyjnej. Nie dotyczy to jednak wewnętrznej złożoności rozbudowanych grup, które nie muszą być ze sobą powiązane.

Jednym ze sposobów znacznego zmniejszenia tego rozprzestrzenienia jest wdrożenie kontroli dostępu opartej na rolach (RBAC). RBAC nie rozmieszczenie DAC.  Kontrola RBAC kompiluje na DAC, dostarczając strukturę do klasyfikowania użytkowników i zasobów IT. Dzięki temu można jawnie wprowadzić swoje relacje i prawa dostępu, które są odpowiednie zgodnie z tą klasyfikacją. Na przykład, przypisując do atrybutów użytkownika, które określają tytuł zadania użytkowników i przypisania projektu, użytkownik może uzyskać dostęp do narzędzi niezbędnych dla zadania użytkownika i danych, które użytkownik musi współtworzyć w określonym projekcie. Gdy użytkownik zaleci inne zadanie i różne przypisania projektu, zmiana atrybutów określających stanowisko użytkownika i projekty automatycznie blokuje dostęp tylko do zasobów wymaganych przez poprzednią pozycję Użytkownicy.

Ponieważ role mogą być zawarte w rolach hierarchicznie, (na przykład role kierownika ds. sprzedaży i przedstawiciel handlowy mogą znajdować się w bardziej ogólnym zakresie sprzedaży), można łatwo przypisywać odpowiednie prawa do określonych ról, a jeszcze w dalszym ciągu odpowiednie prawa do wszystkich osób, które współużytkują tę rolę bardziej włącznie. Na przykład w szpitalu Wszyscy Pracownicy medyczni mogą uzyskać prawo do wyświetlania rekordów pacjentów, ale tylko lekarzy liczy (podrolą roli medycznej) mogą mieć prawo do wprowadzania postanowień dotyczących pacjenta. Podobnie w przypadku użytkowników należących do roli biurowej można odmówić dostępu do rekordów pacjenta, z wyjątkiem urzędników rozliczeń (podrolą roli biurowej), którzy mogli udzielić dostępu do tych części rekordów pacjentów, które są wymagane do rozliczenia pacjenta dla usług dostarczone przez szpital.

Dodatkową zaletą RBAC jest możliwość definiowania i wymuszania rozdzielania obowiązków (SoD). Dzięki temu organizacja może definiować kombinacje ról, które przyznają uprawnienia, które nie powinny być przechowywane przez tego samego użytkownika, tak aby określony użytkownik nie mógł mieć przypisanych ról, które umożliwiają użytkownikowi Zainicjowanie płatności i autoryzację płatności na przykład. Funkcja RBAC umożliwia automatyczne wymuszanie takich zasad, a nie testowanie efektywnej implementacji zasad dla poszczególnych użytkowników.

### <a name="bhold-role-model-objects"></a>Obiekty modelu roli pakietu BHOLD

Dzięki pakietowi pakietu BHOLD można określić i zorganizować role w organizacji, zamapować użytkowników na role i zmapować odpowiednie uprawnienia do ról. Ta struktura jest nazywana modelem roli i zawiera i łączy pięć typów obiektów: 

- Jednostki organizacyjne
- Użytkownicy
- Role
- Uprawnienia
- Aplikacje

#### <a name="organizational-units"></a>Jednostki organizacyjne

Jednostki organizacyjne (OrgUnits) są podmiotem zabezpieczeń, za pomocą którego użytkownicy są zorganizowane w modelu roli pakietu BHOLD. Każdy użytkownik musi należeć do co najmniej jednego OrgUnit. (W przypadku, gdy użytkownik zostanie usunięty z ostatniej jednostki organizacyjnej w pakietu BHOLD, rekord danych użytkownika zostanie usunięty z bazy danych pakietu BHOLD).

> [!Important]
> Jednostki organizacyjne w modelu roli pakietu BHOLD nie należy mylić z jednostkami organizacyjnymi w Active Directory Domain Services (AD DS). Zazwyczaj struktura jednostki organizacyjnej w programie pakietu BHOLD opiera się na organizacji i zasadach firmy, a nie w wymaganiach dotyczących infrastruktury sieciowej.

Chociaż nie jest to wymagane, w większości przypadków jednostki organizacyjne są uporządkowane w pakietu BHOLD do reprezentowania hierarchicznej struktury rzeczywistej organizacji, podobnej do przedstawionej poniżej:

![](media/bhold-concepts-guide/org-chart.png)

W tym przykładzie model roli organizationalganizatinal jednostkę dla firmy jako całość (reprezentowana przez prezesa, ponieważ prezes nie jest częścią jednostki mororganizationalganizatinal) lub głównej jednostki organizacyjnej pakietu BHOLD (która zawsze istnieje) może być używany w tym celu. OrgUnits reprezentujący działy firmy, które są umieszczone przez wiceprezesów, zostałyby przekazane do firmowej jednostki organizacyjnej. Następnie jednostki organizacyjne odpowiadające podmiotom gospodarczym marketingu i sprzedaży byłyby dodawane do jednostek organizacyjnych Marketing i sprzedaż, a jednostki organizacyjne reprezentujące regionalnych menedżerów sprzedaży byłyby umieszczane w jednostce organizacyjnej dla Kierownik ds. sprzedaży w regionie Wschodniej. Jednostki organizacyjne, które nie zarządzają innymi użytkownikami, powinny należeć do działu organizacji w menedżerze sprzedaży wschodnie regiony. Należy pamiętać, że użytkownicy mogą być członkami jednostki organizacyjnej na dowolnym poziomie. Na przykład asystent administracyjny, który nie jest kierownikiem i raportami bezpośrednio dla wiceprezesa, będzie członkiem jednostki organizacyjnej wiceprezesa.

Oprócz reprezentowania struktury organizacyjnej jednostki organizacyjne mogą być również używane do grupowania użytkowników i innych jednostek organizacyjnych zgodnie z kryteriami funkcjonalnymi, takimi jak w przypadku projektów lub specjalizacji. Na poniższym diagramie pokazano, w jaki sposób jednostki organizacyjne byłyby używane do grupowania partnerów sprzedaży w zależności od typu klienta:

![](media/bhold-concepts-guide/org-chart-02.png)

W tym przykładzie każdy podmiot sprzedaży będzie należał do dwóch jednostek organizacyjnych: jeden reprezentuje miejsce skojarzenia w strukturze zarządzania organizacji, a drugi reprezentuje bazę klienta (detaliczną lub firmową). Do każdej jednostki organizacyjnej można przypisać różne role, które z kolei mogą mieć przypisane różne uprawnienia dostępu do zasobów IT organizacji. Ponadto role można dziedziczyć z nadrzędnych jednostek organizacyjnych, upraszczając proces propagowania ról do użytkowników. Z drugiej strony można zapobiegać dziedziczeniu określonych ról, upewniając się, że określona rola jest skojarzona tylko z odpowiednimi jednostkami organizacyjnymi.

OrgUnits można utworzyć w pakietu BHOLD Suite przy użyciu głównego portalu sieci Web pakietu BHOLD lub przy użyciu generatora modelu pakietu BHOLD.

#### <a name="users"></a>Użytkownicy

Jak wspomniano powyżej, każdy użytkownik musi należeć do co najmniej jednej jednostki organizacyjnej (OrgUnit). Ponieważ jednostki organizacyjne są głównym mechanizmem kojarzenia użytkownika z rolami, w większości organizacji dany użytkownik należy do wielu OrgUnits, aby ułatwić skojarzenie ról z tym użytkownikiem. Jednak w niektórych przypadkach może być konieczne skojarzenie roli z użytkownikiem niezależnie od dowolnych OrgUnits, do których należy użytkownik. W związku z tym użytkownik może być przypisany bezpośrednio do roli, a także do uzyskiwania ról z OrgUnits, do którego należy użytkownik.

Jeśli użytkownik nie jest aktywny w organizacji (na przykład w przypadku urlopu medycznego), użytkownik może zostać zawieszony, który odwołuje wszystkie uprawnienia użytkownika bez usuwania użytkownika z modelu roli. Po powrocie do cła można ponownie aktywować użytkownika, co spowoduje przywrócenie wszystkich uprawnień udzielonych przez role użytkownika.

Obiekty dla użytkowników mogą być tworzone pojedynczo w pakietu BHOLD za pośrednictwem portalu sieci Web pakietu BHOLD Core lub można je zaimportować zbiorczo przy użyciu generatora modelu pakietu BHOLD lub przy użyciu łącznika zarządzania dostępem z usługą synchronizacji FIM w celu zaimportowania informacji o użytkowniku z takie źródła jako Active Directory Domain Services lub aplikacje kadrowe.

Użytkowników można tworzyć bezpośrednio w usłudze pakietu BHOLD bez używania usługi synchronizacji FIM. Może to być przydatne w przypadku modelowania ról w środowisku przedprodukcyjnym lub testowym. Można również zezwolić użytkownikom zewnętrznym (takim jak pracownikom podwykonawców) na przypisywanie ról, a tym samym dostęp do zasobów IT bez dodawania do bazy danych pracownika; jednak Ci użytkownicy nie będą mogli korzystać z funkcji samoobsługowych pakietu BHOLD.

#### <a name="roles"></a>Role

Jak wspomniano wcześniej, w modelu kontroli dostępu opartej na rolach (RBAC) uprawnienia są skojarzone z rolami, a nie pojedynczymi użytkownikami. Dzięki temu każdy użytkownik może dać każdemu użytkownikowi uprawnienia wymagane do wykonania obowiązków użytkownika, zmieniając role użytkownika, a nie przydzielając ani nie odmawiając uprawnień użytkownika. W związku z tym przypisanie uprawnień nie wymaga już uczestnictwa działu IT, ale można je wykonać w ramach zarządzania firmą. Rola może agregować uprawnienia do uzyskiwania dostępu do różnych systemów bezpośrednio lub za pomocą podról, co dodatkowo zmniejsza potrzebę zaangażowania działu IT w Zarządzanie uprawnieniami użytkownika.

Należy pamiętać, że role są funkcją samego modelu RBAC; zazwyczaj role nie są obsługiwane w aplikacjach docelowych. Dzięki temu funkcja RBAC może być używana razem z istniejącymi aplikacjami, które nie są przeznaczone do używania ról lub do zmiany definicji ról, muszą spełniać potrzeby zmiany modeli firmy bez konieczności modyfikowania samych aplikacji. Jeśli aplikacja docelowa została zaprojektowana do korzystania z ról, można kojarzyć role w modelu roli pakietu BHOLD z odpowiednimi rolami aplikacji, traktując role specyficzne dla aplikacji jako uprawnienia.

W pakietu BHOLD można przypisać rolę do użytkownika głównie za pomocą dwóch mechanizmów:

- Przypisanie roli do jednostki organizacyjnej (jednostki organizacyjnej), do której należy użytkownik
- Przypisanie roli bezpośrednio do użytkownika

Rola przypisana do nadrzędnej jednostki organizacyjnej opcjonalnie może być dziedziczona przez jej składowe jednostki organizacyjne. Gdy rola jest przypisywana lub dziedziczona przez jednostkę organizacyjną, można ją wyznaczyć jako obowiązującą lub proponowaną rolę. Jeśli jest to skuteczna rola, wszyscy użytkownicy w jednostce organizacyjnej są przypisani do roli. Jeśli jest to proponowana rola, musi być aktywowana dla każdej jednostki organizacyjnej użytkownika lub członka, aby stała obowiązywać z członkami tego użytkownika lub jednostki organizacyjnej. Dzięki temu można przypisywać użytkownikom podzestaw ról skojarzonych z jednostką organizacyjną, a nie automatycznie przypisywać wszystkich ról jednostki organizacyjnej do wszystkich członków. Ponadto role mogą mieć określone daty rozpoczęcia i zakończenia, a limity mogą być umieszczane w procentach użytkowników w jednostce organizacyjnej, dla której rola może być skuteczna.

Na poniższym diagramie przedstawiono, w jaki sposób pojedynczy użytkownik może mieć przypisaną rolę w pakietu BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Na tym diagramie rola A jest przypisana do jednostki organizacyjnej jako rola dziedziczna i dlatego jest dziedziczona przez jej składowe jednostki organizacyjne i wszystkich użytkowników w tych jednostkach organizacyjnych. Rola B jest przypisana jako proponowana rola dla jednostki organizacyjnej. Aby użytkownik w jednostce organizacyjnej mógł zostać autoryzowany przy użyciu uprawnień roli, musi zostać aktywowany. Rola C jest efektywną rolą, dlatego uprawnienia są stosowane natychmiast do wszystkich użytkowników w jednostce organizacyjnej. Rola D jest połączona bezpośrednio z użytkownikiem i dlatego uprawnienia do nich mają zastosowanie natychmiast.

Ponadto można aktywować rolę dla użytkownika na podstawie atrybutów użytkownika. Aby uzyskać więcej informacji, zobacz Autoryzacja oparta na atrybutach.

#### <a name="permissions"></a>Uprawnienia

Uprawnienie w pakietu BHOLD odnosi się do autoryzacji zaimportowanej z aplikacji docelowej. Oznacza to, że jeśli pakietu BHOLD jest skonfigurowany do pracy z aplikacją, otrzymuje listę autoryzacji, które pakietu BHOLD mogą łączyć się z rolami. Na przykład po dodaniu Active Directory Domain Services (AD DS) do pakietu BHOLD jako aplikacji otrzymuje listę grup zabezpieczeń, które jako uprawnienia pakietu BHOLD mogą być połączone z rolami w pakietu BHOLD.

Uprawnienia są specyficzne dla aplikacji. PAKIETU BHOLD zapewnia jednolity, ujednolicony widok uprawnień, aby uprawnienia mogły być skojarzone z rolami bez konieczności poznania szczegółów implementacji uprawnień przez menedżerów ról. W tym przypadku różne systemy mogą wymuszać uprawnienia inaczej. Łącznik specyficzny dla aplikacji z usługi synchronizacji FIM do aplikacji określa, w jaki sposób zmiany uprawnień dla użytkownika są udostępniane tej aplikacji. 

#### <a name="applications"></a>Aplikacje

PAKIETU BHOLD implementuje metodę stosowania kontroli dostępu opartej na rolach (RBAC) do aplikacji zewnętrznych. Oznacza to, że po zainicjowaniu pakietu BHOLD z użytkownikami i uprawnieniami z aplikacji pakietu BHOLD może skojarzyć te uprawnienia z użytkownikami, przypisując role do użytkowników, a następnie łącząc uprawnienia z rolami. Proces w tle aplikacji może następnie zmapować odpowiednie uprawnienia do swoich użytkowników na podstawie mapowania ról/uprawnień w pakietu BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Opracowywanie modelu roli pakietu BHOLD Suite

Aby ułatwić tworzenie modelu roli, pakiet pakietu BHOLD Suite oferuje Generator modeli, narzędzie, które jest proste do użycia i wszechstronne.

Przed użyciem generatora modeli należy utworzyć serię plików, która definiuje obiekty używane przez generator modelu do konstruowania modelu roli. Aby uzyskać informacje na temat sposobu tworzenia tych plików, zobacz informacje techniczne dotyczące pakietu Microsoft pakietu BHOLD Suite.

Pierwszym krokiem przy użyciu generatora modelu pakietu BHOLD jest zaimportowanie tych plików w celu załadowania podstawowych bloków konstrukcyjnych do generatora modelu. Po pomyślnym załadowaniu plików można określić kryteria używane przez generator modelu do tworzenia kilku klas ról:

- Role członkostwa przypisane do użytkownika w oparciu o OrgUnits (jednostki organizacyjne), do których należy użytkownik
- Role atrybutów, które są przypisane do użytkownika na podstawie atrybutów użytkownika w bazie danych pakietu BHOLD
- Proponowane role, które są połączone z jednostką organizacyjną, ale muszą być aktywowane dla określonych użytkowników
- Role własności, które przydzielą kontrolę użytkownika na jednostki organizacyjne i role, dla których właściciel nie jest określony w importowanych plikach

> [!Important]
> Podczas przekazywania plików zaznacz pole wyboru **Zachowaj istniejący model** tylko w środowiskach testowych. W środowiskach produkcyjnych należy użyć generatora modeli, aby utworzyć początkowy model roli. Nie można jej użyć do zmodyfikowania istniejącego modelu roli w bazie danych pakietu BHOLD.

Po utworzeniu przez generator modelu ról w modelu roli można wyeksportować model roli do bazy danych pakietu BHOLD w postaci pliku XML.

### <a name="advanced-bhold-features"></a>Zaawansowane funkcje pakietu BHOLD

Poprzednie sekcje opisują podstawowe funkcje kontroli dostępu opartej na rolach (RBAC) w pakietu BHOLD. W tej sekcji opisano dodatkowe funkcje w pakietu BHOLD, które mogą zapewnić zwiększone zabezpieczenia i elastyczność implementacji RBAC w organizacji. Ta sekcja zawiera przegląd następujących funkcji pakietu BHOLD:

- Kardynalności
- Rozdzielenie obowiązków
- Uprawnienia z możliwością dostosowania kontekstu
- Autoryzacja oparta na atrybutach
- Elastyczne typy atrybutów


#### <a name="cardinality"></a>Kardynalności

*Kardynalność* odnosi się do implementacji reguł biznesowych, które zostały zaprojektowane w celu ograniczenia liczby przypadków, w których dwie jednostki mogą być ze sobą powiązane. W przypadku pakietu BHOLD można ustalić reguły kardynalności dla ról, uprawnień i użytkowników.

Można skonfigurować rolę, aby ograniczyć następujące czynności:

- Maksymalna liczba użytkowników, dla których można aktywować proponowaną rolę
- Maksymalna liczba ról, które mogą być połączone z rolą
- Maksymalna liczba uprawnień, które mogą być połączone z rolą

Można skonfigurować uprawnienie do ograniczania następujących czynności:

- Maksymalna liczba ról, które mogą być połączone z uprawnieniem
- Maksymalna liczba użytkowników, którym można udzielić uprawnienia

Można skonfigurować użytkownika tak, aby ograniczyć następujące czynności:

- Maksymalna liczba ról, które mogą być połączone z użytkownikiem
- Maksymalna liczba uprawnień, które mogą być przypisane do użytkownika za pomocą przypisań ról

#### <a name="separation-of-duties"></a>Rozdzielenie obowiązków

Rozdzielenie obowiązków (SoD) jest zasadą biznesową, która dąży do uniemożliwienia jednostkom wykonywania działań, które nie powinny być dostępne dla jednej osoby. Na przykład pracownik nie powinien w stanie zażądać płatności i autoryzować płatności. Zasada SoD umożliwia organizacjom wdrożenie systemu kontroli i bilansów w celu zminimalizowania ryzyka związanego z błędem pracownika lub ich niepowodzeniem.

PAKIETU BHOLD implementuje SoD przez umożliwienie definiowania niezgodnych uprawnień. Gdy te uprawnienia są zdefiniowane, pakietu BHOLD wymusza SoD, uniemożliwiając tworzenie ról, które są połączone z niezgodnymi uprawnieniami, niezależnie od tego, czy są połączone bezpośrednio, czy przez dziedziczenie, i uniemożliwiając użytkownikom przypisanie wielu ról, gdy połączone, przydzielą ponownie niezgodne uprawnienia przez przypisanie bezpośrednie lub dziedziczenie. Opcjonalnie można zastąpić konflikty.

#### <a name="context-adaptable-permissions"></a>Uprawnienia z możliwością dostosowania kontekstu

Tworząc uprawnienia, które mogą być automatycznie modyfikowane na podstawie atrybutu obiektu, można zmniejszyć łączną liczbę uprawnień do zarządzania. Uprawnienia z możliwością dostosowania kontekstu (CAP) umożliwiają zdefiniowanie formuły jako atrybutu uprawnienia, która modyfikuje sposób zastosowania uprawnień przez aplikację skojarzoną z uprawnieniem. Można na przykład utworzyć formułę, która zmienia uprawnienia dostępu do folderu plików (za pośrednictwem grupy zabezpieczeń skojarzonej z listą kontroli dostępu), w zależności od tego, czy użytkownik należy do jednostki organizacyjnej (jednostki organizacyjnej) zawierającej pracownicy w całym czasie lub w danej umowie. Jeśli użytkownik jest przenoszony z jednej jednostki organizacyjnej do innej, nowe uprawnienie jest automatycznie stosowane i stare uprawnienie zostanie zdezaktywowane. 

Formuła zakończenia może wysyłać zapytania dotyczące wartości atrybutów, które zostały zastosowane do aplikacji, uprawnień, jednostek organizacyjnych i użytkowników.

#### <a name="attribute-based-authorization"></a>Autoryzacja oparta na atrybutach

Jednym ze sposobów, aby kontrolować, czy rola, która jest połączona z jednostką organizacyjną (jednostka organizacyjna) została aktywowana dla określonego użytkownika w jednostce organizacyjnej, ma używać autoryzacji opartej na atrybutach (ABA). Za pomocą ABA można automatycznie aktywować rolę tylko wtedy, gdy spełnione są pewne reguły na podstawie atrybutów użytkownika. Można na przykład połączyć rolę z jednostką organizacyjną, która stanie się aktywna dla użytkownika tylko wtedy, gdy stanowisko użytkownika jest zgodne z tytułem zadania w regule ABA. Eliminuje to konieczność ręcznego uaktywniania proponowanej roli dla użytkownika. Zamiast tego można aktywować rolę dla wszystkich użytkowników w jednostce organizacyjnej, którzy mają wartość atrybutu, która spełnia warunki reguły ABA roli. Można łączyć reguły, aby rola została aktywowana tylko wtedy, gdy atrybuty użytkownika spełniają wszystkie reguły ABA określone dla roli.

Należy pamiętać, że wyniki testów reguł ABA są ograniczone przez ustawienia kardynalności. Na przykład jeśli ustawienie kardynalności reguły określa, że nie można przypisać roli więcej niż dwóch użytkowników, a w przeciwnym razie reguła ABA będzie aktywować rolę dla czterech użytkowników, rola zostanie aktywowana tylko dla pierwszych dwóch użytkowników, którzy przechodzą test ABA.

#### <a name="flexible-attribute-types"></a>Elastyczne typy atrybutów

System atrybutów w pakietu BHOLD jest wysoce rozszerzalny. Można zdefiniować nowe typy atrybutów dla takich obiektów, jak użytkownicy, jednostki organizacyjne (jednostki organizacyjne) i role. Atrybuty mogą być zdefiniowane, aby mieć wartości, które są liczbami całkowitymi, wartość logiczna (tak/nie), alfanumeryczne, daty, godziny i adresy e-mail. Atrybuty można określić jako pojedyncze wartości lub listę wartości.

## <a name="attestation"></a>Zaświadczanie

Pakiet pakietu BHOLD zawiera narzędzia, których można użyć do sprawdzenia, czy poszczególni użytkownicy mają odpowiednie uprawnienia do wykonywania ich zadań firmowych. Administrator może użyć portalu dostarczonego przez moduł zaświadczania pakietu BHOLD, aby zaprojektować proces zarządzania zaświadczeniem.

Proces zaświadczania jest realizowany przez kampanie, w których Stewards kampanii otrzymuje okazję i środki, aby sprawdzić, czy użytkownicy, dla których są odpowiedzialni, mają odpowiedni dostęp do aplikacji zarządzanych przez pakietu BHOLD i odpowiednie uprawnienia w ramach te aplikacje. Właściciel kampanii jest wyznaczeni do nadzorowania kampanii i upewnienia się, że Kampania została przeprowadzona prawidłowo. Kampanię można utworzyć tak, aby była wykonywana raz lub cyklicznie.

Zazwyczaj Steward dla kampanii będzie menedżerem, który zaświadczy prawa dostępu użytkowników należących do co najmniej jednej jednostki organizacyjnej, dla której Menedżer jest odpowiedzialny. Stewards można automatycznie wybrać dla użytkowników zapisywanych w kampanii na podstawie atrybutów użytkownika, a Stewards dla kampanii można zdefiniować przez wystawienie ich w pliku, który mapuje każdego użytkownika zarejestrowanego w kampanii na Steward.

Po rozpoczęciu kampanii pakietu BHOLD wysyła wiadomość e-mail z powiadomieniem do Stewards i właściciela kampanii, a następnie wysyła okresowe przypomnienia, aby ułatwić im zachowanie postępu w kampanii. Stewards są kierowane do portalu kampanii, gdzie mogą zobaczyć listę użytkowników, dla których są odpowiedzialni, oraz ról przypisanych do tych użytkowników. Stewards może następnie potwierdzić, czy są odpowiedzialni za każdego z wymienionych użytkowników i zatwierdzić lub odmówić prawa dostępu każdego z wymienionych użytkowników.

Właściciele kampanii używają również portalu do monitorowania postępu kampanii, a działania kampanii są rejestrowane, aby właściciele kampanii mogli analizować akcje, które zostały wykonane w trakcie kampanii.

## <a name="analytics"></a>Analiza

Jednym z ważnych zagadnień związanych z implementowaniem kompleksowej kontroli dostępu opartej na prawach jest równowaga między utrzymaniem ścisłej kontroli dostępu i uniknięciem niepotrzebnych (lub gorszych, nieoczekiwanych) barier w dostępie. Konieczność utrzymania tego salda często skutkuje strukturą kontroli dostępu, która jest tak skomplikowana, że nieoczekiwane interakcje między zasadami są prawie nieuniknione.

Z tego powodu ważne jest, aby można było analizować skutki zasad kontroli dostępu, zanim zostaną one rzeczywiście umieszczone. Moduł analityczny pakietu pakietu BHOLD Suite daje możliwość wykonywania tej analizy, umożliwiając tworzenie reguł reprezentujących zasady, a następnie wyświetlanie użytkowników, których uprawnienia są zgodne lub powodują konflikt z regułą. Na podstawie tej analizy można dostosować zasady lub zmodyfikować role i uprawnienia, aby wyeliminować konflikty z zasadami.

Portal pakietu BHOLD Analytics umożliwia konstruowanie zestawów reguł składających się z co najmniej jednej reguły, która została utworzona w celu przetestowania określonych zasad lub grup zasad. Reguła składa się z następujących głównych części:

- Tytuł i opis, które umożliwiają identyfikację i opisywanie reguły
- Stan wskazujący, czy reguła jest gotowa do przeglądu, jest przeglądana lub zatwierdzana
- Zestaw elementów (na przykład użytkownicy lub uprawnienia), który ma zostać przetestowany przez regułę
- Opcjonalne filtry podzestawów, które są wyrażeniami, których można użyć do wybrania odpowiedniej podgrupy elementu do zbadania
- Jeden lub więcej filtrów reguł, które są wyrażeniami reprezentującymi testowane zasady.

Reguła może przetestować jeden z następujących zestawów elementów:

- Użytkownicy
- Jednostki organizacyjne
- Role
- Uprawnienia
- Aplikacje
- Konta

Na poniższym diagramie przedstawiono prostą regułę składającą się z dwóch reguł podzbioru i dwóch reguł filtrów:

![](media/bhold-concepts-guide/rules.png)

Zwróć uwagę na różnice wynikające z niepowodzenia filtru podzbioru i niepowodzenia filtru reguł: niepowodzenie filtru podzestawu powoduje usunięcie obiektu elementu z testów przez kolejne filtry, podczas gdy filtr reguły powoduje niezgodność obiektu. Tylko te obiekty, które przechodzą wszystkie filtry podzestawów i wszystkie filtry reguł są zgłaszane jako zgodne.

Każdy filtr składa się z typu, operatora (który jest zależny od typu), klucza (jednego z elementów) i wartości, względem których klucz jest testowany przez operatora. Na przykład następujący filtr przetestuje, czy liczba użytkowników w podzbiorze elementów przekracza 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Typ:**   | Liczba   |
| **Głównych**  | Użytkownicy  |
| **Zakład**  | >  |
| **Wartość:** | 10 |

Filtry reguł mogą mieć trzy typy i używać operatorów specyficznych dla ich typu, jak wskazano:

- Atrybut
  - < i >
  - = i! =
  - **Wyświetlana**
  - **Nie zawiera**
- Liczba
  - < i >
  - = i! =
- Restrykcyjne
  - **Musi mieć wszystkie**
  - **Nie może istnieć i nie może zawierać wszystkich**
  - **Może mieć tylko wszystkie i mogą mieć tylko wszystkie**
  - **Dostępne wyłącznie dla wszystkich**

> [!Note]
> Filtry restrykcyjne mogą używać wskazanych operatorów do testowania klucza względem zestawu wielu wartości.

Na przykład jeśli chcesz przetestować implementację zasad podziału obowiązków (SoD), które określają, że żaden użytkownik, który ma uprawnienia do płatności, ma także uprawnienia do zatwierdzania płatności, można utworzyć regułę podobną do następującej:

|   |  |
|---|--|
|Nazwa:| Test SoD płatności|
|Postaci| Użytkownicy|
|Filtr podzestawu:| Posiadanie każdej płatności żądania uprawnień|
|Filtr reguł: | Nie może mieć żadnych płatności zatwierdzania uprawnień|

Po uruchomieniu tej reguły moduł pakietu BHOLD Analytics wyświetli liczbę użytkowników w wybranym podzbiorze (liczbę użytkowników z uprawnieniem Żądaj płatności), liczbę użytkowników, którzy są zgodni z regułą, oraz liczbę użytkowników, którzy nie są zgodni z tą regułą. Następnie można wyświetlić niezgodnych użytkowników, aby można było poprawić niespójność.

Oprócz wyświetlania wyników można również zapisać raport jako plik z wartościami rozdzielanymi przecinkami (CSV) lub XML, aby umożliwić przeanalizowanie wyników później. Możesz również dostosować raport wynikowy, aby wyświetlić dodatkowe informacje, które mogą pomóc lepiej zrozumieć wpływ. Na przykład w przypadku testowania użytkowników można wyświetlić (lub zgłosić) konta zgodnych lub niezgodnych użytkowników, aby zobaczyć, które aplikacje są wykorzystywane.

Ponieważ reguła może zawierać wiele filtrów, można połączyć filtry w celu sprawdzenia, czy istnieje określona kombinacja warunków. Domyślnie wynikiem jest iloczyn i test logiczny wszystkich filtrów, ale można określić, czy ma być wykonywany test kombinacji filtru.

Na przykład jeśli zasady biznesowe wymagają od menedżerów posiadania uprawnienia Modyfikuj płatność lub zatwierdzenia płatności, można przetestować zgodność z zasadami, tworząc regułę podobną do następującej:


|  |  |
|--|--|
|Nazwa: | Modyfikowanie testu SoD płatności|
|Postaci | Użytkownicy |
|Filtr podzestawu: | Posiadanie dowolnego menedżera ról|
| Filtry reguł: |Musi mieć uprawnienia Modyfikuj płatność </br> Musi mieć zatwierdzenie zgody|

Domyślnie każdy użytkownik, który jest menedżerem, który ma uprawnienie Modyfikuj płatność i płatność żądania płatności, będzie raportowany jako zgodny. Zasady wymagają jednak, aby Menedżer miał uprawnienia, niekoniecznie oba te elementy. Aby przetestować rzeczywistą zgodność z zasadami, należy użyć operatora logicznego OR z regułą, aby określić, czy istnieją menedżerów, którzy nie mają uprawnień do modyfikowania płatności lub zatwierdzania płatności.

W przeciwieństwie do innych operatorów, **wyłączny jest dowolny** i **wyłączność wszystkich** operatorów wskazują zgodność dla obiektów, które w przeciwnym razie byłyby wykluczone przez filtr podzestawu. Na przykład w celu przetestowania zasad, które wszyscy menedżerowie (i tylko menedżerowie) mają uprawnienie zatwierdzanie recenzji, można utworzyć regułę w następujący sposób:

|  |  |
|--|--|
|Nazwa: | Przejrzyj test zatwierdzania|
|Postaci | Użytkownicy|
| Filtr podzestawu: | Posiadanie dowolnego menedżera ról
|Filtr reguł: | Dowolna zgoda na zatwierdzenie ma wyłącznie uprawnienia|

Ta reguła będzie raportowana jako użytkownicy zgodni z menedżerami i uprawnieni do zatwierdzania recenzji oraz użytkowników, którzy nie mają uprawnień do zatwierdzania recenzji. Z kolei kierownicy, którzy nie mają uprawnień i użytkowników, którzy nie są menedżerami, ale mają uprawnienia, które są zgłaszane jako niezgodne.

Jak wspomniano wcześniej, można połączyć reguły z zestawem reguł, ułatwiając kategoryzowanie reguł i zarządzanie nimi w celu spełnienia wymagań firmy.

Można również zdefiniować zestaw filtrów globalnych, które po włączeniu są stosowane do każdej testowanej reguły. Jeśli często konieczne jest wykluczenie określonego podzbioru rekordów podczas testowania reguł z różnych zestawów reguł, można określić filtry globalne, które można włączać lub wyłączać w razie potrzeby.

## <a name="reporting"></a>Raportowanie

Moduł raportowania pakietu BHOLD umożliwia wyświetlanie informacji w modelu roli za pomocą różnych raportów. Moduł raportowania pakietu BHOLD zawiera obszerny zestaw wbudowanych raportów, a także kreatora, który służy do tworzenia podstawowych i zaawansowanych raportów niestandardowych. Po uruchomieniu raportu można natychmiast wyświetlić wyniki lub zapisać wyniki w pliku programu Microsoft Excel (. xlsx). Aby wyświetlić ten plik przy użyciu programu Microsoft Excel 2000, programu Microsoft Excel 2002 lub Microsoft Excel 2003, można pobrać i zainstalować pakiet zgodności Microsoft Office dla formatów plików programów Word, Excel i PowerPoint.


Moduł raportowania pakietu BHOLD jest używany głównie do tworzenia raportów na podstawie bieżących informacji o rolach. Aby generować raporty na potrzeby inspekcji zmian informacji o tożsamości, program Forefront Identity Manager 2010 R2 ma możliwość raportowania dla usługi FIM, która jest zaimplementowana w magazynie danych System Center Service Manager. Aby uzyskać więcej informacji na temat raportowania FIM, zobacz dokumentację programu Forefront Identity Manager 2010 i Forefront Identity Manager 2010 R2 w bibliotece technicznej programu Forefront Identity Management.

Kategorie objęte wbudowanymi raportami obejmują następujące elementy:

- Administration
- Zaświadczanie
- Kontrolek
- Access Control do wewnątrz
- Rejestrowanie
- Model
- Statystyki
- Przepływ pracy

Można tworzyć raporty i dodawać je do tych kategorii lub definiować własne kategorie, w których można umieszczać raporty niestandardowe i wbudowane.

Podczas tworzenia raportu Kreator poprowadzi Cię przez podawanie następujących parametrów:

- Informacje identyfikacyjne, w tym nazwa, opis, Kategoria, użycie i odbiorcy
- Pola, które mają być wyświetlane w raporcie
- Zapytania określające, które elementy mają być analizowane
- Kolejność, w której wiersze mają być sortowane
- Pola używane do dzielenia raportu na sekcje
- Filtry w celu uściślenia elementów, które są zwracane w raporcie

Na każdym etapie kreatora można wyświetlić podgląd raportu zgodnie z jego definicją i zapisać go, jeśli nie trzeba podawać dodatkowych parametrów. Możesz również przejść z powrotem do poprzednich kroków, aby zmienić parametry określone wcześniej w kreatorze.

## <a name="access-management-connector"></a>Łącznik zarządzania dostępem

Moduł łącznika zarządzania dostępem do pakietu BHOLD Suite obsługuje początkową i ciągłą synchronizację danych w pakietu BHOLD. Łącznik zarządzania dostępem współpracuje z usługą synchronizacji FIM do przenoszenia danych między podstawową bazą danych pakietu BHOLD, metaverseem programu MIM oraz aplikacjami docelowymi i magazynami tożsamości.

Poprzednie wersje pakietu BHOLD wymagały wielu MAs do sterowania przepływem danych między tabelami programu MIM i pośrednimi bazami danych pakietu BHOLD Database. W programie pakietu BHOLD Suite z dodatkiem SP1 łącznik zarządzania dostępem umożliwia definiowanie agentów zarządzania (MAs) w programie MIM, które zapewniają transfer danych bezpośrednio między pakietu BHOLD i MIM.

## <a name="mim-integration"></a>Integracja z programem MIM

Ważną i wydajną funkcją programów Forefront Identity Manager 2010 i Forefront Identity Manager 2010 R2 jest Portal samoobsługowy, który umożliwia użytkownikom końcowym wyświetlanie informacji o tożsamości i członkostwie oraz zarządzanie nimi. Integracja z programem MIM rozszerza Portal programu MIM za pomocą samoobsługowego zarządzania rolami. Na przykład przy użyciu funkcji pakietu BHOLD w portalu MIM użytkownik może zażądać przypisania roli i może wyświetlać aktywne role i oczekujące żądania. Do administratorów delegowanych mogą być udzielane dodatkowe możliwości, takie jak możliwość żądania przypisywania ról innym użytkownikom.

Należy pamiętać, że rozszerzenia pakietu BHOLD w portalu MIM obsługują Samoobsługowe zarządzanie rolami i przepływami pracy oraz raportowanie. Inne funkcje administracyjne pakietu BHOLD, a także zaświadczanie, są dostarczane przez portale sieci Web modułów pakietu BHOLD, które są hostowane niezależnie od portalu programu MIM.

## <a name="next-steps"></a>Następne kroki

- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
