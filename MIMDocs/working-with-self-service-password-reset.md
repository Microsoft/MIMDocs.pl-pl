---
title: Praca z samoobsługowego resetowania hasła | Dokumentacja firmy Microsoft
description: Dowiedz się, jakie zmiany wprowadzono w funkcji samoobsługowego resetowania hasła (SSPR) w programie MIM 2016, łącznie z obsługą uwierzytelniania wieloskładnikowego.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/30/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 3a86569a8de77f4cf4d5aeafe0cd01dab40232b3
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358470"
---
# <a name="self-service-password-reset-deployment-options"></a>Opcje wdrażania samoobsługowego resetowania hasła

Nowi klienci, którzy są [licencjonowane na potrzeby usługi Azure Active Directory — wersja Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing), firma Microsoft zaleca używanie [usługi Azure AD samoobsługowego resetowania haseł](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks.md) oferują środowisko użytkownika końcowego.  Samoobsługowe haseł usługi Azure AD zapewnia zarówno oparte na sieci web i zintegrowane Windows do użytkownika, aby zresetować własne hasło resetowania i obsługuje wiele tych samych funkcji jak program MIM, w tym alternatywny adres e-mail i bramy pytań i odpowiedzi.  Podczas wdrażania usługi Azure AD samoobsługowego resetowania hasła, program Azure AD Connect obsługuje [zapisania zwrotnego nowych haseł w usługach AD DS](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback.md)i MIM [usługi powiadamiania o zmianie hasła](deploying-mim-password-change-notification-service-on-domain-controller.md) może służyć do przekazywania hasła do innych systemów, takich jak serwer katalogowy innego dostawcy, jak również.  Wdrażanie programu MIM dla [zarządzania hasłami](infrastructure/mim2016-password-management.md) nie wymaga usługi MIM lub portale resetowania lub rejestracji haseł programu MIM do wdrożenia.  Można zamiast niego, wykonaj następujące kroki:

- Pierwsze, jeśli musisz wysyłać hasła do katalogów innych niż Azure AD i AD DS, wdrażania synchronizacji programu MIM za pomocą łączników usług domenowych Active Directory i wszelkie dodatkowe docelowych systemach, konfigurowanie programu MIM dla [zarządzania hasłami](infrastructure/mim2016-password-management.md) i wdrażanie [usługi powiadamiania o zmianie hasła](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Następnie, jeśli musisz wysyłać hasła do katalogów innych niż Usługa Azure AD, skonfiguruj program Azure AD Connect dla [zapisania zwrotnego nowych haseł w usługach AD DS](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback.md).
- Opcjonalnie [użytkowników Zarezerwuj wstępnie](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-authenticationdata.md).
- Na koniec [wdrażanie usługi Azure AD samoobsługowego resetowania hasła użytkownikom końcowym](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-deployment.md).

Dla istniejących klientów, którzy wcześniej wdrożyli Forefront Identity Manager (FIM) dla hasła samoobsługowego resetowania i są licencjonowane na potrzeby usługi Azure Active Directory — wersja Premium, firma Microsoft zaleca planowanie przejścia do haseł usługi Azure AD do zresetowania.  Można przejść użytkownikom końcowym usługi Azure AD Samoobsługowe resetowanie haseł bez konieczności ich ponownie zarejestrować, przez [synchronizowanie lub ustawienie za pomocą programu PowerShell użytkownika alternatywny adres e-mail lub telefon komórkowy numer](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-authenticationdata.md). Po użytkownicy są zarejestrowani do samoobsługowego hasła usługi Azure AD zresetować, portal resetowania hasła FIM można zlikwidować.

W przypadku klientów, które nie zostały jeszcze wdrożone usługi Azure AD Samoobsługowe resetowanie haseł dla swoich użytkowników, program MIM znajdują się również samoobsługowego resetowania haseł portali.  W porównaniu do programu FIM, program MIM 2016 zawiera następujące zmiany:

- Portal samoobsługowego resetowania haseł programu MIM i ekran logowania Windows zezwala użytkownikom odblokowanie ich kont bez konieczności zmieniania swoich haseł.
- Dodano nową bramę uwierzytelniania Phone Gate programu MIM. Dzięki temu uwierzytelniania użytkowników za pomocą połączeń telefonicznych za pośrednictwem usługi Microsoft Azure Multi-Factor Authentication (MFA).

Program MIM 2016 kompilacji wydania do wersji 4.5.26.0 polegać klienta do pobrania usługi Azure Multi-Factor Authentication Software Development Kit (zestaw SDK usługi MFA Azure).  Ten zestaw SDK jest przestarzała i zestawu SDK usługi Azure MFA będą obsługiwane w przypadku istniejących klientów tylko aż do daty wycofania 14 listopada 2018 r. Do tego czasu klienci muszą kontaktować się obsługi klienta platformy Azure do odbierania wygenerowanego pakietu poświadczenia usługi MFA, ponieważ będą one nie można pobrać zestaw SDK usługi Azure MFA. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>NOWOŚĆ! Zaktualizuj bieżącą konfigurację usługi Azure MFA do serwera Azure Multi-Factor Authentication

To [artykułu](working-with-mfaserver-for-mim.md) opisano, jak zaktualizować portal resetowania haseł programu MIM wdrażania i konfiguracji usługi PAM za pomocą usługi Azure Multi-Factor Authentication dla usługi Multi-Factor authentication.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Wdrażanie programu MIM portalu samoobsługowego resetowania hasła przy użyciu usługi Azure MFA, Multi-Factor Authentication

W poniższej sekcji opisano sposób wdrażania portalu resetowania haseł programu MIM przy użyciu usługi Azure MFA, Multi-Factor Authentication.  Te kroki są tylko niezbędne dla klientów, którzy nie używają usługi Azure AD Samoobsługowe resetowanie haseł dla swoich użytkowników.

Microsoft Azure Multi-Factor Authentication jest usługą uwierzytelniania, która wymaga weryfikowania prób zalogowania się przez użytkowników przy użyciu aplikacji mobilnej, połączenia telefonicznego lub wiadomości tekstowej. Jest ona dostępna do użycia z usługą Microsoft Azure Active Directory oraz jako usługa dla aplikacji w chmurze i lokalnych aplikacji przedsiębiorstw.

Usługa Azure MFA zapewnia dodatkowy mechanizm uwierzytelniania wspierający istniejące procesy uwierzytelniania, takie jak proces używany przez program MIM w przypadku pomocy związanej z logowaniem użytkowników korzystających z samoobsługi.

Usługa Azure MFA umożliwia uwierzytelnianie i weryfikację tożsamości użytkowników, którzy usiłują odzyskać dostęp do swoich kont i zasobów. Uwierzytelnianie jest dokonywane przy użyciu usługi SMS lub połączeń telefonicznych.   Im silniejsze uwierzytelnianie, tym większa pewność, że osoba usiłująca odzyskać dostęp jest rzeczywistym użytkownikiem będącym właścicielem danej tożsamości. Po uwierzytelnieniu użytkownik może wybrać nowe hasło, które zastąpi stare hasło.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Wymagania wstępne związane z konfigurowaniem samoobsługowego odblokowywania kont i resetowania haseł przy użyciu usługi MFA

W tej sekcji założono, że pobrane i ukończył wdrożenie programu Microsoft Identity Manager 2016 [synchronizacji programu MIM, usługa MIM i portalu programu MIM składniki](microsoft-identity-manager-deploy.md), w tym następujących składników i usług:

-   System Windows Server 2008 R2 lub nowszy został skonfigurowany jako serwer usługi Active Directory, łącznie z Usługami domenowymi w usłudze Active Directory i kontrolerem domeny z wyznaczoną domeną (domena „firmowa”).

-   Zdefiniowano zasady grupy dla blokowania kont.

-   Usługa synchronizacji programu MIM 2016 (Sync) jest zainstalowana i uruchomiona na serwerze przyłączonym do domeny usługi AD.

-   Usługa i portal programu MIM 2016, w tym portal rejestracji SSPR i portal resetowania SSPR, są zainstalowane i uruchomione na serwerze (mogą znajdować się w tej samej lokalizacji, w której znajduje się usługa synchronizacji).

-   Usługa synchronizacji programu MIM jest skonfigurowana z synchronizacją tożsamości usługi AD programu MIM, w tym:

    -   Konfigurowanie agenta zarządzania Active Directory Management Agent (ADMA) zapewniającego łączność z usługami AD DS oraz możliwość importowania danych tożsamości z usługi Active Directory i eksportowania ich do tej usługi.

    -   Konfigurowanie agenta zarządzania programu MIM (MIM MA) zapewniającego łączność z bazą danych usługi FIM oraz możliwość importowania danych tożsamości z bazy danych usługi FIM i eksportowania ich do tej bazy.

    -   Konfigurowanie reguł synchronizacji w portalu MIM, zezwalających na synchronizację danych użytkowników i ułatwiających działania oparte na synchronizacji w usłudze MIM.

-   Dodatki i rozszerzenia programu MIM 2016, łącznie ze zintegrowanym klientem logowania systemu Windows dla funkcji SSPR, zostały wdrożone na serwerze lub oddzielnym komputerze klienckim.

Ten scenariusz wymaga posiadania licencji CAL programu MIM dla usługi użytkownikom, jak i subskrypcji usługi Azure MFA.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Przygotowanie programu MIM do pracy z uwierzytelnianiem wieloskładnikowym
Skonfiguruj synchronizację programu MIM do obsługi resetowania haseł i odblokowywania kont. Aby uzyskać więcej informacji, zobacz [Instalowanie dodatków i rozszerzeń usługi FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Instalowanie funkcji SSPR usługi FIM](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Bramy uwierzytelniania SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) i [Przewodnik po laboratorium testowym funkcji SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx).

W następnej sekcji zostanie skonfigurowany dostawca usługi Azure MFA w usłudze Microsoft Azure Active Directory. W ramach tego procesu zostanie wygenerowany plik zawierający materiał uwierzytelniania, umożliwiający usłudze MFA kontaktowanie się z usługą Azure MFA.  Aby kontynuować, należy uzyskać subskrypcję Azure.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Rejestrowanie dostawcy uwierzytelniania wieloskładnikowego na platformie Azure

1.  Tworzenie [dostawcę usługi MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider.md).

2. Otwórz zgłoszenie do pomocy technicznej i zażądać bezpośredni zestaw SDK dla programu ASP.net 2.0 C#. Zestaw SDK tylko zostanie udzielona bieżący użytkownicy programu MIM z usługą MFA, ponieważ bezpośredni zestaw SDK jest przestarzała. Nowi klienci powinna przyjąć następnej wersji programu MIM, która integruje się z serwera usługi MFA.

3. Skopiuj wynikowy plik ZIP do każdego systemu, w którym jest zainstalowana usługa MIM.  Należy pamiętać, że plik ZIP zawiera klucz używany do uwierzytelniania w usłudze Azure MFA.

### <a name="update-the-configuration-file"></a>Aktualizowanie pliku konfiguracji

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

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>Konfigurowanie bramy telefonicznej lub bramy SMS haseł jednorazowych

1.  Uruchom program Internet Explorer i przejdź do portalu MIM, uwierzytelniając się jako administrator programu MIM, a następnie kliknij przycisk **Przepływy pracy** na pasku nawigacyjnym po lewej stronie.

    ![Obraz nawigacji w portalu MIM](media/MIM-SSPR-workflow.jpg)

2.  Zaznacz pole wyboru **Przepływ pracy AuthN w zakresie resetowania hasła**.

    ![Obraz przepływów pracy w portalu MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Kliknij kartę **Działania**, a następnie przewiń w dół do pozycji **Dodaj działanie**.

4.  Wybierz pozycję **Phone Gate** lub **One-Time Password SMS Gate**, kliknij przycisk **Wybierz**, a następnie kliknij przycisk **OK**.

Użytkownicy w organizacji mogą teraz rejestrować się w celu resetowania haseł.  W trakcie tego procesu będą oni wprowadzać swoje numery telefonów służbowych lub komórkowych, aby umożliwić systemowi ustanawianie połączeń telefonicznych z nimi (lub wysyłanie do nich wiadomości SMS).

#### <a name="register-users-for-password-reset"></a>Rejestrowanie użytkowników w celu resetowania haseł

1.  Użytkownicy uruchamiają przeglądarkę sieci Web i przechodzą do portalu rejestracji na potrzeby resetowania haseł programu MIM.  (Zazwyczaj ten portal będzie skonfigurowany z uwierzytelnianiem systemu Windows).  W portalu użytkownicy muszą ponownie podać swoją nazwę użytkownika i hasło w celu potwierdzenia ich tożsamości.

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

5.  ! [MIM ac
6.  Liczba pomyślnie odblokowanego image](media/MIM-SSPR-account-unlock.JPG)

6.  Jeśli użytkownik wybierze opcję resetowania hasła, będzie musiał wpisać nowe hasło dwa razy i kliknąć przycisk **Dalej**, aby zmienić hasło.
