---
title: Podstawowa pakietu BHOLD instalacja | Dokumentacja firmy Microsoft
description: Dokument core instalacji pakietu BHOLD
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 4499c2846655ff75462794d684cae44f03134f1c
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333534"
---
# <a name="bhold-core-installation"></a>Instalacja pakietu BHOLD Core

Moduł BHOLD Core udostępnia kluczowe funkcje pakietu BHOLD w danym środowisku. Moduł BHOLD Core musi być zainstalowane i skonfigurowane na serwerze w sieci lokalnej, zanim będzie można zainstalować inne moduły pakietu BHOLD.

## <a name="bhold-core-installation-requirements"></a>Wymagania dotyczące instalacji Core pakietu BHOLD

Moduł BHOLD Core stanowi podstawę pakietu BHOLD firmy Microsoft. Przed zainstalowaniem innych modułów pakietu BHOLD firmy Microsoft, należy zainstalować moduł BHOLD Core.

### <a name="bhold-core-hardware-requirements"></a>Wymagania sprzętowe Core pakietu BHOLD

Moduł BHOLD Core stanowi podstawę pakietu BHOLD firmy Microsoft. Przed zainstalowaniem innych modułów pakietu BHOLD firmy Microsoft, należy zainstalować moduł BHOLD Core.

|          |        |          |
|----------|--------|----------|
|**Składnik** |**Minimum** | **Zalecane** |
|Procesor | 64-bitowy procesor | Wyspecjalizowanymi 64-bitowy procesor |
| Pamięć |3 GB | 6 GB lub więcej |
|Magazyn| 30 GB dostępnego miejsca |Zależy od rozmiaru wdrożenia |
|Karta sieciowa| Połączenie 100 MB/s z serwerem SQL i programu Forefront Identity Manager (FIM) | 1Gbps połączenia z serwerem SQL i programu FIM|

Zalecenia te zależą od implementacji typowych i nie biorą pod uwagę inne aplikacje uruchomione na serwerze. Może być konieczne użycie wyższej wydajności składników w zależności od konkretnego środowiska.

### <a name="bhold-core-software-requirements"></a>Wymagania dotyczące oprogramowania Core pakietu BHOLD

Moduł BHOLD Core musi być zainstalowany na komputerze, który spełnia następujące wymagania:

- Serwer musi być uruchomiony system Windows Server 2012 (64-bitowy), Windows Server 2016 
- Serwer musi należeć do domeny usługi Active Directory Domain Services (AD DS). W środowiskach testowych serwer może być kontrolerem domeny usług AD DS.
- Podstawowe BHOLD musi być zainstalowany użytkownik zalogowany przy użyciu konta w tej samej domenie co serwer i który należy do grupy Administratorzy domeny w domenie i do grupy administratorów na serwerze.
- Microsoft Internet Information Services (IIS) za pomocą platformy ASP.NET musi być zainstalowany na serwerze. Usługi IIS musi być skonfigurowany z uwierzytelnianiem Windows włączone. Jeśli podstawowe BHOLD jest instalowany w systemie Windows Server 2012/2016, musi być zainstalowany zgodność z narzędziami zarządzania usług IIS 6. Jeśli BHOLD Core jest instalowany w systemie Windows Server 2012, należy zainstalować narzędzia obsługi skryptów 6 w usługach IIS
- Należy zainstalować platformę .NET 3.5.
  - BHOLD Core wymagają .NET 3.5, dlatego nie można zainstalować w instalacji Server Core.
- Program Silverlight 4 jest wymagany przez inne moduły BHOLD, a więc zaleca się zainstalować przed zainstalowaniem pakietu BHOLD Core.
- Microsoft SQL Server 2014 lub Microsoft SQL Server 2016 można zainstalować pakietu BHOLD podstawowego serwera lub na innym serwerze w sieci LAN. 

