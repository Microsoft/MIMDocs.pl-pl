---
title: Instalacja pakietu BHOLD Core | Microsoft Docs
description: Podstawowy dokument instalacji pakietu pakietu BHOLD Suite
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4b18d3caa866767524c56ce184e787a190e9390
ms.sourcegitcommit: 8ba50298cef65e8cc90402282e88410fad86b4d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2019
ms.locfileid: "75187314"
---
# <a name="bhold-core-installation"></a>Instalacja pakietu BHOLD Core

Moduł pakietu BHOLD Core udostępnia najważniejsze funkcje pakietu pakietu BHOLD Suite w danym środowisku. Przed zainstalowaniem innych modułów pakietu BHOLD Suite należy zainstalować i skonfigurować moduł pakietu BHOLD Core na serwerze w sieci lokalnej.

## <a name="bhold-core-installation-requirements"></a>Wymagania dotyczące instalacji pakietu BHOLD Core

Moduł pakietu BHOLD Core stanowi podstawę pakietu Microsoft pakietu BHOLD Suite. Przed zainstalowaniem innych modułów Microsoft pakietu BHOLD Suite należy zainstalować moduł pakietu BHOLD Core.

### <a name="bhold-core-hardware-requirements"></a>PAKIETU BHOLD podstawowe wymagania sprzętowe

Moduł pakietu BHOLD Core stanowi podstawę pakietu Microsoft pakietu BHOLD Suite. Przed zainstalowaniem innych modułów Microsoft pakietu BHOLD Suite należy zainstalować moduł pakietu BHOLD Core.

|          |        |          |
|----------|--------|----------|
|**Składnik** |**Minimum** | **Zalecane** |
|Procesor | 64-bitowy procesor | Procesor wielordzeniowy 64-bitowy |
| Pamięć |3 GB | 6 GB lub więcej |
|Magazyn| dostępne 30 GB |Zależy od rozmiaru wdrożenia |
|Karta sieciowa| połączenie 100 MB z serwerem SQL i programem Forefront Identity Manager (FIM) | 1Gbps połączenie z serwerem SQL i programem FIM|

Zalecenia te są oparte na typowych implementacjach i nie należy uwzględniać innych aplikacji uruchomionych na serwerze. Może być konieczne użycie składników o wyższej wydajności w zależności od konkretnego środowiska.

### <a name="bhold-core-software-requirements"></a>PAKIETU BHOLD podstawowe wymagania dotyczące oprogramowania

Moduł pakietu BHOLD Core musi być zainstalowany na komputerze, który spełnia następujące wymagania:

- Na serwerze musi być uruchomiony system Windows Server 2012 (64-bitowy), system Windows Server 2016 
- Serwer musi być członkiem domeny Active Directory Domain Services (AD DS). W środowiskach testowych serwer może być AD DS kontrolerem domeny.
- Rdzeń pakietu BHOLD musi być zainstalowany przez użytkownika zalogowanego przy użyciu konta w tej samej domenie co serwer, który należy do grupy Administratorzy domeny w domenie oraz do grupy administratorów na serwerze.
- Na serwerze musi być zainstalowany program Microsoft Internet Information Services (IIS) z ASP.NET. Usługi IIS muszą być skonfigurowane z włączonym uwierzytelnianiem systemu Windows. Jeśli pakietu BHOLD rdzeń jest instalowany w systemie Windows Server 2012/2016, należy zainstalować zgodność z usługami IIS w wersji 6. Jeśli pakietu BHOLD rdzeń jest instalowany w systemie Windows Server 2012, muszą być zainstalowane narzędzia do obsługi skryptów usług IIS 6.
- Należy zainstalować platformę .NET 3,5.
  - Ponieważ pakietu BHOLD Core wymaga programu .NET 3,5, nie można go zainstalować w trybie Server Core.
- Program Silverlight 4 jest wymagany przez inne moduły pakietu BHOLD i dlatego zaleca się jego zainstalowanie przed zainstalowaniem pakietu BHOLD Core.
- Microsoft SQL Server 2014 lub Microsoft SQL Server 2016 musi być zainstalowany na serwerze pakietu BHOLD Core lub na innym serwerze w sieci LAN. 

