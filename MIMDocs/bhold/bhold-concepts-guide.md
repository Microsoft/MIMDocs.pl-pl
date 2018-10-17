---
title: Przewodnik pojęcia pakietu BHOLD firmy Microsoft | Dokumentacja firmy Microsoft
description: Rozpoczęcie pracy ze składnikami programu MIM 2016 przez instalację i konfigurację usługi synchronizacji.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 32bd77140cf70047eaa02d363a1348e73783f87a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358843"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Przewodnik pojęcia pakietu BHOLD firmy Microsoft

Microsoft Identity Manager 2016 (MIM) umożliwia organizacjom zarządzanie w całym cyklu życia tożsamości użytkowników i ich skojarzone poświadczenia. Może on zostać skonfigurowany do synchronizacji tożsamości, centralne zarządzanie certyfikatami i haseł i zainicjowanie obsługi administracyjnej użytkowników we wszystkich systemach heterogenicznych. Z programem MIM organizacje z branży IT można definiować i automatyzować procesy używane do zarządzania tożsamościami od tworzenia po ewentualne wycofanie.

Pakiet BHOLD Microsoft rozszerza te możliwości programu MIM, dodając kontroli dostępu opartej na rolach. BHOLD umożliwia organizacjom do definiowania ról użytkowników oraz kontrolować dostęp do poufnych danych i aplikacji. Dostęp opiera się co to jest odpowiedni dla tych ról. Pakiet BHOLD obejmuje, usługami i narzędziami, które upraszczają modelowania relacji roli w organizacji. BHOLD mapuje tych ról, prawa i sprawdź, czy definicje ról i skojarzonymi prawami są prawidłowo stosowane do użytkowników. Te funkcje są w pełni zintegrowane z programem MIM, zapewniając bezproblemowe środowisko dla użytkowników końcowych i informatyków podobne.

Ten przewodnik pomaga zrozumieć, jak pakietu BHOLD współpracuje z usługą MIM i obejmuje następujące tematy:

- Kontrola dostępu oparta na rolach
- Poświadczenie
- Analiza
- Raportowanie
- Dostęp do łącznika zarządzania
- Integracja programu MIM

## <a name="role-based-access-control"></a>Kontrola dostępu oparta na rolach

To najbardziej typowa metoda kontroli dostępu użytkownika do danych i aplikacji za pośrednictwem arbitralnej kontroli dostępu (DAC). W najbardziej typowych implementacjach każdy obiekt znaczące ma zidentyfikowanych właściciela. Właściciel ma możliwość udzielania lub odmawiania dostępu do obiektu, aby inne osoby na podstawie indywidualne tożsamości lub członkostwo w grupie. W praktyce DAC zwykle powoduje mnóstwo grup zabezpieczeń, niektóre, które odzwierciedlają strukturę organizacyjną, inne osoby, które reprezentują funkcjonalnych grup (na przykład typy zadań lub przydziałów projektu) i innych, które składają się z makeshift kolekcji użytkowników i urządzenia, które są połączone więcej celów tymczasowych. Wraz z potrzebami organizacji, członkostwo w tych grupach staje się coraz trudniejsze do zarządzania. Na przykład jeśli pracownik został przeniesiony z jednego projektu do innego, grup, które są używane do kontrolowania dostępu do zasobów projekty muszą być aktualizowane ręcznie. W takich przypadkach nie jest niczym niezwykłym błędy występują błędy, które może utrudniać zabezpieczenia projektu lub wydajność.

Program MIM zawiera funkcje, które pomagają uniknąć tego problemu, zapewniając automatyczne kontrolę nad członkostwa grupy i dystrybucji na liście. Jednak nie eliminuje złożoność wewnętrzne rozmnażające grup, które nie są zawsze powiązane ze sobą w taki sposób, ze strukturą.

Jednym ze sposobów jest znacznie krótszy to rozprzestrzenianie jest wdrażanie kontroli dostępu opartej na rolach (RBAC). RBAC wykluczają DAC.  RBAC opiera się na aplikacji DAC, zapewniając strukturę do klasyfikowania użytkowników i zasoby informatyczne. Dzięki temu, że jawne ich relacje i prawa dostępu, które są odpowiednie, zgodnie z danej klasyfikacji. Na przykład przypisując go do użytkownika stanowisko atrybuty określające użytkowników i przydziałów projektu, użytkownik może otrzymać dostęp do narzędzia potrzebne do użytkownika zadania i dane, które użytkownik chce przyczyniają się do określonego projektu. Po użytkownik przyjmuje różne zadania i przypisania innego projektu, zmiana atrybutów, które określają stanowisko użytkownika i projektów automatycznie blokuje dostęp do zasobów wymagane tylko dla użytkowników poprzedniej pozycji.

