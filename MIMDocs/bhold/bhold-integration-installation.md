---
title: Instalacja integracji BHOLD FIM/MIM | Dokumentacja firmy Microsoft
description: "Moduł integracji BHOLD dodać roli samoobsługowego zarządzania MIM i usługi FIM"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-fimmim-integration-installation"></a>BHOLD FIM/MIM integracji instalacji

Moduł BHOLD FIM integracji dodaje zarządzania rolami samoobsługi do Microsof Identity Manager, co umożliwia użytkownikom na żądanie dodatkowych ról i wymuszania, które można wykonać na tych ról. Moduł BHOLD FIM integracji rozszerza portalu programu FIM, aby ułatwić zarządzanie rolami użytkowników w ramach ogólnej administracji programu FIM. W tym temacie opisano, jak należy skonfigurować infrastrukturę sieci, aby możliwe było zainstalować i używać modułu BHOLD FIM integracji. Obejmuje ona również sposób instalowania modułu BHOLD FIM integracji i konfiguracji wymaganej po zainstalowaniu modułu BHOLD FIM integracji.

## <a name="bhold-fim-integration-installation-requirements"></a>Wymagania dotyczące instalacji BHOLD FIM integracji

Moduł BHOLD FIM integracji rozszerza portalu programu FIM i usługi FIM aby umożliwić użytkownikom zarządzanie ich role w portalu programu FIM. Z tego powodu jest niezbędne, że moduł BHOLD Core i wymagane funkcje FIM zainstalować i skonfigurować przed zainstalowaniem modułu BHOLD FIM integracji.
Poniżej przedstawiono składniki oprogramowania, które musi znajdować się na komputerze przed zainstalowaniem modułu BHOLD FIM integracji:

- Program Microsoft Identity Manager 2016 portalu i usługi
- Program Microsoft Silverlight 3 lub nowszym
- Internetowe usługi informacyjne i platformy ASP.NET
- Narzędzia programu Microsoft Silverlight

Ponadto moduły BHOLD Core i dostępu do zarządzania łącznika musi już zostać wdrożony na serwerze w środowisku, a FIM musi mieć skonfigurowaną co najmniej jeden agent zarządzania BHOLD. Informacje o instalowaniu i konfigurowaniu modułu BHOLD Core, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Aby uzyskać informacje o instalowaniu i używaniu modułu dostępu do zarządzania łącznika, zobacz [instalacji łącznika zarządzania dostępu](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx) i [Przewodnik po laboratorium testowym: BHOLD dostępu administracyjnego łącznika](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx).

>[!IMPORTANT]
Nazwa bazy danych usługi FIM musi być usługę programu FIM. BHOLD FIM integracji instalacja nie powiedzie się, jeśli nie zainstalowano usługi FIM z domyślną nazwą bazy danych usługi FIM.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem instalowania modułu BHOLD FIM integracji, należy utworzyć katalog BHOLD w katalogu głównym dysku C: (C:\BHOLD).

Ponadto należy przygotować się do zawierają informacje, które Kreator BHOLD FIM integracji instalacji wymaga, aby ukończyć instalację. Następujący arkusz pomoże Ci Zarejestruj te informacje, więc wszystko będzie gotowe do dostarczenia go w razie potrzeby.

### <a name="bholdfim-account-settings"></a>Ustawienia konta BHOLDFim

| **Element**                            | **Opis**                                                                                                                                                                                                               | **Wartość**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Za pomocą dostawcy zabezpieczeń w domenie** | Po wybraniu Określa, że zabezpieczenia usług domenowych w usłudze Active Directory będzie kontrolował dostęp do podstawowych BHOLD.                                                                                                                    | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                          | Określa zawierający domeny **konto usługi** utworzonego podczas instalowania BHOLD Core. Aby uzyskać więcej informacji, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest nieprawidłowe. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (short), a nie w pełni kwalifikowaną nazwę (FQDN). Na przykład jeśli nazwa FQDN domeny, to fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **Nazwa użytkownika**                        | Określa nazwę logowania konta użytkownika usługi BHOLD Core.                                                                                                                                                              | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                        | Określa hasło konta użytkownika usługi.                                                                                                                                                                           | Zapisać hasło tutaj: **ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Ustawienia usługi FIM

