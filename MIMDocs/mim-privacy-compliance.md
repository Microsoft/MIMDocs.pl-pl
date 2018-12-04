---
title: Obsługa danych programu Microsoft Identity Manager | Dokumentacja firmy Microsoft
description: Zrozumienie danych programu Microsoft Identity Manager, obsługi do identyfikowania i raporty dotyczące danych w środowisku, podjąć działania w danym system na podstawie funkcji operacyjnych i wymagań.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/02/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: f75eb69360852c9f629b60d4900638c8b51e068a
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825794"
---
# <a name="microsoft-identity-manager-data-handling"></a>Obsługa danych programu Microsoft Identity Manager 

W tym artykule przedstawiono wskazówki, w jaki sposób organizacje mogą decyzje, które można zastosować na wiele połączonych źródeł danych.  Można to osiągnąć za pomocą wyszukiwania, usuwanie, aktualizacja i operacji tworzenia raportów.  Przed podjęciem decyzji, daną metodę, usuwanie lub aktualizacja, ważne jest zrozumienie bieżącego projektu i konfiguracji systemu identity manager (MIM). 

Poniżej przedstawiono kilka scenariuszy klientów należy wziąć pod uwagę i odpowiedzieć na następujące pytania: 

- Jakie dane potrzebujesz możesz Zarządzanie tożsamościami pomagające w procesie biznesowym?
- Bieżące dane gdzie mają być przechowywane w programie MIM?
- Jak będzie używać tych danych w systemie?
- Możesz udostępniania tych danych za pomocą dowolnego sources(Exporting) danych partnerom zewnętrznym
- Co to jest autorytatywne źródło danych i przetwarzania go?
- Co będzie usługi przechowywania danych i usuwania danych w planie w miejscu?
- Czy określono technologii, potrzebne do przetwarzania danych i zarządzanie nimi?

