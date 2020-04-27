---
title: PAKIETU BHOLD z instalacją integracji z programem FIM/MIM | Microsoft Docs
description: Moduł integracji pakietu BHOLD Dodaj Samoobsługowe zarządzanie rolami do programu MIM i usługi FIM
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3005e06606ec4b3b6854003213c712770376b35d
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042206"
---
# <a name="bhold-fimmim-integration-installation"></a>PAKIETU BHOLD z instalacją integracji z programem FIM/MIM

Moduł integracji programu pakietu BHOLD FIM dodaje funkcję samoobsługowego zarządzania rolami do programu Microsof Identity Manager, dzięki czemu użytkownicy mogą żądać dodatkowych ról i wymuszać, kto może podejmować te role. Moduł integracji programu pakietu BHOLD FIM rozszerza Portal programu FIM, aby ułatwić zarządzanie rolami użytkowników w ramach ogólnej administracji programu FIM. W tym temacie opisano, jak należy skonfigurować infrastrukturę sieciową, aby umożliwić instalowanie i Używanie modułu integracji programu pakietu BHOLD FIM. Opisano w nim również, jak zainstalować moduł integracji i konfigurację programu pakietu BHOLD FIM, który jest wymagany po zainstalowaniu modułu integracji programu pakietu BHOLD FIM.

## <a name="bhold-fim-integration-installation-requirements"></a>Wymagania dotyczące instalacji integracji z programem FIM pakietu BHOLD

Moduł integracji programu pakietu BHOLD FIM rozszerza Portal programu FIM i usługę FIM, aby umożliwić użytkownikom zarządzanie swoimi rolami w portalu programu FIM. Z tego powodu bardzo ważne jest, aby przed zainstalowaniem modułu integracji programu FIM pakietu BHOLD pakietu BHOLD rdzeń i wymagane funkcje programu FIM.
Poniżej wymieniono składniki oprogramowania, które muszą znajdować się na komputerze, aby można było zainstalować moduł integracji programu pakietu BHOLD FIM:

- Portal i usługa Microsoft Identity Manager 2016
- Microsoft Silverlight 3 lub nowszy
- Internetowe usługi informacyjne i ASP.NET
- Microsoft Silverlight Tools