Ponieważ role mogą być zawarte w ramach ról w hierarchiczny sposób (na przykład role kierownikiem ds. sprzedaży i przedstawicielem handlowym firmy mogą być zawarte w bardziej ogólnych roli sales), można łatwo przypisać odpowiednie uprawnienia do określonych ról i nadal jeszcze zapewniają odpowiednie uprawnienia do wszystkich użytkowników, którzy udostępnia również bardziej włącznie roli. Na przykład w szpitalu, wszyscy członkowie personelu medycznych może mieć prawo, aby wyświetlić rekordy pacjentów, ale lekarzy (rolą podrzędną rolę medycznych) może mieć prawo do wprowadzania wymagań dla pacjenta. Podobnie użytkownicy należący do roli maszynowy może mieć dostępu do kartoteki pacjentów, z wyjątkiem rozliczeń urzędników (rolą podrzędną rolę biurowych), którzy mogą uzyskać dostęp do tych obszarach rekordy pacjentów, które są wymagane do rozliczania pacjenta dla usług udostępniane przez szpitala.

Dodatkową zaletą RBAC jest możliwość definiujące i wymuszające rozdzielenia obowiązków (darni). Dzięki temu organizacja zdefiniować kombinacje ról, które udzielić uprawnień, które nie powinny być przechowywane przez tego samego użytkownika, aby dany użytkownik nie może przypisać role, które umożliwiają użytkownikom, aby zainicjować płatności, a autoryzowanie płatności, na przykład. RBAC zapewnia możliwość automatycznie wymuszać taką zasadę, niż musieć go obliczyć skutecznego stosowania zasad dla poszczególnych użytkowników.

### <a name="bhold-role-model-objects"></a>Obiekty modelu roli pakietu BHOLD

Za pomocą pakietu BHOLD można określić i organizowanie ról w organizacji, mapa użytkowników do ról i mapy odpowiednich uprawnień do ról. Ta struktura jest nazywany model roli i zawiera i łączy się z pięciu typów obiektów: 

- Jednostki organizacyjne
- Users
- Role
- Uprawnienia
- Aplikacje

#### <a name="organizational-units"></a>Jednostki organizacyjne

Jednostki organizacyjne (OrgUnits) są podmiotu zabezpieczeń oznacza, że za pomocą którego użytkownicy są zorganizowane w modelu roli BHOLD. Każdy użytkownik musi należeć do co najmniej jeden OrgUnit. (W rzeczywistości po usunięciu użytkownika z ostatnich jednostki organizacyjnej w BHOLD użytkownika danych rekord zostanie usunięty z bazy danych BHOLD.)

> [!Important]
> Jednostki organizacyjne w modelu roli pakietu BHOLD nie należy mylić z jednostkami organizacyjnymi w usługach domenowych w usłudze Active Directory (AD DS). Zazwyczaj struktury jednostek organizacyjnych w BHOLD zależy od organizacji i zgodnie z zasadami firmy, nie wymagania dotyczące infrastruktury sieciowej.

Chociaż nie jest wymagane, jednostki organizacyjne są w większości przypadków umieszczonymi w BHOLD do reprezentowania struktury hierarchicznej organizacji rzeczywiste, podobny do przedstawionego poniżej:

![](media/bhold-concepts-guide/org-chart.png)

W tym przykładzie model roli będzie organizationalganizatinal jednostkę corporation jako całość (reprezentowany przez Prezes, ponieważ Prezes nie jest częścią zestawu mororganizationalganizatinal) lub jednostki organizacyjnej głównym pakietu BHOLD (co zawsze istnieje) może służyć do tego celu. OrgUnits reprezentujący oddziałów firmy czele prezydentów Wiceprezes zostają umieszczone w jednostce organizacyjnej firmowych. Następny, organizacji jednostki odpowiadający dyrektorów sprzedaży i marketingu zostaną dodane do jednostki organizacyjnej sprzedaży i marketingu i jednostek organizacyjnych, reprezentujący regionalnych menedżerowie ds. sprzedaży zostanie umieszczony w jednostce organizacyjnej dla wschodnim regionie kierownikiem ds. sprzedaży. Kojarzy sprzedaży, którzy nie zarządzają innych użytkowników, nastąpi elementów członkowskich jednostki organizacyjnej z kierownikiem ds. sprzedaży w regionie wschodnim. Należy pamiętać, że użytkownicy mogą być członkami jednostki organizacyjnej, na dowolnym poziomie. Na przykład Asystenta administracyjnego, który nie jest menedżera i raporty bezpośrednio Wiceprezes, będzie należeć do jednostki organizacyjnej wiceprezes ds.

Oprócz reprezentowania struktury organizacyjnej, jednostek organizacyjnych może również do grupy użytkowników i innych jednostek organizacyjnych, zgodnie z kryteriami funkcjonalności, takie jak dla projektów lub specjalizacji. Na poniższym diagramie przedstawiono sposób organizacji jednostki może służyć do grupowania kojarzy sprzedaży, zależnie od typu klienta:

![](media/bhold-concepts-guide/org-chart-02.png)

W tym przykładzie każdy pracownik sprzedaży będzie należeć do dwóch jednostek organizacyjnych: jeden reprezentujący skojarzenia miejsce w strukturze zarządzania Twojej organizacji i jeden reprezentujący bazy klientów skojarzenia (sprzedaży lub firmowe). Każda jednostka organizacyjna można przypisać różne role, które z kolei mogą być przypisane różne uprawnienia do uzyskiwania dostępu do organizacji zasobów informatycznych. Ponadto role mogą być dziedziczone z nadrzędnego jednostek organizacyjnych, upraszczając proces propagowanie ról do użytkowników. Z drugiej strony określonych ról nie są dziedziczone, zapewniając, że określonej roli są skojarzone tylko z odpowiednich jednostek organizacyjnych.