Aby ułatwić zrozumienie bieżącego środowiska programu MIM może korzystać z następującego narzędzia do dokumentu środowiska programu MIM lub odroczone do dokumentów projektu wdrożenia.
- [Zezwala na Dokumentator programu MIM — można wyeksportować bieżącej konfiguracji](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Wyszukiwanie i identyfikowanie danych osobowych
Wyszukiwanie danych w programie MIM będzie zależna od konfiguracji i instalacji. Większości środowisk są połączone ze sobą, ale w celu uściślenia możemy Przerwano ich przez składnik wysokiego poziomu.

### <a name="synchronization-service"></a>Usługa synchronizacji

Wszystkie dane w programie MIM, które odnoszą się do użytkowników jest tworzony na podstawie usługi Active Directory (AD) i źródłami danych działu KADR. Podczas wyszukiwania danych osobowych, pierwsze miejsce, należy rozważyć, wyszukiwanie jest AD lub połączonych źródeł danych. 

Jeśli nie masz pewności źródło własności możesz śledzić tego użytkownika z poziomu konsoli Menedżera usługi synchronizacji programu MIM, kliknij na pasku wyszukiwania Metaverse, aby wyświetlić identyfikację danych osobowych, które są przechowywane w bazie danych. Użytkownicy mogą wyszukiwać dla określonego użytkownika lub atrybutu.

- Do wykonania przeglądu lub wyszukiwania danych obiekty użytkownika
    - Otwórz klienta usługi synchronizacji
        - Za pomocą obiektu metaverse designer umożliwia można zobaczyć, że przepływ atrybutu importuje i pierwszeństwo.
![Program mim — ochrona prywatności — compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Korzystania z wyszukiwania metaverse umożliwia wyszukiwanie na obiektów i atrybutów w bazie danych ![mim — ochrona prywatności — compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Po znalezieniu obiektu, kliknięcie obiektu spowoduje otwarcie strony profilu użytkownika. Szczegóły obiektu zapewniają kompleksową szczegółowe informacje o obiekcie, jego atrybuty ostatniej modyfikacji i źródło własności i powiązane połączonego źródła danych pochodzące z poniższym przykładzie konfiguracja agenta zarządzania.

![Program mim — ochrona prywatności — zgodność z przepisami. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Usługa i Portal / PAM
Jeśli masz wystąpienie usługi i portalu PAM zainstalowany lub jest możliwość wyszukiwania użytkowników jest ważne. 

Po zainstalowaniu portalu, możesz wyszukać dowolnego atrybutu lub zapytanie dla danego użytkownika przy użyciu interfejsu użytkownika.

Jeśli masz tylko serwer usługi (bez interfejsu użytkownika portalu) zainstalowane można uruchomić składni wyszukiwania, w oparciu o [FIMAutomation PSSnapin], znaleziono przykład [tutaj](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

Lub skorzystać z funkcji PAM można użyć tej samej składni powyżej [modułu MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specjalnie polecenie cmdlet get-użytkownik PAM, aby wyszukać użytkownika w ramach środowiska PAM.

Inne opcje raportowania, aby wyszukać na dostępnych danych znajduje się w portalu i usługi.
- [Raportowanie hybrydowe](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Raportowanie za pomocą programu SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Usługa Bhold Core ma interfejs użytkownika, który umożliwia wyszukiwanie dla użytkownika lub atrybutów. 

![Wyszukiwanie pakietu bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Jeśli planowana jest synchronizacja BHOLD z [dostęp do łącznika zarządzania](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) usługi synchronizacji można wyświetlić obiekty połączonego użytkownika i atrybutów wysyłania pakietu BHOLD Core.

Można również załadować moduł raportowania pakietu BHOLD.

- [Raportowania pakietu BHOLD](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Zarządzanie certyfikatami
Certyfikat zarządzania usługi search jest wbudowana w Interfejsie użytkownika. Administrator spowoduje uruchomienie i wybierz pozycję "Znajdź użytkowników i widoku lub zarządzanie informacjami"  

![Wyszukiwanie cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Eksportowanie danych osobowych
Ponieważ dane powiązane z jednostkami w programie MIM jest relacją pochodną z wielu źródeł, większość danych są przechowywane w bazie danych usługi synchronizacji. Z tego powodu należy wyeksportować dane dotyczące obiektu z synchronizacji programu MIM, lub można określić właścicielem tych danych.

### <a name="synchronization-service"></a>Usługa synchronizacji
Usługi synchronizacji eksportowania danych po prostu wybierz dane z wyszukiwania interfejsu użytkownika i skopiuj i Wklej do woluminu csv lub preferowanym formacie. Jest innym sposobem, aby wyeksportować te dane do tworzenia opartych na plikach agenta zarządzania można usunąć bieżące dane potrzebne informacje użytkownik z flagą zainteresowania. Można znaleźć przykładzie użycia oparte na plikach MA [tutaj](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Usługa i Portal / PAM
Usługa i portal wraz z PAM, możesz wyeksportować te dane, uruchom składni wyszukiwania, w oparciu o [FIMAutomation PSSnapin], znaleziono przykład [tutaj](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) i przekazać go do [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

Lub skorzystać z funkcji PAM można użyć tej samej składni powyżej [modułu MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specjalnie get-użytkownik PAM wyszukać użytkownika w środowisku usługi PAM i przekazać go do formatu csv.

- [Przykład zapytania usługi MIM przy użyciu programu PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Dane pakietu Bhold można wyeksportować za pomocą pakietu bhold raportowania modułu do preferowanym przez Ciebie formacie.

### <a name="certificate-management"></a>Zarządzanie certyfikatami
Certyfikat zarządzania danych dotyczących danych osobowych jest podłączony do usługi active directory. Administrator może wyeksportować te dane przy użyciu programu powershell usługi Active Directory.

## <a name="updating-personal-data"></a>Aktualizowanie danych osobowych

Dane osobowe dotyczące użytkowników lub obiektów w rozwiązaniach programu MIM jest zazwyczaj uzyskiwane z obiektu użytkownika w Twojej organizacji połączonych źródeł danych. Ponieważ wszelkie zmiany wprowadzone do profilu użytkownika w źródle HR lub innego systemu autorytatywne rekordzie, takich jak usługi AD następnie są odzwierciedlane w usługi synchronizacji programu MIM.

### <a name="synchronization-service"></a>Usługa synchronizacji

W celu wykonywania operacji zarządzania, Administratorzy muszą być częścią operacji synchronizacji lub administratora zdefiniowane [tutaj](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

Aktualizowanie danych odbywa się przez Definiowanie reguły ze źródłowego urzędu. Konsola zarządzania pomaga zidentyfikować źródło uprawnień do aktualizowania go w miejscu źródłowym. Innym rozwiązaniem jest tworzenie reguły synchronizacji lub rozszerzenie reguły do kontrolowania, aktualizowanie danych, jeśli źródła, takich jak dane Kadrowe nadal musi pozostać. Są to opcje avialible obsługiwane.

Aby uzyskać więcej informacji dotyczących różnych sposobów, aby zaktualizować atrybut poniżej. 

- [Za pomocą rozszerzeń reguł](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Informacje na temat synchronizacji danych z systemami zewnętrznymi](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Usługa i Portal / PAM

Usługa i Portal, aby dołączyć dane PAM może zostać zaktualizowana przy użyciu poleceń cmdlet FIMAutomation lub PAM. W przypadku portalu można również bezpośrednio zaktualizować, wyszukując i zmodyfikuj obiekt. Jedno, należy pamiętać, jak i w zależności od konfiguracji zaktualizowania z poziomu portalu nie oznacza, że będzie w dalszym ciągu. Źródło własności jest stopniu zależą od konfiguracji ogólnej.

### <a name="bhold"></a>BHOLD

Użytkownicy mogą bezpośrednio aktualizowane przy użyciu interfejsu użytkownika BHOLD Core lub dostęp do łącznika zarządzania.

### <a name="certificate-management"></a>Zarządzanie certyfikatami

Użytkownicy w usłudze zarządzania certyfikatów są odbicia z usługi active directory. Aby zaktualizować Użyj usługi Active Directory, aby zmienić szczegóły obiektu.

## <a name="deleting-personal-data"></a>Usuwanie danych osobowych

>[!Note] 
> Ten artykuł zawiera wskazówki dotyczące sposobów usuwania danych osobowych z programu Microsoft Identity Manager i mogą służyć do obsługi zobowiązania w RODO. Jeśli szukasz ogólnych informacji na temat rozporządzenia RODO, zobacz [sekcję RODO w Portalu zaufania usług](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Dane w usłudze MIM jest synchronizowane i zawsze zaktualizowany z jego połączonego źródła danych. Gdy obiekt zostanie usunięty w lokalizacji docelowej, można obsługiwać danych obiektu w programie MIM na potrzeby badania zabezpieczeń. Usunięcie obiektu jest konfigurowana w ramach połączonych danych źródłowych reguły lub reguły extension(code) i/lub reguły usuwania obiektów.

### <a name="synchronization-service"></a>Usługa synchronizacji
Synchronizacja usługi tyle metody obsługi danych lub usuwania danych w zależności od procesów biznesowych. Aby lepiej zrozumieć, poniżej przedstawiono niektóre artykuły, aby lepiej zrozumieć opcje dotyczące usuwania i aktualizowania atrybutów: 

- [Informacje na temat anulowania aprowizacji](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Za pomocą rozszerzeń reguł](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Najlepsze rozwiązania w programie MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Usługa i Portal / PAM

Zalecane jest usługa i Portal zachowanie Konfiguracja przechowywania zasobów systemowych programu domyślne 30 dni. Informuje usługę, gdy zostaną usunięte, nie tylko dane żądania, ale również dla dowolnego obiektu, który ma zostać wyczyszczone z systemu. Proces odbywa się, wszystkie dane z połączonych z tym obiektem skasowanie obejmuje to wszystkie dane rejestracji samoobsługowego resetowania HASEŁ. To jest odtwarzany w powyższej konfiguracji usunięcia obiektu. Dysponujemy jednej tabeli zostały przechowujemy guid obiektów. Do redukowania łącznego rozmiaru tabeli w kompilacji wersją 4.4.1459 dodaliśmy znajduje się w procesie zwanym FIM_DeleteExpiredSystemObjectsJob szczegółowe informacje na temat tego procesu [tutaj](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![Program mim — ochrona prywatności — zgodność srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold, takich jak większość systemów połączenie z usługą synchronizacji można skonfigurować tak, można usunąć po obiektu źródłowego, takich jak HR jest usuwany. Jest ona konfigurowana na agencie zarządzania. i kontrolowane przez reguły usuwania obiektów, zgodnie z opisem w obszarze funkcje usługi synchronizacji.

Innym rozwiązaniem jest usunięcie obiektu użytkownika bezpośrednio z poziomu interfejsu użytkownika Core BHOLD. W zależności od konfiguracji to prawidłowo ale należy pamiętać, inicjowania obsługi logiki można ponownie utworzyć tego użytkownika, w przeciwnym razie usunięte w miejscu źródłowym.
![Program mim — ochrona prywatności — zgodność bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Zarządzanie certyfikatami
Aby usunąć użytkownika z zarządzania certyfikatami w usłudze, Usuń użytkownika jest w usłudze active directory.

Zarządzanie certyfikatami w postaci, w jakiej będzie przechowywane tylko uid profilu z usług certyfikatów za pomocą sAMAccountName domeny. Po usunięciu użytkownika z usługi AD pamięci podręcznej użytkownika występuje tylko w przypadku certyfikatów normalnego zarejestrowanych. Nie zaleca się usunięcie wszystkiego w bazie danych, ponieważ może to spowodować szkody ogólnego działania środowiska.

## <a name="opt-out-of-telemetry"></a>Zrezygnować z telemetrii
Wcześniejsze kompilacje programu FIM/MIM używane do zbiera anonimowe dane telemetryczne dotyczące każdego wdrożenia i przekazuje te dane przy użyciu protokołu HTTPS do serwerów firmy Microsoft. Te dane został użyty przez firmę Microsoft do ulepszania przyszłych wersji programu FIM/MIM w przeszłości.

>[!Note] 
> W kolejnych wersjach 4.5.x.x lub większa dane zostaną wyłączone kolekcje.

Można wyłączyć danych kolekcji w poprzedniej wersji, uruchom Zmień tryb i usuń zaznaczenie opcji następujący wiersz:

![mim — ochrona prywatności — zgodności — programu ceip. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

Edytowanie rejestru lub ustaw wartość na 0: Manager\2010 tożsamości HKLM\SOFTWARE\Microsoft\Forefront CEIP (składnik)

![Program mim — ochrona prywatności — zgodność ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Następne kroki 
- [Powiązane SQL ochrony prywatności wskazówki](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [RODO części portal zaufania usługi](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 archiwum: Zwiększania obciążenia — Implementowanie Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