Ponadto moduły łącznika zarządzania usługami pakietu BHOLD Core i Access muszą być już wdrożone na serwerze w środowisku, a program FIM musi być skonfigurowany przy użyciu co najmniej jednego agenta zarządzania pakietu BHOLD. Aby uzyskać informacje na temat instalowania i konfigurowania modułu pakietu BHOLD Core, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Aby uzyskać informacje o instalowaniu i używaniu modułu łącznika zarządzania dostępem, zobacz temat [Instalacja łącznika zarządzania dostępem](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) i [Przewodnik po laboratorium testowym: Łącznik zarządzania dostępem pakietu BHOLD](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> Nazwa bazy danych usługi FIM musi być usługi FIMService. PAKIETU BHOLD integracja programu FIM nie powiedzie się, jeśli program FIM nie zostanie zainstalowany z domyślną nazwą bazy danych usługi FIM.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem instalacji modułu integracji programu pakietu BHOLD FIM należy utworzyć katalog pakietu BHOLD w katalogu głównym dysku C: Disk (C:\BHOLD).

Ponadto należy przygotować się do podania informacji wymaganych przez Kreatora instalacji integracji usługi FIM pakietu BHOLD do ukończenia instalacji. Poniższy arkusz pomoże Ci zarejestrować te informacje, dzięki czemu będzie można je w razie potrzeby udostępnić.

### <a name="bholdfim-account-settings"></a>Ustawienia konta BHOLDFim

| **Element**                            | **Opis**                                                                                                                                                                                                               | **Wartościami**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Używanie dostawcy zabezpieczeń w domenie** | Po wybraniu określa, że zabezpieczenia Active Directory Domain Services będą kontrolować dostęp do pakietu BHOLD rdzeń.                                                                                                                    | zaznacz pole wyboru. **Ważne:** Instalacja nie powiedzie się, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domain**                          | Określa domenę zawierającą **konto usługi** , które zostało utworzone podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest ona niepoprawna. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (krótkiej), a nie w pełni kwalifikowanej nazwy domeny (FQDN). Na przykład, jeśli nazwa FQDN domeny to fabrikam.com, określ nazwę domeny jako FABRIKAM. |
| **Uż**                        | Określa nazwę logowania konta użytkownika usługi pakietu BHOLD Core.                                                                                                                                                              | W tym miejscu wpisz nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                        | Określa hasło konta użytkownika usługi.                                                                                                                                                                           | Napisz hasło tutaj: **Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Ustawienia usługi FIM

| **Element**            | **Opis**                                                                                                                                                                                                                               | **Wartościami**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Użytkownik**            | Określa nazwę logowania konta z uprawnieniami administratora dla programu FIM. Firma Microsoft zdecydowanie zaleca, aby nie używać konta skojarzonego z użytkownikiem głównym w pakietu BHOLD Core (domyślnie konto używane do zainstalowania rdzeń pakietu BHOLD). | W tym miejscu wpisz nazwę konta użytkownika:                                                                   |
| **Hasło**        | Określa hasło konta użytkownika administratora programu FIM.                                                                                                                                                                                 | Napisz hasło tutaj: **Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji. |
| **Baza danych programu FIM**    | Określa nazwę bazy danych usługi FIM.                                                                                                                                                                                               | Usługi FIMService                                                                                          |
| **Adres IP/port witryny sieci Web** | Określa nazwę lub adres IP serwera portalu programu FIM oraz port witryny sieci Web.                                                                                                                                                               | W tym miejscu wpisz nazwę lub adres i port serwera:                                                     |

### <a name="bhold-core-connection"></a>PAKIETU BHOLD podstawowe połączenie

| **Element**               | **Opis**                                                                                                                                                                                                                                                                                                                                                                               | **Wartościami**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domain**             | Określa nazwę domeny konta określonego w polu **użytkownik**, poniżej. Określ domenę w formacie NetBIOS (krótki).                                                                                                                                                                                                                                                                   | W tym miejscu wpisz nazwę domeny konta użytkownika?                                                            |
| **Użytkownik**               | Określa nazwę logowania konta **użytkownika pakietu BHOLD, który jest nadzorcą** wszystkich użytkowników i ról i ma uprawnienia do łączenia i rozłączania ról użytkownika. Firma Microsoft zdecydowanie zaleca, aby nie używać konta skojarzonego z użytkownikiem głównym w pakietu BHOLD Core (domyślnie konto używane do zainstalowania rdzeń pakietu BHOLD). To konto może być tym samym kontem, które jest używane do nawiązywania połączenia z usługą FIM | W tym miejscu wpisz nazwę konta użytkownika:                                                                   |
| **Hasło**           | Określa hasło konta użytkownika określonego w polu **użytkownik**.                                                                                                                                                                                                                                                                                                                             | Napisz hasło tutaj: **Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji. |
| **Adres IP/komputera** | Określa adres IP serwera witryny sieci Web pakietu BHOLD Core. Nie należy używać nazwy serwera.                                                                                                                                                                                                                                                                                                        | Napisz tutaj adres IP:                                                                          |
| **Numer portu**        | Określa numer portu witryny sieci Web pakietu BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Tutaj wpisz numer portu:                                                                         |

## <a name="bhold-fim-integration-setup"></a>PAKIETU BHOLD — konfiguracja integracji programu FIM

Aby zainstalować moduł integracji programu pakietu BHOLD FIM, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, na którym ma zostać zainstalowany moduł integracji programu pakietu BHOLD FIM:

- BholdFIMIntegration<em>\<wersja\></em>\_pliku MSI

Zastąp * \<wersję\> * numerem wersji programu pakietu BHOLD wersja integracji z programem FIM, którą instalujesz.

Aby uruchomić plik programu jako administrator, kliknij plik prawym przyciskiem myszy, a następnie kliknij polecenie **Uruchom jako administrator**.

![uruchamianie pliku MSI](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Zadania poinstalacyjne

Po zainstalowaniu integracji z usługą pakietu BHOLD FIM należy skonfigurować program Microsoft SharePoint, aby przyznać uprawnienia właściciela lokacji konta usługi pakietu BHOLD. Ponadto, jeśli Portal programu FIM jest skonfigurowany do korzystania z zabezpieczeń SSL (SSL), należy zmodyfikować pliki, które zawierają odwołania do adresów stron pakietu BHOLD dodanych do portalu programu FIM.

### <a name="configuring-sharepoint"></a>Konfigurowanie programu SharePoint

Aby zapewnić prawidłowe działanie, integracja z usługą FIM pakietu BHOLD wymaga, aby konto usługi pakietu BHOLD miało uprawnienia do elementu członkowskiego lokacji w programie Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Aby przyznać uprawnienia elementu członkowskiego do konta usługi pakietu BHOLD

1.  Zaloguj się do serwera z uruchomioną integracją programu pakietu BHOLD FIM z uprawnieniami administratora.

2.  Kliknij przycisk **Start**, a następnie kliknij pozycję **Internet Exporer**.

3.  Na pasku adresu wpisz <https://localhost> , czy program SharePoint jest skonfigurowany do korzystania z zabezpieczeń SSL, w <http://localhost>przeciwnym razie.

4.  Po lewej stronie strony **zespołu** kliknij pozycję **osoby i grupy**.

5.  W obszarze **grupy** kliknij pozycję **Członkowie witryny zespołu**, a następnie w środkowym okienku Pasek narzędzi kliknij pozycję **Nowy**, a następnie kliknij pozycję **Dodaj użytkowników**.

6.  Na stronie **Dodawanie użytkowników: Witryna zespołu** w obszarze **Użytkownicy/grupy**wpisz BHOLDApplicationGroup, a następnie kliknij przycisk Sprawdź nazwy w polu **Użytkownicy/grupy** . Nazwa grupy powinna zostać rozpoznana w celu uwzględnienia nazwy domeny.

7.  Kliknij pozycję **Udziel użytkownikom uprawnień bezpośrednio**, wybierz pozycję **pełna kontrola — Pełna kontrola**, a następnie kliknij przycisk **OK**.

8.  Sprawdź, czy BHOLDApplicationGroup jest wymieniony w obszarze **uprawnienia: Witryna zespołu**, a następnie zamknij program Internet Explorer.

![uruchamianie pliku MSI](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Konfigurowanie pakietu BHOLD do obsługi protokołu SSL

Jeśli portal programu FIM jest skonfigurowany do korzystania z zabezpieczeń SSL, należy zmodyfikować pliki na serwerze programu FIM, aby otworzyć linki do stron pakietu BHOLD. Pliki znajdują się w następującym folderze: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

W poniższej tabeli wymieniono pliki oraz oryginalne i zmienione wersje ciągów, które mają być edytowane.

| **Plik**                  | **Oryginalny ciąg**                                                                                                                   | **Zmieniony ciąg**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analiza. aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns. aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | 
| AttestationMain. aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx? hideMenu = 1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx? hideMenu = 1 |
| Raportowanie. aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Samoobsługowy. aspx          | RoleExchangePoint = http://\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. svc, transportmode = transport | RoleExchangePoint = https://\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. svc, transportmode = transport |

Gdzie:

-   BHOLD_Server określa nazwę serwera pakietu BHOLD, która została znaleziona w oryginalnej wersji pliku * \<\> *

-   MIM_Server określa nazwę serwera programu FIM, która została znaleziona w oryginalnej wersji pliku * \<\> *

-   BHOLD_Server_FQDN określa w pełni kwalifikowaną nazwę domeny (FQDN) serwera pakietu BHOLD * \<\> *

-   MIM_Port określa numer portu serwera programu FIM, który znajduje się w oryginalnej wersji pliku * \<\> *

-   MIM_Server_FQDN określa nazwę FQDN serwera programu FIM * \<\> *

-   MIM_SSL_Port określa inny port do użycia z protokołem SSL na serwerze programu FIM * \<\> *

### <a name="enable-approval-workflows-in-bhold-core"></a>Włącz przepływy pracy zatwierdzania w pakietu BHOLD rdzeń

Gdy usługa FIM i pakietu BHOLD są zintegrowane do samoobsługi, przepływy pracy dla zatwierdzeń są uruchamiane w usłudze FIM. Jest to podobne do modelu przepływu pracy dla żądań, które pochodzą z portalu FIM, na przykład gdy użytkownik przesyła żądanie dołączenia do listy dystrybucyjnej. Istnieją jednak kluczowe różnice między przepływami pracy roli pakietu BHOLD i innymi przepływami pracy obsługiwanymi w usłudze FIM. W przypadku pakietu BHOLD parametry przepływu pracy, które określają, którzy użytkownicy muszą zatwierdzić żądanie roli, pochodzą z pakietu BHOLD, a nie są przechowywane w definicjach przepływu pracy w bazie danych usługi FIM. Parametry te są dostarczane do usługi FIM przez pakietu BHOLD, gdy następuje pierwsze żądanie, a przepływ pracy w ramach akcji komunikuje wyniki z powrotem do pakietu BHOLD Core.

PAKIETU BHOLD wybiera osobę zatwierdzającą dla żądania samoobsługi na jeden z trzech sposobów:

-   **Menedżer linii jako osoba zatwierdzająca: wybór oparty na rolach dla jednostki organizacyjnej (OrgUnit)** Jeśli rola ma atrybut o nazwie roleType, który jest ustawiony na osobę zatwierdzającą lub eskalację, a jeśli ta rola jest połączona z co najmniej jednym użytkownikiem w kontekście OrgUnit, żądania od użytkowników w ramach tego OrgUnit muszą zostać zatwierdzone przez jednego z użytkowników połączonych z rolą przy użyciu elementu role osoby zatwierdzającej lub eskalacji.

-   **Menedżer linii jako osoba zatwierdzająca: wybór oparty na atrybutach dla elementu OrgUnit** Każdy OrgUnit może mieć jeden lub więcej atrybutów, które określają aliasy użytkowników, którzy mogą zatwierdzać przypisania ról dla innych użytkowników w OrgUnit. Te atrybuty mają nazwę approver1, approver2 i tak dalej. Gdy użytkownik w OrgUnit żąda przypisania roli, pakietu BHOLD kieruje żądanie (za pomocą programu FIM) do użytkowników określonych przez atrybuty osoby zatwierdzającej OrgUnit. Jeśli OrgUnit nie ma żadnego z tych zestawów atrybutów, pakietu BHOLD sprawdza element nadrzędny OrgUnits do głównego OrgUnit.

-   **Menedżer ról jako osoba zatwierdzająca: wybór oparty na atrybutach dla roli** Rola może mieć jeden lub więcej atrybutów (również o nazwie approver1 itd.), które określają aliasy użytkowników, którzy mogą zatwierdzać przypisanie roli. Gdy użytkownik zażąda przypisania roli, która ma te atrybuty osoby zatwierdzającej, pakietu BHOLD kieruje żądanie do użytkowników określonych przez atrybuty.

Jeśli osoba zatwierdzająca dla żądania roli samoobsługi nie została określona przez jedną z tych metod, domyślnie pakietu BHOLD automatycznie przypisuje rolę bez konieczności zatwierdzania. Z tego powodu natychmiast po zainstalowaniu integracji z programem pakietu BHOLD FIM należy skonfigurować główny OrgUnit przy użyciu aliasu osoby zatwierdzającej, takiej jak konto główne. Uniemożliwi to użytkownikowi niezamierzone uzyskanie roli przed zaimplementowaniem bardziej kompleksowych zasad zatwierdzania.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Aby skonfigurować osobę zatwierdzającą dla elementu głównego OrgUnit

1.  Zaloguj się do serwera pakietu BHOLD Core jako administrator.

2.  Kliknij przycisk **Start**, a następnie kliknij pozycję **Internet Explorer**.

3.  Na pasku adresu programu Internet Explorer wpisz <http://localhost:5151/bhold/core>, a następnie naciśnij klawisz ENTER.

4.  Na stronie głównej pakietu BHOLD Core w obszarze **atrybut def**kliknij pozycję **typy atrybutów**.

5.  Na stronie **Typ atrybutu** kliknij przycisk **Dodaj**.

6.  Na **stronie Dodawanie typu atrybutu**w polu **tożsamość**wpisz approver1, na liście **Typ danych** kliknij pozycję **alfanumeryczne**, w polu **długość maksymalna**wpisz 255, w polu **Lista wartości**kliknij przycisk **nie**, w **języku angielskim**wpisz tekst zatwierdzającego 1, kliknij przycisk **OK**, a następnie kliknij przycisk **gotowe**.

7.  W lewym okienku w obszarze **atrybut def** kliknij pozycję **zestawy typów atrybutów**.

8.  Na stronie **zestawy typów atrybutów** kliknij przycisk **Dodaj**.

9.  Na stronie **Dodawanie zestawu typów atrybutów** w polu **Opis**wpisz atrybuty OrgUnit, a następnie kliknij przycisk **OK**.

10. Na stronie **atrybuty OrgUnit** rozwiń węzeł **typy atrybutów**, a następnie kliknij przycisk **Modyfikuj**.

11. Na liście **Typ atrybutu** kliknij pozycję **approver1**, kliknij pozycję **Dodaj**, a następnie kliknij pozycję **gotowe**.

12. W lewym okienku kliknij pozycję **typy obiektów**.

13. Na stronie **typy obiektów** kliknij pozycję **OrgUnit**.

14. Na stronie **Typ obiektu/OrgUnit** rozwiń węzeł **zestawy typów atrybutów**, a następnie kliknij przycisk **Modyfikuj**.

15. Na stronie Typ **atrybutu linku Ustaw/OrgUnit** , w **kolejności**wpisz 10, na liście **Typ atrybutu** kliknij pozycję **atrybuty OrgUnit**, kliknij przycisk **Dodaj**, a następnie kliknij przycisk **gotowe**.

16. W lewym okienku w obszarze **model**kliknij pozycję **jednostki organizacyjne**.

17. Na stronie **jednostki organizacyjne** kliknij pozycję **główny**.

18. Na stronie **jednostka organizacyjna/główna** kliknij przycisk **Modyfikuj**.

19. Na stronie **Modyfikuj atrybuty jednostki organizacyjnej/głównej** w obszarze **osoba zatwierdzająca**wpisz domenę i nazwę użytkownika, który będzie zatwierdzał żądania przypisywania ról, w formacie * \<\>**\<użytkownik\>*\\domeny, gdzie * \<domena\> * jest nazwą NetBIOS (krótka), a * \<użytkownik\> * jest nazwą logowania użytkownika.
20. Kliknij przycisk **OK**.

> [!IMPORTANT]
> Domena i nazwa użytkownika muszą być zgodne z domyślnym aliasem użytkownika w podstawowej bazie danych pakietu BHOLD.

Alternatywą dla określenia osoby zatwierdzającej dla jednostek organizacyjnych jest określenie osoby zatwierdzającej dla proponowanych ról w podstawowej bazie danych pakietu BHOLD. W tym celu należy utworzyć atrybut approver1, dodać go do atrybutu poziomego złożenia skojarzonego z typem obiektu roli, a następnie zmodyfikować każdą proponowaną rolę, aby określić osobę zatwierdzającą.

Aby zapewnić lepsze zabezpieczenia przepływu pracy, oprócz osób zatwierdzających, należy wyznaczyć dodatkowe tryby zatwierdzania i użytkowników, tworząc i wypełniając następujące atrybuty dla OrgUnits i ról:

- eskalacja<em>\<n\></em>

- Właściciel<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- powiadomienie<em>\<n\></em>

gdzie * \<n\> * wskazuje opcjonalny sufiks numeryczny, aby zapewnić wiele atrybutów tego samego typu.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Sprawdź, czy przepływy pracy zatwierdzania zostały skonfigurowane w usłudze FIM

PAKIETU BHOLD integracja z usługą FIM umożliwia tworzenie zestawów, definicji przepływu pracy i reguł zasad zarządzania (reguł MPR) w usłudze FIM. Jeśli dostosowano wdrożenie programu FIM w celu zmiany zestawów administratorów lub zestawów użytkowników, którzy mogą wykonywać żądania, należy się upewnić, że reguł MPR odwołuje się do poprawnych zestawów użytkownika.

> [!NOTE]
> Aby użytkownicy portalu FIM mogli korzystać z funkcji samoobsługowych udostępnianych przez usługę pakietu BHOLD, konta użytkowników muszą zostać zsynchronizowane z bazą danych pakietu BHOLD z usługi synchronizacji FIM. W szczególności musi istnieć rekord użytkownika w bazie danych pakietu BHOLD Core i w bazie danych usługi FIM dla każdego użytkownika, który może wykonać żądanie samoobsługowe lub określić jako osobę zatwierdzającą lub eskalację do obsługi żądań samoobsługi.

## <a name="next-steps"></a>Następne kroki

- Informacje o instalowaniu portalu FIM i innych funkcji programu FIM można znaleźć w temacie [Planning and Architecture](https://technet.microsoft.com/library/ee808044.aspx) in the Microsoft Forefront Technical Library.
- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
