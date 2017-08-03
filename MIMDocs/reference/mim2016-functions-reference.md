---
title: "Funkcja odwołania dla programu Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Odwołanie do funkcji programu Microsoft Identity Manager 2016


W programie Microsoft Identity Manager (MIM) 2016, funkcje umożliwiają modyfikowanie wartości atrybutu przed przepływu ich do obiektu docelowego w działanie funkcji lub deklaratywne inicjowania obsługi administracyjnej. Celem niniejszego dokumentu jest zapewniają Przegląd funkcji dostępnych oraz opis korzystania z nich.

Konfigurowanie mapowania przepływu atrybutów jest podstawowe zadania, podczas konfigurowania reguły synchronizacji. Najprostsza forma mapowanie przepływu atrybutu jest bezpośredniego mapowania. Wskazywany przez nazwę bezpośredniego mapowania przyjmuje wartość atrybutu źródłowego i stosuje je do atrybutu docelowego skonfigurowany. Istnieją przypadki, w którym musi albo można zmodyfikować istniejące wartości atrybutu lub nowe wartości atrybutów, które mają być obliczane przed system stosuje je do miejsca docelowego.

Funkcje są wbudowane metody używane do definiowania typu modyfikacji należy aparatu synchronizacji do zastosowania podczas generowania wartości atrybutu dla miejsca docelowego.

W programie MIM można grupować istniejące funkcje na następujące kategorie:

-   **Funkcje manipulowania danymi**. Funkcje do wykonywania różnych operacji manipulowania na ciągach.

-   **Funkcje pobierania danych**. Funkcje, aby wyodrębnić dane z wartości atrybutów.

-   **Funkcje generowania danych**. Funkcje do generowania wartości.

-   **Funkcje logiki**. Funkcje do wykonywania operacji na podstawie warunków.

Poniższe sekcje zawierają szczegółowe informacje o funkcjach w każdej kategorii.

## <a name="data-manipulation-functions"></a>Funkcje manipulowania danymi

Funkcje manipulowania danymi są używane do wykonywania różnych operacji manipulowania na ciągach.

| Łączenie        |   |
|--------------------|-------------------------|
| Opis        | Funkcja ZŁĄCZ.teksty służy do dwóch lub więcej ciągów.                                                                                                       |
| Podpis funkcji | ciąg1 + ciąg2...                                                                                                                                                     |
| Dane wejściowe             | Dwa lub więcej ciągów                                                                                                                                                        |
| Operacje         | Wszystkie parametry ciągu wejściowego są połączone ze sobą.                                                                                                              |
| Dane wyjściowe             | Jeden ciąg        |


| Wielkie litery         |         |
|-------------------|---------|
| Opis        | Funkcja wielkie litery konwertuje wszystkie znaki w ciągu na wielkie litery.         |
| Podpis funkcji | Ciąg UpperCase(string)                                                                                                                                                   |
| Dane wejściowe             | Jeden ciąg                                                                                                                                                                 |
| Operacje         | Małymi literami parametru wejściowego są konwertowane na wielkich liter. Przykład: UpperCase("test") wynikiem "TEST".                                     |
| Dane wyjściowe             | Jeden ciąg                                                              |


