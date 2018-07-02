---
title: Przewodnik koncepcje pakietu Microsoft BHOLD | Dokumentacja firmy Microsoft
description: Rozpoczęcie pracy ze składnikami programu MIM 2016 przez instalację i konfigurację usługi synchronizacji.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/14/2017
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 521025de3dc16a9bda02aed8287faeb3449192c1
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290071"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Przewodnik koncepcje pakietu Microsoft BHOLD

Program Microsoft Identity Manager 2016 (MIM) umożliwia organizacjom zarządzanie całym cyklu ich życia tożsamości użytkowników i ich skojarzone poświadczenia. Może być skonfigurowana do synchronizacji tożsamości, centralne zarządzanie certyfikatami i hasła i udostępnić użytkownikom we wszystkich systemach heterogenicznych. Z programem MIM organizacji IT można zdefiniować i automatyzować procesy używane do zarządzania tożsamościami z tworzenia do wycofania.

Pakiet BHOLD Microsoft rozszerza możliwości programu MIM, dodając kontroli dostępu opartej na rolach. BHOLD umożliwia organizacji w celu definiowania ról użytkowników oraz kontrolować dostęp do poufnych danych i aplikacji. Dostęp jest oparty co to jest odpowiedni dla tych ról. Pakiet BHOLD obejmuje usług i narzędzi, które upraszczają modelowania relacji roli w organizacji. BHOLD mapuje te role praw i upewnić się, że definicje ról i skojarzonymi prawami poprawnie są stosowane do użytkowników. Te możliwości są w pełni zintegrowana z programem MIM, zapewniająca nie zakłóca pracy użytkowników końcowych i podobne personel działu informatycznego.

Ten przewodnik pomaga zrozumieć sposób działania pakietu BHOLD z programem MIM i obejmuje następujące tematy:

- Kontrola dostępu oparta na rolach
- Poświadczenie
- Analityka
- Raportowanie
- Łącznik zarządzania dostępu
- Integracja MIM

## <a name="role-based-access-control"></a>Kontrola dostępu oparta na rolach

Najbardziej typowa metoda sterowania dostępem użytkowników do danych i aplikacji jest za pośrednictwem kontroli dostępu (DAC). W typowych wdrożeniach każdy obiekt znaczących ma zidentyfikowanych właściciela. Właściciel ma możliwość udzielania lub odmawiania dostępu do obiektów innym użytkownikom na podstawie tożsamości pojedynczych lub członkostwa w grupie. W praktyce DAC zazwyczaj wynikiem nadmiar grup zabezpieczeń, niektóre odzwierciedlające struktura organizacyjna, inne osoby, reprezentujące funkcjonalności grupowania (na przykład typy zadań lub przydziałów projektu) i innych składających się z makeshift kolekcji użytkowników i urządzenia, które są połączone więcej celów tymczasowego. Wzrostem organizacji członkostwo w tych grupach staje się coraz trudniejsze do zarządzania. Na przykład jeśli pracownik został przeniesiony z jednego projektu do innego, grup, które są używane do kontrolowania dostępu do zasobów projektów należy aktualizować ręcznie. W takich przypadkach nie jest często błędy występują błędy, które mogą utrudniać bezpieczeństwo projektu lub wydajność.

Program MIM zawiera funkcje, które pomagają uniknąć tego problemu, zapewniając automatyczne kontrolę nad członkostwa grupy i dystrybucji na liście. Jednak nie eliminuje złożoność wewnętrzne rozmnażające grup, które nie są zawsze powiązane ze sobą w postaci struktury.

Jest jednym ze sposobów znacznie zmniejszyć to mnożenie przez wdrożenie kontroli dostępu opartej na rolach (RBAC). RBAC wykluczają się DAC.  RBAC opiera się na DAC, podając struktury klasyfikowania użytkowników i zasoby IT. Dzięki temu można utworzyć jawnej ich relacje i praw dostępu, które są odpowiednie zgodnie z klasyfikacji. Na przykład przez przypisanie użytkownikowi atrybuty, które określają użytkowników stanowisko i przydziałów projektu, użytkownik może uzyskać dostęp do narzędzia potrzebne do zadania i dane, które użytkownik chce przyczyniają się do określonego projektu użytkownika. Gdy użytkownik przyjmuje różne zadania i przydziały inny projekt, zmiany atrybutów, które określają stanowisko użytkownika i projektów automatycznie blokuje dostęp do zasobów tylko wymaganych do poprzedniej pozycji użytkowników.

Ponieważ role mogą być zawarte w ramach ról w hierarchiczny sposób (na przykład role menedżera sprzedaży i przedstawiciel handlowy mogą być zawarte w bardziej ogólne roli sprzedaży), łatwo ją przypisać odpowiednie uprawnienia do określonych ról i nadal jeszcze Podaj odpowiednie prawa do każdego użytkownika, który udostępnia również rolę więcej włącznie. Na przykład w szpital, wszystkie osoby medyczne może mieć prawo do wyświetlania rekordów pacjentów, ale lekarzy (rolą podrzędną rolę medyczne) może mieć prawo do wprowadzania wymagań dla pacjenta. Podobnie użytkownicy należący do roli maszynowy może mieć dostępu do pacjenta rekordów z wyjątkiem rozliczeń urzędników (rolą podrzędną rolę biurowych), którzy mogą uzyskać dostęp do tych fragmentów rekordów pacjentów, które są wymagane, aby rozliczyć pacjenta dla usług udostępniane przez szpital Twoich.

