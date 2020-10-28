---
title: Łącznik programu PowerShell | Microsoft Docs
description: W tym artykule opisano sposób konfigurowania łącznika programu Windows PowerShell dla firmy Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: a26d7f0fdc157f3f4dd8d3fedadaf7d63bac89c9
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760558"
---
# <a name="windows-powershell-connector-technical-reference"></a>Dokumentacja techniczna łącznika programu Windows PowerShell
W tym artykule opisano łącznik programu Windows PowerShell. Artykuł dotyczy następujących produktów:

* Microsoft Identity Manager 2016 (programie MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Należy użyć poprawki 4.1.3671.0 lub nowszej [KB3092178](https://support.microsoft.com/kb/3092178).

W przypadku programie MIM2016 i FIM2010R2 łącznik jest dostępny do pobrania z [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Omówienie łącznika programu PowerShell
Łącznik programu PowerShell umożliwia integrację usługi synchronizacji z systemami zewnętrznymi oferującymi interfejsy API oparte na programie Windows PowerShell. Łącznik udostępnia mostek między funkcjami platformy zarządzania łącznością rozszerzoną (ECMA2) opartą na wywołaniach i programu Windows PowerShell. Aby uzyskać więcej informacji na temat struktury ECMA, zobacz [Informacje o programie Extensible 2,2 Connectivity Management Agent](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika upewnij się, że na serwerze synchronizacji są dostępne następujące elementy:

* Microsoft .NET 4.5.2 Framework lub nowszy
* Windows PowerShell 2,0, 3,0 lub 4,0

Zasady wykonywania na serwerze usługi synchronizacji muszą być skonfigurowane tak, aby umożliwić łącznikowi uruchamianie skryptów programu Windows PowerShell. Jeśli skrypty, które są uruchamiane przez łącznik, są podpisane cyfrowo, skonfiguruj zasady wykonywania, uruchamiając to polecenie:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Utwórz nowy łącznik
Aby utworzyć łącznik programu Windows PowerShell w usłudze synchronizacji, należy podać serię skryptów programu Windows PowerShell, które wykonują kroki żądane przez usługę synchronizacji. W zależności od źródła danych, z którym jest nawiązywane połączenie oraz wymaganych funkcji, skrypty, które należy zaimplementować, różnią się. W tej sekcji omówiono każdy ze skryptów, które mogą być zaimplementowane i kiedy są wymagane.

Łącznik programu Windows PowerShell jest przeznaczony do przechowywania każdego skryptu w bazie danych usługi synchronizacji. Chociaż istnieje możliwość uruchamiania skryptów przechowywanych w systemie plików, łatwiej jest wstawiać treść każdego skryptu bezpośrednio do konfiguracji łącznika.

Aby utworzyć łącznik programu PowerShell, w obszarze **usługa synchronizacji** wybierz pozycję **agent zarządzania** i **Utwórz** . Wybierz łącznik **programu PowerShell (Microsoft)** .

![Utwórz łącznik](./media/microsoft-identity-manager-2016-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Łączność
Podaj parametry konfiguracji do nawiązania połączenia z systemem zdalnym. Te wartości są bezpiecznie przechowywane przez usługę synchronizacji i udostępniane skryptom programu Windows PowerShell po uruchomieniu łącznika.

![Łączność](./media/microsoft-identity-manager-2016-connector-powershell/connectivity.png)

Można skonfigurować następujące parametry łączności:

**Połączenia**


|              Parametr               | Wartość domyślna |                                                                                                                                                                                                                  Przeznaczenie                                                                                                                                                                                                                   |
|--------------------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                Serwer                |    <Blank>    |                                                                                                                                                                                             Nazwa serwera, z którym ma nawiązać połączenie łącznik.                                                                                                                                                                                              |
|                Domena                |    <Blank>    |                                                                                                                                                                                    Domena poświadczenia do przechowywania do użycia podczas uruchamiania łącznika.                                                                                                                                                                                    |
|                 Użytkownik                 |    <Blank>    |                                                                                                                                                                                   Nazwa użytkownika poświadczenia do przechowywania do użycia podczas uruchamiania łącznika.                                                                                                                                                                                   |
|               Hasło               |    <Blank>    |                                                                                                                                                                                   Hasło poświadczenia do przechowywania do użycia podczas uruchamiania łącznika.                                                                                                                                                                                   |
|    Personifikuj konto łącznika     |     Fałsz     | W przypadku wartości true usługa synchronizacji uruchamia skrypty programu Windows PowerShell w kontekście podanych poświadczeń. Jeśli jest to możliwe, zaleca się, aby parametr **$Credentials** był przesyłany do każdego skryptu zamiast personifikacji. Aby uzyskać więcej informacji na temat dodatkowych uprawnień wymaganych do korzystania z tej opcji, zobacz [dodatkowa konfiguracja personifikacji](#additional-configuration-for-impersonation). |
| Załaduj profil użytkownika podczas personifikacji |     Fałsz     |                          Instruuje system Windows, aby załadował profil użytkownika poświadczeń łącznika podczas personifikacji. Jeśli personifikowany użytkownik ma profil mobilny, łącznik nie załaduje profilu mobilnego. Aby uzyskać więcej informacji na temat dodatkowych uprawnień wymaganych do użycia tego parametru, zobacz [dodatkowa konfiguracja personifikacji](#additional-configuration-for-impersonation).                           |
|    Typ logowania podczas personifikacji     |     Brak      |                                                                                                                                                                      Typ logowania podczas personifikacji. Aby uzyskać więcej informacji, zapoznaj się z dokumentacją [dwLogonType][dw] .                                                                                                                                                                       |
|         Tylko podpisane skrypty          |     Fałsz     |                                                                                                    W przypadku wartości true łącznik programu Windows PowerShell sprawdza, czy każdy skrypt ma prawidłowy podpis cyfrowy. W przypadku wartości false upewnij się, że zasady wykonywania programu Windows PowerShell serwera usługi synchronizacji są RemoteSigned lub nie są ograniczone.                                                                                                     |

**Moduł wspólny**  
Łącznik umożliwia przechowywanie w konfiguracji udostępnionego modułu programu Windows PowerShell. Gdy łącznik uruchamia skrypt, moduł programu Windows PowerShell jest wyodrębniany do systemu plików, dzięki czemu może zostać zaimportowany przez każdy skrypt.

W przypadku skryptów synchronizacji importu, eksportu i hasła moduł wspólny jest wyodrębniany do folderu MAData łącznika. W przypadku skryptów odnajdywania schematu, walidacji, hierarchii i partycji moduł wspólny jest wyodrębniany do folderu% TEMP%. W obu przypadkach, wyodrębniony skrypt typowego modułu jest nazwany zgodnie z ustawieniem Nazwa skryptu typowego modułu.

Aby załadować moduł o nazwie FIMPowerShellConnectorModule. PSM1 z folderu MAData, użyj następującej instrukcji: `Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Aby załadować moduł o nazwie FIMPowerShellConnectorModule. PSM1 z folderu% TEMP%, należy użyć następującej instrukcji: `Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Sprawdzanie poprawności parametru**  
Skrypt walidacji to opcjonalny skrypt programu Windows PowerShell, którego można użyć, aby upewnić się, że parametry konfiguracji łącznika podane przez administratora są prawidłowe. Sprawdzanie poprawności serwera, poświadczeń połączenia i parametrów łączności są typowymi użyciami skryptu walidacji. Skrypt walidacji jest wywoływany po zmodyfikowaniu następujących kart i okien dialogowych:

* Łączność
* Parametry globalne
* Konfiguracja partycji

Skrypt walidacji otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |Karta Konfiguracja lub okno dialogowe, które wyzwoliło żądanie walidacji. |
| ConfigParameters |[[String][keyk] , [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |

Skrypt walidacji powinien zwrócić pojedynczy obiekt ParameterValidationResult do potoku.

**Odnajdywanie schematu**  
Skrypt odnajdywania schematu jest obowiązkowy. Ten skrypt zwraca typy obiektów, atrybuty i ograniczenia atrybutów używane przez usługę synchronizacji podczas konfigurowania reguł przepływu atrybutów. Skrypt odnajdywania schematu jest uruchamiany podczas tworzenia łącznika i wypełnia schemat łącznika. Jest on również używany przez akcję Odśwież schemat w Synchronization Service Manager.

Skrypt odnajdywania schematu otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk] , [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |

Skrypt musi zwrócić pojedynczy obiekt [schematu][schema] do potoku. Obiekt schematu składa się z obiektów [SchemaType][schemaT] , które reprezentują typy obiektów (na przykład: Users and Groups). Obiekt SchemaType przechowuje kolekcję obiektów [SchemaAttribute][schemaA] , które reprezentują atrybuty (na przykład: imię, nazwisko i adres pocztowy) typu.

**Dodatkowe parametry**  
Oprócz standardowych ustawień konfiguracji można zdefiniować dodatkowe niestandardowe ustawienia konfiguracji, które są specyficzne dla wystąpienia łącznika. Parametry te można określić na poziomach łącznika, partycji lub uruchamiania i uzyskać dostęp z odpowiedniego skryptu programu Windows PowerShell. Niestandardowe ustawienia konfiguracji mogą być przechowywane w bazie danych usługi synchronizacji w formacie zwykłego tekstu lub mogą być szyfrowane. Usługa synchronizacji automatycznie szyfruje i odszyfrowuje bezpieczne ustawienia konfiguracji, jeśli jest to wymagane.

Aby określić niestandardowe ustawienia konfiguracji, oddziel nazwę każdego parametru przecinkami (,).

Aby uzyskać dostęp do niestandardowych ustawień konfiguracji ze skryptu, musisz mieć sufiks nazwy z podkreśleniem ( \_ ) i zakresem parametru (Global, Partition lub RunStep). Aby na przykład uzyskać dostęp do globalnego parametru FileName, użyj tego fragmentu kodu: `$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Możliwości
Karta możliwości projektanta agenta zarządzania definiuje zachowanie i funkcjonalność łącznika. Nie można modyfikować opcji wprowadzonych na tej karcie, gdy łącznik został utworzony. W tej tabeli wymieniono ustawienia możliwości.

![Możliwości](./media/microsoft-identity-manager-2016-connector-powershell/capabilities.png)

| Możliwość | Opis |
| --- | --- |
| [Styl nazwy wyróżniającej][dnstyle] |Wskazuje, czy łącznik obsługuje nazwy wyróżniające i jeśli tak, jakiego stylu. |
| [Typ eksportu][exportT] |Określa typ obiektów, które są prezentowane dla skryptu eksportu. <li>AttributeReplace — zawiera pełen zestaw wartości dla atrybutu wielowartościowego, gdy atrybut ulega zmianie.</li><li>AttributeUpdate — zawiera tylko przyrosty do atrybutu wielowartościowego, gdy atrybut ulega zmianie.</li><li>MultivaluedReferenceAttributeUpdate — zawiera pełen zestaw wartości dla atrybutów wielowartościowych niezwiązanych z odwołaniami i tylko różnic dla atrybutów odwołania wielowartościowego.</li><li>ObjectReplace — zawiera wszystkie atrybuty obiektu po zmianie atrybutów</li> |
| [Normalizacja danych][DataNorm] |Instruuje usługę synchronizacji, aby znormalizować atrybuty zakotwiczenia przed ich udostępnieniem skryptom. |
| [Potwierdzenie obiektu][oconf] |Konfiguruje zachowanie oczekującego importowania w usłudze synchronizacji. <li>Normalne — domyślne zachowanie, które oczekuje, że wszystkie eksportowane zmiany zostaną potwierdzone przy użyciu importu</li><li>NoDeleteConfirmation — po usunięciu obiektu nie ma oczekującego importu.</li><li>NoAddAndDeleteConfirmation — po utworzeniu lub usunięciu obiektu nie ma oczekującego importu.</li> |
| Użyj nazwy wyróżniającej jako kotwicy |Jeśli styl nazwy wyróżniającej jest ustawiony na LDAP, atrybut zakotwiczenia dla obszaru łącznika jest również nazwą wyróżniającą. |
| Współbieżne operacje kilku łączników |Po zaznaczeniu tego pola wyboru można jednocześnie uruchamiać wiele łączników programu Windows PowerShell. |
| Partycje |Po zaznaczeniu tego pola łącznik obsługuje wiele partycji i odnajdywanie partycji. |
| Hierarchia |Po zaznaczeniu ten łącznik obsługuje hierarchiczną strukturę stylu LDAP. |
| Włącz Importowanie |Po zaznaczeniu tego łącznika importuje dane za pośrednictwem skryptów importu. |
| Włącz import różnicowy |Po zaznaczeniu tego pola łącznik może żądać różnic między skryptami importu. |
| Włącz eksportowanie |Po zaznaczeniu tego pola wyboru łącznik eksportuje dane za pośrednictwem skryptów eksportu. |
| Włącz pełny eksport |Po zaznaczeniu tej opcji, skrypty eksportu obsługują eksportowanie całego miejsca łącznika. Aby można było użyć tej opcji, należy również zaznaczyć pole wyboru Włącz eksportowanie. |
| Brak wartości referencyjnych w pierwszym przebiegu eksportu |Po zaznaczeniu tej opcji atrybuty odwołań są eksportowane w drugim przebiegu eksportu. |
| Włącz zmianę nazwy obiektu |Po zaznaczeniu tej opcji nazwy wyróżniające można modyfikować. |
| Delete-Add jako Zastąp |Po zaznaczeniu tej opcji operacje usuwania Dodaj są eksportowane jako pojedyncze zastąpienie. |
| Włącz operacje na hasłach |Po zaznaczeniu tej opcji obsługiwane są skrypty synchronizacji haseł. |
| Włącz opcję eksportowania hasła przy pierwszym przebiegu |Po zaznaczeniu tej opcji hasła ustawione podczas aprowizacji są eksportowane podczas tworzenia obiektu. |

### <a name="global-parameters"></a>Parametry globalne
Karta parametry globalne w projektancie agenta zarządzania umożliwia skonfigurowanie skryptów programu Windows PowerShell uruchamianych przez łącznik. Możesz również skonfigurować wartości globalne dla niestandardowych ustawień konfiguracji zdefiniowanych na karcie łączność.

**Odnajdywanie partycji**  
Partycja jest oddzielną przestrzenią nazw w jednym udostępnionym schemacie. Na przykład w Active Directory Każda domena jest partycją w obrębie jednego lasu. Partycja jest grupą logiczną dla operacji importu i eksportu. Importowanie i eksportowanie ma partycję jako kontekst, a wszystkie operacje odbywają się w tym kontekście. Partycje powinny reprezentować hierarchię w protokole LDAP. Nazwa wyróżniająca partycji jest używana podczas importowania, aby sprawdzić, czy wszystkie zwrócone obiekty znajdują się w zakresie partycji. Nazwa wyróżniająca partycji jest również używana podczas aprowizacji z programu Metaverse do obszaru łącznika w celu określenia partycji, z którą obiekt powinien zostać skojarzony podczas eksportowania.

Skrypt odnajdywania partycji otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |

Skrypt musi zwrócić pojedynczy obiekt [partycji][part] lub listę [T] obiektów partycji do potoku.

**Odnajdywanie hierarchii**  
Skrypt odnajdywania hierarchii jest używany tylko wtedy, gdy styl nazwy wyróżniającej to LDAP. Skrypt służy do przeglądania i wybierania zestawu kontenerów, które są uznawane za lub poza zakresem dla operacji importu i eksportu. Skrypt powinien udostępniać tylko listę węzłów, które są bezpośrednim elementem podrzędnym węzła głównego dostarczonego do skryptu.

Skrypt odnajdywania hierarchii otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |
| ParentNode |[HierarchyNode][hn] |Węzeł główny hierarchii, w której skrypt powinien zwrócić bezpośrednie elementy podrzędne. |

Skrypt musi zwrócić pojedynczy podrzędny obiekt HierarchyNode lub listę [T] podrzędnych obiektów HierarchyNode do potoku.

#### <a name="import"></a>Importuj
Łączniki obsługujące operacje importowania muszą implementować trzy skrypty.

**Rozpocznij Importowanie**  
Skrypt BEGIN import jest uruchamiany na początku kroku importowania przebiegu. W tym kroku można nawiązać połączenie z systemem źródłowym i wykonać kroki przygotowawcze przed zaimportowaniem danych z podłączonego systemu.

Skrypt BEGIN import otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje skrypt o typie przebiegu importu (Delta lub pełna), partycji, hierarchii, znaku wodnego i oczekiwanego rozmiaru strony. |
| Typy |[Schemat][schema] |Schemat zaimportowanego obszaru łącznika. |

Skrypt musi zwrócić pojedynczy obiekt [OpenImportConnectionResults][oicres] do potoku, na przykład: `Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Importuj dane**  
Skrypt importowania danych jest wywoływany przez łącznik do momentu, gdy skrypt nie wskazuje, że nie ma więcej danych do zaimportowania. Łącznik programu Windows PowerShell ma rozmiar strony 9 999 obiektów. Jeśli skrypt zwróci więcej niż 9 999 obiektów do zaimportowania, należy obsłużyć stronicowanie. Łącznik uwidacznia niestandardową Właściwość danych, której można użyć do przechowywania znaku wodnego, tak aby za każdym razem, gdy skrypt danych importu został wywołany, skrypt wznawia Importowanie obiektów, w których został pozostawiony.

Skrypt Importuj dane otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Przechowuje znak wodny (CustomData), który może być używany podczas importowania stronicowanego i importowania różnicowego. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje skrypt o typie przebiegu importu (Delta lub pełna), partycji, hierarchii, znaku wodnego i oczekiwanego rozmiaru strony. |
| Typy |[Schemat][schema] |Schemat zaimportowanego obszaru łącznika. |

Skrypt Importuj dane musi napisać obiekt listy [[CSEntryChange][csec]] do potoku. Ta kolekcja składa się z atrybutów CSEntryChange, które reprezentują każdy importowany obiekt. Podczas pełnego przebiegu importowania ta kolekcja powinna mieć pełny zestaw obiektów CSEntryChange, które mają wszystkie atrybuty dla każdego obiektu. Podczas importowania różnicowego obiekt CSEntryChange powinien zawierać różnice poziomu atrybutu dla każdego obiektu do zaimportowania lub pełną reprezentację obiektów, które uległy zmianie (tryb zastępowania).

**Zakończ Importowanie**  
Po zakończeniu przebiegu importowania zostanie uruchomiony skrypt końcowy importu. Ten skrypt powinien wykonywać wszystkie wymagane zadania oczyszczania (na przykład zamykanie połączeń z systemami i reagowanie na błędy).

Końcowy skrypt importu otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje skrypt o typie przebiegu importu (Delta lub pełna), partycji, hierarchii, znaku wodnego i oczekiwanego rozmiaru strony. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Informuje skrypt o przyczynie, że Importowanie zostało zakończone. |

Skrypt musi zwrócić pojedynczy obiekt [CloseImportConnectionResults][cicres] do potoku, na przykład: `Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Eksportowanie
Tak samo jak w przypadku architektury importu łącznika, łączniki obsługujące eksport muszą implementować trzy skrypty.

**Rozpocznij eksportowanie**  
Skrypt BEGIN Export jest uruchamiany na początku kroku eksportowania przebiegu. W tym kroku można nawiązać połączenie z systemem źródłowym i wykonać wszelkie kroki przygotowawcze przed wyeksportowaniem danych do podłączonego systemu.

Skrypt BEGIN Export otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje skrypt o typie przebiegu eksportu (Delta lub pełna), partycji, hierarchii i oczekiwanego rozmiaru strony. |
| Typy |[Schemat][schema] |Schemat eksportowanego obszaru łącznika. |

Skrypt nie powinien zwracać żadnych danych wyjściowych do potoku.

**Eksportuj dane**  
Usługa synchronizacji wywołuje skrypt eksportu danych tyle razy, ile jest to konieczne, aby przetwarzać wszystkie oczekujące eksporty. Jeśli przestrzeń łącznika ma więcej oczekujących eksportów niż rozmiar strony łącznika, skrypt eksportu danych może być wywoływany wiele razy i może wielokrotnie dla tego samego obiektu.

Skrypt eksportu danych otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |
| CSEntries |[CSEntryChange][csec] IList |Lista wszystkich obiektów obszaru łącznika z oczekującymi eksportami do przetworzenia podczas tego przebiegu. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje skrypt o typie przebiegu eksportu (Delta lub pełna), partycji, hierarchii i oczekiwanego rozmiaru strony. |
| Typy |[Schemat][schema] |Schemat eksportowanego obszaru łącznika. |

Skrypt eksportu danych musi zwrócić obiekt [PutExportEntriesResults][peeres] do potoku. Ten obiekt nie musi zawierać informacji o wyniku dla każdego wyeksportowanego łącznika, chyba że wystąpi błąd lub zmiana atrybutu zakotwiczenia. Na przykład, aby zwrócić obiekt PutExportEntriesResults do potoku: `Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Zakończ eksportowanie**  
Po zakończeniu przebiegu eksportu, końcowy skrypt eksportu zostanie uruchomiony. Ten skrypt powinien wykonywać wszystkie wymagane zadania oczyszczania (na przykład zamykanie połączeń z systemami i reagowanie na błędy).

Skrypt końcowy eksportu otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje skrypt o typie przebiegu eksportu (Delta lub pełna), partycji, hierarchii i oczekiwanego rozmiaru strony. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Informuje skrypt o przyczynach zakończenia eksportowania. |

Skrypt nie powinien zwracać żadnych danych wyjściowych do potoku.

#### <a name="password-synchronization"></a>Synchronizacja haseł
Łączników programu Windows PowerShell można używać jako celu do zmiany/resetowania haseł.

Skrypt hasła odbiera następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[[String][keyk], [ConfigParameter][cp]] |Tabela parametrów konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzone przez administratora na karcie łączność. |
| Partycja |[Podzielić][part] |Partycja katalogu, w której znajduje się CSEntry. |
| CSEntry |[CSEntry][cse] |Wpis miejsca na łączniku dla obiektu, który otrzymał zmianę lub zresetowanie hasła. |
| OperationType |String |Wskazuje, czy operacja jest resetowana ( **SetPassword** ), czy zmiana ( **ChangePassword** ). |
| PasswordOptions |[PasswordOptions][pwdopt] |Flagi określające zachowanie dotyczące resetowania hasła. Ten parametr jest dostępny tylko wtedy, gdy OperationType jest **SetPassword** . |
| OldPassword |String |Wypełniono przy użyciu starego hasła dla zmiany hasła. Ten parametr jest dostępny tylko wtedy, gdy OperationType jest **ChangePassword** . |
| NoweHasło |String |Wypełniane nowym hasłem obiektu, który powinien zostać ustawiony przez skrypt. |

Skrypt hasła nie powinien zwracać żadnych wyników do potoku programu Windows PowerShell. W przypadku wystąpienia błędu w skrypcie hasła skrypt powinien zgłosić jeden z następujących wyjątków, aby poinformować usługę synchronizacji o problemie:

* [PasswordPolicyViolationException][pwdex1] — zgłaszany, jeśli hasło nie spełnia zasad haseł w połączonym systemie.
* [PasswordIllFormedException][pwdex2] — zgłaszany, jeśli hasło jest niedozwolone dla połączonego systemu.
* [PasswordExtension][pwdex3] — zgłoszone dla wszystkich innych błędów w skrypcie hasła.

## <a name="sample-connectors"></a>Przykładowe łączniki
Aby zapoznać się z pełnym omówieniem dostępnych łączników przykładowych, zobacz [Kolekcja łączników programu Windows PowerShell][samp].

## <a name="other-notes"></a>Inne notatki
### <a name="additional-configuration-for-impersonation"></a>Dodatkowa konfiguracja personifikacji
Przyznaj użytkownikowi, który personifikuje następujące uprawnienia na serwerze usługi synchronizacji:

Dostęp do odczytu do następujących kluczy rejestru:

* HKEY_USERS \\ [SynchronizationServiceServiceAccountSID] \Software\Microsoft\PowerShell
* HKEY_USERS \\ [SynchronizationServiceServiceAccountSID] \Environment

Aby określić identyfikator zabezpieczeń (SID) konta usługi synchronizacji, uruchom następujące polecenia programu PowerShell:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Dostęp do odczytu do następujących folderów systemu plików:

* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\MaData \\ {connectorname}

Zastąp nazwę łącznika programu Windows PowerShell symbolem zastępczym {Connectorname}.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
* Aby uzyskać informacje na temat włączania rejestrowania w celu rozwiązywania problemów z łącznikiem, zobacz [jak włączyć śledzenie ETW dla łączników](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
