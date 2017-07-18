---
title: "Odzyskiwanie awaryjne usługi PAM | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować usługę Privileged Access Management pod kątem wysokiej dostępności i odzyskiwania po awarii."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2fab9af837ed11b1f2f7f32c9ced6d79c8cc9d00
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# Zagadnienia związane z wysoką dostępnością i odzyskiwaniem po awarii w środowisku bastionu
<a id="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment" class="xliff"></a>
W tym artykule omówiono zagadnienia związane z wysoką dostępnością i odzyskiwaniem po awarii w przypadku wdrażania Usług domenowych Active Directory (AD DS) i programu Microsoft Identity Manager 2016 (MIM) na potrzeby funkcji zarządzania dostępem uprzywilejowanym (Privileged Access Management, PAM).

Przedsiębiorstwa skupiają się na wysokiej dostępności i odzyskiwaniu po awarii w przypadku obciążeń w systemie Windows Server, programie SQL Server i usłudze Active Directory. Ważna jest jednak również dostępność środowiska bastionu funkcji PAM. Środowisko bastionu to kluczowy element infrastruktury informatycznej przedsiębiorstwa, ponieważ użytkownicy wchodzą w interakcje z jego składnikami w celu przyjmowania ról administracyjnych. Aby uzyskać więcej ogólnych informacji na temat wysokiej dostępności, możesz pobrać oficjalny dokument [Microsoft High Availability Overview](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc).

## Wysoka dostępność i odzyskiwanie po awarii — scenariusze
<a id="high-availability-and-disaster-recovery-scenarios" class="xliff"></a>

Planując wysoką dostępność i odzyskiwanie po awarii, należy rozważyć następujące kwestie:

- Na jakie funkcje może mieć wpływ awaria?
- Jakie funkcje mają krytyczne znaczenie dla przedsiębiorstwa i/lub dla działania systemu informatycznego?
- Jakie występują zagrożenia mogące prowadzić do awarii w tych systemach?

Zakres uwzględnionych funkcji ma wpływ na całkowity koszt wdrożenia i obsługi, w związku z czym organizacje mogą nadać pewnym funkcjom wyższy priorytet niż innym, a także zaakceptować ryzyko tymczasowych przestojów w przypadku funkcji o niższym priorytecie. W poniższej tabeli przedstawiono przykład priorytetów, jakie można ustalić w organizacji:

| **Funkcja lasu bastionu** | **Względny priorytet podczas odzyskiwania** | **Środki zaradcze w przypadku niedostępności funkcji** |
| --------------------------- | --------------------- | -------------- |
| Tworzenie relacji zaufania         | Niski | Zaczekać na przywrócenie środowiska bastionu |
| Migracja użytkowników i grup   | Niski | Zaczekać na przywrócenie środowiska bastionu |
| Administracja programu MIM          | Niski | Zaczekać na przywrócenie środowiska bastionu |
| Aktywacja ról uprzywilejowanych  | Średni | Dedykowane konta z kartą inteligentną umożliwiające ręczne dodawanie użytkowników do grup administracyjnych |
| Zarządzanie zasobami         | Wysoki | Dedykowane konta z kartą inteligentną umożliwiające ręczne dodawanie użytkowników do grup administracyjnych |
| Monitorowanie użytkowników i grup w istniejącym lesie | Niski | Zaczekać na przywrócenie środowiska bastionu |

Teraz omówimy kolejno poszczególne funkcje lasu bastionu.

### Tworzenie relacji zaufania
<a id="trust-establishment" class="xliff"></a>

Pomiędzy domenami w istniejącym lesie a lasem środowiska bastionu musi istnieć relacja zaufania na poziomie lasu. Dzięki temu użytkownicy uwierzytelnieni w środowisku bastionu mogą administrować zasobami w istniejących lasach. Dodatkowa konfiguracja może być wymagana na przykład w celu zezwolenia na migrację użytkowników z istniejących domen we wcześniejszych wersjach systemu Windows Server.