Dodatkowa korzyść RBAC jest możliwość definiujące i wymuszające podział obowiązków (darni). Dzięki temu organizacji zdefiniować kombinacje ról, które udzielić uprawnień, które nie powinny być objęte przez tego samego użytkownika, dzięki czemu ról, które umożliwiają użytkownikowi zainicjować płatności i autoryzować płatności, na przykład nie można przypisać określonego użytkownika. RBAC zapewnia możliwość egzekwowania takie zasady automatycznie zamiast do oceny skutecznego stosowania zasad dla poszczególnych użytkowników.

### <a name="bhold-role-model-objects"></a>Obiekty modelu roli BHOLD

Pakiet BHOLD możesz określić i organizowanie ról w organizacji, mapy użytkowników do ról, a mapy odpowiednich uprawnień do ról. Ta struktura jest nazywany model roli, a nie zawiera i łączy się z pięciu typów obiektów: 

- Jednostki organizacyjne
- Users
- Role
- Uprawnienia
- Aplikacje

#### <a name="organizational-units"></a>Jednostki organizacyjne

Jednostki organizacyjne (OrgUnits) są podstawowych środków za pomocą której użytkownicy są zorganizowane w modelu BHOLD roli. Każdy użytkownik musi należeć do co najmniej jeden OrgUnit. (W rzeczywistości usunięcie użytkownika z ostatnich jednostki organizacyjnej w BHOLD rekordu danych użytkownika zostaną usunięte z bazy danych BHOLD.)

> [!Important]
> Jednostki organizacyjne w modelu roli BHOLD nie należy mylić z jednostki organizacyjnej w usługach domenowych w usłudze Active Directory (AD DS). Zazwyczaj struktury jednostek organizacyjnych w BHOLD opiera się na organizacji i zasad firmy, nie wymagania dotyczące infrastruktury sieci.

Chociaż nie jest wymagany, w większości przypadków jednostek organizacyjnych struktura BHOLD do reprezentowania strukturę hierarchiczną rzeczywiste organizacji, podobny do przedstawionego poniżej:

![](media/bhold-concepts-guide/org-chart.png)

W tym przykładzie model roli czy jednostka organizationalganizatinal corporation jako całość (reprezentowane przez prezesa, ponieważ Prezes nie jest częścią jednostki mororganizationalganizatinal) lub jednostki organizacyjnej głównego BHOLD (który zawsze istnieje) może służyć do tego celu. OrgUnits reprezentujący działów firmowych kierowany przez prezesa vice zostanie umieszczony w jednostce organizacyjnej firmy. Następny, organizacyjnej jednostki odpowiadający dyrektorów sprzedaży i marketingu zostanie dodany do jednostki organizacyjnej sprzedaży i marketingu i jednostek organizacyjnych reprezentujący regionalnych menedżerów zostanie umieszczony w jednostce organizacyjnej dla Menedżer sprzedaży regionu wschodnie. Stowarzyszonych sprzedaży, którzy nie Zarządzaj innych użytkowników, nastąpi członkami jednostki organizacyjnej Menedżera sprzedaży regionu wschodnie. Należy pamiętać, że użytkownicy mogą być elementami członkowskimi jednostki organizacyjnej na dowolnym poziomie. Na przykład Asystenta administracyjnego, który nie jest Menedżer i raporty bezpośrednio Wiceprezes, będzie członkiem Wiceprezes w jednostce organizacyjnej.

Oprócz reprezentujący struktura organizacyjna, jednostek organizacyjnych mogą służyć do grupy użytkowników i inne jednostki organizacyjne, zgodnie z funkcjonalności kryteria, takich jak dla projektów lub specjalizacji. Na poniższym diagramie przedstawiono sposób organizacji jednostki będzie służyć do grupowania skojarzone sprzedaży według typu klienta:

![](media/bhold-concepts-guide/org-chart-02.png)

W tym przykładzie każdy pracownik sprzedaży może należeć do dwóch jednostek organizacyjnych: jeden reprezentujący skojarzenia miejsce w strukturze zarządzania organizacji i jeden reprezentujący skojarzenia bazy klientów (sprzedaży lub firmowej). Każdej jednostki organizacyjnej można przypisać różne role, które z kolei można przypisać różne uprawnienia do uzyskiwania dostępu do organizacji zasobów informatycznych. Ponadto role mogą być dziedziczone z nadrzędnej jednostki organizacyjne, uproszczeniu procesu propagowanie role użytkowników. Z drugiej strony określonych ról podejmują są dziedziczone, zapewniając, że konkretnej roli są skojarzone tylko z odpowiednich jednostek organizacyjnych.

