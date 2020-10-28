---
title: Microsoft Identity Manager 2016 z dodatkiem SP1 | Microsoft Docs
description: Kompleksowa lista warunków, do których odwołuje się Microsoft Identity Manager 2016 z dodatkiem SP1.
keywords: Terminologia
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/28/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: afe167cdcd6ca548ef34e802f5606bee6ba5b31e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760900"
---
# <a name="microsoft-identity-manager-2016-sp1-terminology"></a>Microsoft Identity Manager 2016 z dodatkiem SP1

Ten dokument zawiera kompleksową listę warunków, do których odwołuje się Microsoft Identity Manager 2016 z dodatkiem SP1.

## <a name="a"></a>A

**Sprawdzanie poprawności grupy Active Directory** : procedura zaimplementowana w programie MIM 2016, która zapewnia unikatowość nazwy konta grupy w domenie przechowywanej w Active Directory.

**przepływ pracy akcji** : przepływ pracy, który wykonuje akcję. Obejmuje to wysłanie wiadomości e-mail z powiadomieniem i zatwierdzenie zmian w bazie danych usługi MIM.

**działanie** : działanie przepływu pracy to podstawowy blok konstrukcyjny dla przepływów pracy Windows Workflow Foundation (WF). Obejmuje logikę, która jest inicjowana w czasie projektowania i czasu wykonywania podczas kompilowania i uruchamiania przepływów pracy.

**zestaw działań** : A. DLL lub. Plik EXE zawierający zestaw .NET, który implementuje logikę dla działania przepływu pracy.

**zakotwiczenie** : co najmniej jeden unikatowy atrybut typu obiektu, który nie zmienia i reprezentuje obiekt w połączonym źródle danych, z którym jest połączony obiekt przestrzeni łącznika (na przykład numer pracownika lub identyfikator użytkownika).

**zatwierdzenie** : zatwierdzenie jest punktem decyzji przepływu pracy, którego można użyć do uzyskania autoryzacji od osoby przed kontynuowaniem przepływu pracy.

**wiadomość e-mail o zatwierdzeniu** : Jeśli żądanie wymaga zatwierdzenia przed zatwierdzeniem, wiadomość e-mail o zatwierdzeniu jest wysyłana do zidentyfikowanych osób zatwierdzających.

**żądanie zatwierdzenia** : żądanie, które wymaga zatwierdzenia. Na przykład wiadomość e-mail wysłana przez program MIM 2016 do osoby zatwierdzającej jako część przetwarzania działania zatwierdzania.

**odpowiedź dotycząca zatwierdzenia** : odpowiedź na żądanie zatwierdzenia. Zawiera informacje o tym, czy żądanie zostało zatwierdzone, czy nie. Na przykład wiadomość e-mail wysłana z dodatku MIM dla programu Outlook w odpowiedzi na żądanie zatwierdzenia.

**folder wyszukiwania zatwierdzeń** : foldery wyszukiwania utworzone za pomocą dodatku MIM dla programu Outlook, które umożliwiają użytkownikowi przeglądanie oczekujących i ukończonych zatwierdzeń oraz aktualizacji żądań zatwierdzenia.

**próg zatwierdzenia** : Liczba pozytywnych komunikatów z odpowiedzią na zatwierdzenie, które są konieczne, aby żądanie kontynuowało przetwarzanie.

osoba **zatwierdzająca** : osoba, która przekaże zatwierdzenie żądania przejścia do kolejnego etapu. Odbierają one komunikaty o żądaniu zatwierdzenia, jeśli jest używany dodatek programu MIM for Outlook. Zobacz również wpis dla osoby zatwierdzającej eskalacji.

**przepływ atrybutów** : definiuje kierunek przepływu wartości atrybutów między usługą MIM a innymi systemami zewnętrznymi.

**aktywność uwierzytelniania** : działanie przepływu pracy, które weryfikuje tożsamość użytkownika. Na przykład bramę do resetowania hasła i Brama uwierzytelniania karty inteligentnej. Zobacz również wpis "Brama pytań i odpowiedzi".

**wyzwanie uwierzytelniania** : okno dialogowe, które wymaga od użytkownika podania odpowiedzi na potrzeby uwierzytelniania w programie MIM 2016. Na przykład pytania dla użytkownika dotyczące resetowania haseł.

**działanie wyzwania uwierzytelniania** : działanie Windows Workflow Foundation, które służy do konfigurowania wyzwania wystawionego dla użytkownika w celu uwierzytelniania w programie MIM 2016.

**przepływ pracy autoryzacji** : przepływ pracy z działaniami, które należy wykonać przed zatwierdzeniem żądania do bazy danych. Przykłady to weryfikacja i zatwierdzenie danych.
<br/>

## <a name="c"></a>C

**Wyczyść atrybut rejestracji** : ten atrybut czyści rejestrację skojarzoną z przepływem pracy uwierzytelniania. Na przykład w przypadku wyzwania pytania i odpowiedzi odpowiedzi są przechowywane w programie MIM 2016 w postaci danych rejestracyjnych. Po zaznaczeniu pola wyboru Rejestracja i zapisaniu przepływu pracy dane rejestracji zostaną usunięte, co wymaga od użytkowników ponownego zarejestrowania.

**obliczany element członkowski (lub członek)** : zestaw zasobów tylko do odczytu obliczany na podstawie kombinacji ręcznie zarządzanych elementów członkowskich i filtru.

**Łącznik** : obiekt w przestrzeni łącznika, który reprezentuje obiekt w połączonym źródle danych i jest aktualnie połączony z obiektem w obiekcie Metaverse przy użyciu wstępnie zdefiniowanych reguł. Katalog meta używa obiektów łączników do synchronizowania wartości atrybutów między połączonym źródłem danych a Metaverse.

**Filtr łącznika** : reguła, która służy do zapobiegania łączeniu obiektów przestrzeni łącznika z obiektami Metaverse.

**przestrzeń łącznika** : obszar przejściowy, który zawiera reprezentacje wybranych obiektów i atrybutów w połączonym źródle danych. Obiekt przestrzeni łącznika to obiekt w miejscu łącznika, który jest tworzony przez import danych z połączonego źródła danych lub przy użyciu reguł w programie MIM do tworzenia nowych obiektów w różnych połączonych źródłach danych. Te obiekty przechowują wartości atrybutów, które mogą być importowane lub eksportowane z odpowiednich obiektów w połączonym źródle danych.

**Count XPath** : wyrażenie XPath, które zwraca wartość liczbową, która ma być renderowana w nawiasach po nazwie wyświetlanej zasobu.

**element członkowski oparty na kryteriach** : zestaw zasobów tylko do odczytu obliczany na podstawie kombinacji elementów członkowskich grupy statycznej i filtru.

**członkostwo oparte na kryteriach** : Grupa, w której członkostwo grupy jest określane przez filtr. Zobacz również wpis "static Membership".

**członek między lasami** : członek grupy zabezpieczeń, którego konto użytkownika znajduje się w innym lesie niż konto grupy.

**Obliczanie grup obejmujących wiele lasów** : działanie zewnętrzne, które umieszcza między członkami lasu grupy w zestawie obcy Security Principal (FSP) skojarzonym z lasem, w którym znajduje się grupa.


