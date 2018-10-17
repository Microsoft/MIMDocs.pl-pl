---
title: Instalacja integracji pakietu BHOLD FIM/programu MIM | Dokumentacja firmy Microsoft
description: Moduł integracji pakietu BHOLD Dodaj zarządzania rolą samoobsługi do programu MIM i programu FIM
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 317c9ae4c940a509b6ac328cd5bb7cd7baa4dde9
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358809"
---
# <a name="bhold-fimmim-integration-installation"></a>Instalacja programu FIM/MIM integracji pakietu BHOLD

Moduł integracji usługi FIM BHOLD dodano funkcję zarządzania rolą samoobsługi do Microsof programu Identity Manager, co umożliwia użytkownikom na żądanie dodatkowe role i wymuszania, które można wykonać na tych ról. Moduł integracji usługi FIM BHOLD rozszerza portalu programu FIM, aby ułatwić zarządzanie rolami użytkowników w ramach ogólnej administracji programu FIM. W tym temacie opisano, jak należy skonfigurować infrastrukturę sieci, aby umożliwić instalowanie i używanie modułu BHOLD FIM integracji. Omówiono również sposób instalowania modułu BHOLD FIM integracji oraz konfiguracji wymaganej po zainstalowaniu modułu BHOLD FIM integracji.

## <a name="bhold-fim-integration-installation-requirements"></a>Wymagania dotyczące instalacji pakietu BHOLD FIM integracji

Moduł integracji usługi FIM pakietu BHOLD rozszerza portalu programu FIM i usługi FIM Service umożliwiającą użytkownikom zarządzanie ich ról w portalu programu FIM. Z tego powodu jest istotne, że moduł BHOLD Core i niezbędnych funkcji programu FIM należy zainstalować i skonfigurować przed zainstalowaniem modułu BHOLD FIM integracji.
Poniżej przedstawiono składniki oprogramowania, które musi być obecny na komputerze, zanim będzie można zainstalować moduł BHOLD FIM integracji:

- Microsoft Identity Manager 2016 portalem i usługą
- Program Microsoft Silverlight 3 lub nowszym
- Internetowe usługi informacyjne i platformy ASP.NET
- Narzędzia Microsoft Silverlight