OrgUnits można tworzyć w pakiet BHOLD przy użyciu portalu sieci web BHOLD Core lub za pomocą generatora BHOLD modelu.

#### <a name="users"></a>Users

Jak wspomniano powyżej, każdy użytkownik musi należeć do co najmniej jednej jednostki organizacyjnej (OrgUnit). Ponieważ jednostki organizacyjne są główna mechanizm kojarzenia użytkownika z ról, w większości organizacji dany użytkownik należy do wielu OrgUnits, aby ułatwić powiązane role z użytkownikiem. W niektórych przypadkach jednak może być konieczne rolę można skojarzyć z użytkownikiem oprócz żadnych OrgUnits, której należy użytkownik. W rezultacie użytkownik można przypisać bezpośrednio do roli, jak również uzyskania ról z OrgUnits, do której należy użytkownik.

Gdy użytkownik nie jest aktywny w ramach organizacji (podczas optymalizacji pozostaw medyczne, na przykład), użytkownik może zostać zawieszone, która odwołuje wszystkie uprawnienia użytkownika bez usuwania z modelu roli użytkownika. Na zwrócenie do opłat, użytkownik może ponownie uaktywnić, przywraca wszystkie uprawnienia przyznane przez bieżącego użytkownika.

Obiekty użytkowników można tworzyć osobno w BHOLD za pośrednictwem portalu sieci web BHOLD Core lub ich można zaimportować zbiorczo za pomocą Generator modeli BHOLD lub za pomocą łącznika zarządzania dostępu usługi synchronizacji FIM do importowania informacji o użytkownikach z takie źródła jako aplikacje usług domenowych w usłudze Active Directory lub zasobów ludzkich.

Użytkownicy mogą być tworzone bezpośrednio w BHOLD bez użycia usługi synchronizacji programu FIM. Może to być przydatne podczas modelowania ról w środowisku przedprodukcyjnym lub testowym środowisku. Możesz również zezwolić użytkowników zewnętrznych (na przykład pracownicy podwykonawców) przypisane role i w związku z tym uzyskać dostęp do zasobów informatycznych bez dodawane do bazy danych pracownika. Jednak te użytkownicy nie będą mogli korzystać z funkcji samoobsługowego BHOLD.

#### <a name="roles"></a>Role

Jak wcześniej zanotowane, w obszarze model kontroli dostępu opartej na rolach uprawnienia są skojarzone z ról, a nie poszczególnych użytkowników. Dzięki temu można nadać każdego użytkownika uprawnień wymaganych do wykonywania obowiązków użytkownika przez zmienia role użytkownika, a nie oddzielnie przyznawanie lub odbieranie praw uprawnień użytkownika. W rezultacie przypisanie uprawnień nie wymaga już udział działu IT, ale zamiast tego można wykonać w ramach zarządzania biznesowe. Rolę można zagregować uprawnienia dostępu do innych systemów, bezpośrednio lub za pomocą podrzędne dalsze zmniejszając potrzebę zaangażowania IT w zarządzanie uprawnieniami użytkownika.

Należy pamiętać, że role są funkcją modelu RBAC. Zazwyczaj role nie są udostępnione do aplikacji docelowej. Ta umożliwia RBAC do użycia z istniejących aplikacji, które nie są przeznaczone do użycia ról lub zmiana definicje ról zaspokoić potrzeby zmiany modeli biznesowych bez konieczności modyfikowania same aplikacje. Jeśli aplikacja docelowa jest przeznaczony do użycia ról, następnie możesz skojarzone ról w modelu roli BHOLD odpowiednie role aplikacji traktując ról specyficzne dla aplikacji jako uprawnień.

W BHOLD można przypisać rolę użytkownikowi, przede wszystkim za pomocą dwóch mechanizmów:

- Przez przypisanie roli organizacyjnej jednostki (jednostki organizacyjnej), której członkiem jest użytkownik
- Przez przypisanie roli bezpośrednio do użytkownika

Rola przypisana do nadrzędnej jednostki organizacyjnej opcjonalnie mogą być dziedziczone przez jego elementu członkowskiego jednostki organizacyjnej. Gdy rola jest przypisany do lub dziedziczone przez jednostkę organizacyjną, może zostać wyznaczony jako skuteczne lub proponowanych roli. Jeśli jest efektywną rolę, wszystkich użytkowników w jednostce organizacyjnej przypisano rolę. Jeśli rola proponowanych, musi być aktywowana dla każdego użytkownika lub elementu członkowskiego jednostki organizacyjnej do zaczynają obowiązywać dla tego użytkownika lub elementy członkowskie jednostki organizacyjnej. Dzięki temu można przypisywać użytkownikom podzbiór role skojarzone z jednostką organizacyjną, a nie automatyczne przypisywanie wszystkich ról jednostki organizacyjnej, do wszystkich elementów członkowskich. Ponadto role można przydzielić daty rozpoczęcia i zakończenia i limity można umieścić na procent użytkowników w jednostce organizacyjnej, dla której roli może być skuteczne.