| Małe litery          |                                 |
|--------------------|---------------------------------|
| Opis        | Funkcja małe konwertuje wszystkie znaki w ciągu na małe litery.                                                                                                  |
| Podpis funkcji | Ciąg LowerCase(string)                                                                                                                                                   |
| Dane wejściowe             | Jeden ciąg                                                                                                                                                                 |
| Operacje         | Wszystkie znaki na wielkie litery parametru wejściowego są konwertowane na małych liter. Przykład: LowerCase("TeSt") wynikiem "test".                                     |
| Dane wyjściowe             | Jeden ciąg               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Opis        | ProperCase funkcji konwertuje pierwszy znak każdego wyrazu rozdzielonych spacjami w ciągu na wielkie litery, a wszystkie inne znaki są konwertowane na małe litery.           |
| Podpis funkcji | Ciąg ProperCase(string)                                                                                                                                                  |
| Dane wejściowe             | Jeden ciąg                                                                                                                                                                 |
| Operacje         | Pierwszy znak każdego wyrazu rozdzielonych spacjami w parametrze wejściowym zostanie przekonwertowany na wielkie litery, a wszystkie znaki na wielkie litery są konwertowane na małych liter. Jeśli wyraz w parametrze wejściowym rozpoczyna się od znaku z systemem innym niż alfabetu, pierwszego znaku słowa nie jest konwertowany na wielkie litery. <br/> Przykłady: <br/> -ProperCase("TEsT") wynikiem "Test". <br/> -ProperCase("britta simon") wynikiem "Britta Simona". <br/>-ProperCase("TEsT") wynikiem "Test". <br/> -ProperCase("\$TEsT") wynikiem "\$Test".|
| Dane wyjściowe             | Jeden ciąg      |


| Przytp              |      |
|--------------------|------|
| Opis        | Funkcja LTrim usuwa spacji wiodących z ciągu.                                                                                                             |
| Podpis funkcji | Ciąg LTrim(string)                                                                                                                                                       |
| Dane wejściowe             | Jeden ciąg                                                                                                                                                                 |
| Operacje         | Wiodące białe znaki zawarte w parametrze wejściowym zostaną usunięte. <br/><br/>Przykład: Powoduje Przytp ("Test") "Test".                                              |
| Dane wyjściowe             | Jeden ciąg      |



| Przytk              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Opis        | Funkcja RTrim usuwa spacje końcowe z ciąg.                                                                 |
| Podpis funkcji | Ciąg RTrim(string)                                                                                                            |
| Dane wejściowe             | Jeden ciąg                                                                                                                      |
| Operacje         | Końcowe znaki odstępu zawarte w parametrze wejściowym zostaną usunięte. Przykład: Powoduje RTrim ("Test") "Test".  |
| Dane wyjściowe             | Jeden ciąg                                                                                                                      |


| TRIM               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Opis        | Funkcja przycinania usuwa spacji wiodących i końcowych białych z ciągu.                                                      |
| Podpis funkcji | Ciąg Trim(string)                                                                                                             |
| Dane wejściowe             | Jeden ciąg                                                                                                                      |
| Operacje         | Wiodące i końcowe znaki odstępu zawarte w parametrach są usuwane. Przykład: Przycinanie ("Test") powoduje "Test". |
| Dane wyjściowe             | Jeden ciąg                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Opis        | RightPad funkcja prawo podkładki typu ciąg, długość określonej za pomocą znak dopełnienia podana.                          |
| Podpis funkcji | Ciąg RightPad (ciąg, długość, padCharacter)                                                                                   |
| Operacje         | Jeśli długość ciągu jest mniejsza niż długość, następnie padCharacter jest wielokrotnie dołączany na końcu ciągu dopóki nie długości równej długości. <br/> Przykłady: <br/> -RightPad("User", 10, "0") spowoduje "User000000". <br/> -RightPad(RandomNum(1,10), 5, "0") może spowodować "9000".   |
| Dane wyjściowe                                                                                                                                                          | Jeśli ciąg ma długość większą niż lub równa długości, zostanie zwrócony ciąg jest taki sam jak ciąg. Jeśli długość ciągu jest mniejsza niż długość, nowy ciąg o długości żądaną zostanie zwrócony ciąg zawierający dopełniane przy padCharacter. Jeśli ciąg ma wartość null, funkcja zwraca pusty ciąg. |   |   |
>[!NOTE]
**padCharacter** może być znaku spacji, ale nie może mieć wartości null. Jeśli długość **ciąg** jest równa lub większa niż **długość**, **ciąg** zwróceniu niezmieniona.


