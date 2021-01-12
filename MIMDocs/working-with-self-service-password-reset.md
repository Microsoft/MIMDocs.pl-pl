---
title: Praca z Self-Service resetowania hasła | Microsoft Docs
description: Dowiedz się, jakie zmiany wprowadzono w funkcji samoobsługowego resetowania hasła (SSPR) w programie MIM 2016, łącznie z obsługą uwierzytelniania wieloskładnikowego.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 16901978fd5b51a4a986b07e580d8162dd159575
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104144"
---
# <a name="self-service-password-reset-deployment-options"></a>Opcje wdrażania resetowania hasła Self-Service

W przypadku nowych klientów, którzy mają [licencję na Azure Active Directory — wersja Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing), zalecamy korzystanie z funkcji samoobsługowego [resetowania haseł usługi Azure AD](/azure/active-directory/authentication/concept-sspr-howitworks) w celu zapewnienia środowiska użytkownika końcowego.  Funkcja samoobsługowego resetowania haseł w usłudze Azure AD umożliwia użytkownikowi zresetowanie własnego hasła oraz obsługę wielu takich samych funkcji, jak program MIM, w tym alternatywnej poczty e-mail i pytania&bram.  W przypadku wdrażania funkcji samoobsługowego resetowania hasła w usłudze Azure AD Azure AD Connect obsługuje [zapisywanie nowych haseł do AD DS](/azure/active-directory/authentication/concept-sspr-writeback), a [Usługa powiadamiania o zmianie hasła](deploying-mim-password-change-notification-service-on-domain-controller.md) programu MIM może służyć do przekazywania haseł do innych systemów, takich jak serwer katalogowy innego dostawcy.  Wdrażanie programu MIM do [zarządzania hasłami](infrastructure/mim2016-password-management.md) nie wymaga wdrożenia usługi MIM ani samoobsługowego resetowania hasła lub rejestracji w programie MIM.  Zamiast tego można wykonać następujące czynności:

- Najpierw, jeśli trzeba będzie wysyłać hasła do katalogów innych niż Azure AD i AD DS, wdrożyć program MIM Sync z łącznikami, aby Active Directory Domain Services i wszystkie dodatkowe systemy docelowe, skonfigurować program MIM do [zarządzania hasłami](infrastructure/mim2016-password-management.md) i wdrożyć [usługę powiadamiania o zmianie hasła](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Następnie, jeśli chcesz wysyłać hasła do katalogów innych niż usługa Azure AD, skonfiguruj Azure AD Connect [zapisywania nowych haseł, aby AD DS](/azure/active-directory/authentication/concept-sspr-writeback).
- Opcjonalnie [zarejestruj wstępnie użytkowników](/azure/active-directory/authentication/howto-sspr-authenticationdata).
- Na koniec należy wdrożyć funkcję samoobsługowego [resetowania haseł w usłudze Azure AD dla użytkowników końcowych](/azure/active-directory/authentication/howto-sspr-deployment).

W przypadku istniejących klientów, którzy wcześniej wdrożyły program Forefront Identity Manager (FIM) do samoobsługowego resetowania hasła i mają licencję na Azure Active Directory — wersja Premium, zalecamy zaplanowanie przejścia do funkcji samoobsługowego resetowania haseł w usłudze Azure AD.  Możesz przełączać użytkowników końcowych do samoobsługowego resetowania haseł usługi Azure AD bez konieczności ponownego rejestrowania, [synchronizując lub ustawiając za pomocą programu PowerShell alternatywny adres e-mail lub numer telefonu komórkowego użytkownika](/azure/active-directory/authentication/howto-sspr-authenticationdata). Po zarejestrowaniu użytkowników do samoobsługowego resetowania hasła w usłudze Azure AD można zlikwidować Portal resetowania haseł programu FIM.

W przypadku klientów, którzy nie wdrożono jeszcze funkcji samoobsługowego resetowania hasła usługi Azure AD dla swoich użytkowników, program MIM udostępnia również portale samoobsługowego resetowania hasła.  W porównaniu do programu FIM w programie MIM 2016 wprowadzono następujące zmiany:

- Portal resetowania haseł programu MIM Self-Service i ekran logowania do systemu Windows umożliwiają użytkownikom odblokowywanie ich kont bez konieczności zmiany haseł.
- Dodano nową bramę uwierzytelniania, bramę telefoniczną do programu MIM. Pozwala to na uwierzytelnianie użytkowników za pośrednictwem połączenia telefonicznego za pośrednictwem usługi Microsoft Azure Multi-Factor Authentication (MFA).

Wydanie programu MIM 2016 kompiluje się do wersji 4.5.26.0 z klientem w celu pobrania zestawu Azure Multi-Factor Authentication Software Development Kit (Azure MFA SDK).  Ten zestaw SDK jest przestarzały, a klienci powinni przejść do korzystania z programu MIM SSPR z serwerem usługi Azure MFA lub funkcji samoobsługowego resetowania hasła usługi Azure AD. W tym [artykule](working-with-mfaserver-for-mim.md) opisano sposób aktualizowania portalu samoobsługowego resetowania haseł programu MIM i konfiguracji PAM przy użyciu usługi Azure serwer Multi-Factor Authentication na potrzeby uwierzytelniania wieloskładnikowego.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Wdrażanie portalu resetowania haseł programu MIM Self-Service przy użyciu usługi Azure MFA dla Multi-Factor Authentication

W poniższej sekcji opisano sposób wdrażania portalu funkcji samoobsługowego resetowania haseł w programie MIM przy użyciu usługi Azure MFA na potrzeby uwierzytelniania wieloskładnikowego.  Te kroki są niezbędne tylko w przypadku klientów, którzy nie korzystają z funkcji samoobsługowego resetowania hasła w usłudze Azure AD dla swoich użytkowników.

Microsoft Azure Multi-Factor Authentication jest usługą uwierzytelniania, która wymaga weryfikowania prób zalogowania się przez użytkowników przy użyciu aplikacji mobilnej, połączenia telefonicznego lub wiadomości tekstowej. Jest ona dostępna do użycia z usługą Microsoft Azure Active Directory oraz jako usługa dla aplikacji w chmurze i lokalnych aplikacji przedsiębiorstw.

Usługa Azure MFA zapewnia dodatkowy mechanizm uwierzytelniania wspierający istniejące procesy uwierzytelniania, takie jak proces używany przez program MIM w przypadku pomocy związanej z logowaniem użytkowników korzystających z samoobsługi.

Usługa Azure MFA umożliwia uwierzytelnianie i weryfikację tożsamości użytkowników, którzy usiłują odzyskać dostęp do swoich kont i zasobów. Uwierzytelnianie jest dokonywane przy użyciu usługi SMS lub połączeń telefonicznych.   Im silniejsze uwierzytelnianie, tym większa pewność, że osoba usiłująca odzyskać dostęp jest rzeczywistym użytkownikiem będącym właścicielem danej tożsamości. Po uwierzytelnieniu użytkownik może wybrać nowe hasło, które zastąpi stare hasło.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Wymagania wstępne związane z konfigurowaniem samoobsługowego odblokowywania kont i resetowania haseł przy użyciu usługi MFA

W tej sekcji założono, że pobrano i ukończono wdrożenie składnika synchronizacji programu MIM Microsoft Identity Manager 2016 [, usługi MIM i portalu MIM](microsoft-identity-manager-deploy.md), w tym następujących składników i usług:

-   System Windows Server 2008 R2 lub nowszy został skonfigurowany jako serwer usługi Active Directory, łącznie z Usługami domenowymi w usłudze Active Directory i kontrolerem domeny z wyznaczoną domeną (domena „firmowa”).

-   Zdefiniowano zasady grupy dla blokowania kont.

-   Usługa synchronizacji programu MIM 2016 (Sync) jest zainstalowana i uruchomiona na serwerze przyłączonym do domeny usługi AD.

-   Usługa i portal programu MIM 2016, w tym portal rejestracji SSPR i portal resetowania SSPR, są zainstalowane i uruchomione na serwerze (mogą znajdować się w tej samej lokalizacji, w której znajduje się usługa synchronizacji).

-   Usługa synchronizacji programu MIM jest skonfigurowana z synchronizacją tożsamości usługi AD programu MIM, w tym:

    -   Konfigurowanie agenta zarządzania Active Directory Management Agent (ADMA) zapewniającego łączność z usługami AD DS oraz możliwość importowania danych tożsamości z usługi Active Directory i eksportowania ich do tej usługi.

    -   Konfigurowanie agenta zarządzania programu MIM (MIM MA) zapewniającego łączność z bazą danych usługi FIM oraz możliwość importowania danych tożsamości z bazy danych usługi FIM i eksportowania ich do tej bazy.

    -   Konfigurowanie reguł synchronizacji w portalu MIM, zezwalających na synchronizację danych użytkowników i ułatwiających działania oparte na synchronizacji w usłudze MIM.

-   Dodatki i rozszerzenia programu MIM 2016, łącznie ze zintegrowanym klientem logowania systemu Windows dla funkcji SSPR, zostały wdrożone na serwerze lub oddzielnym komputerze klienckim.

Ten scenariusz wymaga posiadania licencji CAL programu MIM dla użytkowników oraz subskrypcji usługi Azure MFA.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Przygotowanie programu MIM do pracy z uwierzytelnianiem wieloskładnikowym
Skonfiguruj synchronizację programu MIM do obsługi resetowania haseł i odblokowywania kont. Aby uzyskać więcej informacji, zobacz [Instalowanie dodatków i rozszerzeń usługi FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Instalowanie funkcji SSPR usługi FIM](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Bramy uwierzytelniania SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) i [Przewodnik po laboratorium testowym funkcji SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx).


### <a name="update-the-configuration-file"></a>Aktualizowanie pliku konfiguracji

> [!NOTE]
> Ta sekcja była oparta na wcześniejszych wskazówkach przy użyciu pliku ZIP dostarczonego przez zestaw SDK usługi Azure MFA. Zamiast tego należy zastosować wskazówki zawarte w artykule na temat [YSE platformy serwer Multi-Factor Authentication Azure](working-with-mfaserver-for-mim.md).


1. Zaloguj się do komputera z zainstalowaną usługą MIM jako użytkownik, który zainstalował program MIM.

2. Utwórz nowy folder katalogu znajdujący się poniżej katalogu, w którym zainstalowano usługę MIM, takim jak **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Korzystając z Eksploratora Windows, przejdź do folderu **\pf\certs** pliku ZIP pobranego w poprzedniej sekcji, a następnie skopiuj plik **cert_key.p12** do nowego katalogu.

4.  W pliku zip zestawu SDK, w folderze **\pf** otwórz plik **pf_auth. cs**.

5.  Znajdź trzy następujące parametry: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Obraz kodu pliku pf_auth.cs](media/MIM-SSPR-pFile.png)