Za pomocą portalu sieci web pakietu BHOLD Core lub za pomocą generatora modeli pakietu BHOLD OrgUnits można utworzyć pakietu BHOLD Suite.

#### <a name="users"></a>Users

Jak wspomniano powyżej, każdy użytkownik musi należeć do co najmniej jedną jednostkę organizacyjną (OrgUnit). Ponieważ jednostki organizacyjne są jednostki mechanizm kojarzenia użytkownika z ról, w większości organizacji dany użytkownik należy do wielu OrgUnits, aby ułatwić Skojarz role z użytkownikiem. W niektórych przypadkach jednak może być konieczne do skojarzenia z roli użytkownika, niezależnie od wszelkich OrgUnits, której należy użytkownik. W związku z tym użytkownikowi można przypisać bezpośrednio do roli, a także uzyskiwania ról z OrgUnits, do której należy użytkownik.

Gdy użytkownik nie jest aktywny w organizacji (trochę natychmiast medycznych urlopu, na przykład), użytkownik może zostać zawieszone, która odwołuje wszystkie uprawnienia użytkownika bez usuwania użytkownika z roli modelu. Po powrocie do podatku, użytkownik może ponownie uaktywnić, przywraca wszystkie uprawnienia przyznane przez bieżącego użytkownika.

Obiekty użytkowników można tworzyć osobno w pakietu BHOLD za pośrednictwem portalu sieci web Core pakietu BHOLD lub importowane zbiorczo za pomocą generatora modeli pakietu BHOLD lub za pomocą łącznika zarządzania dostęp za pomocą usługi synchronizacji programu FIM do importowania informacji o użytkowniku z źródeł, takich jak Active Directory Domain Services lub zarządzania zasobami ludzkimi aplikacji.

Użytkownicy mogą być tworzone bezpośrednio w BHOLD bez korzystania z programu FIM Synchronization Service. Może to być przydatne, gdy modelowania ról w środowisku przedprodukcyjnym lub testowym środowisku. Możesz również zezwolić użytkowników zewnętrznych (na przykład pracownicy podwykonawcy) może być przypisane role i dlatego wydane dostępu do zasobów informatycznych, bez dodawane do bazy danych pracownika. Jednak te użytkownicy nie będą mogli korzystać z funkcji samoobsługi BHOLD.

#### <a name="roles"></a>Role

Jak wspomniano wcześniej, w ramach modelu kontroli dostępu opartej na rolach uprawnienia są skojarzone z rolami, a nie poszczególnych użytkowników. Umożliwia ona każdemu użytkownikowi zapewnić uprawnień wymaganych do wykonywania obowiązków użytkownika, zmieniając role użytkownika, zamiast oddzielnie udzielanie lub odmawianie uprawnień użytkownika. W konsekwencji przypisanie uprawnień nie wymaga już udział działu IT, ale zamiast tego mogą być wykonywane w ramach zarządzania firmy. Rola może agregować uprawnienia dostępu do różnych systemów, bezpośrednio lub za pomocą podrzędne, co dodatkowo ogranicza potrzebę dodatkowych zasobów informatycznych w zarządzanie uprawnieniami użytkowników.

Jest ważne, należy pamiętać, że role są funkcją modelu RBAC. Zazwyczaj role nie są udostępnione do aplikacji docelowej. Dzięki temu funkcji RBAC w usłudze można używać razem z istniejących aplikacji, które nie są zaprojektowane role lub zmień rolę definicje potrzeb zmiana modeli biznesowych bez konieczności modyfikowania same aplikacje. Jeśli aplikacja docelowa jest przeznaczony do stosowania ról, następnie można skojarzone role w modelu roli BHOLD przy użyciu odpowiednich ról aplikacji przez traktowanie ról specyficzne dla aplikacji jako uprawnień.

W BHOLD możesz przypisać rolę użytkownikowi, głównie za pośrednictwem dwóch mechanizmów:

- Przez przypisanie roli organizacyjnej jednostki (jednostki organizacyjnej), której członkiem jest użytkownik
- Przypisanie roli bezpośrednio do użytkownika

Rola przypisana do nadrzędnej jednostki organizacyjnej opcjonalnie mogą być dziedziczone przez jej elementu członkowskiego jednostek organizacyjnych. Gdy rola przypisana do lub jest dziedziczona przez jednostkę organizacyjną, może być wyznaczony jako skuteczne lub proponowanej roli. Jeśli efektywną rolę, wszyscy użytkownicy w tej jednostce organizacyjnej przypisano rolę. Jeśli jest to rola proponowane, musi być aktywowana dla każdego użytkownika lub elementu członkowskiego jednostki organizacyjnej przestanie obowiązywać dla tego użytkownika lub elementów członkowskich jednostki organizacyjnej. Dzięki temu można przypisać użytkowników z podzbiorem ról skojarzony z jednostką organizacyjną, a nie automatyczne przypisywanie wszystkich ról jednostki organizacyjnej do wszystkich elementów członkowskich. Ponadto można podać ról daty rozpoczęcia i zakończenia i limity można umieścić na procencie użytkowników w ramach jednostki organizacyjnej, dla której rolą może być skuteczne.