**wyrażenie niestandardowe** : język opisu używany do definiowania funkcji lub przepływów atrybutów w trybie zaawansowanym.
<br/>

## <a name="d"></a>D

**zestaw docelowy (lub definicja zasobu docelowego po żądaniu)** : zestaw, w którym zasób jest przenoszony z powodu żądania zmiany atrybutów tego zasobu.

**domyślne działanie weryfikacji grupy** : działanie przepływu pracy, które określa, czy żądanie zarządzania grupami mogłoby naruszać konfigurację lub zasady programu MIM 2016 lub Active Directory.

**odłączenie** : obiekt w przestrzeni łącznika, który reprezentuje obiekt w połączonym źródle danych i nie jest obecnie połączony z obiektem w obiekcie Metaverse.

**obiekty rozłączne** : Istnieją trzy typy obiektów odłączeń: rozłączenia, jawne odłączenia i odfiltrowane odłączania.

**Nazwa wyświetlana** : atrybut zasobu, który pojawia się w interfejsie użytkownika w celu zidentyfikowania tego zasobu. Wartość użyta w nazwie wyświetlanej powinna być niejednoznaczna i czytelna. Należy podać nazwę wyświetlaną, jeśli chce użyć zasobu w różnych kontrolkach portalu programu MIM, takich jak selektor zasobów.

**Grupa dystrybucyjna** : Kolekcja zasobów, które są najczęściej użytkownikami i innymi grupami, które mogą być wysyłane jednocześnie pocztą e-mail, wysyłając je do skrzynki pocztowej dla grupy.

**Konfiguracja domeny** : zasób konfiguracji używany do modelowania Active Directory domen.

**Grupa lokalna domeny** : Grupa z lokalnym zakresem domeny jest grupą Active Directory, która zabezpiecza zasoby w danej domenie i które mogą zawierać członków z tego lasu lub dowolnego lasu zaufanego.

**upuść plik** : plik Drop jest plikiem dziennika XML reprezentującym potencjalne lub zachodzące eksport lub import.

**wartość atrybutu dynamicznego** : wartość atrybutu, który jest obliczany na podstawie innych atrybutów. Na przykład atrybut name jest obliczany przez połączenie podaną nazwę i nazwisko.

**Grupa dynamiczna** : Grupa, której członkostwo jest automatycznie ustalane i aktualizowane przez program MIM 2016 przez zagwarantowanie, że grupa zawiera wszystkie zasoby (takie jak osoby, grupy, komputery), które znajdują się w warunkach, które są wyrażane przy użyciu XPath.
<br/>

## <a name="e"></a>E

**Wyliczenie** : Lista zasobów zwróconych przez usługę MIM 2016.

**Eskalacja** : Jeśli zatwierdzenie nie zostanie zakończone w określonym czasie, zatwierdzenie zostanie eskalacjne, a do zatwierdzenia zostaną dodane dodatkowe osoby zatwierdzające.

**osoba zatwierdzająca eskalacji** : użytkownik, który odbiera komunikaty o żądaniu zatwierdzenia, jeśli zatwierdzanie nie powiedzie się. Zobacz również wpis "osoba zatwierdzająca".

**Explicit łącznik** : obiekt w miejscu łącznika, który jest połączony z obiektem w magazynie Metaverse i nie może zostać odłączony przez filtr łącznika. Jawny łącznik można utworzyć ręcznie tylko za pomocą łącznika i można go rozłączyć tylko przez zainicjowanie obsługi administracyjnej lub za pomocą łącznika.

**jawny odłączenie** : obiekt w miejscu łącznika, który nie jest połączony z obiektem w magazynie Metaverse i może być sprzężony tylko przy użyciu łącznika. Obiekt zostaje jawnym odłączeniem przez ręczne odłączenie obiektu za pomocą łącznika.

**eksport** : Eksportowanie to proces wypychania zmian do połączonych źródeł danych. Eksport jest zawsze operacją różnicową opartą na ostatnim pomyślnym zaimportowaniu. Program MIM przetwarza zmiany, ale nie przetwarza zmian w obszarze łącznika, dopóki zmiany nie zostaną potwierdzone przez nowy import. W zależności od typu używanego agenta zarządzania, zmiany zaimplementowane przez eksport mogą znajdować się na poziomie atrybutu, obiektu lub wartości.

**Eksportowanie przepływu atrybutów** : proces translacji atrybutów obiektu z Metaverse do obszaru łącznika. Ten proces może dotyczyć mapowań jeden-do-jednego, stosowanie rozszerzeń reguł do modyfikowania atrybutów lub ustawiania wartości atrybutów statycznych. Wyeksportowane atrybuty są przemieszczane w obszarze łącznika dla następnego eksportu do połączonego źródła danych.

**Extensible Assertion Markup Language (XAML)** : język oparty na języku XML, w którym reprezentowane są definicje przepływu pracy.

**Filtr zakresu systemu zewnętrznego** : określa zasoby, które są identyfikowane i filtrowane z katalogu źródłowego na podstawie określonego warunku.

**Typ zasobu systemu zewnętrznego** : jest to typ zasobu w systemie zewnętrznym, do którego są połączone zasoby programu MIM 2016.

**Flaga tworzenia zasobów systemu zewnętrznego** : parametr reguły synchronizacji wskazujący, czy zasób należy utworzyć w obszarze łącznika, jeśli jest to zależne od kryteriów relacji, które nie istnieją w systemie zewnętrznym. Zobacz flaga tworzenia zasobów w programie MIM 2016. <!-- Not sure what this is, should this be a term? -->

**zakres systemu zewnętrznego** : parametr reguły synchronizacji zawierającej filtr przedstawiający zasoby w systemie zewnętrznym, do którego ma zastosowanie reguła.
<br/>

## <a name="f"></a>F

**Filtr** : wyrażenie zawierające warunki filtru. Filtr dopasowuje zasób, jeśli każdy z warunków filtru zawartych w filtrze jest zgodny z zasobem. W programie MIM 2016 filtr używa składni XPath.

odłączony **Filtr** : obiekt w obszarze łącznika, który nie może być sprzężony, lub być rzutowany do obiektu w obiekcie Metaverse opartym na regułach filtru łącznika w skojarzonym agencie zarządzania.

**Agent zarządzania programu FIM** : Agent zarządzania, który synchronizuje się między usługą programu MIM 2016 i usługą synchronizacji programu MIM 2016.

**Usługa kliencka resetowania haseł programu FIM/MIM** : dotyczy to usługi serwera proxy, która znajduje się na komputerze użytkownika końcowego, który komunikuje się z serwerem programu MIM 2016.

**Rozszerzenia do resetowania haseł programu FIM/MIM** : dotyczy to kodu, który znajduje się na komputerze użytkownika końcowego, który rozszerza funkcje logowania systemu Windows w celu uwzględnienia samoobsługowego resetowania hasła.

**Flaga tworzenia zasobów programu FIM/MIM** : parametr reguły synchronizacji wskazujący, czy zasób powinien zostać utworzony w bazie danych programu MIM 2016, jeśli jest to zależne od kryteriów relacji, które nie istnieją. Zobacz również wpis "Flaga tworzenia zasobów systemu zewnętrznego".

