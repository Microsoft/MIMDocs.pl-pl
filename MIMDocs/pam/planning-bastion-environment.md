---
title: "Planowanie środowiska bastionu | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b8af77d2354428da19d91d5f02b490012835f544
ms.openlocfilehash: 0ed48d43825e1a876c4d96cafcb6c17cac26610f


---

# Planowanie środowiska bastionu

Dodanie środowiska bastionu z dedykowanym lasem administracyjnym do usługi Active Directory umożliwia organizacjom łatwe zarządzanie kontami administracyjnymi, stacjami roboczymi i grupami w środowisku mającym silniejsze kontrole zabezpieczeń niż ich istniejące środowisko produkcyjne.

Taka architektura umożliwia stosowanie szeregu kontroli, których nie można wykonać w architekturze pojedynczego lasu lub są w niej trudne do skonfigurowania. Obejmuje to aprowizację kont jako standardowych nieuprzywilejowanych użytkowników w lesie administracyjnym, które są wysoce uprzywilejowane w środowisku produkcyjnym, co w większym stopniu zapewnia techniczne wymuszanie ładu. Taka architektura umożliwia również korzystanie z funkcji selektywnego uwierzytelniania zaufania w celu ograniczania logowań (i widoczności poświadczeń) tylko do autoryzowanych hostów. W sytuacjach, w których żądany jest wyższy poziom gwarancji dla lasu produkcyjnego bez ponoszenia kosztów i wprowadzania złożoności pełnego ponownego tworzenia, las administracyjny może zapewnić środowisko, które zwiększa poziom gwarancji środowiska produkcyjnego.

Oprócz dedykowanego lasu administracyjnego można użyć innych technik. Obejmują one ograniczanie widoczności poświadczeń administracyjnych, ograniczanie uprawnień ról użytkowników w tym lesie i zapewnianie, że zadania administracyjne nie są wykonywane na hostach używanych do standardowych działań użytkowników (na przykład korzystania z poczty e-mail i przeglądania sieci Web).

## Zagadnienia dotyczące najlepszych rozwiązań

Dedykowany las administracyjny to standardowy las usługi Active Directory w pojedynczej domenie używany do zarządzania usługą Active Directory. Korzyścią z używania lasów i domen administracyjnych jest większa liczba zabezpieczeń niż w przypadku lasów produkcyjnych ze względu na ich ograniczone przypadki użycia. Ponadto ponieważ ten las jest oddzielony i nie ufa istniejącym lasom organizacji, naruszenie bezpieczeństwa w innym lesie nie będzie stanowiło zagrożenia dla tego lasu dedykowanego.

Z projektem lasu administracyjnego są związane następujące kwestie:

### Ograniczony zakres

Zaletą lasu administracyjnego są wysokie gwarancje bezpieczeństwa i mniejszy obszar narażony na ataki. W lesie mogą znajdować się dodatkowe funkcje zarządzania i aplikacje, ale każde zwiększenie zakresu spowoduje zwiększenie obszaru narażonego na ataki w lesie i jego zasobach. Celem jest ograniczenie funkcji lasu, aby obszar narażony na ataki był jak najmniejszy.

Zgodnie z [modelem warstwy](tier-model-for-partitioning-administrative-privileges.md) partycjonowania uprawnień administracyjnych konta w dedykowanym lesie administracyjnym powinny znajdować się w pojedynczej warstwie, zwykle w warstwie 0 lub 1. W przypadku lasu w warstwie 1 należy rozważyć ograniczenie go do określonego zakresu aplikacji (np. aplikacje finansowe) lub społeczności użytkowników (np. zewnętrzni dostawcy IT).

### Ograniczone zaufanie

Produkcyjny las *CORP* powinien ufać administracyjnemu lasowi *PRIV*, ale nie odwrotnie. Może to być zaufanie domeny lub zaufanie lasu. Domena lasu administracyjnego nie musi ufać zarządzanym domenom i lasom w celu zarządzania usługą Active Directory, ale dodatkowe aplikacje mogą wymagać dwukierunkowej relacji zaufania, walidacji zabezpieczeń i testowania.