Na poniższym diagramie przedstawiono, jak użytkownik może mieć przypisaną rolę w BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Na poniższym diagramie roli A jest przypisany do jednostki organizacyjnej jako rola dziedziczone, a więc jest dziedziczona przez jej elementu członkowskiego jednostki organizacyjne i wszyscy użytkownicy w tych jednostkach organizacyjnych. Jako proponowane rola przypisano rolę B dla jednostki organizacyjnej. Musi zostać aktywowana, aby użytkownik w jednostce organizacyjnej, może być autoryzowane przy użyciu uprawnień w ramach roli. Rola języka C jest efektywną rolę, dzięki czemu jego uprawnienia natychmiast zastosowane do wszystkich użytkowników w jednostce organizacyjnej. Rola D jest połączone bezpośrednio do użytkownika, i dlatego jego uprawnienia natychmiast zastosowane do tego użytkownika.

Ponadto można aktywować rolę dla użytkownika na podstawie atrybutów użytkownika. Aby uzyskać więcej informacji zobacz autoryzacja na podstawie atrybutu.

#### <a name="permissions"></a>Uprawnienia

Dla uprawnienia w BHOLD odnosi się do autoryzacji importowane z aplikacji docelowej. Oznacza to gdy BHOLD jest skonfigurowane do pracy z aplikacją, otrzymuje listę autoryzacji, które BHOLD można połączyć z ról. Na przykład po dodaniu usługi Active Directory Domain Services (AD DS) do pakietu BHOLD jako aplikacji otrzymuje listę grup zabezpieczeń, które jako BHOLD uprawnienia, mogą być połączone do ról w BHOLD.

Uprawnienia są specyficzne dla aplikacji. BHOLD zapewnia jednym, ujednoliconym widoku uprawnienia, dzięki czemu można kojarzyć uprawnienia z rolami bez konieczności roli menedżerów poznać szczegóły implementacji uprawnienia. W praktyce różnych systemów może wymuszać uprawnienia inaczej. Łącznik specyficzne dla aplikacji z programu FIM Synchronization Service do aplikacji określa, jak zmiany uprawnień dla użytkownika są dostarczane do tej aplikacji. 

#### <a name="applications"></a>Aplikacje

BHOLD implementuje metodę stosowania kontroli dostępu opartej na rolach (RBAC) do aplikacji zewnętrznych. Oznacza to gdy BHOLD był zaopatrzony użytkowników i uprawnień z aplikacji, pakietu BHOLD można skojarzyć te uprawnienia użytkownikom przez przypisywanie ról do użytkowników, a następnie łącząc uprawnienia do ról. Proces w tle aplikacji może być przeprowadzenie mapowania odpowiednich uprawnień do jego użytkowników na podstawie mapowania uprawnienia/roli w BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Tworzenie modelu roli pakietu BHOLD

Aby pomóc w tworzeniu modelu ról, pakietu BHOLD udostępnia Generator modelu, który jest łatwy w użyciu i wszechstronne narzędzie.

Przed użyciem Generator modelu, należy utworzyć szereg pliki definiujące obiekty, które Generator modelu używa do konstruowania modelu roli. Aby uzyskać informacje dotyczące sposobu tworzenia tych plików Zobacz dokumentacja techniczna pakietu BHOLD firmy Microsoft.

Pierwszym krokiem przy użyciu generatora modeli pakietu BHOLD jest importowanie tych plików, aby załadować podstawowe bloki konstrukcyjne do Generator modelu. Gdy pliki zostały pomyślnie załadowane, możesz określić kryteria, które używa Generator modelu w celu utworzenia kilka rodzajów ról:

- Ról członkostwa, które są przypisane do użytkownika w oparciu OrgUnits (jednostki organizacyjnej), której należy użytkownik
- Atrybut role, które są przypisane do użytkownika, na podstawie atrybutów użytkownika w bazie danych pakietu BHOLD
- Proponowana role, które są połączone z jednostką organizacyjną, ale musi być aktywowana dla konkretnych użytkowników
- Przyznaj użytkownikowi kontrolę nad jednostkach organizacyjnych i role, których właścicielem nie jest określony w importowanych plików role własności

> [!Important]
> Po przekazaniu plików wybierz **zachować istniejący Model** pole wyboru tylko w środowiskach testowych. W środowiskach produkcyjnych należy użyć Generator modeli do utworzenia początkowej modelu roli. Nie można używać go, aby zmodyfikować istniejący model roli w bazie danych BHOLD.

Po utworzeniu Generator modeli tych ról w modelu ról, można następnie wyeksportować model roli, aby baza danych BHOLD w postaci pliku XML.

### <a name="advanced-bhold-features"></a>Zaawansowane funkcje pakietu BHOLD

Przedstawione w poprzednich sekcjach opisano podstawowe funkcje kontroli dostępu opartej na rolach (RBAC) w BHOLD. W tej sekcji opisano dodatkowe funkcje pakietu BHOLD, która może zapewnić większe bezpieczeństwo i elastyczność implementacji RBAC w Twojej organizacji. Ta sekcja zawiera omówienie następujących funkcji BHOLD:

- Kardynalność
- Podział obowiązków
- Kontekst potężnej uprawnień
- Autoryzacja na podstawie atrybutu
- Typy elastyczne atrybutów


#### <a name="cardinality"></a>Kardynalność

*Kardynalność* odwołuje się do stosowania reguł biznesowych, które są przeznaczone do ograniczenia liczby dwie jednostki mogą być powiązane ze sobą. W przypadku pakietu BHOLD reguły Kardynalność może zostać nawiązana ról, uprawnienia i użytkowników.

Można skonfigurować role, aby ograniczyć następujące czynności:

- Maksymalna liczba użytkowników, dla których można aktywować rolę proponowanego
- Maksymalna liczba podrzędne, które mogą być połączone do roli
- Maksymalna liczba uprawnienia, które mogą być połączone do roli

Można skonfigurować uprawnienia, aby ograniczyć następujące czynności:

- Maksymalna liczba ról, które mogą być połączone do uprawnień
- Maksymalna liczba użytkowników, którym można udzielić uprawnienia

Można skonfigurować użytkownika, aby ograniczyć następujące czynności:

- Maksymalna liczba ról, które mogą być połączone do użytkownika
- Maksymalna liczba uprawnień przypisanych do użytkownika za pomocą przypisań ról

#### <a name="separation-of-duties"></a>Podział obowiązków

Rozdzielenie obowiązków (darni) to zasady biznesowe, że stara, aby uniemożliwić osobom możliwość wykonywania akcji, które nie powinny być dostępne dla jednej osoby. Na przykład pracownik powinna nie można zażądać płatności i autoryzowanie płatności. Zasady darni umożliwia organizacjom Implementowanie system kontroli i zrównoważenia, aby zminimalizować ich narażenia na zagrożenia z błąd pracownika lub naruszenie.

BHOLD implementuje darni, umożliwiając Definiowanie niezgodne uprawnień. Gdy te uprawnienia są zdefiniowane, BHOLD wymusza darni zapobieganie tworzeniu role, które są połączone z niezgodną uprawnień, czy są połączone bezpośrednio lub poprzez dziedziczenie i poprzez uniemożliwienie użytkownikom miałyby przypisać wiele ról, gdy w połączeniu, przyznano uprawnienia niezgodne ponownie przypisania bezpośredniego lub przez dziedziczenie. Opcjonalnie można przesłonić, konflikty.

#### <a name="context-adaptable-permissions"></a>Kontekst potężnej uprawnień

Tworząc uprawnienia, które mogą być automatycznie modyfikowane na podstawie atrybutu obiektu, można zmniejszyć całkowita liczba uprawnienia, które trzeba zarządzać. Kontekst potężnej uprawnienia (CAP) pozwalają zdefiniować formułę jako atrybut uprawnień, który modyfikuje sposób stosowania uprawnienia aplikacji skojarzonych z uprawnieniem. Na przykład można utworzyć formułę, która zmienia uprawnienia dostępu do folderu plików (za pośrednictwem grupy zabezpieczeń skojarzone z listy kontroli dostępu folderu) czy użytkownik należy do organizacji jednostki (jednostki organizacyjnej) zawierającej pełnym wymiarze czasu lub umowy pracowników. Jeśli użytkownik zostanie przeniesiony z jednej jednostki organizacyjnej do innego, nowe uprawnienie jest automatycznie stosowana i starego uprawnienie jest zdezaktywowane. 

Formuła zakończenia można badać wartości atrybutów, które zostały zastosowane do aplikacji, uprawnienia, jednostki organizacyjne i użytkowników.

#### <a name="attribute-based-authorization"></a>Autoryzacja na podstawie atrybutu

Jednym ze sposobów formant czy rolę, która jest połączona z jednostką organizacyjną (jednostki organizacyjnej) aktywowano dla określonego użytkownika w jednostce organizacyjnej jest użycie autoryzacji opartych na atrybutach (ABA). Za pomocą ABA, można automatycznie aktywować rolę, tylko wtedy, gdy są spełnione pewne reguły na podstawie atrybutów użytkownika. Na przykład można połączyć roli do jednostki organizacyjnej, która stanie się aktywny dla użytkownika tylko wtedy, gdy stanowisko użytkownika odpowiada stanowisko w regule ABA. Eliminuje potrzebę ręcznie uaktywnić proponowane rolę dla użytkownika. Zamiast tego można aktywować rolę wszystkich użytkowników w jednostce organizacyjnej, którzy mają wartość atrybutu, który spełnia reguły ABA ról. Zasady można łączyć tak, że rola jest aktywowany tylko wtedy, gdy atrybuty użytkownika spełniać wszystkie reguły ABA określone dla roli.

Należy pamiętać, wyniki testów reguły ABA są ograniczeni ustawienia kardynalności. Na przykład jeśli ustawienie kardynalności reguły określa, że nie więcej niż dwóch użytkowników można przypisać roli i regułę ABA w przeciwnym razie może aktywować rolę dla czterech użytkowników, roli zostaną uaktywnione tylko w przypadku dwóch pierwszych użytkowników, którzy przeszedł ABA test.

#### <a name="flexible-attribute-types"></a>Typy elastyczne atrybutów

