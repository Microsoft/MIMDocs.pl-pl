---
title: Planowanie środowiska bastionu | Dokumentacja firmy Microsoft
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aeaf82e6875739cb6ff8ee7b7d96ced55e07adab
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010748"
---
# <a name="planning-a-bastion-environment"></a>Planowanie środowiska bastionu

Dodanie środowiska bastionu z dedykowanym lasem administracyjnym do Active Directory umożliwia organizacjom zarządzanie kontami administracyjnymi, stacjami roboczymi i grupami w środowisku z silniejszymi kontrolami zabezpieczeń niż w istniejącym środowisku produkcyjnym.

> [!NOTE]
> Podejście PAM ze środowiskiem bastionu udostępnionym przez program MIM jest przeznaczone do użycia w niestandardowej architekturze dla izolowanych środowisk, w których dostęp do Internetu nie jest dostępny, gdy ta konfiguracja jest wymagana przez regulację lub w środowiskach izolowanych o dużym wpływie, takich jak laboratoria badawcze w trybie offline i rozłączona technologia kontroli i w środowiskach pozyskiwania danych. Jeśli Active Directory jest częścią środowiska połączonego z Internetem, zapoznaj się z tematem [Zabezpieczanie uprzywilejowanego dostępu](/security/compass/overview) , aby uzyskać więcej informacji na temat lokalizacji do uruchomienia.

Ta architektura umożliwia kontrolki, które nie są możliwe lub łatwo skonfigurowane w architekturze jednego lasu. Obejmuje to aprowizację kont jako standardowych nieuprzywilejowanych użytkowników w lesie administracyjnym, które są wysoce uprzywilejowane w środowisku produkcyjnym, co w większym stopniu zapewnia techniczne wymuszanie ładu. Architektura ta pozwala również korzystać z funkcji uwierzytelniania selektywnego w relacji zaufania jako metody ograniczenia możliwości logowania (i ujawnienia poświadczeń) tylko do grupy autoryzowanych hostów. W sytuacjach, w których od lasu produkcyjnego oczekuje się podniesienia poziomu bezpieczeństwa bez zwiększania kosztu i złożoności projektu związanego z całkowitą przebudową infrastruktury, środowisko lasu administracyjnego może zwiększyć bezpieczeństwo środowiska produkcyjnego.

Oprócz dedykowanego lasu administracyjnego można użyć innych technik. Obejmują one ograniczanie widoczności poświadczeń administracyjnych, ograniczanie uprawnień ról użytkowników w tym lesie i zapewnianie, że zadania administracyjne nie są wykonywane na hostach używanych do standardowych działań użytkowników (na przykład korzystania z poczty e-mail i przeglądania sieci Web).

## <a name="best-practice-considerations"></a>Zagadnienia dotyczące najlepszych rozwiązań

Dedykowany las administracyjny to standardowy las usługi Active Directory w pojedynczej domenie używany do zarządzania usługą Active Directory. Korzyścią z używania lasów i domen administracyjnych jest większa liczba zabezpieczeń niż w przypadku lasów produkcyjnych ze względu na ich ograniczone przypadki użycia. Ponadto ponieważ ten las jest oddzielony i nie ufa istniejącym lasom organizacji, naruszenie bezpieczeństwa w innym lesie nie będzie stanowiło zagrożenia dla tego lasu dedykowanego.

Z projektem lasu administracyjnego są związane następujące kwestie:

### <a name="limited-scope"></a>Ograniczony zakres

Zaletą lasu administracyjnego są wysokie gwarancje bezpieczeństwa i mniejszy obszar narażony na ataki. W lesie mogą znajdować się dodatkowe funkcje zarządzania i aplikacje, ale każde zwiększenie zakresu spowoduje zwiększenie obszaru narażonego na ataki w lesie i jego zasobach. Celem jest ograniczenie funkcji lasu, aby obszar narażony na ataki był jak najmniejszy.

Zgodnie z [modelem warstwy](tier-model-for-partitioning-administrative-privileges.md) partycjonowania uprawnień administracyjnych konta w dedykowanym lesie administracyjnym powinny znajdować się w pojedynczej warstwie, zwykle w warstwie 0 lub 1. W przypadku lasu w warstwie 1 należy rozważyć ograniczenie go do określonego zakresu aplikacji (np. aplikacje finansowe) lub społeczności użytkowników (np. zewnętrzni dostawcy IT).

### <a name="restricted-trust"></a>Ograniczone zaufanie