| **Element**            | **Opis**                                                                                                                                                                                                                               | **Wartość**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **User**            | Określa nazwę logowania konta z uprawnieniami administratora dla programu FIM. Firma Microsoft zaleca, aby używać konta skojarzonego z użytkownika root w BHOLD Core (domyślnie konto używane do zainstalowania BHOLD Core). | Napisz tutaj nazwę konta użytkownika:                                                                   |
| **Hasło**        | Określa hasło konta administratora programu FIM.                                                                                                                                                                                 | Zapisać hasło tutaj: **ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji. |
| **Baza danych usługi FIM**    | Określa nazwę bazy danych usługi FIM.                                                                                                                                                                                               | Usługę programu FIM                                                                                          |
| **Port IP witryny sieci Web** | Określa nazwę lub adres IP serwera portalu programu FIM oraz port witryny sieci Web.                                                                                                                                                               | Napisz nazwę serwera lub adres i port w tym miejscu:                                                     |

### <a name="bhold-core-connection"></a>Połączenie podstawowe BHOLD

| **Element**               | **Opis**                                                                                                                                                                                                                                                                                                                                                                               | **Wartość**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domeny**             | Określa nazwę domeny konta określonego w **użytkownika**, poniżej. Określ domenę w formacie (short) NetBIOS.                                                                                                                                                                                                                                                                   | Zapis nazwę konta użytkownika domeny w tym miejscu?                                                            |
| **User**               | Nazwa logowania konta **użytkownik BHOLD kierownik** wszystkich użytkowników i role oraz ma uprawnienia do łącza i odłączyć ról użytkownika. Firma Microsoft zaleca, aby używać konta skojarzonego z użytkownika root w BHOLD Core (domyślnie konto używane do zainstalowania BHOLD Core). To konto może być to samo konto, używanej do nawiązania połączenia usługi FIM | Napisz tutaj nazwę konta użytkownika:                                                                   |
| **Hasło**           | Określa hasło konta użytkownika określonego w **użytkownika**.                                                                                                                                                                                                                                                                                                                             | Zapisać hasło tutaj: **ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji. |
| **Adres IP/komputera** | Określa adres IP podstawowego BHOLD serwera witryny sieci Web. Nie należy używać nazwy serwera.                                                                                                                                                                                                                                                                                                        | Napisz tutaj adres IP:                                                                          |
| **Numer portu**        | Określa numer portu witryny sieci Web BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Zapisz tutaj numer portu:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Instalator BHOLD FIM integracji

Aby zainstalować moduł BHOLD FIM integracji, zaloguj się jako członek grupy Administratorzy domeny, Pobierz następującego pliku i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł BHOLD FIM integracji na:

- BholdFIMIntegration*\<wersji\>*\_Release.msi

Zastąp  *\<wersji\>*  z numerem wersji instalowanej wersji BHOLD FIM integracji.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