System atrybutów w BHOLD jest wysoce rozszerzalny. Można zdefiniować nowe typy atrybutów dla takich obiektów jako użytkownicy organizacji jednostki (jednostki organizacyjnej) i ról. Atrybuty można zdefiniować wartości, które są liczbami całkowitymi, atrybut typu wartość logiczna (tak/nie), litery, cyfry, daty, godziny i wiadomości e-mail adresów. Atrybuty można określić jako pojedyncze wartości lub listy wartości.

## <a name="attestation"></a>Poświadczenie

Pakiet BHOLD zawiera narzędzia, których można użyć, aby sprawdzić, czy poszczególnych użytkowników ma odpowiednie uprawnienia do wykonywania ich zadań biznesowych. Administrator można użyć portalu, dostarczone przez moduł zaświadczania pakietu BHOLD projektować Zarządzaj procesu zaświadczania.

Proces zaświadczania odbywa się za pomocą kampanie, w których kampanii stewards jest możliwość i oznacza, że sprawdzić, czy użytkownicy, dla których są one odpowiedzialne mają odpowiedni dostęp do aplikacji zarządzanych BHOLD i odpowiednie uprawnienia w ramach te aplikacje. Nadzoruj kampanii i upewnij się, że kampanii jest przeprowadzana prawidłowo nazw jest wyznaczony właściciel kampanii. Występuje raz lub cykliczne można utworzyć kampanię.

Zazwyczaj steward kampanii będzie menedżera, który potwierdza prawa dostępu użytkowników należących do jednego lub więcej jednostek organizacyjnych, za które odpowiada menedżera. Stewards automatycznie wybiera się dla użytkowników, że zaświadczenia w ramach kampanii, na podstawie atrybutów użytkownika lub przez wymienienie ich w pliku, który mapuje każdego użytkownika jest potwierdzone w kampanii do steward można zdefiniować stewards dla kampanii.

Po rozpoczęciu kampanii BHOLD wysyła wiadomość e-mail z powiadomieniem do stewards kampanii i właściciela, a następnie wysyła okresowe przypomnienia, aby pomóc im Obsługa postęp w kampanii. Stewards nastąpi przekierowanie do portalu kampanii, w którym można zobaczyć listę użytkowników, dla których są one odpowiedzialne i role, które są przypisane do tych użytkowników. Stewards następnie można potwierdzić, czy ich są odpowiedzialni za każdy z listy użytkowników i Zatwierdź lub Odrzuć prawa dostępu do każdego z listy użytkowników.

Właściciele kampanii również korzystać z portalu do monitorowania postępu kampanii i działań marketingowych są rejestrowane, dzięki czemu właściciele kampanii można analizować akcje, które zostały wykonane w ramach kampanii.

## <a name="analytics"></a>Analiza

Jedną z istotne zagadnienia po wdrożenie systemu kontroli łącznie z kompleksowym dostępem na podstawie praw równowagę między zachowaniem kontroli dostępu i unikanie niepotrzebnych (lub niższa, nieoczekiwany) barier w celu uzyskania dostępu do. Nakład pracy w celu zachowania tej równowagi często powoduje w strukturze kontroli dostępu, więc skomplikowanego, że nieoczekiwany interakcje między zasady są prawie nieuniknione.

Z tego powodu należy mieć możliwość analizowania wpływu zasady kontroli dostępu, zanim zostaną one faktycznie w miejscu. Moduł analizy pakietu BHOLD zapewnia możesz możliwość wykonywania tej analizy, co pozwala na tworzenie reguł, które reprezentują zasad, a następnie przedstawiające użytkowników, którego uprawnienia są zgodne lub konflikt z regułą. Na podstawie tej analizy, można dostosować zasady lub zmodyfikować role i uprawnienia w celu wyeliminowania konfliktów z zasadami.

Portal analizy BHOLD oferuje możliwość skonstruowania zestawów reguł, który składa się z co najmniej jedną regułę, które tworzysz do testowania konkretne zasady lub grupa zasad. Reguła oświadczeń składa się z następujących głównych składników:

- Tytuł i opis, które umożliwiają identyfikujących i opisujących regułę
- Stan, który wskazuje, czy reguła jest gotowa do przeglądu, są przeglądane lub zatwierdzone
- Element ustawiony (na przykład użytkowników lub uprawnienia), reguła została opracowana w celu przetestowania
- Filtry podzbioru opcjonalne, które są wyrażeniami, które można użyć, aby zaznaczyć podgrupę odpowiedniego elementu do badania
- Co najmniej jeden filtr reguły, które są wyrażeniami, które reprezentują zasad poddawana testom.

Regułę można przetestować dowolne spośród następujących zestawów elementów:

- Users
- Jednostki organizacyjne
- Role
- Uprawnienia
- Aplikacje
- Konta

Na poniższym diagramie przedstawiono prostej reguły składające się z dwóch podzbiór reguł i dwie reguły filtru:

![](media/bhold-concepts-guide/rules.png)

Należy zanotować różnicę w efekcie przechodzenie w filtrze podzestawu i kończy się niepowodzeniem filtr reguły: niepowodzenie filtrowania podzbioru usuwa obiekt elementu z testowanie przez kolejne filtry, gdy kończy się niepowodzeniem filtr reguły powoduje, że obiekt jest raportowane jako niezgodne. Te obiekty, które pomyślnie przejść wszystkie filtry podzbioru i wszystkie filtry reguły są raportowane jako zgodne.

