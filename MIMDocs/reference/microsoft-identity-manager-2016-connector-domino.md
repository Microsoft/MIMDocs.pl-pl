---
title: Łącznik programu Lotus Domino | Microsoft Docs
description: W tym artykule opisano sposób konfigurowania łącznika programu Lotus Domino firmy Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/05/2018
ms.author: billmath
ms.openlocfilehash: 46e80bce942b32bce7b7c3fb148973c249136b3f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760583"
---
# <a name="lotus-domino-connector-technical-reference"></a>Dokumentacja techniczna łącznika Lotus Domino
W tym artykule opisano łącznik programu Lotus Domino. Artykuł dotyczy następujących produktów:

* Microsoft Identity Manager 2016 (programie MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Należy użyć poprawki 4.1.3671.0 lub nowszej [KB3092178](https://support.microsoft.com/kb/3092178).

W przypadku programie MIM2016 i FIM2010R2 łącznik jest dostępny do pobrania z [Centrum pobierania Microsoft](https://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Przegląd łącznika programu Lotus Domino
Łącznik programu Lotus Domino umożliwia integrację usługi synchronizacji z serwerem programu Lotus Domino firmy IBM.

Z perspektywy wysokiego poziomu następujące funkcje są obsługiwane przez bieżącą wersję łącznika:

| Cechy | Pomoc techniczna |
| --- | --- |
| Połączone źródło danych |Serwer: <li>Lotus Domino 8,5. x</li><li>Program Lotus Domino 9. x</li>Klient:<li>Lotus Domino 8,5. x</li><li>Lotus Notes 9. x</li> |
| Scenariusze |<li>Zarządzanie cyklem życia obiektów</li><li>Zarządzanie grupami</li><li>Zarządzanie hasłami</li> |
| Operacje |<li>Import pełny i Delta</li><li>Eksportowanie</li><li>Ustawianie i Zmienianie hasła przy haśle HTTP</li> |
| Schemat |<li>Osoba (użytkownik mobilny, kontakt (osoby bez certyfikatu))</li><li>Group (Grupa)</li><li>Zasób (zasoby, pokój, spotkanie online)</li><li>Baza danych poczty</li><li>Dynamiczne odnajdywanie atrybutów dla obsługiwanych obiektów</li><li>Obsługa do 250 niestandardowych wypełnień w organizacji & jednostki organizacyjne (OU)</li> |

Łącznik programu Lotus Domino używa klienta programu Lotus Notes do komunikowania się z serwerem programu Lotus Domino. W związku z tą zależnością na serwerze synchronizacji należy zainstalować obsługiwanego klienta programu Lotus Notes. Komunikacja między klientem a serwerem jest implementowana za pomocą interfejsu programu Lotus Notes .NET Interop (Interop.domino.dll). Ten interfejs ułatwia komunikację między platformą Microsoft.NET a klientem programu Lotus Notes i obsługuje dostęp do dokumentów i widoków programu Lotus Domino. W przypadku importu różnicowego istnieje również możliwość użycia natywnego interfejsu języka C++ (w zależności od wybranej metody importowania różnicowego).

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika upewnij się, że na serwerze synchronizacji są spełnione następujące wymagania wstępne:

* Microsoft .NET 4.5.2 Framework lub nowszy
* Klient programu Lotus Notes musi być zainstalowany na serwerze synchronizacji
* Łącznik programu Lotus Domino wymaga, aby na serwerze katalogu Domino była obecna domyślna baza danych schematu LDAP programu Lotus Domino (schema. NF). Jeśli nie istnieje, można zainstalować ją, uruchamiając lub ponownie uruchamiając usługę LDAP na serwerze Domino.

### <a name="connected-data-source-permissions"></a>Uprawnienia połączonego źródła danych
Aby wykonać dowolne z obsługiwanych zadań w łączniku programu Lotus Domino, musisz być członkiem następujących grup:

* Administratorzy dostępu pełnego
* Administratorzy
* Administratorzy baz danych

W poniższej tabeli wymieniono uprawnienia wymagane dla każdej operacji:

| Operacja | Prawa dostępu |
| --- | --- |
| Importuj |<li>Odczytaj dokumenty publiczne</li><li> Administrator pełnego dostępu (gdy użytkownik jest członkiem grupy administratorów o pełnych uprawnieniach, ma automatycznie dostęp do listy ACL).</li> |
| Eksportowanie i Ustawianie hasła |Dostęp skuteczny: <li>Tworzenie dokumentów</li><li>Usuwanie dokumentów</li><li>Odczytaj dokumenty publiczne</li><li>Zapisuj dokumenty publiczne</li><li>Replikowanie lub kopiowanie dokumentów</li>W przypadku operacji eksportu potrzebne są również następujące role: <li>Zasób</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Operacje bezpośrednie i AdminP
Operacje należy przeprowadzić bezpośrednio do katalogu Domino lub przez proces AdminP. W poniższej tabeli wymieniono wszystkie obsługiwane obiekty, operacje i, jeśli ma zastosowanie, powiązana Metoda implementacji:

**Podstawowa książka adresowa**

| Obiekt | Utwórz | Aktualizacja | Usuń |
| --- | --- | --- | --- |
| Osoba |AdminP |Direct |AdminP |
| Group (Grupa) |AdminP |Direct |AdminP |
| MailInDB |Direct |Direct |Direct |
| Zasób |AdminP |Direct |AdminP |

**Pomocnicza książka adresowa**

| Obiekt | Utwórz | Aktualizacja | Usuń |
| --- | --- | --- | --- |
| Osoba |Nie dotyczy |Direct |Direct |
| Group (Grupa) |Direct |Direct |Direct |
| MailInDB |Direct |Direct |Direct |
| Zasób |NIE DOTYCZY |NIE DOTYCZY |NIE DOTYCZY |

Po utworzeniu zasobu tworzony jest dokument notatki. Podobnie po usunięciu zasobu dokument notatki jest usuwany.

### <a name="ports-and-protocols"></a>Porty i protokoły
Serwery Client i Domino programu IBM Lotus komunikują się za pomocą funkcji uwagi zdalne wywołanie procedury (NRPC), gdzie NRPC powinien korzystać z protokołu TCP/IP. Domyślny numer portu to 1352, ale może zostać zmieniony przez administratora Domino.

### <a name="not-supported"></a>Nieobsługiwane
Bieżąca wersja łącznika programu Lotus Domino nie obsługuje następujących operacji:

* Przenoszenie skrzynek pocztowych między serwerami.

## <a name="create-a-new-connector"></a>Utwórz nowy łącznik
### <a name="client-software-installation-and-configuration"></a>Instalacja i konfiguracja oprogramowania klienckiego
**Przed** zainstalowaniem łącznika programu należy zainstalować na serwerze Program Lotus Notes.

Podczas instalacji programu upewnij się, że została **zainstalowana jedna instalacja użytkownika** . Domyślna **Instalacja przez wiele użytkowników** nie działa.  
![Notes1](./media/microsoft-identity-manager-2016-connector-domino/notes1.png)

Na stronie funkcje Zainstaluj tylko wymagane funkcje programu Lotus Notes i **Logowanie jednokrotne klienta** . Pojedyncze logowanie jest wymagane, aby łącznik mógł zalogować się na serwerze Domino.  
![Notes2](./media/microsoft-identity-manager-2016-connector-domino/notes2.png)

**Uwaga:** Uruchom program Lotus Notes raz dla użytkownika znajdującego się na tym samym serwerze co konto, którego używasz jako konta usługi łącznika. Upewnij się również, że klient programu Lotus Notes jest zamknięty na serwerze. Nie może być uruchomiona w tym samym czasie, gdy łącznik próbuje nawiązać połączenie z serwerem Domino.

### <a name="create-connector"></a>Utwórz łącznik
Aby utworzyć łącznik programu Lotus Domino, w obszarze **usługa synchronizacji** wybierz pozycję **agent zarządzania** i **Utwórz** . Wybierz łącznik **programu Lotus Domino (Microsoft)** .  
![Połączenie](./media/microsoft-identity-manager-2016-connector-domino/createconnector.png)

Jeśli Twoja wersja usługi synchronizacji oferuje możliwość konfigurowania **architektury** , upewnij się, że łącznik ma ustawioną wartość domyślną, aby uruchomić w **procesie** .

### <a name="connectivity"></a>Łączność
Na stronie łączność należy określić nazwę serwera programu Lotus Domino i wprowadzić poświadczenia logowania.  
![Łączność](./media/microsoft-identity-manager-2016-connector-domino/connectivity.png)

Właściwość serwera Domino obsługuje dwa formaty dla nazwy serwera:

* ServerName
* ServerName/DirectoryName

Format **servername/DirectoryName** jest preferowanym formatem dla tego atrybutu, ponieważ zapewnia szybszy czas odpowiedzi, gdy łącznik kontaktuje się z serwerem Domino.

Podany plik UserID jest przechowywany w bazie danych konfiguracji usługi synchronizacji.

W przypadku **importu różnicowego** można korzystać z następujących opcji:

* **Brak** . Łącznik nie wykonuje żadnych importów różnicowych.
* **Dodaj/Aktualizuj** . Łącznik wykonuje operacje dodawania i aktualizowania importu różnicowego. W przypadku usunięcia wymagana jest **pełna operacja importu** . Ta operacja korzysta z programu .NET Interop.
* **Dodaj/zaktualizuj/Usuń** . Łącznik wykonuje operacje dodawania, aktualizowania i usuwania różnicowego importu. Ta operacja korzysta z natywnych interfejsów C++.

W obszarze **Opcje schematu** są dostępne następujące opcje:

* **Domyślny schemat** . Łącznik wykrywa schemat z serwera Domino. Jest to opcja domyślna.
* **DSML — schemat** . Używane tylko wtedy, gdy serwer Domino nie uwidacznia schematu. Następnie można utworzyć plik DSML ze schematem i zaimportować go zamiast tego. Aby uzyskać więcej informacji na temat DSML, zobacz [języka Oasis](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Po kliknięciu przycisku Dalej zostaną zweryfikowane parametry konfiguracji identyfikatora użytkownika i hasła.

### <a name="global-parameters"></a>Parametry globalne
Na stronie parametry globalne można skonfigurować strefę czasową i opcję importowania i eksportowania.  
![Parametry globalne](./media/microsoft-identity-manager-2016-connector-domino/globalparameters.png)

Parametr **strefy czasowej serwera Domino** definiuje lokalizację serwera Domino.

Ta opcja konfiguracji jest wymagana do obsługi operacji **importu różnicowego** , ponieważ umożliwia ona określenie zmian między ostatnimi dwoma importami.

> [!Note]
> Począwszy od aktualizacji 2017, ekran Global Parameters zawiera opcję usuwania bazy danych poczty użytkownika podczas usuwania użytkownika.

![Usuń skrzynkę pocztową użytkownika](./media/microsoft-identity-manager-2016-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Ustawienia importowania, Metoda
**Wykonanie pełnego importu przez** program obejmuje następujące opcje:

* Wyszukiwanie
* Widok (zalecane)

**Wyszukiwanie** korzysta z indeksowania w programie Domino, ale często indeksy nie są aktualizowane w czasie rzeczywistym, a dane zwrócone z serwera nie zawsze są poprawne. W przypadku systemu z wieloma zmianami ta opcja zazwyczaj nie działa prawidłowo i zapewnia fałszywe usunięcia w niektórych sytuacjach. **Wyszukiwanie** jest jednak szybsze niż w **widoku** .

Opcja **Widok** jest zalecana, ponieważ zapewnia poprawny stan danych. Wyszukiwanie jest nieco wolniejsze.

#### <a name="creation-of-virtual-contact-objects"></a>Tworzenie wirtualnych obiektów kontaktów
Opcje **Włącz tworzenie \_ obiektu Contact** są następujące:

* Brak
* Wartości, które nie są odwołaniami
* Odwołania i wartości niereferencyjne

W programie Domino atrybuty odwołania mogą zawierać wiele różnych formatów odwołujących się do innych obiektów. Aby można było reprezentować różne odmiany, łącznik implementuje \_ obiekty Contact, znane także jako **kontakty wirtualne** (VC). Te obiekty są tworzone, aby można było dołączyć do istniejących obiektów MV lub zaprojektować jako nowe obiekty. W ten sposób odwołania do atrybutów można zachować.

Przez włączenie tego ustawienia i jeśli zawartość atrybutu odwołania nie jest formatem DN, \_ tworzony jest obiekt Contact. Na przykład atrybut elementu członkowskiego grupy może zawierać adresy SMTP. Istnieje również możliwość, że w atrybutach referencyjnych są obecne krótkie nazwy i inne atrybuty. W tym scenariuszu wybierz **wartości, które nie są odwołaniami** . Ta konfiguracja jest najbardziej typowym ustawieniem dla implementacji Domino.

W przypadku skonfigurowania programu Lotus Domino do posiadania oddzielnych książek adresowych o różnych nazwach wyróżniających, które reprezentują ten sam obiekt, możliwe jest również utworzenie \_ obiektów kontaktów dla wszystkich wartości odniesienia, które znajdują się w książce adresowej. W tym scenariuszu wybierz opcję **odwołania i wartości, które nie są odwołaniami** .

Jeśli w atrybucie **FullName** w elemencie Domino istnieje wiele wartości, wówczas należy również włączyć tworzenie kontaktów wirtualnych, aby można było rozpoznać odwołania. Na przykład ten atrybut może mieć wiele wartości po zawarciu małżeństwa lub rozwodu. Zaznacz pole wyboru **Włącz... FullName ma wiele wartości** w tym scenariuszu.

Przez przyłączenie do poprawnych atrybutów \_ obiekty Contact byłyby przyłączone do obiektu MV.

Te obiekty mają \_ dodany numer VC = Contact do swojej nazwy wyróżniającej.

#### <a name="import-settings-conflict-object"></a>Ustawienia importowania, obiekt powodujący konflikt
**Wyklucz obiekt konfliktu**

W przypadku dużej implementacji programu Domino istnieje możliwość, że wiele obiektów ma tę samą nazwę DN z powodu problemów z replikacją. W takich przypadkach łącznik zobaczy dwa obiekty o różnych UniversalIDs, ale tej samej nazwie wyróżniającej. Ten konflikt mógłby spowodować utworzenie obiektu przejściowego w przestrzeni łącznika. Łącznik może ignorować obiekty, które zostały wybrane w programie Domino jako ofiary replikacji. Zaleca się pozostawienie tego pola wyboru.

#### <a name="export-settings"></a>Eksportowanie ustawień
Jeśli opcja **Użyj AdminP do aktualizowania odwołań** jest niezaznaczona, a następnie eksport atrybutów referencyjnych, takich jak element członkowski, jest wywołaniem bezpośrednim i nie korzysta z procesu AdminP. Tej opcji należy używać tylko wtedy, gdy AdminP nie został skonfigurowany do obsługi integralności referencyjnej.

#### <a name="routing-information"></a>Informacje o routingu
W programie Domino istnieje możliwość, że atrybut Reference ma osadzone informacje routingu jako sufiks do nazwy wyróżniającej. Na przykład atrybut member w grupie może zawierać <strong>CN = example/organization@ABC </strong>. Sufiks @ABC jest informacjami o routingu. Informacje o routingu są używane przez program Domino do wysyłania wiadomości e-mail do poprawnego systemu Domino, który może być systemem w innej organizacji. W polu Informacje o routingu możesz określić sufiksy routingu używane w organizacji w zakresie łącznika. Jeśli jedna z tych wartości zostanie znaleziona jako sufiks w atrybucie odwołania, informacje o routingu zostaną usunięte z odwołania. Jeśli nie można dopasować sufiksu routingu w wartości odniesienia do jednej z określonych wartości, \_ tworzony jest obiekt Contact. Te \_ obiekty kontaktowe są tworzone za pomocą **typu ro <RoutingSuffix> = @** wstawionego do nazwy wyróżniającej. W przypadku tych \_ obiektów kontaktowych dodawane są również następujące atrybuty, które umożliwiają dołączanie do rzeczywistego obiektu w razie potrzeby: \_ routingname, \_ ContactName, \_ DisplayName i UniversalID.

#### <a name="additional-address-books"></a>Dodatkowe książki adresowe
Jeśli nie masz zainstalowanej **pomocy** dotyczącej katalogów, która zawiera nazwy dodatkowych książek adresowych, możesz wprowadzić te książki adresowe ręcznie.

#### <a name="multivalued-transformation"></a>Transformacja wielowartościowa
Wiele atrybutów w programie Lotus Domino jest z wieloma wartościami. Odpowiednie atrybuty Metaverse są zazwyczaj pojedynczej wartości. Konfigurując opcję Importuj i Eksportuj operację, należy włączyć łącznik, aby ułatwić wymagane tłumaczenie atrybutów, których dotyczy.

**Eksportowanie**  
Opcja eksportowania operacji obsługuje dwa tryby:

* Dołącz element
* Zamień element

**Zamień element** — po wybraniu tej opcji łącznik zawsze usuwa bieżące wartości atrybutu w programie Domino i zastępuje je podanymi wartościami. Podana wartość może być wartością pojedynczą lub wielowartościową.

Przykład: atrybut Assistant obiektu osoby ma następujące wartości:

* CN = Greg Winston/OU = contoso/O = Ameryka Północna, NAB = Names. NF
* CN = Jan Kowalski/OU = contoso/O = Ameryka Północna, NAB = Names. NF

Jeśli do tego obiektu osoby jest przypisany nowy asystent o nazwie **David Alexander** , wynikiem jest:

* CN = David Alexander/OU = contoso/O = Ameryka Północna, NAB = Names. NF

**Dołącz element** — po wybraniu tej opcji łącznik zachowuje istniejące wartości w atrybucie w Domino i Wstaw nowe wartości w górnej części listy dane.

Przykład: atrybut Assistant obiektu osoby ma następujące wartości:

* CN = Greg Winston/OU = contoso/O = Ameryka Północna, NAB = Names. NF
* CN = Jan Kowalski/OU = contoso/O = Ameryka Północna, NAB = Names. NF

Jeśli do tego obiektu osoby jest przypisany nowy asystent o nazwie **David Alexander** , wynikiem jest:

* CN = David Alexander/OU = contoso/O = Ameryka Północna, NAB = Names. NF
* CN = Greg Winston/OU = contoso/O = Ameryka Północna, NAB = Names. NF
* CN = Jan Kowalski/OU = contoso/O = Ameryka Północna, NAB = Names. NF

**Importuj**  
Opcja importowania operacji obsługuje dwa tryby:

* Domyślne
* Wielowartościowe wartości pojedyncze

**Domyślne** — po wybraniu opcji domyślnej wszystkie wartości wszystkich atrybutów są importowane.

**Wielowartościowa do pojedynczej wartości** — w przypadku wybrania tej opcji Atrybut wielowartościowy jest konwertowany na atrybut o pojedynczej wartości. Jeśli istnieje więcej niż jedna wartość, używana jest wartość u góry (zazwyczaj jest to najnowsza wartość).

Przykład: atrybut Assistant obiektu osoby ma następujące wartości:

* CN = David Alexander/OU = contoso/O = Ameryka Północna, NAB = Names. NF
* CN = Greg Winston/OU = contoso/O = Ameryka Północna, NAB = Names. NF
* CN = Jan Kowalski/OU = contoso/O = Ameryka Północna, NAB = Names. NF

Najnowsza aktualizacja tego atrybutu to **David Alexander** . Ponieważ opcja importowania jest ustawiona na wielowartościową wartość pojedynczą, łącznik importuje **David Alexander** do obszaru łącznika.

Logika do konwersji atrybutów wielowartościowych na atrybuty jednowartościowe nie ma zastosowania do atrybutu elementu członkowskiego grupy ani do atrybutu Person FullName.

Istnieje również możliwość skonfigurowania reguł transformacji importu i eksportu dla atrybutów wielowartościowych dla atrybutu, jako wyjątek dla reguły globalnej. Aby skonfigurować tę opcję, wprowadź [ObjectType]. [AttributeName] na **liście atrybutów wykluczenia importu** i **Eksportuj listę atrybutów wykluczenia** . Jeśli na przykład wprowadzisz Person. Assistant, a Flaga globalna zostanie ustawiona do importowania wszystkich wartości, tylko pierwsza wartość zostanie zaimportowana dla Asystenta.

#### <a name="certifiers"></a>Certyfikaty zapoświadczające
Wszystkie jednostki organizacyjne są wyświetlane przez łącznik. Aby można było eksportować obiekty osób do podstawowej książki adresowej, musi on mieć za, który ma swoje hasło.

Jeśli wszystkie poświadczające mają takie samo hasło, można użyć **hasła dla wszystkich Certifers** . Następnie możesz wprowadzić tutaj hasło i określić tylko plik poświadczający.

W przypadku zaimportowania tylko nie trzeba określać żadnych zapoświadczających.

### <a name="configure-provisioning-hierarchy"></a>Skonfiguruj hierarchię aprowizacji
Podczas konfigurowania łącznika programu Lotus Domino Pomiń tę stronę okna dialogowego. Łącznik programu Lotus Domino nie obsługuje udostępniania hierarchii.  
![Hierarchia aprowizacji](./media/microsoft-identity-manager-2016-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie
Konfigurując partycje i hierarchie, należy wybrać podstawową książkę adresową o nazwie NAB = Names. NF. Oprócz podstawowej książki adresowej można wybrać dodatkowe książki adresowe, jeśli istnieją.  
![Partycje](./media/microsoft-identity-manager-2016-connector-domino/partitions.png)

### <a name="select-attributes"></a>Wybierz atrybuty
Podczas konfigurowania atrybutów należy wybrać wszystkie atrybuty, które są poprzedzone przez **\_ MMS \_** . Te atrybuty są wymagane podczas inicjowania obsługi administracyjnej nowych obiektów w programie Lotus Domino

![Atrybuty](./media/microsoft-identity-manager-2016-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Zarządzanie cyklem życia obiektów
Ta sekcja zawiera omówienie różnych obiektów w programie Domino.

### <a name="person-objects"></a>Obiekty osób
Obiekt Person reprezentuje użytkowników w organizacji i jednostkach organizacyjnych. Oprócz atrybutów domyślnych Administratorzy Domino mogą dodawać atrybuty niestandardowe do obiektu osoby. Obiekt osoby musi zawierać co najmniej wszystkie atrybuty obowiązkowe. Aby uzyskać pełną listę obowiązkowych atrybutów, zobacz [Właściwości programu Lotus Notes](#lotus-notes-properties). Aby zarejestrować obiekt osoby, należy spełnić następujące wymagania wstępne:

* Książka adresowa (Names. NF) musi być zdefiniowana i powinna być podstawową książką adresową.
* Aby zarejestrować określonego użytkownika w organizacji/jednostce organizacyjnej, musisz mieć identyfikator poświadczający O/OU i hasło.
* Należy ustawić określony zestaw właściwości programu Lotus Notes dla obiektu osoba. Te właściwości są używane do aprowizacji obiektu osoby. Aby uzyskać więcej informacji, zobacz sekcję o nazwie [Właściwości programu Lotus Notes](#lotus-notes-properties) w dalszej części tego dokumentu.
* Początkowe hasło HTTP dla osoby to atrybut i ustawiony podczas aprowizacji.
* Obiekt osoby musi być jednym z następujących trzech obsługiwanych typów:
  1. Normalny użytkownik, który ma plik poczty i plik identyfikatora użytkownika
  2. Użytkownik mobilny (normalny użytkownik, który zawiera wszystkie pliki mobilnej bazy danych)
  3. Kontakty (użytkownik bez pliku identyfikatora)

Osoby (z wyjątkiem kontaktów) mogą być bardziej pogrupowane dla użytkowników w Stanach Zjednoczonych i użytkowników międzynarodowych zdefiniowane przez \_ wartość \_ Właściwości IDRegType MMS. Te osoby używają klienta uwagi do uzyskiwania dostępu do serwerów programu Lotus Domino, mają identyfikator notatki i dokument osoby. Jeśli są używane notatki poczty e-mail, mają także plik poczty. Użytkownik musi być zarejestrowany, aby stał się aktywny. Aby uzyskać więcej informacji, zobacz:

* [Konfigurowanie użytkowników notatek](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Rejestracja użytkownika](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Zarządzanie użytkownikami](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Zmiana nazw użytkowników](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_RENAMING_A_LOTUS_INOTES_USER_STEPS.html)

Wszystkie te operacje są wykonywane w programie Lotus Domino i następnie zaimportowane do usługi synchronizacji.

### <a name="resources-and-rooms"></a>Zasoby i pokoje
Zasób jest innym typem bazy danych w programie Lotus Domino. Zasoby mogą być pokojem konferencyjnym z różnymi rodzajami sprzętu, takimi jak projektory. Istnieją podtypy zasobów obsługiwane przez łącznik programu Lotus Domino, które są zdefiniowane przez atrybut typu zasobu:

| Typ zasobu | Atrybut typu zasobu |
| --- | --- |
| Zmieścić |1 |
| Zasób (inne) |2 |
| Spotkanie online |3 |

Aby typ obiektu zasobu działał, wymagane są następujące elementy:

* Baza danych rezerwacji zasobów powinna już istnieć na połączonym serwerze Domino
* Lokacja jest już zdefiniowana dla zasobu

Baza danych rezerwacji zasobów zawiera trzy typy dokumentów:

* Profil witryny
* Zasób
* Rezerwacja

Więcej informacji o konfigurowaniu bazy danych rezerwacji zasobów znajduje się w temacie [Konfigurowanie bazy danych zastrzeżeń zasobów](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Tworzenie, aktualizowanie i usuwanie zasobów**  
Operacje tworzenia, aktualizowania i usuwania są wykonywane przez łącznik programu Lotus Domino w bazie danych rezerwacji zasobów. Zasoby są tworzone jako dokumenty z nazwami. NF (czyli podstawowa książka adresowa). Aby uzyskać więcej informacji na temat edytowania i usuwania zasobów, zobacz [Edytowanie i usuwanie dokumentów zasobów](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Operacja importowania i eksportowania zasobów**  
Zasoby mogą być importowane do usługi synchronizacji i eksportowane z niej, podobnie jak każdy inny typ obiektu. Wybierz typ obiektu jako zasób podczas konfiguracji. W przypadku pomyślnej operacji eksportowania powinny istnieć szczegółowe informacje dotyczące typu zasobu, bazy danych konferencji i nazwy lokacji.

### <a name="mail-in-databases"></a>Bazy danych Mail-In
Baza danych Mail-In jest bazą danych, która została zaprojektowana w celu otrzymywania wiadomości e-mail. Jest to Skrzynka pocztowa programu Lotus Domino, która nie jest skojarzona z żadnym konkretnym kontem użytkownika programu Lotus Domino (oznacza to, że nie ma własnego pliku ID i hasła). Baza danych poczty e-mail ma unikatowy identyfikator użytkownika ("krótka nazwa") i jego własny adres e-mail.

Jeśli istnieje potrzeba oddzielnej skrzynki pocztowej z własnymi adresami e-mail, które mogą być współużytkowane przez różnych użytkowników (na przykład group@contoso.com ), tworzona jest baza danych poczty. Dostęp do tej skrzynki pocztowej jest kontrolowany za pomocą listy Access Control (ACL), która zawiera nazwiska użytkowników, którzy mogą otwierać skrzynkę pocztową.

Aby zapoznać się z listą wymaganych atrybutów, zobacz sekcję o nazwie [obowiązkowe atrybuty](#mandatory-attributes) w dalszej części tego artykułu.

Jeśli baza danych została zaprojektowana do odbierania wiadomości e-mail, w programie Lotus Domino tworzony jest dokument bazy danych Mail-In. Ten dokument musi znajdować się w katalogu Domino każdego serwera, który przechowuje kopię bazy danych programu. Aby uzyskać szczegółowy opis tworzenia dokumentu bazy danych w programie, zobacz [Tworzenie dokumentu bazy danych Mail-In](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Przed utworzeniem bazy danych Mail-In baza danych powinna już istnieć (powinna zostać utworzona przez administratora programu Lotus) na serwerze Domino.

### <a name="group-management"></a>Zarządzanie grupami
Szczegółowe omówienie zarządzania grupami programu Lotus Domino można znaleźć w następujących zasobach:

* [Korzystanie z grup](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Tworzenie grupy](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Tworzenie i modyfikowanie grup](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Zarządzanie grupami](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Zmiana nazwy grupy](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Zarządzanie hasłami
Dla zarejestrowanego użytkownika programu Lotus Domino istnieją dwa typy haseł:

1. Hasło użytkownika (zapisane w pliku User.id)
2. Hasło internetowe/HTTP

Łącznik programu Lotus Domino obsługuje tylko operacje z hasłem HTTP.

Aby zarządzać hasłami, należy włączyć zarządzanie hasłami dla łącznika w projektancie agenta zarządzania. Aby włączyć zarządzanie hasłami, wybierz pozycję **Włącz zarządzanie hasłami** na stronie **Konfiguruj rozszerzenia** .  
![Konfigurowanie rozszerzeń](./media/microsoft-identity-manager-2016-connector-domino/configureextensions.png)

Łącznik programu Lotus Domino obsługuje następujące operacje dotyczące hasła internetowego:

* Ustawianie hasła: ustawienie hasła ustawia nowe hasło HTTP/Internet w programie Domino. Domyślnie konto jest również odblokowane. Flaga odblokowywania jest uwidaczniana w interfejsie WMI aparatu synchronizacji.
* Zmień hasło: w tym scenariuszu użytkownik może chcieć zmienić hasło lub monit o zmianę hasła po określonym czasie. Aby ta operacja została wykonana, oba (stare i nowe hasło) są obowiązkowe. Po zmianie nowe hasło jest aktualizowane w programie Lotus Domino.

Aby uzyskać więcej informacji, zobacz:

* [Korzystanie z funkcji blokowania Internetu](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Zarządzanie hasłami internetowymi](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Informacje referencyjne
Ta sekcja zawiera listę opisów atrybutów i wymagania dotyczące atrybutów dla łącznika programu Lotus Domino.

### <a name="lotus-notes-properties"></a>Właściwości programu Lotus Notes
W przypadku aprowizacji obiektów osób do katalogu programu Lotus Domino obiekty muszą mieć określony zestaw właściwości z określonymi wartościami. Te wartości są wymagane tylko w przypadku operacji tworzenia.

W poniższej tabeli wymieniono te właściwości i opisano ich opisy.

| Właściwość | Opis |
| --- | --- |
| \_MMS_AltFullName |Alternatywna pełna nazwa użytkownika. |
| \_MMS_AltFullNameLanguage |Język używany do określania alternatywną pełną nazwę użytkownika. |
| \_MMS_CertDaysToExpire |Liczba dni od bieżącego dnia przed wygaśnięciem certyfikatu. Jeśli nie zostanie określony, domyślna Data to dwa lata od bieżącej daty. |
| \_MMS_Certifier |Właściwość, która zawiera nazwę hierarchii organizacyjnej zapoświadczający. Na przykład: OU = OrganizationUnit, O = org, C = Country. |
| \_MMS_IDPath |Jeśli właściwość jest pusta, żaden plik identyfikacyjny użytkownika nie zostanie utworzony lokalnie na serwerze synchronizacji. Jeśli właściwość zawiera nazwę pliku, w folderze madata zostanie utworzony plik identyfikatora użytkownika. Właściwość może również zawierać pełną ścieżkę. |
| \_MMS_IDRegType |Osoby mogą być klasyfikowane jako kontakty, użytkownicy i użytkownicy międzynarodowi. Poniższa tabela zawiera listę możliwych wartości: <li>0 — kontakt</li><li>1 — użytkownik USA</li><li>2 — użytkownik Międzynarodowy</li> |
| \_MMS_IDStoreType |Właściwość wymagana dla użytkowników w Stanach Zjednoczonych i międzynarodowym. Właściwość zawiera liczbę całkowitą określającą, czy tożsamość użytkownika jest przechowywana jako załącznik w książce adresowej notatki lub w pliku poczty osoby. Jeśli plik identyfikatora użytkownika jest załącznikiem w książce adresowej, można go opcjonalnie utworzyć jako plik z \_ MMS_IDPath. <li>Pusty plik identyfikatora magazynu w magazynie identyfikatorów, brak pliku identyfikacyjnego (używany do kontaktów).</li><li> 1 — załącznik w książce adresowej notatki. \_Właściwość MMS_Password musi być ustawiona dla plików identyfikacji użytkownika, które są załącznikami</li><li>2 — Identyfikator magazynu w pliku poczty osoby. Aby \_ można było utworzyć plik poczty podczas rejestracji osoby, MMS_UseAdminP musi być ustawiona na wartość false. \_Właściwość MMS_Password musi być ustawiona dla plików identyfikacji użytkownika.</li> |
| \_MMS_MailQuotaSizeLimit |Liczba megabajtów dozwolona dla bazy danych plików poczty e-mail. |
| \_MMS_MailQuotaWarningThreshold |Liczba megabajtów dozwolona dla bazy danych plików poczty e-mail przed wystawieniem ostrzeżenia. |
| \_MMS_MailTemplateName |Plik szablonu wiadomości e-mail służący do tworzenia pliku poczty e-mail użytkownika. Jeśli szablon zostanie określony, plik poczty zostanie utworzony przy użyciu określonego szablonu. Jeśli szablon nie zostanie określony, do utworzenia pliku zostanie użyty plik szablonu domyślnego. |
| \_MMS_OU |Opcjonalna właściwość, która jest nazwą jednostki organizacyjnej w obszarze certyfikacji. Ta właściwość powinna być pusta dla kontaktów. |
| \_MMS_Password |Właściwość wymagana dla użytkowników. Właściwość zawiera hasło do pliku identyfikacyjnego obiektu. |
| \_MMS_UseAdminP |Właściwość powinna być ustawiona na wartość true, jeśli plik poczty powinien być utworzony przez proces AdminP na serwerze Domino (asynchronicznie w procesie eksportu). Jeśli właściwość ma wartość false, plik poczty zostanie utworzony za pomocą użytkownika Domino (synchronicznie w procesie eksportu). |

Dla użytkownika ze skojarzonym plikiem identyfikacyjnym \_ właściwość MMS_Password musi zawierać wartość. Aby uzyskać dostęp do poczty e-mail za pośrednictwem klienta programu Lotus Notes, właściwości MailServer i MailFile użytkownika muszą zawierać wartość.

Aby można było uzyskać dostęp do poczty e-mail za pośrednictwem przeglądarki sieci Web, następujące właściwości muszą zawierać wartości:

* MailFile — Właściwość wymagana, która zawiera ścieżkę na serwerze programu Lotus Domino, na którym jest przechowywany plik poczty.
* MailServer — Właściwość wymagana, która zawiera nazwę serwera programu Lotus Domino. Ta wartość jest nazwą używaną podczas tworzenia pliku poczty programu Lotus na serwerze Domino.
* HTTPPassword — Właściwość opcjonalna, która zawiera hasło dostępu do sieci Web dla obiektu.

Aby uzyskać dostęp do serwera Domino bez możliwości poczty, właściwość HTTPPassword musi zawierać wartość. Właściwość MailFile i Właściwość MailServer mogą być puste.

\_Gdy MMS_ IDStoreType = 2 (Identyfikator magazynu w pliku poczty), właściwość MailSystem dla NotesRegistrationclass ma wartość REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Atrybuty obowiązkowe
Łącznik programu Lotus Domino głównie obsługuje te typy obiektów (typy dokumentów):

* Group (Grupa)
* Baza danych Mail-In
* Osoba
* Kontakt (osoba bez zapoświadczania)
* Zasób

Ta sekcja zawiera listę atrybutów, które są obowiązkowe dla każdego obsługiwanego obiektu do eksportowania na serwer Domino.

| Typ obiektu | Atrybuty obowiązkowe |
| --- | --- |
| Group (Grupa) |<li>ListName</li> |
| Baza danych Main-In |<li>Pełna nazwa</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Osoba |<li>LastName (Nazwisko)</li><li>MailFile</li><li>Krótką nazwę maszyny</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Kontakt (osoba bez zapoświadczania) |<li>\_MMS_IDRegType</li> |
| Zasób |<li>Pełna nazwa</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Witryna</li><li>Nazwa wyświetlana</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Typowe problemy i pytania
### <a name="schema-detection-does-not-work"></a>Wykrywanie schematu nie działa
Aby można było wykryć schemat, konieczne jest, aby na serwerze Domino był obecny plik Schema. NF. Ten plik jest wyświetlany tylko wtedy, gdy na serwerze jest zainstalowany protokół LDAP. Jeśli schemat nie jest wykrywalny, sprawdź następujące kwestie:

* Plik Schema. NF jest obecny w folderze głównym serwera Domino
* Użytkownik ma uprawnienia do wyświetlania pliku schema. NF.
* Wymuś ponowne uruchomienie serwera LDAP. Otwórz **konsolę programu Lotus Domino** i użyj polecenia **mów LDAP ReloadSchema** , aby ponownie załadować schemat.

### <a name="not-all-secondary-address-books-are-visible"></a>Nie wszystkie pomocnicze książki adresowe są widoczne
Łącznik programu Domino opiera się na **pomocy katalogu** funkcji, aby można było znaleźć pomocnicze książki adresowe. Jeśli brakuje pomocniczych książek adresowych, sprawdź, czy [Pomoc katalogu](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_DIRECTORY_ASSISTANCE.html) została włączona i skonfigurowana na serwerze Domino.

### <a name="custom-attributes-in-domino"></a>Atrybuty niestandardowe w Domino
Istnieje kilka sposobów, aby zwiększyć schemat, aby był wyświetlany jako atrybut niestandardowy użyteczny przez łącznik.

**Podejście 1: rozszerzona schemat programu Lotus Domino**

1. Utwórz kopię szablonu katalogu programu Domino {PUBNAMES. NTF} wykonując następujące [kroki](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nie należy dostosowywać domyślnego szablonu katalogu programu IBM Lotus Domino):
2. Otwórz kopię szablonu katalogu programu Domino {CONTOSO. NTF}, który został utworzony w programie Domino Designer, i wykonaj następujące kroki:
   * Kliknij pozycję elementy udostępnione i rozwiń podformularze
   * Kliknij dwukrotnie pozycję $ {ObjectName} InheritableSchema (gdzie {ObjectName} jest nazwą domyślnej klasy obiektu strukturalnego, na przykład: Person).
   * Nazwij atrybut, który chcesz dodać do schematu {MyPersonAtrribute} i odpowiadającego temu atrybutowi. Utwórz pole, wybierając menu **Utwórz** , a następnie wybierz **pole** z menu.
   * W dodanym polu ustaw jego właściwości, wybierając typ, styl, rozmiar, czcionkę i inne powiązane parametry pola okno Właściwości.
   * Zachowaj wartość domyślną atrybutu, która jest taka sama jak nazwa podaną dla tego atrybutu (na przykład jeśli nazwa atrybutu to PersonName, Zachowaj wartość domyślną z tą samą nazwą).
   * Zapisz podformularz $ {ObjectName} InheritableSchema z zaktualizowanymi wartościami.
3. Zastąp szablon katalogu Domino {PUBNAMES. NTF} z nowym dostosowany szablonem {CONTOSO. NTF}, wykonując następujące [kroki](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zamknij i Otwórz konsolę programu Domino, aby ponownie uruchomić usługę LDAP i ponownie załadować schemat LDAP:
   * W konsoli programu Domino Wstaw polecenie w obszarze tekst **polecenia programu Domino** , aby ponownie uruchomić zadanie usługi LDAP [restart](https://www.ibm.com/support/knowledgecenter/SSKTMJ_10.0.1/admin/conf_startingandstoppingtheldapservice_c.html).
   * Aby ponownie załadować użycie schematu LDAP polecenie Powiadom LDAP ReloadSchema
5. Otwórz administratora programu Domino i wybierz pozycję osoby & grupy, aby zobaczyć dodany atrybut jest widoczny w polu Domino Dodaj osobę.
6. Otwórz plik Schema. NF z poziomu karty **pliki** i zobacz dodany atrybut jest uwzględniony w klasie obiektów dominoPerson LDAP.

**Podejście 2: Tworzenie auxClass z atrybutem niestandardowym i kojarzenie z klasą obiektów**

1. Utwórz kopię szablonu katalogu programu Domino {PUBNAMES. NTF} wykonując następujące [czynności](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nigdy nie dostosowuj domyślnego szablonu katalogu programu IBM Lotus Domino):
2. Otwórz kopię szablonu katalogu programu Domino {CONTOSO. NTF}, który został utworzony w programie Domino Designer.
3. W lewym okienku wybierz pozycję współużytkowany kod, a następnie podformularze.
4. Kliknij pozycję Nowy podformularz
5. Wykonaj poniższe czynności, aby określić właściwości dla nowego podformularza:
   * Przy otwartym nowym podformularzu wybierz pozycję projektowanie — właściwości podformularza
   * Obok właściwości Nazwa wprowadź nazwę pomocniczej klasy obiektu — na przykład TestSubform.
   * Zachowaj Właściwość Options "include w instrukcji INSERT... wybrane okno dialogowe
   * Usuń zaznaczenie właściwości Options "renderowanie przez HTML w notatkach".
   * Pozostaw inne właściwości tak samo, i Zamknij pole właściwości podformularza.
   * Zapisz i Zamknij nowy podformularz.
6. Wykonaj następujące czynności, aby dodać pole do definiowania pomocniczej klasy obiektów:
   * Otwórz podformularz, który został utworzony.
   * Wybierz pozycję Utwórz pole.
   * Obok pozycji Nazwa na karcie podstawowe okna dialogowego pole określ dowolną nazwę, na przykład: {MyPersonTestAttribute}.
   * W dodanym polu ustaw jego właściwości, wybierając jego typ, styl, rozmiar, czcionkę i powiązane właściwości.
   * Zachowaj wartość domyślną atrybutu, która jest taka sama jak nazwa podaną dla tego atrybutu (na przykład jeśli nazwa atrybutu to MyPersonTestAttribute, Zachowaj wartość domyślną o tej samej nazwie).
   * Zapisz podformularz z zaktualizowanymi wartościami i wykonaj następujące czynności:
     * W lewym okienku wybierz pozycję współużytkowany kod, a następnie podformularze.
     * Wybierz nowy podformularz i wybierz projekt projektowanie właściwości.
     * Kliknij trzecią kartę z lewej strony i wybierz pozycję **Propaguj ten zakaz zmiany projektu** .
7. Otwórz podformularz $ {ObjectName} ExtensibleSchema, (gdzie {ObjectName} jest nazwą domyślnej klasy obiektu strukturalnego, na przykład — osoba).
8. Wstaw zasób i wybierz podformularz (utworzony na przykład – TestSubform) i Zapisz podformularz $ {ObjectName} ExtensibleSchema.
9. Zastąp szablon katalogu Domino {PUBNAMES. NTF} z nowym dostosowany szablonem {CONTOSO. NTF}, wykonując następujące [kroki](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zamknij i Otwórz konsolę programu Domino, aby ponownie uruchomić usługę LDAP i ponownie załadować schemat LDAP:
    * W konsoli programu Domino Wstaw polecenie w obszarze tekst **polecenia programu Domino** , aby ponownie uruchomić zadanie usługi LDAP [restart](https://www.ibm.com/support/knowledgecenter/SSKTMJ_10.0.1/admin/conf_startingandstoppingtheldapservice_c.html).
    * Aby ponownie załadować użycie schematu LDAP polecenie poinformuj LDAP **ReloadSchema** .
11. Otwórz administratora programu Domino i wybierz pozycję osoby & grupy, aby zobaczyć dodany atrybut jest odzwierciedlony w polu Dodaj osobę (na karcie inne).
12. Otwórz plik Schema. NF z poziomu karty **pliki** i zobacz dodany atrybut jest widoczny w obszarze Klasa pomocnicza obiektu TestSubform LDAP.

**Podejście 3: Dodawanie atrybutu niestandardowego do klasy Extensibleobject**

1. Otwórz plik {Schema. NF} umieszczony w katalogu głównym
2. Wybierz pozycję klasy obiektów LDAP z menu po lewej stronie w obszarze **wszystkie dokumenty schematu** , a następnie kliknij przycisk **Dodaj klasę obiektów** :
3. Podaj nazwę LDAP w postaci {zzzExtensibleSchema} (gdzie ZZZ to nazwa domyślnej klasy obiektu strukturalnego, na przykład Person). Na przykład, aby zwiększyć schemat klasy obiektu osoby, podaj nazwę LDAP {PersonExtensibleSchema}.
4. Podaj nazwę klasy obiektów nadrzędnych, dla której chcesz zwiększyć schemat. Na przykład aby zwiększyć schemat klasy obiektu osoby, podaj nazwę klasy obiektów nadrzędnych {dominoPerson}:
5. Podaj prawidłowy identyfikator OID odpowiadający klasie Object.
6. Wybierz atrybuty rozszerzone/niestandardowe w obszarze obowiązkowe lub opcjonalne typy atrybutów pola zgodnie z wymaganiami:
7. Po dodaniu wymaganych atrybutów do ExtensibleObjectClass kliknij pozycję **zapisz & Zamknij** .
8. ExtensibleObjectClass jest tworzony dla odpowiedniej klasy obiektu domyślnego z rozszerzonymi atrybutami.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
* Aby uzyskać informacje na temat włączania rejestrowania w celu rozwiązywania problemów z łącznikiem, zobacz [jak włączyć śledzenie ETW dla łączników](https://go.microsoft.com/fwlink/?LinkId=335731).