Na potrzeby tworzenia relacji zaufania kontrolery domen w istniejącym lesie oraz składniki MIM i AD w środowisku bastionu muszą być w trybie online.  Jeśli w czasie tworzenia relacji zaufania nastąpi awaria któregokolwiek ze składników, administrator może przeprowadzić jedną ponowną próbę po rozwiązaniu problemu.  W przypadku odzyskania kontrolerów domeny w istniejącym lesie lub środowiska bastionu po awarii można użyć poleceń cmdlet programu PowerShell `Test-PAMTrust` i `Test-PAMDomainConfiguration`, dostępnych w programie MIM, w celu sprawdzenia, czy relacja zaufania została zachowana.

### Migracja użytkowników i grup
<a id="user-and-group-migration" class="xliff"></a>

Po utworzeniu relacji zaufania można utworzyć w środowisku bastionu kopie grup oraz konta użytkowników dla członków grup oraz osób zatwierdzających. Umożliwi to tym użytkownikom aktywację ról uprzywilejowanych i ponowne uzyskanie członkostwa w grupach.

Na potrzeby migracji użytkowników i grup kontrolery domen w istniejącym lesie oraz składniki MIM i AD w środowisku bastionu muszą być w trybie online.   Jeśli kontrolery domen w istniejącym lesie są niedostępne, nie można dodawać kolejnych użytkowników i grup do środowiska bastionu, ale nie ma to wpływu na istniejących użytkowników i istniejące grupy. Jeśli w czasie migracji nastąpi awaria któregokolwiek ze składników, administrator może przeprowadzić jedną ponowną próbę po rozwiązaniu problemu.

### Administracja programu MIM
<a id="mim-administration" class="xliff"></a>
Po przeprowadzeniu migracji użytkowników i grup administrator może skonfigurować w programie MIM przypisania ról, oznaczając użytkowników jako kandydatów do aktywacji w rolach.  Może także skonfigurować zasady programu MIM dotyczące zatwierdzania i usługi Azure MFA.  

Na potrzeby administracji programu MIM składniki MIM i AD w środowisku bastionu muszą być w trybie online.

### Aktywacja ról uprzywilejowanych
<a id="privileged-role-activation" class="xliff"></a>
Aby aktywować rolę uprzywilejowaną, użytkownik musi uwierzytelnić się w domenie w środowisku bastionu oraz przesłać żądanie do programu MIM.  Program MIM zawiera interfejsy API REST i SOAP oraz interfejs użytkownika w programie PowerShell i na stronie sieci Web.

Na potrzeby aktywacji ról uprzywilejowanych składniki MIM i AD w środowisku bastionu muszą być w trybie online.  Ponadto jeśli w programie MIM skonfigurowano [usługę Azure MFA w celu aktywacji](use-azure-mfa-for-activation.md) wybranej roli, wymagany jest dostęp do Internetu w celu nawiązania kontaktu z usługą Azure MFA.

### Zarządzanie zasobami
<a id="resource-management" class="xliff"></a>
Po pomyślnym aktywowaniu użytkownika w roli kontroler domeny może wygenerować dla niego bilet protokołu Kerberos, którego mogą używać kontrolery domen w istniejących domenach. Będzie także rozpoznawał tymczasowe członkostwo użytkownika w nowych grupach.

Na potrzeby zarządzania zasobami kontroler domeny w domenie zasobu oraz kontroler domeny w środowisku bastionu muszą być w trybie online.  Po aktywacji użytkownika wygenerowanie biletu protokołu Kerberos nie wymaga, aby programy MIM i SQL były w trybie online w środowisku bastionu.  (Należy zwrócić uwagę, że jeśli poziomem funkcjonalnym środowiska bastionu jest system Windows Server 2012 R2, program MIM musi być w trybie online w celu zakończenia tymczasowego członkostwa w grupie).

### Monitorowanie użytkowników i grup w istniejącym lesie
<a id="monitoring-of-users-and-groups-in-the-existing-forest" class="xliff"></a>
Program MIM zawiera również usługę monitorowania PAM, regularnie sprawdzającą użytkowników i grupy w istniejących domenach i wprowadzającą odpowiednie zmiany w bazie danych programu MIM i w usłudze AD.  Usługa ta nie musi być w trybie online na potrzeby aktywacji ról ani zarządzania zasobami.

Na potrzeby monitorowania kontrolery domen w istniejącym lesie oraz składniki MIM i AD w środowisku bastionu muszą być w trybie online.  

