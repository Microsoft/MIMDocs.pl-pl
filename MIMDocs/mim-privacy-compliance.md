---
title: Obsługa danych Microsoft Identity Manager | Microsoft Docs
description: Zrozumienie Microsoft Identity Manager obsługi danych w celu identyfikowania i raportowania danych w środowisku, podejmowanie działań w danym systemie na podstawie funkcji operacyjnych i wymagań.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: b89f7561869e154ed5639835d1233e19e356ee76
ms.sourcegitcommit: f87be3d09cee6a8880b3a6babf32e0d064fde36b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87176748"
---
# <a name="microsoft-identity-manager-data-handling"></a>Obsługa danych Microsoft Identity Manager 

Ten artykuł zawiera wskazówki dotyczące sposobu, w jaki organizacje mogą podejmować decyzje, które mogą być stosowane w wielu połączonych źródłach danych.  Można to osiągnąć za pomocą operacji wyszukiwania, usuwania, aktualizacji i raportów.  Przed podjęciem decyzji o usunięciu lub aktualizacji należy zrozumieć bieżący projekt i konfigurację systemu programu Identity Manager (MIM). 

Poniżej przedstawiono kilka scenariuszy, w których klienci będą musieli rozważyć i odpowiedzieć na następujące pytania: 

- Jakie dane są potrzebne do zarządzania tożsamościami, aby pomóc w procesie biznesowym?
- Gdzie są przechowywane bieżące dane w programie MIM?
- Jak można używać tych danych w systemie?
- Czy te dane są udostępniane wszystkim zewnętrznym partnerom danych (eksportowanie)
- Co to jest autorytatywne źródło danych i jego przetwarzanie?
- W jaki sposób będzie odbywać się plan przechowywania i usuwania danych?
- Czy zidentyfikowano całą technologię potrzebną do przetwarzania danych i zarządzania nimi?