**Funkcja** : składnik, który może być uwzględniony w regule synchronizacji lub definicji przepływu pracy do przetwarzania wartości danych.
<br/>

## <a name="g"></a>G

**brama** : działanie przepływu pracy używane w fazie uwierzytelniania przetwarzania żądania. Zobacz również wpis "Brama pytań i odpowiedzi".

**zagnieżdżanie grup** : pole definicji grupy, które określa, czy grupa zawiera inne grupy jako członków bieżącej grupy.

**zakres grupy** : pole definicji grupy, jedno lub "lokalne", "globalne" lub "uniwersalne". Aby uzyskać więcej informacji, zobacz Active Directory zakresem grupy.
<br/>

## <a name="i"></a>I

**Import** : proces przeniesienia połączonych obiektów źródła danych do obszaru łącznika na potrzeby tworzenia, modyfikowania, usuwania lub weryfikacji. Import może być operacją pełną lub różnicową. W przypadku pełnego importu program MIM żąda wszystkich wydzielonych obiektów z połączonego źródła danych i usuwa wszystkie obiekty tymczasowe, dla których odpowiedni obiekt nie został odebrany podczas tego importu. W efekcie ten krok profilu przebiegu jest przydatny do czyszczenia obiektów przemieszczania w obszarze łącznika. Obiekty, które zostały odebrane z połączonego źródła danych, są przemieszczane w obszarze łącznika. W przypadku importu różnicowego w celu zapewnienia żądanych wyników, połączone źródło danych musi implementować postać znaku wodnego. Połączone źródło danych używa znaku wodnego, aby wskazać, kiedy miało miejsce ostatnie zmiany w obiekcie. MIM odczytuje znak wodny, aby określić, co należy uwzględnić w imporcie różnicowym. Przykładem jest numer USN Active Directory.

**Import — atrybut Flow (IAF)** : przepływ atrybutu importu jest procesem importowania atrybutu z obszaru łącznika do Metaverse. Ten proces może polegać na zastosowaniu mapowań atrybutów jeden-do-jednego, przy użyciu rozszerzeń reguł do modyfikowania atrybutów lub ustawiania atrybutów statycznych.

**adres URL obrazu** : adres URL pliku obrazu, który ma być RENDEROWANY w interfejsie użytkownika portalu programu MIM 2016.

**przepływ początkowy** : początkowy przepływ jest przepływem wartości atrybutu, który jest stosowany tylko raz, gdy zasób jest tworzony po raz pierwszy. Oznacza to, że początkowe hasło jest tworzone tylko w przypadku tworzenia konta po raz pierwszy.

**interaktywny przepływ pracy** : przepływ pracy wymagający odpowiedzi od użytkownika żądającego zmiany, na przykład w celu przeprowadzania dodatkowych kontroli uwierzytelniania.
<br/>

## <a name="j"></a>J

**Join** : sprzężenie to proces łączenia obiektu przestrzeni łącznika z istniejącym obiektem Metaverse. Wartości atrybutów przepływają tylko między obiektami połączonymi.

**Dołącz do żądania grupy** : żądanie dodania użytkownika do grupy.
<br/>

## <a name="l"></a>L

**Blokada** : ustawienie konfiguracji dla zasobu osoby w bazie danych programu MIM 2016, które ogranicza tę osobę od uwierzytelniania do programu MIM 2016 lub do resetowania haseł.

**brama blokady** : działanie przepływu pracy w fazie uwierzytelniania przetwarzania żądania w celu zablokowania użytkownikowi, który nie powiódł się uwierzytelnić. Zobacz również wpis "blokada" i "Brama pytań i odpowiedzi".

**próg blokady** : jest to liczba całkowita, która określa, ile razy użytkownik może zakończyć pracę, zanim zostanie zablokowany przez czas trwania blokady.Ustawienie domyślne to 3. Dolny limit to 0, a górny limit to 99.

**czas trwania blokady** : jest to liczba całkowita, która określa czas trwania w minutach, przez który użytkownik jest zablokowany, po osiągnięciu progu blokady.Ustawienie domyślne to 15 minut.Dolny limit dla tego ustawienia wynosi 1, a górny limit to 9999.Górny limit umożliwia administratorowi ustawienie górnego limitu na więcej niż jeden dzień.

**Liczba progów blokady przed trwałą blokadą** : jest to kontrolka liczb całkowitych, która umożliwia administratorowi skonfigurowanie wartości liczbowej dla liczby przypadków, w których użytkownik może osiągnąć próg blokady, zanim zostanie trwale zablokowany.  Stała blokada oznacza, że użytkownik musi zostać odblokowany przez administratora systemu. Domyślnie jest to wartość 3.Zakresem tego ustawienia jest z zakresu od 1 do 99.
<br/>

## <a name="m"></a>M

**agent zarządzania** : Agent zarządzania (ma) nawiązuje połączenie z konkretnym połączonym źródłem danych z katalogiem. Jest on odpowiedzialny za przeniesienie danych z połączonego źródła danych do programu MIM i regułami, które określają, czy dane tożsamości mają być objęte uczestnictwem w programie MIM, czy z innymi połączonymi źródłami danych. Po zmodyfikowaniu danych w katalogu źródłowym agent zarządzania może wyeksportować dane do połączonych źródeł danych, aby zapewnić synchronizację połączonego źródła danych z danymi w programie MIM. Agent zarządzania jest używany zamiast używania łącznika opartego na agentach.

**Reguła zasad zarządzania (MPR)** : reguły zasad zarządzania (reguł MPR) zapewniają mechanizm modelowania reguł przetwarzania biznesowego dla żądań przychodzących do serwera programu MIM 2016. Kontrolują uprawnienia do żądania operacji na zasobach programu MIM 2016 wraz z przepływami pracy, które są wyzwalane przez te żądania.
   
   Istnieją dwa typy reguł MPR:
   
- Żądanie reguł MPR: Udziel uprawnień i uruchom przepływy pracy (wywoływane przed wykonaniem żądanej operacji).
- Ustawianie przejścia reguł MPR: uruchamiaj tylko przepływy pracy (reakcja na zastosowana zmiana stanu).
   
  Głównymi obiektami projektu dla reguł MPR są:
   
- Uprawnienia modelowania
- Mapowanie przepływów pracy modelowania
- Zmiany modelowania
- Modelowanie definicji zwrotnych
- Modelowanie dostępu nieuwierzytelnionego użytkownika
- Modelowanie zasad czasowych
- Uprawnienia filtru modelowania

**ręcznie zarządzany element członkowski** : członkostwo grupy lub zestawu, które składa się z ręcznie wybranej listy użytkowników, grup lub innych zasobów.

**Metaverse** : centralny magazyn danych używany przez program MIM zawiera zagregowane informacje o tożsamości z wielu połączonych źródeł danych, co zapewnia jeden globalny, Zintegrowany widok wszystkich połączonych obiektów. Nie można użyć elementu Metaverse jako tabeli ani widoku zagregowanych danych tożsamości dla żadnej aplikacji innej niż MIM, ponieważ może to spowodować uszkodzenie.

**monitorowana Skrzynka pocztowa** : Skrzynka pocztowa, którą usługa MIM 2016 monitoruje, aby otrzymywać zatwierdzenia i żądać wiadomości E-mail z dodatku MIM dla programu Outlook.
<br/>

