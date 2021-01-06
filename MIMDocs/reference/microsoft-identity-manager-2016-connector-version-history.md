---
title: Historia wersji łącznika | Microsoft Docs
description: W tym temacie wymieniono wszystkie wersje łączników dla programu Forefront Identity Manager (FIM) i Microsoft Identity Manager (MIM)
services: active-directory
documentationcenter: ''
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/31/2020
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f0b61059f9010523fa4f7b6a6ced987e5ab2dc49
ms.sourcegitcommit: 8f81767ec92e1b80658aaebb9463aa4d62396d43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927697"
---
# <a name="connector-version-release-history"></a>Historia wersji łącznika

Łączniki łączą określone połączone źródła danych z Microsoft Identity Manager (MIM) i Azure AD Connect. (W programie Forefront Identity Manager łączniki były znane jako agenci zarządzania). Wiele łączników, takich jak łączniki, aby udostępnić użytkownikom Active Directory, są dostarczane jako część instalacji usługi synchronizacji programu MIM i pakietu instalacyjnego Azure AD Connect. Ponadto więcej łączników, takich jak serwery katalogowe innych firm, są dostarczane jako oddzielne pobieranie, dzięki czemu można je częściej aktualizować w celu dodania obsługi połączenia z zaktualizowanymi wersjami systemów docelowych innych firm.  

> [!NOTE]
> Ten temat dotyczy głównie tylko łączników usługi FIM i MIM. O ile nie zostanie jawnie wywołana poniżej, te łączniki nie są obsługiwane na potrzeby instalacji na Azure AD Connect. Wydane łączniki są preinstalowane na Azure AD Connect podczas uaktualniania do określonej kompilacji.


W tym temacie wymieniono wszystkie wersje pakietu łączników ogólnych, które zostały wydane niezależnie od programu MIM.  Aby uzyskać listę łączników, które są obsługiwane w programie MIM, zobacz [obsługiwane łączniki w programie mim 2016 SP2](../supported-management-agents.md).  Niektórzy partnerzy utworzyli własne łączniki w ten sposób, a pełna lista jest dostępna w witrynie wiki [FIM 2010 i MIM 2016: agenci zarządzania z partnerów](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-mim-2016-management-agents-from-partners.aspx).


Powiązane linki:

* [Pobierz najnowsze łączniki](https://go.microsoft.com/fwlink/?LinkId=717495)
* Dokumentacja referencyjna [łącznika ogólnego LDAP](microsoft-identity-manager-2016-connector-genericldap.md)
* Dokumentacja dotycząca [ogólnego łącznika SQL](microsoft-identity-manager-2016-connector-genericsql.md)
* Dokumentacja dotycząca [łącznika usług sieci Web](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws)
* Dokumentacja dotycząca [łącznika programu PowerShell](microsoft-identity-manager-2016-connector-powershell.md)
* Dokumentacja dotycząca [łącznika programu Lotus Domino](microsoft-identity-manager-2016-connector-domino.md)
* Dokumentacja dotycząca [łącznika magazynu profilu użytkownika programu SharePoint](https://go.microsoft.com/fwlink/?LinkID=331344)

## <a name="1113470-december-2020"></a>1.1.1347.0 (grudzień 2020)
### <a name="fixed-issues"></a>Naprawione problemy
- Łącznik grafu
  - Rozwiązano problem polegający na tym, że łącznik niepoprawnie wysyła zaproszenia B2B podczas tworzenia grupy z włączoną obsługą poczty lub kontaktu

## <a name="1113460-november-2020"></a>1.1.1346.0 (listopad 2020)
### <a name="fixed-issues"></a>Naprawione problemy
- Łącznik grafu
  - Rozwiązano problem związany z uszkodzeniem pamięci podręcznej łącznika lokalnego powodującym błędy przebiegu importu różnicowego
  - Rozwiązano problem ze zduplikowanymi wpisami zgłoszonymi przez łącznik podczas pełnego przebiegu importu powodującego błędy odnajdywania
  - Rozwiązano problem z nieprawidłowym importem złożonych typów danych, np. *employeeOrgData*
- Ogólny łącznik SQL
  - Rozwiązano problem z niepowodzeniem uwierzytelniania SQL Native ze względu na Właściwość parametrów połączenia DSN *TrustedConnection* ustawiona na *wartość false* . 
- Ogólny łącznik LDAP
  - Rozwiązano problem polegający na tym, że przetwarzanie wpisów *OpenLDAP* *accessLog* w przypadku importu różnicowego powoduje nieprawidłowe zmiany członkostwa w grupie i inne błędy

## <a name="1113020-september-2020"></a>1.1.1302.0 (2020 września)
### <a name="fixed-issues"></a>Naprawione problemy
- Łącznik grafu
  - Rozwiązano problem z odświeżaniem schematu dla punktu końcowego */beta*

## <a name="1113010-august-2020"></a>1.1.1301.0 (sierpień 2020)
### <a name="fixed-issues"></a>Naprawione problemy
- Łącznik grafu
  - Naprawiono usterkę z błędami importu spowodowaną przez pustą wartość atrybutu *UserType*
  - Usunięto usterkę z niemożliwym do odczytu pamięci podręcznej łączników błędów importu różnicowego spowodowanych błędami odnajdywania
  - Przekroczenie limitu czasu raportowania usługi i serwera proxy
  - Zaktualizowano odnajdywanie schematu dla punktu końcowego */beta* 
### <a name="enhancements"></a>Ulepszenia 
- Łącznik grafu
  - Dodano obsługę odczytu i zapisu wartości niestandardowych atrybutów rozszerzenia katalogu
  - Dodano obsługę odczytywania *głównych członków usługi* Groups w punkcie końcowym */beta* 
  - Ulepszenia wydajności dla przebiegów importu różnicowego związanych z odnajdywaniem schematu
  - Łącznik grafu może teraz zapraszać zewnętrznych użytkowników należących do członków

  > [!NOTE]
  > Jeśli używasz zaproszenia gościa w kompilacji 1.1.1170.0 łącznika, zaktualizuj reguły synchronizacji, korzystając z następującej logiki:

  - Przepływy wychodzące
    - Użytkownik jest zapraszany podczas eksportowania tworzenia użytkownika, a eksport obejmuje atrybut *poczty* , ale nie *Atrybut userPrincipalName*.  Jeśli jest *dostarczany, zostanie* utworzony użytkownik, a nie zaproszenie
    - Atrybut *UserType* definiuje tylko, czy użytkownik stanie się *członkiem* , czy *gościem* (domyślnie *, jeśli nie* jest ustawiony)
  - Przepływy przychodzące
    - Wartości atrybutów *userPrincipalName* dla użytkowników zewnętrznych są renderowane jako "AS-IS"

## <a name="1111700-april-2020"></a>1.1.1170.0 (kwiecień 2020)
### <a name="fixed-issues"></a>Naprawione problemy
- Ogólny łącznik SQL
   - Naprawiono usterkę z strategią eksportu z użyciem zapytań i aktualizacjami atrybutów wielowartościowych
- Łącznik programu Lotus Notes
   - Grupy z notatek pomocniczych książki adresowe nie są już usuwane przez proces *AdminP* . Operacja usuwania bezpośredniego jest używana teraz
- Ogólny łącznik LDAP
   - Naprawiono usterkę z atrybutami operacji katalogu LDAP, np. *pwdUpdateTime*, niewidocznymi w schemacie
### <a name="enhancements"></a>Ulepszenia 
- Łącznik grafu   
   - Nazwy UPN zewnętrznych użytkowników-Gości nie są już renderowane jako "AS-IS", zamiast tego są wyświetlane w obszarze łącznika, aby wyglądały jak wiadomości e-mail
   - Dodano obsługę dla aprowizacji użytkowników-gościa B2B

   Aby zaprosić użytkowników-Gości B2B, należy:
   - Przyznawanie uprawnień do zapraszania Gości do aplikacji usługi Azure AD skojarzonej z łącznikiem grafu
   - Ukończ sekcję konfiguracji łącznika w przypadku zapraszania użytkowników zewnętrznych: Ustaw adres URL przekierowania zaproszenia (obowiązkowy) i zdecyduj, czy wysyłać wiadomości e-mail z zaproszeniem
   - Ustaw obowiązkowe atrybuty w regule synchronizacji danych wychodzących:
     - "Gość" =>*UserType* (tylko przepływ początkowy)
     - zewnętrzny adres e-mail =>*userPrincipalName*
     - CustomExpression ("CN =" + csObjectID + ", OBJECT = User") =>*DN* (tylko początkowy przepływ)
     - csObjectID = *identyfikator*>(tylko początkowy przepływ)

## <a name="1111300-february-2020"></a>1.1.1130.0 (luty 2020)
### <a name="fixed-issues"></a>Naprawione problemy
- Łącznik grafu
   - Eksport nie powiedzie się już podczas próby dodania elementu członkowskiego do grupy, która została już dodana
   - obsługa atrybutu wirtualnego "export_password" jest stała, nie trzeba już konfigurować przepływu atrybutu "Password"
   - Stały przepływ eksportu atrybutów ciągów o wartości wielowartościowej
   - Rozwiązano kilka usterek wpływających na import Delta
   - Naprawiono różne potencjalne błędy formatu DateTime
- Ogólny łącznik SQL
   - Rozwiązano różne błędy interfejsu użytkownika
   - Naprawiono obsługę nieprawidłowych odwołań podczas importowania różnicowego
   - Naprawiono usterkę z strategią importu różnicowego Change Tracking SQL i tabel wielowartościowych w celu poprawnego importowania zmian członkostwa w grupie
   - Naprawiono usterkę z wartościami atrybutów, które nie są czyszczone podczas eksportowania
   - Naprawiono usterkę z ostatnim elementem atrybutu odwołania wielowartościowego, który nie jest usuwany podczas eksportowania
   - Naprawiono usterkę z odświeżeniem schematu powodującą, że atrybuty odwołania są ustawiane na ciągi
   - Naprawiono usterkę z wartościami parametrów procedury składowanej, które są obcinane do 397 bajtów
   - Naprawiono usterkę przy użyciu tabel i widoków Oracle w przypadku wykrywania schematu z uwzględnieniem wielkości liter
- Łącznik programu Lotus Notes
   - Lepsza wydajność podczas importowania członków grupy
   - Pełny import nie powiedzie się już z błędami odwołania o wartości null
   - Naprawiono usterkę z usuwaniem skrzynek pocztowych uwag po ustawieniu listy ACL
   - Puste nazwy grup nie powodują już błędów importu różnicowego
   - Naprawiono usterkę z niedrukowanymi znakami pozostawionymi w atrybutach po usunięciu wartości ciągu
- Ogólny łącznik LDAP
   - Import Delta nie zawiera już wartości literału "Replace", jeśli nie ustawiono żadnej wartości w źródle danych dziennika.
### <a name="enhancements"></a>Ulepszenia 
- Łącznik grafu
   - Dodano obsługę dla suwerennych chmur i możliwość skonfigurowania adresów URL zasobów i nazwy grafów
   - Nieobsługiwane atrybuty są odfiltrowane i ukrywane we właściwościach łącznika po przeprowadzeniu odnajdywania schematu

## <a name="4418001-july-2019"></a>4.4.1800.1 (lipiec 2019)
### <a name="enhancements"></a>Usprawni
- Łącznik magazynu profilu użytkownika programu SharePoint
   - Dodano obsługę programu SharePoint Server 2019. Łącznik jest dostępny do pobrania z [Centrum pobierania Microsoft](https://go.microsoft.com/fwlink/?linkid=279713) 

## <a name="119530-june-2019"></a>1.1.953.0 (czerwiec 2019)

### <a name="fixed-issues"></a>Rozwiązano problemy:
- Ogólny łącznik LDAP
   - Import Delta nie powiedzie się, jeśli pole zmiany jest puste w katalogu internetowym Oracle
- Ogólny łącznik SQL
   - Rozwiązano problem związany z niestandardową strategią importowania zapytań SQL przy użyciu parametrów StartIndex i EndIndex, co spowodowało zaimportowanie tylko pierwszej strony
   - Rozwiązano problem związany z strategią importowania tabeli/widoku podczas odczytywania danych z programu MS SQL, co spowoduje zaimportowanie tylko pierwszej strony
- Łącznik grafu:
   - Kontakty organizacyjne są teraz obsłużone prawidłowo, nie ma już wiadomości e-mail
   - Rozwiązano problem z operacjami importowania i eksportowania atrybutów Menedżera. Łącznik nie działa już, gdy Menedżer jest pusty. Wartość Menedżera jest poprawnie aktualizowana w usłudze Azure AD
   - Rozwiązano problem związany z importem różnicowym pustych wartości
   - Rozwiązano problem związany z awarią łącznika podczas importowania różnicowego, gdy lokalny plik pamięci podręcznej jest uszkodzony
   - Rozwiązano kilka problemów związanych z nieprawidłowym eksportem pustych wartości lub w przypadku zmiany tylko wielkości atrybutu.
   - Łącznik obsługuje teraz operacje usuwania/dodawania dla tego samego atrybutu podczas eksportowania przebiegu poprawnie
   - Rozwiązano kilka problemów dotyczących długotrwałych operacji importowania i eksportowania, gdy tokeny dostępu były wygasane podczas uruchamiania łącznika. Łącznik będzie teraz odnawiać tokeny dostępu w razie potrzeby bez błędów
   - Rozwiązano problem z ostatnim członkiem grupy, która nie jest usuwana
### <a name="enhancements"></a>Usprawni
- Ogólny łącznik SQL
   - parametr commandTimeout czytnika danych jest ustawiony tak, aby odpowiadał przekroczeniu limitu czasu łącznika. Jeśli masz długotrwałe zapytania, których wykonanie trwa dłużej niż 30 sekund, możesz zwiększyć wartość parametru "limit czasu polecenia" w sekcji "łączność"
- Łącznik grafu: 
   - Dodano wielowątkową strategię importowania członkostwa w grupach, aby zwiększyć wydajność importu. Import Delta pozostaje operacją jednowątkową
   - Dodano obsługę złożonych typów schematów, które mają atrybuty, takie jak OnPremisesExtentionAttributes. *, które są teraz dostępne
   - Dodano obsługę atrybutu export_password, aby uniknąć eksportu błędów eksport-zmiana-nie-importowanych i nie pokazuj początkowego hasła w przestrzeni łącznika. Zachowanie jest podobne do innych łączników ECMA2
   - Dodano procedurę obsługi do obsługi ograniczania żądań HTTP. Gdy usługa Azure AD Replica odbiera zbyt wiele żądań od klienta, może odpowiedzieć na Retry-After instrukcji. Łącznik zostanie wstrzymany i ponowienie próby zamiast niepowodzenia
   - Profil importowania różnicowego nie będzie już uruchamiany w przypadku zdefiniowania filtrów zapytań. Jeśli chcesz zaimportować tylko określone obiekty z usługi Azure AD, na przykład użytkownicy o nazwisku rozpoczynającym się od znaku *, funkcja importowania różnicowego zostanie zablokowana


## <a name="119130-january-2019"></a>1.1.913.0 \( stycznia 2019\)

### <a name="fixed-issues"></a>Rozwiązano problemy:

* Ogólne SQL:
    * Naprawiono błąd regresji w ogólnym łączniku SQL, w którym odczytywano tylko pierwsze 5000 obiektów.
* Łącznik grafu:
    * Rozwiązano problem polegający na tym, że łącznik interfejs API programu Graph nie mógł odczytać/zapisać z dzierżawy lub utworzyć nowego łącznika, gdy opcja beta została wybrana na potrzeby łączności.

### <a name="enhancements"></a>Usprawni

* Łącznik grafu:
    * Ustaw protokół TLS 1,2 jako domyślny dla łącznika grafu.
* Ogólny łącznik SQL:
    * Ogólny łącznik SQL został przetestowany z oprogramowaniem Oracle 12c i Oracle 18c.

## <a name="119110"></a>1.1.911.0

> [!NOTE]  
> Ta wersja kompilacji łącznika jest używana tylko w ramach wdrożeń Azure AD Connect i nie jest dostępna do użycia w wdrożeniach programu MIM.
> 
> W porównaniu z poprzednią wersją łącznika nie zawiera ulepszeń ani aktualizacji dla klientów programu MIM.

-  Aktualizuje łączniki niestandardowe (na przykład ogólny łącznik LDAP i ogólny łącznik SQL) dostarczane z Azure AD Connect.  

## <a name="118610"></a>1.1.861.0

### <a name="fixed-issues"></a>Rozwiązano problemy:
* Zmień tytuły wszystkich ustawień łącznika z "Forefront to Microsoft"

* Uwagi dotyczące programu Lotus: 
    * Błędy w modelu COM z programem Lotus czasami nie zwracają błędów
* Ogólny LDAP:
    * Gldap dodatkowy symbol dla DI z serwerem usługi PING
* Ogólne usługi sieci Web:
    * Wystąpił błąd podczas eksportowania analizy JSON
* Ogólne SQL:
    * Eksportuj atrybut binarny
    * Typy obiektów nie mogą być podciągami siebie nawzajem
    * Zmiany w tabeli wielowartościowej nie są śledzone w operacji "import Delta", jeśli "Strategia różnicowa" ma wartość "Change Tracking"
* Łącznik grafu (publiczna wersja zapoznawcza)
    * Błąd podczas usuwania grupy
    * Aktualizowanie User-Agent do nagłówka http
    * Łącznik nie sprawdza poprawności identyfikatora klienta i klucza tajnego klienta
    * Zarejestrowana nazwa dzierżawy powinna zostać przycięta

### <a name="enhancements"></a>Usprawni
* Uwagi programu Lotus: * Dodaj możliwość zwiększenia limitu czasu za pomocą interfejsu użytkownika
* Łącznik programu Graph (publiczna wersja zapoznawcza) * atrybut hasła jest filtrowany podczas importowania, aby wyeliminować "Eksport-nie-ponownie importowany".
    * Dodanie obsługi $filter parametru zapytania — ograniczone do operacji ze wszystkimi filtrami, które działają w zapytaniach różnicowych, również będzie działała w łączniku * zaktualizowane do używania nextLink bezpośrednio zamiast wyodrębniania skipToken w celu uzyskania szczegółów dotyczących stronicowania w [tym miejscu](https://developer.microsoft.com/en-us/graph/docs/concepts/paging)

## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Rozwiązano problemy:
* Rozpoznano ConnectorsLog system. Diagnostics. EventLogInternal. InternalWriteEvent (komunikat: urządzenie dołączone do systemu nie działa)
* W tej wersji łączników należy zaktualizować przekierowanie powiązania z 3.3.0.0-4.1.3.0 do 4.1.4.0 w miiserver.exe.config
* Ogólne usługi sieci Web:
    * Nie można zapisać rozpoznanej prawidłowej odpowiedzi JSON w narzędziu konfiguracji
* Ogólne SQL:
    * Wartość Eksportuj zawsze generuje tylko zapytanie Update na potrzeby operacji usuwania. Dodano do wygenerowania kwerendy usuwania
    * Zapytanie SQL, które pobiera obiekty do operacji importu różnicowego, jeśli "Strategia różnicowa" ma wartość "Change Tracking". W tej implementacji znane ograniczenie: import Delta z trybem "Change Tracking" nie śledzi zmian w atrybutach wielowartościowych
    * Dodano możliwość wygenerowania zapytania Delete dla wielkości liter, gdy konieczne jest usunięcie ostatniej wartości atrybutu wielowartościowego i ten wiersz nie zawiera żadnych innych danych poza wartością, co jest niezbędne do usunięcia.
    * Obsługa metody System. ArgumentException w przypadku zaimplementowania parametrów WYJŚCIOWYch przez SP
    * Nieprawidłowe zapytanie, aby wykonać operację eksportu do pola, które ma typ varbinary (max)
    * Problem z zmienną Listaparametrów został zainicjowany dwukrotnie (w funkcjach ExportAttributes i GetQueryForMultiValue)


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Rozwiązano problemy:

* Uwagi dotyczące programu Lotus:
  * Filtrowanie opcji niestandardowych zapoświadczań
  * Importowanie klasy ImportOperations Naprawiono definicję operacji, które mogą być uruchamiane w trybie "widoki" i który znajduje się w trybie "Search".
* Ogólny LDAP:
  * Katalog OpenLDAP używa nazwy wyróżniającej jako kotwicy zamiast entryUUI. Nowa opcja do łącznika GLDAP, która umożliwia modyfikowanie kotwicy
* Ogólne SQL:
  * Ustalono eksport do pola, które ma typ varbinary (max).
  * W przypadku dodawania danych binarnych ze źródła danych do obiektu CSEntry funkcja DataTypeConversion nie udała się na zero bajtów. Stała funkcja DataTypeConversion klasy CSEntryOperationBase.




### <a name="enhancements"></a>Usprawni

* Ogólne SQL:
  * Możliwość konfigurowania trybu wykonywania procedury składowanej z nazwanymi parametrami lub bez nazwy jest dodawana w oknie konfiguracji ogólnego agenta zarządzania SQL Server na stronie "parametry globalne". Na stronie "parametry globalne" jest pole wyboru z etykietą "Użyj nazwanych parametrów do wykonania procedury składowanej", która jest odpowiedzialna za tryb wykonywania procedury składowanej z nazwanymi parametrami.
    * Obecnie możliwość wykonywania procedury składowanej z nazwanymi parametrami działa tylko dla baz danych IBM DB2 i MSSQL. W przypadku baz danych Oracle i MySQL to podejście nie działa: 
      * Składnia SQL programu MySQL nie obsługuje parametrów nazwanych w procedurach składowanych.
      * Sterownik ODBC dla programu Oracle nie obsługuje parametrów nazwanych w procedurach składowanych.

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Rozwiązano problemy:

* Ogólne usługi sieci Web:
  * Rozwiązano problem uniemożliwiający utworzenie projektu protokołu SOAP, gdy wystąpiły dwa lub więcej punktów końcowych.
* Ogólne SQL:
  * W trakcie operacji importowania GSQL nie był prawidłowo konwertowany, gdy jest zapisywany w miejscu łącznika. Domyślny format daty i godziny dla przestrzeni łącznika GSQL został zmieniony z "RRRR-MM-DD GG: mm: SSS" na "RRRR-MM-DD GG: mm: SSS".

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Rozwiązano problemy:

* Ogólne usługi sieci Web:
  * Narzędzie WSconfig nie przekonwertował prawidłowo tablicy JSON z "przykładowego żądania" dla metody usługi REST. Spowodowało to problemy z serializacji tej tablicy JSON dla żądania REST.
  * Narzędzie konfiguracji łącznika usługi sieci Web nie obsługuje używania symboli spacji w nazwach atrybutów JSON. 
    * Wzorzec podstawienia można dodać ręcznie do pliku WSConfigTool.exe.config, na przykład  ```<appSettings> <add key="JSONSpaceNamePattern" value="__" /> </appSettings>```
      > [!NOTE]
      > Klucz JSONSpaceNamePattern jest wymagany jako eksport, zostanie wyświetlony następujący błąd: komunikat: pusta nazwa jest niedozwolona. 

* Uwagi dotyczące programu Lotus:
  * Jeśli opcja **Zezwalaj na niestandardowe urzędy certyfikacji dla organizacji/jednostek organizacyjnych** jest wyłączona, łącznik kończy się niepowodzeniem podczas eksportowania (aktualizacja) po przeniesieniu przepływu eksportu wszystkie atrybuty zostaną wyeksportowane do programu Domino, ale w czasie eksportowania KeyNotFoundException jest zwracany do synchronizacji. 
    * Dzieje się tak, ponieważ operacja zmiany nazwy kończy się niepowodzeniem, gdy próbuje zmienić nazwę WYRÓŻNIAJĄCą (atrybut nazwy użytkownika), zmieniając jeden z poniższych atrybutów:  
      - LastName (Nazwisko)
      - FirstName (Imię)
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - jednostki organizacyjnej
      - altcommonname

  * Gdy opcja **Zezwalaj na niestandardowe zaświadczanie dla organizacji/jednostek organizacyjnych** jest włączona, ale wymagane certyfikaty zaświadczania są nadal puste, a następnie KeyNotFoundException.

### <a name="enhancements"></a>Usprawni

* Ogólne SQL:
  * **Scenariusz: zaprojektowano ponownie:** Funkcja "*"
  * **Opis rozwiązania:** Zmieniono podejście do [obsługi atrybutów odwołań wielowartościowych](microsoft-identity-manager-2016-connector-genericsql.md).


### <a name="fixed-issues"></a>Rozwiązano problemy:

* Ogólne usługi sieci Web:
  * Nie można zaimportować konfiguracji serwera, jeśli jest obecny łącznik usługi WebService
  * Łącznik usługi WebService nie działa z wieloma usługami sieci Web

* Ogólne SQL:
  * Nie są wyświetlane żadne typy obiektów dla atrybutu o pojedynczej wartości, do którego istnieje odwołanie
  * Importowanie różnicowe przy Change Tracking strategii usuwania obiektu, gdy wartość zostanie usunięta z tabeli wielowartościowej
  * OverflowException w łączniku GSQL z programem DB2 w dniu/400

Programami
  * Dodano opcję enable\disable wyszukiwanie jednostek organizacyjnych przed otwarciem strony GlobalParameters

## <a name="114430"></a>wersji 1.1.443.0

Wydanie: 2017 marca

### <a name="enhancements"></a>Ulepszenia 

* Ogólne SQL:</br>
  **Objawy scenariusza:**  Jest to dobrze znane ograniczenie dotyczące łącznika SQL, w którym dozwolony jest tylko odwołanie do jednego typu obiektu i wymaganie krzyżowego odwołania z elementami członkowskimi. </br>
  **Opis rozwiązania:** W kroku przetwarzania dla odwołań została wybrana opcja "*", wszystkie kombinacje typów obiektów zostaną zwrócone z powrotem do aparatu synchronizacji.

> [!Important]
> - Spowoduje to utworzenie wielu symboli zastępczych
> - Jest to wymagane, aby upewnić się, że nazewnictwo jest unikatowymi typami wielu obiektów.


* Ogólny LDAP:</br>
  **Scenariusz:** W przypadku wybrania tylko kilku kontenerów w określonej partycji, wyszukiwanie nadal zostanie wykonane w całej partycji. Konkretna zostanie przefiltrowana według usługi synchronizacji, ale nie przez MA, co może spowodować spadek wydajności. </br>

  **Opis rozwiązania:** Zmieniono kod łącznika GLDAP, aby umożliwić Przechodzenie przez wszystkie kontenery i wyszukiwanie obiektów w każdym z nich zamiast wyszukiwania w całej partycji.


* Lotus Domino:

  **Scenariusz:** Obsługa usuwania poczty w programie Domino podczas eksportowania przez osobę. </br>
  **Rozwiązanie:** Konfigurowalna obsługa usuwania poczty podczas eksportowania.

### <a name="fixed-issues"></a>Rozwiązano problemy:
* Ogólne usługi sieci Web:
  * Gdy zmieniasz adres URL usługi w domyślnych projektach SAP WSconfig za pomocą narzędzia konfiguracji sieci Web, wystąpi następujący błąd: nie można znaleźć części ścieżki

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Ogólny LDAP:
  * Łącznik GLDAP nie widzi wszystkich atrybutów w AD LDS
  * Przerywa działanie kreatora, gdy nie wykryto atrybutów UPN w schemacie katalogu LDAP
  * Importy różnicowe z błędami odnajdywania nie występują podczas pełnego importu, gdy atrybut "objectClass" nie jest wybrany
  * Na stronie konfiguracji "Konfigurowanie partycji i hierarchii" nie są wyświetlane żadne obiekty, które typ są równe partycji dla nowych serwerów w ogólnym  
  KATALOG LDAP MA. Są one wyświetlane tylko w przypadku obiektów z partycji RootDSE.


* Ogólne SQL:
  * Poprawka dla ogólnego znaku wodnego instrukcji SQL import różnicowego atrybutu zaimportowanego
  * Podczas eksportowania wartości deleted\added atrybutu wielowartościowego nie są one deleted\added w źródle danych.  


* Uwagi dotyczące programu Lotus:
  * Określone pole "Full Name" jest prawidłowo wyświetlane w obiekcie Metaverse, jednak podczas eksportowania do uwagi wartość atrybutu jest zerowa lub pusta.
  * Poprawka dla zduplikowanego błędu certyfikacji
  * Gdy na łączniku programu Lotus Domino z innymi obiektami zostanie wybrany obiekt bez danych, zostanie wyświetlony komunikat o błędzie odnajdywania podczas wykonywania pełnego importu.
  * Gdy import Delta jest uruchamiany na łączniku programu Lotus Domino, po zakończeniu tego działania usługa Microsoft.IdentityManagement.MA.LotusDomino.Service.exe czasami zwraca błąd aplikacji.
  * Członkostwo w grupach działa prawidłowo i jest utrzymywane, z wyjątkiem sytuacji, gdy podczas wykonywania eksportu użytkownik próbuje usunąć użytkownika z członkostwa, który zostanie wyświetlony jako pomyślne z aktualizacją, ale użytkownik nie zostanie usunięty z członkostwa w programie Lotus Notes.
  * Szansa wyboru trybu eksportu jako "Dołącz element u dołu" została dodana w graficznym interfejsie użytkownika konfiguracji programu Lotus MA dołączać nowe elementy na dole podczas eksportowania dla atrybutów wielowartościowych.
  * Łącznik doda wymaganą logikę, aby usunąć plik z folderu poczty i magazynu identyfikatorów.
  * Usuwanie członkostwa nie działa dla elementu członkowskiego Cross NAB.
  * Pomyślnie usunięto wartości z atrybutu wielowartościowego

## <a name="111170"></a>1.1.117.0
Wydanie: 2016 marca

**Nowy łącznik**  
Początkowa wersja [ogólnego łącznika SQL](microsoft-identity-manager-2016-connector-genericsql.md).

**Nowe funkcje:**

* Ogólny łącznik LDAP:
  * Dodano obsługę importowania różnicowego za pomocą isode.
* Łącznik usług sieci Web:
  * Zaktualizowano działanie csEntryChangeResult i działanie setImportErrorCode, aby umożliwić zwrot błędów na poziomie obiektów z powrotem do aparatu synchronizacji.
  * Zaktualizowano szablony SAP6 i SAP6User w celu użycia nowej funkcji błędu poziomu obiektu.
* Łącznik programu Lotus Domino:
  * W przypadku eksportu potrzebny jest jeden poświadczający na książkę adresową. Teraz można użyć tego samego hasła dla wszystkich wystawcy, aby ułatwić zarządzanie.

**Rozwiązano problemy:**

* Ogólny łącznik LDAP:
  * W przypadku oprogramowania IBM Tivoli DS niektóre atrybuty odwołania nie zostały prawidłowo wykryte.
  * W przypadku otwarcia protokołu LDAP podczas importowania różnicowego, odstępy na początku i na końcu ciągów zostały obcięte.
  * W przypadku Novell i NetIQ, eksport, który przeniósł obiekt między jednostkami organizacyjnymi/kontenerami, i w tym samym czasie zmieniono nazwę obiektu.
* Łącznik usług sieci Web:
  * Jeśli usługa sieci Web ma wiele punktów końcowych dla tego samego powiązania, łącznik nie odnajduje prawidłowo tych punktów końcowych.
* Łącznik programu Lotus Domino:
  * Eksportowanie atrybutu fullName do bazy danych poczty nie zadziałało.
  * Eksport, który został dodany i usunięty członek z grupy tylko wyeksportował dodani członkowie.
  * Jeśli dokument programu uwagi jest nieprawidłowy (atrybut isValid ma wartość false), łącznik kończy się niepowodzeniem.

## <a name="older-releases"></a>Starsze wersje
Przed marcem 2016 łączniki zostały wydane jako tematy pomocy technicznej.

**Ogólny łącznik LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, 2015 września
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, 2015 marca
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, 2015 stycznia
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, 2014 września
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, 2014 marca

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, 2014 września

**Program PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, 2014 września

**Program Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, 2015 września
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, 2015 marca
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, 2014 sierpnia
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, 2014 lutego  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, 2013 października
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, 2013 sierpnia

## <a name="troubleshooting"></a>Rozwiązywanie problemów 

> [!NOTE]
> Podczas aktualizowania Microsoft Identity Manager lub AADConnect przy użyciu dowolnego z łączników ECMA2. 

Przed uaktualnieniem należy odświeżyć definicję łącznika w celu dopasowania lub w 6947 dzienniku zdarzeń aplikacji zostanie wyświetlony następujący błąd: "wersja zestawu w konfiguracji łącznika usługi AAD (" X. X. XXX. X ") jest wcześniejsza niż wersja rzeczywista (" X. X. XXX. X ") elementu" C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll ".

Aby odświeżyć definicję:
* Otwórz właściwości wystąpienia łącznika
* Kliknij kartę połączenie/Połącz z
  * Wprowadź hasło dla konta łącznika
* Kliknij poszczególne karty właściwości, z kolei
  * Jeśli ten typ łącznika ma kartę partycje z przyciskiem odświeżania, kliknij przycisk Odśwież na tej karcie.
* Po uzyskaniu dostępu do wszystkich kart właściwości kliknij przycisk OK, aby zapisać zmiany.

## <a name="other-connectors"></a>Inne łączniki

Oprócz łączników wymienionych powyżej łączniki programu SharePoint oraz starszego łącznika dla systemu Windows Azure Active Directory również zostały rozdystrybuowane osobno od programu MIM.

### <a name="sharepoint-user-profile"></a>Profil użytkownika programu SharePoint

Łącznik programu Forefront Identity Manager dla profilu użytkownika programu SharePoint ułatwia synchronizowanie informacji o tożsamości z magazynem profilów użytkowników w programie SharePoint 2013 i SharePoint 2016.   Wersja 4.3.2430.0 tego łącznika opublikowane 12/19/2016 można pobrać z [Centrum pobierania Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=41164).

Więcej informacji na temat tego łącznika można znaleźć w [pakiecie zbiorczym poprawek](https://support.microsoft.com/en-us/help/3156030/hotfix-rollup-build-4-3-2201-0-is-available-for-forefront-identity-man) i instrukcje dotyczące [korzystania z przykładowego rozwiązania MIM w programie SharePoint Server 2016](https://docs.microsoft.com/SharePoint/administration/use-a-sample-mim-solution-in-sharepoint-server-2016).

### <a name="forefront-identity-manager-connector-for-windows-azure-active-directory-legacy-connector"></a>Łącznik programu Forefront Identity Manager dla systemu Windows Azure Active Directory (stary łącznik)

Łącznik usługi Azure AD dla programu FIM to wczesna technologia do synchronizowania informacji o tożsamości do Azure Active Directory. Łącznik usługi Azure AD dla programu FIM, wersja 1.0.6635.0069 od 19 lutego 2014, jest w trakcie zawieszania funkcji. Nie otrzymuje żadnych [aktualizacji, ale](https://www.microsoft.com/en-us/download/details.aspx?id=41166) nadal są obsługiwane. Rozwiązanie przy użyciu programu FIM i łącznika usługi Azure AD zostało zastąpione przez [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect). Firma Microsoft zaleca, aby nie rozpocząć nowego wdrożenia przy użyciu tego łącznika.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat dokumentacji [ogólnych łączników LDAP](microsoft-identity-manager-2016-connector-genericldap.md) .
Dowiedz się więcej na temat ogólnej dokumentacji dotyczącej[łącznika SQL](microsoft-identity-manager-2016-connector-genericsql.md) .
Dowiedz się więcej na temat dokumentacji dotyczącej [łącznika usług sieci Web](microsoft-identity-manager-2016-ma-ws.md) .
Dowiedz się więcej na temat dokumentacji dotyczącej [łącznika programu PowerShell](microsoft-identity-manager-2016-connector-powershell.md) .
Dowiedz się więcej na temat dokumentacji dotyczącej [łącznika programu Lotus Domino](microsoft-identity-manager-2016-connector-domino.md) .