Produkcyjny las *CORP* powinien ufać administracyjnemu lasowi *PRIV*, ale nie odwrotnie. Zaufaniem może być zaufanie domeny lub zaufanie lasu. Domena lasu administracyjnego nie musi ufać zarządzanym domenom i lasom w celu zarządzania usługą Active Directory, ale dodatkowe aplikacje mogą wymagać dwukierunkowej relacji zaufania, walidacji zabezpieczeń i testowania.

Należy korzystać z uwierzytelniania selektywnego w celu zapewnienia, że konta w lesie administracyjnym używają tylko odpowiednich hostów produkcyjnych. Do obsługi kontrolerów domeny i delegowania uprawnień w usłudze Active Directory zwykle wymaga to przyznania prawa „Zezwolono na logowanie” dla kontrolerów domeny wyznaczonym kontom administracyjnym warstwy 0 w lesie administracyjnym. Zobacz [Konfigurowanie ustawień uwierzytelniania selektywnego](https://technet.microsoft.com/library/cc816580.aspx) , aby uzyskać więcej informacji.

## <a name="maintain-logical-separation"></a>Zachowanie separacji logicznej

Aby zapewnić, że na środowisko bastionu nie będą miały wpływu istniejące lub przyszłe przypadki naruszenia zabezpieczeń w organizacyjnej usłudze Active Directory, podczas przygotowywania systemu do użycia środowiska bastionu należy wziąć pod uwagę następujące wytyczne:

- Serwery z systemem Windows nie powinny być przyłączone do domeny ani wykorzystywać dystrybucji oprogramowania lub ustawień z istniejącego środowiska.

- Środowisko bastionu musi zawierać własne Usługi domenowe Active Directory, zapewniając protokół Kerberos i LDAP, serwer DNS oraz usługi związane z czasem w środowisku bastionu.

- Program MIM nie powinien używać farmy baz danych SQL w istniejącym środowisku. Program SQL Server należy wdrożyć na dedykowanych serwerach w środowisku bastionu.

- Środowisko bastionu wymaga programu Microsoft Identity Manager 2016 — w szczególności muszą zostać wdrożone usługa MIM i składniki PAM.

- Oprogramowanie do tworzenia kopii zapasowej i nośniki kopii zapasowej dla środowiska bastionu muszą być przechowywane oddzielnie od systemów w istniejących lasach, aby administrator w istniejącym lesie nie mógł wykorzystać kopii zapasowej środowiska bastionu.

- Użytkownicy, którzy zarządzają serwerami środowiska bastionu, muszą logować się ze stacji roboczych niedostępnych dla administratorów w istniejącym środowisku, aby nie nastąpił wyciek poświadczeń środowiska bastionu.

## <a name="ensure-availability-of-administration-services"></a>Zapewnianie dostępności usług administracyjnych

Ponieważ administrowanie aplikacjami zostanie przeniesione do środowiska bastionu, rozważ, jak zapewnić wystarczającą dostępność w celu spełnienia wymagań tych aplikacji. Te techniki to m.in.:

- Wdrożenie Usług domenowych Active Directory na wielu komputerach w środowisku bastionu. Co najmniej dwie usługi są niezbędne w celu zapewnienia ciągłego uwierzytelniania nawet wtedy, gdy jeden serwer będzie tymczasowo uruchamiany ponownie w celu wykonania zaplanowanej konserwacji. W celu obsługi większych obciążeń lub zarządzania zasobami i administratorami w wielu regionach geograficznych mogą być wymagane dodatkowe komputery.

- Przygotuj konta w ramach awarii w istniejącym lesie i dedykowanym lesie administracyjnym, które są przeznaczone do celów awaryjnych.

- Wdrożenie programu SQL Server i usługi MIM na wielu komputerach w środowisku bastionu.

- Przechowywanie kopii zapasowej usługi AD i programu SQL dla każdej zmiany definicji użytkowników lub ról w dedykowanym lesie administracyjnym.

## <a name="configure-appropriate-active-directory-permissions"></a>Konfigurowanie odpowiednich uprawnień usługi Active Directory

Las administracyjny musi być skonfigurowany z najmniejszymi uprawnieniami na podstawie wymagań dotyczących administracji usługi Active Directory.

- Kontom znajdującym się w lesie administracyjnym, które są używane do zarządzania środowiskiem produkcyjnym, nie należy przyznawać uprawnień administracyjnych do lasu administracyjnego ani stacji roboczych i domen, które zawiera ten las.

- Uprawnienia administracyjne dotyczące lasu administracyjnego powinny być ściśle kontrolowane przez proces w trybie offline, aby ograniczyć możliwość wymazania dzienników inspekcji przez osobę atakującą lub złośliwe oprogramowanie. Dodatkowa korzyść polega na zapewnieniu, że członkowie personelu z kontami administratora w środowisku produkcyjnym nie mogą złagodzić ograniczeń swoich kont, a przez to zwiększyć zagrożenia bezpieczeństwa dla organizacji.

- Las administracyjny powinien być zgodny z konfiguracją Menedżera zgodności zabezpieczeń firmy Microsoft dla domeny, w tym również z rygorystyczną konfiguracją protokołów uwierzytelniania.

Podczas tworzenia środowiska bastionu przed zainstalowaniem programu Microsoft Identity Manager należy określić i utworzyć konta, które będą używane na potrzeby administracyjne w ramach tego środowiska. Dotyczy to następujących kont:

- **Konta awaryjne** powinny mieć możliwość logowania się tylko do kontrolerów domeny w środowisku bastionu.

- **Administratorzy z czerwonymi kartami** aprowizują inne konta i przeprowadzają niezaplanowaną konserwację. Tym kontom nie jest przyznawany jakikolwiek dostęp do istniejących lasów ani systemów znajdujących się poza środowiskiem bastionu. Poświadczenia, np. karta inteligentna, powinny być fizycznie zabezpieczone i należy zalogować się przy użyciu tych kont.

- **Konta usług** wymagane przez program Microsoft Identity Manager, program SQL Server i inne oprogramowanie.

## <a name="harden-the-hosts"></a>Wzmacnianie zabezpieczeń hostów

Wszystkie hosty, w tym kontrolery domeny, serwery i stacje robocze przyłączone do lasu administracyjnego, powinny mieć zainstalowane najnowsze systemy operacyjne i dodatki Service Pack oraz być zawsze aktualne.

- Aplikacje wymagane do wykonywania czynności administracyjnych należy wstępnie zainstalować na stacjach roboczych, aby konta ich używające nie musiały należeć do grupy administratorów lokalnych w celu zainstalowania tych aplikacji. Konserwacja kontrolera domeny może być zwykle przeprowadzona przy użyciu protokołu RDP i narzędzi administracji zdalnej serwera.

- Hosty w lesie administracyjnym powinny być skonfigurowane pod kątem automatycznej instalacji aktualizacji zabezpieczeń. Może to powodować ryzyko przerywania operacji konserwacji kontrolera domeny, ale znacząco zmniejsza zagrożenie bezpieczeństwa związane z brakiem poprawek usuwających luki.

### <a name="identify-administrative-hosts"></a>Identyfikowanie hostów administracyjnych

Ryzyko dotyczące systemu lub stacji roboczej powinno być mierzone na podstawie działań o najwyższym ryzyku, które odbywają się w danym systemie lub na danej stacji roboczej, takich jak przeglądanie sieci Web, wysyłanie i odbieranie wiadomości e-mail lub korzystanie z innych aplikacji, które przetwarzają nieznaną lub niezaufaną zawartość.

Hosty administracyjne obejmują następujące komputery:

- Komputer stacjonarny, na którym poświadczenia administratora są fizycznie wpisywane lub wprowadzane.

- Administracyjne „serwery przeskoku”, na których są uruchamiane sesje i narzędzia administracyjne.

- Wszystkie hosty, na których są wykonywane działania administracyjne, w tym te, które korzystają ze standardowego pulpitu użytkownika z uruchomionym klientem RDP w celu zdalnego administrowania serwerami i aplikacjami.

- Serwery hostujące aplikacje, które muszą być administrowane i do których nie można uzyskać dostępu za pomocą protokołu RDP z funkcją ograniczonego trybu administratora lub zdalnej obsługi środowiska Windows PowerShell.

### <a name="deploy-dedicated-administrative-workstations"></a>Wdrażanie dedykowanych administracyjnych stacji roboczych

Mimo niedogodności mogą być wymagane oddzielne stacje robocze ze wzmocnionymi zabezpieczeniami dla użytkowników z poświadczeniami administracyjnymi o dużym znaczeniu. Należy zapewnić host z poziomem zabezpieczeń, który jest równy lub większy niż poziom uprawnień powierzony poświadczeniom. Rozważ zastosowanie następujących środków w celu dodatkowej ochrony:

- **Zweryfikowanie wszystkich nośników w kompilacji jako czystych**, aby ograniczyć ryzyko ze strony złośliwego oprogramowania zainstalowanego w obrazie głównym lub wstrzykniętego do pliku instalacyjnego podczas pobierania lub przechowywania.

- **Linie bazowe zabezpieczeń** powinny być używane jako konfiguracje uruchamiania. Menedżer zgodności zabezpieczeń firmy Microsoft (SCM, Security Compliance Manager) może ułatwić skonfigurowanie planów bazowych na hostach administracyjnych.

- **Bezpieczny rozruch** w celu wyeliminowania problemów z osobami atakującymi lub złośliwym oprogramowaniem próbujących załadować niepodpisany kod do procesu rozruchu.

- **Ograniczenie oprogramowania**, aby zapewnić, że na hostach administracyjnych uruchamiane jest tylko autoryzowane oprogramowanie. Klienci mogą używać funkcji AppLocker dla tego zadania z zatwierdzoną listą autoryzowanych aplikacji, aby zapobiec wykonywaniu złośliwego oprogramowania i nieobsługiwanym aplikacjom.

- **Pełne szyfrowanie woluminów** w celu ograniczenia fizycznej utraty komputerów, takich jak laptopy administracyjne używane zdalnie.

- **Ograniczenia portów USB** w celu ochrony przed infekcją fizyczną.

- **Izolacja sieci** chroniąca przed atakami sieciowymi i nieumyślne akcje administracyjne. Zapory hosta powinny blokować wszystkie połączenia przychodzące z wyjątkiem tych, które są jawnie wymagane, i blokować wszystkie niepotrzebne wychodzący dostęp do Internetu.

- Ochrona przed **złośliwym oprogramowaniem** w celu ochrony przed znanymi zagrożeniami i złośliwym oprogramowaniem.

- **Środki ograniczające ryzyko ze strony programów wykorzystujących luki**, aby chronić przed nieznanymi zagrożeniami i programami wykorzystującymi luki, w tym zestaw narzędzi Enhanced Mitigation Experience Toolkit (EMET).

- **Analiza obszaru ataków** pozwala uniknąć wprowadzenia nowych wektorów ataków do systemu Windows podczas instalacji nowego oprogramowania. Narzędzia takie jak Attack Surface Analyzer (ASA) ułatwiają ocenę ustawień konfiguracyjnych hosta i identyfikację metod ataków, które stały się dostępne po wprowadzeniu zmian w oprogramowaniu lub konfiguracji.

- **Uprawnienia administracyjne** nie powinny być nadawane użytkownikom na ich komputerach lokalnych.

- **Tryb administracji z ograniczeniami** dla wychodzących sesji protokołu RDP, z wyjątkiem sytuacji, gdy jest to wymagane przez rolę. Aby uzyskać więcej informacji, zobacz [Co nowego w usługach pulpitu zdalnego w systemie Windows Server](https://technet.microsoft.com/library/dn283323.aspx).

Niektóre z tych środków mogą wydawać się skrajne, ale w ostatnich latach miało miejsce wiele sytuacji ilustrujących możliwości, które posiadają doświadczeni atakujący chcący złamać zabezpieczenia swoich celów.

## <a name="prepare-existing-domains-to-be-managed-by-the-bastion-environment"></a>Przygotowywanie istniejących domen do zarządzania przez środowisko bastionu

Program MIM używa poleceń cmdlet programu PowerShell, aby ustanowić zaufanie między istniejącymi domenami usługi AD, a dedykowanym lasem administracyjnym w środowisku bastionu. Po wdrożeniu środowiska bastionu i przed przekonwertowaniem jakichkolwiek użytkowników lub grup do metody JIT za pomocą poleceń cmdlet `New-PAMTrust` i `New-PAMDomainConfiguration` zostaną zaktualizowane relacje zaufania domeny i utworzone artefakty niezbędne dla usługi AD i programu MIM.

Po zmianie istniejącej topologii usługi Active Directory polecenia cmdlet `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` i `Remove-PAMDomainConfiguration` mogą zostać użyte do zaktualizowania relacji zaufania.

## <a name="establish-trust-for-each-forest"></a>Ustanawiania relacji zaufania dla każdego lasu

Polecenie cmdlet `New-PAMTrust` należy uruchomić jeden raz dla każdego istniejącego lasu. Jest ono wywoływane na komputerze usługi MIM w domenie administracyjnej. Parametrami tego polecenia jest nazwa domeny najwyższego poziomu istniejącego lasu i poświadczenia administratora tej domeny.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Po ustanowieniu relacji zaufania skonfiguruj każdą domenę, aby umożliwić zarządzanie z poziomu środowiska bastionu w sposób opisany w następnej sekcji.

## <a name="enable-management-of-each-domain"></a>Włączanie funkcji zarządzania każdej domeny

Istnieje siedem wymagań dotyczących włączania zarządzania dla istniejącej domeny.

### <a name="1-a-security-group-on-the-local-domain"></a>1. Grupa zabezpieczeń w domenie lokalnej

W istniejącej domenie musi znajdować się grupa, której nazwa jest nazwą domeny NetBIOS, po której następują trzy znaki dolara ($), np. *CONTOSO$$$*. Zakres grupy musi mieć wartość *Lokalny w domenie*, a typ grupy musi mieć wartość *Zabezpieczenia*. Dzięki temu grupy mogą być tworzone w dedykowanym lesie administracyjnym z tym samym identyfikatorem zabezpieczeń co grupy w tej domenie. Utwórz tę grupę przy użyciu następującego polecenia programu PowerShell wykonywanego przez administratora istniejącej domeny i uruchom na stacji roboczej przyłączonej do istniejącej domeny:

```PowerShell
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### <a name="2-success-and-failure-auditing"></a>2. Inspekcja sukcesów i niepowodzeń

Ustawienia zasad grupy na kontrolerze domeny na potrzeby inspekcji muszą obejmować inspekcję sukcesów i niepowodzeń dla zarządzania kontem inspekcji i dostępu do usługi katalogowej inspekcji. Może to zostać przeprowadzone za pomocą konsoli zarządzania zasadami grupy przez administratora istniejącej domeny i uruchomione na stacji roboczej przyłączonej do istniejącej domeny:

3. Przejdź do   >  **menu Start Narzędzia administracyjne**  >  **zasady grupy zarządzanie**.

4. Przejdź do **lasu: contoso. Local**  >  **domen**  >  **contoso. Local** kontrolery  >  **domeny**  >  **domyślne zasady kontrolerów domeny**. Zostanie wyświetlony komunikat informacyjny.

    ![Domyślne zasady kontrolerów domeny — zrzut ekranu](media/pam-group-policy-management.jpg)

5. Kliknij prawym przyciskiem myszy pozycję **Domyślne zasady kontrolerów domeny** i wybierz polecenie **Edytuj**. Zostanie wyświetlone nowe okno.

6. W oknie Edytor zarządzania zasadami grupy w obszarze domyślne drzewo zasad kontrolerów domeny Przejdź do pozycji **Konfiguracja komputera**  >  **zasady**  >  **Ustawienia systemu Windows** ustawienia  >  **zabezpieczeń**  >  **Zasady lokalne** zasady  >  **inspekcji**.

    ![Edytor zarządzania zasadami grupy — zrzut ekranu](media/pam-group-policy-management-editor.jpg)

5. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Przeprowadź inspekcję zarządzania kontami**, a następnie wybierz polecenie **Właściwości**. Wybierz pozycję **Definiuj następujące ustawienia zasad**, zaznacz pole wyboru obok pozycji **Powodzenie**, zaznacz pole wyboru obok pozycji **Niepowodzenie**, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**.

6. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Przeprowadź inspekcję dostępu do usługi katalogowej**, a następnie wybierz polecenie **Właściwości**. Wybierz pozycję **Definiuj następujące ustawienia zasad**, zaznacz pole wyboru obok pozycji **Powodzenie**, zaznacz pole wyboru obok pozycji **Niepowodzenie**, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**.

    ![Ustawienia zasad sukcesów i niepowodzeń — zrzut ekranu](media/pam-group-policy-management-editor2.jpg)

7. Zamknij okno Edytor zarządzania zasadami grupy i okno Zarządzanie zasadami grupy. Następnie zastosuj ustawienia inspekcji, otwierając okno programu PowerShell i wpisując następujące polecenie:

    ```cmd
    gpupdate /force /target:computer
    ```

Komunikat „Pomyślnie ukończono aktualizowanie zasad komputera” powinien zostać wyświetlony po kilku minutach.

### <a name="3-allow-connections-to-the-local-security-authority"></a>3. Zezwalaj na połączenia z urzędem zabezpieczeń lokalnych

Kontrolery domeny muszą zezwalać na połączenia RCP przez protokół TCP/IP dla urzędu zabezpieczeń lokalnych (LSA, Local Security Authority) ze środowiska bastionu. W starszych wersjach systemu Windows Server obsługa protokołu TCP/IP w urzędzie LSA musi zostać włączona w rejestrze:

```PowerShell
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### <a name="4-create-the-pam-domain-configuration"></a>4. Utwórz konfigurację domeny PAM

Polecenie cmdlet `New-PAMDomainConfiguration` musi zostać wywołane na komputerze usługi MIM w domenie administracyjnej. Parametrami tego polecenia jest nazwa istniejącej domeny i poświadczenia administratora tej domeny.

```PowerShell
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### <a name="5-give-read-permissions-to-accounts"></a>5. Nadaj uprawnienia do odczytu kontom

Konta w lesie bastionu używane do ustanawiania ról (administratorzy korzystający z poleceń cmdlet `New-PAMUser` i `New-PAMGroup`) oraz konto używane przez usługę monitorowania MIM potrzebują uprawnień do odczytu w tej domenie.

Wykonanie poniższych kroków spowoduje włączenie dostępu do odczytu dla użytkownika *PRIV\Administrator* do domeny *Contoso* w kontrolerze domeny *CORPDC*:

1. Upewnij się, że zalogowano się w kontrolerze domeny CORPDC jako administrator domeny Contoso (na przykład Contoso\Administrator).

2. Uruchom przystawkę Użytkownicy i komputery usługi Active Directory.

3. Kliknij prawym przyciskiem myszy domenę **contoso.local** i wybierz polecenie **Deleguj kontrolę**.

4. Na karcie Wybrani użytkownicy i grupy kliknij przycisk **Dodaj**.

5. W wyskakującym okienku Wybieranie: Użytkownicy, komputery lub grupy kliknij pozycję **Lokalizacje**, a następnie zmień lokalizację na *priv.contoso.local*. W polu nazwy obiektu wpisz *Administratorzy domeny*, a następnie kliknij pozycję **Sprawdź nazwy**. Po wyświetleniu wyskakującego okienka wpisz nazwę użytkownika *priv\administrator* i hasło.

6. Po ciągu Administratorzy domeny wpisz ciąg *; MIMMonitor*. Po podkreśleniu nazw Administratorzy domeny i MIMMonitor kliknij przycisk **OK**, a następnie kliknij przycisk **Dalej**.

7. Na liście typowych zadań wybierz pozycję **Odczytywanie wszystkich informacji o użytkowniku**, kliknij przycisk **Dalej**, a następnie kliknij przycisk **Zakończ**.

18. Zamknij stronę Użytkownicy i komputery usługi Active Directory.

### <a name="6-a-break-glass-account"></a>6. konto szklanej przerwy

Jeśli celem projektu zarządzania dostępem uprzywilejowanym jest ograniczenie liczby kont z uprawnieniami administratora domeny trwale przypisanymi do domeny, w domenie musi istnieć konto *awaryjne* w przypadku wystąpienia w przyszłości problemów z relacją zaufania. Konta służące do awaryjnego dostępu do lasu produkcyjnego powinny istnieć w każdej domenie i z ich poziomu powinno być możliwe zalogowanie się tylko do kontrolerów domeny. W przypadku organizacji z wieloma lokacjami na potrzeby nadmiarowości mogą być wymagane dodatkowe konta.

### <a name="7-update-permissions-in-the-bastion-environment"></a>7. aktualizowanie uprawnień w środowisku bastionu

Przejrzyj uprawnienia obiektu *AdminSDHolder* w kontenerze systemu w tej domenie. Obiekt *AdminSDHolder* zawiera unikatową listę kontroli dostępu służącą do kontrolowania uprawnień podmiotów zabezpieczeń, które są członkami wbudowanych uprzywilejowanych grup usługi Active Directory. Sprawdź, czy wprowadzono jakiekolwiek zmiany mogące mieć wpływ na użytkowników z uprawnieniami administracyjnymi w domenie, ponieważ te uprawnienia nie będą miały zastosowania względem użytkowników, których konto znajduje się w środowisku bastionu.

## <a name="select-users-and-groups-for-inclusion"></a>Wybieranie użytkowników i grup do włączenia

Następnym krokiem jest zdefiniowanie ról PAM, kojarząc użytkowników i grupy, do których powinny mieć dostęp. Ci użytkownicy i grupy zwykle będą podzbiorem użytkowników i grup dla warstwy identyfikowanej jako zarządzana w środowisku bastionu. Aby uzyskać więcej informacji, zobacz [Defining roles for Privileged Access Management](defining-roles-for-pam.md) (Definiowanie ról na potrzeby funkcji Privileged Access Management).
