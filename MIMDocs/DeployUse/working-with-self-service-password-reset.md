---
title: "Praca z funkcją samoobsługowego resetowania hasła | Program Microsoft Identity Manager"
description: "Dowiedz się, jakie zmiany wprowadzono w funkcji samoobsługowego resetowania hasła (SSPR) w programie MIM 2016, łącznie z obsługą uwierzytelniania wieloskładnikowego."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f9b01ac2cee2b96f64a9fda917f4f4146ca2eeda
ms.openlocfilehash: 3a4350e54383dc1098c778090bb29b435082045f


---

# Praca z funkcją samoobsługowego resetowania hasła
W programie Microsoft Identity Manager 2016 rozszerzono funkcję samoobsługowego resetowania hasła. Wprowadzono następujące ulepszenia:

-   Portal samoobsługowego resetowania hasła i ekran logowania do systemu Windows umożliwiają teraz użytkownikom odblokowanie ich kont bez konieczności zmiany haseł lub telefonicznego kontaktowania się z administratorami pomocy technicznej. Konta użytkowników mogą być blokowane z wielu uzasadnionych powodów, takich jak wprowadzenie starego hasła, użycie komputerów z obsługą dwóch języków i skonfigurowanie klawiatury dla niewłaściwego języka lub próba zalogowania się do udostępnionej stacji roboczej, która została już otwarta dla konta innej osoby.

-   Dodano nową bramę uwierzytelniania Phone Gate. Umożliwia ona wykonywanie rozmów telefonicznych w celu uwierzytelniania użytkowników.

-   Dodano obsługę dla usługi Microsoft Azure Multi-Factor Authentication (MFA). Ta funkcja może być używana dla istniejącej bramy SMS haseł jednorazowych lub nowej bramy Phone Gate.

## Uwierzytelnianie wieloskładnikowe Azure
Microsoft Azure Multi-Factor Authentication jest usługą uwierzytelniania, która wymaga weryfikowania prób zalogowania się przez użytkowników przy użyciu aplikacji mobilnej, połączenia telefonicznego lub wiadomości tekstowej. Jest ona dostępna do użycia z usługą Microsoft Azure Active Directory oraz jako usługa dla aplikacji w chmurze i lokalnych aplikacji przedsiębiorstw.

Usługa Azure MFA zapewnia dodatkowy mechanizm uwierzytelniania wspierający istniejące procesy uwierzytelniania, takie jak proces używany przez program MIM w przypadku pomocy związanej z logowaniem użytkowników korzystających z samoobsługi.

Usługa Azure MFA umożliwia uwierzytelnianie i weryfikację tożsamości użytkowników, którzy usiłują odzyskać dostęp do swoich kont i zasobów. Uwierzytelnianie jest dokonywane przy użyciu usługi SMS lub połączeń telefonicznych.   Im silniejsze uwierzytelnianie, tym większa pewność, że osoba usiłująca odzyskać dostęp jest rzeczywistym użytkownikiem będącym właścicielem danej tożsamości. Po uwierzytelnieniu użytkownik może wybrać nowe hasło, które zastąpi stare hasło.

## Wymagania wstępne związane z konfigurowaniem samoobsługowego odblokowywania kont i resetowania haseł przy użyciu usługi MFA
W tej sekcji założono, że użytkownik pobrał niezbędne pliki i ukończył wdrożenie programu Microsoft Identity Manager 2016, w tym następujących składników i usług:

-   System Windows Server 2008 R2 lub nowszy został skonfigurowany jako serwer usługi Active Directory, łącznie z Usługami domenowymi w usłudze Active Directory i kontrolerem domeny z wyznaczoną domeną (domena „firmowa”).

-   Zdefiniowano zasady grupy dla blokowania kont.

-   Usługa synchronizacji programu MIM 2016 (Sync) jest zainstalowana i uruchomiona na serwerze przyłączonym do domeny usługi AD.

-   Usługa i portal programu MIM 2016, w tym portal rejestracji SSPR i portal resetowania SSPR, są zainstalowane i uruchomione na serwerze (mogą znajdować się w tej samej lokalizacji, w której znajduje się usługa synchronizacji).