6.  W folderze **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** otwórz następujący plik: **MfaSettings**.xml.

7.  Skopiuj wartości z parametrów `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` w pliku pf_aut.cs do odpowiednich elementów xml w pliku MfaSettings.xml.

8.  W pliku zip zestawu SDK, z folderu \pf\certs wyodrębnij plik **cert_key.p12** i wprowadź pełną ścieżkę do niego w pliku MfaSettings.xml do elementu xml `<CertFilePath>`.

9. W elemencie `<username>` wprowadź dowolną nazwę użytkownika.

10. W elemencie `<DefaultCountryCode>` wprowadź swój domyślny kod kraju. Ten kod będzie przypisywany do numerów telefonów zarejestrowanych dla użytkowników bez kodu kraju. Jeśli użytkownik ma międzynarodowy kod kraju, musi on zostać uwzględniony w zarejestrowanym numerze telefonu.

11. Zapisz plik MfaSettings.xml z taką samą nazwą w tej samej lokalizacji.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>Konfigurowanie bramy telefonicznej lub bramy SMS haseł jednorazowych

1.  Uruchom program Internet Explorer i przejdź do portalu MIM, uwierzytelniając się jako administrator programu MIM, a następnie kliknij przycisk **Przepływy pracy** na pasku nawigacyjnym po lewej stronie.

    ![Obraz nawigacji w portalu MIM](media/MIM-SSPR-workflow.jpg)