## Opcje wdrażania
<a id="deployment-options" class="xliff"></a>
Sekcja [Omówienie środowiska](environment-overview.md) przedstawia podstawową topologię umożliwiającą zapoznanie się z technologią — nie jest ona przeznaczona do zapewniania wysokiej dostępności. W tej sekcji opisano sposób rozszerzenia tej topologii w celu zapewnienia wysokiej dostępności, zarówno w przypadku organizacji mających jedną lokację, jak i tych mających większą liczbę istniejących lokacji.

### Obsługa sieci
<a id="networking" class="xliff"></a>

Ruch sieciowy pomiędzy komputerami w środowisku bastionu powinien być oddzielony od istniejących sieci, na przykład poprzez użycie innej sieci fizycznej lub wirtualnej.  W zależności od występujących zagrożeń dla środowiska bastionu może być konieczne także zastosowanie niezależnych fizycznych połączeń pomiędzy tymi komputerami.  Niektóre technologie klastra trybu failover są związane z dodatkowymi wymaganiami dotyczącymi interfejsów sieciowych.

Komputery hostujące Usługi domenowe Active Directory i komputery hostujące usługi MIM w środowisku bastionu wymagają dwukierunkowej łączności z zasobami w istniejącym lesie na potrzeby następujących operacji:
- uwierzytelnianie użytkowników przez kontrolery domen w lesie PRIV;
- żądanie aktywacji przez użytkowników;
- generowanie dla użytkowników biletów protokołu Kerberos, których mogą używać zasoby w istniejącym lesie;
- monitorowanie domen w istniejącym lesie przez program MIM;
- wysyłanie wiadomości e-mail przez program MIM za pośrednictwem serwerów znajdujących się w istniejącym lesie.

### Minimalne topologie na potrzeby wysokiej dostępności
<a id="minimal-high-availability-topologies" class="xliff"></a>
Organizacja może wybrać funkcje w środowisku bastionu wymagające wysokiej dostępności, z następującymi ograniczeniami:

- Wysoka dostępność dowolnej funkcji obsługiwanej przez środowisko bastionu wymaga co najmniej dwóch kontrolerów domeny.  
- Wysoka dostępność na potrzeby żądań aktywacji wymaga co najmniej dwóch komputerów hostujących usługę MIM oraz wysokiej dostępności dla programu SQL Server.
- Wysoka dostępność dla programu SQL Server z klastrami trybu failover wymaga co najmniej dwóch serwerów obsługujących program SQL Server, które nie mogą jednocześnie być kontrolerami domeny.
- Usługa MIM nie powinna być zainstalowana na kontrolerze domeny, aby zminimalizować narażenie poszczególnych serwerów na ataki.

Minimalna topologia zapewniająca wysoką dostępność dla wszystkich funkcji w środowisku bastionu obejmuje co najmniej cztery serwery i magazyn udostępniony. Dwa z tych serwerów należy skonfigurować jako kontrolery domen obsługujące usługi domenowe Active Directory. Dwa pozostałe serwery można skonfigurować jako klaster trybu failover obsługujący program SQL Server i usługę MIM.

Ponadto typowe wdrożenie środowiska bastionu obejmuje również uprzywilejowaną administracyjną stację roboczą umożliwiającą zarządzanie tymi serwerami oraz składnik monitorujący.

Na poniższym diagramie przedstawiono przykładową architekturę:

![Topologia środowiska bastionu — diagram](media/bastion1.png)

Dla każdej z tych funkcji można skonfigurować dodatkowe serwery w celu zapewnienia wyższej wydajności w warunkach obciążenia lub w celu zapewnienia nadmiarowości geograficznej zgodnie z poniższym opisem.

### Wdrożenia z obsługą wielu lokacji
<a id="deployments-supporting-multiple-sites" class="xliff"></a>
Wybór właściwej topologii wdrożenia dla zasobów wdrażanych w wielu lokacjach zależy od następujących trzech czynników:  
- Cele i zagrożenia związane z wysoką dostępnością i odzyskiwaniem po awarii.  
- Możliwości sprzętowe w zakresie hostowania środowiska bastionu.  
- Model administracyjny poszczególnych lokacji.