## <a name="n"></a>N

**działanie powiadamiania** : działanie przepływu pracy w fazie akcji przetwarzania żądania, w którym MIM 2016 wysyła wiadomości e-mail do jednego lub kilku użytkowników w celu powiadomienia ich o żądaniu.

**komunikat powiadomienia** : wiadomość e-mail wysłana przez działanie powiadamiania. Zobacz również wpis "aktywność powiadamiania".
<br/>

## <a name="o"></a>O

**Objectid (ResourceID)** : atrybut zawierający unikatowy identyfikator globalny (GUID) przypisany przez program MIM 2016 do każdego zasobu podczas jego tworzenia. Jest on również znany jako identyfikator zasobu.

**Identyfikator obiektu** : sekwencja liczb używana jako identyfikator pola w certyfikacie cyfrowym X. 509 lub dla typu atrybutu lub klasy obiektów w usłudze katalogowej opartej na protokole LDAP. Identyfikatory obiektów są zwykle przypisane przez dostawców oprogramowania i treści wzorców.

**Typ operacji** : zażądano typu operacji dla zasobu zarządzanego przez program MIM 2016 za pomocą usługi sieci Web. Obejmuje to tworzenie i usuwanie zasobów oraz odczytywanie i modyfikowanie atrybutów zasobów. Ponadto operacje dodawania/usuwania umożliwiają zastosowanie dalszej kontroli operacji modyfikacji w celu kontrolowania tylko wartości atrybutów lub ich usuwania.

**operator** : element filtru, który określa porównanie lub inną relację między wartościami danych.

**zestaw początkowy (lub definicja zasobu docelowego przed żądaniem)** : zestaw, do którego należy zasób przed zmianą atrybutów tego zasobu.
<br/>

## <a name="p"></a>P

**parametr** : podczas aprowizacji nowych zasobów czasami możliwe jest podanie wartości atrybutów ze źródła zewnętrznego, takiego jak użytkownik. Wartość atrybutu jest przenoszona jako parametr, aby można było pomyślnie utworzyć nowy zasób.

**Partition** : logiczna ilość danych w przestrzeni łącznika. Agent zarządzania może utworzyć co najmniej jedną partycję, aby logicznie podzielić dane na oddzielne grupowania logiczne. Każdy wolumin danych jest przetwarzany indywidualnie podczas synchronizacji.

**Resetowanie hasła** : procedura, za pomocą której można zmienić hasło użytkownika na znaną wartość, w sytuacji, gdy użytkownik zapomniał lub utracił swoje hasło. Zobacz również wpis "Rejestracja".

**faza** : każde żądanie utworzenia, aktualizacji lub usunięcia zasobu jest przetwarzane przez trzy fazy przepływu pracy. W fazie uwierzytelniania można przeprowadzić dodatkowe sprawdzanie uwierzytelniania użytkownika żądającego. W fazie autoryzacji zbierane są wszelkie niezbędne zatwierdzenia. W fazie akcji działania są wykonywane po zatwierdzeniu żądania zmiany zasobu.

**obiekty zastępcze** : obiekty w przestrzeni łącznika, które reprezentują pojedynczy poziom hierarchii połączonego źródła danych. Na przykład jeśli chcesz synchronizować obiekty z lasem Active Directory, musisz zaimportować kontenery, które tworzą ścieżkę dla obiektów Active Directory. W przykładzie CN = MikeDan, OU = users, DC = Microsoft, DC = com, obiekty zastępcze zostałyby utworzone dla DC = com i DC = Microsoft, DC = com. Ponadto obiekt symbolu zastępczego może reprezentować obiekt w połączonym źródle danych, do którego zaimportowana wartość atrybutu odwołania (na przykład obiekt, do którego odwołuje się atrybut Manager w obiekcie użytkownika). Obiekty zastępcze nie zawierają wartości atrybutów i nie mogą być połączone z Metaverse.

**Zarządzanie zasadami** : Zarządzanie zasadami w programie MIM 2016 jest możliwe przez konsolę opartą na programie SharePoint na potrzeby tworzenia i wymuszania zasad. Rozszerzalne przepływy pracy oparte na Windows Workflow Foundation umożliwiają użytkownikom Definiowanie, Automatyzowanie i wymuszanie zasad zarządzania tożsamościami. Zarządzanie zasadami obejmuje również niejednorodną synchronizację tożsamości i spójność, którą uzyskuje się dzięki integracji szerokiej gamy systemów operacyjnych sieci, poczty e-mail, bazy danych, katalogu, aplikacji i prostego dostępu do pliku.

**Aktualizacja zasad (lub uruchomiona przy aktualizacji zasad)** : Jeśli przepływ pracy powinien zostać uruchomiony ponownie w wyniku zmiany zestawów lub reguł MPR do niego, flaga aktualizacji zasad służy do wskazywania tego.

**pierwszeństwo** : porządkowanie reguł synchronizacji.

**zestaw podmiotu zabezpieczeń** : zestaw użyty w regule zasad zarządzania w celu określenia zestawu zasobów (zazwyczaj użytkowników), który inicjuje ocenę reguły zasad zarządzania.

**zestaw podmiotu zabezpieczeń względem zasobu** : jest to właściwość zwrotna. Jej wartość jest definiowana w warunkach jednej z właściwości zasobów. Służy do definiowania dynamicznych zasad zarządzania, których warunki są oceniane w kontekście każdego przetwarzanego zasobu docelowego.

**projekcja** : proces tworzenia obiektu w obiekcie Metaverse opartym na regułach projekcji, a następnie automatycznego łączenia tego obiektu z istniejącym obiektem w przestrzeni łącznika.

**Inicjowanie obsługi** : proces tworzenia, zmiany nazwy i anulowania aprowizacji obiektów we wstępnie określonych miejscach łączników na podstawie zmian w obiekcie w Metaverse. Reguły aprowizacji można skonfigurować tak, aby były wywoływane za każdym razem, gdy obiekt Metaverse jest modyfikowany. Te reguły mogą wykonywać akcje na poziomie obiektu, takie jak tworzenie nowego obiektu obszaru łącznika lub rozłączanie istniejących obiektów obszaru łącznika, które są połączone z obiektem Metaverse.
<br/>

## <a name="q"></a>Q

**Brama pytań i odpowiedzi** : działanie przepływu pracy w fazie uwierzytelniania, w którym użytkownik żądający musi dostarczyć odpowiedzi na co najmniej jedno określone pytanie. To działanie jest zwykle używane podczas resetowania hasła, aby użytkownik mógł udowodnić swoją tożsamość przez umożliwienie użytkownikowi wyboru wstępnie określonych pytań, dla których ten użytkownik wie, dla którego użytkownik musi podać poprawną odpowiedź. Zobacz również wpis "Brama blokady".

**Wyzwanie dla pytań** i odpowiedzi: wyzwanie, które wymaga od użytkownika odpowiedzi na szereg pytań w celu uwierzytelnienia w programie MIM 2016.
<br/>

## <a name="r"></a>R

**ustawienia hasła losowego** : ustawienie określające liczbę znaków wymaganych do ustawienia hasła w katalogu zewnętrznym.