Na poniższym diagramie przedstawiono, jak użytkownik może mieć przypisaną rolę w BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Na tym diagramie rola A jest przypisany do jednostki organizacyjnej rolę dziedziczne i dlatego jest dziedziczona przez jego elementu członkowskiego jednostki organizacyjne i wszyscy użytkownicy w tych jednostek organizacyjnych. Rola B przypisano rolę proponowanych dla jednostki organizacyjnej. Musi być aktywowany, zanim może być upoważnionych użytkowników w jednostce organizacyjnej uprawnienia roli. Roli C jest efektywną rolę, więc jego uprawnienia natychmiast dotyczą wszystkich użytkowników w jednostce organizacyjnej. Rola D jest połączony bezpośrednio do użytkownika i dlatego jego uprawnienia natychmiast dotyczą tego użytkownika.

Ponadto można aktywować rolę dla użytkownika na podstawie atrybutów użytkownika. Aby uzyskać więcej informacji zobacz autoryzacji na podstawie atrybutu.

#### <a name="permissions"></a>Uprawnienia

Uprawnienia w BHOLD odpowiada autoryzacji zaimportowane z aplikacji docelowej. Oznacza to gdy BHOLD jest skonfigurowane do pracy z aplikacją, otrzymuje lista autoryzacji, które BHOLD można połączyć z ról. Na przykład woluminowi usług domenowych w usłudze Active Directory (AD DS) do BHOLD jako aplikacji otrzymuje listę grup zabezpieczeń, które jako BHOLD uprawnień, można połączyć z ról w BHOLD.

Uprawnienia są specyficzne dla aplikacji. BHOLD zawiera widok jednej, ujednoliconej uprawnień, więc można kojarzyć uprawnienia z rolami bez konieczności menedżerów roli poznać szczegóły implementacji uprawnień. W praktyce różnych systemów może wymuszać uprawnienia inaczej. Łącznik specyficzne dla aplikacji z usługi synchronizacji FIM do aplikacji określa, jak zmiany uprawnień dla użytkownika są dostarczane do tej aplikacji. 

#### <a name="applications"></a>Aplikacje

BHOLD implementuje metodę stosowania kontroli dostępu opartej na rolach (RBAC) do aplikacji zewnętrznych. Oznacza to gdy BHOLD jest udostępniane z użytkowników i uprawnień z aplikacji, BHOLD można skojarzyć te uprawnienia z użytkownikami przez przypisywanie ról użytkowników, a następnie łącząc uprawnienia do ról. Proces w tle aplikacji można wówczas zmapować odpowiednie uprawnienia do jego użytkowników na podstawie mapowania uprawnienia/roli w BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Tworzenie modelu roli pakietu BHOLD

Aby ułatwić opracowanie modelu roli, pakietu BHOLD zapewnia Generator modeli, łatwy w użyciu i kompleksowe narzędzie.

Przed użyciem Generator modeli, należy utworzyć serię plików, które definiują obiekty używane przez Generator modeli do utworzenia modelu roli. Aby uzyskać informacje dotyczące sposobu tworzenia tych plików Zobacz dokumentacja techniczna pakietu BHOLD firmy Microsoft.

Pierwszym krokiem przy użyciu Generator modeli BHOLD jest do importowania tych plików, aby załadować podstawowe bloki konstrukcyjne do Generator modeli. Jeśli pliki zostały pomyślnie załadowane, możesz określić kryteria wykorzystujące Generator modeli można utworzyć kilka rodzajów ról:

- Członkostwo ról, które są przypisane do użytkowników w oparciu OrgUnits (jednostki organizacyjnej), której należy użytkownik
- Atrybut role, które są przypisane do użytkownika na podstawie atrybutów użytkownika w bazie danych BHOLD
- Proponowane role, które są połączone z jednostką organizacyjną, ale musi być aktywowana dla konkretnych użytkowników
- Własność ról, które przyznać użytkownikowi kontrolę nad jednostki organizacyjne i role, dla których właścicielem nie została określona w importowanych plikach

> [!Important]
> Podczas przekazywania plików, wybierz **zachować istniejący Model** pola wyboru tylko w środowiskach testowych. W środowiskach produkcyjnych należy użyć Generator modeli do utworzenia początkowej modelu roli. Nie można go użyć do modyfikowania istniejącego modelu roli w bazie danych BHOLD.

Po Generator modeli tworzy tych ról w modelu roli, aby baza danych BHOLD w formie pliku XML można następnie wyeksportować model roli.

### <a name="advanced-bhold-features"></a>Zaawansowane funkcje BHOLD

Przedstawione w poprzednich sekcjach opisano podstawowe funkcje kontroli dostępu opartej na rolach (RBAC) w BHOLD. W tej sekcji opisano dodatkowe funkcje BHOLD zapewnić zwiększone zabezpieczenia i możliwość wykonania RBAC Twojej organizacji. Ta sekcja zawiera omówienie BHOLD następujące funkcje:

- Kardynalność
- Podział obowiązków
- Kontekst dostosowywalne uprawnień
- Autoryzacji na podstawie atrybutu
- Typy elastyczne atrybutów


#### <a name="cardinality"></a>Kardynalność