Jedną z najprostszych metod byłoby hostowanie środowiska bastionu w określonej lokacji.  W normalnych warunkach użytkownicy łączą się wówczas z wdrożeniem programu MIM w środowisku bastionu tej lokacji i przesyłają żądanie aktywacji, a aktywacja ma wpływ na wszystkie zasoby we wszystkich lokacjach.  W przypadku przerwania połączenia sieciowego lub niedostępności lokacji hostującej środowisko bastionu można na potrzeby bieżącej administracji uzyskać dostęp do poświadczeń trybu offline w innej lokacji, do momentu przywrócenia połączenia sieciowego.  Ta metoda może sprawdzać się w sytuacjach, w których lokalna administracja w określonej lokacji, na przykład w oddziale organizacji, najprawdopodobniej będzie rzadka i ograniczona do ponownego połączenia tej lokacji z pozostałą częścią sieci organizacji.

![Topologia z jednym bastionem i wieloma lokacjami — diagram](media/bastion2.png)

W celu zapewnienia wysokiej dostępności i odzyskiwania po awarii w różnych lokacjach można także wdrożyć składniki bastionu we wszystkich lokacjach, ze współdzielonym katalogiem PRIV i współdzieloną bazą danych SQL.  W takiej topologii w przypadku przerwania połączenia sieciowego użytkownicy w poszczególnych lokacjach mogą dalej działać niezależnie.

![Topologia z wieloma bastionami i wieloma lokacjami — diagram](media/bastion3.png)

Ograniczeniem tej metody wdrożenia jest fakt, że program SQL Server wymaga klastra obejmującego obie lokacje, co może zwiększyć złożoność wdrożenia. W takiej sytuacji alternatywnym rozwiązaniem może być replikacja usługi Active Directory (lasu PRIV) środowiska bastionu.  W przypadku przerwania połączenia sieciowego między lokacjami użytkownicy w lokacji B, którzy uprzednio aktywowali role uprzywilejowane, będą mogli nadal administrować zasobami w lokacji B.

![Topologia ze zreplikowanym bastionem i wieloma lokacjami — diagram](media/bastion4.png)

Jeśli każda lokacja jest osobnym obszarem administracyjnym, można także wdrożyć większą liczbę niezależnych środowisk bastionu.  Każde z tych środowisk bastionu będzie korzystać z takiego samego oprogramowania, ale nazwy domen będą różne, a katalogi i bazy danych poszczególnych środowisk bastionu nie będą współdzielone. Chcąc zarządzać zasobami w określonej lokacji, użytkownik aktywuje konto użytkownika w środowisku bastionu tej lokacji.

![Topologia z niezależnymi bastionami i wieloma lokacjami — diagram](media/bastion5.png)

Możliwe są w końcu bardziej złożone wdrożenia, na przykład niezależne skonfigurowanie większej liczby bastionów do zarządzania zasobami w określonej domenie.

![Złożona topologia bastionów z wieloma lokacjami — diagram](media/bastion6.png)

### Hostowane środowisko bastionu
<a id="hosted-bastion-environment" class="xliff"></a>
Niektóre organizacje przewidują również utworzenie środowiska bastionu odrębnego od wcześniej istniejących lokacji. Oprogramowanie środowiska bastionu może być hostowane na platformie wirtualizacji w obrębie sieci organizacji lub u zewnętrznego dostawcy hostingu.  Rozpatrując takie podejście, należy uwzględnić następujące kwestie:

- W celu ochrony przed atakami pochodzącymi z istniejących domen należy oddzielić administrację środowiska bastionu od kont administracyjnych istniejącej domeny.
- Środowisko bastionu wymaga połączenia protokołu TCP/IP z kontrolerami domeny w istniejącej domenie.  Lista portów znajduje się w artykule [Konfigurowanie zapory na potrzeby domen i zaufania](https://support.microsoft.com/kb/179442).
- Zwirtualizowane wdrożenie Usług domenowych Active Directory wymaga określonych funkcji platformy wirtualizacji, zgodnie z opisem w artykule [Wdrażanie i konfigurowanie zwirtualizowanego kontrolera domeny](https://technet.microsoft.com/library/jj574223.aspx).
- Wdrożenie z wysoką dostępnością programu SQL Server dla usługi MIM wymaga specjalnej konfiguracji magazynu, zgodnie z opisem w sekcji [Magazyn bazy danych programu SQL Server](#sql-server-database-storage) poniżej.  Nie wszyscy dostawcy hostingu mogą aktualnie oferować hosting systemu Windows Server z konfiguracjami dysków spełniającymi wymagania klastra trybu failover dla programu SQL Server.

## Przygotowanie do wdrożenia i procedury odzyskiwania
<a id="deployment-preparation-and-recovery-procedures" class="xliff"></a>
Przygotowanie do wdrożenia środowiska bastionu zapewniającego wysoką dostępność lub gotowość do odzyskiwania po awarii wymaga zaplanowania sposobu instalacji usługi Active Directory systemu Windows Server, programu SQL Server i jego bazy danych w magazynie udostępnionym oraz usługi MIM i jej składników PAM.

### Windows Server
<a id="windows-server" class="xliff"></a>
System Windows Server zawiera wbudowaną funkcję wysokiej dostępności, umożliwiającą wielu komputerom współdziałanie jako klaster trybu failover. Serwery klastrowane są połączone za pomocą fizycznych kabli i oprogramowania. Jeśli jeden lub kilka węzłów ulegnie awarii, pozostałe węzły rozpoczną udostępnianie usługi w ramach procesu nazywanego trybem failover.   Aby uzyskać więcej informacji, zobacz [Klastry trybu failover — omówienie](https://technet.microsoft.com/library/hh831579.aspx).

Upewnij się, że system operacyjny i aplikacje w środowisku bastionu otrzymują aktualizacje związane z zabezpieczeniami. Niektóre z tych aktualizacji mogą wymagać ponownego uruchomienia serwera, należy zatem skoordynować czas instalowania aktualizacji na poszczególnych serwerach w celu uniknięcia dłuższych przestojów. Możliwym rozwiązaniem jest zastosowanie [aktualizacji typu cluster-aware](https://technet.microsoft.com/library/hh831694.aspx) na serwerach tworzących klaster trybu failover systemu Windows Server.

Serwery w środowisku bastionu będą połączone z domeną i zależne od usług domenowych. Upewnij się, że nie skonfigurowano przypadkowo zależności od określonego kontrolera domeny na potrzeby usług takich jak DNS.

### Usługa Active Directory środowiska bastionu
<a id="bastion-environment-active-directory" class="xliff"></a>
Usługi domenowe Active Directory systemu Windows Server obejmują natywną obsługę wysokiej dostępności i odzyskiwania po awarii.

#### Przygotowanie
<a id="preparation" class="xliff"></a>
Typowe wdrożenie produkcyjne zarządzania dostępem uprzywilejowanym obejmuje co najmniej dwa kontrolery domeny w środowisku bastionu. Instrukcje konfigurowania pierwszego kontrolera domeny w środowisku bastionu zawiera krok 2 artykułu dotyczącego wdrażania, [Przygotowanie kontrolera domeny PRIV](step-2-prepare-priv-domain-controller.md).

Procedurę dodawania kolejnego kontrolera domeny zawiera artykuł [Instalowanie repliki kontrolera domeny systemu Windows Server 2012 w istniejącej domenie (poziom 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE]
> Jeśli kontroler domeny ma być hostowany na platformie wirtualizacji, takiej jak Hyper-V, zapoznaj się z ostrzeżeniami w artykule [Wdrażanie i konfigurowanie zwirtualizowanego kontrolera domeny](https://technet.microsoft.com/library/jj574223.aspx).

#### Odzyskiwanie
<a id="recovery" class="xliff"></a>
Po awarii, przed ponownym uruchomieniem pozostałych serwerów, upewnij się, że w środowisku bastionu jest dostępny co najmniej jeden kontroler domeny.

W obrębie domeny usługa Active Directory rozdziela role FSMO między kontrolerami domeny, zgodnie z opisem w artykule [Jak działają wzorce operacji](https://technet.microsoft.com/library/cc780487.aspx).  Jeśli kontroler domeny uległ awarii, może być konieczne przeniesienie [ról kontrolera domeny](https://technet.microsoft.com/library/cc786438.aspx) przypisanych do tego kontrolera domeny.

Po ustaleniu, że kontroler domeny nie zostanie przywrócony do środowiska produkcyjnego, pamiętaj o sprawdzeniu, czy były do niego przypisane jakiekolwiek role, i zmianie przypisania tych ról stosownie do potrzeb. Instrukcje znajdują się w artykule [Wyświetlanie aktualnych przypisań ról wzorca operacji](https://technet.microsoft.com/library/cc816893.aspx) i powiązanych artykułach.

Zaleca się również sprawdzenie ustawień DNS na komputerach połączonych ze środowiskiem bastionu oraz kontrolerów domeny w domenach CORP mających relację zaufania z tym kontrolerem domeny w celu upewnienia się, że nie występują ustalone zależności od adresu IP komputera tego kontrolera domeny.

### Magazyn bazy danych programu SQL Server
<a id="sql-server-database-storage" class="xliff"></a>
Wdrożenie wysokiej dostępności wymaga zastosowania klastrów trybu failover dla programu SQL Server, a wystąpienia klastrów trybu failover dla programu SQL Server wymagają współdzielenia przez wszystkie węzły magazynu zawierającego bazę danych i dzienniki. Magazyn udostępniony mogą stanowić dyski klastra trybu failover systemu Windows Server, dyski w sieci SAN lub udziały plików na serwerze SMB.  Należy pamiętać, że elementy te muszą być przeznaczone wyłącznie dla środowiska bastionu. Udostępnianie magazynu z innymi obciążeniami poza środowisko bastionu nie jest zalecane, ponieważ może zagrozić integralności środowiska bastionu.

### Serwer SQL
<a id="sql-server" class="xliff"></a>
Usługa MIM wymaga wdrożenia programu SQL Server w środowisku bastionu.   Na potrzeby wysokiej dostępności program SQL można wdrożyć przy użyciu wystąpienia klastra trybu failover. W odróżnieniu od wystąpień autonomicznych, w przypadku wystąpienia klastra trybu failover wysoka dostępność programu SQL Server jest chroniona przez występowanie nadmiarowych węzłów w wystąpieniu klastra trybu failover. W przypadku awarii lub zaplanowanego uaktualnienia własność grupy zasobów jest przenoszona do innego węzła klastra trybu failover systemu Windows Server.

Jeśli wymagana jest obsługa odzyskiwania po awarii, ale nie wysokiej dostępności, zamiast klastra trybu failover można użyć metody wysyłania dziennika, replikacji transakcji, replikacji migawek lub dublowania bazy danych.   

#### Przygotowanie
<a id="preparation" class="xliff"></a>
Instalacja programu SQL Server w środowisku bastionu musi być niezależna od jakichkolwiek istniejących instalacji programu SQL Server w lasach CORP.  Ponadto zaleca się wdrożenie programu SQL Server na serwerze dedykowanym, odrębnym od kontrolera domeny.
Aby uzyskać więcej informacji, zobacz następujący przewodnik programu SQL Server: [Wystąpienia klastra trybu failover funkcji AlwaysOn](https://msdn.microsoft.com/library/ms189134.aspx).

#### Odzyskiwanie
<a id="recovery" class="xliff"></a>
Jeśli skonfigurowano program SQL Server na potrzeby odzyskiwania po awarii z użyciem wysyłania dzienników, należy podjąć działania w celu zaktualizowania programu SQL Server podczas odzyskiwania.  Ponadto wymagane jest ponowne uruchomienie wszystkich wystąpień usługi MIM.

Jeśli program SQL Server uległ awarii lub utracono połączenie między programem SQL Server a usługą MIM, zaleca się ponowne uruchomienie wszystkich wystąpień usługi MIM po przywróceniu programu SQL Server.  To zapewni ponowne nawiązanie połączenia między usługą MIM a programem SQL Server.

### Usługa MIM
<a id="mim-service" class="xliff"></a>
Usługa MIM jest wymagana do przetwarzania żądań aktywacji.  Aby umożliwić wyłączenie komputera hostującego usługę MIM na potrzeby konserwacji przy jednoczesnej kontynuacji odbierania żądań aktywacji, można wdrożyć większą liczbę komputerów usługi MIM.  Należy zwrócić uwagę, że usługa MIM nie uczestniczy w operacjach protokołu Kerberos po dodaniu użytkownika do grupy.  

#### Przygotowanie
<a id="preparation" class="xliff"></a>
Zalecane jest wdrożenie usługi MIM na większej liczbie serwerów połączonych z domeną PRIV.
Planując wdrożenie wysokiej dostępności, zobacz następującą dokumentację systemu Windows Server: [Wymagania sprzętowe klastra trybu failover i opcje magazynu](https://technet.microsoft.com/library/jj612869.aspx) oraz [Tworzenie klastra trybu failover w systemie Windows Server 2012](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx).

W przypadku wdrożenia produkcyjnego na wielu serwerach możesz użyć funkcji równoważenia obciążenia sieciowego (NLB, Network Load Balancing) w celu rozłożenia obciążenia związanego z przetwarzaniem.  Zalecane jest także utworzenie jednego aliasu (na przykład rekordów A lub CNAME) w celu przedstawienia użytkownikowi jednej, wspólnej nazwy.

>[!IMPORTANT]
> W przypadku korzystania z technologii równoważenia obciążenia sieciowego innej niż funkcja NLB w systemie Windows Server 2012 R2 upewnij się, że używane rozwiązanie będzie przekierowywało jedną sesję na ten sam serwer, a nie na losowy serwer.

W przypadku wdrożenia programu MIM na wielu serwerach każda usługa MIM ma zewnętrzną nazwę hosta, nazwę usługi i nazwę partycji usługi.  Domyślna wartość nazwy usługi to nazwa komputera, a domyślne wartości zewnętrznej nazwy hosta i nazwy partycji usługi są konfigurowane podczas instalacji usługi MIM na ekranie, na którym wprowadza się adres serwera usługi MIM. Te trzy nazwy są przechowywane w pliku %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config jako atrybuty `externalHostName`, `serviceName` i `servicePartitionName` węzła konfiguracji `resourceManagementService`.  

Gdy usługa MIM odbiera żądanie, nazwa partycji usługi jest zapisana w tym żądaniu jako atrybut.   Następnie interakcja z tym żądaniem jest dozwolona wyłącznie dla instalacji usługi MIM mających tę samą nazwę partycji usługi.  W związku z tym jeśli scenariusz PAM uwzględnia zatwierdzanie ręczne lub inne długotrwałe procedury przetwarzania żądań, należy upewnić się, że każda usługa MIM ma taki sam atrybut `servicePartitionName` w tym pliku konfiguracyjnym.

#### Odzyskiwanie
<a id="recovery" class="xliff"></a>
Po awarii, przed ponownym uruchomieniem usługi MIM, upewnij się, że w środowisku bastionu jest dostępny co najmniej jeden kontroler domeny usługi Active Directory i serwer SQL.  

Wystąpienie przepływu pracy może zostać zrealizowane wyłącznie przez serwer usługi MIM mający taką samą nazwę partycji usługi i nazwę usługi jak serwer usługi MIM, który je zainicjował.  Jeśli określony komputer hostujący usługę MIM przetwarzającą żądania ulegnie awarii i nie zostanie przywrócony do pracy, będzie konieczne zainstalowanie usługi MIM na nowym komputerze. Po zainstalowaniu nowej usługi MIM należy edytować plik *resourcemanagementservice.exe.config* i zmienić atrybuty `serviceName` i `servicePartitionName` nowego wdrożenia usługi MIM na takie same, jak nazwa hosta i nazwa partycji usługi komputera, który uległ awarii.

### Składniki PAM programu MIM
<a id="mim-pam-components" class="xliff"></a>
Instalator usługi i portalu MIM zawiera również dodatkowe składniki PAM — moduły programu PowerShell i dwie usługi.

#### Przygotowanie
<a id="preparation" class="xliff"></a>
Składniki PAM należy zainstalować na wszystkich komputerach w środowisku bastionu, na których jest instalowana usługa MIM.  Nie można ich dodać później.

#### Odzyskiwanie
<a id="recovery" class="xliff"></a>
Po odzyskaniu po awarii należy sprawdzić, czy usługa MIM działa na co najmniej jednym serwerze.  Następnie należy sprawdzić, czy usługa monitorowania PAM programu MIM również działa na tym serwerze, używając polecenia `net start "PAM Monitoring service"`.

Jeśli poziomem funkcjonalnym lasu środowiska bastionu jest system Windows Server 2012 R2, należy sprawdzić, czy usługa składnika PAM programu MIM również działa na tym serwerze, używając polecenia `net start "PAM Component service"`.