**Typ atrybutu odwołania** : typ atrybutu, w którym wartości atrybutu są atrybutami objectid (Globally Unique Identifier) innych zasobów w programie MIM 2016.

**integralność referencyjna** : ograniczenie w programie MIM 2016, w którym atrybut Reference nie może mieć jako wartość identyfikatora objectid zasobu, który został usunięty.

**rejestracja** : procedura konfigurowania samoobsługowego resetowania hasła dla użytkownika. Zobacz również wpis "Brama pytań i odpowiedzi".

**ponowna rejestracja** : aktualizowanie rejestracji dla wyzwania uwierzytelniania w programie MIM 2016, zwykle wymagane po zmianie zasad administracyjnych na potrzeby rejestracji resetowania haseł.

**Tworzenie relacji** : flagi konfiguracji reguły synchronizacji, które określają, czy zasoby mają być tworzone automatycznie w programie MIM 2016, czy też w systemie zewnętrznym, jeśli nie istnieją zasoby.

**kryteria relacji** : ustawienie reguły synchronizacji, która jest używana do dopasowania zasobów na serwerze i zasobach programu MIM w systemach zewnętrznych.

**Zakończenie relacji** : wskazuje, czy powiązane zasoby w innych systemach zewnętrznych powinny być odłączone (i prawdopodobnie usunięte), gdy reguła synchronizacji nie jest już stosowana.

**Zarządzanie żądaniami** : możliwość korzystania przez użytkownika z przesłanych żądań oraz zarządzania nimi i skojarzonych przepływów pracy.

**reguła zasad zarządzania żądaniami (RMPR)** : typ reguły zasad zarządzania, który jest obliczany i stosowany do przychodzących żądań w celu wykonania operacji. RMPRS są używane przede wszystkim do tworzenia definicji zasad dostępu w programie MIM. Innymi słowy, odpowiedź na sposób obsługi żądania. Po skonfigurowaniu RMPR osoba żądająca znajduje się w zestawie zaprojektowanym do wykonania operacji.

   Architektura programu MIM definiuje sześć różnych operacji, dla których można zdefiniować RMPR:
   
- Utworzyć zasób
- Usuń zasób.
- Odczytaj zasób.
- Dodaj wartość do atrybutu wielowartościowego.
- Usuń wartość z atrybutu wielowartościowego.
- Zmodyfikuj atrybut o pojedynczej wartości.
   
  Podczas definiowania RMPR należy wybrać co najmniej jedną z tych sześciu operacji. Operacje są zawsze definiowane w kontekście osoby żądającej. Każdy warunek wymaga definicji elementu docelowego. Operacja stosowana do elementu docelowego może spowodować przejście stanu zasobu docelowego. RMPR jest zawsze wywoływana przed wykonaniem żądanej operacji. Aby efektywnie określić wartość docelową warunku, należy skonfigurować dwa różne stany:
   
- Definicja zasobu docelowego przed żądaniem: stan elementu docelowego przed zastosowaniem żądania.
- Definicja zasobu docelowego po żądaniu: stan elementu docelowego po zastosowaniu żądania.
   
  Niezależnie od tego, czy konieczne jest zdefiniowanie obu Stanów, zależy od operacji zdefiniowanej przez RMPR. W operacji tworzenia żądany zasób nie ma stanu początkowego. W związku z tym wystarczy skonfigurować definicję zasobu docelowego po żądaniu dla operacji tworzenia.
   
  Operacja odczytu lub usuwania nie powoduje przejścia stanu. W przypadku tych dwóch typów operacji do żądania należy określić tylko definicję zasobu docelowego.
   
  W przypadku operacji modyfikowania lub wszystkich innych kombinacji operacji należy skonfigurować oba Stany, które mogą mieć te same wartości, jeśli nie ma żadnego przejścia stanu.
   
  Można wyznaczać powiązane zasoby względem zleceniodawcy (na przykład obiektu użytkownika żądającego, Menedżera użytkownika docelowego lub właściciela grupy docelowej).
   
  Najprostszą formą odpowiedzi na warunek jest przyznanie uprawnień do wykonania żądanych operacji. Oprócz przyznawania uprawnień można także definiować inne operacje jako odpowiedź na warunek w RMPR. W architekturze programu MIM te operacje są zdefiniowane w postaci przepływów pracy. W momencie przetworzenia danego RMPRu system może nie mieć wystarczających informacji, aby przyznać uprawnienia. W takim przypadku można zdefiniować dodatkowe czynności związane z uwierzytelnianiem i autoryzacją w RMPR, które mają zastosowanie do osoby wykonującej dane żądanie. Na przykład aby udzielić uprawnienia do wykonania żądanej operacji, może być konieczne ręczne Interakcje użytkownika w celu zatwierdzenia operacji.
   
  Na poniższej ilustracji przedstawiono kompletną architekturę RMPR:
   
  ![Architektura RMPR](media/mim-2016-sp1-terms/8f5054080d3f8bd9c73d93a5e3ed51f9.gif)
   
  Po utworzeniu nowego obiektu żądania w programie MIM system wysyła zapytanie do skonfigurowanego RMPRs na potrzeby pasujących obiektów, porównując warunki żądania ze skonfigurowanymi warunkami w RMPRs zarządzania. Jeśli pasujące RMPRs są zlokalizowane, są one stosowane do obiektu żądania w kolejce. Poniższy rysunek przedstawia ten proces:
   
  ![Zgodne RMPRs są zlokalizowane i stosowane do obiektu żądania znajdującego się w kolejce](media/mim-2016-sp1-terms/65bdb65aaffa55a90707ff6f7b13632d.gif)
   
  W portalu programu MIM uprawnienia do operacji muszą zostać jawnie przyznane. Innymi słowy, o ile nie zostanie to przyznane przez RMPR, wszystkie operacje na zasobach są odrzucane. Każdy obiekt żądania wymaga co najmniej jednego RMPR, które przyznaje uprawnienia do wykonywania żądanych operacji na obiekcie docelowym.

**obiekt żądania** : gdy użytkownik wykonuje zadanie w portalu programu MIM lub dodatku MIM dla programu Outlook, jest reprezentowane jako obiekt żądania. Obiekty żądania reprezentują wygodny mechanizm raportowania dla działań w systemie.

   Każdy obiekt żądania ma następujące składniki:
   
- Osoba żądająca: zasób, który prosi o wykonanie operacji.
- Operacja: Akcja, którą chce wykonać Obiekt żądający.
- Obiekt docelowy: zasób, który jest obiektem docelowym żądanej operacji.
   
  Logicznie, obiekt request jest implementacją następującej instrukcji:
   
  `The requester attempts to perform the following operation on this target...`
      
  Na poniższej ilustracji przedstawiono ogólną architekturę obiektu żądania:
   
  ![Architektura obiektów żądania](media/mim-2016-sp1-terms/788e9b3a4fcddb78ff4a05dcb5e61192.gif)
   
  Każdy obiekt żądania ma właściwość stanu wskazującą stan przetwarzania. Żądania przetwarzania mogą wymagać ręcznej interakcji w celu wykonania żądania. Na przykład Właściciel grupy może być wymagany do ręcznego zatwierdzenia żądania innego użytkownika w celu dołączenia do grupy. Oprócz interakcji ręcznej można także skonfigurować program MIM do automatycznego przetwarzania określonego żądania bez konieczności interakcji człowieka.
   
