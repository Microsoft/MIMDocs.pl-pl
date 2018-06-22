---
title: Obsługa danych programu Microsoft Identity Manager | Dokumentacja firmy Microsoft
description: Zrozumienie danych programu Microsoft Identity Manager obsługi do identyfikowania i raporty dotyczące danych w środowisku, podjąć działania w podanej system na podstawie funkcje operacyjne i wymagań.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldiwn
ms.date: 05/22/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6bcf9ab26ba38f3c6eefbdb315d4975320a597b9
ms.sourcegitcommit: 66db63fe2813130764e52381f4f9c8e549d77d39
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/22/2018
ms.locfileid: "34449701"
---
# <a name="microsoft-identity-manager-data-handling"></a>Obsługa danych programu Microsoft Identity Manager 

W tym artykule zawierają wskazówki dotyczące sposobu organizacji za pomocą wyszukiwania, Usuń, aktualizacji i raport operacji decyzji dotyczących organizacji będzie musiał rozwiązaniem, lub zaimplementuj na wiele połączonych źródeł danych. Przed podjęciem decyzji na użytkownika podejście, usuwania lub aktualizacji zrozumienia istniejących danych projektowych i konfiguracji systemu identity manager (MIM) jest bardzo istotne. Poniżej przedstawiono kilka scenariuszy klientów należy wziąć pod uwagę i Odpowiedz na następujące pytania: 

- Jakie dane należy możesz Zarządzanie tożsamościami ułatwiają proces biznesowy?
- Gdzie bieżących danych ma być przechowywany w programie MIM?
- Jak będzie używać tych danych w systemie?
- Są udostępnianie tych danych z żadnych sources(Exporting) danych partnerami zewnętrznymi
- Co to jest wiarygodne źródło danych i przetwarzania go?
- Co spowoduje Twoje przechowywanie danych i usunięcia danych planu?
- Czy zidentyfikowano technologii potrzebne do przetwarzania danych i zarządzać nimi?