-   Usługa synchronizacji programu MIM jest skonfigurowana z synchronizacją tożsamości usługi AD programu MIM, w tym:

    -   Konfigurowanie agenta zarządzania Active Directory Management Agent (ADMA) zapewniającego łączność z usługami AD DS oraz możliwość importowania danych tożsamości z usługi Active Directory i eksportowania ich do tej usługi.

    -   Konfigurowanie agenta zarządzania programu MIM (MIM MA) zapewniającego łączność z bazą danych usługi FIM oraz możliwość importowania danych tożsamości z bazy danych usługi FIM i eksportowania ich do tej bazy.

    -   Konfigurowanie reguł synchronizacji w portalu MIM, zezwalających na synchronizację danych użytkowników i ułatwiających działania oparte na synchronizacji w usłudze MIM.

-   Dodatki i rozszerzenia programu MIM 2016, łącznie ze zintegrowanym klientem logowania systemu Windows dla funkcji SSPR, zostały wdrożone na serwerze lub oddzielnym komputerze klienckim.

## Przygotowanie programu MIM do pracy z uwierzytelnianiem wieloskładnikowym
Skonfiguruj synchronizację programu MIM do obsługi resetowania haseł i odblokowywania kont. Aby uzyskać więcej informacji, zobacz [Instalowanie dodatków i rozszerzeń usługi FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Instalowanie funkcji SSPR usługi FIM](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Bramy uwierzytelniania SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) i [Przewodnik po laboratorium testowym funkcji SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx).

W następnej sekcji zostanie skonfigurowany dostawca usługi Azure MFA w usłudze Microsoft Azure Active Directory. W ramach tego procesu zostanie wygenerowany plik zawierający materiał uwierzytelniania, umożliwiający usłudze MFA kontaktowanie się z usługą Azure MFA.  Aby kontynuować, należy uzyskać subskrypcję Azure.

### Rejestrowanie dostawcy uwierzytelniania wieloskładnikowego na platformie Azure

1.  Przejdź do [klasycznej wersji portalu Azure](http://manage.windowsazure.com) i zaloguj się jako administrator subskrypcji Azure.

2.  Kliknij przycisk **Nowy** na dole po lewej stronie ekranu.

3.  Kliknij pozycje **Usługi aplikacji &gt; Active Directory &gt; Dostawca usługi MFA &gt; Szybkie tworzenie**.

![Obraz szybkiego tworzenia dostawcy usługi MFA w portalu Azure](media/MIM-SSPR-Azureportal.png)

4.  W polu **Nazwa** wprowadź **SSPRMFA**, a następnie kliknij przycisk **Utwórz**.

![Obraz usługi MFA w portalu Azure](media/MIM-SSPR-Azureportal-2.png)

5.  Kliknij pozycję **Active Directory** w menu klasycznej wersji portalu Azure, a następnie kliknij kartę **Dostawcy usługi MFA**.

6.  Kliknij pozycję **SSPRMFA**, a następnie kliknij przycisk **Zarządzaj** w dolnej części ekranu.

    ![Ikona Zarządzaj w portalu Azure](media/MIM-SSPR-ManageButton.png)

7.  W nowym oknie w lewym panelu w obszarze **Konfiguruj** kliknij pozycję **Ustawienia**.

8.  W obszarze **Alarm oszustwa** usuń zaznaczenie pola wyboru **Blokuj użytkownika, gdy zostaje zgłoszone oszustwo**. W ten sposób można zapobiec zablokowaniu całej usługi.

9. W otwartym oknie **Azure Multi-Factor Authentication** kliknij pozycję **SDK** w obszarze **Pliki do pobrania** w menu po lewej stronie.

10. Kliknij link **Pliki do pobrania** w kolumnie ZIP dla pliku z językiem **SDK dla programu ASP.net 2.0 C#**.

    ![Obraz pliku zip do pobrania usługi Azure MFA](media/MIM-SSPR-Azure-MFA.png)

11. Skopiuj wynikowy plik ZIP do każdego systemu, w którym jest zainstalowana usługa MIM.  Należy pamiętać, że plik ZIP zawiera klucz używany do uwierzytelniania w usłudze Azure MFA.

### Aktualizowanie pliku konfiguracji

1. Zaloguj się do komputera z zainstalowaną usługą MIM jako użytkownik, który zainstalował program MIM.

2. Utwórz nowy folder katalogu znajdujący się poniżej katalogu, w którym zainstalowano usługę MIM, takim jak **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Korzystając z Eksploratora Windows, przejdź do folderu **\pf\certs** pliku ZIP pobranego w poprzedniej sekcji, a następnie skopiuj plik **cert_key.p12** do nowego katalogu.

4.  W pliku zip zestawu SDK w folderze **\pf** otwórz plik **pf_auth.cs**.

5.  Znajdź trzy następujące parametry: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Obraz kodu pliku pf_auth.cs](media/MIM-SSPR-pFile.png)