| LeftPad      |     |
|----|-------|
| Opis  | LeftPad funkcja lewej podkładki typu ciąg, długość określonej za pomocą znak dopełnienia podana.    |
| Podpis funkcji      | Ciąg LeftPad (ciąg, długość, padCharacter)     |
| Dane wejściowe |  - **Ciąg.** Ciąg do konsoli. <br/> - **długość.** Liczba całkowita reprezentująca wymagana długość ciągu. <br/> - **padCharacter.** Ciąg, który składa się z jednego znaku ma być używany jako znak konsoli. |
| Operacje  | Jeśli długość ciągu jest mniejsza niż długość, następnie padCharacter jest wielokrotnie dołączany na początku ciąg dopóki nie długości równej długości. <br/> Przykłady: <br/> -LeftPad("User", 10, "0") spowoduje "000000User". <br/> LeftPad(RandomNum(1,10), 5, "0") może spowodować "0009". |  
|Dane wyjściowe | Jeśli ciąg ma długość większą niż lub równa długości, zostanie zwrócony ciąg jest taki sam jak ciąg. <br/> Jeśli długość ciągu jest mniejsza niż długość, nowy ciąg o długości żądaną zostanie zwrócony ciąg zawierający dopełniane przy padCharacter. <br/>  Jeśli **ciąg** jest wartość null, funkcja zwraca pusty ciąg.                                                   |

<[!NOTE]
**padCharacter** może być znaku spacji, ale nie może mieć wartości null. Jeśli długość **ciąg** jest równa lub większa niż **długość**, **ciąg** zwróceniu niezmieniona.

| BitOr    |  |
|----- |------|
| Opis  | Funkcja BitOr Ustawia określony bit flagi do 1.     |
| Podpis funkcji  | Int BitOr(mask, flag)       |  
| Dane wejściowe     | 1. **maski.** Wartość szesnastkowa, która określa bit do ustawienia flagi. <br/> 2. **flagi.** Wartość szesnastkowa musi mieć określone bit zmodyfikowane.    |   
| Operacje         | Ta funkcja konwertuje oba parametry binarna reprezentacja i porównuje je: <br/> — Ustawia nieco 1, jeśli jeden lub oba odpowiednich bitów w maski i flagi 1 i 0, gdy oba odpowiednich bitów są 0. <br/> -Zwraca 1 we wszystkich przypadkach, z wyjątkiem przypadków, w którym odpowiednich bitów oba parametry są równe 0. <br/> Bitowym wynikowy jest "set" (1 lub true) bity tych dwóch argumentów operacji. Jeśli wiele bitów ma wartość 1 w maski można ustawić wiele bity flagi.  |
| Dane wyjściowe             | Nowa wersja **flagi**, z bitami określone w **maski** ustawioną wartość 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Opis        | Funkcja BitAnd Ustawia określony bit flagi 0.                           |
| Podpis funkcji | Int BitOr(mask, flag)                                                              |
| Dane wejściowe             | 1. **maski.** Wartość szesnastkowa określający bit do modyfikowania flagę. <br/> 2. **flagi.** Wartość szesnastkowa, który musi mieć określone bit zmodyfikowane   |
| Operacje         | Ta funkcja konwertuje oba parametry binarna reprezentacja i porównuje je: <br/> — Ustawia bit 0, jeśli jeden lub oba odpowiednich bitów w **maski** i **flagi** są 0 i 1, jeśli oba odpowiednich bitów 1. <br/> We wszystkich przypadkach, z wyjątkiem przypadków, w których odpowiednich bitów oba parametry są 1 — zwraca wartość 0. Wiele bity flagi można ustawić na 0, jeśli wiele bitów ma wartość 0 w **maski.** |
| Dane wyjściowe             | Nowa wersja **flagi** z bitami określone w **maski** równa 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Opis       | Funkcja DateTimeFormat jest używany do formatowania wartości daty i godziny w postaci ciągu, w określonym formacie.     |
| Podpis funkcji   | Ciąg DateTimeFormat (daty/godziny, format)      |
| Dane wejściowe   | 1. daty/godziny. Ciąg reprezentujący datę i godzinę do formatowania.  <br/> 2. **format.** Ciąg reprezentujący można przekonwertować na format.  |   
| Operacje           | Ciąg formatu określonego w formacie jest stosowany do daty/godziny w ciągu daty/godziny. <br/> Ciąg określony w formacie musi mieć prawidłowy format daty/godziny. Jeśli nie jest dostępne, zostanie zwrócony błąd wskazujący, że format nie jest prawidłowy format daty/godziny. <br/> Przykład: DateTime ("12/25/2007", "RRRR MM-dd") powoduje "2007-12-25".|   
| Dane wyjściowe     | Ciąg wynikające ze stosowania **format** do **daty/godziny.**   |