Należy korzystać z uwierzytelniania selektywnego w celu zapewnienia, że konta w lesie administracyjnym używają tylko odpowiednich hostów produkcyjnych. Do obsługi kontrolerów domeny i delegowania uprawnień w usłudze Active Directory zwykle wymaga to przyznania prawa „Zezwolono na logowanie” dla kontrolerów domeny wyznaczonym kontom administracyjnym warstwy 0 w lesie administracyjnym. Aby uzyskać więcej informacji, zobacz [Configuring Selective Authentication Settings](http://technet.microsoft.com/library/cc755844.aspx) (Konfigurowanie selektywnych ustawień uwierzytelniania).

## Zachowanie separacji logicznej

Aby zapewnić, że na środowisko bastionu nie będą miały wpływu istniejące lub przyszłe przypadki naruszenia zabezpieczeń w organizacyjnej usłudze Active Directory, podczas przygotowywania systemu do użycia środowiska bastionu należy wziąć pod uwagę następujące wytyczne:

- Serwery z systemem Windows nie powinny być przyłączone do domeny ani wykorzystywać dystrybucji oprogramowania lub ustawień z istniejącego środowiska.

- Środowisko bastionu musi zawierać własne Usługi domenowe Active Directory, zapewniając protokół Kerberos i LDAP, serwer DNS oraz usługi związane z czasem w środowisku bastionu.

- Program MIM nie powinien używać farmy baz danych SQL w istniejącym środowisku. Program SQL Server należy wdrożyć na dedykowanych serwerach w środowisku bastionu.

- Środowisko bastionu wymaga programu Microsoft Identity Manager 2016 — w szczególności muszą zostać wdrożone usługa MIM i składniki PAM.

- Oprogramowanie do tworzenia kopii zapasowej i nośniki kopii zapasowej dla środowiska bastionu muszą być przechowywane oddzielnie od systemów w istniejących lasach, aby administrator w istniejącym lesie nie mógł wykorzystać kopii zapasowej środowiska bastionu.

- Użytkownicy, którzy zarządzają serwerami środowiska bastionu, muszą logować się ze stacji roboczych niedostępnych dla administratorów w istniejącym środowisku, aby nie nastąpił wyciek poświadczeń środowiska bastionu.

## Zapewnianie dostępności usług administracyjnych

Ponieważ administrowanie aplikacjami zostanie przeniesione do środowiska bastionu, rozważ, jak zapewnić wystarczającą dostępność w celu spełnienia wymagań tych aplikacji. Te techniki to m.in.:

- Wdrożenie Usług domenowych Active Directory na wielu komputerach w środowisku bastionu. Co najmniej dwie usługi są niezbędne w celu zapewnienia ciągłego uwierzytelniania nawet wtedy, gdy jeden serwer będzie tymczasowo uruchamiany ponownie w celu wykonania zaplanowanej konserwacji. W celu obsługi większych obciążeń lub zarządzania zasobami i administratorami w wielu regionach geograficznych mogą być wymagane dodatkowe komputery.

- Przygotowanie kont awaryjnych w istniejącym lesie oraz dedykowany las administracyjny w razie awarii.

- Wdrożenie programu SQL Server i usługi MIM na wielu komputerach w środowisku bastionu.

- Przechowywanie kopii zapasowej usługi AD i programu SQL dla każdej zmiany definicji użytkowników lub ról w dedykowanym lesie administracyjnym.

## Konfigurowanie odpowiednich uprawnień usługi Active Directory

Las administracyjny musi być skonfigurowany z najmniejszymi uprawnieniami na podstawie wymagań dotyczących administracji usługi Active Directory.

- Kontom w lesie administracyjnym używanym do administrowania środowiskiem produkcyjnym nie należy przyznawać uprawnień administracyjnych do lasu administracyjnego ani znajdujących się w nim domen i stacji roboczych.

- Uprawnienia administracyjne dotyczące lasu administracyjnego powinny być ściśle kontrolowane przez proces w trybie offline, aby ograniczyć możliwość wymazania dzienników inspekcji przez osobę atakującą lub złośliwe oprogramowanie. Pomaga to także zapewnić, że pracownicy z produkcyjnymi kontami administracyjnymi nie zmniejszą ograniczeń dotyczących własnych kont, co spowoduje zwiększenie ryzyka dla organizacji.

- W przypadku lasu administracyjnego należy stosować konfiguracje Menedżera zgodności zabezpieczeń firmy Microsoft (SCM, Security Compliance Manager) dla domeny, w tym silne konfiguracje protokołów uwierzytelniania.

Podczas tworzenia środowiska bastionu przed zainstalowaniem programu Microsoft Identity Manager należy określić i utworzyć konta, które będą używane na potrzeby administracyjne w ramach tego środowiska. Dotyczy to następujących kont:

- **Konta awaryjne** powinny mieć możliwość logowania się tylko do kontrolerów domeny w środowisku bastionu.

- **Administratorzy z czerwonymi kartami** aprowizują inne konta i przeprowadzają niezaplanowaną konserwację. Tym kontom nie jest przyznawany jakikolwiek dostęp do istniejących lasów ani systemów znajdujących się poza środowiskiem bastionu. Poświadczenia, np. karta inteligentna, powinny być zabezpieczone fizycznie, a korzystanie z takiego konta powinno być rejestrowane.

- **Konta usług** wymagane przez program Microsoft Identity Manager, program SQL Server i inne oprogramowanie.

## Wzmacnianie zabezpieczeń hostów

Wszystkie hosty, w tym kontrolery domeny, serwery i stacje robocze przyłączone do lasu administracyjnego, powinny mieć zainstalowane najnowsze systemy operacyjne i dodatki Service Pack oraz być zawsze aktualne.

- Aplikacje wymagane do wykonywania czynności administracyjnych należy wstępnie zainstalować na stacjach roboczych, aby konta ich używające nie musiały należeć do grupy administratorów lokalnych w celu zainstalowania tych aplikacji. Konserwacja kontrolera domeny może być zwykle przeprowadzona przy użyciu protokołu RDP i narzędzi administracji zdalnej serwera.

- Na hostach lasu administracyjnego powinny być automatycznie stosowane aktualizacje zabezpieczeń. Mimo że może to spowodować ryzyko przerwania operacji obsługi kontrolera domeny, zapewnia to znaczące zmniejszenie zagrożeń bezpieczeństwa wynikających z luk w zabezpieczeniach, względem których nie zostały zastosowane poprawki.

### Identyfikowanie hostów administracyjnych

Ryzyko dotyczące systemu lub stacji roboczej powinno być mierzone na podstawie działań o najwyższym ryzyku, które odbywają się w danym systemie lub na danej stacji roboczej, takich jak przeglądanie sieci Web, wysyłanie i odbieranie wiadomości e-mail lub korzystanie z innych aplikacji, które przetwarzają nieznaną lub niezaufaną zawartość.

Hosty administracyjne obejmują następujące komputery:

- Komputer stacjonarny, na którym poświadczenia administratora są fizycznie wpisywane lub wprowadzane.

- Administracyjne „serwery przeskoku”, na których są uruchamiane sesje i narzędzia administracyjne.

- Wszystkie hosty, na których są wykonywane działania administracyjne, w tym te, które korzystają ze standardowego pulpitu użytkownika z uruchomionym klientem RDP w celu zdalnego administrowania serwerami i aplikacjami.

- Serwery hostujące aplikacje, które muszą być administrowane i do których nie można uzyskać dostępu za pomocą protokołu RDP z funkcją ograniczonego trybu administratora lub zdalnej obsługi środowiska Windows PowerShell.

### Wdrażanie dedykowanych administracyjnych stacji roboczych

Mimo niedogodności mogą być wymagane oddzielne stacje robocze ze wzmocnionymi zabezpieczeniami dla użytkowników z poświadczeniami administracyjnymi o dużym znaczeniu. Należy zapewnić host z poziomem zabezpieczeń, który jest równy lub większy niż poziom uprawnień powierzony poświadczeniom. Rozważ zastosowanie następujących środków w celu dodatkowej ochrony:

- **Zweryfikowanie wszystkich nośników w kompilacji jako czystych**, aby ograniczyć ryzyko ze strony złośliwego oprogramowania zainstalowanego w obrazie głównym lub wstrzykniętego do pliku instalacyjnego podczas pobierania lub przechowywania.

- **Plany bazowe zabezpieczeń** powinny być używane jako konfiguracja początkowa. Menedżer zgodności zabezpieczeń firmy Microsoft (SCM, Security Compliance Manager) może ułatwić skonfigurowanie planów bazowych na hostach administracyjnych.

- **Bezpieczny rozruch**, aby ograniczyć ryzyko ze strony podejmowanych przez osoby atakujące lub złośliwe oprogramowanie próby załadowania niepodpisanego kodu do procesu rozruchu.

- **Ograniczenie oprogramowania**, aby zapewnić, że na hostach administracyjnych uruchamiane jest tylko autoryzowane oprogramowanie. Do wykonania tego zadania klienci mogą użyć funkcji AppLocker z listą dozwolonych autoryzowanych aplikacji, aby łatwiej zapobiec uruchamianiu złośliwego oprogramowania i nieobsługiwanych aplikacji.

- **Pełne szyfrowanie woluminu**, aby ograniczyć straty związane z fizyczną utratą komputerów, takich jak laptopy administracyjne używane zdalnie.

- **Ograniczenia portów USB** w celu ochrony przed infekcją fizyczną.

- **Izolacja sieci** w celu ochrony przed atakami sieciowymi oraz przypadkowymi działaniami administracyjnymi. Zapory hostów powinny blokować wszystkie połączenia przychodzące z wyjątkiem tych jawnie wymaganych i blokować cały zbędny ruch wychodzący do Internetu.

- **Oprogramowanie chroniące przed złośliwym kodem** do ochrony przed znanym złośliwym oprogramowaniem i znanymi zagrożeniami.

- **Środki ograniczające ryzyko ze strony programów wykorzystujących luki**, aby chronić przed nieznanymi zagrożeniami i programami wykorzystującymi luki, w tym zestaw narzędzi Enhanced Mitigation Experience Toolkit (EMET).

- **Analiza obszaru narażonego na ataki**, aby uniemożliwić wykorzystanie nowych metod ataku w systemie Windows podczas instalacji nowego oprogramowania. Narzędzia takie jak Attack Surface Analyzer (ASA) ułatwiają ocenę ustawień konfiguracyjnych hosta i identyfikację metod ataków, które stały się dostępne po wprowadzeniu zmian w oprogramowaniu lub konfiguracji.

- **Uprawnienia administracyjne** nie powinny być nadawane użytkownikom na ich komputerach lokalnych.

- **Tryb administracji z ograniczeniami** dla wychodzących sesji protokołu RDP, z wyjątkiem sytuacji, gdy jest to wymagane przez rolę. Aby uzyskać więcej informacji, zobacz [Co nowego w usługach pulpitu zdalnego w systemie Windows Server](https://technet.microsoft.com/library/dn283323.aspx).

Niektóre z tych środków mogą wydawać się skrajne, ale w ostatnich latach miało miejsce wiele sytuacji ilustrujących możliwości, które posiadają doświadczeni atakujący chcący złamać zabezpieczenia swoich celów.

## Przygotowywanie istniejących domen do zarządzania przez środowisko bastionu

Program MIM używa poleceń cmdlet programu PowerShell, aby ustanowić zaufanie między istniejącymi domenami usługi AD, a dedykowanym lasem administracyjnym w środowisku bastionu. Po wdrożeniu środowiska bastionu i przed przekonwertowaniem jakichkolwiek użytkowników lub grup do metody JIT za pomocą poleceń cmdlet `New-PAMTrust` i `New-PAMDomainConfiguration` zostaną zaktualizowane relacje zaufania domeny i utworzone artefakty niezbędne dla usługi AD i programu MIM.

Po zmianie istniejącej topologii usługi Active Directory polecenia cmdlet `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` i `Remove-PAMDomainConfiguration` mogą zostać użyte do zaktualizowania relacji zaufania.

### Ustanawiania relacji zaufania dla każdego lasu

Polecenie cmdlet `New-PAMTrust` należy uruchomić jeden raz dla każdego istniejącego lasu. Jest ono wywoływane na komputerze usługi MIM w domenie administracyjnej. Parametrami tego polecenia jest nazwa domeny najwyższego poziomu istniejącego lasu i poświadczenia administratora tej domeny.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Po ustanowieniu relacji zaufania skonfiguruj każdą domenę, aby umożliwić zarządzanie z poziomu środowiska bastionu w sposób opisany w następnej sekcji.

### Włączanie funkcji zarządzania każdej domeny

Istnieje siedem wymagań dotyczących włączania zarządzania dla istniejącej domeny.

#### 1. Grupa zabezpieczeń w domenie lokalnej

W istniejącej domenie musi znajdować się grupa, której nazwa jest nazwą domeny NetBIOS, po której następują trzy znaki dolara ($), np. *CONTOSO$$$*. Zakres grupy musi mieć wartość *Lokalny w domenie*, a typ grupy musi mieć wartość *Zabezpieczenia*. Dzięki temu grupy mogą być tworzone w dedykowanym lesie administracyjnym z tym samym identyfikatorem zabezpieczeń co grupy w tej domenie. Utwórz tę grupę za pomocą następującego polecenia programu PowerShell wykonanego przez administratora istniejącej domeny i uruchomionego na stacji roboczej przyłączonej do istniejącej domeny:

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

#### 2. Inspekcja sukcesów i niepowodzeń

Ustawienia zasad grupy na kontrolerze domeny na potrzeby inspekcji muszą obejmować inspekcję sukcesów i niepowodzeń dla zarządzania kontem inspekcji i dostępu do usługi katalogowej inspekcji. Może to zostać przeprowadzone za pomocą konsoli zarządzania zasadami grupy przez administratora istniejącej domeny i uruchomione na stacji roboczej przyłączonej do istniejącej domeny:

3. Wybierz kolejno pozycje **Start** > **Narzędzia administracyjne** > **Zarządzanie zasadami grupy**.

4. Wybierz pozycję **Las: contoso.local** > **Domeny** > **contoso.local** > **Kontrolery domeny** > **Domyślne zasady kontrolerów domeny**. Zostanie wyświetlony komunikat informacyjny.

    ![Domyślne zasady kontrolerów domeny — zrzut ekranu](media/pam-group-policy-management.jpg)

5. Kliknij prawym przyciskiem myszy pozycję **Domyślne zasady kontrolerów domeny** i wybierz polecenie **Edytuj**. Zostanie wyświetlone nowe okno.

6. W oknie Edytor zarządzania zasadami grupy w drzewie Domyślne zasady kontrolerów domeny wybierz pozycję **Konfiguracja komputera** > **Zasady** > **Ustawienia systemu Windows** > **Ustawienia zabezpieczeń** > **Zasady lokalne** > **Zasady inspekcji**.

    ![Edytor zarządzania zasadami grupy — zrzut ekranu](media/pam-group-policy-management-editor.jpg)

5. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Przeprowadź inspekcję zarządzania kontami**, a następnie wybierz polecenie **Właściwości**. Wybierz pozycję **Definiuj następujące ustawienia zasad**, zaznacz pole wyboru obok pozycji **Powodzenie**, zaznacz pole wyboru obok pozycji **Niepowodzenie**, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**.

6. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Przeprowadź inspekcję dostępu do usługi katalogowej**, a następnie wybierz polecenie **Właściwości**. Wybierz pozycję **Definiuj następujące ustawienia zasad**, zaznacz pole wyboru obok pozycji **Powodzenie**, zaznacz pole wyboru obok pozycji **Niepowodzenie**, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**.

    ![Ustawienia zasad sukcesów i niepowodzeń — zrzut ekranu](media/pam-group-policy-management-editor2.jpg)

7. Zamknij okno Edytor zarządzania zasadami grupy i okno Zarządzanie zasadami grupy. Następnie zastosuj ustawienia inspekcji, otwierając okno programu PowerShell i wpisując następujące polecenie:

    ```
    gpupdate /force /target:computere
    ```

Po upływie kilku minut powinien zostać wyświetlony komunikat „Aktualizacja zasad komputera została ukończona pomyślnie”.

#### 3. Zezwalanie na połączenia z urzędem zabezpieczeń lokalnych

Kontrolery domeny muszą zezwalać na połączenia RCP przez protokół TCP/IP dla urzędu zabezpieczeń lokalnych (LSA, Local Security Authority) ze środowiska bastionu. W starszych wersjach systemu Windows Server obsługa protokołu TCP/IP w urzędzie LSA musi zostać włączona w rejestrze:

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

#### 4. Tworzenie konfiguracji domeny PAM

Polecenie cmdlet `New-PAMDomainConfiguration` musi zostać wywołane na komputerze usługi MIM w domenie administracyjnej. Parametrami tego polecenia jest nazwa istniejącej domeny i poświadczenia administratora tej domeny.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

#### 5. Nadawanie kontom uprawnień do odczytu

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

#### 6. Konto awaryjne

Jeśli celem projektu zarządzania dostępem uprzywilejowanym jest ograniczenie liczby kont z uprawnieniami administratora domeny trwale przypisanymi do domeny, w domenie musi istnieć konto *awaryjne* w przypadku wystąpienia w przyszłości problemów z relacją zaufania. Konta służące do awaryjnego dostępu do lasu produkcyjnego powinny istnieć w każdej domenie i z ich poziomu powinno być możliwe zalogowanie się tylko do kontrolerów domeny. W przypadku organizacji z wieloma lokacjami na potrzeby nadmiarowości mogą być wymagane dodatkowe konta.

#### 7. Aktualizowanie uprawnień w środowisku bastionu

Przejrzyj uprawnienia obiektu *AdminSDHolder* w kontenerze systemu w tej domenie. Obiekt *AdminSDHolder* zawiera unikatową listę kontroli dostępu służącą do kontrolowania uprawnień podmiotów zabezpieczeń, które są członkami wbudowanych uprzywilejowanych grup usługi Active Directory. Sprawdź, czy wprowadzono jakiekolwiek zmiany mogące mieć wpływ na użytkowników z uprawnieniami administracyjnymi w domenie, ponieważ te uprawnienia nie będą miały zastosowania względem użytkowników, których konto znajduje się w środowisku bastionu.

## Wybieranie użytkowników i grup do włączenia

Następnym krokiem jest zdefiniowanie ról PAM, kojarząc użytkowników i grupy, do których powinny mieć dostęp. Będzie to zazwyczaj podzbiór użytkowników i grup dla warstwy zidentyfikowanej jako zarządzana w środowisku bastionu. Aby uzyskać więcej informacji, zobacz [Defining roles for Privileged Access Management](defining-roles-for-pam.md) (Definiowanie ról na potrzeby funkcji Privileged Access Management).



<!--HONumber=Jul16_HO3-->


