---
title: Podstawowa BHOLD instalacja | Dokumentacja firmy Microsoft
description: Dokument core instalacji pakietu BHOLD
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 33fbe63528d5d7c543ae286f934654538782b4d5
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/14/2017
---
# <a name="bhold-core-installation"></a>Instalacja BHOLD Core

Moduł BHOLD Core zapewnia kluczowych funkcji usługi pakietu BHOLD w danym środowisku. Moduł BHOLD Core musi być zainstalowana i skonfigurowana na serwerze w sieci lokalnej, zanim będzie można zainstalować w innych modułach pakietu BHOLD.

## <a name="bhold-core-installation-requirements"></a>Wymagania dotyczące instalacji BHOLD Core

Moduł BHOLD Core stanowi podstawę pakietu BHOLD firmy Microsoft. Przed zainstalowaniem inne moduły pakietu BHOLD firmy Microsoft, należy zainstalować moduł BHOLD Core.

### <a name="bhold-core-hardware-requirements"></a>Wymagania sprzętowe BHOLD Core

Moduł BHOLD Core stanowi podstawę pakietu BHOLD firmy Microsoft. Przed zainstalowaniem inne moduły pakietu BHOLD firmy Microsoft, należy zainstalować moduł BHOLD Core.

|          |        |          |
|----------|--------|----------|
|**Składnik** |**Minimalna** | **Zalecane** |
|Procesor | 64-bitowy procesor | Wielordzeniowych 64-bitowy procesor |
| Pamięć |3 GB | 6 GB lub więcej |
|Magazyn| Dostępne 30 GB |Zależy od rozmiaru wdrożenia |
|Karta sieciowa| 100 MB/s połączenie z serwerem SQL i Forefront Identity Manager (FIM) | Połączenie serwera SQL i programu FIM 1 GB/s|

Te zalecenia są oparte na typowych wdrożeniach i nie uwzględnia innych aplikacji uruchomionych na serwerze. Być może należy użyć wyższej wydajności składników, w zależności od konkretnego środowiska.

### <a name="bhold-core-software-requirements"></a>Wymagania dotyczące oprogramowania BHOLD Core

Moduł BHOLD Core musi być zainstalowany na komputerze, który spełnia następujące wymagania:

- Serwer musi działać system Windows Server 2012 (64-bitowy), Windows Server 2016 
- Serwer musi należeć do domeny usług domenowych w usłudze Active Directory (AD DS). W środowisku testowym serwer może być nazwą kontrolera domeny usług AD DS.
- Podstawowe BHOLD mogą być instalowane przez użytkownika zalogowanego konta w tej samej domenie co serwer i który należy do grupy Administratorzy domeny w domenie i do grupy administratorów na serwerze.
- Microsoft Internet informacji Services (IIS) z platformy ASP.NET, należy skonfigurować na serwerze. Usługi IIS musi być skonfigurowany z włączone uwierzytelnianie systemu Windows. Jeśli podstawowe BHOLD jest instalowany w systemie Windows Server 2012/2016, musi być zainstalowana zgodność z narzędziami zarządzania usług IIS 6. Jeśli podstawowe BHOLD jest instalowany w systemie Windows Server 2012, muszą być zainstalowane narzędzia skryptów programu IIS 6
- Należy zainstalować platformę .NET 3.5.
  - Podstawowe BHOLD wymaga .NET 3.5, dlatego nie można zainstalować w instalacji Server Core.
- Program Silverlight 4 jest wymagany przez inne moduły BHOLD i dlatego zaleca się zainstalować przed zainstalowaniem BHOLD Core.
- Musi być zainstalowany program Microsoft SQL Server 2014 lub Microsoft SQL Server 2016, server BHOLD Core lub na innym serwerze w sieci LAN. 