Komputery klienckie z systemem Windows muszą mieć uruchomiony program Microsoft Internet Explorer w wersji 6 lub nowszej oraz program Microsoft Silverlight w wersji 4 lub nowszej.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Moduł pakietu BHOLD Core wymaga konta użytkownika, które jest używane do uwierzytelniania i autoryzowania usługi pakietu BHOLD Core na serwerze i innych jednostkach sieciowych. W tej sekcji wyjaśniono, jak utworzyć i skonfigurować konto użytkownika i dostarcza arkusz preinstalacyjny, który pomoże zebrać informacje potrzebne do ukończenia instalacji pakietu BHOLD Core.

>{! WAŻNE} podczas instalowania pakietu BHOLD Core można użyć istniejącej bazy danych SQL Server jako podstawowej bazy danych pakietu BHOLD lub zezwolić na utworzenie podstawowej bazy danych pakietu BHOLD przez Kreatora instalacji pakietu BHOLD. Jeśli zdecydujesz się używać istniejącej bazy danych, musisz się upewnić, że żaden inny użytkownik nie będzie mógł uzyskać dostępu do bazy danych podczas instalowania pakietu BHOLD rdzeń. Przed zainstalowaniem pakietu BHOLD Core należy sprawdzić, czy kontrola dostępu bazy danych SQL Server nie zezwala na dostęp przez żadną osobę inną niż użytkownik instalujący pakietu BHOLD Core.

## <a name="required-user-and-group"></a>Wymagany użytkownik i Grupa

Moduł pakietu BHOLD Core musi być w stanie zalogować się do domeny przy użyciu konta użytkownika, które jest przeznaczone do tego celu, które jest członkiem dwóch określonych grup zabezpieczeń, w tym tych, które są tworzone specjalnie dla modułu pakietu BHOLD Core. Aby wykonać tę procedurę, członkostwo w grupie Administratorzy domeny jest wymagane.
**Aby utworzyć i skonfigurować użytkownika i grupę zabezpieczeń pakietu BHOLD Core**

1.  Na kontrolerze domeny kliknij przycisk **Start**, wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij pozycję **Active Directory Użytkownicy i komputery**.

2.  W drzewie konsoli rozwiń domenę, w której ma zostać utworzone konto, kliknij prawym przyciskiem myszy pozycję **Użytkownicy**, wskaż polecenie **Nowy**, a następnie kliknij pozycję **Grupa**.

3.  W oknie dialogowym **nowy obiekt — Grupa** w polu **Nazwa grupy**wpisz nazwę grupy (pakietu BHOLD default: BHOLDApplicationGroup), a następnie kliknij przycisk **OK**.

4.  Kliknij prawym przyciskiem myszy pozycję **Użytkownicy**, wskaż polecenie **Nowy**, a następnie kliknij pozycję **użytkownik**.

5.  W polu Imię i **nazwisko**wpisz nazwę, która pomoże zidentyfikować konto, na przykład konto usługi pakietu BHOLD Core.

6.  W polu **Nazwa logowania użytkownika**wpisz nazwę użytkownika pakietu BHOLD podstawowe konto usługi (pakietu BHOLD default: b1user), a następnie kliknij przycisk **dalej**.

7.  W polu **hasło** i **Potwierdź hasło**wpisz hasło dla konta usługi.

8.  Wyczyść pole wyboru **użytkownik musi zmienić hasło przy następnym logowaniu**, wybierz opcję **użytkownik nie może zmienić hasła** i **hasła nigdy nie wygasa**, kliknij przycisk **dalej**, a następnie kliknij przycisk **Zakończ**.

9.  W okienku wyników konsoli kliknij prawym przyciskiem myszy konto użytkownika, a następnie kliknij przycisk **Dodaj do grupy**.

10. W oknie dialogowym **Wybieranie grup** wpisz nazwę wyświetlaną utworzonej wcześniej grupy, wpisz średnik (;), a następnie wpisz IIS_IUSRS.

11. Kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.  

Na komputerze, na którym ma zostać zainstalowany moduł pakietu BHOLD Core, należy wykonać poniższą procedurę. Aby wykonać tę procedurę, użytkownik musi być zalogowany jako członek grupy Administratorzy domeny.

## <a name="bhold-core-installation-worksheet"></a>Arkusz instalacji pakietu BHOLD Core