**model przetwarzania żądania** : model przetwarzania żądań w programie MIM składa się z trzech głównych faz:

- Faza 1: uwierzytelnianie
- Faza 2: Autoryzacja
- Faza 3: Akcja
   
  Przepływy pracy, z których każdy zawiera jeden lub więcej działań, mogą być dołączane do każdej z tych faz i uruchamiane w kontekście wykonywania jednego żądania. Żądanie można zainicjować za pomocą pojedynczego wywołania użytkownika do jednego z punktów końcowych usługi sieci Web lub za pośrednictwem użytkownika tworzącego żądanie w portalu programu MIM.
   
  Na poniższej ilustracji przedstawiono relacje składników przetwarzania żądań:
   
  ![Związek ze składnikami przetwarzania żądań](media/mim-2016-sp1-terms/0e57445d8b9a3216c31add80eb26493f.gif)
   
  Żądania są przetwarzane w następującej kolejności:
   
- Żądanie utworzenia obiektu: program MIM 2016 tworzy obiekt request w odpowiedzi na wywołanie jednego z punktów końcowych usługi sieci Web lub z powodu żądania zainicjowanego za pomocą portalu MIM.
   
- Ocena MPR: prawa osoby żądającej do żądania akcji są sprawdzane, a obliczenia odpowiednich przepływów pracy są wykonywane. Żądanie jest sprawdzane pod kątem mapowań do dowolnych obiektów MPR. Aby zamapować na regułę MPR, należy dopasować wszystkie odpowiednie pola elementu MPR dla wymaganej operacji. Obejmuje to obiekt żądający, operacja, zasób docelowy i atrybuty. Jeśli wszystkie te warunki, łącznie z atrybutami, których to dotyczy, są spełnione dla żądania przychodzącego, do żądania jest dopasowywany odpowiedni element MPR. Żądanie musi być zmapowane do co najmniej jednej reguły MPR, która przyznaje uprawnienia w ramach swojej definicji. Jeśli wartość jest równa true, żądanie przechodzi przez etap sprawdzania uprawnień przetworzenia żądania. Jeśli to nie jest prawda, żądanie nie powiedzie się. System określa również przejścia, które są częścią żądania, i lokalizuje wszystkie powiązane z nimi reguł MPR.
   
- Uwierzytelnianie: w programie MIM 2016 uruchamiane są przepływy pracy uwierzytelniania pojedynczo w kolejności niedeterministycznej w celu potwierdzenia tożsamości żądającej.
   
- Autoryzacja: MIM 2016 potwierdza uprawnienie żądającego do wykonania żądanej operacji względem zasobu określonego w żądaniu. Wszystkie zależne przepływy pracy autoryzacji są uruchamiane równolegle, ale żądanie nie jest zatwierdzane do magazynu obiektów programu MIM, chyba że wszystkie przepływy pracy zostały ukończone i wszystkie zakończyły się powodzeniem.
   
- Przetwarzanie: MIM 2016 wykonuje żądaną operację w sklepie z aplikacjami programu MIM.
   
- Akcja: program MIM 2016 wykonuje wszystkie procesy, które mają zostać wykonane ze względu na żądaną operację. Wszystkie przepływy pracy akcji są uruchamiane równolegle. Operacje odczytu nie mają żadnych przepływów pracy zastosowanych do ich przetwarzania. Obejmuje to skonfigurowane przepływy pracy w RMPR oraz przepływy pracy w reguł MPR zestawu.
   
  >[!NOTE]
  >Żądania inicjowane przez konto synchronizacji pomijają wszystkie przepływy pracy uwierzytelniania i autoryzacji, które mają zastosowanie do nich. Stosowane są wszystkie odpowiednie przepływy pracy akcji.

**Obiekt żądający** : tożsamość użytkownika lub usługi, która przesłała żądanie do programu MIM 2016.

**zakres osoby żądającej** : skonfigurowana kolekcja użytkowników, którzy mogą przesłać żądanie. Może to być "Wszyscy" lub określony zestaw użytkowników zdefiniowany przez filtr.

**zasób** : wystąpienie określonego typu zasobu w programie MIM 2016. Każdy zasób jest jednoznacznie identyfikowany przez jego atrybut ObjectID (ResourceID).

**Konfiguracja wyświetlania formantów zasobów (RCDC)** : RCDCs to zasoby konfiguracji, które są używane do renderowania interfejsu użytkownika w formancie zasobów (RC) na potrzeby tworzenia określonego typu zasobu w programie MIM 2016.

**bieżący zestaw zasobów** : część definicji warunkowej reguł zarządzania (MPR). Kolekcja zasobów docelowych w momencie odebrania żądania. Dotyczy typów operacji odczytu, usuwania i modyfikowania.

**zestaw końcowy zasobów** : część definicji warunku w regułach zasad zarządzania. Kolekcja zasobów docelowych po przetworzeniu żądania. Dotyczy tylko typów operacji tworzenia i modyfikowania.

**Hierarchia zasobów** : w usłudze katalogowej hierarchia wpisu zasobu jest kolekcją wpisów katalogu między podstawą kontekstu nazewnictwa a tym wpisem zasobu.

**zakres zasobów** : zestaw zasobów, dla których można przesłać żądanie.

**Typ zasobu** : część schematu, która definiuje reprezentację zasobu w programie MIM 2016.

**Mapowanie typu zasobu** : relacja między typem zasobu używanym do reprezentowania zasobu w programie MIM 2016 i klasą zasobów służącą do reprezentowania tego zasobu w magazynie Metaverse.

**rola** : podmiot zabezpieczeń przypisany do organizacji używany do zarządzania prawami dostępu.

**rozszerzenie reguł** : rozszerzenie reguł jest biblioteką dołączaną dynamicznie (dll), która zawiera zdefiniowany zestaw reguł do zarządzania danymi. Rozszerzenia reguł można używać podczas synchronizacji, aby rozszerzać funkcjonalność. Na przykład można użyć rozszerzenia reguł, aby połączyć dane z dwóch atrybutów źródłowych i przekierować je do jednego atrybutu docelowego (na przykład `sn` i `givenName` do `displayName` ).

**historia uruchamiania** : zestaw statystyk, który pokazuje wyniki pojedynczego uruchomienia agenta zarządzania.

**profil przebiegu** : profil przebiegu reprezentuje zestaw kroków, które określają sposób uruchamiania agenta zarządzania i ustawień konfiguracji, które określają sposób działania agenta zarządzania. Agent zarządzania może mieć wiele profili uruchamiania, które są przechowywane z agentem zarządzania. Profil przebiegu składa się z co najmniej jednego kroku przebiegu profilu.
<br/>

## <a name="s"></a>S

**folder wyszukiwania** : zobacz wpis dla folderu wyszukiwania zatwierdzeń.