Komputery klienckie systemu Windows musi działać program Microsoft Internet Explorer w wersji 6 lub nowszej i wersji oprogramowania Microsoft Silverlight 4 lub nowszego.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Moduł BHOLD Core wymaga konta użytkownika, który jest używany do uwierzytelniania i autoryzacji usługi podstawowej BHOLD do serwera oraz inne jednostki sieci. W tej sekcji opisano sposób tworzenia i konfigurowania tego konta użytkownika i zapewnia preinstalacji arkusza, dzięki którym można zebrać informacje potrzebne do ukończenia instalacji Core BHOLD.

>{! WAŻNE} podczas instalowania BHOLD Core, można użyć istniejącej bazy danych programu SQL Server co baza danych BHOLD Core lub umożliwiają Kreatora instalacji Core BHOLD do utworzenia bazy danych BHOLD Core. Jeśli chcesz użyć istniejącej bazy danych, należy się upewnić, inni użytkownicy nie mogą dostęp do bazy danych podczas instalowania BHOLD Core. Przed zainstalowaniem BHOLD Core, sprawdź, czy kontroli dostępu do bazy danych programu SQL Server nie zezwalają na dostęp przez nikt inny oprócz użytkownika, który instaluje podstawowe BHOLD.

## <a name="required-user-and-group"></a>Wymagane użytkowników i grup

Moduł BHOLD Core musi mieć możliwość logowania się do domeny za pomocą konta użytkownika, który jest przeznaczony do tego celu i który jest elementem członkowskim dwóch grup zabezpieczeń, w tym utworzony specjalnie z myślą o module głównym BHOLD. Do wykonania tej procedury jest wymagane członkostwo w grupie Administratorzy domeny.
**Aby utworzyć i skonfigurować BHOLD Core użytkowników i grup zabezpieczeń**

1.  Na kontrolerze domeny, kliknij przycisk **Start**, wskaż polecenie **narzędzia administracyjne**, a następnie kliknij przycisk **użytkownicy usługi Active Directory i komputery**.

2.  W drzewie konsoli rozwiń węzeł domeny, w którym konto ma być tworzony, kliknij prawym przyciskiem myszy **użytkowników**, wskaż **nowy**, a następnie kliknij przycisk **grupy**.

3.  W **nowy obiekt — grupa** okna dialogowego, **Nazwa grupy**, wpisz nazwę grupy (domyślny BHOLD: BHOLDApplicationGroup), a następnie kliknij przycisk **OK**.

4.  Kliknij prawym przyciskiem myszy **użytkowników**, wskaż polecenie **nowy**, a następnie kliknij przycisk **użytkownika**.

5.  W **imię i nazwisko**, wpisz nazwę, która pomoże zidentyfikować konta, na przykład BHOLD konta usługi Core.

6.  W **nazwa logowania użytkownika**, wpisz nazwę użytkownika konta usługi BHOLD Core (domyślna BHOLD: b1user), a następnie kliknij przycisk **dalej**.

7.  W **hasło** i **Potwierdź hasło**, wpisz hasło dla konta usługi.

8.  Wyczyść **użytkownik musi zmienić hasło przy następnym logowaniu**, wybierz pozycję **użytkownik nie może zmienić hasła** i **hasło nigdy nie wygasa**, kliknij przycisk **dalej**, a następnie kliknij przycisk **Zakończ**.

9.  W okienku wyników konsoli kliknij prawym przyciskiem myszy konto użytkownika, a następnie kliknij przycisk **dodać do grupy**.

10. W **Wybieranie grup** okno dialogowe, wpisz nazwę wyświetlaną grupy, który został utworzony wcześniej, wpisz średnikiem (;), a następnie wpisz IIS_IUSRS.

11. Kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.  

Na komputerze, na którym ma zostać zainstalowany moduł BHOLD Core należy wykonać poniższą procedurę. Jako członek grupy Administratorzy domeny, aby wykonać tę procedurę, należy być zalogowanym.

## <a name="bhold-core-installation-worksheet"></a>Arkusz instalacji BHOLD Core