*Kardynalność* odwołuje się do stosowania reguły biznesowe, które są zaprojektowane, aby ograniczyć liczbę razy, dwie jednostki mogą być powiązane ze sobą. W przypadku BHOLD można ustanowić zasady Kardynalność ról, uprawnienia i użytkowników.

Można skonfigurować role, aby ograniczyć następujące czynności:

- Maksymalna liczba użytkowników, dla których można aktywować proponowanych roli
- Maksymalna liczba podrzędne, które mogą być połączone z roli
- Maksymalna liczba uprawnienia, które mogą być połączone z roli

Można skonfigurować uprawnienia, aby ograniczyć następujące czynności:

- Maksymalna liczba ról, które mogą być połączone z uprawnień
- Maksymalna liczba użytkowników, którym można udzielić uprawnienia

Można skonfigurować użytkownika, aby ograniczyć następujące czynności:

- Maksymalna liczba ról, które można połączyć z użytkownikiem
- Maksymalna liczba uprawnienia, które mogą być przypisane do użytkownika za pomocą przypisań ról

#### <a name="separation-of-duties"></a>Podział obowiązków

Podział obowiązków (darni) jest reguły biznesowe, która wyszukiwania, aby uniemożliwić osobom możliwość akcje, które nie powinny być dostępne do pojedynczej osoby. Na przykład pracownik ma nie mogli do żądania płatności i autoryzować płatności. Zasada darni umożliwia organizacjom zaimplementować system kontroli i zrównoważenia, aby zminimalizować ich narażenia na ryzyko związane z błąd pracownika lub naruszenie.

BHOLD implementuje darni, umożliwiając definiowanie uprawnień niezgodne. Po zdefiniowaniu tych uprawnień BHOLD wymusza darni zapobieganie tworzeniu ról, które są połączone z niezgodną uprawnienia, czy są połączone bezpośrednio lub przez dziedziczenie i zapobiegając użytkowników przed przypisane wiele ról, gdy połączyć, przyznano uprawnienia niezgodne ponownie przez przypisanie bezpośrednio lub przez dziedziczenie. Opcjonalnie można zastąpić konfliktów.

#### <a name="context-adaptable-permissions"></a>Kontekst dostosowywalne uprawnień

Tworząc uprawnienia, które mogą być automatycznie modyfikowane na podstawie atrybutu obiektu można zmniejszyć liczbę uprawnienia, które muszą zarządzać. Kontekst dostosowywalne uprawnienia (CAP) umożliwiają zdefiniowanie formułę jako atrybut uprawnienia, które modyfikuje sposób stosowania uprawnień przez aplikację skojarzoną z uprawnieniami. Na przykład można utworzyć formuła, której zmiany uprawnień dostępu do folderu plików (za pośrednictwem grupy zabezpieczeń skojarzonych z listy kontroli dostępu folderu) na podstawie czy użytkownik należy do organizacji jednostki (jednostki organizacyjnej) zawierającej stałego lub umowy pracowników. Jeśli użytkownik zostanie przeniesiony z jednej jednostki organizacyjnej do innego, nowe uprawnienie jest automatycznie stosowana i starego uprawnienie jest wyłączone. 

Formuła centralnych zasad dostępu można zbadać wartości atrybutów, które zostały zastosowane do aplikacji, uprawnienia jednostki organizacyjne i użytkowników.

#### <a name="attribute-based-authorization"></a>Autoryzacji na podstawie atrybutu

Jednym ze sposobów kontroli czy roli, która jest połączona z jednostką organizacyjną (jednostki organizacyjnej) aktywowano dla określonego użytkownika w jednostce organizacyjnej jest użycie autoryzacji na podstawie atrybutów (ABA). Przy użyciu ABA, można automatycznie aktywować rolę tylko wtedy, gdy są spełnione pewne reguły na podstawie atrybutów użytkownika. Na przykład możesz połączyć rolę z jednostką organizacyjną, która staje się aktywny dla użytkownika tylko wtedy, gdy stanowisko w regule ABA odpowiada stanowisko użytkownika. Eliminuje to potrzebę ręcznie uaktywnić proponowanych rolę dla użytkownika. Zamiast tego można aktywować rolę, dla wszystkich użytkowników w jednostce organizacyjnej, którzy mają wartość atrybutu, który spełnia reguły ABA ról. Zasady można łączyć, dzięki czemu roli jest aktywowane tylko wtedy, gdy atrybuty użytkownika spełnia wszystkie reguły ABA określone dla roli.

Należy pamiętać, że wyniki testów reguły ABA są ograniczone przez ustawienia kardynalności. Na przykład jeśli ustawienie kardynalności reguły określa, że nie więcej niż dwóch użytkowników można przypisać rolę i regułę ABA w przeciwnym razie może aktywować rolę do czterech użytkowników, tylko dla dwóch pierwszych użytkowników, które przejdą testów ABA zostanie uaktywniona roli.

#### <a name="flexible-attribute-types"></a>Typy elastyczne atrybutów