6.  W folderze **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** otwórz następujący plik: **MfaSettings**.xml.

7.  Skopiuj wartości z parametrów `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` w pliku pf_aut.cs do odpowiednich elementów xml w pliku MfaSettings.xml.

8.  W pliku zip zestawu SDK, z folderu \pf\certs wyodrębnij plik **cert_key.p12** i wprowadź pełną ścieżkę do niego w pliku MfaSettings.xml do elementu xml `<CertFilePath>`.

9. W elemencie `<username>` wprowadź dowolną nazwę użytkownika.

10. W elemencie `<DefaultCountryCode>` wprowadź swój domyślny kod kraju. Ten kod będzie przypisywany do numerów telefonów zarejestrowanych dla użytkowników bez kodu kraju. Jeśli użytkownik ma międzynarodowy kod kraju, musi on zostać uwzględniony w zarejestrowanym numerze telefonu.

11. Zapisz plik MfaSettings.xml z taką samą nazwą w tej samej lokalizacji.

#### Konfigurowanie bramy telefonicznej lub bramy SMS haseł jednorazowych

1.  Uruchom program Internet Explorer i przejdź do portalu MIM, uwierzytelniając się jako administrator programu MIM, a następnie kliknij przycisk **Przepływy pracy** na pasku nawigacyjnym po lewej stronie.

    ![Obraz nawigacji w portalu MIM](media/MIM-SSPR-workflow.jpg)