![uruchomiona msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Zadań po instalacji

Po zainstalowaniu BHOLD FIM integracji, należy skonfigurować Microsoft SharePoint, nadaj uprawnienia właściciela witryny konta usługi BHOLD. Jeśli FIM Portal jest skonfigurowany do używania protokołu Secure Sockets Layer (SSL) zabezpieczeń, należy zmodyfikować pliki, które zawierają odwołania do adresów stron BHOLD dodany do portalu programu FIM.

### <a name="configuring-sharepoint"></a>Konfigurowanie programu SharePoint

Aby działać poprawnie, BHOLD FIM integracji wymaga konta usługi BHOLD uprawnień członek witryny Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Aby udzielić uprawnień członek witryny dla konta usługi BHOLD

1.  Zaloguj się na serwerze z uruchomioną BHOLD FIM integracji z uprawnieniami administratora.

2.  Kliknij przycisk **Start**, a następnie kliknij przycisk **Internet Exporer**.

3.  Na pasku adresu wpisz <https://localhost> programu SharePoint jest skonfigurowany do używania zabezpieczeń SSL, w przeciwnym razie wpisz <http://localhost>.

4.  Po lewej stronie **witryny zespołu** kliknij przycisk **osoby i grupy**.

5.  W obszarze **grup** kliknij **członkom zespołu**i na pasku narzędzi w środkowym okienku kliknij **nowy**, a następnie kliknij przycisk **Dodaj użytkowników**.

6.  Na **Dodawanie użytkowników: witryny zespołu** strony w **użytkowników i grup**, wpisz BHOLDApplicationGroup, a następnie kliknij przycisk Sprawdź nazwy w obszarze **użytkowników i grup** pole. Nazwa grupy powinien być rozpoznawany zawierają nazwę domeny.

7.  Kliknij przycisk **bezpośrednio nadać użytkownikom uprawnienia**, wybierz pozycję **Pełna kontrola — ma pełną kontrolę**, a następnie kliknij przycisk **OK**.

8.  Sprawdź, czy BHOLDApplicationGroup jest wyświetlane w **uprawnienia: witryny zespołu**, a następnie zamknij program Internet Explorer.

![uruchomiona msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Konfigurowanie BHOLD do obsługi protokołu SSL

Jeśli FIM Portal jest skonfigurowany do używania zabezpieczeń SSL, należy zmodyfikować pliki na serwerze programu FIM, aby łączy do stron BHOLD zostanie otwarty. Pliki znajdują się w następującym folderze: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

W poniższej tabeli wymieniono pliki i wersji oryginalnej i zmienione ciągów do edycji.

| **Plik**                  | **Oryginalnego ciągu**                                                                                                                   | **Ciąg zmienione**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.HTML | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.HTML       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.HTML |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.HTML |
| Selfservice.aspx          | RoleExchangePoint = http: / /\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint.svc TransportMode = transportu | RoleExchangePoint = https: / /\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint / BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Gdzie:

-   *\<BHOLD_Server\>*  Określa nazwę serwera BHOLD jako znaleziony w oryginalnej wersji pliku

-   *\<MIM_Server\>*  Określa nazwę serwera FIM jako znaleziony w oryginalnej wersji pliku

-   *\<BHOLD_Server_FQDN\>*  określa w pełni kwalifikowaną nazwę (FQDN) serwera BHOLD

-   *\<MIM_Port\>*  Określa numer portu serwera usługi programu FIM, tak jak w oryginalnej wersji pliku

-   *\<MIM_Server_FQDN\>*  Określa nazwę FQDN serwera usługi FIM

-   *\<MIM_SSL_Port\>*  Określa inny port do użycia przy użyciu protokołu SSL na serwerze programu FIM

### <a name="enable-approval-workflows-in-bhold-core"></a>Włącz przepływów pracy w BHOLD Core

Gdy FIM i BHOLD są zintegrowane dla samoobsługi, przepływy pracy dla zatwierdzenia są uruchamiane w usłudze FIM. Efekt jest podobny do modelu przepływu dla żądań, które pochodzą z portalu programu FIM, na przykład gdy użytkownik przesyła żądanie do dołączenia do listy dystrybucyjnej. Brak podstawowych różnic między przepływami pracy roli BHOLD a inne przepływy pracy, jednak hostowanych w usłudze FIM. W przypadku BHOLD parametry przepływu pracy, które określają, którzy użytkownicy muszą zatwierdzić żądanie roli pochodzić BHOLD, a nie są przechowywane w definicji przepływu pracy w bazie danych usługi FIM. Te parametry są dostarczane z usługą FIM przez BHOLD przy pierwszym żądaniu skierowanym i przepływu pracy akcji komunikuje się wyniki z powrotem do podstawowej BHOLD.

BHOLD wybiera osobą zatwierdzającą dla żądania samoobsługi w jednym z trzech sposobów:

-   **Menedżer wierszy jako osoba zatwierdzająca: wybór opartej na rolach dla jednostki organizacyjnej (OrgUnit)** roli ma atrybut o nazwie roletype ustawionym osoba zatwierdzająca lub schody ruchome, a jeśli ta rola jest połączony z co najmniej jednego użytkownika w kontekście OrgUnit, żąda od Użytkownicy w tym OrgUnit musi być zatwierdzony przez jeden z użytkowników, które jest połączone z roli z roletype osoba zatwierdzająca lub schody ruchome.

-   **Menedżer wierszy jako osoba zatwierdzająca: opartych na atrybutach wybór OrgUnit** OrgUnit każdy może mieć jeden lub więcej atrybutów, które określają aliasy użytkowników, którzy mogą zatwierdzić przypisania roli dla innych użytkowników w OrgUnit. Te atrybuty są nazywane approver1, approver2 i tak dalej. Gdy użytkownik w OrgUnit zażąda przypisania roli, BHOLD kieruje żądanie (za pośrednictwem programu FIM) do użytkowników określonych przez atrybuty osoba zatwierdzająca OrgUnit. Jeśli OrgUnit nie ma zestawu tych atrybutów, BHOLD sprawdza nadrzędnej OrgUnits do głównego OrgUnit.

-   **Menedżer ról jako osoba zatwierdzająca: opartych na atrybutach wyboru roli** rola może mieć jeden lub więcej atrybutów (również nazwanego approver1 itd.) określające aliasy użytkowników, którzy mogą zatwierdzić przypisania roli. Gdy użytkownik zażąda do przypisania roli, która ma ustawiony atrybut te osoby zatwierdzającej, BHOLD kieruje żądanie do użytkowników określonych przez atrybuty.

Jeśli nie określono osobą zatwierdzającą dla żądania samoobsługi roli za pomocą jednej z tych metod, domyślnie BHOLD automatycznie przypisuje rolę bez konieczności zatwierdzenia. Z tego powodu od razu po zainstalowaniu BHOLD FIM integracji, należy skonfigurować główny OrgUnit z aliasem osoba zatwierdzająca, takich jak konta głównego. Uniemożliwi to użytkownik przypadkowo udzielenia rolę przed bardziej szczegółowe zasady zatwierdzenia.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Aby skonfigurować osobą zatwierdzającą dla elementu głównego OrgUnit

1.  Zaloguj się na serwer podstawowy BHOLD jako administrator.

2.  Kliknij przycisk **Start**, a następnie kliknij przycisk **programu Internet Explorer**.

3.  Na pasku adresu programu Internet Explorer wpisz <core-http://localhost:5151/bhold>, a następnie naciśnij klawisz Enter.

4.  W przypadku jądra BHOLD strony głównej, w obszarze **def atrybutu**, kliknij przycisk **typy atrybutów**.

5.  Na **typ atrybutu** kliknij przycisk **Dodaj**.

6.  Na **Dodaj atrybut typu strony**w **tożsamości**, wpisz approver1, **— typ danych** kliknij **alfanumeryczne**, w **Maksymalna długość**, wpisz 255, **listy wartości**, kliknij przycisk **nr**w **angielski**, wpisz 1 osoba zatwierdzająca, kliknij przycisk **OK**, a następnie kliknij przycisk **gotowe**.

7.  W lewym okienku w obszarze **def atrybutu** kliknij **ustawia typ atrybutu**.

8.  Na **ustawia typ atrybutu** kliknij przycisk **Dodaj**.

9.  Na **dodać zestawu atrybutów typu** strony w **opis**wpisz OrgUnit atrybutów, a następnie kliknij przycisk **OK**.

10. Na **atrybuty OrgUnit** rozwiń pozycję **typy atrybutów**, a następnie kliknij przycisk **Modyfikuj**.

11. W **typ atrybutu** kliknij **approver1**, kliknij przycisk **Dodaj**, a następnie kliknij przycisk **gotowe**.

12. W okienku po lewej stronie kliknij **typy obiektów**.

13. Na **typy obiektów** kliknij przycisk **OrgUnit**.

14. Na **obiekt typu/OrgUnit** rozwiń pozycję **ustawia typ atrybutu**, a następnie kliknij przycisk **Modyfikuj**.

15. Na **łącza typu atrybutu zestawu/OrgUnit** strony w **kolejności**, 10, należy wpisać w **ustawiony atrybut typu** kliknij **atrybuty OrgUnit**, Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **gotowe**.

16. W lewym okienku w obszarze **modelu**, kliknij przycisk **jednostek organizacyjnych**.

17. Na **jednostek organizacyjnych** kliknij przycisk **głównego**.

18. Na **jednostki organizacyjnej/root** kliknij przycisk **Modyfikuj**.

19. Na **zmodyfikować jednostki organizacyjnej atrybuty/root** strony w **osoba zatwierdzająca**, wpisz nazwę domeny i nazwę użytkownika, który będzie zatwierdzać żądania przypisania roli, w formacie  *\<domeny\>*\\*\<użytkownika\>*, gdzie  *\<domeny\>*  jest Nazwa domeny NetBIOS (skrót) i  *\<użytkownika\>*  jest nazwą logowania użytkownika.
20. Kliknij przycisk **OK**.

>[!IMPORTANT]
Domena i nazwa użytkownika musi być zgodna domyślny alias użytkownika w bazie danych BHOLD Core.

Jako alternatywę do określania osobą zatwierdzającą dla jednostek organizacyjnych można określić osobą zatwierdzającą dla proponowanych ról w bazie danych BHOLD Core. W tym celu należy utworzyć atrybutu approver1, dodaj go do atrybutu poziomego złożenia skojarzone z typem obiektu roli, a następnie zmodyfikuj każdej roli proponowanych określone osoby zatwierdzającej.

Aby zapewnić lepsze zabezpieczenia przepływu pracy, oprócz osób zatwierdzających, należy wyznaczyć tryby dodatkowego zatwierdzenia i użytkowników przez tworzenie i wypełnianie następujące atrybuty OrgUnits i ról:

- Schody ruchome*\<n\>*

- Właściciel*\<n\>*

- securityOfficer*\<n\>*

- powiadomienia*\<n\>*

gdzie  *\< n \>*  wskazuje opcjonalne liczbowego sufiksu zapewnienie wiele atrybutów tego samego typu.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Sprawdź przepływów pracy skonfigurowane w usłudze FIM

BHOLD FIM integracji instalacji tworzy zestawy, definicje przepływów pracy i reguły zasad zarządzania (MPR) z usługą FIM. Jeśli były dostosowane wdrożenia FIM można zmienić grupy Administratorzy i grupy użytkowników, którzy mogą wysyłać żądania, należy upewnić się, czy MPR odwołują się do zestawów użytkownika.

>[!NOTE]
Zanim użytkownicy portalu FIM można użyć funkcji samoobsługi udostępniane przez BHOLD, należy zsynchronizować konta użytkowników do BHOLD bazy danych usługi synchronizacji programu FIM. W szczególności musi istnieć rekord użytkownika w bazie danych BHOLD rdzeni i w bazie danych usługi FIM dla każdego użytkownika, który można wysłać żądanie samoobsługi lub jest określony jako osoba zatwierdzająca lub schody ruchome dla żądań samoobsługi.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje dotyczące instalowania portalu programu FIM i inne funkcje programu FIM, zobacz [planowanie i architektura](https://technet.microsoft.com/library/ee808044.aspx) w bibliotece technicznej firmy Microsoft Forefront.
- [Przewodnik instalacji BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