Aby ułatwić zrozumienie bieżącego środowiska programu MIM, można użyć następującego narzędzia do dokumentowania środowiska programu MIM lub przełożyć do dokumentów projektu implementacji.
- [Dokument programu MIM — umożliwia eksportowanie bieżącej konfiguracji](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Wyszukiwanie i identyfikowanie danych osobowych
Wyszukiwanie danych w programie MIM będzie zależeć od konfiguracji i konfiguracji. Większość środowisk jest wzajemnie połączonych, ale dla jasności pozostało, że są one przez składnik wysokiego poziomu.

### <a name="synchronization-service"></a>Usługa synchronizacji

Wszystkie dane w programie MIM, które odnoszą się do użytkowników, pochodzą ze źródeł danych Active Directory (AD) i kadr. Podczas wyszukiwania danych osobowych najpierw należy rozważyć wyszukiwanie w usłudze AD lub połączonym źródle danych. 

Jeśli nie masz pewności, że źródło uprawnień można śledzić przy użyciu konsoli Synchronization Service Manager programu MIM, kliknij pasek wyszukiwania Metaverse, aby wyświetlić identyfikowalne dane osobowe, które są przechowywane w bazie danych. Użytkownicy mogą wyszukiwać określonego użytkownika lub atrybut.

- Aby przeprowadzić przegląd lub przeszukać dane obiektów użytkownika
    - Otwórz klienta usługi synchronizacji
        - Za pomocą projektanta Metaverse można zobaczyć Importy i pierwszeństwo przepływu atrybutów.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Użycie wyszukiwania Metaverse umożliwia wyszukanie dowolnego obiektu i atrybutu w bazie danych ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Po znalezieniu obiektu, kliknięcie obiektu spowoduje otwarcie strony profilu użytkownika. Szczegóły obiektu udostępniają kompleksowe szczegóły dotyczące obiektu, jego atrybutów, ostatniej modyfikacji i źródła uprawnień oraz powiązane źródło danych pochodzące z konfiguracji agenta zarządzania poniżej.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Usługa i Portal/PAM
Jeśli masz wystąpienie usługi i portalu lub zainstalowano moduł PAM z możliwością wyszukiwania użytkowników, jest on ważny. 

Jeśli zainstalowano Portal, można użyć interfejsu użytkownika do wyszukania dowolnego atrybutu lub zapytania dla określonego użytkownika.

Jeśli masz zainstalowany tylko serwer usługi (bez interfejsu użytkownika portalu), możesz uruchomić składnię wyszukiwania na podstawie [FIMAutomation PSSnapin], przykładu znalezionego [tutaj](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

W usłudze PAM można użyć tej samej składni powyżej lub do wyszukania użytkownika w środowisku PAM można użyć [modułu MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) .

Inne opcje raportowania do wyszukiwania dostępnych danych są dostępne w portalu usługi i.
- [Raportowanie hybrydowe](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Raportowanie za pomocą SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Usługa pakietu BHOLD Core ma interfejs użytkownika, który umożliwia wyszukiwanie użytkownika lub atrybutów. 

![wyszukiwanie pakietu BHOLD](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Jeśli synchronizujesz pakietu BHOLD z [łącznikiem zarządzania dostępem](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) dla usługi synchronizacji, będziesz mieć możliwość wyświetlenia połączonych obiektów użytkownika i atrybutów, które wysyłamy do pakietu BHOLD rdzeń.

Można również załadować moduł raportowania pakietu BHOLD.

- [Raportowanie pakietu BHOLD](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Zarządzanie certyfikatami
Wyszukiwanie usługi zarządzania certyfikatami jest wbudowane w interfejs użytkownika. Zostanie uruchomiony administrator i zostanie wybrana opcja "Znajdź użytkownika i Wyświetl ich informacje".  

![Wyszukiwanie cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Eksportowanie danych osobowych
Ponieważ dane związane z jednostkami w programie MIM pochodzą z wielu źródeł, większość danych jest przechowywanych w bazie danych usługi synchronizacji. Z tego powodu należy wyeksportować dane dotyczące obiektów z usługi synchronizacji programu MIM lub określić właściciela tych danych.

### <a name="synchronization-service"></a>Usługa synchronizacji
Usługi synchronizacji do eksportowania danych po prostu wybierają dane z interfejsu użytkownika wyszukiwania i kopiowania i wklejania w formacie CSV lub preferowanym. Innym sposobem, aby wyeksportować te dane, jest utworzenie pliku, który jest wymagany do porzucenia bieżących danych o oflagowanym użytkowniku. Automatyzacji z korzystaniem z usługi File-based MA można znaleźć [tutaj](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Usługa i Portal/PAM
Usługa i Portal wraz z modułem PAM można wyeksportować te dane, uruchom składnię wyszukiwania na podstawie [FIMAutomation PSSnapin], przykładu znalezionego [tutaj](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) i potoku do pliku [CSV](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

W usłudze PAM można użyć tej samej składni powyżej lub użyć [modułu MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) w specjalnej części Get-pamuser, aby wyszukać użytkownika w środowisku Pam i przetworzyć go do pliku CSV.

- [Przykładowe zapytanie dotyczące usługi MIM przy użyciu programu PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Dane pakietu BHOLD można eksportować przy użyciu modułu raportowania pakietu BHOLD do preferowanego formatu.

### <a name="certificate-management"></a>Zarządzanie certyfikatami
Dane zarządzania certyfikatami powiązane z danymi osobowymi są połączone z usługą Active Directory. Administrator może wyeksportować te dane przy użyciu programu Active Directory PowerShell.

## <a name="updating-personal-data"></a>Aktualizowanie danych osobowych

Dane osobowe dotyczące użytkowników lub obiektów w rozwiązaniach programu MIM zazwyczaj pochodzą od obiektu użytkownika w połączonych źródłach danych organizacji. Ze względu na to, że wszelkie zmiany wprowadzone w profilu użytkownika w źródle danych lub w innym autorytatywnym systemie rekordów, takie jak Usługa AD, zostaną odzwierciedlone w usłudze synchronizacji programu MIM.

### <a name="synchronization-service"></a>Usługa synchronizacji

W celu wykonywania operacji zarządzania administratorzy muszą być częścią operacji synchronizacji lub administratora zdefiniowanego w [tym miejscu](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

Aktualizowanie danych odbywa się przez definiowanie reguł ze źródła urzędu. Konsola zarządzania pomaga identyfikować Źródło urzędu, aby zaktualizować je do źródła. Inną opcją jest utworzenie reguły synchronizacji lub przedziału reguły w celu kontrolowania aktualizacji danych, jeśli źródło takie jak dane HR nadal musi pozostać. Są dostępne opcje obsługiwane.

Aby uzyskać więcej informacji na temat różnych sposobów aktualizowania atrybutów, zobacz poniżej. 

- [Używanie rozszerzeń reguł](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Informacje na temat synchronizacji danych z systemami zewnętrznymi](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Usługa i Portal/PAM

Usługa i portal do dołączania danych PAM można aktualizować za pomocą poleceń cmdlet FIMAutomation lub PAM. Jeśli masz Portal, możesz również bezpośrednio zaktualizować przez wyszukiwanie i modyfikowanie obiektu. Należy pamiętać, że w zależności od konfiguracji po prostu Aktualizacja z portalu nie oznacza, że pozostanie. Ponieważ źródło urzędu jest wysoce zależne od konfiguracji ogólnej.

### <a name="bhold"></a>BHOLD

Użytkowników można aktualizować bezpośrednio przy użyciu interfejsu użytkownika pakietu BHOLD Core lub łącznika zarządzania dostępem.

### <a name="certificate-management"></a>Zarządzanie certyfikatami

Użytkownicy w usłudze zarządzania certyfikatami to wszystkie odbicia z usługi Active Directory. Aby zaktualizować szczegóły obiektu, użyj Active Directory.

## <a name="deleting-personal-data"></a>Usuwanie danych osobowych

>[!Note] 
> Ten artykuł zawiera wskazówki dotyczące sposobów usuwania danych osobowych z Microsoft Identity Manager i można ich użyć do obsługi zobowiązań w ramach Rodo. Jeśli szukasz ogólnych informacji o Rodo, zobacz [sekcję Rodo w portalu zaufania usługi](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Dane w programie MIM są synchronizowane i zawsze aktualizowane ze swojego połączonego źródła danych. Po usunięciu obiektu w miejscu docelowym dane obiektu w programie MIM mogą być utrzymywane do celów badania zabezpieczeń. Usuwanie obiektu jest konfigurowane dla reguł źródła danych lub rozszerzenia reguły (kod) i/lub usuwania obiektów.

### <a name="synchronization-service"></a>Usługa synchronizacji
Usługa synchronizacji na wiele sposobów obsługi danych lub usuwania danych w zależności od procesów biznesowych. Aby pomóc zrozumieć, poniżej przedstawiono niektóre artykuły ułatwiające zrozumienie opcji usuwania i aktualizowania atrybutów: 

- [Informacje na temat anulowania aprowizacji](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Używanie rozszerzeń reguł](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Najlepsze praktyki dotyczące programu MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Usługa i Portal/PAM

Zalecane jest, aby usługa & Portal zachowywać domyślne 30-dniowe konfigurację przechowywania zasobów systemowych. Oznacza to, że usługa zostanie usunięta, nie tylko Zażądaj danych, ale również dla każdego obiektu, który musi zostać wyczyszczony z systemu. Po wystąpieniu tego procesu wszystkie dane połączone z tym obiektem zostaną usunięte. obejmuje to wszystkie dane rejestracji SSPR. Spowoduje to odtworzenie powyższej konfiguracji usuwania obiektów. Mamy jedną tabelę przechowującą identyfikator GUID obiektów. Aby zmniejszyć całkowity rozmiar tabeli w 4.4.1459 kompilacji, dodaliśmy proces o nazwie FIM_DeleteExpiredSystemObjectsJob szczegóły tego procesu można znaleźć [tutaj](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Pakietu BHOLD jak większość systemów podłączonych do usługi synchronizacji można skonfigurować do usuwania po usunięciu obiektu źródłowego, takiego jak HR. Ta konfiguracja jest konfigurowana w agencie zarządzania. i kontrolowane przez reguły usuwania obiektów zgodnie z opisem w obszarze funkcje usługi synchronizacji.

Innym rozwiązaniem jest usunięcie obiektu użytkownika bezpośrednio z interfejsu użytkownika pakietu BHOLD Core. W zależności od konfiguracji może to potrwać, ale Uwaga logika aprowizacji może odtworzyć tego użytkownika, jeśli nie zostanie usunięty ze źródła.
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Zarządzanie certyfikatami
Aby usunąć użytkownika z programu CM, Usuń użytkownika z usługi Active Directory.

Zarządzanie certyfikatami w taki sposób, aby przechowywać identyfikator UID profilu z usług certyfikatów przy użyciu konta sAMAccountName domeny. Po usunięciu użytkownika z usługi AD pamięć podręczna użytkownika jest obecna tylko dla certyfikatów, które zostały zarejestrowane. Nie zalecamy usuwania wszystkiego w bazie danych, ponieważ może to spowodować całkowite szkody dla działania środowiska.

## <a name="opt-out-of-telemetry"></a>Wycofaj dane telemetryczne
Poprzednie kompilacje FIM/MIM używane do zbierania danych telemetrycznych anonimowe o każdym wdrożeniu i przesyłają te dane za pośrednictwem protokołu HTTPS do serwerów firmy Microsoft. Te dane były używane przez firmę Microsoft, aby pomóc w ulepszaniu przyszłych wersji programu FIM/MIM w przeszłości.

>[!Note] 
> W nowszych wersjach 4.5. x. x lub większej zbieranie danych zostanie wyłączone.

Aby wyłączyć zbieranie danych w poprzedniej wersji, uruchom tryb zmiany i usuń zaznaczenie następującego monitu:

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

lub Edytuj rejestr i ustaw wartość 0: (składnik) CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Następne kroki 
- [Wskazówki dotyczące prywatności związanych z programem SQL](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Sekcja Rodo portalu zaufania usługi](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [Archiwum programu FIM 2010: zwiększanie wdrożenia programu Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