**zakres wyszukiwania** : określa właściwości dla konkretnego kontekstu wyszukiwania, który użytkownik może wykonać w portalu programu MIM 2016. Na przykład użytkownik może wybrać zakres wyszukiwania z listy rozwijanej dla "Wszyscy użytkownicy", "wszystkie listy dystrybucyjne" "Moje oczekujące zatwierdzenia", a wyniki wyszukiwania byłyby ograniczone do elementów spełniających te kryteria, a także do wszelkich warunków wyszukiwania określonych przez użytkownika.

**deskryptor zabezpieczeń** : struktura i powiązane dane zawierające informacje o zabezpieczeniach dla zabezpieczonego zasobu. Deskryptor zabezpieczeń identyfikuje właściciela zasobu i grupę podstawową. Może również zawierać listę DACL kontrolującą dostęp do zasobu oraz listę SACL, która kontroluje rejestrowanie prób uzyskania dostępu do zasobu.

**podmiot zabezpieczeń** : tożsamość służąca do zarządzania zabezpieczeniami, na przykład konto użytkownika, które może uwierzytelniać się w usłudze.

**token zabezpieczający** : element protokołu, który przetransferuje informacje o uwierzytelnianiu i autoryzacji na podstawie poświadczeń. W protokołach usług sieci Web token zabezpieczający jest reprezentowany jako element XML w nagłówku protokołu SOAP zdefiniowanym przez usługę WS-Security.

**Usługa tokenu zabezpieczającego** : usługa implementująca protokół WS-Trust do zarządzania relacjami zaufania między klientami i usługami na podstawie wymiany tokenów zabezpieczających.

**sekwencyjny przepływ pracy** : wszystkie przepływy pracy w programie MIM 2016 pochodzą z sekwencyjnego przepływu pracy programu Windows Workflow Foundation. Zawiera kilka przepływów pracy w kolejności sekwencyjnej.

**konto usługi** : konto systemu Windows przypisane do użycia przez usługę systemu Windows, a nie do logowania się do systemu komputerowego. Reprezentuje konto systemowe programu MIM.

**Zestaw** : nazwana kolekcja zasobów. Zazwyczaj zestawy są używane do organizowania zasobów na podstawie reguł. Członkostwo w zestawie jest ręcznie — zarządzane lub oparte na kryteriach. Oznacza to, że można ręcznie dodawać zasoby do zestawu i definiować kryteria, które automatycznie dodaje zasoby do zestawu na podstawie instrukcji filtru. Gdy zasób spełnia kryteria filtrowania, zostanie automatycznie dodany do powiązanego zestawu.

**Reguła zasad zarządzania przejściami (TMPR)** : reguła zasad zarządzania stosowana do zmian członkostwa w zestawie. Ustaw reguł tmpr Zastosuj przepływy pracy akcji, gdy obiekt przechodzi do lub z określonego zestawu w elemencie MPR.

   Istnieją dwa typy reguł tmpr:
   
- Przejście w: zasób stanie się członkiem zestawu przejść.
- Przechodzenie: zasób opuszcza zestaw przejść.
   
   >[!NOTE]
   >Po usunięciu zestawu przejść system traktuje usunięcie jako zdarzenie przejścia dla obiektów, których to dotyczy.
      
  Odpowiedź jest odpowiedzią na zastosowaną zmianę stanu. Po wywołaniu powiązanej reguły MPR warunek został już zastosowany. Oznacza to, że zasoby, których to dotyczy, zostały już przenoszone do lub z zestawu przejścia. W przypadku reguł tmpr cel odpowiedzi nie definiuje reakcji na żądaną operację, ale definiuje odpowiedź do zastosowanej operacji. Innymi słowy, w przypadku zestawu MPR opartego na przeniesieniu nie ma znaczenia, jak stan został osiągnięty. Istotnie są konsekwencje zmiany stanu.
   
  Podczas konfigurowania reguły MPR opartej na przejściu w programie MIM należy określić następujące trzy ustawienia:
   
- Zestaw przejść
- Typ przejścia
- Przepływy pracy zasad
   
  Przepływy pracy zasad to definicje procesów, które należy wywołać w odpowiedzi na zmianę stanu. Najczęstszym przypadkiem użycia dla reguł MPR opartych na stanie są przyznawanie lub Odwoływanie uprawnień, a także anulowania aprowizacji w zewnętrznych źródłach danych.
   
  Na poniższej ilustracji przedstawiono kompletną architekturę opartej na przejściu zestaw MPR:

  ![Ustawianie architektury TMPR](media/mim-2016-sp1-terms/e12154d35b48cec676f160930ff33662.gif)
  
  Ustawienia reguł MPR oparte na przejściu są aktywowane przez żądania. Gdy żądanie jest przetwarzane i zatwierdzane przez RMPR, usługa MIM decyduje również o tym, czy zatwierdzone żądanie skutkuje przechodzeniem stanu oraz czy istnieje oparta na przejściu stanowa MPR, która obsługuje zmiany stanu.
  
  Na poniższej ilustracji przedstawiono relacje między żądaniem a zestawem MPR opartym na przejściu:
  
  ![Relacja między żądaniem a zestawem TMPR](media/mim-2016-sp1-terms/426be3a3a5380720fa1eaa3e7df360df.gif)

**SID** : unikatowa wartość używana do identyfikowania konta użytkownika, konta grupy lub sesji logowania.

**SOAP** : protokół służący do wymiany informacji o strukturze między składnikami oprogramowania.

**Synchronizacja** : proces utrzymywania wybranych danych w wielu źródłach danych w umowie. Synchronizacja reprezentuje operacje na obiektach wyłącznie w programie MIM. Synchronizacja może być operacją dla całego zestawu danych, dla którego jest zdefiniowana w agencie zarządzania lub operacji Delta, zgodnie ze zmianami od ostatniej znanej operacji. Krok synchronizacji przebiegu profilu definiuje procesy synchronizacji ruchu przychodzącego i wychodzącego.

   Krok profilu przebiegu synchronizacji ma dwa podtypy:
   
- Synchronizacja zmian
- Pełna synchronizacja
   
  Podczas synchronizacji różnicowej program MIM przetwarza tylko zaimportowane obiekty, które są obiektami tymczasowymi oflagowanymi jako oczekujące na import. Ten krok profilu przebiegu jest przydatny do przetwarzania tylko tych obiektów, które mają oczekujące zmiany, ale nie zostały przetworzone podczas poprzedniego przebiegu synchronizacji.
   
  Synchronizacja różnicowa jest używana w dwóch wstępnie zdefiniowanych profilach uruchamiania i zachowuje się nieco inaczej w każdym z nich. Pierwszy profil przebiegu jest synchronizacją różnicową, gdzie nie są wykonywane żadne zadania importowania z żadnego połączonego źródła, ale wszystkie obiekty w obszarze łącznika są oceniane i wszystkie obiekty z oczekującymi zmianami są przetwarzane. Drugi profil przebiegu jest połączony z różnicą importu i synchronizacji różnicowej. Ten profil przebiegu importuje tylko te obiekty i atrybuty z połączonego źródła danych, których wartości uległy zmianie od czasu ostatniego uruchomienia agenta zarządzania. Reguły agenta zarządzania są następnie ponownie stosowane tylko do obiektów, które mają oczekujące zmiany z importu różnicowego. Obiekty bez oczekujących zmian z tego importu różnicowego nie są oceniane.
   
  Podczas pełnej synchronizacji program MIM oblicza i stosuje reguły synchronizacji do wszystkich obiektów przemieszczania w przestrzeni łącznika. Pełna synchronizacja powinna zostać zainicjowana, gdy zmiany zostały zastosowane do reguł danego środowiska. W zależności od liczby obiektów w obszarze łącznika może to być operacja czasu i intensywnie korzystających z zasobów, dlatego należy unikać często zmieniających się reguł synchronizacji w środowisku produkcyjnym.

