---
title: Ogólny łącznik LDAP | Microsoft Docs
description: W tym artykule opisano sposób konfigurowania łącznika ogólnego LDAP firmy Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.reviewer: davidste
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 06/26/2018
ms.author: billmath
ms.openlocfilehash: 5b19b4fd9d45797fcc6b02091386a27aec3c0abf
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835727"
---
# <a name="generic-ldap-connector-technical-reference"></a>Dokumentacja techniczna ogólnego łącznika LDAP
W tym artykule opisano ogólny łącznik LDAP. Artykuł dotyczy następujących produktów:

* Microsoft Identity Manager 2016 (programie MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Należy użyć poprawki 4.1.3671.0 lub nowszej [KB3092178](https://support.microsoft.com/kb/3092178).

W przypadku programie MIM2016 i FIM2010R2 łącznik jest dostępny do pobrania z [Centrum pobierania Microsoft](https://go.microsoft.com/fwlink/?LinkId=717495).

W przypadku odwoływania się do specyfikacji IETF RFC w tym dokumencie jest używany format (RFC [numer RFC]/[sekcja w dokumencie RFC]), na przykład (RFC 4512/4.3).
Więcej informacji można znaleźć pod adresem [https://tools.ietf.org/](https://tools.ietf.org/) . W lewym panelu wprowadź numer RFC w oknie dialogowym **pobieranie dokumentu** i przetestuj go, aby upewnić się, że jest on prawidłowy.

## <a name="overview-of-the-generic-ldap-connector"></a>Omówienie łącznika ogólnego LDAP
Ogólny łącznik LDAP umożliwia integrację usługi synchronizacji z serwerem LDAP v3.

Niektóre operacje i elementy schematu, takie jak te, które są konieczne do wykonania importu różnicowego, nie są określone w dokumentach RFC IETF. W przypadku tych operacji obsługiwane są tylko katalogi LDAP określone jawnie.

W celu nawiązania połączenia z katalogami testuje się przy użyciu konta głównego/administratora.  Aby użyć innego konta do zastosowania bardziej szczegółowych uprawnień, może być konieczne zapoznanie się z zespołem katalogu LDAP.

Z perspektywy wysokiego poziomu następujące funkcje są obsługiwane przez bieżącą wersję łącznika:

| Cecha | Pomoc techniczna |
| --- | --- |
| Połączone źródło danych |Łącznik jest obsługiwany przez wszystkie serwery LDAP v3 (zgodne ze standardem RFC 4510). Został przetestowany z następującymi: <li>Microsoft Usługi LDS Active Directory (AD LDS)</li><li>Wykaz globalny Microsoft Active Directory (AD GC)</li><li>Serwer katalogowy 389</li><li>Serwer usługi Apache</li><li>IBM Tivoli DS</li><li>Katalog isode</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Otwórz DJ</li><li>Otwórz usługi DS</li><li>Otwórz katalog LDAP (openldap.org)</li><li>Oracle (wcześniej Sun) Directory Server Enterprise Edition</li><li>Serwer katalogu wirtualnego RadiantOne (VDS)</li><li>Serwer Sun z jednym katalogiem</li><li>Microsoft Active Directory Domain Services (AD DS)</li><ul><li>W przypadku większości scenariuszy należy użyć wbudowanego łącznika Active Directory, a niektóre funkcje mogą nie być obsługiwane</li></ul>**Ważne znane katalogi lub funkcje nie są obsługiwane:**<li>Microsoft Active Directory Domain Services (AD DS)<ul><li>Usługa powiadamiania o zmianie hasła (PCNS)</li><li>Inicjowanie obsługi administracyjnej programu Exchange</li><li>Usuwanie aktywnych urządzeń synchronizacji</li><li>Obsługa nTDescurityDescriptor</li></ul></li><li>Oracle Internet Directory (OID)</li> |
| Scenariusze |<li>Zarządzanie cyklem życia obiektów</li><li>Zarządzanie grupami</li><li>Zarządzanie hasłami</li> |
| Operacje |W przypadku wszystkich katalogów LDAP obsługiwane są następujące operacje: <li>Pełny import</li><li>Eksportowanie</li>Następujące operacje są obsługiwane tylko dla określonych katalogów:<li>Import zmian</li><li>Ustawianie hasła, zmiana hasła</li> |
| Schemat |<li>Wykryto schemat w schemacie LDAP (RFC3673 i RFC4512/4.2)</li><li>Obsługuje klasy strukturalne, klasy pomocnicze i klasę obiektu rozszerzalnobject (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Obsługa importu różnicowego i zarządzania hasłami
Obsługiwane katalogi dla importu różnicowego i zarządzania hasłami:

* Microsoft Usługi LDS Active Directory (AD LDS)
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła
* Wykaz globalny Microsoft Active Directory (AD GC)
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła
* Serwer katalogowy 389
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła
* Serwer usługi Apache
  * Nie obsługuje importowania różnicowego, ponieważ ten katalog nie ma trwałego dziennika zmian
  * Obsługuje Ustawianie hasła
* IBM Tivoli DS
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła
* Katalog isode
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła
* Novell eDirectory i NetIQ eDirectory
  * Obsługuje operacje dodawania, aktualizowania i zmiany nazwy dla importu różnicowego
  * Nie obsługuje operacji usuwania dla importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła
* Otwórz DJ
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła
* Otwórz usługi DS
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła
* Otwórz katalog LDAP (openldap.org)
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła
  * Nie obsługuje zmiany hasła
* Oracle (wcześniej Sun) Directory Server Enterprise Edition
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła
* Serwer katalogu wirtualnego RadiantOne (VDS)
  * Musi być w wersji 7.1.1 lub nowszej
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła
* Serwer Sun z jednym katalogiem
  * Obsługuje wszystkie operacje importu różnicowego
  * Obsługuje Ustawianie hasła i Zmienianie hasła

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika upewnij się, że na serwerze synchronizacji są dostępne następujące elementy:

* Microsoft .NET 4.5.2 Framework lub nowszy

### <a name="detecting-the-ldap-server"></a>Wykrywanie serwera LDAP
Łącznik korzysta z różnych technik wykrywających i identyfikujących serwer LDAP. Łącznik używa elementu głównego DSE, nazwy dostawcy/wersji i sprawdza schemat w celu znalezienia unikatowych obiektów i atrybutów, które wiadomo, że istnieją na niektórych serwerach LDAP. Te dane, jeśli zostały znalezione, są używane do wstępnego wypełniania opcji konfiguracji w łączniku.

### <a name="connected-data-source-permissions"></a>Uprawnienia połączonego źródła danych
Aby wykonać operacje importowania i eksportowania obiektów w połączonym katalogu, konto łącznika musi mieć wystarczające uprawnienia. Łącznik musi mieć uprawnienia do zapisu, aby można było eksportować i uprawnienia do odczytu. Konfiguracja uprawnień jest przeprowadzana w ramach środowiska zarządzania samego katalogu docelowego.

### <a name="ports-and-protocols"></a>Porty i protokoły
Łącznik używa numeru portu określonego w konfiguracji, który domyślnie jest 389 dla LDAP i 636 dla LDAPs.

W przypadku LDAPs należy użyć protokołu SSL 3,0 lub TLS. Protokół SSL 2,0 nie jest obsługiwany i nie można go aktywować.

### <a name="required-controls-and-features"></a>Wymagane kontrolki i funkcje
Aby łącznik działał prawidłowo, muszą być dostępne następujące kontrolki/funkcje LDAP na serwerze LDAP:  
`1.3.6.1.4.1.4203.1.5.3` Filtry true/false

Filtr prawda/fałsz jest często nieraportowany jako obsługiwany przez katalogi LDAP i może być wyświetlany na **stronie globalnej** w obszarze **obowiązkowe funkcje nie zostały znalezione**. Służy do tworzenia **lub** filtrowania kwerend LDAP, na przykład podczas importowania wielu typów obiektów. Jeśli można zaimportować więcej niż jeden typ obiektu, serwer LDAP obsługuje tę funkcję.

W przypadku korzystania z katalogu, w którym unikatowym identyfikatorem jest zakotwiczenie, należy również udostępnić następujące informacje (Aby uzyskać więcej informacji, zobacz sekcję [Konfigurowanie kotwic](#configure-anchors) ):  
`1.3.6.1.4.1.4203.1.5.1` Wszystkie atrybuty operacyjne

Jeśli katalog zawiera więcej obiektów niż można dopasować do katalogu, zaleca się używanie stronicowania. Aby stronicowanie działało, potrzebna jest jedna z następujących opcji:

**Opcja 1:**  
`1.2.840.113556.1.4.319` pagedResultsControl

**Opcja 2:**  
`2.16.840.1.113730.3.4.9` VLVControl  
`1.2.840.113556.1.4.473` SortControl

W przypadku włączenia obu opcji w konfiguracji łącznika pagedResultsControl jest używana.

`1.2.840.113556.1.4.417` ShowDeletedControl

ShowDeletedControl jest używana tylko z metodą import Delta USNChanged, aby można było zobaczyć usunięte obiekty.

Łącznik próbuje wykryć opcje dostępne na serwerze. Jeśli nie można wykryć opcji, na stronie globalnej we właściwościach łącznika jest wyświetlane ostrzeżenie. Nie wszystkie serwery LDAP zawierają wszystkie formanty/funkcje, które obsługują, a nawet jeśli to ostrzeżenie jest obecne, łącznik może być niesprawny.

### <a name="delta-import"></a>Import zmian
Import Delta jest dostępny tylko po wykryciu katalogu obsługi. Obecnie są używane następujące metody:

* Accesslog LDAP. Zobacz [ http://www.openldap.org/doc/admin24/overlays.html#Access Rejestrowanie](http://www.openldap.org/doc/admin24/overlays.html#Access%20Logging)
* Dziennik zmian LDAP. Wyświetlania [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Znacznik czasu. W przypadku usługi Novell/NetIQ eDirectory łącznik używa ostatniej daty/godziny do uzyskania utworzonych i zaktualizowanych obiektów. Firma Novell/NetIQ eDirectory nie zapewnia równoważnej metody do pobierania usuniętych obiektów. Tej opcji można również użyć, jeśli na serwerze LDAP nie jest aktywna żadna inna metoda importowania różnicowego. Ta opcja nie umożliwia importowania usuniętych obiektów.
* USNChanged. Wyświetlania [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nieobsługiwane
Następujące funkcje LDAP nie są obsługiwane:

* Odwołania LDAP między serwerami (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Utwórz nowy łącznik
Aby utworzyć ogólny łącznik LDAP, w obszarze **usługa synchronizacji** wybierz pozycję **agent zarządzania** i **Utwórz**. Wybierz łącznik **Generic LDAP (Microsoft)** .

![Połączenie](./media/microsoft-identity-manager-2016-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Łączność
Na stronie łączność należy określić informacje dotyczące hosta, portu i powiązania. W zależności od tego, które powiązanie zostało zaznaczone, dodatkowe informacje mogą być podane w poniższych sekcjach.

![Łączność](./media/microsoft-identity-manager-2016-connector-genericldap/connectivity.png)

* Ustawienie limitu czasu połączenia jest używane tylko podczas pierwszego połączenia z serwerem podczas wykrywania schematu.
* Jeśli wiązanie jest anonimowe, wówczas nie są używane żadne nazwy użytkownika/hasła ani certyfikaty.
* W przypadku innych powiązań wprowadź informacje w polu Nazwa użytkownika/hasło lub wybierz certyfikat.
* W przypadku uwierzytelniania przy użyciu protokołu Kerberos należy również podać obszar/domenę użytkownika.

Pole tekstowe **aliasy atrybutów** jest używane dla atrybutów zdefiniowanych w schemacie ze składnią RFC4522. Te atrybuty nie mogą być wykrywane podczas wykrywania schematu, a łącznik potrzebuje pomocy, aby zidentyfikować te atrybuty. Na przykład następujące elementy muszą zostać wprowadzone w polu aliasy atrybutów, aby poprawnie identyfikować atrybut userCertificate jako atrybut binarny:

`userCertificate;binary`

Poniżej przedstawiono przykład sposobu, w jaki ta konfiguracja może wyglądać następująco:

![Łączność](./media/microsoft-identity-manager-2016-connector-genericldap/connectivityattributes.png)

Zaznacz pole wyboru **Uwzględnij atrybuty operacyjne w schemacie** , aby uwzględnić również atrybuty utworzone przez serwer. Należą do nich atrybuty, takie jak godzina utworzenia obiektu i czas ostatniej aktualizacji.

Wybierz opcję **Dołącz atrybuty rozszerzalne w schemacie** , jeśli obiekty rozszerzalne (RFC4512/4.3) są używane i włączenie tej opcji pozwala na użycie każdego atrybutu dla wszystkich obiektów. Wybranie tej opcji sprawia, że schemat jest bardzo duży, chyba że podłączony katalog używa tej funkcji, zaleca się pozostawienie zaznaczenia opcji.

### <a name="global-parameters"></a>Parametry globalne
Na stronie parametry globalne można skonfigurować nazwę wyróżniającą w dzienniku zmian różnicowych i dodatkowych funkcjach LDAP. Strona jest wstępnie wypełniona informacjami podanymi przez serwer LDAP.

![Łączność](./media/microsoft-identity-manager-2016-connector-genericldap/globalparameters.png)

Górna sekcja zawiera informacje udostępniane przez sam serwer, na przykład nazwę serwera. Łącznik sprawdza także, czy kontrolki obowiązkowe są obecne w głównym DSE. Jeśli te kontrolki nie są wyświetlane na liście, zostanie wyświetlone ostrzeżenie. Niektóre katalogi LDAP nie wyświetlają wszystkich funkcji w głównym DSE i istnieje możliwość, że łącznik działa bez problemów, nawet jeśli występuje ostrzeżenie.

Pola wyboru **obsługiwane kontrolki** kontrolują zachowanie niektórych operacji:

* Po zaznaczeniu drzewa jest usuwana hierarchia z jednym wywołaniem LDAP. W przypadku usunięcia zaznaczenia drzewa łącznik wykonuje cykliczne usuwanie, jeśli jest to potrzebne.
* Po wybraniu wyników z stronicowania łącznik wykonuje zaimportowane stronicowanie o rozmiarze określonym w kroku przebiegu.
* VLVControl i SortControl są alternatywą dla pagedResultsControl do odczytu danych z katalogu LDAP.
* Jeśli wszystkie trzy opcje (pagedResultsControl, VLVControl i SortControl) są niewybrane, łącznik importuje wszystkie obiekty w jednej operacji, co może się nie powieść, jeśli jest to duży katalog.
* ShowDeletedControl jest używana tylko wtedy, gdy metoda importu różnicowego to USNChanged.

Nazwa DN dziennika zmian jest kontekstem nazewnictwa używanym przez dziennik zmian różnicowych, na przykład **CN = dziennik** zmian. Ta wartość musi być określona, aby można było importować przyrostowo.

Poniżej znajduje się lista domyślnych dzienników zmian DNs:

| Katalog | Dziennik zmian różnicowych |
| --- | --- |
| Microsoft AD LDS i AD GC |Wykrywane automatycznie. USNChanged. |
| Serwer usługi Apache |Niedostępne. |
| Katalog 389 |Dziennik zmian. Wartość domyślna do użycia: **CN = dziennika zmian** |
| IBM Tivoli DS |Dziennik zmian. Wartość domyślna do użycia: **CN = dziennika zmian** |
| Katalog isode |Dziennik zmian. Wartość domyślna do użycia: **CN = dziennika zmian** |
| Novell/NetIQ eDirectory |Niedostępne. Znacznik czasu. Łącznik używa daty/godziny ostatniej aktualizacji w celu uzyskania dodanych i zaktualizowanych rekordów. |
| Otwórz DJ/DS |Dziennik zmian.  Wartość domyślna do użycia: **CN = dziennika zmian** |
| Otwórz katalog LDAP |Dziennik dostępu. Wartość domyślna do użycia: **CN = accesslog** |
| DSEE firmy Oracle |Dziennik zmian. Wartość domyślna do użycia: **CN = dziennika zmian** |
| RadiantOne VDS |Katalog wirtualny. Zależy od katalogu podłączonego do usługi VDS. |
| Serwer Sun z jednym katalogiem |Dziennik zmian. Wartość domyślna do użycia: **CN = dziennika zmian** |

Atrybut Password jest nazwą atrybutu, który ma być używany przez łącznik do ustawiania hasła w operacjach zmiany hasła i ustawiania hasła.
Ta wartość jest domyślnie ustawiona na **userPassword** , ale można ją zmienić w razie potrzeby dla określonego systemu LDAP.

Na liście dodatkowe partycje można dodać dodatkowe przestrzenie nazw, które nie są wykrywane automatycznie. Można na przykład użyć tego ustawienia, jeśli kilka serwerów tworzy klaster logiczny, który powinien zostać zaimportowany w tym samym czasie. Tak jak Active Directory może mieć wiele domen w jednym lesie, ale wszystkie domeny współdzielą jeden schemat, to samo może być symulowane przez wprowadzenie dodatkowych przestrzeni nazw w tym polu. Każda przestrzeń nazw może importować z różnych serwerów i być skonfigurowana na stronie Konfiguruj partycje i hierarchie. Naciśnij klawisze CTRL + ENTER, aby uzyskać nowy wiersz.

### <a name="configure-provisioning-hierarchy"></a>Skonfiguruj hierarchię aprowizacji
Ta strona służy do mapowania składnika nazwy wyróżniającej, na przykład jednostki organizacyjnej, na typ obiektu, który powinien być zainicjowany, na przykład organizationalUnit.

![Hierarchia aprowizacji](./media/microsoft-identity-manager-2016-connector-genericldap/provisioninghierarchy.png)

Konfigurując hierarchię aprowizacji, można skonfigurować łącznik tak, aby automatycznie utworzył strukturę w razie potrzeby. Na przykład jeśli istnieje przestrzeń nazw DC = contoso, DC = com i nowy obiekt CN = Jan, OU = Seattle, c = US, DC = contoso, DC = com jest inicjowany, łącznik może utworzyć obiekt typu kraj dla Stanów Zjednoczonych i organizationalUnit dla Seattle, jeśli te nie znajdują się jeszcze w katalogu.

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie
Na stronie partycje i hierarchie zaznacz wszystkie przestrzenie nazw z obiektami, które mają zostać zaimportowane i wyeksportowane.

![Partycje](./media/microsoft-identity-manager-2016-connector-genericldap/partitions.png)

Dla każdej przestrzeni nazw można również skonfigurować ustawienia łączności, które przesłonić wartości określone na ekranie łączności. Jeśli te wartości są pozostawione jako domyślne wartości puste, zostanie użyta informacja z ekranu łączności.

Istnieje również możliwość wybrania kontenerów i jednostek organizacyjnych, z których łącznik powinien importować i eksportować.

Podczas wykonywania wyszukiwania odbywa się to we wszystkich kontenerach w partycji. W przypadkach, gdy istnieje duża liczba kontenerów, to zachowanie prowadzi do obniżenia wydajności.

> [!NOTE]
> Począwszy od aktualizacji z marca 2017 do ogólnego wyszukiwania łączników LDAP można ograniczyć zakres tylko do wybranych kontenerów. Można to zrobić, zaznaczając pole wyboru "Wyszukaj tylko w wybranych kontenerach", jak pokazano na poniższej ilustracji.

![Przeszukaj tylko wybrane kontenery](./media/microsoft-identity-manager-2016-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Konfiguruj zakotwiczenia
Ta strona ma zawsze wstępnie skonfigurowaną wartość i nie można jej zmienić. Jeśli dostawca serwera został zidentyfikowany, zakotwiczenie może zostać zapełnione z niezmiennym atrybutem, na przykład identyfikator GUID obiektu. Jeśli nie został on wykryty lub wiadomo, że nie ma atrybutu niezmiennego, łącznik używa nazwy DN (nazwa wyróżniająca) jako zakotwiczenia.

![Kotwice](./media/microsoft-identity-manager-2016-connector-genericldap/anchors.png)


Poniżej znajduje się lista serwerów LDAP i używanych kotwic:

| Katalog | Zakotwiczony atrybut |
| --- | --- |
| Microsoft AD LDS i AD GC |objectGUID |
| Serwer katalogowy 389 |dn |
| Katalog Apache |dn |
| IBM Tivoli DS |dn |
| Katalog isode |dn |
| Novell/NetIQ eDirectory |GUID |
| Otwórz DJ/DS |dn |
| Otwórz katalog LDAP |dn |
| ODSEE firmy Oracle |dn |
| RadiantOne VDS |dn |
| Serwer Sun z jednym katalogiem |dn |

## <a name="other-notes"></a>Inne notatki
Ta sekcja zawiera informacje o aspektach, które są specyficzne dla tego łącznika lub z innych przyczyn.

### <a name="delta-import"></a>Import zmian
Przyrostowy znak wodny w otwartym LDAP to data/godzina UTC. Z tego powodu zegary między usługą synchronizacji programu FIM i otwartym katalogiem LDAP muszą zostać zsynchronizowane. W przeciwnym razie niektóre wpisy w dzienniku zmian różnicowych mogą zostać pominięte.

W przypadku programu Novell eDirectory import Delta nie wykrywa żadnych usuniętych obiektów. Z tego powodu konieczne jest okresowe uruchamianie pełnego importowania w celu znalezienia wszystkich usuniętych obiektów.

W przypadku katalogów z dziennikiem zmian różnicowych, który jest oparty na dacie i godzinie, zdecydowanie zaleca się uruchomienie pełnego importu w regularnych odstępach czasu. Ten proces umożliwia aparatowi synchronizacji znalezienie i rozróżnienie między serwerem LDAP a aktualną przestrzenią łącznika.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
* Aby uzyskać informacje na temat włączania rejestrowania w celu rozwiązywania problemów z łącznikiem, zobacz [jak włączyć śledzenie ETW dla łączników](https://go.microsoft.com/fwlink/?LinkId=335731).