Komputery klienckie Windows musi działać program Microsoft Internet Explorer w wersji 6 lub nowszej i wersji oprogramowania Microsoft Silverlight 4 lub nowszej.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Moduł BHOLD Core wymaga konta użytkownika, który jest używany do uwierzytelniania i autoryzowania usługi Core pakietu BHOLD na serwerze usługi i inne jednostki sieci. W tej sekcji wyjaśniono, jak utworzyć i skonfigurować konta użytkownika i zapewnia preinstalacji arkusza, który pomoże Ci zebrać informacje potrzebne do ukończenia Instalacja podstawowa pakietu BHOLD.

>{! WAŻNE} podczas instalacji pakietu BHOLD Core, możesz użyć istniejącej bazy danych programu SQL Server jako bazy danych BHOLD Core i umożliwić kreatora Instalacja podstawowa pakietu BHOLD w celu utworzenia bazy danych BHOLD Core dla Ciebie. Jeśli zdecydujesz się używać istniejącej bazy danych, upewnij się, czy inni użytkownicy nie mogą dostęp do bazy danych, podczas instalowania pakietu BHOLD Core. Przed zainstalowaniem pakietu BHOLD Core, sprawdź kontroli dostępu do bazy danych programu SQL Server nie zezwalaj na dostęp przez wskazanego użytkownika, który instaluje podstawowe BHOLD.

## <a name="required-user-and-group"></a>Wymagane użytkowników i grup

Moduł BHOLD Core musi mieć możliwość logowania się do domeny przy użyciu konta użytkownika, który jest przeznaczone do tego celu i który jest elementem członkowskim dwóch grup zabezpieczeń, w tym utworzony specjalnie dla modułu Core pakietu BHOLD. Członkostwo w grupie Administratorzy domeny jest wymagany do wykonania tej procedury.
**Aby utworzyć i skonfigurować podstawowe BHOLD użytkowników i grup zabezpieczeń**

1.  Na kontrolerze domeny kliknij **Start**, wskaż polecenie **narzędzia administracyjne**, a następnie kliknij przycisk **użytkownicy usługi Active Directory i komputery**.

2.  W drzewie konsoli rozwiń domenę, w których konto ma zostać utworzony, kliknij prawym przyciskiem myszy **użytkowników**, wskaż polecenie **New**, a następnie kliknij przycisk **grupy**.

3.  W **nowy obiekt — grupa** dialogowym **Nazwa grupy**, wpisz nazwę grupy (domyślny BHOLD: BHOLDApplicationGroup), a następnie kliknij przycisk **OK**.

4.  Kliknij prawym przyciskiem myszy **użytkowników**, wskaż polecenie **New**, a następnie kliknij przycisk **użytkownika**.

5.  W **imię i nazwisko**, wpisz nazwę, która pomoże zidentyfikować konta, na przykład BHOLD konta usługi podstawowej.

6.  W **nazwa logowania użytkownika**, wpisz nazwę użytkownika konta usługi podstawowej BHOLD (domyślny BHOLD: b1user), a następnie kliknij przycisk **dalej**.

7.  W **hasło** i **Potwierdź hasło**, wpisz hasło dla konta usługi.

8.  Wyczyść **użytkownik musi zmienić hasło przy następnym logowaniu**, wybierz opcję **użytkownik nie może zmienić hasła** i **hasło nigdy nie wygasa**, kliknij przycisk **dalej**, a następnie kliknij przycisk **Zakończ**.

9.  W okienku wyników konsoli kliknij prawym przyciskiem myszy konto użytkownika, a następnie kliknij przycisk **dodać do grupy**.

10. W **Wybieranie grup** okno dialogowe, wpisz nazwę wyświetlaną grupy, która została utworzona wcześniej, należy wpisać je średnikiem (;), a następnie wpisz IIS_IUSRS.

11. Kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.  

Poniższa procedura odbywa się na komputerze, na którym ma zostać zainstalowany moduł BHOLD Core. Użytkownik musi być zalogowany jako członek grupy Administratorzy domeny, aby wykonać tę procedurę.

## <a name="bhold-core-installation-worksheet"></a>Arkusz instalacji Core pakietu BHOLD