Każdy filtr składa się z typem, operatora (jest to zależne od typu), klucz (jeden z elementów) i wartości, względem którego klucza jest testowane przez operatora. Na przykład następujący filtr może sprawdzić, czy liczba użytkowników w podzbiorze elementu przekracza 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Typ:**   | Liczba   |
| **Klucz:**  | Users  |
| **Operator**  | >  |
| **Wartość:** | 10 |

Filtry zasad można się z trzech typów i operatory specyficzne dla ich typu, jak wskazano:

- Atrybut
  - < i >
  - = i! =
  - **zawiera**
  - **Nie zawiera**
- Liczba
  - < i >
  - = i! =
- Restrykcyjny
  - **Musi to być dowolny i musi mieć wszystkie**
  - **Nie może zawierać żadnych i nie może mieć wszystko**
  - **Można tylko ma i może mieć tylko wszystkie**
  - **Nie ma żadnej wyłącznie i wyłącznie mają wszystkie**

> [!Note]
> Filtry ograniczające można używać wskazany operatorów, aby przetestować klucza z zestawem wiele wartości.

Na przykład jeśli chcesz przetestować implementacji podziału obowiązków (darni) zasad, stwierdzający nie użytkownika, który ma uprawnienia do żądania płatności jest również mieć uprawnienie zatwierdzić płatności, można utworzyć reguły podobne do następującego:

|   |  |
|---|--|
|Nazwa:| Test darni płatności|
|Element:| Users|
|Filtr podzbioru:| Wszelkie uprawnienia żądanie płatności|
|Reguła filtru: | Nie może mieć żadnych uprawnień Zatwierdź płatności|

Po uruchomieniu tej reguły, moduł analizy BHOLD Wyświetla liczbę użytkowników w wybranej podzestawu (liczba użytkowników z uprawnieniem żądanie płatności), liczbę użytkowników, które są zgodne z regułą i liczbę użytkowników, które nie są zgodne z regułą. Następnie można wyświetlić niezgodnych użytkowników, dzięki czemu można poprawić niespójność.

Oprócz wyświetlania wyników, można również Zapisz raport jako wartości rozdzielanych przecinkami (CSV) lub plik XML, aby możliwe było później analizować wyniki. Można również dostosować wynikowego raportu, aby wyświetlić dodatkowe informacje, które mogą pomóc lepiej zrozumieć wpływ. Na przykład jeśli testujesz użytkowników, możesz wyświetlać (lub zgłosić) kont użytkowników zgodnych i niezgodnych, aby było widać aplikacji, które są zaangażowane.

Reguła może zawierać wiele filtrów, możesz połączyć z filtrów, aby sprawdzić czy istnieją określonej kombinacji warunków. Domyślnie wynik jest wynikiem testu logicznego wszystkich filtrów, ale można określić, czy można przeprowadzić testu lub kombinacji filtru.

Na przykład zasady firmy wymagają menedżerom mają uprawnienia do modyfikowania płatności lub uprawnienia do zatwierdzania płatności, następnie można sprawdzić zgodność z zasadami tworząc reguły podobne do następującego:


|  |  |
|--|--|
|Nazwa: | Zmodyfikuj Test darni płatności|
|Element: | Users |
|Filtr podzbioru: | Posiadanie żadnej roli menedżera|
| Reguły filtrów: |Musi mieć żadnych uprawnień Modyfikowanie płatności </br> Musi mieć żadnych uprawnień Zatwierdź płatności|

Domyślnie każdy użytkownik, który jest kierownikiem, kto ma uprawnienia do żądania płatności i zmodyfikować płatności będą raportowane jako zgodne. Zasady wymaga jednak, że menedżera jest albo uprawnienie, nie musi to być zarówno. Aby przetestować rzeczywista zgodność z zasadami, musi być operatora logicznego OR z regułą Określa, czy wszystkie menedżerów, którzy nie mają uprawnień do modyfikowania płatności lub uprawnienie zatwierdzić płatności.

W odróżnieniu od innych operatorów **wyłącznie nie ma żadnej** i **wyłącznie mają wszystkie** operatory wskazują zgodności dla obiektów, które w przeciwnym razie mogłyby być wykluczone przez filtr podzbioru. Na przykład, aby sprawdzić zasady, czy wszyscy menedżerowie (i tylko menedżerowie) ma uprawnienia zatwierdzić przeglądy można utworzyć regułę w następujący sposób:

|  |  |
|--|--|
|Nazwa: | Przegląd zatwierdzenia Test|
|Element: | Users|
| Filtr podzbioru: | Posiadanie żadnej roli menedżera
|Reguła filtru: | Wyłącznie mają przeglądy zatwierdzić wszystkie uprawnienia|

Ta zasada będzie raportowane jako zgodne użytkowników, którzy są menedżerów i zatwierdzić przeglądy uprawnień i użytkowników, którzy nie mają menedżerów i którzy nie mają uprawnienia do zatwierdzania przeglądów. Z drugiej strony menedżerów, którzy mają to uprawnienie, ale bez tego uprawnienia i użytkowników, którzy nie są menedżerowie są zgłaszane jako niezgodne.

