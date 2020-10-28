---
title: Ogólny łącznik SQL | Microsoft Docs
description: W tym artykule opisano sposób konfigurowania łącznika ogólnego programu SQL firmy Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 862ea1ed910905daa0f2e0be97838f3da2423661
ms.sourcegitcommit: 2950ef38055bf78e3b98e4cd84771b04c2aa5584
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/08/2020
ms.locfileid: "92761045"
---
# <a name="generic-sql-connector-technical-reference"></a>Informacje techniczne dotyczące ogólnego łącznika SQL
W tym artykule opisano ogólny łącznik SQL. Artykuł dotyczy następujących produktów:

* Microsoft Identity Manager 2016 (programie MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Należy użyć poprawki 4.1.3671.0 lub nowszej [KB3092178](https://support.microsoft.com/kb/3092178).

W przypadku programie MIM2016 i FIM2010R2 łącznik jest dostępny do pobrania z [Centrum pobierania Microsoft](https://go.microsoft.com/fwlink/?LinkId=717495).

Aby wyświetlić ten łącznik w działaniu, zobacz [ogólny krok po kroku łącznika SQL](microsoft-identity-manager-2016-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Omówienie ogólnego łącznika SQL
Ogólny łącznik SQL umożliwia integrację usługi synchronizacji z systemem bazy danych, który oferuje łączność ODBC.  

Z perspektywy wysokiego poziomu następujące funkcje są obsługiwane przez bieżącą wersję łącznika:

| Cechy | Pomoc techniczna |
| --- | --- |
| Połączone źródło danych |Łącznik jest obsługiwany ze wszystkimi 64-bitowymi sterownikami ODBC *. Został przetestowany z następującymi: <li>Microsoft SQL Server & SQL Azure</li><li>IBM DB2 10. x</li><li>IBM DB2 9. x</li><li>Oracle 10 & 11g</li><li>Oracle 12c i 18c</li><li>Baza danych MySQL 5. x</li> |
| Scenariusze |<li>Zarządzanie cyklem życia obiektów</li><li>Zarządzanie hasłami</li> |
| Operacje |<li>Pełny import i import różnicowy, eksport</li><li>Dla eksportu: Dodaj, Usuń, Aktualizuj i Zamień</li><li>Ustawianie hasła, zmiana hasła</li> |
| Schemat |<li>Dynamiczne odnajdywanie obiektów i atrybutów</li> |

> [!NOTE]
> * Połączenia ze źródłami danych, które nie są wymienione powyżej, są obecnie ograniczone do scenariuszy tylko do odczytu.

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika upewnij się, że na serwerze synchronizacji są dostępne następujące elementy:

* Microsoft .NET 4.5.2 Framework lub nowszy
* 64-bitowe sterowniki klienta ODBC
* Jeśli łącznik jest używany do komunikowania się z programem Oracle 12c, wymagany jest program Oracle Instant Client 12.2.0.1 lub nowszy z pakietem ODBC.
* Jeśli łącznik jest używany do komunikowania się z programem Oracle 18c, wymagany jest program Oracle Instant Client 18.3.0.0 lub nowszy z pakietem ODBC oraz zmienna systemowa NLS_LANG do obsługi znaków UTF8.

### <a name="permissions-in-connected-data-source"></a>Uprawnienia w połączonym źródle danych
Aby utworzyć lub wykonać dowolne z obsługiwanych zadań w ogólnym łączniku SQL, musisz dysponować:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Porty i protokoły
Aby porty wymagane przez sterownik ODBC działały, zapoznaj się z dokumentacją dostawcy bazy danych.

## <a name="create-a-new-connector"></a>Utwórz nowy łącznik
Aby utworzyć ogólny łącznik SQL, w obszarze **usługa synchronizacji** wybierz pozycję **agent zarządzania** i **Utwórz** . Wybierz **ogólny łącznik SQL (Microsoft)** .

![Połączenie](./media/microsoft-identity-manager-2016-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Łączność
Łącznik używa pliku DSN ODBC do łączności. Utwórz plik DSN przy użyciu **źródeł danych ODBC** znajdujących się w menu Start w obszarze **Narzędzia administracyjne** . W narzędziu administracyjnym Utwórz **PLIKOWE DSN** , aby można było je dostarczyć do łącznika.

![Połączenie](./media/microsoft-identity-manager-2016-connector-genericsql/connectivity.png)

Ekran łączności jest pierwszy podczas tworzenia nowego ogólnego łącznika SQL. Najpierw musisz podać następujące informacje:

* Ścieżka pliku DSN
* Authentication
  * Nazwa użytkownika
  * Hasło

Baza danych powinna obsługiwać jedną z następujących metod uwierzytelniania:

* **Uwierzytelnianie systemu Windows** : uwierzytelniana baza danych używa poświadczeń systemu Windows w celu zweryfikowania użytkownika. Określona nazwa użytkownika i hasło są używane do uwierzytelniania w bazie danych. To konto wymaga uprawnień do bazy danych.
* **Uwierzytelnianie SQL** : uwierzytelniana baza danych używa nazwy użytkownika/hasła zdefiniowanej przez ekran łączności, aby połączyć się z bazą danych. Jeśli przechowujesz nazwę użytkownika/za pomocą hasła w pliku DSN, poświadczenia podane na ekranie łączności mają pierwszeństwo.
* **Azure SQL Database uwierzytelniania** : Aby uzyskać więcej informacji, zobacz [nawiązywanie połączenia z SQL Database przy użyciu uwierzytelniania Azure Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).

**Nazwa DN jest zakotwiczeniem** : w przypadku wybrania tej opcji Nazwa wyróżniająca jest również używana jako atrybut zakotwiczenia. Może służyć do prostej implementacji, ale ma również następujące ograniczenie:

* Łącznik obsługuje tylko jeden typ obiektu. W związku z tym wszystkie atrybuty odwołania mogą odwoływać się tylko do tego samego typu obiektu.

**Typ eksportu: obiekt Replace** : podczas eksportowania, gdy tylko niektóre atrybuty uległy zmianie, cały obiekt ze wszystkimi atrybutami jest eksportowany i zastępuje istniejący obiekt.

### <a name="schema-1-detect-object-types"></a>Schemat 1 (wykrywanie typów obiektów)
Na tej stronie można skonfigurować, w jaki sposób łącznik będzie znajdować różne typy obiektów w bazie danych.

Każdy typ obiektu jest prezentowany jako partycja i jest skonfigurowany do dalszej **konfiguracji partycji i hierarchii** .

![schema1a](./media/microsoft-identity-manager-2016-connector-genericsql/schema1a.png)

**Metoda wykrywania typu obiektu** : Łącznik obsługuje te metody wykrywania typu obiektu.

* **Stała wartość** : Podaj listę typów obiektów z listą rozdzieloną przecinkami. Na przykład: `User,Group,Department`.  
  ![schema1b](./media/microsoft-identity-manager-2016-connector-genericsql/schema1b.png)
* **Tabela/widok/procedura składowana** : Podaj nazwę tabeli/widoku/procedury składowanej, a następnie nazwę kolumny, która zawiera listę typów obiektów. Jeśli używasz procedury składowanej, podaj również parametry w formacie **[name]: [Direction]: [Value]** . Podaj każdy parametr w osobnym wierszu (Użyj kombinacji klawiszy CTRL + ENTER, aby uzyskać nowy wiersz).  
  ![schema1c](./media/microsoft-identity-manager-2016-connector-genericsql/schema1c.png)
* **Zapytanie SQL** : Ta opcja umożliwia podanie zapytania SQL, które zwraca pojedynczą kolumnę z typami obiektów, na przykład `SELECT [Column Name] FROM TABLENAME` . Zwracana kolumna musi być typu String (varchar).

### <a name="schema-2-detect-attribute-types"></a>Schemat 2 (wykrywanie typów atrybutów)
Na tej stronie można skonfigurować sposób wykrywania nazw i typów atrybutów. Opcje konfiguracji są wyświetlane dla każdego typu obiektu wykrytego na poprzedniej stronie.

![schema2a](./media/microsoft-identity-manager-2016-connector-genericsql/schema2a.png)

**Metoda wykrywania typu atrybutu** : Łącznik obsługuje te metody wykrywania typu atrybutu z każdym wykrytym typem obiektu na ekranie schematu 1.

* **Tabela/widok/procedura składowana** : Podaj nazwę tabeli/widoku/procedury przechowywanej, która powinna być używana do znajdowania nazw atrybutów. Jeśli używasz procedury składowanej, podaj również parametry w formacie **[name]: [Direction]: [Value]** . Podaj każdy parametr w osobnym wierszu (Użyj kombinacji klawiszy CTRL + ENTER, aby uzyskać nowy wiersz). Aby wykryć nazwy atrybutów w atrybucie wielowartościowym, Podaj listę tabel lub widoków rozdzieloną przecinkami. Scenariusze wielowartościowe nie są obsługiwane, gdy tabela nadrzędna i podrzędna mają takie same nazwy kolumn.
* **Zapytanie SQL** : Ta opcja umożliwia podanie zapytania SQL, które zwraca pojedynczą kolumnę z nazwami atrybutów, na przykład `SELECT [Column Name] FROM TABLENAME` . Zwracana kolumna musi być typu String (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schemat 3 (Zdefiniuj kotwicę i nazwę wyróżniającą)
Ta strona umożliwia skonfigurowanie atrybutu kotwicy i nazwy wyróżniającej dla każdego wykrytego typu obiektu. Można wybrać wiele atrybutów, aby zakotwiczyć kotwicę jako unikatową.

![schema3a](./media/microsoft-identity-manager-2016-connector-genericsql/schema3a.png)

* Nie ma na liście atrybutów wielowartościowych i logicznych.
* Nie można użyć tego samego atrybutu dla nazwy wyróżniającej i kotwicy, chyba że na stronie łączności jest zaznaczona **nazwa DN** .
* Jeśli na stronie łączność została wybrana **nazwa DN** , ta strona wymaga tylko atrybutu nazwy wyróżniającej. Ten atrybut będzie również używany jako atrybut zakotwiczenia.

  ![schema3b](./media/microsoft-identity-manager-2016-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schemat 4 (Definiowanie typu atrybutu, odwołania i kierunku)
Ta strona umożliwia skonfigurowanie typu atrybutu, takiego jak Integer, binary lub Boolean, oraz kierunek dla każdego atrybutu. Wszystkie atrybuty ze **schematu strony 2** są wymienione w uwzględnieniu atrybutów wielowartościowych.

![schema4a](./media/microsoft-identity-manager-2016-connector-genericsql/schema4a.png)

* **Typ danych** : używany do mapowania typu atrybutu na te typy znane przez aparat synchronizacji. Wartością domyślną jest użycie tego samego typu, który został wykryty w schemacie SQL, ale nie można łatwo wykryć wartości DateTime i Reference. Dla tych należy określić **Właściwość DateTime** lub **Reference** .
* **Kierunek** : można ustawić kierunek atrybutu do zaimportowania, eksportu lub ImportExport. ImportExport jest wartością domyślną.

![schema4b](./media/microsoft-identity-manager-2016-connector-genericsql/schema4b.png)

Uwagi:

* Jeśli łącznik nie jest wykrywalny, używa typu danych String.
* **Zagnieżdżone tabele** mogą być traktowane jako jednokolumnowe tabele baz danych. Firma Oracle przechowuje wiersze zagnieżdżonej tabeli w nieokreślonej kolejności. Jednak w przypadku pobrania tabeli zagnieżdżonej do zmiennej PL/SQL wiersze otrzymują kolejne indeksy dolne, zaczynając od 1. Zapewnia to podobny dostęp do poszczególnych wierszy.
* **VARRYS** nie są obsługiwane w łączniku.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schemat 5 (Definiowanie partycji dla atrybutów odwołania)
Na tej stronie można skonfigurować dla wszystkich atrybutów referencyjnych, do których odwołuje się partycja (typ obiektu).

![schema5](./media/microsoft-identity-manager-2016-connector-genericsql/schema5.png)

Jeśli używasz **nazwy wyróżniającej** , należy użyć tego samego typu obiektu, z którego się odwołujesz. Nie można odwołać się do innego typu obiektu.

> [!NOTE]
> Począwszy od aktualizacji 2017 marca istnieje teraz opcja dla "*", gdy ta opcja jest wybrana, wszystkie możliwe typy elementów członkowskich zostaną zaimportowane.

![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/any-option.png)

> [!IMPORTANT]
>  Od maja 2017 wartość " \* " oznacza, że **każda opcja** została zmieniona w celu obsługi przepływu importu i eksportu. Jeśli chcesz użyć tej opcji, ta wielowartościowa tabela/widok powinna mieć atrybut, który zawiera typ obiektu.

![](./media/microsoft-identity-manager-2016-connector-genericsql/any-02.png)

 </br> Jeśli wybrano wartość "*", należy również określić nazwę kolumny z typem obiektu.</br> ![](./media/microsoft-identity-manager-2016-connector-genericsql/any-03.png)

Po zaimportowaniu zobaczysz coś podobnego do poniższego obrazu:

  ![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Parametry globalne
Strona parametrów globalnych służy do konfigurowania różnicowego importu, daty/godziny i metody hasła.

![globalparameters1](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters1.png)



Ogólny łącznik SQL obsługuje następujące metody importu różnicowego:

* **Wyzwalacz** : zobacz [Generowanie widoków różnicowych przy użyciu wyzwalaczy](https://technet.microsoft.com/library/cc708665.aspx).
* **Znak wodny** : ogólne podejście, które może być używane z dowolnymi bazami danych. Zapytanie o znak wodny jest wstępnie wypełnione na podstawie dostawcy bazy danych. Kolumna limitu musi znajdować się w każdej używanej tabeli/widoku. Ta kolumna musi śledzić wstawienia i aktualizacje w tabelach jako i w tabelach zależnych (wielowartościowych lub podrzędnych). Zegary między usługą synchronizacji a serwerem bazy danych muszą być zsynchronizowane. W przeciwnym razie niektóre wpisy w imporcie różnicowym mogą zostać pominięte.  
  Bieg
  * Strategia nie obsługuje usuniętych obiektów.
* **Migawka** : (działa tylko z Microsoft SQL Server) [Generowanie widoków różnicowych przy użyciu migawek](https://technet.microsoft.com/library/cc720640.aspx)
* **Change Tracking** : (działa tylko z Microsoft SQL Server) na [temat Change Tracking](https://msdn.microsoft.com/library/bb933875.aspx)  
  Ograniczenia:
  * Atrybut zakotwiczenia & DN musi być częścią klucza podstawowego dla wybranego obiektu w tabeli.
  * Zapytanie SQL nie jest obsługiwane podczas importowania i eksportowania za pomocą Change Tracking.

**Dodatkowe parametry** : Określ strefę czasową serwera bazy danych wskazującą, gdzie znajduje się serwer bazy danych. Ta wartość jest używana do obsługi różnych formatów atrybutów czasu & Date.

Łącznik zawsze przechowuje datę i datę i godzinę w formacie UTC. Aby można było prawidłowo skonwertować datę i godziny, należy określić strefę czasową serwera bazy danych i format. Format powinien być wyrażony w formacie .NET.

Podczas eksportowania każdy atrybut daty i godziny musi być podany dla łącznika w formacie czasu UTC.

![globalparameters2](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters2.png)

**Konfiguracja hasła** : Łącznik zapewnia możliwości synchronizacji haseł i obsługuje ustawianie i Zmienianie hasła.

Łącznik udostępnia dwie metody obsługi synchronizacji haseł:

* **Procedura składowana** : Ta metoda wymaga dwóch procedur składowanych do obsługi ustawiania & zmienić hasła. Wpisz wszystkie parametry dla operacji Dodaj i Zmień hasło w polu **Ustaw hasło Sp** i **Zmień hasło Sp** parametrów odpowiednio do poniższego przykładu.
  ![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters3.png)
* **Rozszerzenie hasła** : Ta metoda wymaga biblioteki DLL rozszerzenia hasła (należy podać nazwę biblioteki DLL rozszerzenia implementującej interfejs [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) ). Zestaw rozszerzenia hasła musi być umieszczony w folderze rozszerzeń, aby łącznik mógł załadować bibliotekę DLL w czasie wykonywania.
  ![globalparameters4](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters4.png)

Należy również włączyć zarządzanie hasłami na stronie **Konfigurowanie rozszerzenia** .
![globalparameters5](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie
Na stronie partycje i hierarchie wybierz pozycję Wszystkie typy obiektów. Każdy typ obiektu jest własną partycją.

![partitions1](./media/microsoft-identity-manager-2016-connector-genericsql/partitions1.png)

Możesz również zastąpić wartości zdefiniowane na stronie **łączność** lub **parametry globalne** .

![partitions2](./media/microsoft-identity-manager-2016-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Konfiguruj zakotwiczenia
Ta strona jest tylko do odczytu, ponieważ zakotwiczenie zostało już zdefiniowane. Wybrany atrybut zakotwiczenia jest zawsze dołączany do typu obiektu, aby upewnić się, że pozostaje unikatowy w obrębie typów obiektów.

![Kotwice](./media/microsoft-identity-manager-2016-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Skonfiguruj parametr kroku przebiegu
Te kroki są konfigurowane w profilach uruchamiania na łączniku. Te konfiguracje służą do wykonywania rzeczywistej pracy nad importowaniem i eksportowaniem danych.

### <a name="full-and-delta-import"></a>Import pełny i Delta
Ogólny łącznik SQL obsługuje importowanie pełne i różnicowe przy użyciu następujących metod:

* Tabela
* Widok
* Procedura składowana
* Zapytanie SQL

![runstep1](./media/microsoft-identity-manager-2016-connector-genericsql/runstep1.png)

**Tabela/widok**  
Aby zaimportować atrybuty wielowartościowe dla obiektu, należy podać nazwę tabeli/widoku w **nazwie wielowartościowej tabeli/widoków** i odpowiednich warunków sprzężenia w **warunku sprzężenia** z tabelą nadrzędną. Jeśli źródło danych zawiera więcej niż jedną tabelę wielowartościową, można użyć Union do jednego widoku.

> [!IMPORTANT]
> Ogólny agent zarządzania SQL może korzystać tylko z jednej tabeli wielowartościowej. Nie umieszczaj w nazwie wielowartościowej tabeli/widoków więcej niż jednej nazwy tabeli. Jest to ograniczenie ogólnego języka SQL.


Przykład: chcesz zaimportować obiekt Employee i wszystkie jego atrybuty wielowartościowe. Istnieją dwie tabele o nazwie Employee (główna tabela) i dział (wiele wartości).
Wykonaj następujące czynności:

* Wpisz **Pracownik** w **tabeli/widoku/Sp** .
* Dział typów w **nazwie wielowartościowej tabeli/widoków** .
* Na przykład wpisz warunek sprzężenia między działem & pracownika w **warunku sprzężenia** `Employee.DEPTID=Department.DepartmentID` .
  ![runstep2](./media/microsoft-identity-manager-2016-connector-genericsql/runstep2.png)

**Procedury składowane**  
![runstep3](./media/microsoft-identity-manager-2016-connector-genericsql/runstep3.png)

* Jeśli masz dużo danych, zaleca się zaimplementowanie stronicowania przy użyciu procedur składowanych.
* Aby można było obsłużyć procedurę składowaną stronicowania, należy podać indeks początkowy i końcowy. Zobacz: [wydajne stronicowanie przy użyciu dużej ilości danych](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndex i @EndIndex zostały zastąpione w czasie wykonywania z odpowiednią wartością rozmiaru strony skonfigurowaną na stronie **Konfiguruj krok** . Na przykład gdy łącznik Pobiera pierwszą stronę, a rozmiar strony jest ustawiony na 500, w takiej sytuacji @StartIndex może być 1 i @EndIndex 500. Te wartości zwiększają się, gdy łącznik pobiera kolejne strony i zmienia @StartIndex @EndIndex wartość &.
* Aby wykonać sparametryzowane procedury składowane, podaj parametry w `[Name]:[Direction]:[Value]` formacie. Wprowadź każdy parametr w osobnym wierszu (Użyj kombinacji klawiszy CTRL + ENTER, aby uzyskać nowy wiersz).
* Ogólny łącznik SQL obsługuje również operację importowania z połączonych serwerów w Microsoft SQL Server. Jeśli informacje powinny być pobierane z tabeli na połączonym serwerze, należy podać tabelę w formacie: `[ServerName].[Database].[Schema].[TableName]`
* Ogólny łącznik SQL obsługuje tylko te obiekty, które mają podobną strukturę (nazwę i typ danych aliasu) między informacjami o uruchamianych krokach i wykrywaniem schematu. Jeśli wybrany obiekt z schematu i podane informacje w kroku przebiegu są inne, łącznik SQL nie obsługuje tego typu scenariuszy.

**Zapytanie SQL**  
![runstep4](./media/microsoft-identity-manager-2016-connector-genericsql/runstep4.png)

![runstep5](./media/microsoft-identity-manager-2016-connector-genericsql/runstep5.png)

* Zapytania z wieloma zestawami wyników nie są obsługiwane.
* Zapytanie SQL obsługuje stronicowanie i zapewnia indeks początkowy oraz indeks końcowy jako zmienną do obsługi stronicowania.

### <a name="delta-import"></a>Import zmian
![runstep6](./media/microsoft-identity-manager-2016-connector-genericsql/runstep6.png)

Konfiguracja importu różnicowego wymaga pewnej konfiguracji w porównaniu z pełnym importowaniem.

* W przypadku wybrania podejścia wyzwalacza lub migawki w celu śledzenia zmian różnicowych Podaj tabelę historii lub bazę danych migawek w **tabeli historii lub polu Nazwa bazy danych migawek** .
* Należy również podać warunek sprzężenia między tabelą historii a tabelą nadrzędną, na przykład `Employee.ID=History.EmployeeID`
* Aby śledzić transakcję w tabeli nadrzędnej z tabeli historii, należy podać nazwę kolumny, która zawiera informacje o operacji (Dodaj/Aktualizuj/Usuń).
* W przypadku wybrania znaku wodnego do śledzenia zmian różnicowych Podaj nazwę kolumny, która zawiera informacje o operacji w polu **Nazwa kolumny znacznika** .
* Kolumna **atrybutu typu zmiany** jest wymagana dla typu zmiany. Ta kolumna mapuje zmianę występującą w tabeli podstawowej lub tabeli wielowartościowej na typ zmiany w widoku Delta. Ta kolumna może zawierać Modify_Attribute typ zmiany dla zmiany poziomu atrybutu lub Dodaj, Modyfikuj lub usuń typ zmiany dla typu zmiany na poziomie obiektu. Jeśli jest coś innego niż domyślna wartość Dodaj, Modyfikuj lub Usuń, możesz zdefiniować te wartości przy użyciu tej opcji.

### <a name="export"></a>Eksportowanie
![runstep7](./media/microsoft-identity-manager-2016-connector-genericsql/runstep7.png)

Ogólny łącznik SQL obsługujący eksport przy użyciu czterech obsługiwanych metod, takich jak:

* Tabela
* Widok
* Procedura składowana
* Zapytanie SQL

**Tabela/widok**  
W przypadku wybrania opcji tabela/widok łącznik generuje odpowiednie zapytania, aby wykonać eksport.

**Procedury składowane**  
![runstep8](./media/microsoft-identity-manager-2016-connector-genericsql/runstep8.png)

W przypadku wybrania opcji procedura składowana eksportowanie wymaga trzech różnych procedur składowanych do wykonywania operacji wstawiania/aktualizowania/usuwania.

* **Dodaj nazwę Sp** : Ta wtyczka jest uruchamiana, jeśli dowolny obiekt jest dostępny do łącznika do wstawienia w odpowiedniej tabeli.
* **Aktualizuj nazwę Sp** : Ta wtyczka jest uruchamiana, jeśli dowolny obiekt jest dostępny do łącznika dla aktualizacji w odpowiedniej tabeli.
* **Usuń nazwę Sp** : Ta wtyczka jest uruchamiana, jeśli dowolny obiekt jest dostępny do łącznika do usunięcia w odpowiedniej tabeli.
* Atrybut wybrany ze schematu używanego jako wartość parametru procedury składowanej. Na przykład, `@EmployeeName: INPUT: EmployeeName` w schemacie łączników jest wybrana wartość (EmployeeName), a łącznik zastępuje odpowiednie wartości podczas eksportowania.
* Aby uruchomić sparametryzowane procedury składowane, podaj parametry w `[Name]:[Direction]:[Value]` formacie. Wprowadź każdy parametr w osobnym wierszu (Użyj kombinacji klawiszy CTRL + ENTER, aby uzyskać nowy wiersz).

**Zapytanie SQL**  
![runstep9](./media/microsoft-identity-manager-2016-connector-genericsql/runstep9.png)

Jeśli wybierzesz opcję zapytania SQL, eksport wymaga trzech różnych zapytań do wykonywania operacji wstawiania/aktualizowania/usuwania.

* **Wstaw zapytanie** : to zapytanie jest wykonywane, jeśli dowolny obiekt jest dostępny do łącznika do wstawienia w odpowiedniej tabeli.
* **Zapytanie aktualizacji** : to zapytanie jest wykonywane, jeśli dowolny obiekt jest dostępny do łącznika dla aktualizacji w odpowiedniej tabeli.
* **Usuń zapytanie** : to zapytanie jest wykonywane, jeśli dowolny obiekt jest dostępny do łącznika do usunięcia w odpowiedniej tabeli.
* Atrybut wybrany ze schematu używanego jako wartość parametru do zapytania, na przykład `Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Rozwiązywanie problemów
* Aby uzyskać informacje na temat włączania rejestrowania w celu rozwiązywania problemów z łącznikiem, zobacz [jak włączyć śledzenie ETW dla łączników](https://go.microsoft.com/fwlink/?LinkId=335731).