Ponadto modułów BHOLD Core i dostęp do łącznika zarządzania muszą już być rozmieszczone na serwerze w środowisku i FIM musi mieć skonfigurowaną co najmniej jednego pakietu BHOLD agenta zarządzania. Aby uzyskać informacji o instalowaniu i konfigurowaniu moduł BHOLD Core, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Uzyskać informacji o instalowaniu i używaniu modułu dostępu do zarządzania łącznika, zobacz [instalacja łącznika do dostępu do zarządzania](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) i [Przewodnik po laboratorium testowym: łącznika zarządzania dostęp do pakietu BHOLD](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> Nazwa bazy danych usługi FIM musi być usługę programu FIM. Instalacja pakietu BHOLD FIM Integration zakończy się niepowodzeniem, jeśli FIM nie została zainstalowana z domyślną nazwą bazy danych usługi FIM.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem instalowania modułu BHOLD FIM integracji, trzeba utworzyć katalog pakietu BHOLD w katalogu głównym dysku C: (C:\BHOLD).

Ponadto, musisz mieć możliwość informacje, które Kreator pakietu BHOLD FIM integracji instalacji wymaga, aby ukończyć instalację. Następujący arkusz pomoże Zarejestruj te informacje będą gotowe do dostarczenia go, gdy jest to konieczne.

### <a name="bholdfim-account-settings"></a>Ustawienia konta BHOLDFim

| **Element**                            | **Opis**                                                                                                                                                                                                               | **Wartość**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń w domenie** | Po wybraniu Określa, czy zabezpieczeń Active Directory Domain Services będzie kontrolować dostęp do pakietu BHOLD Core.                                                                                                                    | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                          | Określa domeny zawierający **konto usługi** utworzony podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę, tylko wtedy, gdy będzie ona nieprawidłowa. **Ważne:** Podaj nazwę domeny przy użyciu nazwy NetBIOS (krótki), a nie w pełni kwalifikowana nazwa domeny (FQDN). Na przykład jeśli nazwę FQDN domeny fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **Nazwa użytkownika**                        | Określa nazwę logowania konta użytkownika BHOLD podstawowe usługi.                                                                                                                                                              | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                        | Określa hasło konta użytkownika usługi.                                                                                                                                                                           | Zapisz hasło w tym miejscu: **ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Ustawienia usługi FIM

| **Element**            | **Opis**                                                                                                                                                                                                                               | **Wartość**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **User**            | Określa nazwę logowania konta z uprawnieniami administratora dla programu FIM. Firma Microsoft zaleca nieużywanie na koncie skojarzonym z użytkownika root w BHOLD Core (domyślnie konto użyte do zainstalowania pakietu BHOLD Core). | Napisz tutaj nazwę konta użytkownika:                                                                   |
| **Hasło**        | Określa hasło konta administratora programu FIM.                                                                                                                                                                                 | Zapisz hasło w tym miejscu: **ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji. |
| **Baza danych programu FIM**    | Określa nazwę bazy danych usługi FIM Service.                                                                                                                                                                                               | Usługę programu FIM                                                                                          |
| **Port IP witryny sieci Web** | Określa nazwę lub adres IP serwera portalu programu FIM oraz port witryny sieci Web.                                                                                                                                                               | Napisz nazwę serwera lub adres i port w tym miejscu:                                                     |

### <a name="bhold-core-connection"></a>Połączenie podstawowe pakietu BHOLD

| **Element**               | **Opis**                                                                                                                                                                                                                                                                                                                                                                               | **Wartość**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domeny**             | Określa nazwę domeny konta określonego w **użytkownika**poniżej. Określ domenę w formacie (short) NetBIOS.                                                                                                                                                                                                                                                                   | Zapis nazwę konta użytkownika domeny w tym miejscu?                                                            |
| **User**               | Nazwa logowania konta **użytkownika BHOLD, że kierownik** wszystkich użytkowników i ról i ma uprawnienia do łączenie i rozłączanie ról użytkownika. Firma Microsoft zaleca nieużywanie na koncie skojarzonym z użytkownika root w BHOLD Core (domyślnie konto użyte do zainstalowania pakietu BHOLD Core). To konto może być tego samego konta używane do łączenia się do programu FIM | Napisz tutaj nazwę konta użytkownika:                                                                   |
| **Hasło**           | Określa hasło konta użytkownika określonego w **użytkownika**.                                                                                                                                                                                                                                                                                                                             | Zapisz hasło w tym miejscu: **ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji. |
| **Adres IP/komputera** | Określa adres IP pakietu BHOLD podstawowego serwera witryny sieci Web. Nie należy używać nazwy serwera.                                                                                                                                                                                                                                                                                                        | Wpisz tutaj adres IP:                                                                          |
| **Numer portu**        | Określa numer portu witryny sieci Web pakietu BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Zapisz tutaj numer portu:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Instalacja pakietu BHOLD FIM Integration

Aby zainstalować moduł BHOLD FIM integracji, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł integracji usługi FIM pakietu BHOLD na:

- BholdFIMIntegration<em>\<wersji\></em>\_Release.msi

Zastąp *\<wersji\>* numerem wersji, wersji pakietu BHOLD FIM integracji, którym ją instalujesz.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

![Uruchamianie Instalatora msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Zadań poinstalacyjnych

Po zainstalowaniu pakietu BHOLD FIM integracji, należy skonfigurować Microsoft SharePoint do nadaj uprawnienia właściciela witryny kontu usługi BHOLD. Jeśli portalu programu FIM jest skonfigurowany do używania protokołu Secure Sockets Layer (SSL), zabezpieczeń, należy zmodyfikować pliki, które zawierają odwołania do adresów stron BHOLD dodany do portalu programu FIM.

### <a name="configuring-sharepoint"></a>Konfigurowanie programu SharePoint

Działał prawidłowo, BHOLD FIM Integration wymaga pakietu BHOLD konto usługi musi mieć uprawnienia członek witryny Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Aby udzielić uprawnień elementu członkowskiego witryny do konta usługi pakietu BHOLD

1.  Zaloguj się do serwera z uruchomioną BHOLD FIM integracji z uprawnieniami administratora.

2.  Kliknij przycisk **Start**, a następnie kliknij przycisk **Internet Exporer**.

3.  Na pasku adresu wpisz <https://localhost> Jeśli program SharePoint jest skonfigurowany do używania protokołu SSL, zabezpieczeń, w przeciwnym razie wpisz <http://localhost>.

4.  W lewej części **witryny zespołu** kliknij **osoby i grupy**.

5.  W obszarze **grup** kliknij **członkom zespołu**i w pasku narzędzi w środkowym okienku kliknij **New**, a następnie kliknij przycisk **Add Users**.

6.  Na **Add Users: witryny zespołu** strony w **użytkowników/grup**, wpisz BHOLDApplicationGroup, a następnie kliknij przycisk Sprawdź nazwy w obszarze **użytkowników/grup** pole. Nazwa grupy powinna być rozwiązywana na zawierają nazwę domeny.

7.  Kliknij przycisk **bezpośrednio nadać użytkownikom uprawnienia**, wybierz opcję **Pełna kontrola — ma pełną kontrolę**, a następnie kliknij przycisk **OK**.

8.  Sprawdź, czy BHOLDApplicationGroup znajduje się w **uprawnienia: witryny zespołu**, a następnie zamknij program Internet Explorer.

![Uruchamianie Instalatora msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Konfigurowanie pakietu BHOLD do obsługi protokołu SSL

Jeśli portalu programu FIM jest skonfigurowany do używania protokołu SSL, zabezpieczeń, należy zmodyfikować plików na serwerze programu FIM, aby łącza do stron BHOLD zostanie otwarty. Pliki znajdują się w następującym folderze: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

W poniższej tabeli wymieniono pliki i oryginalnego i zmiany wersji ciągi, które można edytować.

| **Plik**                  | **Oryginalny ciąg**                                                                                                                   | **Zmieniono parametry**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.HTML | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.HTML       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.HTML |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.HTML |
| Selfservice.aspx          | RoleExchangePoint = http: / /\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint.svc TransportMode = transportu | RoleExchangePoint = https: / /\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint / BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Gdzie:

-   *\<BHOLD_Server\>*  Określa nazwę serwera BHOLD jako znaleziony w oryginalnej wersji pliku

-   *\<MIM_Server\>*  Określa nazwę serwera programu FIM, tak jak w programie oryginalną wersję pliku

-   *\<BHOLD_Server_FQDN\>*  określa w pełni kwalifikowana nazwa domeny (FQDN) serwera pakietu BHOLD

-   *\<MIM_Port\>*  Określa numer portu serwera usługi programu FIM, tak jak w programie oryginalną wersję pliku

-   *\<MIM_Server_FQDN\>*  Określa nazwę FQDN serwera programu FIM

-   *\<MIM_SSL_Port\>*  Określa inny port do użycia przy użyciu protokołu SSL na serwerze programu FIM

### <a name="enable-approval-workflows-in-bhold-core"></a>Włącz przepływy pracy zatwierdzania w Core pakietu BHOLD

Gdy FIM a BHOLD są zintegrowane do samoobsługi, przepływy pracy dla zatwierdzeń są uruchamiane w usłudze FIM. Jest to podobne do modelu przepływu pracy dla żądań, które pochodzą z portalu programu FIM, na przykład gdy użytkownik przesyła żądanie do dołączenia do listy dystrybucyjnej. Istnieją podstawowe różnice między BHOLD roli przepływy i inne przepływy pracy, jednak hostowanej w usłudze FIM. W przypadku pakietu BHOLD parametry przepływu pracy, które określają, użytkowników, którzy muszą zatwierdzić żądanie roli pochodzą BHOLD zamiast przechowywania go w definicji przepływu pracy w bazie danych usługi FIM Service. Te parametry są przekazywane do usługi FIM Service przez BHOLD, przy pierwszym żądaniu skierowanym i przepływ pracy akcji komunikuje się wyniki z powrotem do podstawowej BHOLD.

BHOLD wybiera osobą zatwierdzającą dla żądania samoobsługi w jednym z trzech sposobów:

-   **Menedżer wierszy jako osoba zatwierdzająca: wybór opartej na rolach dla jednostki organizacyjnej (OrgUnit)** rola ma atrybut o nazwie roletype, który jest ustawiony na osoba zatwierdzająca lub schodów ruchomych, a jeśli ta rola jest połączony z co najmniej jeden użytkownik w kontekście OrgUnit, żąda od Użytkownicy w tym OrgUnit muszą zostać zatwierdzone przez jednego z użytkowników, które jest połączone z roli o roletype osoba zatwierdzająca lub schodów ruchomych.

-   **Menedżer wierszy jako osoba zatwierdzająca: opartych na atrybutach wybór OrgUnit** OrgUnit każdy może mieć jeden lub więcej atrybutów, które określają aliasy użytkowników, którzy mogą zatwierdzać przypisania roli dla innych użytkowników w OrgUnit. Te atrybuty są nazywane approver1, approver2 i tak dalej. Po użytkownik w OrgUnit żądania przypisania roli, BHOLD kieruje żądania (przy użyciu programu FIM) do użytkowników określonych przez atrybuty osoba zatwierdzająca OrgUnit. W przypadku OrgUnit nie ma żadnego z tych zestawów atrybutów, BHOLD sprawdza nadrzędnego OrgUnits do głównego OrgUnit.

-   **Menedżer ról jako osoba zatwierdzająca: opartych na atrybutach wybranej roli** rola może mieć jeden lub więcej atrybutów (również o nazwie approver1 itd.), które określają aliasy użytkowników, którzy mogą zatwierdzać przypisania roli. Gdy użytkownik zażąda do przypisania roli, która ma tych atrybutów osoba zatwierdzająca, wartość, BHOLD kieruje żądanie do użytkowników określonych przez atrybuty.

Jeśli osoba zatwierdzająca żądania rolą samoobsługi nie jest określony za pomocą jednej z tych metod, domyślnie BHOLD automatycznie przypisuje rolę bez konieczności zatwierdzania. Z tego powodu od razu po zainstalowaniu pakietu BHOLD FIM integracji, należy skonfigurować katalog główny OrgUnit z aliasem osobę zatwierdzającą, takich jak konto główne. Uniemożliwi to użytkownik przypadkowo zostanie im przyznany rolę przed można zaimplementować bardziej złożone zasady zatwierdzania.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Aby skonfigurować osobą zatwierdzającą dla głównego OrgUnit

1.  Zaloguj się na serwer podstawowy BHOLD jako administrator.

2.  Kliknij przycisk **Start**, a następnie kliknij przycisk **programu Internet Explorer**.

3.  Na pasku adresu programu Internet Explorer wpisz <http://localhost:5151/bhold/core>, a następnie naciśnij klawisz Enter.

4.  Na podstawowych BHOLD stronę główną, w obszarze **def atrybut**, kliknij przycisk **typy atrybutów**.

5.  Na **typ atrybutu** kliknij **Dodaj**.

6.  Na **Dodaj atrybut typ strony**w **tożsamości**, wpisz approver1, **— typ danych** kliknij **alfanumeryczne**, w **Maksymalna długość**, 255, wpisz **listę wartości**, kliknij przycisk **nie**w **angielski**, wpisz 1 osoby zatwierdzającej, kliknij przycisk **OK**, a następnie kliknij przycisk **gotowe**.

7.  W okienku po lewej stronie w obszarze **def atrybut** kliknij **zestawów typów atrybutów**.

8.  Na **zestawów typów atrybutów** kliknij **Dodaj**.

9.  Na **Dodaj atrybut typu zestawu** stronie **opis**wpisz OrgUnit atrybutów, a następnie kliknij przycisk **OK**.

10. Na **atrybuty OrgUnit** rozwiń **typy atrybutów**, a następnie kliknij przycisk **Modyfikuj**.

11. W **typ atrybutu** kliknij **approver1**, kliknij przycisk **Dodaj**, a następnie kliknij przycisk **gotowe**.

12. W okienku po lewej stronie kliknij **typy obiektów**.

13. Na **typy obiektów** kliknij **OrgUnit**.

14. Na **obiektu typu/OrgUnit** rozwiń **zestawów typów atrybutów**, a następnie kliknij przycisk **Modyfikuj**.

15. Na **Link atrybutu typu zestawu/OrgUnit** strony w **kolejności**, 10, wpisz **atrybutu typu zestawu** kliknij **atrybuty OrgUnit**, Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **gotowe**.

16. W okienku po lewej stronie w obszarze **modelu**, kliknij przycisk **jednostek organizacyjnych**.

17. Na **jednostek organizacyjnych** kliknij **głównego**.

18. Na **jednostkę organizacyjną/root** kliknij **Modyfikuj**.

19. Na **zmodyfikować jednostki organizacyjnej atrybuty/root** stronie **osoba zatwierdzająca**, wpisz nazwę domeny i nazwę użytkownika, który spowoduje zatwierdzenie żądania przypisania roli, w formacie  *\<domeny\>*\\*\<użytkownika\>*, gdzie *\<domeny\>* jest Nazwa domeny NetBIOS (krótki) i *\<użytkownika\>* jest nazwą logowania użytkownika.
20. Kliknij przycisk **OK**.

> [!IMPORTANT]
> Domena i nazwa użytkownika musi być zgodna domyślny alias użytkownika w bazie danych BHOLD Core.

Zamiast określania osobą zatwierdzającą dla jednostek organizacyjnych możesz określić osobą zatwierdzającą dla proponowanych role w bazie danych BHOLD Core. Aby to zrobić, Utwórz atrybut approver1, dodaj go do atrybutu poziomego złożenia skojarzonej z typem obiektu roli, a następnie zmodyfikuj każdej proponowane roli, aby określić osoby zatwierdzającej.

Aby zapewnić lepsze zabezpieczenia przepływu pracy, oprócz osób zatwierdzających, należy wyznaczyć trybów dodatkowego zatwierdzenia i użytkowników przez tworzenie i wypełnianie następujące atrybuty OrgUnits i ról:

- schodów ruchomych<em>\<n\></em>

- właściciel<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- powiadomienie<em>\<n\></em>

gdzie *\<n\>* wskazuje opcjonalne sufiksu liczbowego, aby zapewnić wiele atrybutów tego samego typu.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Sprawdź skonfigurowane w usłudze FIM przepływy pracy zatwierdzania

Instalacja pakietu BHOLD FIM integracji tworzy zestawy, definicje przepływów pracy i reguły zasad zarządzania (MPR) do usługi FIM Service. Jeśli dostosowano miał wdrożenia programu FIM, aby zmienić zestawów administratorów lub grupy użytkowników, którzy mogą wysyłać żądania, należy upewnić się, czy reguły MPR odwołują się do zestawów właściwy użytkownik.

> [!NOTE]
> Zanim użytkownicy portalu programu FIM, można użyć funkcji samoobsługi udostępniane przez BHOLD, należy zsynchronizować konta użytkowników do bazy danych BHOLD FIM Synchronization Service. W szczególności musi istnieć rekord użytkownika w bazie danych BHOLD podstawowych i w bazie danych usługi FIM Service dla każdego użytkownika, który zgłosić wniosek samoobsługi lub została ona określona jako osoba zatwierdzająca lub schodów ruchomych dla żądań samoobsługi.

## <a name="next-steps"></a>Kolejne kroki

- Aby uzyskać informacje o instalowaniu portalu programu FIM i inne funkcje programu FIM, zobacz [planowania i architektury](https://technet.microsoft.com/library/ee808044.aspx) w bibliotece technicznej Forefront firmy Microsoft.
- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