Przed rozpoczęciem Zainstaluj moduł BHOLD Core, musisz być przygotowana do informacje, które kreator Instalacja podstawowa pakietu BHOLD wymaga, aby ukończyć instalację. Następujący arkusz pomoże Zarejestruj te informacje będą gotowe do dostarczenia go, gdy jest to konieczne. Każda sekcja odnosi się do strony w Kreatorze instalacji pakietu BHOLD Core.

### <a name="account-settings"></a>Ustawienia konta

| **Element**                                    | **Opis**                                                                                                                                                                                                                                                                                             | **Wartość**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, czy zabezpieczeń Active Directory Domain Services będzie kontrolować dostęp do pakietu BHOLD Core.                                                                                                                                                                                                  | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                 |
| **Domeny**                                  | Określa domenę zawierającą serwer BHOLD, konta usługi i grupy aplikacji. **Ważne:** Podaj nazwę domeny przy użyciu nazwy NetBIOS (krótki), a nie w pełni kwalifikowana nazwa domeny (FQDN). Na przykład jeśli nazwę FQDN domeny fabrikam.com, należy określić nazwę domeny jako firmy CONTOSO. | Napisz nazwę domeny, w tym miejscu:                                                                                                                                        |
| **Grupy aplikacji**                       | Określa nazwę grupy zabezpieczeń, który został wcześniej utworzony [wymaganych użytkowników i grup](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Napisz nazwę grupy, w tym miejscu:                                                                                                                                         |
| **Użytkownik usługi**                            | Określa nazwę logowania konta użytkownika usługi, który został wcześniej utworzony [wymaganych użytkowników i grup](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Napisz tutaj nazwę konta użytkownika:                                                                                                                                  |
| **Hasło**                                | Określa hasło konta użytkownika BHOLD podstawowe usługi.                                                                                                                                                                                                                                              | Zapisz hasło w tym miejscu: **ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji.                                                                |
| **Port IP witryny sieci Web**                         | Określa adres IP i port numer witryny sieci Web ma zostać utworzony na serwerze intranetowym z. Zmiana wartości domyślnej (\*) tylko wtedy, gdy nie użyje tego samego adresu IP jako domyślna witryna sieci Web. Zmień numer portu do portu dostępne tylko wtedy, gdy jest to domyślny port (5151) jest już używana.             | Jeśli adres IP niestandardowy jest używany przez domyślną witrynę sieci Web, zapisz je w tym miejscu: Jeśli domyślny numer portu jest już używana, należy zapisać numer portu witryny sieci Web pakietu BHOLD tutaj: |

### <a name="database-settings"></a>Ustawienia bazy danych

| **Element**                                       | **Opis**                                                                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj zintegrowanych zabezpieczeń**                    | Określa, że uwierzytelnianie Windows jest używane do dostępu do bazy danych.                                                                                                                                                                                                     | Zaznacz pole wyboru, jeśli jest używane uwierzytelnianie Windows, aby połączyć się z serwerem SQL. Usuń zaznaczenie pola wyboru, jeśli jest używane uwierzytelnianie programu SQL Server. Baza danych muszą zostać utworzone przed systemem BHOLD podstawowych ustawień Jeśli uwierzytelnianie programu SQL Server jest używany. **Uwaga:** Jeśli używane jest uwierzytelnianie Windows, użytkownik musi być zalogowany przy użyciu konta które ma roli serwera sysadmin na serwerze bazy danych. |
| **Użytkownika bazy danych** i **hasła bazy danych** | Określa nazwę użytkownika i hasło użytkownika z roli serwera sysadmin na serwerze bazy danych. Wartości te są dostarczane tylko wtedy, gdy jest używane uwierzytelnianie programu SQL Server.                                                                                               | Zapis nazwę użytkownika w tym miejscu programu SQL Server: zapis w tym miejscu hasło użytkownika programu SQL Server: **Uwaga:** koniecznie Zapisz to hasło w lokalizacji ukryte, bezpieczne.                                                                                                                                                                                                                                                  |
| **Serwer bazy danych** i **Nazwa bazy danych**   | Określa nazwę NetBIOS serwera bazy danych i nazwę bazy danych (domyślny: b1), Instalacja podstawowa pakietu BHOLD zostanie utworzona. Jeśli nie używasz domyślnego wystąpienia serwera bazy danych, określ wystąpienie serwera bazy danych w postaci  *\<serwera\>*\\*\<wystąpienia\>* . | Zapis nazwy serwera (lub serwera i wystąpienia) w tym miejscu: zapisać nazwę w bazie danych:                                                                                                                                                                                                                                                                                                                   |
| **Wprowadzić ograniczenia dotyczące użytkownika bazy danych**    | Przestarzałe.                                                                                                                                                                                                                                                                 | Nie zmieniaj wartości domyślne                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Instalacja podstawowa pakietu BHOLD

Aby zainstalować moduł BHOLD Core, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł Core pakietu BHOLD na: 

- BholdCore  *\<wersji\>*\_Release.msi

Zastąp *\<wersji\>* numerem wersji, wersji pakietu BHOLD Core, którym ją instalujesz.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

### <a name="postinstallation-settings"></a>Ustawienia o

Po ukończeniu Instalacja podstawowa pakietu BHOLD należy skonfigurować zaporę Windows i zmienić ustawienia zaawansowane w puli aplikacji BHOLD Core w Internetowych usługach informacyjnych w celu ukończenia konfiguracji pakietu BHOLD Core. Jeśli to konieczne, możesz również zmienić BHOLD atrybutów systemowych, zgodnie z wymaganiami.

#### <a name="configuring-windows-firewall"></a>Konfigurowanie zapory Windows

Jeśli użytkownicy będą uzyskiwać dostęp do pakietu BHOLD za pomocą przeglądarki sieci web na komputerach zdalnych, należy skonfigurować zapory Windows na serwerze BHOLD Core, aby zezwalać na połączenia przychodzące do portu witryny sieci Web, który określiłeś, po zainstalowaniu pakietu BHOLD Core.

Aby wykonać tę procedurę, musi być członkiem grupy Administratorzy na komputerze lokalnym.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Aby zezwolić na połączenia przychodzące do witryny sieci Web pakietu BHOLD

1.  Kliknij przycisk **Start**, wskaż polecenie **narzędzia administracyjne**, kliknij prawym przyciskiem myszy **zapory Windows z ustawieniami zaawansowanymi**, a następnie kliknij przycisk **Uruchom jako administrator**.

2.  W okienku po lewej stronie kliknij **reguły ruchu przychodzącego**, a następnie w okienku po prawej stronie kliknij **nową regułę**.

3.  W Kreatorze nowej reguły ruchu przychodzącego kliknij **portu**, a następnie kliknij przycisk **dalej**.

4.  Upewnij się, że **TCP** wybrano w **określone porty lokalne**, wpisz numer portu domyślnego BHOLD Core (5151) lub numer portu, który określiłeś, gdy zainstalowana BHOLD Core, a następnie kliknij  **Następny**.

5.  Upewnij się, że **Zezwalaj na połączenie** jest zaznaczone, a następnie kliknij przycisk **dalej**.

6.  Na **profilu** strony, usuń zaznaczenie pól wyboru dla lokalizacji, z których użytkownik nie ma dostęp do witryny sieci Web pakietu BHOLD, a następnie kliknij **dalej**.

7.  Na **nazwa** stronie, wpisz nazwę reguły (na przykład zezwalania na połączenia przychodzące do pakietu BHOLD Core), a następnie kliknij **Zakończ**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Włączanie aplikacji 32-bitowych dla puli aplikacji Core pakietu BHOLD

Aby umożliwić usług IIS do poprawnego działania z modułem BHOLD podstawowe, należy skonfigurować pulę aplikacji Core pakietu BHOLD, aby umożliwić obsługę aplikacji 32-bitowych. Należy zalogować się przy użyciu konta używane do instalowania modułu Core pakietu BHOLD, aby wykonać tę procedurę.

**Aby włączyć obsługę aplikacji 32-bitowych dla puli aplikacji Core pakietu BHOLD**

1.  Aby otworzyć Menedżera internetowych usług informacyjnych, kliknij przycisk **Start**, wskaż **narzędzia administratora**, a następnie kliknij przycisk **Internet Information Services (IIS) Manager**.

2.  W drzewie konsoli rozwiń nazwę serwera, a następnie kliknij przycisk **pul aplikacji**.

3.  W **pul aplikacji** listy, kliknij prawym przyciskiem myszy **CoreAppPool**, a następnie kliknij przycisk **Zaawansowane ustawienia**.

4.  W **Zaawansowane ustawienia** dialogowym **Włącz 32-bitowych aplikacji** listy wybierz **True**, a następnie kliknij przycisk **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Ustanawianie główną nazwę usługi dla witryny sieci Web pakietu BHOLD

Jeśli nazwa sieci, który jest używany do kontaktowania się z witryny sieci Web pakietu BHOLD nie jest taka sama jak nazwa hosta serwera, należy ustanowić główną nazwę usługi (SPN) dla protokołu HTTP. Na przykład jeśli używasz rekordu zasobu CNAME w systemie DNS, aby określić alias dla serwera lub jeśli używasz równoważenia obciążenia sieciowego, należy zarejestrować tych adresów z dodatkową siecią w usłudze Active Directory. Jeśli nie to zrobić, program Internet Explorer nie można używać protokołu Kerberos podczas nawiązywania kontaktu z witryny sieci Web pakietu BHOLD.

> [!IMPORTANT]
> Jeśli moduł BHOLD Core jest zainstalowany na tym samym komputerze co portalu programu FIM, należy utworzyć rekordy zasobów DNS (CNAME i A) przy użyciu różnych nazw hostów dla serwerów z uruchomioną BHOLD Core i server uruchomionym portalem programu FIM. Tylko jedna nazwa SPN, można ustanowić dla konkretnej pary server alias, usługi typu w/i BHOLD Core i portalu programu FIM wymagają oddzielne nazwy SPN ponieważ zwykle są one uruchamiane w ramach różnych kont. Polecenia setspn zgłasza błąd, jeśli nazwa SPN została ustanowiona w ramach innego konta.

Członkostwo w grupie **Administratorzy domeny**, albo równoważnej, jest minimalnym wymaganiem do wykonania tej procedury.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Aby ustanowić główną nazwę usługi witryny sieci Web pakietu BHOLD

1.  Na kontrolerze domeny usług domenowych Active Directory, kliknij przycisk **Start**, kliknij przycisk **wszystkie programy**, kliknij przycisk **Akcesoria**, kliknij prawym przyciskiem myszy **wiersza polecenia** , a następnie kliknij przycisk **Uruchom jako administrator**.

2.  W wierszu polecenia wpisz następujące polecenie i naciśnij klawisz ENTER: setspn – S HTTP / *\<networkalias\> \<domeny\>* \\ *\<accountname\>* gdzie:

    -   *\<networkalias\>*  to adres, używanego przez klientów do kontaktowania się z witryny sieci Web pakietu BHOLD

    -   *\<domeny\>*\\*\<accountname\>*  jest nazwą domeny i użytkownika konta usługi Core pakietu BHOLD, który został utworzony podczas instalowania pakietu BHOLD Core.

3.  Powtórz poprzedni krok dla wszystkich innych nazw używanych przez klientów do kontaktowania się z witryny sieci Web pakietu BHOLD na przykład, CNAME aliasy, nazw, które zawierają w pełni kwalifikowanej nazwy domeny lub nazwy, które zawierają nazwę NetBIOS domeny (short).

#### <a name="setting-bhold-system-attributes"></a>Ustawianie atrybutów systemowych pakietu BHOLD

Aby sprawdzić, czy instalacji modułu BHOLD Core zakończyło się pomyślnie, Otwórz portal pakietu BHOLD podstawowych i wyświetlanie atrybutów systemowych. Ponadto upewnij się, że funkcje modułu BHOLD Core poprawnie w środowisku, można zmodyfikować następującymi atrybutami systemowymi pakietu BHOLD, zgodnie z potrzebami:

| **Atrybut**                | **Opis**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Wartość Y, jeśli BHOLD witryny sieci Web jest uruchomiona na usługi sieci web klastra, aby upewnić się, że ostatnio wyświetlane elementy działają poprawnie. Wartość N, jeśli działa witryny sieci Web pakietu BHOLD na autonomicznym serwerze usług IIS.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Wartość Y upewnij się, że jednostki organizacyjne (orgunits) mogą być przenoszone tylko do orgunits przy użyciu tego samego typu organizacji jako orgunit nadrzędnej. Na przykład zapobiega to orgunit projektu przenoszonych do orgunit działu. Należy ustawić N, aby zezwolić orgunit mają być umieszczone w orgunit innego typu. |
| **Uruchom dni między ABA**     | Ustaw na liczbę całkowitą dwucyfrowy Określ interwał (w dniach) między dwoma przebiegami autoryzacji opartych na atrybutach (ABA). Na przykład aby określić, czy działa ABA będą oddzielone dwa dni, wpisz 02.                                                                                                                     |
| **Godzina rozpoczęcia ABA uruchamiania**    | Ustaw na liczbę całkowitą dwucyfrowy Określa godzinę dnia, gdy nastąpi autoryzacji opartych na atrybutach, uruchom. Na przykład aby określić, że ABA Uruchom odbędzie się o 11:00 należy wpisać 23 (23:00).                                                                                                             |
| **Kardynalność systemu**       | Wartość N, jeśli nie chcesz, aby zaznaczyć Kardynalność system w pakietu BHOLD. Wartość domyślna wynosi: Y.                                                                                                                                                                                                                             |
| **Rejestrowanie**                  | Wartość N, jeśli nie chcesz, aby zmiany, które mają być rejestrowane. Wartość domyślna wynosi: Y.                                                                                                                                                                                                                                            |
| **Przetwarzanie SystemQueue**   | Wartość N, jeśli nie chcesz, aby system kolejki przetwarzania. Nie należy zmieniać tej wartości, chyba że kierowane do tego celu przez Centrum pomocy technicznej.                                                                                                                                                                                           |

Użytkownik musi być zalogowany jako członek grupy Administratorzy domeny, aby wykonać tę procedurę.

**Aby ustawić atrybut systemowy pakietu BHOLD**

1.  Kliknij przycisk **Start**, kliknij przycisk **wszystkie programy**, a następnie kliknij przycisk **programu Internet Explorer**.

2.  W polu Adres wpisz, gdzie *\<serwera\>* jest nazwą serwera witryny sieci Web pakietu BHOLD i *\<portu\>* jest powiązany numer portu witryny sieci Web.

3.  Kliknij przycisk **Home**, kliknij przycisk **wartości**, a następnie kliknij przycisk **Modyfikuj**.

4.  Znajdź nazwę atrybutu, który chcesz zmienić, wpisz nową wartość w polu obok nazwy atrybutu, a następnie kliknij **OK**.

## <a name="next-steps"></a>Kolejne kroki

Po zainstalowaniu pakietu BHOLD podstawowych i zweryfikować, czy instalacja zakończyła się pomyślnie, można zainstalować dodatkowe moduły. W tym momencie baza danych BHOLD jest zasadniczo pusta, zawierający tylko jedno konto użytkownika, konto główne i jedną jednostkę organizacyjną (orgunit) orgunit głównego. Aby dodać większą liczbę użytkowników w bazie danych BHOLD, można zainstalować modułu dostępu do łącznika zarządzania lub modułu generatora modeli pakietu BHOLD w zależności od potrzeb. Moduł dostępu łącznika zarządzania umożliwia importowanie danych użytkownika z usługi synchronizacji FIM lub generatora modeli pakietu BHOLD służy do importowania danych użytkownika z zestawu plików ze strukturą. Aby uzyskać więcej informacji na temat korzystania z łącznika zarządzania dostęp do modułu, zobacz [Przewodnik po laboratorium testowym: łącznika zarządzania dostęp do pakietu BHOLD](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Aby uzyskać więcej informacji na temat korzystania z modułu generatora modeli pakietu BHOLD zobacz:

- [Przewodnik pojęcia pakietu BHOLD firmy Microsoft](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