2.  Zaznacz pole wyboru **Przepływ pracy AuthN w zakresie resetowania hasła**.

    ![Obraz przepływów pracy w portalu MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Kliknij kartę **Działania**, a następnie przewiń w dół do pozycji **Dodaj działanie**.

4.  Wybierz pozycję **Phone Gate** lub **One-Time Password SMS Gate**, kliknij przycisk **Wybierz**, a następnie kliknij przycisk **OK**.

Użytkownicy w organizacji mogą teraz rejestrować się w celu resetowania haseł.  W trakcie tego procesu będą oni wprowadzać swoje numery telefonów służbowych lub komórkowych, aby umożliwić systemowi ustanawianie połączeń telefonicznych z nimi (lub wysyłanie do nich wiadomości SMS).

#### Rejestrowanie użytkowników w celu resetowania haseł

1.  Użytkownicy uruchamiają przeglądarkę sieci Web i przechodzą do portalu rejestracji na potrzeby resetowania haseł programu MIM.  (Zazwyczaj ten portal będzie skonfigurowany z uwierzytelnianiem systemu Windows).  W portalu użytkownicy muszą ponownie podać swoją nazwę użytkownika i hasło w celu potwierdzenia ich tożsamości.

    Będą oni monitowani o przejście do portalu rejestracji haseł i uwierzytelnienie się przy użyciu nazwy użytkownika i hasła.

2.  Wymagane będzie wprowadzenie kodu kraju, spacji i numeru telefonu w polu **Numer telefonu** lub **Telefon komórkowy**, a następnie kliknięcie przycisku **Dalej**.

    ![Obraz weryfikacji telefonu w portalu MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Obraz weryfikacji telefonu komórkowego w portalu MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## Czy takie rozwiązanie jest odpowiednie dla Twoich użytkowników?
Po skonfigurowaniu i uruchomieniu wszystkich składników można zapoznać się z procedurami resetowania haseł przez użytkowników, którzy po powrocie z urlopu nie pamiętają swoich danych uwierzytelniania.

Istnieją dwa sposoby resetowania hasła i odblokowania konta przez użytkownika (użycie ekranu logowania systemu Windows lub portalu samoobsługowego).

Instalując dodatki i rozszerzenia programu MIM na komputerze przyłączonym do domeny i połączonym za pośrednictwem sieci organizacji z usługą MIM, użytkownicy mogą odzyskać zapomniane hasło na ekranie logowania do komputera.  Poniższe kroki objaśniają ten proces:

#### Resetowanie hasła zintegrowane z logowaniem do komputera z systemem Windows

1.  Jeśli użytkownik wprowadzi nieprawidłowe hasło kilka razy na ekranie logowania, musi kliknąć link **Problemy z logowaniem?** .

    ![Obraz ekranu logowania](media/MIM-SSPR-problemsloggingin.JPG)

    Kliknięcie tego linku powoduje wyświetlenie ekranu resetowania hasła programu MIM, na którym można zmienić swoje hasło lub odblokować konto.

    ![Obraz resetowania hasła programu MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  Użytkownik zostanie przekierowany do strony uwierzytelniania. Jeśli skonfigurowano usługę MFA, zostanie wykonane połączenie telefoniczne z użytkownikiem.

3.  Działając w tle, usługa Azure MFA nawiązuje następnie połączenie telefoniczne z numerem podanym podczas tworzenia konta usługi.

4.  Gdy użytkownik odbierze połączenie, będzie monitowany o naciśnięcie przycisku # na klawiaturze telefonu. Następnie użytkownik klika przycisk **Dalej** w portalu.

    Jeśli zostaną skonfigurowane również inne bramy, użytkownik będzie monitowany o podanie dodatkowych informacji na następnych ekranach.

    > [!NOTE]
    > Jeśli użytkownik nie będzie cierpliwy i kliknie przycisk **Dalej** przed naciśnięciem przycisku #, uwierzytelnianie nie powiedzie się.

5.  Po pomyślnym uwierzytelnieniu użytkownik będzie mógł skorzystać z dwóch opcji: odblokowanie konta i zachowanie bieżącego hasła lub skonfigurowanie nowego hasła.

6.  Następnie użytkownik musi wprowadzić nowe hasło dwa razy, aby zresetować hasło.

#### Dostęp z portalu samoobsługowego

1.  Użytkownicy mogą otworzyć przeglądarkę sieci Web, przejść do **portalu resetowania haseł** i wprowadzić swoją nazwę użytkownika, a następnie kliknąć przycisk **Dalej**.

    Jeśli skonfigurowano usługę MFA, zostanie wykonane połączenie telefoniczne z użytkownikiem. Działając w tle, usługa Azure MFA nawiązuje następnie połączenie telefoniczne z numerem podanym podczas tworzenia konta usługi.

    Jeśli użytkownik odbierze połączenie, będzie monitowany o naciśnięcie przycisku # na klawiaturze telefonu. Następnie użytkownik klika przycisk **Dalej** w portalu.

2.  Jeśli zostaną skonfigurowane również inne bramy, użytkownik będzie monitowany o podanie dodatkowych informacji na następnych ekranach.

    > [!NOTE]
    > Jeśli użytkownik nie będzie cierpliwy i kliknie przycisk **Dalej** przed naciśnięciem przycisku #, uwierzytelnianie nie powiedzie się.

3.  Użytkownik będzie musiał wybrać, czy chce zresetować hasło, czy odblokować konto. Jeśli użytkownik wybierze opcję odblokowania konta, jego konto zostanie odblokowane.

    ![Obraz odblokowania konta (asystent logowania programu MIM)](media/MIM-SSPR-accountUnlock.JPG)

4.  Po pomyślnym uwierzytelnieniu użytkownik będzie mógł skorzystać z dwóch opcji: zachowanie bieżącego hasła lub ustawienie nowego hasła.

5.  ![Obraz pomyślnie odblokowanego konta programu MIM](media/MIM-SSPR-account-unlock.JPG)

6.  Jeśli użytkownik wybierze opcję resetowania hasła, będzie musiał wpisać nowe hasło dwa razy i kliknąć przycisk **Dalej**, aby zmienić hasło.

    ![Obraz resetowania hasła (asystent logowania programu MIM)](media/MIM-SSPR-PR1.JPG)



<!--HONumber=Jun16_HO4-->