System atrybutów w BHOLD jest wysoce rozszerzalne. Możesz zdefiniować nowe typy atrybutów dla takich obiektów użytkowników i organizacji jednostki (jednostki organizacyjnej) i ról. Atrybuty można zdefiniować mają wartości będące liczbami całkowitymi, wartość logiczna (tak/nie), alfanumeryczne, daty, godziny i poczty e-mail adresów. Atrybuty można określić jako pojedynczy wartości lub listę wartości.

## <a name="attestation"></a>Poświadczenie

Pakiet BHOLD zawiera narzędzia, których można użyć, aby sprawdzić, czy poszczególni użytkownicy mają odpowiednie uprawnienia do wykonywania swoich zadań biznesowych. Administrator można użyć portalu podana przez moduł BHOLD zaświadczania o zaprojektować Zarządzaj procesem zaświadczania.

Proces zaświadczania jest przeprowadzane za pomocą kampanie, w których kampanii stewards są możliwość i oznacza sprawdzić, czy użytkownicy, dla których są one odpowiedzialne mają odpowiedni dostęp do aplikacji zarządzanych BHOLD i odpowiednie uprawnienia w te aplikacje. Właściciel kampanii wyznaczono nadzorują kampanii i upewnij się, że kampanii przebiega prawidłowo. Występuje raz lub cykliczne można utworzyć kampanię.

Zazwyczaj steward kampanii będzie manager, który potwierdza prawa dostępu użytkowników należących do jednego lub więcej jednostek organizacyjnych, za które odpowiada menedżera. Stewards automatycznie wybiera się dla użytkowników jest potwierdzone w kampanii na podstawie atrybutów użytkownika lub poprzez wyszczególnienie je w pliku, który mapuje każdego użytkownika jest potwierdzone w kampanii do steward można zdefiniować stewards kampanii.

Po rozpoczęciu kampanii BHOLD wysyła wiadomość e-mail do stewards kampanii i właściciela, a następnie wysyła okresowe przypomnienia, aby ułatwić im Obsługa postęp w kampanii. Stewards przekierowany do portalu kampanii, w którym można zobaczyć listę użytkowników, dla których są one odpowiedzialne i role, które są przypisane do tych użytkowników. Stewards następnie można potwierdzić, czy ich są odpowiedzialne za każdą z listy użytkowników i zatwierdzenie lub odrzucenie prawa dostępu każdego z listy użytkowników.

Właściciele kampanii również korzystać z portalu do monitorowania postępu kampanii i działań w ramach kampanii są rejestrowane, właściciele kampanii można analizować działania, które zostały wykonane w ramach kampanii.

## <a name="analytics"></a>Analityka

Jedną z istotnych kwestii, gdy wdrożenie systemu kontroli kompleksowe na podstawie praw dostępu jest kompromis między zachowaniu kontroli dostępu i unikanie niepotrzebnych (lub, pogarsza się wraz z, nieoczekiwany) bariery dostępu. Starań, aby zachować saldo to często skutkuje strukturę kontroli dostępu, tak że nieoczekiwany interakcje między zasadami są prawie nieuniknione złożoną.

Dlatego jest ważne móc analizować skutków zasad kontroli dostępu, zanim zostaną one faktycznie w miejscu. Moduł Analytics pakietu BHOLD daje możesz możliwość wykonywania analiza ta umożliwia tworzenie reguł, które reprezentują zasad, a następnie przedstawiający użytkowników którego uprawnienia są zgodne lub konflikt z regułą. Na podstawie tej analizy, można dostosować zasady lub modyfikowania ról i uprawnień w celu wyeliminowania konfliktów z zasadami.

Portal analityka BHOLD daje możliwość utworzenia zestawów reguł, który składa się z co najmniej jednej reguły, utworzonych przez Ciebie do testowania konkretne zasady lub zasad grupy. Reguła składa się z następujących głównych składników:

- Tytuł i opis, które umożliwiają identyfikujące i opisujące reguły
- Stan, który wskazuje, czy reguła jest gotowa do przeglądu, trwa recenzji lub zatwierdzenia
- Ustaw element (na przykład użytkownicy lub uprawnienia) która reguła została opracowana w celu przetestowania
- Filtry opcjonalne podzestawu wyrażeń, które można użyć do wybrania odpowiednich podgrupy elementu, który ma być zbadana
- Jeden lub więcej reguł filtry wyrażeń, które reprezentują zasad testowana.

Regułę można przetestować któregokolwiek z następujących elementów:

- Users
- Jednostki organizacyjne
- Role
- Uprawnienia
- Aplikacje
- Konta

Na poniższym diagramie przedstawiono Prosta reguła składające się z dwóch podzbiór reguł i dwie reguły filtrowania:

![](media/bhold-concepts-guide/rules.png)

Różnice w efekcie niepowodzeniem filtrze podzestawu i niepowodzeniem filtru reguły: niepowodzenie filtrze podzestawu usuwa obiekt elementu testowania przez kolejne filtry, gdy niepowodzeniem filtru reguły powoduje, że obiekt, aby były zgłaszane jako niezgodne. Tylko te obiekty, które przejdą wszystkie filtry podzestawu i wszystkie filtry reguły są raportowane jako zgodne.