Jak wspomniano wcześniej, można łączyć zasady w zestaw reguł, ułatwiając umożliwiające kategoryzowanie i zarządzanie nimi reguły, aby spełnić wymagania biznesowe.

Można również zdefiniować zestaw globalne filtry, po włączeniu, zastosować do dowolnej reguły, który jest testowany. Jeśli potrzebujesz często do wykluczenia konkretnego podzbioru rekordów podczas testowania reguł w różnych zestawów reguł, można określić filtry globalne, które można włączyć lub wyłączyć zgodnie z potrzebami.

## <a name="reporting"></a>Raportowanie

Moduł raportowania pakietu BHOLD daje możliwość wyświetlania informacji w modelu roli przy użyciu szerokiej gamy raportów. Moduł raportowania pakietu BHOLD udostępnia wiele wbudowanych raportów, a także zawiera kreatora, który służy do tworzenia niestandardowych raportów podstawowe i zaawansowane. Po uruchomieniu raportu możesz natychmiast wyświetlić wyniki lub zapisać wyniki w pliku programu Excel (xlsx). Aby wyświetlić ten plik za pomocą programu Microsoft Excel 2000, programu Microsoft Excel 2002 lub Microsoft Excel 2003, można pobrać i zainstalować pakiet zgodności programu Microsoft Office dla programów Word, Excel i formaty plików programu PowerPoint.


Umożliwia moduł raportowania pakietu BHOLD przede wszystkim tworzenia raportów, które są oparte na bieżących informacji o roli. Do generowania raportów dotyczących inspekcji zmiany informacji o tożsamości programu Forefront Identity Manager 2010 R2 ma możliwości raportowania usługi FIM, która jest zaimplementowana w magazynie danych programu System Center Service Manager. Aby uzyskać więcej informacji na temat raportowania usługi FIM zobacz dokumentację programu Forefront Identity Manager 2010 i Forefront Identity Manager 2010 R2 w bibliotece technicznej Forefront Identity Management.

Kategorie objętych wbudowanych raportów są następujące:

- Administration
- Poświadczenie
- Formanty
- Kontrola dostępu czynnego
- Rejestrowanie
- Model
- statystyki
- Przepływ pracy

Można tworzyć raporty i dodawać je do tych kategorii lub można zdefiniować własne kategorie, w których można umieścić niestandardowych i wbudowanych raportów.

Podczas tworzenia raportu, Kreator przeprowadzi Cię przez podanie następujących parametrów:

- Informacje identyfikacyjne, w tym nazwę, opis, kategorii, użycia i odbiorców
- Pola, które mają być wyświetlane w raporcie
- Zapytania określające elementy, które mają być analizowane
- Kolejność, w którym mają być sortowane wierszy
- Pola używane do Podziel raportu na sekcje
- Filtry w celu uściślenia elementy, które są zwracane w raporcie

Na każdym etapie kreatora możesz wyświetlić podgląd raportu zdefiniowano ją do tej pory i zapisz go, jeśli jest konieczne wymagają podania dodatkowych parametrów. Można także cofnąć do poprzednich kroków do zmiany parametrów, które określono wcześniej w kreatorze.

## <a name="access-management-connector"></a>Dostęp do łącznika zarządzania

Moduł BHOLD Suite dostępu do zarządzania łącznika obsługuje zarówno początkowy, jak i bieżące synchronizacji danych do pakietu BHOLD. Dostęp do łącznika zarządzania współpracuje z FIM Synchronization Service do przenoszenia danych między baza danych BHOLD Core, metaverse programu MIM i aplikacjach docelowych i magazyny tożsamości.

Poprzednie wersje pakietu BHOLD wymaga wielu MAs do sterowania przepływem danych między usługą MIM i pośredniego tabel bazy danych BHOLD. W dodatku SP1 dla pakietu BHOLD dostęp do łącznika zarządzania pozwala zdefiniować agentów zarządzania (MAs) w programie MIM, które zapewniają transferu danych bezpośrednio między BHOLD i MIM.

## <a name="mim-integration"></a>Integracja programu MIM

Ważną i zaawansowaną funkcją Forefront Identity Manager 2010 oraz programu Forefront Identity Manager 2010 R2 jest portalu samoobsługowego, która umożliwia użytkownikom końcowym wyświetlanie i zarządzanie informacjami o tożsamości i członkostwa. Integracja programu MIM rozszerza portalu MIM za pomocą funkcji zarządzania rolą samoobsługi. Na przykład za pomocą funkcji BHOLD w portalu programu MIM, użytkownik może zażądać przypisania roli i można wyświetlić aktywnych ról i oczekujące żądania. Dodatkowe funkcje można udzielić do administratorów delegowanych, takie jak możliwość żądania przypisania roli dla innych użytkowników.

Należy pamiętać, że rozszerzenia portalu programu MIM BHOLD obsługuje rolą samoobsługi i zarządzanie przepływem pracy i raportowania jest. Innych funkcji administracyjnych BHOLD, a także w przypadku zaświadczania są dostarczane przez portale sieci web modułów BHOLD, które są obsługiwane osobno z portalu programu MIM.

## <a name="next-steps"></a>Kolejne kroki

- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