>[!Note]                                                                                                                                                                             
Znaki zaakceptować, aby utworzyć formaty zdefiniowane przez użytkownika, zobacz [formaty daty/godziny zdefiniowane przez użytkownika](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Opis       | ConvertSidToString konwertuje tablicę bajtów zawierającą identyfikator zabezpieczeń na ciąg.         |
| Podpis funkcji      | Ciąg ConvertSidToString(ObjectSID)    |
| Dane wejściowe  | **ObjectSID.** Tablica bajtów zawierająca identyfikator zabezpieczeń (SID).   |
| Operacje    | Określony identyfikator SID binarny jest konwertowana na ciąg.    |
| Dane wyjściowe              | Reprezentacja ciągu identyfikatora SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Opis         | **ConvertStringToGuid** funkcja konwertuje reprezentację ciągu identyfikatora GUID to binarna reprezentacja identyfikatora GUID.      |
| Funkcja            | Byte [] ConvertStringToGuid(stringGuid)  |  
| Dane wejściowe              | **Identyfikator GUID.** Ciąg sformatowany w tym wzorcu: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, gdzie wartość identyfikatora GUID jest reprezentowany jako ciąg cyfr szesnastkowych w grupach, 8, 4, 4, 4, a 12 cyfr i rozdzielone myślnikami. Przykład zwracanej wartości to "382c74c3-721d-4f34-80e557657b6cbc27".  |
| Operacje          | Ciąg **Guid** określony w parametrze 1 jest konwertowana na jego reprezentacja binarna. <br/> Jeśli ciąg nie jest prawidłowy reprezentacja **Guid**, funkcja odrzuca argumentu z powodu następującego błędu: <br/> **Parametr funkcji ConvertStringToGuid musi być prawidłowym identyfikatorem Guid reprezentującym.**  |
| Dane wyjściowe              | Binarna reprezentacja identyfikatora Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Opis         | Funkcja ReplaceString zamienia wszystkie wystąpienia ciągu na inny ciąg.  |   
| Funkcja            | Ciąg ReplaceString (ciąg, OldValue, NewValue)    |                                                                          
| Dane wejściowe              | 1. **Ciąg.** Ciąg, w którym zastąpienie wartości. <br/> 2. **OldValue.** Ciąg do wyszukania oraz do zastąpienia. <br/> 3. NewValue. Ciąg do zastąpienia w. |
| Operacje          | Wszystkie wystąpienia OldValue w parametrach są zastępowane NewValue. Funkcja musi mieć możliwość obsługi następujących znaków specjalnych: <br/> - **\n.** Nowy wiersz. <br/> - **\r.** Powrót karetki. <br/> - **\t.** Karta. <br/> Przykład: ReplaceString ("One\n\rMicrosoft\n\r\Way", "\n\r","") zwraca "One Microsoft Way". |   
| Dane wyjściowe              | Ciąg zawierający wszystkie wystąpienia **OldValue** w ciągu zastąpione **NewValue.**      |

## <a name="data-retrieval-functions"></a>Funkcje pobierania danych

Funkcje pobierania danych są używane do wykonywania operacji, które pobrać żądaną znaków z ciągu.

| Word       |        |
|--------------------|---------------|
| Opis        | Funkcja Word zwraca słowa w ciągu, na podstawie parametrów opisujące ograniczniki oraz numer programu word do zwrócenia.                                                                |
| Podpis funkcji | Word ciąg (string, numer, ograniczniki)                                                                                                                                                                        |
| Dane wejściowe             | 1. **ciągu.** Ciąg, z którego ma zostać zwrócona wyrazu. <br/> 2. **numer.** Liczba identyfikująca numer word, jaki ma zostać zwrócony. <br/> 3. **ograniczników.** Ciąg reprezentujący ograniczników, które mają być używane do identyfikowania słów. |
| Operacje         | Każdy ciąg znaków w ciągu, który jest oddzielony przez jeden ze znaków w ograniczniki jest identyfikowany jako wyraz. Zwrócono słowa, które zostało znalezione w pozycji określony w parametrze 3 (numer): <br/> -Jeśli liczba < 1, zwraca pusty ciąg. <br/> — Jeśli ciąg ma wartość null, zwraca pusty ciąg. <br/><br/> Przykłady: <br/> 1. Word ("Test; funkcji %;", 3, "; $& %") zwraca "function". <br/> 2. Word ("Test; Funkcja", 2,";") Zwraca "" (ciąg pusty). 3. Word ("Test; funkcji %;", 0,"; $& %") zwraca ""(ciąg pusty).
| Dane wyjściowe             | Ciąg zawierający słowo na pozycji zostanie wyświetlony monit o podanie. Jeśli **ciąg** zawiera mniej niż liczbą słów lub **ciąg** nie zawiera słów identyfikowane przez **ograniczników,** zwracany jest pustym ciągiem. |  


| Lewe               |   |
|-------|-------|
| Opis        | Po lewej stronie funkcja zwraca określoną liczbę znaków z lewej strony ciągu.       |
| Podpis funkcji | Ciąg Left (ciąg, numChars)     |
| Dane wejściowe             | 1. **ciągu.** Ciąg, z którego ma zostać zwrócona znaków. 2. **numChars.** Liczba identyfikująca liczbę znaków do zwrócenia z początku ciąg.         |
| Operacje         | **numChars** znaki są zwracane z pierwszą pozycję ciągu. <br/> Przykład: Lewe ("Britta Simona", 3) zwraca "Bri".   |
| Dane wyjściowe             | Ciąg zawierający pierwszych znaków numChars w ciągu.  <br/> -Jeśli numChars = 0, zwraca pusty ciąg. <br/> — Jeśli numChars < 0, zwraca ciąg wejściowy. <br/> — Jeśli ciąg ma wartość null, zwraca pusty ciąg. |




| Prawe       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Opis | Funkcja PRAWY zwraca określoną liczbę znaków z prawej strony (Zakończ), ciągu.                                 |
| Podpis funkcji   | Prawo ciąg (string, numChars)   |
| Dane wejściowe      | 1. **Ciąg.** Ciąg, z którego ma zostać zwrócona znaków. <br/> 2. **numChars.** Liczba identyfikująca liczbę znaków do zwrócenia z końca ciągu.  |
| Operacje  | **numChars.** Zwracane są znaków od końca ciągu. <br/> Przykład: Prawa ("Britta Simona", 3) zwraca "mon".                  |
| Dane wyjściowe      | Ciąg zawierający ostatnie znaki numChars w ciągu. Jeśli numChars = 0, zwraca pusty ciąg. <br/> -Jeśli **numChars** < 0, zwraca ciąg wejściowy. <br/> — Jeśli ten ciąg ma wartość null, zwraca pusty ciąg. <br/> — Jeśli ciąg zawiera mniej znaków niż liczba numChars określony w, zostanie zwrócony ciąg jest taki sam jak ciąg. |




| MID         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Opis | Funkcja Mid zwraca określoną liczbę znaków od określonej pozycji w ciągu.                              |
| Podpis funkcji    | Ciąg Mid(string, pos, numChars)                                                                                             |
| Dane wejściowe      | 1. **ciągu.** Ciąg, z którego ma zostać zwrócona znaków.   <br/> 2. **pos.** Liczba identyfikująca pozycję początkową w ciągu dla zwracania znaków. <br/> 3. **numChars.** Liczba identyfikująca liczbę znaków do zwrócenia z pozycji w ciągu.  |
| Operacje  | Zwraca **numChars** znaków, zaczynając od pozycji **pos** w ciągu. <br/>Przykład: Mid ("Britta Simona", 3, 5) zwróci "itta". |
| Dane wyjściowe      | Ciąg zawierający **numChars** znaków od pozycji **pos** w ciągu: <br/> -Jeśli **numChars** = 0, zwraca pusty ciąg. <br/> -Jeśli **numChars** < 0, zwraca pusty ciąg. <br/> -Jeśli **pos** > długość ciągu, zwraca ciąg wejściowy. <br/> -Jeśli **pos** ≤ 0, zwraca ciąg wejściowy. <br/> -Jeśli **ciąg** ma wartość null, zwraca pusty ciąg. <br/> Jeśli istnieją nie **numChar** znaki pozostałych **ciąg** od pozycji **pos**, jak liczby znaków mogą być zwracane są zwracane.

## <a name="data-generation-functions"></a>Funkcje generowania danych

Funkcje generowania danych są używane do generowania wartości dla określonych typów danych.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Opis        | CRLF generuje powrotu i wiersza karetki źródła danych. Ta funkcja umożliwia dodanie nowego wiersza. |
| Podpis funkcji | Ciąg CRLF                                                                              |
| Dane wejściowe             | Brak parametrów                                                                            |
| Operacje         | Zwracany jest znaku CRLF.                                                                      |
|                    | Przykład: AddressLine1 CRLF() + AddressLine2 wyniki: <br/> -AddressLine1 <br/> -AddressLine2 |
| Dane wyjściowe             | Znaku CRLF jest dane wyjściowe.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Opis        | Funkcja RandomNum zwraca liczbę losową w określonym przedziale czasu.                                       |   
| Podpis funkcji | Int RandomNum(start, end)                                                                                         |   
| Dane wejściowe             | - **Uruchom**. Liczba identyfikująca dolny limit losowych wartości do wygenerowania.   <br/> - **end**. Liczba identyfikująca górny limit losowych wartości do wygenerowania.  |
| Operacje         | Liczba większa niż lub równa **start** i mniejsza niż lub równa **zakończenia** jest generowany. <br/>  Przykład: Random(0,999) może zwrócić 100.                      |
| Dane wyjściowe             | Liczbę losową z zakresu określonego przez **start** i **zakończenia**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Opis        | *EscapeDNComponent* metoda przetwarza ciągu wejściowego na podstawie typu agenta zarządzania, który jest używany. |
| Podpis funkcji | Ciąg EscapeDNComponent(string)                                                                                  |
| Dane wejściowe     | **ciąg**. Ciąg, który służy do przetwarzania nazwy wyróżniającej. Ciąg nie powinna zawierać znaków ze zmienionym znaczeniem. |
| Operacje | Metoda EscapeDNComponent z MIISUtils jest używany do wykonania tej operacji. Ta metoda przetwarza ciągu wejściowego na podstawie typu agenta zarządzania, który jest używany. <br/> Ponieważ agentów zarządzania różnych wymagają nazwy wyróżniającej różnych formatach, ta metoda przetwarza ciągów wejściowych oparte na typie agenta zarządzania. Typy Lightweight Directory Access Protocol LDAP) nazwa wyróżniająca takie asActive Directory® Domain Services, serwer katalogowy Sun (dawniej inne serwer katalogowy), programu Microsoft Exchange Server; Hierarchiczna nonLDAP, takich jak Microsoft programu Lotus Notes; i zewnętrznych, takich jak nazwy wyróżniające bazy danych i XML bez LDAP. <br/> ** Nazwa wyróżniająca LDAP: ** <br/> -Wszelkich nieprawidłowych znaków XML w części zawierającej wartość danej części są zakodowane w formacie szesnastkowym. <br/>-Wszelkie niedozwolone znaki (w tym nieprawidłowych znaków XML) w części zawierającej nazwę danej części generuje błąd. <br/> -Następujące znaki są anulowane: <br/> &nbsp;&nbsp;&nbsp;-Przecinkiem (',') <br/> &nbsp;&nbsp;&nbsp;-Znak równości ("=") <br/> &nbsp;&nbsp;&nbsp;— Znak Plus ("+") <br/> &nbsp;&nbsp;&nbsp;— Mniejsza-znak większości ("<") <br/> &nbsp;&nbsp;&nbsp;— Większa-znak większości (">") <br/> &nbsp;&nbsp;&nbsp;-Znak (#) <br/> &nbsp;&nbsp;&nbsp;-Średnika (';') <br/> &nbsp;&nbsp;&nbsp;-Ukośnika odwrotnego ("\') <br/> &nbsp;&nbsp;&nbsp;-Znak cudzysłowu ("" ") <br/> — Jeśli ostatni znak w ciągu jest spacją, miejsca została zmieniona. <br/> -Wszelkie nadmiarowe początkowe lub końcowe spacje wokół części nazwy są usuwane. <br/> — Dla agenta zarządzania XML Jeśli istnieje wiele części, następnie części są porządku alfabetycznym. <br/> — Jeśli określonych jest wiele części, ciąg nazwy wyróżniającej jest złączeniem pojedyncze ciągi oddzielone znak plus. <br/> -Błąd jest generowany, jeśli ciąg wejściowy nie jest poprawnie sformułowany, LDAP-wyróżniające ciąg nazwy. <br/><br/> **Hierarchiczna z systemem innym niż LDAP** <br/> -Tych agentów zarządzania nie obsługują wieloczęściowego składników. Jeśli wielu ciągów są przekazywane do EscapeDNComponent, jest zgłaszany ArgumentException. <br/> — Jeśli znaków w ciągu wejściowym nieprawidłowych znaków XML, jest zgłaszany ArgumentException. <br/> -Wszystkie przecinkami i ukośników odwrotnych w ciągu wejściowym będą miały zmienione znaczenie. <br/> — Jeśli ostatni znak w ciągu jest spacją, miejsca została zmieniona. <br/><br/> **Zewnętrznych:** <br/> 1. Jeśli żadnej części jest plikiem binarnym lub zawiera nieprawidłowy znak XML, ta część jest przechowywana jako wersji nieprzetworzone dane zakodowane w formacie szesnastkowym na początku ciąg jest poprzedzony znakiem '#'. Na przykład jeśli element "AxC" (gdzie x oznacza niedozwolony znak XML, takie jak "0x10"), tej części są kodowane jako "#410010004300". <br/> 2. W przeciwnym razie są anulowane wszystkich wystąpień z następujących znaków: <br/> &nbsp;&nbsp;&nbsp;-Ukośnika odwrotnego ("\') <br/> &nbsp;&nbsp;&nbsp;-Przecinkiem (',') <br/> &nbsp;&nbsp;&nbsp;— Znak Plus ("+") <br/> &nbsp;&nbsp;&nbsp;-Znak (#) <br/> 3. Jeśli ostatni znak w ciągu danego części jest spacją, miejsca zostanie zmienione znaczenie. <br/> 4. Jeśli określonych jest wiele części, ciąg nazwy wyróżniającej jest złączeniem pojedyncze ciągi oddzielone znak plus.
| Dane wyjściowe      | Ciąg zawierający prawidłową nazwę domeny.                                                                                                                  |   

>[!NOTE]
Sprawdzanie poprawności nazwy wyróżniającej jest mniej rygorystyczne niż składni zdefiniowane w specyfikacji LDAP. EscapeDNComponent(String[]) umożliwia nazwę części, aby zawierać dowolną kombinację jeden lub więcej znaków "" – "z", "A" — "Z", "0"-"9", "-", i ".". <br/>
Nie jest możliwe określenie części binarnej przy użyciu tej metody. Jednak jest możliwe części binarnej **CommitNewConnector** Jeśli nazwy wyróżniającej jest tworzony z atrybutów zakotwiczenia i jeden z atrybutów zakotwiczenia jest typem binarnym.


| Wartość null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Opis | Funkcja Null jest używane do definiowania tego MA nie ma atrybutu do ich współtworzenia i pierwszeństwo atrybutów powinno być kontynuowane z MA dalej. |   
| Podpis funkcji    | Pusty ciąg    |
| Dane wejściowe      | Brak parametrów                                                                                                                                             |   
| Operacje  | Zwrócona wartość Null. <br/> Przykład: IIF(Eq(domain), "Nieznany", Null())                                                                                           |   
| Dane wyjściowe      | Wartość Null jest danych wyjściowych.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Funkcje logiki
Funkcje logiki są używane do wykonywania operacji na podstawie warunków obliczone przez system.

| IIF        |  |
|-------------|---|
| Opis | Funkcja IIF zwraca jeden zestaw możliwych wartości na podstawie określonego warunku.    |
| Podpis funkcji   | Obiekt IIF (warunek, Wartość_dla_prawdy Wartość_dla_fałszu)   |                                                 |
| Dane wejściowe      | 1. **Warunek**. Dowolna wartość lub wyrażenie, które może przyjąć wartość true lub false. 2. **Wartość_dla_prawdy** wartość zwracana, gdy warunek jest TRUE. <br/> 3. **Wartość_dla_fałszu** wartość zwracana, gdy warunek jest FALSE. <br/><br/> Poniżej przedstawiono funkcje dostępne do użycia jako wyrażenia w funkcji IIF jako **warunek:** <br/> **Korektora** Ta funkcja porównuje dwa argumenty pod kątem równości. <br/> **NotEquals.** Ta funkcja porównuje dwa argumenty dla nierówności i zwrócenie wartości true, jeśli nie są takie same, jak i wartość false, jeśli są one takie same.<br/> Przykład: NotEquals (EmployeeType, "Wykonawcy")<br/> **Mniejsze.** Ta funkcja zawiera porównanie dwóch liczb, w przeciwnym razie zwraca wartość true, jeśli pierwsze jest mniejsze niż sekundę i wartość false.<br/>Przykład: LessThan (wynagrodzenie, 100000) <br/>**Większe.** Ta funkcja porównuje dwie liczb, zwrócenie wartości true, jeśli pierwsze jest większe niż drugi i wartość false w przeciwnym razie wartość.<br/> Przykład: GreaterThan (wynagrodzenie, 100000) <br/> **LessThanOrEquals.** Ta funkcja porównuje dwie liczb, zwrócenie wartości true, jeśli pierwsze jest mniejsze niż lub równa na sekundę i wartość false w przeciwnym razie wartość.<br/>Przykład: LessThanOrEquals (wynagrodzenie, 100000) <br/> **GreaterThanOrEquals.* Ta funkcja porównuje dwie liczb, zwrócenie wartości true, jeśli pierwszy jest większa niż lub równa sekundę i wartość false w przeciwnym razie wartość. <br/>Przykład: GreaterThanOrEquals (wynagrodzenie, 100000)<br/> IsPresent. Przyjmuje tej funkcji, jakie wejściowy atrybutu w schemacie ILM i zwraca wartość true, jeśli nie jest atrybutem null i wartość false, jeśli ten atrybut ma wartość null.|
| Operacje  | Jeśli **warunku** daje w wyniku wartość true, zwróć **Wartość_dla_prawdy.** W przeciwnym razie zwraca **Wartość_dla_fałszu.** <br/>Przykład: IIF (Eq (EmployeeType, "Intern"), "t-" + aliasu, aliasu) zwraca alias użytkownika z "t-" dodane na początku, jeśli użytkownik jest intern. W przeciwnym wypadku zwraca alias użytkownika, ponieważ jest. |
| Dane wyjściowe      | Dane wyjściowe **Wartość_dla_prawdy** Jeśli wynikiem warunku jest PRAWDA lub **Wartość_dla_fałszu** Jeśli wynikiem warunku jest FAŁSZ. |      