Aby ułatwić zrozumienie bieżącego środowiska MIM może korzystać z następującego narzędzia dokumentu środowiska MIM lub odroczenie do dokumentów projektu wdrożenia.
- [Umożliwia Dokumentator MIM — można wyeksportować bieżącej konfiguracji](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Wyszukiwanie i zidentyfikowaniu danych osobowych
Wyszukiwanie danych w ramach MIM będzie zależna od instalacji i konfiguracji. Większości środowisk są połączone ze sobą, ale dla jasności możemy spowodowało przerwanie ich przez składnik wysokiego poziomu.

### <a name="synchronization-service"></a>Usługa synchronizacji

Wszystkie dane w programie MIM, które odnoszą się do użytkowników jest pobierany z usługi Active Directory (AD) i źródłami danych HR. Podczas wyszukiwania danych osobowych, pierwsze miejsce, należy rozważyć wyszukiwanie jest AD lub podłączone do źródła danych. 

Jeśli nie masz pewności źródłowego urzędu można śledzić tego użytkownika z konsoli programu MIM Synchronization Service Manager, kliknij na pasku wyszukiwania Metaverse, aby wyświetlić osobistych danych umożliwiających identyfikację użytkownika, który jest przechowywany w bazie danych. Użytkownicy mogą wyszukiwać określonego użytkownika lub atrybutu.

- Przejrzyj lub wyszukiwania danych obiektów użytkownika
    - Otwórz klienta usługi synchronizacji
        - Przy użyciu metaverse designer umożliwiają można zobaczyć, że przepływ atrybutów importuje i pierwszeństwo.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Za pomocą wyszukiwania metaverse umożliwia wyszukiwanie na obiektów i atrybutów w bazie danych ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Po znalezieniu obiekt, klikając obiekt spowoduje otwarcie strony profilu użytkownika. Szczegóły obiektu Podaj z kompleksowe szczegółowe informacje o obiekcie, jego atrybuty ostatnio modyfikowany ani źródła urzędu i powiązane połączonego źródła danych pochodzi z poniższym przykładzie konfiguracji agenta zarządzania.

![mim-privacy zgodności. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Usługa i Portal / PAM
Jeśli masz wystąpienie usługi i portalu PAM zainstalowany lub jest ważne jest można wyszukiwać użytkowników. 

Po zainstalowaniu aplikacji Portal umożliwia interfejsu użytkownika wyszukiwania na żadnych atrybutów ani zapytanie dla danego użytkownika.

Jeśli masz tylko serwer usługi (bez interfejsu użytkownika portalu) zainstalowany można uruchomić składni wyszukiwania, w oparciu o [FIMAutomation PSSnapin], znaleziono przykład [tutaj](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM można użyć składni powyżej lub skorzystać z [modułu MIMPAM](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specjalnie polecenia cmdlet get-użytkownik PAM aby wyszukać użytkownika w środowisku PAM.

Inne opcje raportowania, aby wyszukać dostępnych danych znajduje się w portalu i usługi.
- [Raportowanie hybrydowe](https://docs.microsoft.com/en-us/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Zgłoszenie SCSM](https://docs.microsoft.com/en-us/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Usługi podstawowej Bhold ma interfejsu użytkownika, który służy do wyszukiwania dla użytkownika lub atrybutów. 

![Wyszukiwanie bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Jeśli planowana jest synchronizacja BHOLD z [dostępu łącznika zarządzania](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-access-management-connector-install) usługi synchronizacji można wyświetlić podłączoną obiektów i atrybutów wysyłanie do BHOLD core.

Można także załadować moduł raportowania BHOLD.

- [Raportowanie BHOLD](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Zarządzanie certyfikatami
Certyfikat zarządzania usługi wyszukiwania jest wbudowana w Interfejsie użytkownika. Administrator będzie uruchamianie i wybierz "Znajdowanie użytkownika i widoku lub zarządzanie informacjami o ich"  

![Wyszukiwanie cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Eksportowanie danych osobowych
Ponieważ dane powiązane z jednostkami w programie MIM pochodzi z wielu źródeł, większość dane są przechowywane w bazie danych usługi synchronizacji. Z tego powodu należy wyeksportować dane dotyczące obiektu z synchronizacji programu MIM lub można określić właścicielem tych danych.

### <a name="synchronization-service"></a>Usługa synchronizacji
Usługi synchronizacji eksportowania danych po prostu wybierz dane z wyszukiwania interfejsu użytkownika i skopiuj i Wklej do woluminu csv lub preferowanym formatem. Inny sposób, aby wyeksportować te dane jest aby utworzyć agenta zarządzania na podstawie pliku można usunąć bieżące dane potrzebne o oflagowanego użytkownika zainteresowań. Exmaple użycia MA na podstawie plików można znaleźć [tutaj](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Usługa i Portal / PAM
Usługi i portalu oraz PAM można wyeksportować te dane, uruchom składni wyszukiwania, w oparciu o [FIMAutomation PSSnapin], znaleziono przykład [tutaj](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) i przekazać go do [csv](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM można użyć składni powyżej lub skorzystać z [modułu MIMPAM](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specjalnie get-użytkownik PAM aby wyszukać użytkownika w środowisku PAM i przekazać ją w potoku do udostępnionych woluminów klastra.

- [Przykładowe zapytanie usługi MIM przy użyciu programu PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Można wyeksportować danych Bhold przy użyciu bhold raportowania modułu do preferowany format.

### <a name="certificate-management"></a>Zarządzanie certyfikatami
Certyfikat zarządzania danych związanych z danych osobowych jest podłączony do usługi active directory. Administrator może wyeksportować te dane przy użyciu programu powershell usługi Active Directory.

## <a name="updating-personal-data"></a>Aktualizowanie danych osobowych

Danych osobowych dotyczących użytkowników lub obiektów w rozwiązaniach MIM zwykle jest pochodną obiektu użytkownika w Twojej organizacji połączonych źródeł danych. Ponieważ zmiany wprowadzone do profilu w źródle HR lub inny system autorytatywne rekordu, takie jak AD następnie są odzwierciedlane w usługi synchronizacji programu MIM.

### <a name="synchronization-service"></a>Usługa synchronizacji

W celu wykonywania operacji zarządzania, Administratorzy muszą być częścią operacje synchronizacji lub administrator zdefiniowane [tutaj](https://docs.microsoft.com/en-us/previous-versions/mim/jj590183(v%3dws.10)).

Aktualizowanie danych odbywa się, definiując reguły ze źródła urzędu. Konsola zarządzania pomaga zidentyfikować źródło uprawnień do aktualizowania go w źródle. Innym rozwiązaniem jest tworzenie reguły synchronizacji lub rozszerzenie reguły do kontrolowania aktualizowania danych, jeśli źródło, takich jak dane Kadrowe nadal musi pozostać. Są to opcje avialible obsługiwane.

Aby uzyskać więcej informacji na różne sposoby aktualizacji atrybutów zobacz poniżej. 

- [Za pomocą rozszerzenia Zasady](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Informacje na temat synchronizacji danych z systemami zewnętrznymi](https://docs.microsoft.com/en-us/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Usługa i Portal / PAM

Usługi i portalu, aby dołączyć dane PAM może zostać zaktualizowana przy użyciu poleceń cmdlet FIMAutomation lub PAM. Jeśli masz portalu można również bezpośrednio zaktualizować przez wyszukiwanie i zmodyfikować obiekt. Jedyną operacją, należy pamiętać, jak i w zależności od konfiguracji po prostu aktualizacji z portalu nie oznaczają, że będzie ona. Jako źródło urzędu jest wysoce zależne od konfiguracji ogólnej.

### <a name="bhold"></a>BHOLD

Użytkownicy można bezpośrednio zaktualizować za pomocą interfejsu użytkownika BHOLD Core lub łącznika zarządzania dostępu.

### <a name="certificate-management"></a>Zarządzanie certyfikatami

Użytkownicy w usłudze zarządzania certyfikatu są odbicia z usługi active directory. Aby zaktualizować Użyj usługi Active Directory, aby zmienić szczegóły obiektu.

## <a name="deleting-personal-data"></a>Usuwanie danych osobowych

>[!Note] 
> Ten artykuł zawiera wskazówki dotyczące sposobów usuwania danych osobowych z programu Microsoft Identity Manager i może służyć do obsługi sieci zobowiązań GDPR. Jeśli szukasz informacji ogólnych o GDPR zobacz [GDPR części portalu zaufania usługi](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Dane w programie MIM są zsynchronizowane i zawsze aktualizowane na podstawie jego połączonego źródła danych. Po usunięciu obiektu docelowego dane obiektu w usłudze MIM mogą być obsługiwane na potrzeby badania zabezpieczeń. Usunięcie obiektu jest skonfigurowana dla poszczególnych reguł źródła danych połączenia lub extension(code) reguły i/lub reguły usuwania obiektów.

### <a name="synchronization-service"></a>Usługa synchronizacji
Synchronizacja usługi tyle metody obsługi danych lub usuwania danych w zależności od procesów biznesowych. Aby lepiej zrozumieć, poniżej przedstawiono niektóre artykuły, aby pomóc w zrozumieniu opcji na usuwanie i aktualizowanie atrybutów: 

- [Informacje na temat anulowania aprowizacji](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Za pomocą rozszerzenia Zasady](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Najlepsze rozwiązania w zakresie MIM](https://docs.microsoft.com/en-us/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Usługa i Portal / PAM

Zalecane jest usługa i Portal pozostawienie domyślnej konfiguracji przechowywania zasobów systemu 30 dni. Informuje usługi, gdy zostaną usunięte, nie tylko dane żądania, ale również dowolnego obiektu, który ma być usunięte z systemu. W momencie proces usunięciu wszystkich danych połączonych z tym obiektem obejmuje wszystkie dane rejestracji SSPR. To jest odtwarzany w powyższej konfiguracji usuwania obiektów. Mamy jednej tabeli zostały przechowujemy identyfikator guid obiektu. Aby zmniejszyć ogólny rozmiar tabeli w kompilacji 4.4.1459 dodaliśmy w procesie nazywanym FIM_DeleteExpiredSystemObjectsJob szczegóły tego procesu można znaleźć [tutaj](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacy zgodności srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold, takie jak większość systemów połączony z usługą synchronizacji można skonfigurować do usunięcia raz obiekt źródłowy, jak HR zostanie usunięta. To jest skonfigurowana dla agenta zarządzania. i kontrolowane przez zasady usunięcie obiektu zgodnie z opisem w funkcji usługi synchronizacji.

Innym rozwiązaniem jest usuwania obiektu użytkownika z interfejsu użytkownika Core BHOLD. W zależności od konfiguracji to można działają prawidłowo, ale należy pamiętać, inicjowanie obsługi logiki można ponownie utworzyć tego użytkownika, jeśli nie usunął w źródle.
![mim-privacy zgodności bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Zarządzanie certyfikatami
Aby usunąć użytkownika z CM, Usuń użytkownik jest w usłudze active directory.

Zarządzanie certyfikatami w postaci, w jakiej będzie przechowywane tylko uid profilu z usługi certyfikatów z sAMAccountName domeny. Po usunięciu użytkownika z usługi AD w pamięci podręcznej użytkownika będzie on wyświetlany dla certyfikatów, Zmień, które mają one zarejestrowane. Nie zaleca się usunięcie niczego w bazie danych, ponieważ może to spowodować szkody ogólną operacji środowiska.

## <a name="opt-out-of-telemetry"></a>Wypisz telemetrii
Wcześniejsze kompilacje programu FIM/MIM używane do zbiera anonimowe dane telemetryczne dotyczące każdego wdrożenia i przesyła te dane przy użyciu protokołu HTTPS do serwerów firmy Microsoft. Te dane została użyta przez firmę Microsoft w celu ulepszania przyszłych wersji FIM/MIM w przeszłości.

>[!Note] 
> W kolejnych wersjach 4.5.x.x lub większa danych zostaną wyłączone kolekcje.

Aby wyłączyć danych kolekcji w poprzedniej wersji, uruchom Zmień tryb i usuń zaznaczenie następujący wiersz:

![mim-privacy zgodności-poprawy jakości obsługi klienta. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

Edytowanie rejestru lub ustaw wartość na 0: Manager\2010 tożsamości HKLM\SOFTWARE\Microsoft\Forefront poprawy jakości obsługi klienta (składnik)

![mim-privacy zgodności ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Następne kroki 
- [Powiązane SQL ochrony prywatności wskazówki](https://docs.microsoft.com/en-us/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Sekcja GDPR zaufania z portalu](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 archiwum: Zwiększać — wdrożenie programu Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)