Przed rozpoczęciem instalacji modułu pakietu BHOLD Core należy przygotować się do podania informacji wymaganych przez Kreatora instalacji podstawowej pakietu BHOLD do ukończenia instalacji. Poniższy arkusz pomoże Ci zarejestrować te informacje, dzięki czemu będzie można je w razie potrzeby udostępnić. Każda sekcja odnosi się do strony w Kreatorze instalacji podstawowej pakietu BHOLD.

### <a name="account-settings"></a>Ustawienia konta

| **Element**                                    | **Opis**                                                                                                                                                                                                                                                                                             | **Wartość**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Używanie dostawcy zabezpieczeń na komputerze/domenie** | Po wybraniu określa, że zabezpieczenia Active Directory Domain Services będą kontrolować dostęp do pakietu BHOLD rdzeń.                                                                                                                                                                                                  | zaznacz pole wyboru. **Ważne:** Instalacja nie powiedzie się, jeśli to pole wyboru nie jest zaznaczone.                                                                 |
| **Domeny**                                  | Określa domenę zawierającą serwer pakietu BHOLD, konto usługi i grupę aplikacji. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (krótkiej), a nie w pełni kwalifikowanej nazwy domeny (FQDN). Na przykład, jeśli nazwa FQDN domeny to fabrikam.com, określ nazwę domeny jako CONTOSO. | W tym miejscu wpisz nazwę domeny:                                                                                                                                        |
| **Grupa aplikacji**                       | Określa nazwę grupy zabezpieczeń, która została wcześniej utworzona w [wymaganym użytkowniku i grupie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Napisz tutaj nazwę grupy:                                                                                                                                         |
| **Użytkownik usługi**                            | Określa nazwę logowania konta użytkownika usługi utworzonego wcześniej w [wymaganym użytkowniku i grupie](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | W tym miejscu wpisz nazwę konta użytkownika:                                                                                                                                  |
| **Hasło**                                | Określa hasło konta użytkownika usługi pakietu BHOLD Core.                                                                                                                                                                                                                                              | Napisz hasło tutaj: **Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji.                                                                |
| **Adres IP/port witryny sieci Web**                         | Określa adres IP i numer portu witryny sieci Web, która ma zostać utworzona na serwerze intranetowym. Zmień wartość domyślną (\*) tylko wtedy, gdy nie będziesz używać tego samego adresu IP, który jest domyślną witryną sieci Web. Zmień numer portu na dostępny port tylko wtedy, gdy port domyślny (5151) jest już w użyciu.             | Jeśli w domyślnej witrynie sieci Web jest używany niedomyślny adres IP, Zapisz go tutaj: Jeśli domyślny numer portu jest już używany, Zapisz numer portu witryny sieci Web pakietu BHOLD tutaj: |

### <a name="database-settings"></a>Ustawienia bazy danych

| **Element**                                       | **Opis**                                                                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Korzystanie ze zintegrowanych zabezpieczeń**                    | Określa, że uwierzytelnianie systemu Windows jest używane w celu uzyskania dostępu do bazy danych.                                                                                                                                                                                                     | Zaznacz to pole wyboru, jeśli uwierzytelnianie systemu Windows jest używane do nawiązywania połączenia z SQL Server. Usuń zaznaczenie tego pola wyboru, jeśli jest używane uwierzytelnianie SQL Server. Baza danych musi zostać utworzona przed uruchomieniem Instalatora pakietu BHOLD Core, jeśli jest używane uwierzytelnianie SQL Server. **Uwaga:** Jeśli jest używane uwierzytelnianie systemu Windows, użytkownik musi być zalogowany przy użyciu konta z rolą serwera sysadmin na serwerze bazy danych. |
| Hasło **użytkownika bazy danych** i **bazy danych** | Określa nazwę użytkownika i hasło użytkownika z rolą serwera sysadmin na serwerze bazy danych. Te wartości są dostarczane tylko wtedy, gdy używane jest uwierzytelnianie SQL Server.                                                                                               | Napisz SQL Server nazwę użytkownika tutaj: Napisz SQL Server hasło użytkownika tutaj: **Uwaga:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji.                                                                                                                                                                                                                                                  |
| Nazwa **serwera bazy danych** i **bazy danych**   | Określa nazwę NetBIOS serwera bazy danych i nazwę bazy danych (domyślnie: B1), którą utworzy Instalator pakietu BHOLD Core. Jeśli nie używasz domyślnego wystąpienia serwera bazy danych, Określ wystąpienie serwera bazy danych w formularzu *\<server\>* \\ *\<wystąpienia\>* . | W tym miejscu wpisz nazwę serwera (lub serwera i wystąpienia): w tym miejscu wpisz nazwę bazy danych:                                                                                                                                                                                                                                                                                                                   |
| **Wprowadź ograniczenia dla użytkownika bazy danych**    | Nieaktualne.                                                                                                                                                                                                                                                                 | Nie zmieniaj wartości domyślnej                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Konfiguracja pakietu BHOLD Core

Aby zainstalować moduł pakietu BHOLD Core, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, na którym zamierzasz zainstalować moduł pakietu BHOLD Core w: 

- BholdCore *\<wersja\>* \_Release. msi

Zastąp *\<version\>wersją* wersji pakietu BHOLD Core, którą instalujesz.

Aby uruchomić plik programu jako administrator, kliknij plik prawym przyciskiem myszy, a następnie kliknij polecenie **Uruchom jako administrator**.

### <a name="postinstallation-settings"></a>Ustawienia Postinstallation

Po zakończeniu instalacji pakietu BHOLD Core należy skonfigurować zaporę systemu Windows i zmienić ustawienia zaawansowane w puli aplikacji pakietu BHOLD Core w Internet Information Services, aby ukończyć konfigurację pakietu BHOLD Core. W razie potrzeby należy również zmienić atrybuty systemu pakietu BHOLD, aby spełniały określone wymagania.

#### <a name="configuring-windows-firewall"></a>Konfigurowanie zapory systemu Windows

Jeśli użytkownicy będą uzyskiwać dostęp do usługi pakietu BHOLD przy użyciu przeglądarek sieci Web na komputerach zdalnych, należy skonfigurować zaporę systemu Windows na serwerze pakietu BHOLD Core, aby zezwolić na połączenia przychodzące do portu witryny sieci Web, który został określony podczas instalacji pakietu BHOLD Core.

Aby wykonać tę procedurę, użytkownik musi być członkiem grupy Administratorzy na komputerze lokalnym.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Aby zezwolić na połączenia przychodzące z witryną sieci Web pakietu BHOLD

1.  Kliknij przycisk **Start**, wskaż polecenie **Narzędzia administracyjne**, kliknij prawym przyciskiem myszy pozycję **Zapora systemu Windows z ustawieniami zaawansowanymi**, a następnie kliknij polecenie **Uruchom jako administrator**.

2.  W lewym okienku kliknij pozycję **reguły ruchu przychodzącego**, a następnie w prawym okienku kliknij pozycję **Nowa reguła**.

3.  W Kreatorze nowej reguły ruchu przychodzącego kliknij pozycję **port**, a następnie kliknij przycisk **dalej**.

4.  Upewnij się, że wybrano opcję **TCP** , w **określonych portach lokalnych**, wpisz domyślny numer portu pakietu BHOLD (5151) lub numer portu określony podczas instalacji pakietu BHOLD Core, a następnie kliknij przycisk **dalej**.

5.  Upewnij się, że jest zaznaczone **pole wyboru Zezwalaj na połączenie** , a następnie kliknij przycisk **dalej**.

6.  Na stronie **profil** wyczyść pola wyboru dla lokalizacji, z których nie chcesz uzyskiwać dostępu do witryny sieci Web pakietu BHOLD, a następnie kliknij przycisk **dalej**.

7.  Na stronie **Nazwa** wpisz nazwę reguły (na przykład Zezwalaj na połączenia przychodzące do pakietu BHOLD rdzeń), a następnie kliknij przycisk **Zakończ**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Włączanie aplikacji 32-bitowych dla puli aplikacji pakietu BHOLD Core

Aby zapewnić prawidłowe działanie usług IIS z modułem pakietu BHOLD Core, należy skonfigurować pulę aplikacji pakietu BHOLD Core, aby obsługiwała aplikacje 32-bitowe. Aby wykonać tę procedurę, użytkownik musi być zalogowany przy użyciu konta używanego do zainstalowania modułu pakietu BHOLD Core.

**Aby włączyć obsługę aplikacji 32-bitowych dla puli aplikacji pakietu BHOLD Core**

1.  Aby otworzyć Menedżera Internet Information Services, kliknij przycisk **Start**, wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij polecenie **Menedżer Internet Information Services (IIS)** .

2.  W drzewie konsoli rozwiń nazwę serwera, a następnie kliknij pozycję **Pule aplikacji**.

3.  Na liście **Pule aplikacji** kliknij prawym przyciskiem myszy pozycję **CoreAppPool**, a następnie kliknij pozycję **Ustawienia zaawansowane**.

4.  W oknie dialogowym **Ustawienia zaawansowane** na liście **Włącz 32-bitowe aplikacje** wybierz opcję **prawda**, a następnie kliknij przycisk **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Ustanawianie nazwy głównej usługi dla witryny sieci Web pakietu BHOLD

Jeśli nazwa sieci używana do kontaktowania się z witryną sieci Web pakietu BHOLD nie jest taka sama jak nazwa hosta serwera, należy ustanowić główną nazwę usługi (SPN) dla protokołu HTTP. Na przykład jeśli w systemie DNS używasz rekordu zasobu CNAME do określenia aliasu dla serwera lub jeśli używasz równoważenia obciążenia sieciowego, należy zarejestrować te dodatkowe adresy sieciowe w Active Directory. Jeśli wykonanie tej czynności nie powiedzie się, program Internet Explorer nie będzie mógł użyć protokołu Kerberos podczas kontaktowania się z witryną sieci Web pakietu BHOLD.

> [!IMPORTANT]
> Jeśli moduł pakietu BHOLD Core jest zainstalowany na tym samym komputerze co Portal programu FIM, należy utworzyć rekordy zasobów DNS (CNAME lub A) o różnych nazwach hostów dla serwerów z systemem pakietu BHOLD Core oraz na serwerze z uruchomionym portalem programu FIM. Dla określonej pary typu usługi/aliasu serwera można określić tylko jedną nazwę SPN, tak aby pakietu BHOLD rdzeń i Portal FIM wymagały oddzielnych nazw SPN, ponieważ zazwyczaj są one uruchamiane na różnych kontach. Setspn polecenie zgłasza błąd, jeśli nazwa SPN została już ustanowiona przy użyciu innego konta.

Członkostwo w grupie **Administratorzy domeny**, albo równoważnej, jest minimalnym wymaganiem do wykonania tej procedury.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Aby określić nazwę SPN witryny sieci Web pakietu BHOLD

1.  Na Active Directory Domain Services kontrolerze domeny kliknij przycisk **Start**, kliknij pozycję **Wszystkie programy**, kliknij pozycję **akcesoria**, kliknij prawym przyciskiem myszy pozycję **wiersz polecenia**, a następnie kliknij polecenie **Uruchom jako administrator**.

2.  W wierszu polecenia wpisz następujące polecenie, a następnie naciśnij klawisz ENTER: Setspn – S HTTP/ *\<networkalias\> \<domain\>* \\ *\<AccountName\>* , gdzie:

    -   *\<networkalias\>* to adres używany przez klientów do kontaktowania się z witryną sieci Web pakietu BHOLD

    -   *\<domeny\>* \\ *\<AccountName\>* to domena i nazwa użytkownika konta usługi pakietu BHOLD Core utworzonego podczas instalacji pakietu BHOLD Core.

3.  Powtórz poprzedni krok dla wszystkich innych nazw używanych przez klientów do kontaktowania się z witryną sieci Web pakietu BHOLD, na przykład aliasów CNAME, nazwy zawierające w pełni kwalifikowaną nazwę domeny lub nazwy, które zawierają nazwę NetBIOS (krótką).

#### <a name="setting-bhold-system-attributes"></a>Ustawianie atrybutów systemu pakietu BHOLD

Aby sprawdzić, czy instalacja modułu pakietu BHOLD Core zakończyła się pomyślnie, Otwórz Portal pakietu BHOLD Core i sprawdź atrybuty systemowe. Ponadto, aby upewnić się, że moduł pakietu BHOLD Core funkcjonuje prawidłowo w danym środowisku, można zmodyfikować następujące atrybuty systemu pakietu BHOLD w odpowiedni sposób:

| **Atrybut**                | **Opis**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nohistory**                | Ustaw wartość Y, jeśli witryna pakietu BHOLD jest uruchomiona w klastrowanej usłudze sieci Web, aby upewnić się, że ostatnio wyświetlane elementy działają prawidłowo. Ustaw wartość N, jeśli witryna sieci Web pakietu BHOLD jest uruchomiona na autonomicznym serwerze IIS.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Ustaw wartość Y, aby upewnić się, że jednostki organizacyjne (orgunits) można przenieść tylko do orgunits z tym samym typem organizacyjnym co element nadrzędny orgunit. Na przykład uniemożliwia to przeniesienie projektu do orgunit działu orgunit. Ustaw wartość N, aby zezwolić orgunit na umieszczenie w orgunit innego typu. |
| **Dni między ABA**     | Ustaw na dwucyfrową liczbę całkowitą, aby określić interwał (w dniach) między dwoma uruchomieniami uwierzytelniania opartego na atrybutach (ABA). Na przykład, aby określić, że uruchomienia ABA będą oddzielone przez dwa dni, wpisz 02.                                                                                                                     |
| **Godzina rozpoczęcia ABA**    | Ustaw na dwucyfrową liczbę całkowitą, aby określić godzinę dnia, o której nastąpi uruchomienie autoryzacji opartej na atrybutach. Na przykład, aby określić, że uruchomienie ABA będzie odbywać się o godzinie 11:00 (23:00), typ 23.                                                                                                             |
| **Kardynalność systemu**       | Ustaw wartość N, jeśli nie chcesz, aby kontrola kardynalności systemu pakietu BHOLD. Wartość domyślna to Y.                                                                                                                                                                                                                             |
| **Rejestrowanie**                  | Ustaw wartość N, jeśli nie chcesz, aby zmiany zostały zarejestrowane. Wartość domyślna to Y.                                                                                                                                                                                                                                            |
| **Przetwarzanie SystemQueue**   | Ustaw wartość N, jeśli nie chcesz przetwarzać kolejki systemowej. Nie zmieniaj tej wartości, chyba że zostanie to zlecone przez pomoc techniczną.                                                                                                                                                                                           |

Aby wykonać tę procedurę, użytkownik musi być zalogowany jako członek grupy Administratorzy domeny.

**Aby ustawić atrybut systemowy pakietu BHOLD**

1.  Kliknij przycisk **Start**, kliknij pozycję **Wszystkie programy**, a następnie kliknij pozycję **Internet Explorer**.

2.  W polu Adres wpisz, gdzie *\<server\>* jest nazwą serwera witryny sieci Web pakietu bhold i *\<port\>* jest numerem portu powiązanym z witryną sieci Web.

3.  Kliknij pozycję **Strona główna**, kliknij pozycję **wartości**, a następnie kliknij przycisk **Modyfikuj**.

4.  Znajdź nazwę atrybutu, który chcesz zmienić, wpisz nową wartość w polu obok nazwy atrybutu, a następnie kliknij przycisk **OK**.

## <a name="next-steps"></a>Następne kroki

Po zainstalowaniu programu pakietu BHOLD Core i zweryfikowaniu, że instalacja zakończyła się pomyślnie, można zainstalować dodatkowe moduły. W tym momencie baza danych pakietu BHOLD będzie zasadniczo pusta, która zawiera tylko jedno konto użytkownika, konto główne i jedną jednostkę organizacyjną (orgunit), orgunit główny. Aby dodać więcej użytkowników do bazy danych pakietu BHOLD, można zainstalować moduł łącznika zarządzania dostępem lub moduł generator modelu pakietu BHOLD, w zależności od potrzeb. Moduł łącznika zarządzania dostępem służy do importowania danych użytkownika z usługi synchronizacji programu FIM. można też użyć generatora modelu pakietu BHOLD do zaimportowania danych użytkownika z zestawu plików ze strukturą. Aby uzyskać więcej informacji o korzystaniu z modułu łącznika zarządzania dostępem, zobacz [Przewodnik po laboratorium testowym: Łącznik zarządzania dostępem pakietu BHOLD](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Aby uzyskać więcej informacji o korzystaniu z modułu generatora modelu pakietu BHOLD, zobacz:

- [Przewodnik dotyczący pojęć związanych z pakietem Microsoft pakietu BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft pakietu BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
