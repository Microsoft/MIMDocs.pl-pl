---
title: "Najlepsze rozwiązania dotyczące programu Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.translationtype: MT
ms.sourcegitcommit: 3bb89e2c86724e6f6d32e4043fa37da74e2b7b24
ms.openlocfilehash: a0d00c7e5d99e43d3fb0b3011a3851f7194bfdf2
ms.contentlocale: pl-pl
ms.lasthandoff: 07/10/2017


---


# Najlepsze rozwiązania dotyczące programu Microsoft Identity Manager 2016
<a id="microsoft-identity-manager-2016-best-practices" class="xliff"></a>

W tym temacie opisano najlepsze rozwiązania dotyczące wdrażania i obsługi programu Microsoft Identity Manager 2016 (MIM)

## Konfiguracja usługi SQL
<a id="sql-setup" class="xliff"></a>
>[!NOTE]
Poniższe zalecenia dotyczące konfigurowania serwera z uruchomioną usługą SQL zakładają wystąpienie usługi SQL dedykowane usłudze FIMService i wystąpienie usługi SQL dedykowane bazie danych usługi FIMSynchronizationService. Jeśli usługa FIMService jest uruchomiona w środowisku skonsolidowanym, konieczne będzie wprowadzenie dostosowań odpowiednich dla danej konfiguracji.

Konfiguracja serwera usługi SQL (Structured Query Language) ma kluczowe znaczenie dla optymalnej wydajności systemu. Osiągnięcie optymalnej wydajności programu MIM w dużych wdrożeniach zależy od stosowania najlepszych rozwiązań dla serwera z uruchomioną usługą SQL. Aby uzyskać więcej informacji, zobacz następujące tematy dotyczące najlepszych rozwiązań dla usługi SQL:

-   [Storage Top 10 Best Practices](http://go.microsoft.com/fwlink/?LinkID=183663) (10 najlepszych rozwiązań dotyczących magazynu)

-   [Optimizing tempdb Performance](http://go.microsoft.com/fwlink/?LinkID=188267) (Optymalizacja wydajności bazy danych tempdb)

-   [SQL Server Best Practices Article](http://go.microsoft.com/fwlink/?LinkID=188268) (Najlepsze rozwiązania dotyczące programu SQL Server)

-   [Reorganizing and Rebuilding Indexes](http://go.microsoft.com/fwlink/?LinkID=188269) (Reorganizacja i ponowne tworzenie indeksów)

### Ustawianie wstępnego rozmiaru plików danych i plików dziennika
<a id="presize-data-and-log-files" class="xliff"></a>

Nie należy polegać na automatycznym zwiększaniu. Zamiast tego należy ręcznie zarządzać wzrostem tych plików. Można pozostawić opcję automatycznego zwiększania ze względów bezpieczeństwa, ale należy aktywnie zarządzać wzrostem plików danych. Przykładowe rozmiary bazy danych programu MIM można znaleźć w dokumencie [FIM Capacity Planning Guide](http://go.microsoft.com/fwlink/?LinkID=185246) (Przewodnik planowania pojemności programu FIM).

### Aby ustawić wstępne rozmiary plików danych i dziennika usługi SQL
<a id="to-presize-sql-data-and-log-files" class="xliff"></a>

1.  Uruchom program SQL Server Management Studio.

2.  Przejdź do usługi FIMService bazy danych, kliknij prawym przyciskiem myszy usługę FIMService, a następnie kliknij pozycję Właściwości.

3.  Na stronie Pliki rozciągnij pliki bazy danych do wymaganego rozmiaru.

### Izolowanie dziennika od plików danych
<a id="isolate-log-from-data-files" class="xliff"></a>

Zastosuj najlepsze rozwiązania dotyczące serwera SQL, aby odizolować pliki dzienników transakcji od plików dzienników danych dla baz danych na oddzielnych dyskach fizycznych.

Tworzenie dodatkowych plików tempdb

W celu uzyskania optymalnej wydajności zaleca się utworzenie jednego pliku danych na każdy rdzeń procesora CPU w pliku tempdb.

### Aby utworzyć dodatkowe pliki tempdb
<a id="to-create-additional-tempdb-files" class="xliff"></a>

1.  Uruchom program SQL Server Management Studio.

2.  Przejdź do bazy danych tempdb w obszarze Systemowe bazy danych, kliknij prawym przyciskiem myszy element tempdb, a następnie kliknij pozycję Właściwości.

3.  Na stronie Pliki utwórz po jednym pliku danych dla każdego rdzenia procesora CPU. Pamiętaj, aby oddzielić pliki danych od plików dziennika bazy danych tempdb na różnych dyskach i jednostkach dysków.

### Zapewnianie wystarczającej ilości miejsca dla plików dziennika
<a id="ensure-adequate-space-for-log-files" class="xliff"></a>

Ważne jest zrozumienie wymagań dyskowych modelu odzyskiwania. Tryb odzyskiwania prostego może być odpowiedni podczas początkowego ładowania systemu, aby ograniczyć użycie miejsca na dysku, ale dane utworzone po najnowszej kopii zapasowej są narażone na utratę danych. W przypadku korzystania z trybu odzyskiwania pełnego konieczne jest zarządzanie użyciem dysku przez tworzenie kopii zapasowych, co obejmuje częste wykonywanie kopii zapasowych dziennika transakcji, aby zapobiec wysokiemu użyciu miejsca na dysku. Aby uzyskać więcej informacji, zobacz [Modele odzyskiwania — omówienie](http://go.microsoft.com/fwlink/?LinkID=185370).

### Ograniczanie pamięci serwera SQL
<a id="limit-sql-server-memory" class="xliff"></a>

W zależności od ilości pamięci dostępnej na serwerze SQL oraz tego, czy serwer SQL jest udostępniany innym usługom (tj. usłudze MIM 2016 Service i usłudze MIM 2016 Synchronization Service), można ograniczyć użycie pamięci serwera SQL. Można to zrobić, wykonując następujące kroki.

1.  Uruchom program SQL Server Enterprise Manager.

2.  Wybierz opcję Nowe zapytanie.

3.  Uruchom następujące zapytanie:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  Ten przykład powoduje ponowne skonfigurowanie serwera SQL do używania nie więcej niż 12 gigabajtów (GB) pamięci.

4.  Zweryfikuj ustawienie przy użyciu następującego zapytania:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### Konfiguracja tworzenia kopii zapasowych i odzyskiwania
<a id="backup-and-recovery-configuration" class="xliff"></a>

Ogólnie rzecz biorąc, wykonywanie kopii zapasowych baz danych należy przeprowadzać zgodnie z zasadami tworzenia kopii zapasowych przyjętymi w organizacji. Jeśli nie zaplanowano wykonywania przyrostowych kopii zapasowych dzienników, należy ustawić dla bazy danych tryb odzyskiwania prostego. Przed wdrożeniem strategii tworzenia kopii zapasowych upewnij się, że rozumiesz implikacje różnych modeli odzyskiwania, a także wymagania dotyczące miejsca na dysku dla tych modeli. Model odzyskiwania pełnego wymaga częstego wykonywania kopii zapasowych dziennika w celu uniknięcia wysokiego użycia miejsca na dysku. Aby uzyskać więcej informacji, zobacz [Modele odzyskiwania — omówienie](http://go.microsoft.com/fwlink/?LinkID=185370) i [FIM 2010 Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (Przewodnik tworzenia kopii zapasowych i przywracania danych programu FIM 2010).

## Tworzenie konta administratora kopii zapasowych dla usługi FIMService po instalacji
<a id="create-a-backup-administrator-account-for-the-fimservice-after-installation" class="xliff"></a>


>[!IMPORTANT]
Elementy członkowskie zestawu Administratorzy usługi FIMService mają unikatowe uprawnienia o kluczowym znaczeniu dla działania wdrożenia programu FIM. Jeśli nie możesz zalogować się jako część zestawu Administratorzy, jedynym rozwiązaniem jest przywrócenie poprzedniej kopii zapasowej systemu. Aby uniknąć tej sytuacji, zalecane jest dodanie innych użytkowników do zestawu administracyjnego programu FIM w ramach konfiguracji po instalacji.

## Usługa FIM Service
<a id="fim-service" class="xliff"></a>


### Konfigurowanie skrzynki pocztowej programu Exchange dla usługi FIM Service
<a id="configuring-fim-service-service-exchange-mailbox" class="xliff"></a>

Poniżej przedstawiono najlepsze rozwiązania dotyczące konfigurowania programu Microsoft Exchange Server dla konta usługi MIM 2016 Service.

- Konto usługi należy skonfigurować tak, aby akceptowało pocztę tylko z wewnętrznych adresów e-mail. W szczególności skrzynka pocztowa konta usługi nigdy nie powinna mieć możliwości odbierania poczty z zewnętrznych serwerów SMTP.

#### Aby skonfigurować konto usługi
<a id="to-configure-the-service-account" class="xliff"></a>

1.  W konsoli zarządzania programu Exchange wybierz **konto usługi FIM Service**.

2.  Wybierz opcję Właściwości, wybierz pozycję Ustawienia przepływu poczty e-mail, a następnie wybierz pozycję **Ograniczenia dostarczania poczty**.

3.  Zaznacz pole wyboru **Wymagaj uwierzytelnienia od wszystkich nadawców**.

Aby uzyskać więcej informacji, zobacz artykuł [Configure Message Delivery Restrictions](http://go.microsoft.com/fwlink/?LinkID=183625) (Konfigurowanie ograniczeń dostarczania wiadomości).

-   Konto usługi należy skonfigurować tak, aby odrzucało wiadomości o rozmiarze przekraczającym 1 MB. Zastosuj najlepsze rozwiązanie, aby [skonfigurować limity rozmiaru wiadomości](http://go.microsoft.com/fwlink/?LinkID=183626) dla skrzynki pocztowej lub folderu publicznego z włączoną obsługą poczty.

-   Konto usługi należy skonfigurować tak, aby miało przydział magazynowania skrzynki pocztowej wynoszący 5 GB. W celu uzyskania optymalnych wyników należy zastosować najlepsze rozwiązania wymienione w artykule [Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929) (Konfigurowanie limitów przydziału magazynowania dla skrzynki pocztowej).

## Portal programu MIM
<a id="mim-portal" class="xliff"></a>


### Wyłączanie indeksowania programu SharePoint
<a id="disable-sharepoint-indexing" class="xliff"></a>

Zaleca się wyłączenie indeksowania programu Microsoft Office SharePoint®. Nie ma żadnych dokumentów, które muszą być indeksowane, a indeksowanie powoduje generowanie wielu wpisów dziennika błędów i potencjalne problemy z wydajnością programu FIM 2010. Aby wyłączyć indeksowanie programu SharePoint

1.  Na serwerze hostującym portal programu MIM 2016 kliknij menu Start.

2.  Kliknij pozycję Wszystkie programy.

3.  Na liście Wszystkie programy kliknij pozycję Narzędzia administracyjne.

4.  W obszarze Narzędzia administracyjne kliknij pozycję Administracja centralna programu SharePoint.

5.  Na stronie Administracja centralna kliknij pozycję Operacje.

6.  Na stronie Operacje, w obszarze Konfiguracja globalna, kliknij pozycję Definicje zadań czasomierza.

7.  Na stronie Definicje zadań czasomierza kliknij opcję Odśwież usługę SharePoint Services Search.

8.  Na stronie Edytowanie zadania czasomierza kliknij opcję Wyłącz.

## Początkowe ładowanie danych programu MIM 2016
<a id="mim-2016-initial-data-load" class="xliff"></a>

W tej sekcji przedstawiono szereg kroków służących zwiększeniu wydajności początkowego ładowania danych z systemu zewnętrznego do programu FIM 2010. Ważne jest zrozumienie, że niektóre z tych kroków to czynności tymczasowe wykonywane podczas początkowego zapełniania systemu i należy je zresetować po jego ukończeniu. Jest to jednorazowa operacja, a nie ciągła synchronizacja.

>[!NOTE]
Aby uzyskać więcej informacji o synchronizowaniu użytkowników między programem FIM 2010 a usługami Active Directory Domain Services (AD DS), zobacz artykuł [How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277) (Synchronizowanie użytkowników z usługi Active Directory z programem FIM) w dokumentacji programu FIM.

>[!IMPORTANT]
Upewnij się, że zastosowane zostały najlepsze rozwiązania opisane w sekcji „Konfiguracja usługi SQL” w niniejszym przewodniku.                                                                                                                                                      |

### Krok 1. Skonfigurowanie serwera SQL na potrzeby początkowego ładowania danych
<a id="step-1-configure-the-sql-server-for-initial-data-load" class="xliff"></a>
Jeśli planujesz początkowe ładowanie dużej ilości danych, możesz skrócić czas potrzebny do zapełnienia bazy danych, tymczasowo wyłączając wyszukiwanie pełnotekstowe i ponownie włączając je po ukończeniu eksportu w agencie zarządzania programu MIM 2016 (FIM MA).

Aby tymczasowo wyłączyć wyszukiwanie pełnotekstowe:

1.  Uruchom program SQL Server Management Studio.

2.  Wybierz opcję Nowe zapytanie.

3.  Uruchom następujące instrukcje SQL:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING =
MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

Ważne jest zrozumienie wymagań dyskowych dla używanego modelu odzyskiwania serwera SQL. W zależności od harmonogramu tworzenia kopii zapasowych można rozważyć użycie trybu odzyskiwania prostego podczas początkowego ładowania systemu, aby ograniczyć użycie miejsca na dysku, ale należy rozumieć konsekwencje tego wyboru z perspektywy utraty danych.
W przypadku korzystania z trybu odzyskiwania pełnego konieczne jest zarządzanie użyciem dysku przez tworzenie kopii zapasowych, co obejmuje częste wykonywanie kopii zapasowych dziennika transakcji, aby zapobiec wysokiemu użyciu miejsca na dysku.

>[!IMPORTANT]
Niewdrożenie tych procedur może spowodować wysokie użycie miejsca na dysku i doprowadzić do jego wyczerpania. Dodatkowe informacje na ten temat można znaleźć w artykule [Modele odzyskiwania — omówienie](http://go.microsoft.com/fwlink/?LinkID=185370). Dodatkowe informacje zawiera przewodnik [FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (Przewodnik tworzenia kopii zapasowych i przywracania danych programu FIM).

### Krok 2. Zastosowanie minimalnej niezbędnej konfiguracji programu MIM podczas procesu ładowania
<a id="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process" class="xliff"></a>

W procesie ładowania początkowego należy zastosować tylko minimalną konfigurację wymaganą do konfiguracji programu FIM dla reguł zasad zarządzania (reguł MPR) i definicji zestawu. Po zakończeniu ładowania danych należy utworzyć dodatkowe zestawy wymagane dla wdrożenia. Użyj ustawienia Uruchom przy aktualizacji zasad w przepływach pracy akcji, aby wstecznie zastosować te zasady do załadowanych danych.

### Krok 3. Skonfigurowanie i zapełnienie usługi FIM Service przy użyciu danych tożsamości zewnętrznych
<a id="step-3-configure-and-populate-the-fim-service-with-external-identity-data" class="xliff"></a>


W tym punkcie należy wykonać procedury opisane w przewodniku „How do I Synchronize Users from Active Directory

Domain Services to FIM” (Synchronizowanie użytkowników z usługi Active Directory z programem FIM) w celu skonfigurowania i zsynchronizowania systemu z użytkownikami z usługi Active Directory. Jeśli zachodzi potrzeba synchronizacji informacji o grupie, procedury dla tego procesu są opisane w przewodniku „How do I Synchronize Users from Active Directory Domain Services to FIM” (Synchronizowanie użytkowników z usługi Active Directory z programem FIM).

#### Sekwencje synchronizacji i eksportowania
<a id="synchronization-and-export-sequences" class="xliff"></a>

W celu optymalizacji wydajności należy uruchomić eksport po uruchomieniu synchronizacji, które skutkuje dużą liczbą oczekujących operacji eksportu w przestrzeni łącznika.

Następnie należy uruchomić potwierdzający import na agencie zarządzania skojarzonym z tą przestrzenią łącznika. Na przykład gdy trzeba uruchomić profile uruchamiania synchronizacji na kilku agentach zarządzania w ramach początkowego ładowania danych, należy uruchomić eksport z następującym po nim importem zmian po uruchomieniu każdej pojedynczej synchronizacji.

Dla każdego źródłowego agenta zarządzania, który jest częścią cyklu inicjowania, wykonaj następujące kroki:

1.  Pełny import na źródłowym agencie zarządzania.

2.  Pełna synchronizacja na źródłowym agencie zarządzania.

3.  Eksport na wszystkich objętych docelowych agentach zarządzania z przygotowanymi operacjami eksportu.

4.  Import zmian na wszystkich objętych docelowych agentach zarządzania z przygotowanymi operacjami eksportu.

### Krok 4. Zastosowanie pełnej konfiguracji programu MIM
<a id="step-4-apply-your-full-mim-configuration" class="xliff"></a>


Po zakończeniu początkowego ładowania danych należy zastosować pełną konfigurację programu MIM dla danego wdrożenia.

W zależności od scenariuszy może to obejmować tworzenie dodatkowych zestawów, reguł MPR i przepływów pracy. W przypadku zasad wymagających wstecznego zastosowania do wszystkich istniejących obiektów w systemie należy użyć ustawienia Uruchom przy aktualizacji zasad w przepływach pracy akcji, aby wstecznie zastosować te zasady do załadowanych danych.

### Krok 5. Ponowne skonfigurowanie usługi SQL w celu przywrócenia poprzednich ustawień
<a id="step-5-reconfigure-sql-to-previous-settings" class="xliff"></a>


Pamiętaj, aby zmienić ustawienia usługi SQL na jej normalne ustawienia. Obejmuje to następujące działania:

-   Włączenie wyszukiwania pełnotekstowego

-   Zaktualizowanie zasad tworzenia kopii zapasowej zgodnie z zasadami organizacji

Po zakończeniu początkowego ładowania danych należy ponownie włączyć wyszukiwanie pełnotekstowe. Uruchom następujące instrukcje SQL, aby ponownie włączyć wyszukiwanie pełnotekstowe:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Jeśli musisz przełączyć się do trybu odzyskiwania prostego, upewnij się, że harmonogram tworzenia kopii zapasowych został ponownie skonfigurowany zgodnie z zasadami tworzenia kopii zapasowych w organizacji. Dodatkowe szczegóły harmonogramów tworzenia kopii zapasowych programu FIM są dostępne w przewodniku [FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (Przewodnik tworzenia kopii zapasowych i przywracania danych programu FIM2010).

## Migracja konfiguracji
<a id="configuration-migration" class="xliff"></a>


### Unikanie zmieniania nazw wyświetlanych
<a id="avoid-changing-display-names" class="xliff"></a>

Dla wielu typów obiektów, takich jak reguły MPR, skrypt syncproduction.ps1 używa nazwy wyświetlanej jako jedynego atrybutu zakotwiczenia między dwoma systemami. W związku z tym zmiana nazwy wyświetlanej istniejącej reguły MPR spowoduje usunięcie istniejącej reguły MPR, a następnie utworzenie nowej reguły MPR. Dzieje się tak, ponieważ proces migracji nie może pomyślnie dołączyć reguł MPR, których kryteria dołączania zostały zmienione. Aby uniknąć tego problemu, można powiązać atrybut niestandardowy ze wszystkimi typami obiektów konfiguracji i używać tego atrybutu jako kryteriów dołączania. Dzięki temu można modyfikować nazwy wyświetlane bez wpływu na proces migracji.

### Unikanie zmiany zawartości plików pośrednich
<a id="avoid-changing-the-content-of-intermediate-files" class="xliff"></a>

Format pliku i interfejs programowania aplikacji (interfejs API) obiektów niskiego poziomu są publiczne i manipulacje są obsługiwane przez deweloperów, ale nie zaleca się zmiany zawartości formatów pośrednich podczas migracji. Może być jednak konieczne usunięcie całej klasy ImportObjects z pliku changes.xml lub wykonanie operacji znajdź i zamień na pliku pilot.xml, aby zastąpić numery wersji lub informacje o wersji pilotażowej systemu DNS (Domain Name System) informacjami o wersji produkcyjnej systemu DNS.

### Upewnianie się, że numer wersji jest poprawny w pliku pilot.xml podczas migrowania między różnymi wersjami
<a id="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions" class="xliff"></a>

Chociaż migracje między numerami wersji nie są zalecane ani obsługiwane, często można to zrobić przez zastąpienie numeru wersji pilotażowej numerem wersji produkcyjnej w pliku pilot.xml. W szczególności obiekty WorkflowDefinition i

ActivityInformationConfiguration wymagają numeru wersji, aby precyzyjnie odwoływać się do działań przepływu pracy w środowisku produkcyjnym. Niewykonanie zastąpienia numeru wersji powoduje, że polecenie cmdlet Compare-FIMConfig identyfikuje różnice między atrybutami języka Extensible Object Markup Language (XOML) w obiektach WorkflowDefinition i migruje numer wersji pilotażowej. Usługa FIM Service wersji produkcyjnej może nie być w stanie uruchomić działań przepływu pracy z niepoprawnym numerem wersji.

### Unikanie odwołań cyklicznych
<a id="avoid-cyclic-references" class="xliff"></a>

Ogólnie rzecz biorąc, odwołania cykliczne w konfiguracji programu MIM nie są zalecane.
Jednak cykle czasami występują, gdy zestaw A odwołuje się do zestawu B, a zestaw B odwołuje się do zestawu A. Aby uniknąć problemów z odwołaniami cyklicznymi, należy zmienić definicję zestawu A lub zestawu B tak, aby nie odwoływały się do siebie nawzajem. Następnie należy ponownie uruchomić proces migracji. Jeśli masz odwołania cykliczne i w rezultacie polecenie cmdlet Compare-FIMConfig zwraca błąd, konieczne jest przerwanie cyklu ręcznie. Ponieważ polecenie cmdlet Compare-FIMConfig wyświetla listę zmian uporządkowaną według pierwszeństwa, wymaga to, aby wśród odwołań obiektów konfiguracji nie istniały żadne odwołania cykliczne.

## Zabezpieczenia
<a id="security" class="xliff"></a>

### Konto agenta MIM MA
<a id="mim-ma-account" class="xliff"></a>

Konto agenta MIM MA nie jest traktowane jako konto usługi i powinno być kontem zwykłego użytkownika. Konto musi być w stanie zalogować się lokalnie, aby konto usługi FIM Synchronization Service mogło dokonać jego personifikacji.

Aby włączyć konto agenta MIM MA w celu umożliwienia logowania lokalnego

1.  Kliknij przycisk Start, kliknij pozycję Narzędzia administracyjne, a następnie kliknij program Zasady zabezpieczeń lokalnych.

2.  Otwórz węzeł Zasady lokalne, a następnie kliknij pozycję Przypisywanie praw użytkownika.

3.  W zasadach Zezwalaj na logowanie lokalne upewnij się, że konto agenta FIM MA jest jawnie określone lub dodaj je do jednej z grup, którym już przyznano dostęp.

### Konta usług FIM Synchronization Service i FIM Service
<a id="fim-synchronization-service-and-fim-services-accounts" class="xliff"></a>

Aby skonfigurować w bezpieczny sposób serwery, na których są uruchamiane składniki serwera programu MIM, konta usług powinny być ograniczone. Używając poprzedniej procedury do włączenia konta agenta MIM MA, ustaw następujące ograniczenia dla kont usług FIM Synchronization Service i FIM Service:

-   Odmowa logowania w trybie wsadowym

-   Odmowa logowania lokalnego

-   Odmowa dostępu do tego komputera z sieci

Konta usług nie powinny być członkiem lokalnej grupy administratorów.

Konto usługi FIM Synchronization Service nie powinno być członkiem grupy zabezpieczeń używanej do kontrolowania dostępu do usługi FIM Synchronization Service (grupy rozpoczynające się od FIMSync, na przykład FIMSyncAdmins itd.).

>[!IMPORTANT]
 Jeśli wybrano opcję w celu używania tego samego konta dla obu kont usług, a usługi FIM Service i FIM Synchronization Service są oddzielone, nie można włączyć ustawienia „Odmowa dostępu do tego komputera z sieci” na serwerze usługi synchronizacji mms. Odmowa dostępu uniemożliwi usłudze FIM Service kontaktowanie się z usługą FIM Synchronization Service w celu zmiany konfiguracji i zarządzania hasłami.

### Resetowanie haseł wdrożone na komputerach typu kiosk powinno ustawiać zabezpieczenia lokalne w celu czyszczenia pliku stronicowania pamięci wirtualnej
<a id="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile" class="xliff"></a>

W przypadku wdrażania resetowania haseł programu FIM na stacji roboczej, która ma być kioskiem, zaleca się włączenie ustawienia zasad zabezpieczeń lokalnych „Zamknięcie: wyczyść plik stronicowania pamięci wirtualnej” w celu zapewnienia, że poufne informacje z pamięci procesu nie będą dostępne dla nieautoryzowanych użytkowników.

### Implementowanie protokołu SSL dla portalu programu FIM
<a id="implementing-ssl-for-the-fim-portal" class="xliff"></a>

Zdecydowanie zaleca się używanie protokołu Secure Sockets Layer (SSL) na serwerze portalu programu FIM do zabezpieczenia ruchu między klientami a serwerem.

Aby zaimplementować protokół SSL:

1.  Na serwerze portalu programu MIM otwórz Menedżera usług IIS.

2.  Kliknij nazwę komputera lokalnego.

3.  Kliknij pozycję Certyfikaty serwera.

4.  Kliknij pozycję Utwórz żądanie certyfikatu.

5.  W polu tekstowym Nazwa pospolita wprowadź nazwę serwera.

6.  Kliknij przycisk Dalej, a następnie kliknij przycisk Dalej.

7.  Zapisz plik w dowolnej lokalizacji. Musisz mieć dostęp do tej lokalizacji w kolejnych krokach.

8.  W programie Windows Internet Explorer® przejdź do strony https://nazwa_serwera/certsrv. Zastąp element nazwa_serwera nazwą serwera wystawiającego certyfikat.

9.  Kliknij opcję Żądaj nowego certyfikatu.

10. Kliknij opcję Prześlij żądanie zaawansowane.

11. Kliknij opcję Prześlij żądanie certyfikatu, używając pliku szyfrowanego algorytmem base-64.

12. Wklej zawartość pliku, który został zapisany w poprzednim kroku.

13. W polu Szablon certyfikatu wybierz Serwer sieci Web.

14. Kliknij przycisk Prześlij.

15. Zapisz certyfikat na pulpicie.

16. W Menedżerze usług IIS kliknij pozycję Zakończone żądanie certyfikacji.

17. Wskaż Menedżerowi usług IIS certyfikat właśnie zapisany na pulpicie.

18. Jako przyjazną nazwę wpisz nazwę serwera.

19. Kliknij pozycję Witryny, a następnie wybierz pozycję SharePoint — 80.

20. Kliknij pozycję Powiązania, a następnie kliknij przycisk Dodaj.

21. Wybierz opcję https.

22. Jako certyfikat wybierz ten, który ma taką samą nazwę jak serwer (jest to certyfikat, który właśnie został zaimportowany).

23. Kliknij przycisk OK.

24. Usuń powiązanie HTTP.

25. Kliknij pozycję Ustawienia protokołu SSL, a następnie zaznacz opcję Wymagaj protokołu SSL.

26. Zapisz ustawienia.

27. Kliknij przycisk Start, kliknij pozycję Narzędzia administracyjne, a następnie kliknij pozycję Administracja centralna programu SharePoint 3.0.

28. Kliknij pozycję Operacje, a następnie kliknij opcję Mapowania dostępu alternatywnego.

29. Kliknij pozycję http://nazwa_serwera.

30. Zmień ciąg http://nazwa_serwera na https://nazwa_serwera, a następnie kliknij przycisk OK.

31. Kliknij przycisk Start, kliknij pozycję Uruchom, wpisz polecenie iisreset, a następnie kliknij przycisk OK.

## Wydajność
<a id="performance" class="xliff"></a>

Dla uzyskania optymalnej wydajności konfiguracji:

-   Zastosuj najlepsze rozwiązania dotyczące konfiguracji usługi SQL opisane w sekcji „Konfiguracja usługi SQL” w tym dokumencie.

-   Wyłącz indeksowanie programu SharePoint w witrynie portalu programu FIM 2010 R2. Aby uzyskać więcej informacji, zobacz sekcję „Konfiguracja usługi SQL” w tym dokumencie.

## Najlepsze rozwiązania specyficzne dla funkcji (chcę to usunąć i zwinąć tę sekcję, aby mieć tylko określone funkcje na poziomie nagłówka 2 i 3)
<a id="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3" class="xliff"></a>


### Zarządzanie żądaniami
<a id="request-management" class="xliff"></a>

Domyślnie program MIM 2016 czyści obiekty systemowe, które utraciły ważność, w tym zakończone żądania wraz ze skojarzonymi zatwierdzeniami, odpowiedzi na zatwierdzenia i wystąpienia przepływów pracy, po upływie 30 dni. Jeśli Twoja organizacja potrzebuje dłuższej historii żądań, należy wyeksportować żądania z programu MIM i przechowywać je w pomocniczej bazie danych, aby zachować je przez okres dłuższy niż 30 dni. Chociaż 30-dniowe okno usuwania żądań można skonfigurować, wydłużenie tego okna może niekorzystnie wpłynąć na wydajność ze względu na dodatkowe obiekty w systemie.

### Reguły zasad zarządzania
<a id="management-policy-rules" class="xliff"></a>

#### Należy używać odpowiedniego typu reguł MPR
<a id="use-the-appropriate-mpr-type" class="xliff"></a>

Program MIM udostępnia dwa typy reguł MPR: żądania i przejścia między zestawami.

-   Reguły MPR żądania (RMPR)

 - Służą do definiowania zasad kontroli dostępu (uwierzytelniania, autoryzacji i akcji) dla operacji tworzenia, odczytu, aktualizacji lub usuwania (CRUD) w odniesieniu do zasobów.
 - Stosowane podczas wydawania operacji CRUD w odniesieniu do zasobu docelowego w programie FIM.
   - Zakres określony przez kryteria dopasowania zdefiniowane w regule, tj. do których żądań CRUD reguła będzie stosowana.


-   Reguły MPR przejścia między zestawami (TMPR)
 - Służą do definiowania zasad niezależnie od tego, jak obiekt wszedł w bieżący stan reprezentowany przez zestaw przejść. Reguły TMPR służą do modelowania zasad uprawnień.
 - Stosowane, kiedy zasób wchodzi do skojarzonego zestawu lub go opuszcza.
 - Zakres ograniczony do elementów członkowskich zestawu.

>[UWAGA] Aby uzyskać więcej informacji, zobacz [Projektowanie reguł dotyczących zasad firmowych](http://go.microsoft.com/fwlink/?LinkID=183691).

#### Reguły MPR należy włączać tylko w razie potrzeby
<a id="only-enable-mprs-as-necessary" class="xliff"></a>

Podczas stosowania konfiguracji należy stosować zasadę najmniejszych uprawnień. Reguły MPR kontrolują zasady dostępu do wdrożenia programu FIM. Należy włączyć tylko funkcje używane przez większość użytkowników. Na przykład nie wszyscy użytkownicy używają programu FIM do zarządzania grupą, a więc skojarzone reguły MPR zarządzania grupami powinny być wyłączone. Domyślnie program FIM jest dostarczany z wyłączoną większością uprawnień użytkowników niebędących administratorami.

#### Zamiast bezpośrednio modyfikować wbudowane reguły MPR, należy je duplikować
<a id="duplicate-built-in-mprs-instead-of-directly-modifying" class="xliff"></a>

Jeśli zachodzi potrzeba zmodyfikowania wbudowanych reguł MPR, należy utworzyć nową regułę MPR z wymaganą konfiguracją i wyłączyć wbudowaną regułę MPR. Daje to gwarancję, że wszelkie przyszłe zmiany wbudowanych reguł MPR wprowadzane przez proces uaktualniania nie wpłyną niekorzystnie na konfigurację systemu.

#### Uprawnienia użytkowników końcowych powinny korzystać z jawnych list atrybutów dostosowanych do potrzeb biznesowych użytkowników
<a id="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs" class="xliff"></a>

Używanie jawnych list atrybutów pomaga zapobiec przypadkowemu przyznaniu uprawnień nieuprzywilejowanym użytkownikom podczas dodawania atrybutów do obiektów.
Administratorzy powinni jawnie udzielić dostępu do nowych atrybutów zamiast próbować usunąć dostęp.

Dostęp do danych powinien być ograniczony do potrzeb biznesowych użytkownika. Na przykład członkowie grupy nie powinni mieć dostępu do atrybutu filtru grupy, której są członkami. Filtr może nieodwracalnie ujawnić dane organizacji, do których użytkownik normalnie nie miałby dostępu.

#### Reguły MPR powinny odzwierciedlać czynne uprawnienia w systemie
<a id="mprs-should-reflect-effective-permissions-in-the-system" class="xliff"></a>

Należy unikać nadawania uprawnień do atrybutów, których użytkownik nigdy nie może używać. Na przykład nie należy przyznawać uprawnienia do modyfikowania podstawowych atrybutów zasobu, takich jak objectType. Pomimo reguły MPR każda próba zmodyfikowania typu zasobu po jego utworzeniu zostanie odrzucona przez system.

#### Uprawnienia Odczyt powinny być oddzielone od uprawnień Modyfikowanie i Tworzenie w przypadku używania jawnych atrybutów w regułach MPR
<a id="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs" class="xliff"></a>

W przypadku jawnego określania atrybutów w regułach MPR atrybuty wymagane dla uprawnień Tworzenie i Modyfikowanie są zazwyczaj inne niż te, które są dostępne dla uprawnień Odczyt. Na przykład uprawnienia Odczyt mogą być przyznawane względem atrybutów systemowych, takich jak Creator lub objectId, podczas gdy uprawnień Tworzenie lub Modyfikowanie nie można określić dla atrybutów systemowych.

#### Uprawnienia Tworzenie powinny być oddzielone od uprawnień Modyfikowanie w przypadku używania jawnych atrybutów w regułach MPR
<a id="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules" class="xliff"></a>

Operacja tworzenia wymaga, aby użytkownik wybrał atrybut objectType jako część operacji. Jest to podstawowy atrybut systemowy, którego nie można zmodyfikować po operacji tworzenia.

#### Należy używać jednej reguły MPR żądania dla wszystkich atrybutów z takimi samymi wymaganiami dotyczącymi dostępu
<a id="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements" class="xliff"></a>

W przypadku atrybutów z takimi samymi wymaganiami dotyczącymi dostępu, które nie powinny się zmienić, można połączyć je w pojedynczej regule MPR żądania w celu zwiększenia wydajności.

#### Należy unikać przyznawania nieograniczonego dostępu nawet wybranym grupom podmiotu zabezpieczeń
<a id="avoid-giving-unrestricted-access-even-to-selected-principal-groups" class="xliff"></a>

W programie FIM uprawnienia są definiowane jako pozytywne potwierdzenie. Ponieważ program FIM nie obsługuje odmowy uprawnień, przydzielenie nieograniczonego dostępu do zasobu komplikuje określanie jakichkolwiek wykluczeń w uprawnieniach. Najlepszym rozwiązaniem jest przyznanie tylko niezbędnych uprawnień.

>[!NOTE]
Sekcja dotycząca uprawnień znajduje się poniżej. Zastanawiam się, jak je scalić, aby uniknąć tworzenia nagłówków poziomu 5
#### Niestandardowe uprawnienia należy definiować przy użyciu reguł TMPR
<a id="use-tmprs-to-define-custom-entitlements" class="xliff"></a>

Niestandardowe uprawnienia należy zdefiniować, używając reguł MPR przejścia między zestawami (TMPR) zamiast reguł RMPR.
Reguły TMPR udostępniają model oparty na stanach w celu przypisywania lub usuwania uprawnień na podstawie członkostwa w określonych zestawach przejść (lub rolach) i towarzyszących działań przepływu pracy. Reguły TMPR powinny być zawsze definiowane w parach: jedna dla zasobów przechodzących do i jedna dla zasobów wychodzących z zestawu. Ponadto każda reguła MPR przejścia powinna zawierać oddzielne przepływy pracy dla działań aprowizacji i cofania aprowizacji.

>[!NOTE]
Każdy przepływ pracy cofania aprowizacji powinien zapewniać, że atrybut Uruchom przy aktualizacji zasad ma wartość true.

#### Regułę MPR przejścia między zestawami należy włączyć jako ostatnią
<a id="enable-the-set-transition-in-mpr-last" class="xliff"></a>

Podczas tworzenia pary reguł TMPR regułę MPR przejścia między zestawami należy włączyć jako ostatnią. Ta kolejność gwarantuje, że żaden zasób nie pozostanie z uprawnieniem, jeśli zostanie dodany do zestawu i usunięty z niego już po włączeniu reguły MPR przejścia do zestawu, ale przed włączeniem reguły MPR przejścia z zestawu.

#### Przepływy pracy w regule TMPR powinny najpierw sprawdzać stan zasobu docelowego
<a id="workflows-in-tmpr-should-check-target-resource-state-first" class="xliff"></a>

Przepływy pracy aprowizacji powinny najpierw sprawdzić, czy zasób docelowy został już aprowizowany zgodnie z uprawnieniem. Jeśli tak, wówczas nie powinny nic robić.

Przepływy pracy cofania aprowizacji powinny najpierw sprawdzić, czy zasób docelowy został aprowizowany. Jeśli tak, powinny cofnąć aprowizację zasobu docelowego.
W przeciwnym razie nie powinny nic robić.

#### Należy wybrać ustawienie Uruchom przy aktualizacji zasad dla reguł TMPR
<a id="select-run-on-policy-update-for-tmprs" class="xliff"></a>

Zapewnia to stosowanie prawidłowego zachowania aprowizacji podczas implementowania aktualizacji zasad i użycie flagi Uruchom przy aktualizacji zasad w przepływach pracy akcji skojarzonych z regułami TMPR. Daje to gwarancję, że zmiany w definicjach zasad będą stosowały przepływy pracy akcji do nowych elementów członkowskich zestawu przejść.

#### Należy unikać kojarzenia tego samego uprawnienia z dwoma różnymi zestawami przejść
<a id="avoid-associating-the-same-entitlement-with-two-different-transition-sets" class="xliff"></a>

Skojarzenie tego samego uprawnienia z dwoma różnymi zestawami przejść może spowodować niepotrzebne odwoływanie i ponownie przyznawanie uprawnień, jeśli zasób zostanie przeniesiony z jednego zestawu do drugiego. Najlepszym rozwiązaniem jest upewnienie się, że jeden zestaw zawiera wszystkie zasoby, które wymagają skojarzonego uprawnienia. Gwarantuje to relację jeden do jednego między zestawem przejść a uprawnieniem zapewniającym przepływ pracy.

#### Należy użyć odpowiedniej sekwencji operacji podczas usuwania uprawnień w systemie
<a id="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system" class="xliff"></a>

W zależności od kolejności kroków wykonywanych podczas usuwania uprawnień w systemie można uzyskać dwa różne wyniki operacyjne tego działania. Upewnij się, że rozumiesz, która kolejność jest właściwa do osiągnięcia pożądanego efektu.

Aby usunąć uprawnienie z systemu (i odwołać je ze wszystkich elementów członkowskich obecnie posiadających to uprawnienie):

1.  Wyłącz regułę MPR T-In (Przejście do). Zapobiega to nowym przyznaniom.

2.  Usuń filtr zestawu przejść lub zmień go tak, aby zestaw był pusty. To spowoduje przejście wszystkich elementów członkowskich poza zestaw (przejście z) i zastosowanie zasad przejścia z zestawu, w tym skonfigurowanych przepływów pracy anulowania aprowizacji skojarzonych z uprawnieniem.

3.  Wyłącz regułę MPR T-Out (Przejście z).

Aby usunąć uprawnienie, ale pozostawić bieżące elementy członkowskie (na przykład zatrzymać zarządzanie uprawnieniem za pomocą programu FIM):

1.  Wyłącz regułę MPR T-In (Przejście do). Zapobiega to nowym przyznaniom.

2.  Wyłącz regułę MPR T-Out (Przejście z).

3.  Usuń filtr zestawu przejść lub zmień go tak, aby zestaw był pusty. Ponieważ zestaw nie jest już powiązany z regułą TMPR, nie będą stosowane żadne przepływy pracy anulowania aprowizacji.

### Zestawy
<a id="sets" class="xliff"></a>

Podczas stosowania najlepszych rozwiązań dla zestawów należy wziąć pod uwagę wpływ optymalizacji na możliwości zarządzania i łatwość administrowania w przyszłości.
Przed zastosowaniem tych zaleceń należy przeprowadzić odpowiednie testy przy oczekiwanej skali produkcyjnej, aby znaleźć równowagę między wydajnością i możliwościami zarządzania.

>[!NOTE]
Wszystkie poniższe wskazówki dotyczą zestawów dynamicznych i grup dynamicznych.


#### Należy zminimalizować użycie zagnieżdżania dynamicznego
<a id="minimize-the-use-of-dynamic-nesting" class="xliff"></a>

Odnosi się to do filtru zestawu odwołującego się do atrybutu ComputedMember innego zestawu. Typowym powodem zagnieżdżania zestawów jest unikanie duplikowania warunku członkostwa w wielu zestawach. Chociaż takie podejście może prowadzić do poprawy możliwości zarządzania zestawami, dzieje się to kosztem wydajności. Optymalizację pod kątem wydajności można uzyskać, duplikując warunki członkostwa zagnieżdżonego zestawu zamiast zagnieżdżania samego zestawu.

W niektórych przypadkach, aby zaspokoić wymagania funkcjonalne, nie można uniknąć zagnieżdżania zestawów. Są to podstawowe sytuacje, w przypadku których należy zagnieżdżać zestawy. Na przykład aby zdefiniować zestaw wszystkich grup, których właścicielami nie są pełnoetatowi pracownicy, konieczne jest użycie zagnieżdżania zestawów w następujący sposób: `/Group[not(Owner =
/Set[ObjectID = ‘X’]/ComputedMember]`, gdzie „X” to atrybut ObjectID zestawu wszystkich pełnoetatowych pracowników.

#### Należy zminimalizować użycie warunków negatywnych
<a id="minimize-the-use-of-negative-conditions" class="xliff"></a>

Warunki negatywne to warunki członkostwa, które korzystają z następujących operatorów lub funkcji: `!=`, `not()`, `\<`, `\<=`. W celu optymalizacji pod kątem wydajności należy tam, gdzie to możliwe, wyrazić żądany warunek za pomocą wielu warunków pozytywnych, a nie jako warunek negatywny.

#### Należy zminimalizować użycie warunków członkostwa opartych na wielowartościowych atrybutach odwołania
<a id="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes" class="xliff"></a>

Należy zminimalizować użycie warunków opartych na wielowartościowych atrybutach odwołania, ponieważ duża liczba tych zestawów może mieć wpływ na wydajność operacji na atrybucie użytym w warunku członkostwa.

### Resetowanie haseł
<a id="password-reset" class="xliff"></a>

#### Komputery typu kiosk używane do resetowania haseł powinny ustawiać zabezpieczenia lokalne w celu czyszczenia pliku stronicowania pamięci wirtualnej
<a id="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile" class="xliff"></a>

W przypadku wdrażania resetowania haseł programu FIM 2010 na stacji roboczej, która ma być kioskiem, zaleca się włączenie ustawienia zasad zabezpieczeń lokalnych „Zamknięcie: wyczyść plik stronicowania pamięci wirtualnej” w celu zapewnienia, że poufne informacje z pamięci procesu nie będą dostępne dla nieautoryzowanych użytkowników.

#### Użytkownicy powinni zawsze rejestrować się w celu resetowania haseł na komputerze, na którym są zalogowani
<a id="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to" class="xliff"></a>

Gdy użytkownik próbuje zarejestrować się w celu zresetowania hasła za pośrednictwem portalu sieci Web, program FIM 2010 zawsze inicjuje rejestrację w imieniu zalogowanego użytkownika, niezależnie od tego, kto jest zalogowany w witrynie sieci Web. Użytkownicy powinni zawsze rejestrować się w celu resetowania haseł na komputerze, na którym są zalogowani.

#### Nie należy ustawiać klucza rejestru AvoidPdcOnWan na wartość true
<a id="do-not-set-the-avoidpdconwan-registry-key-to-true" class="xliff"></a>

W przypadku korzystania z resetowania haseł programu MIM 2016 nie należy ustawiać klucza rejestru AvoidPdcOnWan na wartość true.

Jeśli ten klucz rejestru jest ustawiony na wartość true, użytkownik najprawdopodobniej przejdzie przez bramy hasła, zresetuje hasło na podstawowym kontrolerze domeny (PDC) i spróbuje się zalogować. Z powodu tego klucza rejestru lokalny kontroler domeny nie wykonuje dodatkowej weryfikacji z użyciem podstawowego kontrolera domeny, w związku z tym odmawia żądania logowania. Jeśli użytkownik otrzyma odmowę kilka razy, jego dostęp do domeny może zostać zablokowany i będzie musiał skontaktować się z działem pomocy technicznej.

#### Nie należy włączać rejestrowania haseł w postaci zwykłego tekstu
<a id="do-not-turn-on-logging-of-clear-text-passwords" class="xliff"></a>

Istnieje możliwość rejestrowania haseł w postaci zwykłego tekstu podczas włączania śledzenia diagnostycznego poziomu usługi w usłudze Windows

Communication Foundation (WCF). Ta opcja nie jest domyślnie włączona i odradza się włączanie jej w środowiskach produkcyjnych. Te hasła są widoczne jako elementy zwykłego tekstu w obrębie zaszyfrowanego komunikatu protokołu SOAP (Simple Object Access Protocol), kiedy użytkownik rejestruje się w celu resetowania haseł. Aby uzyskać więcej informacji, zobacz [Konfigurowanie rejestrowania komunikatów](http://go.microsoft.com/fwlink/?LinkID=168572).

#### Nie należy mapować przepływu pracy autoryzacji na proces resetowania hasła
<a id="do-not-map-an-authorization-workflow-to-the-password-reset-process" class="xliff"></a>

Nie należy dołączać przepływu pracy autoryzacji do operacji resetowania hasła.
Resetowanie hasła wymaga synchronicznej odpowiedzi, a przepływy pracy autoryzacji, które zawierają działania takie jak działanie zatwierdzania, są asynchroniczne.

#### Nie należy mapować wielu działań akcji na proces resetowania hasła
<a id="do-not-map-multiple-action-activities-to-password-reset" class="xliff"></a>

Nie należy dołączać przepływu pracy zawierającego więcej niż jedno działanie akcji do operacji resetowania hasła. Przykładowym scenariuszem jest dołączanie drugiego działania resetowania hasła usług AD DS do reguły MPR resetowania hasła. Ten scenariusz nie jest obsługiwany.

#### Należy wymagać ponownej rejestracji podczas dodawania, usuwania lub zmiany kolejności działań w istniejącym przepływie pracy
<a id="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow" class="xliff"></a>

Podczas dodawania, usuwania lub zmiany kolejności działań uwierzytelniania w istniejącym przepływie pracy należy zawsze wybrać opcję wymagania ponownej rejestracji. Użytkownicy, którzy spróbują uwierzytelnić się w funkcji resetowania haseł po tym, jak działanie zostało dodane do lub usunięte z przepływu pracy, ale przed ponownym zarejestrowaniem się, mogą napotkać niepożądane skutki.

### Konfiguracja portalu i konfiguracja ekranu kontroli zasobów
<a id="portal-configuration-and-resource-control-display-configuration" class="xliff"></a>

#### Należy rozważyć dodanie zastrzeżenia dotyczącego prywatności do strony profilu użytkownika
<a id="consider-adding-a-privacy-disclaimer-to-the-user-profile-page" class="xliff"></a>

W programie MIM domyślnie niektóre informacje o profilu użytkownika mogą być widoczne dla innych użytkowników. Jako informację dla użytkowników administratorzy powinni rozważyć dodanie niestandardowego tekstu spójnego z zasadami firmy do strony profilu użytkownika. Aby uzyskać więcej informacji na temat dodawania niestandardowego tekstu do strony portalu programu MIM, zobacz artykuł [Introduction to Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848) (Wprowadzenie do konfigurowania i dostosowywania portalu programu FIM).

### Schemat
<a id="schema" class="xliff"></a>

#### Nie należy usuwać typów zasobów Osoba i Grupa
<a id="do-not-delete-person-or-group-resource-types" class="xliff"></a>

Chociaż typy zasobów Osoba i Grupa nie są oznaczane jako podstawowe typy zasobów, same zasoby ani atrybuty do nich przypisane nie powinny być usuwane. Interfejs użytkownika w portalu programu MIM wymaga, aby typy zasobów Osoba i Grupa oraz ich atrybuty były obecne.

#### Nie należy modyfikować podstawowych atrybutów
<a id="do-not-modify-the-core-attributes" class="xliff"></a>

Istnieje 13 podstawowych atrybutów przypisanych do wszystkich typów zasobów. Nie należy w żaden sposób modyfikować ich relacji z dowolnym typem zasobów. 13 podstawowych atrybutów to:

-   CreatedTime

-   Creator

-   DeletedTime

-   Description

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Locale

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Nie należy usuwać zasobu schematu z zależnością od wymagań inspekcji

Nie należy usuwać zasobów schematu, gdy nadal istnieją wymagania inspekcji dotyczące tych zasobów.

#### Ignorowanie wielkości liter w wyrażeniach regularnych
<a id="making-regular-expressions-case-insensitive" class="xliff"></a>

W programie FIM przydatne może być ignorowanie wielkości liter w niektórych wyrażeniach regularnych. Aby ignorować wielkość liter w obrębie grupy, można użyć ciągu ?!:. Na przykład dla typu pracownika użyj wyrażenia

`\^(?!:contractor\|full time employee)%.`

#### Obliczanie atrybutu elementu członkowskiego
<a id="calculation-of-the-member-attribute" class="xliff"></a>

Atrybut elementu członkowskiego udostępniany dla aparatu synchronizacji jest faktycznie mapowany na atrybut ComputedMembers. Jest to kombinacja elementów członkowskich opartych na kryteriach i ręcznie wybranych elementów członkowskich. Nawet w przypadku dodania wszystkich trzech atrybutów (Filter, ExplicitMembers i ComputedMembers) dynamiczne obliczanie atrybutu elementu członkowskiego nie wystąpi dla typów zasobów innych niż grupa i zestaw.

#### Początkowe i końcowe spacje w ciągach są ignorowane
<a id="leading-and-trailing-spaces-in-strings-are-ignored" class="xliff"></a>

W programie FIM można wprowadzać ciągi zawierające spacje wiodące i końcowe, ale system FIM ignoruje te spacje. Jeśli prześlesz ciąg zawierający spacje na początku i na końcu, aparat synchronizacji i usługi sieci Web zignorują te spacje.

#### Puste ciągi nie są równe wartości null
<a id="empty-strings-do-not-equal-null" class="xliff"></a>

Puste ciągi nie są równe wartości null w tej wersji programu FIM. Pusty ciąg wejściowy jest traktowany jako prawidłowa wartość. Nieobecny jest traktowany jako wartość null.

### Przetwarzanie przepływów pracy i żądań
<a id="workflow-and-request-processing" class="xliff"></a>

#### Nie należy usuwać domyślnych przepływów pracy, które są dostarczane z programem MIM 2016
<a id="do-not-delete-default-workflows-that-are-shipped-with-mim-2016" class="xliff"></a>

Następujące przepływy pracy są dostarczane z programem FIM 2010 i nie należy ich usuwać:

-   Expiration Workflow (przepływ pracy wygaśnięcia)

-   Filter Validation Workflow for Administrators (przepływ pracy weryfikacji filtru dla administratorów)

-   Filter Validation Workflow for Non-Administrators (przepływ pracy weryfikacji filtru dla użytkowników niebędących administratorami)

-   Group Expiration Notification Workflow (przepływ pracy powiadomienia o wygaśnięciu grupy)

-   Group Validation Workflow (przepływ pracy weryfikacji grupy)

-   Owner Approval Workflow (przepływ pracy zatwierdzania właściciela)

-   Password Reset Action Workflow (przepływ pracy akcji resetowania hasła)

-   Password Reset AuthN Workflow (przepływ pracy AuthN resetowania hasła)

-   Requestor Validation With Owner Authorization (weryfikacja obiektu żądającego z autoryzacją właściciela)

-   Requestor Validation Without Owner Authorization (weryfikacja obiektu żądającego bez autoryzacji właściciela)

-   System Workflow Required for Registration (przepływ pracy systemu wymagany do rejestracji)

#### Nie należy uruchamiać równolegle dwóch lub większej liczby działań zatwierdzenia
<a id="do-not-run-two-or-more-approvalactivities-in-parallel" class="xliff"></a>

Nie należy uruchamiać równolegle dwóch lub większej liczby działań zatwierdzenia. Może to spowodować utknięcie żądania w fazie autoryzacji. Na potrzeby wielu zatwierdzeń należy dołączyć dłuższą listę osób zatwierdzających w zatwierdzeniu lub ustawić symetryczną sekwencję dwóch działań.

#### Działania autoryzacji nie powinny modyfikować danych zasobów programu MIM
<a id="authorization-activities-should-not-modify-mim-resources-data" class="xliff"></a>

Jako części przepływów pracy w przepływach pracy autoryzacji należy unikać używania działań, które modyfikują zasoby programu MIM, takich jak działanie ewaluatora funkcji. Ponieważ żądanie nie zostało zatwierdzone w punkcie autoryzacji przetwarzania, wszelkie modyfikacje informacji o tożsamości mogą zostać zastosowane pomimo możliwego odrzucenia żądania.

### Informacje o partycjach usługi FIM Service
<a id="understanding-fim-service-partitions" class="xliff"></a>

Celem programu FIM jest przetwarzanie żądań, które mogą być inicjowane przez różnych klientów programu FIM, takich jak usługa FIM Synchronization Service i składniki samoobsługi, zgodnie ze skonfigurowanymi zasadami biznesowymi. Z założenia każde wystąpienie usługi FIM Service należy do grupy logicznej składającej się z co najmniej jednego wystąpienia usługi FIM Service, która jest także znana jako partycja usługi FIM Service. Jeśli masz tylko jedno wystąpienie usługi FIM Service wdrożone do obsługi wszystkich żądań, może to powodować opóźnienia przetwarzania. Niektóre operacje mogą nawet przekraczać domyślne wartości limitu czasu odpowiednie dla operacji samoobsługi. Partycje usługi FIM Service mogą pomóc rozwiązać ten problem. Aby uzyskać dodatkowe informacje, zobacz informacje o partycjach usługi FIM Service.