2.  Zaznacz pole wyboru **Przepływ pracy AuthN w zakresie resetowania hasła**.

    ![Obraz przepływów pracy w portalu MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Kliknij kartę **Działania**, a następnie przewiń w dół do pozycji **Dodaj działanie**.

4.  Wybierz kolejno pozycje **telefon** lub Brama  **SMS hasło** , a następnie kliknij **pozycję Wybierz** , a następnie przycisk **OK**.

Uwaga: Jeśli korzystasz z serwera usługi Azure MFA lub innego dostawcy, który generuje hasło jednorazowe, upewnij się, że pole Długość skonfigurowane powyżej ma taką samą długość jak wygenerowane przez dostawcę usługi MFA.  Ta długość musi wynosić 6 dla serwera usługi Azure MFA.  Serwer usługi Azure MFA generuje również swój własny tekst komunikatu, aby wiadomość SMS z tekstem jest ignorowana.

Użytkownicy w organizacji mogą teraz rejestrować się w celu resetowania haseł.  W trakcie tego procesu będą oni wprowadzać swoje numery telefonów służbowych lub komórkowych, aby umożliwić systemowi ustanawianie połączeń telefonicznych z nimi (lub wysyłanie do nich wiadomości SMS).

#### <a name="register-users-for-password-reset"></a>Rejestrowanie użytkowników w celu resetowania haseł

1.  Użytkownik uruchomi przeglądarkę internetową i przejdź do portalu rejestracji resetowania haseł programu MIM.  (Zazwyczaj ten portal będzie skonfigurowany z uwierzytelnianiem systemu Windows).  W portalu użytkownicy muszą ponownie podać swoją nazwę użytkownika i hasło w celu potwierdzenia ich tożsamości.

    Będą oni monitowani o przejście do portalu rejestracji haseł i uwierzytelnienie się przy użyciu nazwy użytkownika i hasła.

2.  Wymagane będzie wprowadzenie kodu kraju, spacji i numeru telefonu w polu **Numer telefonu** lub **Telefon komórkowy**, a następnie kliknięcie przycisku **Dalej**.

    ![Obraz weryfikacji telefonu w portalu MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Obraz weryfikacji telefonu komórkowego w portalu MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Czy takie rozwiązanie jest odpowiednie dla Twoich użytkowników?
Po skonfigurowaniu i uruchomieniu wszystkich składników można zapoznać się z procedurami resetowania haseł przez użytkowników, którzy po powrocie z urlopu nie pamiętają swoich danych uwierzytelniania.

Istnieją dwa sposoby resetowania hasła i odblokowania konta przez użytkownika (użycie ekranu logowania systemu Windows lub portalu samoobsługowego).

Instalując dodatki i rozszerzenia programu MIM na komputerze przyłączonym do domeny i połączonym za pośrednictwem sieci organizacji z usługą MIM, użytkownicy mogą odzyskać zapomniane hasło na ekranie logowania do komputera.  Poniższe kroki objaśniają ten proces:

#### <a name="windows-desktop-login-integrated-password-reset"></a>Resetowanie hasła zintegrowane z logowaniem do komputera z systemem Windows

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

#### <a name="access-from-the-self-service-portal"></a>Dostęp z portalu samoobsługowego

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