Przed rozpoczęciem zainstalować moduł BHOLD Core, musisz być przygotowana do dostarczania informacji, Kreator instalacji Core BHOLD wymagany do ukończenia instalacji. Następujący arkusz pomoże Ci Zarejestruj te informacje, więc wszystko będzie gotowe do dostarczenia go w razie potrzeby. Każda sekcja odnosi się do strony w Kreatorze instalacji BHOLD Core.

### <a name="account-settings"></a>Ustawienia konta

| **Element**                                    | **Opis**                                                                                                                                                                                                                                                                                             | **Wartość**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, że zabezpieczenia usług domenowych w usłudze Active Directory będzie kontrolował dostęp do podstawowych BHOLD.                                                                                                                                                                                                  | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                 |
| **Domeny**                                  | Określa domenę zawierającą serwer BHOLD, konta usługi i grupy aplikacji. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (short), a nie w pełni kwalifikowaną nazwę (FQDN). Na przykład jeśli nazwa FQDN domeny, to fabrikam.com, należy określić nazwę domeny jako CONTOSO. | Wpisz nazwę domeny w tym miejscu:                                                                                                                                        |
| **Grupy aplikacji**                       | Określa nazwę grupy zabezpieczeń utworzonej w [wymaganych użytkowników i grup](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Wpisz nazwę grupy w tym miejscu:                                                                                                                                         |
| **Użytkownik usługi**                            | Nazwa logowania konta użytkownika usługi utworzonego wcześniej w [wymaganych użytkowników i grup](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Napisz tutaj nazwę konta użytkownika:                                                                                                                                  |
| **Hasło**                                | Określa hasło konta użytkownika usługi BHOLD Core.                                                                                                                                                                                                                                              | Zapisać hasło tutaj: **ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji.                                                                |
| **Port IP witryny sieci Web**                         | Określa adres IP i port numer witryny sieci Web ma zostać utworzony na serwerze intranetowym. Zmień wartość domyślną (\*) tylko wtedy, gdy nie będzie używać tego samego adresu IP, jako domyślnej witryny sieci Web. Zmiany numeru portu dostępnego portu tylko wtedy, gdy domyślny port (5151) jest już używana.             | Jeśli adres IP niestandardowy jest używany przez domyślną witrynę sieci Web, zapisać go w tym miejscu: Jeśli domyślny numer portu jest już w użyciu, Zapisz numer portu witryny sieci Web BHOLD tutaj: |

### <a name="database-settings"></a>Ustawienia bazy danych

| **Element**                                       | **Opis**                                                                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj zintegrowanych zabezpieczeń**                    | Określa, że dostęp do bazy danych jest używane uwierzytelnianie systemu Windows.                                                                                                                                                                                                     | Zaznacz pole wyboru, jeśli uwierzytelnianie systemu Windows są używane do łączenia się z serwerem SQL. Wyczyść pole wyboru, jeśli jest używane uwierzytelnianie programu SQL Server. Bazy danych muszą być utworzone przed systemem BHOLD podstawowe ustawienia jeśli uwierzytelniania programu SQL Server jest używany. **Uwaga:** Jeśli używane jest uwierzytelnianie systemu Windows, użytkownik musi być zalogowany przy użyciu konta, które ma roli serwera sysadmin na serwerze bazy danych. |
| **Baza danych użytkownika** i **hasła bazy danych** | Określa nazwę użytkownika i hasło użytkownika z roli serwera sysadmin na serwerze bazy danych. Te wartości są określane tylko wtedy, gdy jest używane uwierzytelnianie programu SQL Server.                                                                                               | Zapisać nazwę użytkownika w tym miejscu programu SQL Server: zapis w tym miejscu hasło użytkownika programu SQL Server: **Uwaga:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                                                  |
| **Serwer bazy danych** i **Nazwa bazy danych**   | Określa nazwę NetBIOS serwera bazy danych i nazwa bazy danych (domyślne: b1) tworzące BHOLD podstawowych ustawień. Jeśli nie używasz domyślnego wystąpienia serwera bazy danych, określ wystąpienie serwera bazy danych w postaci * \<serwera\>*\\*\<wystąpienia\> *. | Nazwa serwera (lub serwera i wystąpienia) w tym miejscu zapisu: wpisz nazwę bazy danych w tym miejscu:                                                                                                                                                                                                                                                                                                                   |
| **Wprowadzić ograniczenia dotyczące użytkownika bazy danych**    | Przestarzałe.                                                                                                                                                                                                                                                                 | Nie należy zmieniać wartości domyślnej                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Instalacja podstawowa BHOLD

Aby zainstalować moduł BHOLD Core, zaloguj się jako członek grupy Administratorzy domeny, Pobierz następującego pliku i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł BHOLD Core na: 

- BholdCore * \<wersji\>*\_Release.msi

Zastąp * \<wersji\> * z numerem wersji instalowanej wersji BHOLD Core.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

### <a name="postinstallation-settings"></a>Ustawienia o

Po zakończeniu instalacji Core BHOLD należy skonfigurować Zaporę systemu Windows i zmienić ustawienia zaawansowane w puli aplikacji BHOLD Core w Internetowych usługach informacyjnych w celu ukończenia konfiguracji BHOLD Core. Jeśli to konieczne, należy również zmienić atrybutów systemowych BHOLD zgodnie z wymaganiami.

#### <a name="configuring-windows-firewall"></a>Konfigurowanie Zapory systemu Windows

Jeśli użytkownicy będą uzyskiwać dostęp do BHOLD za pomocą przeglądarki sieci web na komputerach zdalnych, należy skonfigurować zapory systemu Windows na serwerze BHOLD Core, aby zezwalać na połączenia przychodzące do portu witryny sieci Web określone podczas instalacji BHOLD Core.

Aby wykonać tę procedurę, musi być członkiem grupy Administratorzy na komputerze lokalnym.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Aby zezwolić na połączenia przychodzące do witryny sieci Web BHOLD

1.  Kliknij przycisk **Start**, wskaż polecenie **narzędzia administracyjne**, kliknij prawym przyciskiem myszy **zapory systemu Windows z ustawieniami zaawansowanymi**, a następnie kliknij przycisk **Uruchom jako administrator**.

2.  W okienku po lewej stronie kliknij **reguły ruchu przychodzącego**, a następnie w okienku po prawej stronie kliknij **nową regułę**.

3.  W Kreatorze nowej reguły ruchu przychodzącego kliknij **portu**, a następnie kliknij przycisk **dalej**.

4.  Upewnij się, że **TCP** wybrano w **określone porty lokalne**, wpisz numer portu domyślnego BHOLD Core (5151) lub numer portu, który określony podczas zainstalowany BHOLD Core, a następnie kliknij przycisk ** Następny**.

5.  Upewnij się, że **zezwalały na połączenie** jest zaznaczone, a następnie kliknij przycisk **dalej**.

6.  Na **profilu** Usuń zaznaczenie pól wyboru dla lokalizacji, z których użytkownik nie ma dostęp do witryny sieci Web BHOLD, a następnie kliknij pozycję **dalej**.

7.  Na **nazwa** , wpisz nazwę reguły (na przykład zezwalania na połączenia przychodzące na rdzeń BHOLD), a następnie kliknij przycisk **Zakończ**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Włączanie aplikacji 32-bitowego dla puli aplikacji BHOLD Core

Umożliwia usług IIS do poprawnego działania z modułem BHOLD Core należy skonfigurować BHOLD Core puli aplikacji do obsługi aplikacji 32-bitowych. Trzeba zalogować się przy użyciu konta używane do instalowania modułu BHOLD Core, aby wykonać tę procedurę.

**Aby włączyć obsługę 32-bitowej aplikacji dla puli aplikacji BHOLD Core**

1.  Aby otworzyć Menedżera internetowych usług informacyjnych, kliknij przycisk **Start**, wskaż polecenie **narzędzia administratora**, a następnie kliknij przycisk **Menedżera usług Internet Information Services (IIS)**.

2.  W drzewie konsoli rozwiń nazwę serwera, a następnie kliknij przycisk **pul aplikacji**.

3.  W **pul aplikacji** listy, kliknij prawym przyciskiem myszy **CoreAppPool**, a następnie kliknij przycisk **Zaawansowane ustawienia**.

4.  W **Zaawansowane ustawienia** okna dialogowego, **włączyć 32-bitowych aplikacji** listy, wybierz **True**, a następnie kliknij przycisk **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Ustanawianie główną nazwę usługi dla witryny sieci Web BHOLD

Jeśli nazwa sieci, który jest używany do kontaktowania się z BHOLD witryny sieci Web nie jest taka sama jak nazwa hosta serwera, musisz ustanowić główną nazwę usługi (SPN) dla protokołu HTTP. Na przykład jeśli umożliwia rekord zasobu CNAME w systemie DNS, określają alias serwera lub jeśli używasz równoważenia obciążenia sieciowego, zarejestruj te adresy dodatkowe sieci w usłudze Active Directory. Jeśli nie to zrobić, program Internet Explorer nie można użyć protokołu Kerberos podczas kontaktowania się z BHOLD witryny sieci Web.

>[!IMPORTANT]
Jeśli moduł BHOLD Core jest zainstalowany na tym samym komputerze co portalu programu FIM, należy utworzyć rekordy zasobów DNS (CNAME i A) z różne nazwy hostów dla serwerów z uruchomioną BHOLD Core i serwerze z uruchomioną portalu programu FIM. Można ustalić tylko jedną nazwę SPN dla określonego serwera alias, usługa typu w/pary, a BHOLD Core i portalu programu FIM wymagają oddzielnego SPN ponieważ są one zazwyczaj uruchamiane w ramach różnych kont. Polecenia setspn zgłasza błąd, jeśli nazwa SPN została ustanowiona w ramach innego konta.

Członkostwo w grupie **Administratorzy domeny**, albo równoważnej, jest minimalnym wymaganiem do wykonania tej procedury.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Aby ustanowić SPN BHOLD witryny sieci Web

1.  Na kontrolerze domeny usług domenowych w usłudze Active Directory, kliknij przycisk **Start**, kliknij przycisk **wszystkie programy**, kliknij przycisk **Akcesoria**, kliknij prawym przyciskiem myszy **wiersza polecenia **, a następnie kliknij przycisk **Uruchom jako administrator**.

2.  W wierszu polecenia wpisz następujące polecenie i naciśnij klawisz ENTER: setspn – S HTTP / * \<networkalias\> \<domeny\> * \\ * \<accountname\> * gdzie:

    -   *\<networkalias\> * adres używanego przez klientów do kontaktowania się z witryny sieci Web BHOLD

    -   *\<domeny\>*\\*\<accountname\> * jest domena i nazwa użytkownika konta usługi BHOLD Core utworzony podczas instalacji BHOLD Core.

3.  Powtórz poprzedni krok dla wszystkich innych nazw używanych przez klientów do kontaktowania się z BHOLD witryny sieci Web, na przykład, CNAME aliasy, nazw, które zawierają w pełni kwalifikowaną nazwą domeny lub nazwy, które zawierają nazwę NetBIOS domeny (short).

#### <a name="setting-bhold-system-attributes"></a>Ustawianie atrybutów systemowych BHOLD

Aby sprawdzić, czy instalacja modułu BHOLD Core zakończyła się pomyślnie, Otwórz portal BHOLD Core i wyświetlania atrybutów systemowych. Ponadto upewnij się, że BHOLD podstawowe funkcje modułu prawidłowo w danym środowisku, można zmodyfikować następujące atrybuty systemu BHOLD, zależnie od potrzeb:

| **Atrybut**                | **Opis**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Wartość Y, jeśli BHOLD witryny sieci Web jest uruchomiona na usługi sieci web klastra, aby sprawdzić, czy ostatnio wyświetlanych elementów działają prawidłowo. Wartość N, jeśli BHOLD witryny sieci Web jest uruchomiony na autonomicznym serwerze IIS.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Wartość Y upewnij się, że jednostki organizacyjne (orgunits) można przenieść tylko do orgunits tego samego typu organizacyjnego jako orgunit nadrzędnego. Na przykład zapobiega to orgunit projektu przenoszony do orgunit działu. Należy ustawić N, aby zezwolić orgunit do umieszczenia w orgunit innego typu. |
| **Uruchom dni między ABA**     | Wartość całkowita dwucyfrowe Określ interwał (w dniach) między dwa przebiegi autoryzacji na podstawie atrybutów (ABA). Na przykład aby określić, że działa ABA będą oddzielone dwa dni, wpisz 02.                                                                                                                     |
| **Godzina rozpoczęcia ABA Uruchom**    | Wartość całkowita dwucyfrowe określ godzinę dnia, kiedy nastąpi autoryzacji na podstawie atrybutów, uruchom. Na przykład aby określić, że ABA Uruchom będzie miała miejsce pod 11:00 w dniu należy wpisać 23 (23:00).                                                                                                             |
| **Kardynalność systemu**       | Wartość N, jeśli nie chcesz, aby system Kardynalność zaewidencjonowania BHOLD. Wartość domyślna to Y.                                                                                                                                                                                                                             |
| **Rejestrowanie**                  | Wartość N, jeśli nie chcesz, aby zmiany, które mają być rejestrowane. Wartość domyślna to Y.                                                                                                                                                                                                                                            |
| **Przetwarzanie SystemQueue**   | Wartość N, jeśli nie chcesz, aby system kolejki przetwarzania. Nie należy zmieniać tej wartości, chyba że kierowane do tego celu przez Centrum pomocy technicznej.                                                                                                                                                                                           |

Jako członek grupy Administratorzy domeny, aby wykonać tę procedurę, należy być zalogowanym.

**Aby ustawić atrybut systemowy BHOLD**

1.  Kliknij przycisk **Start**, kliknij przycisk **wszystkie programy**, a następnie kliknij przycisk **programu Internet Explorer**.

2.  W polu adresu wpisz, gdzie * \<serwera\> * jest nazwą serwera, witryny sieci Web BHOLD i * \<portu\> * jest powiązany numer portu witryny sieci Web.

3.  Kliknij przycisk **Home**, kliknij przycisk **wartości**, a następnie kliknij przycisk **Modyfikuj**.

4.  Znajdź nazwę chcesz zmienić, wpisz nową wartość w polu obok nazwy atrybutu, a następnie kliknij atrybut **OK**.

## <a name="next-steps"></a>Następne kroki

Po zainstalowaniu BHOLD Core i zweryfikować, czy instalacja zakończyła się pomyślnie, można zainstalować dodatkowe moduły. W tym momencie baza danych BHOLD jest zasadniczo pusta, zawierający tylko jedno konto użytkownika, konta głównego i jednej jednostki organizacyjnej (orgunit) orgunit głównego. Aby dodać więcej użytkowników do baza danych BHOLD, można zainstalować moduł łącznika zarządzania dostępu lub moduł BHOLD Generator modeli, w zależności od potrzeb. Można użyć modułu dostępu do zarządzania łącznika do importowania danych użytkownika z usługi synchronizacji programu FIM lub Generator modeli BHOLD służy do importowania danych użytkownika z grupy plików strukturalnych. Aby uzyskać więcej informacji o korzystaniu z modułu dostępu do zarządzania łącznika, zobacz [Przewodnik po laboratorium testowym: BHOLD dostępu administracyjnego łącznika](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx).

Aby uzyskać więcej informacji o korzystaniu z modułu BHOLD Generator modeli zobacz:

- [Przewodnik koncepcje pakietu Microsoft BHOLD](https://technet.microsoft.com/en-us/library/jj134102(v=ws.10).aspx)
- [TechnicalReference pakiet Microsoft BHOLD](https://technet.microsoft.com/en-us/library/jj134935(v=ws.10).aspx).