Każdego filtru składa się z typem, operatora (który jest typem zależnych), klucz (jeden z elementów) i wartość, względem którego klucz został przetestowany przez operatora. Na przykład następujący filtr może przetestować, czy liczba użytkowników w podzbiorze elementu przekracza 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Typ:**   | Liczba   |
| **Klucz:**  | Users  |
| **Operator**  | >  |
| **Wartość:** | 10 |

Filtry zasad można się z trzech typów i operatory specyficzne dla ich typu, jak wskazano:

- Atrybut
  - < a >
  - = i! =
  - **Zawiera**
  - **Nie zawiera**
- Liczba
  - < a >
  - = i! =
- Ograniczających
  - **Musi mieć jedną i musi mieć wszystkie**
  - **Nie ma żadnych i nie może mieć wszystkich**
  - **Można tylko ma i może zawierać tylko wszystkie**
  - **Nie ma żadnej wyłącznie i wyłącznie mają wszystkie**

> [!Note]
> Filtry ograniczające można używać operatorów wskazany do testowania klucza z zestawem wielu wartości.

Na przykład jeśli chcesz przetestować implementacji podział obowiązków (darni) zasady, które stany nie użytkownika, który ma uprawnienia do żądania płatności nieprawidłowość również mieć uprawnienie zatwierdzić płatności, można utworzyć reguły podobne do poniższych:

|   |  |
|---|--|
|Nazwa:| Test darni płatności|
|Element:| Users|
|Filtr podzbioru:| Wszystkie uprawnienia żądania płatności|
|Filtr reguły: | Nie może mieć żadnych uprawnień Zatwierdź płatności|

Po uruchomieniu tej reguły, moduł analizy BHOLD Wyświetla liczbę użytkowników w wybranego podzbioru (liczba użytkowników z uprawnieniem płatności żądania), liczba użytkowników, które są zgodne z regułą i liczbę użytkowników, które nie są zgodne z regułą. Następnie można wyświetlić niezgodnych użytkownicy, można poprawić niespójność.

Oprócz wyświetlania wyników, można również Zapisz raport jako plik wartości rozdzielanych przecinkami (CSV) lub plik XML umożliwia analizowanie wyników później. Można również dostosować wynikowego raportu, aby wyświetlić dodatkowe informacje, które pomogą lepiej zrozumieć wpływ. Na przykład jeśli testujesz użytkownicy, można wyświetlić (; raportu) kont użytkowników zgodnych i niezgodnych, dzięki czemu aplikacje, które są związane z

Ponieważ reguła może zawierać wiele filtrów, możesz połączyć filtrów, aby sprawdzić czy istnieją kombinacji warunków. Domyślnie wynik jest wynikiem testu logicznego wszystkich filtrów, ale można określić wykonanie testu lub kombinacji filtru.

Na przykład zasady biznesowe wymagają menedżerów uprawnienia do modyfikowania płatności lub uprawnień do zatwierdzenia płatności, następnie może przetestowanie zgodności z zasadami, tworząc reguły podobne do poniższych:


|  |  |
|--|--|
|Nazwa: | Zmodyfikuj płatności darni testu|
|Element: | Users |
|Filtr podzbioru: | O dowolnej roli menedżera|
| Filtry reguły: |Musi mieć żadnych uprawnień Modyfikuj płatności </br> Musi mieć żadnych uprawnień Zatwierdź płatności|

Domyślnie każdy użytkownik, który jest Menedżer, kto ma uprawnienia do żądania płatności i płatności zmodyfikować będą raportowane jako zgodne. Jednak zasady musi mieć menedżera albo uprawnienie, nie musi to być zarówno. Aby przetestować rzeczywista zgodność z zasadami, należy użyć operatora logicznego OR z regułą do ustalenia, czy są wszystkie menedżerów, którzy nie mają uprawnień do modyfikowania płatności albo uprawnienie zatwierdzić płatności.

W przeciwieństwie do innych operatory **wyłącznie nie ma żadnej** i **wyłącznie mają wszystkie** operatory wskazuje zgodności dla obiektów, które w przeciwnym razie będzie wykluczone przez filtr podzbioru. Na przykład do testowania zasad, że wszyscy menedżerowie (i tylko menedżerowie) ma uprawnień do zatwierdzenia przeglądu można utworzyć reguły w następujący sposób:

|  |  |
|--|--|
|Nazwa: | Test zatwierdzenia przeglądu|
|Element: | Users|
| Filtr podzbioru: | O dowolnej roli menedżera
|Filtr reguły: | Wyłącznie ma wszystkie przeglądy zatwierdzenia uprawnień|

Ta zasada będzie raportowane jako zgodne użytkowników, którzy są menedżerów i mieć uprawnienie zatwierdzić przeglądy i użytkowników, które nie są menedżerów i którzy nie mają uprawnienia do zatwierdzenia przeglądu. Z drugiej strony menedżerów, którzy nie mają tego uprawnienia i użytkowników, którzy nie są menedżerów, ale ma te uprawnienia są zgłaszane jako niezgodne.