**filtr synchronizacji** : filtr uniemożliwiający transfer zasobów w magazynie Metaverse do bazy danych programu MIM 2016.

**reguła synchronizacji** : reguła służąca do przepływu informacji o zasobach między serwerem programu MIM (włącznie z aparatem synchronizacji programu MIM) i połączonym systemem zewnętrznym.
<br/>

## <a name="t"></a>T

**zasady** dotyczące danych czasowych: element MPR przejścia, który jest powiązany z zestawem danych czasowych. Zasady są stosowane w czasie trwania, ponieważ obiekty są przenoszone do i z zestawu w oparciu o definicję zestawu danych czasowych.

**Zestaw** danych czasowych: typ obiektu zestawu, który jest oparty na datach względnych. Zestawy danych czasowych zapewniają mechanizm, który w pełni automatyzuje proces przejścia do lub z zestawu w zależności od czasu. Na przykład zestaw danych czasowych można zdefiniować dla wszystkich grup, które wygasają jeden tydzień od dzisiaj. System automatycznie szacuje obiekty w systemie i dodaje je do tego zestawu codziennie. Inne przykłady umożliwiają dynamiczne definicje odwołania do czasu, takie jak filtr oparty na "x dniach od dzisiaj".

**zdarzenie czasu** : zdarzenie przejścia, które następuje po upływie skonfigurowanego interwału czasu lub osiągnięto określony dzień i godzinę.

**limit czasu** : okres, w którym MIM 2016 czeka na odpowiedzi na zatwierdzenie do momentu eskalacji działania.

**zestaw przejść** : zestaw, który jest używany w definicji reguły zasad zarządzania przejściem. Zasady są stosowane do zmian w członkostwie w zestawie, które mogą być obiektami wchodzącymi lub opuszczanymi z zestawu, w zależności od konfiguracji TMPR.
<br/>

## <a name="u"></a>U

**odblokowana Grupa** : Grupa, w której można zmienić członkostwo grupy przez użytkowników innych niż właściciel grupy.

**Grupa uniwersalna** : Grupa z zakresem uniwersalnym jest grupą Active Directory, która może zawierać członków z określonego lasu. Do grupy uniwersalnej można przypisać uprawnienia w dowolnej domenie lub lesie. Listy dystrybucyjne mają zwykle zakres uniwersalny.Grupa zabezpieczeń z zakresem uniwersalnym może zabezpieczać zasoby w tym samym lesie.

**żądanie aktualizacji** : żądanie zmiany atrybutów zasobu.

**słowo kluczowe użycia** : słowo kluczowe Usage służy do określania, które zakresy wyszukiwania są wyświetlane dla określonej strony w interfejsie użytkownika portalu. Każda Strona widoku listy w interfejsie użytkownika określa zero lub więcej słów kluczowych użycia, a interfejs użytkownika dla tej strony zawiera wszystkie zakresy wyszukiwania, które zawierają pasujące słowa kluczowe. Podczas tworzenia zakresów wyszukiwania klienci mogą określić zero lub więcej słów kluczowych dla zakresu wyszukiwania, aby dostosować zakres wyszukiwania dla danej strony w interfejsie użytkownika. Jest on również używany do określenia zasobu strony głównej i zasobu paska nawigacyjnego, który jest widoczny dla zestawu użytkowników. Służy również do zarządzania schematami w celu ochrony i etykietowania elementów schematu, które są niezbędne przez różne składniki programu MIM.
<br/>

## <a name="w"></a>W

**Portal sieci Web** : interfejs użytkownika zaimplementowany przez aplikację oprogramowania za pośrednictwem składnika serwera sieci Web, takiego jak usługi IIS.

**Usługa sieci Web** : interfejs protokołu do usługi zaimplementowany przy użyciu protokołu opartego na protokole HTTP.

**przepływ pracy** : przepływ pracy to zbiór jednostek elementów o nazwie działania, które są przechowywane jako model, który opisuje proces rzeczywisty. Przepływy pracy umożliwiają opisywanie kolejności i zależności między elementami roboczymi. Ta praca przechodzi przez model od początku do końca, a działania mogą być wykonywane przez osoby lub funkcje systemowe. Innymi słowy, jeśli odpowiedź na żądanie wymaga przetwarzania złożonego, kroki są hermetyzowane w obiekcie przepływu pracy. Przepływy pracy są opcjonalnymi składnikami i ściśle powiązane z reguł MPR. Przepływy pracy definiują działanie lub działania, które muszą wystąpić podczas przetwarzania elementu MPR. Program MIM instaluje kilka domyślnych przepływów pracy, których można użyć jako lub jako podstawę dla niestandardowego przepływu pracy.

   Przykłady działań przepływu pracy:

- Wysyłanie zautomatyzowanej wiadomości e-mail w celu zażądania zatwierdzenia.
- Ograniczanie atrybutów, które użytkownik może zobaczyć podczas wyszukiwania niestandardowego.
- Sprawdzanie poprawności nowej grupy przed zaleceniami AD DS lub MIM.
- Dodawanie lub usuwanie obiektu z zakresu reguły synchronizacji.
   
  Aby rozwiązać wszystkie wymagania dotyczące przetwarzania w danym środowisku, architektura programu MIM definiuje trzy typy przepływów pracy:
   
- Uwierzytelnianie: wykonuje dodatkowe sprawdzanie poprawności tożsamości użytkownika przed kontynuowaniem żądania
- Autoryzacja: sprawdza poprawność żądania przez sekwencję działań, takich jak uzyskanie niezbędnych poza zatwierdzeniem przed przetworzeniem żądania.
- Akcja: przetwarza wszelkie dalsze działania po pomyślnym zakończeniu oryginalnego żądania.
   
  Te trzy przepływy pracy składają się na część modelu przetwarzania żądań.

**definicja przepływu pracy** : definicja przepływu pracy jest przechowywana w formacie xoml, zdefiniowanym przez Windows Workflow Foundation (WF). Definiuje to działania, parametry działań oraz kolejność, w jakiej powinny być uruchamiane.

**Projektant przepływu pracy** : środowisko projektowania dla konstruowania przepływów pracy.

**host przepływu pracy** : składnik serwera, który zajmuje się uruchamianiem przepływów pracy. W programie MIM 2016 usługa MIM 2016 jest hostem przepływu pracy.

**wystąpienie przepływu pracy** : uruchomione wystąpienie definicji przepływu pracy jako efekt żądania.

**Zarządzanie przepływami pracy** : funkcja programu MIM 2016, która zajmuje się projektowaniem przepływów pracy, ich wykonywaniem i zarządzaniem nimi. Zarządzanie przepływami pracy składa się z Projektant przepływu pracy, żądania zarządzania i hosta przepływu pracy.