Jak podano wcześniej, można łączyć zasady w ruleset, co ułatwi do przeprowadzania kategoryzacji i Zarządzaj regułami, aby spełnić wymagania biznesowe.

Można również zdefiniować globalnego zestaw filtrów, które, gdy włączone, są stosowane do reguły jest testowana. Jeśli potrzebujesz często wykluczenia konkretnego podzestawu rekordów podczas testowania reguł w różnych zestawów reguł, można określić filtry globalne, które można włączyć lub wyłączyć zgodnie z potrzebami.

## <a name="reporting"></a>Raportowanie

Moduł raportowania BHOLD daje możliwość wyświetlania informacji w modelu roli przy użyciu szerokiej gamy raportów. Moduł raportowania BHOLD udostępnia wiele wbudowanych raportów, a także zawiera kreatora, który służy do tworzenia niestandardowych raportów podstawowe i zaawansowane. Po uruchomieniu raportu, można natychmiast wyświetlić wyniki lub zapisać wyniki w pliku programu Microsoft Excel (xlsx). Aby wyświetlić ten plik za pomocą programu Microsoft Excel 2000, programu Microsoft Excel 2002 lub Microsoft Excel 2003, można pobrać i zainstalować pakiet Microsoft Office zgodności dla programu Word, Excel i PowerPoint formaty plików.


Możesz użyć modułu raportowania BHOLD przede wszystkim do tworzenia raportów, które są oparte na bieżących informacji o roli. Do generowania raportów inspekcji zmiany informacji o tożsamości programu Forefront Identity Manager 2010 R2 ma możliwości raportowania usługi FIM, która jest zaimplementowana w magazynie danych programu System Center Service Manager. Aby uzyskać więcej informacji na temat raportowania FIM w dokumentacji programu Forefront Identity Manager 2010 i Forefront Identity Manager 2010 R2 w bibliotece technicznej Forefront Identity Management.

Kategorie objęte wbudowanych raportów są następujące:

- Administration
- Poświadczenie
- Formanty
- Kontrola dostępu czynnego
- Rejestrowanie
- Model
- Statystyki
- Przepływ pracy

Można tworzyć raporty i dodaj je do tych kategorii, lub można określić własne kategorie, w których można umieścić raportów wbudowanych i niestandardowych.

Jak utworzyć raport, Kreator przeprowadza użytkownika przez podając następujące parametry:

- Informacje identyfikacyjne, w tym nazwę, opis, kategoria, użycia i odbiorców
- Pola, które mają być wyświetlane w raporcie
- Zapytania, które określają, elementy, które mają być analizowane
- Kolejność, w którym mają być sortowane wierszy
- Pola używane do dzielenia na sekcje raportu
- Filtry w celu uściślenia elementy, które są zwracane w raporcie

Na każdym etapie kreatora można wyświetlić podgląd raportu, zgodnie z ustawieniami wykonanej do tej pory i zapisz go, jeśli nie musisz określić dodatkowe parametry. Można także cofnąć do poprzednich kroków do zmiany parametrów określonych wcześniej w kreatorze.

## <a name="access-management-connector"></a>Łącznik zarządzania dostępu

Moduł łącznika zarządzania dostępu pakiet BHOLD obsługuje zarówno początkowy, jak i trwającej synchronizacji danych do BHOLD. Łącznik zarządzania dostępu współpracuje z usługę synchronizacji programu FIM do przenoszenia danych między baza danych BHOLD Core MIM metaverse i aplikacji docelowej i magazyny tożsamości.

Poprzednie wersje BHOLD wymagane wielu MAs do sterowania przepływem danych między usługą MIM i tabel bazy danych BHOLD pośrednich. Pakiet BHOLD z dodatkiem SP1 łącznika zarządzania dostępu pozwala zdefiniować agentów zarządzania (MAs) w programie MIM, które zapewniają transferu danych bezpośrednio między BHOLD i MIM.

## <a name="mim-integration"></a>Integracja MIM

Ważne i zaawansowanych funkcji programu Forefront Identity Manager 2010 i Forefront Identity Manager 2010 R2 jest portalu samoobsługowego, który umożliwia użytkownikom końcowym wyświetlanie i zarządzanie informacjami o ich tożsamości i członkostwa. Integracja MIM rozszerza portalu programu MIM z samoobsługi roli zarządzania. Na przykład za pomocą funkcji BHOLD w portalu MIM, użytkownik może wysłać żądanie dotyczące przypisania roli i można wyświetlić aktywnych ról i oczekujących żądań. Dodatkowe funkcje mogą być przyznane administratorów delegowanych, takie jak możliwość przypisania ról żądania dla innych użytkowników.

Należy pamiętać, że rozszerzenia BHOLD z portalem programu MIM obsługuje samoobsługi roli i przepływu pracy zarządzania i raportowania jest. Inne funkcje administracji BHOLD, a także zaświadczenie, są udostępniane przez portali sieci web modułów BHOLD, które są obsługiwane oddzielnie z portalu programu MIM.

## <a name="next-steps"></a>Następne kroki

- [Przewodnik instalacji BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
