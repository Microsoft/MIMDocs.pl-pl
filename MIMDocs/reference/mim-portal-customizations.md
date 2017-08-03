---
title: Dostosowywanie programu Microsoft Identity Manager 2016 Portal | Dokumentacja firmy Microsoft
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Dostosowywania portalu programu Microsoft Identity Manager 2016


>[!Warning]
Pamiętaj wyczyścić pamięć podręczną przeglądarki, gdy zostaną wprowadzone wszystkie dostosowania CSS.

W Microsoft Identity Manager 2016 (MIM), można dostosować wybrane elementy do portali hasła, łącznie z Baner logo i zasoby ciągów i kaskadowych arkuszy stylów.

W tym celu kilka ustawień są wymagane w zależności od poziomu dostosowywania. Poniżej znajduje się lista elementów, które są zaangażowane w dostosowywanie rejestracji haseł programu MIM 2016 i zresetuj portali.

-   Folder dostosowania - to jest folder, który będzie sprawdzać MIM 2016, przed użyciem wartości domyślnych. Każdy portal, który będzie można dostosować wymaga folderu dostosowania. Dostosowania należy przypisywać tylko wtedy w tym folderze, ponieważ Instalator nie będzie go zastąpić na uaktualnienia, Zmień tryb instalacji lub naprawy instalacji w trybie.

-   Strings.resources — jest to plik opartych na języku XML, który umożliwia modyfikowanie ciągów, które są wyświetlane w portalu. Ten plik musi znajdować się w folderze dostosowania.

-   Style.CSS — jest to kaskadowy arkusz stylów używany przez portali do dostosowania. Ten arkusz stylów musi zostać utworzony i zmodyfikować, aby zmienić logo lub można całkowicie zastąpić własne dostosowania.

Aby uzyskać szczegółowe instrukcje krok po kroku dotyczące dostosowywania resetowania hasła rejestracji i hasło portali zobacz Przewodnik po laboratorium testowym: prezentacja 2016 rejestracji haseł i dostosowywania portalu resetowania.

>[! WARGNING] Aby MIM do rozpoznawania niestandardowych zmiany, należy ponownie uruchomić usługi IIS, uruchamiając polecenie iisreset.


## <a name="creating-the-customizations-folder"></a>Tworzenie folderu dostosowania

Uruchamianie MIM będzie szukał pliku Strings.resources w folderze dostosowania przed użyciem wartości domyślnych. Należy utworzyć folder dostosowań w katalogu dla portalu, który chcesz dostosować (tj. portalu rejestracji haseł lub portalu resetowania haseł). Jeśli chcesz dostosować obu portalach następnie należy utworzyć folder dostosowań w każdej z następujących czynności:

-   C:\\pliki programów\\Microsoft Forefront Identity Manager\\2010\\portalu rejestracji haseł

-   C:\\pliki programów\\Microsoft Forefront Identity Manager\\2010\\Portal resetowania hasła

### <a name="to-create-the-customization-folder"></a>Aby utworzyć folder dostosowywania

1.  Przejdź do dysku C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\folderu portalu rejestracji haseł.

2.  Utwórz folder o nazwie dostosowania.

3.  Przejdź wstecz jeden poziom do folderu portalu resetowania haseł, a następnie utwórz folder o nazwie dostosowania.

## <a name="customizing-strings"></a>Dostosowywanie ciągów

Wiele ciągów w portalu interfejsu użytkownika można dostosować, tworząc plik Strings.resources i dodać ten plik do folderu dostosowania. Należy utworzyć plik Strings.resources dla każdego portalu, który chcesz dostosować.

### <a name="to-customize-strings"></a>Aby dostosować ciągów

1.  Za pomocą Notatnika, skopiuj do niego następujący kod poniżej i zapisz go w folderze dostosowania jako Strings.resources

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  W obszarze `<!-- Customizations begin here -->` sekcji Zmień nazwę danych odpowiadające ciągów, które chcesz dostosować i wprowadź wartość dla tego ciągu między <value> </value> tagów. Zobacz poniższą listę ciągów, które można dostosować i ich wartości domyślne.

>[!NOTE]
**Strings.resources** pliku jest niezależny od języka. Można utworzyć języka określonych ciągów dostosowane, należy mieć zainstalowany ten pakiet językowy i Zapisz plik w formacie ciągów. `<language>-<culture>.resources`, na przykład Strings.en-us.resources.

Poniżej znajduje się lista portalu ciągów, które można dostosowywać.

<a name="portal-strings"></a>Ciągi portalu
--------------

| Nazwa                                                     | Wartość domyślna                                                                                                                                                                                                                                                                                                                                         | Komentarz                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Informacje         |       |
| ButtonCancel                                             | Anuluj                                                                                 |     |
| ButtonFinish                                             | Zakończ    |    |
| ButtonNext                                               | Dalej    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | Sesja nie jest już aktywne. W przypadku zamknięcia okna lub uruchomić ponownie, klikając poniższe łącze.         |      |
| CancelFinishedTitle                                      | Sesja została zakończona                                                              |      |
| ErrorDescription_3000                                    | Wystąpił błąd. Spróbuj ponownie, a jeśli problem będzie się powtarzać, skontaktuj się z administratorem pomocy technicznej lub systemu. (Błąd 3000)       |          |
| ErrorDescription_3001                                    | Upewnij się, że poprawnie wprowadź nazwę użytkownika. Jeśli nadal nie możesz zresetować hasło, skontaktuj się z sieci      |           |
|                                                          | Pomoc techniczna w celu uzyskania pomocy. (Błąd 3001)   |                   |
| ErrorDescription_3002                                    | Twoja sesja została zakończona. Wróć do strony głównej, aby uruchomić ponownie. (Błąd 3002)                                     |                |
| ErrorDescription_3003                                    | Bieżące konto użytkownika nie jest rozpoznawany przez program Forefront Identity Manager.Please skontaktuj się z administratorem pomocy technicznej lub systemu. (Błąd 3003)           |               |
| ErrorDescription_3004                                    | Nie masz uprawnień do rejestracji w celu resetowania haseł. Skontaktuj się z pomocą techniczną lub administratorem systemu. (Błąd 3004)     |              |
| ErrorDescription_3005                                    | Co najmniej jednego z podanych odpowiedzi nie zgadzają się odpowiedzi podanych podczas kolejności Registration.In hasła można zresetować hasło, podane teraz odpowiedzi muszą pasować do odpowiedzi, podanych podczas rejestracji. Możesz rozpocząć ponownie na stronie głównej lub skontaktuj się z administratorem pomocy technicznej lub systemu. (Błąd 3005) |           |
| ErrorDescription_3006                                    | Wprowadzone hasło jest niepoprawne. Aby zarejestrować do resetowania hasła, należy wprowadzić poprawne hasło. (Błąd 3006)            |              |
| ErrorDescription_3007                                    | Podlegasz tymczasowemu zakazowi resetowania hasła. Spróbuj ponownie później lub skontaktuj się z administratorem pomocy technicznej lub systemu, aby uzyskać pomoc. (Błąd 3007)     |         |
| ErrorDescription_3008                                    | Wystąpił błąd. Spróbuj ponownie, a jeśli problem będzie się powtarzać, skontaktuj się z administratorem pomocy technicznej lub systemu. (Błąd 3008)        |          |
| ErrorDescription_3009                                    | Wprowadzone dane zawierają tekst w formacie, który nie jest dozwolone. Spróbuj ponownie przy użyciu różnych danych wejściowych, lub skontaktuj się z administratorem pomocy technicznej lub systemu. (Błąd 3009)     |             |
| ErrorDescription_3010_Registration                       | Nie jest włączona obsługa skryptów w przeglądarce. Włączyć obsługę skryptów i wróć do strony głównej rejestracji hasła lub skontaktuj się z działu pomocy technicznej, aby uzyskać pomoc.      |               |
| ErrorDescription_3010_Reset                              | Nie jest włączona obsługa skryptów w przeglądarce. Włączyć obsługę skryptów i wróć do strony głównej resetowania hasła lub skontaktuj się z działu pomocy technicznej, aby uzyskać pomoc.     |            |
| ErrorDescription_3011                                    | Ta witryna korzysta z plików cookie. Skonfiguruj przeglądarkę tak, aby akceptowała pliki cookie i spróbuj ponownie lub skontaktuj się z działu pomocy technicznej, aby uzyskać pomoc.          |                                           |
| ErrorDescription_3012                                    | Wprowadzone dane nie odpowiadało kodu zabezpieczeń, który został wysłany do Ciebie. Spróbuj zresetować hasło ponownie lub skontaktuj się z działu pomocy technicznej, aby uzyskać pomoc.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Nie można wysłać kodu zabezpieczeń. Aby uzyskać pomoc, skontaktuj się z działu pomocy technicznej.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Wprowadź nazwę użytkownika w poprawnym formacie.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Wprowadź nazwę użytkownika, aby kontynuować.                                              |                         |
| ErrorMessagePasswordRequired                             | Wprowadź hasło.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Upewnij się, że oba hasła pasują do siebie.                                |           |
| ErrorPageDefaultHeading                                  | Błąd aplikacji                                            |               |
| ErrorPageServerTime                                      | Czas serwera: {0:T}                     | {0} to czas, w którym wyjątek wystąpił. T "powoduje, że przekazany w czasie być w formacie"długo." Spowoduje utworzenie godzinę, minutę i sekundę, oraz prawdopodobnie oznaczenia AM/PM (w zależności od bieżącej kultury). |
| ErrorPageTitle                                           | Forefront Identity Management — Błąd hasła                     |   |
| ErrorTitle_3000                                          | Błąd                                  |  |
| ErrorTitle_3001                                          | Odmowa dostępu                          |  |
| ErrorTitle_3002                                          | Sesja została zakończona                          |  |
| ErrorTitle_3003                                          | Nierozpoznany użytkownika                      |  |
| ErrorTitle_3004                                          | Nieautoryzowany użytkownik                      |  |
| ErrorTitle_3005                                          | Odpowiedzi nie są zgodne                    |  |
| ErrorTitle_3006                                          | Nieprawidłowe hasło                     |  |  
| ErrorTitle_3007                                          | Tymczasowo odmowa dostępu              |  |
| ErrorTitle_3008                                          | Błąd komunikacji                    |  |
| ErrorTitle_3009                                          | Zabronione dane wejściowe                       |  |
| ErrorTitle_3010                                          | Błąd konfiguracji przeglądarki            |  |
| ErrorTitle_3011                                          | Błąd konfiguracji przeglądarki            |  |
| ErrorTitle_3012                                          | Weryfikacja nie powiodła się                    |  |
| ErrorTitle_3013                                          | Nie można wysłać kodu zabezpieczeń   |  |
| FinalizeRegistrationHeading1                             | Jeśli kiedykolwiek zajdzie potrzeba zresetowania hasła:        |   |
| FinalizeRegistrationSubHeading1                          | Przejdź do portalu resetowania haseł   |   |
| FinalizeRegistrationSubHeading2                          | Weryfikacja tożsamości   |   |
| FinalizeRegistrationSubHeading3                          | Wybierz nowe hasło    |     |
| FinishingDescription                                     | Wybierz nowe hasło        |    |
| FinishingTitle                                           | Resetowanie hasła:      |     |
| GotoPortalPrefix                                         | Przejdź do     |        |
| GotoPortalSuffix                                         | Strona główna     |      |
| LabelTroubleshootingLinkText                             | Wyświetl szczegóły           |    |
| LoadingText                                              | Trwa ładowanie...     |  |
| NoScriptTagErrorMessage                                  | Nie jest włączona obsługa skryptów w przeglądarce. Włączyć obsługę skryptów i wróć do strony głównej lub skontaktuj się z działu pomocy technicznej, aby uzyskać pomoc.      |   |
| PasswordResetOperationGeneralErrorMessage                | Wystąpił błąd podczas próby zresetowania hasła.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | Hasło nie jest zgodne z zasadami haseł w organizacji.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Błąd podczas resetowania hasła, użytkownik nie może zmienić hasła.    |   |
| PrivacyStatement                                         | Zasady zachowania poufności informacji                                                      |    |
| RegistrationDescription                                  | Rejestracja hasła samoobsługi     |    |
| RegistrationMission                                      | Jeśli zapomnisz hasło, możesz przywrócić jego samodzielnie bez wywoływania dział pomocy technicznej.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management - rejestracji haseł                                          |   |
| RegistrationSteps                                        | Kliknij przycisk "Dalej", aby rozpocząć proces rejestracji.   |      |
| RegistrationSuccessDescription                           | Jesteś teraz zarejestrowany        |   |
| RegistrationSuccessTitle                                 | Ukończone:   |    |
| RegistrationWelcomeTitle                                 | Rejestracja hasła:    | |
| ResetDescription                                         | Samoobsługowe resetowanie hasła  |    |
| ResetEnterNamePrompt                                     | Wprowadź poniżej swoją nazwę użytkownika     |     |
| ResetEnterPassword                                       | Wprowadź nowe hasło:                                                  |   |
| ResetExample1                                            | Contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Przykłady:  |    |
| ResetPageTitle                                           | Forefront Identity Management - resetowania hasła       |     |
| ResetReenterPassword                                     | Wprowadź ponownie hasło:    | |
| ResetSuccessDescription                                  | Hasło zostało zresetowane.    |    |
| ResetSuccessTitle                                        | Powodzenie:                                |    |
| ResetUseNewPassword                                      | Można teraz używać nowego hasła do logowania się w.     |      |
| ResetUsernameTextFormat                                  | (Resetowanie hasła do {0})       | {0} jest logowania użytkownika             |
| ResetWelcomeTitle                                        | Resetowanie hasła:     |      |
| TroubleshootingEmailSubject                              | Szczegóły błędu przetwarzanie żądań FIM     |       |
| TroubleshootingLabelAttributes                           | Atrybuty:    |    |
| TroubleshootingLabelCloseButton                          | Zamknij       |    |
| TroubleshootingLabelCopyToClipboard                      | Kopiuj do schowka     |     |
| TroubleshootingLabelCorrelationId                        | Identyfikator korelacji:      |          |
| TroubleshootingLabelDetails                              | Szczegóły:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | Informacje został skopiowany do Schowka.           |    |
| TroubleshootingLabelRequestId                            | Identyfikator żądania:                  |    |
| TroubleshootingLabelSendEmail                            | Wyślij informacje pocztą E-mail                            |    |
| TroubleshootingLabelSource                               | Przyczyna:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Wyświetl szczegóły żądania        |              |                                                    
| TroubleshootingLinkText                                  | Informacje dotyczące rozwiązywania problemów          |                  | |



Oto lista ciągów bramę uwierzytelniania, które można dostosowywać.

<a name="authentication-gate-strings"></a>Ciągi bramy uwierzytelniania
---------------------------

| Nazwa                                          | Wartość domyślna                                                                                                                                                                                                                | Commnent |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | Adres e-mail:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | Pole adresu e-mail nie może być pusta.     |          |
| OTPEmailRegistrationFooterReadOnly            | Aby zaktualizować adres e-mail, Przeprowadź proces określony przez organizację lub zadzwoń do pomocy technicznej.                   |          |
| OTPEmailRegistrationFooterReadWrite           | Adres e-mail jest przechowywany przez organizację w usłudze Forefront Identity Manager.                       |          |
| OTPEmailRegistrationGateTitle                 | Weryfikacja adresu e-mail   |          |
| OTPEmailRegistrationHeaderReadOnly            | Jeśli trzeba będzie zresetować hasło, weryfikacyjny kod zabezpieczeń będą wysyłane do poczty e-mail. Jeśli podany poniżej adres e-mail jest niepoprawny, musisz poprawić w celu używania samoobsługowego resetowania hasła. |          |
| OTPEmailRegistrationHeaderReadWrite           | Wprowadź poniżej adres e-mail. Jeśli trzeba będzie zresetować hasło, do poczty e-mail zostanie przesłany kod weryfikacyjny.                 |          |
| OTPEmailResetGateTitle                        | Weryfikacja tożsamości: wiadomości E-mail         |          |
| OTPEmailResetHeader                           | Wprowadź poniżej kod zabezpieczeń. Kod zabezpieczeń został wysłany na adres e-mail zarejestrowany przez tę organizację.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | Określona wartość nie jest zgodny z formatem oczekiwanym.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | Pole kodu zabezpieczeń nie może być pusta.        |          |
| OTPResetVerificationLabel                     | Kod zabezpieczeń:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | Aby zaktualizować numer telefonu komórkowego, Przeprowadź proces określony przez organizację lub zadzwoń do pomocy technicznej.   ||
| OTPSmsRegistrationFooterReadWrite                     | Numer telefonu komórkowego jest przechowywany przez organizację w usłudze Forefront Identity Manager.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Weryfikacja telefonu komórkowego                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | Jeśli trzeba będzie zresetować hasło, weryfikacyjny kod zabezpieczeń zostanie wysłany na telefon komórkowy. Jeśli numer telefonu komórkowego, pokazano poniżej nie jest poprawna, należy poprawić w celu używania samoobsługowego resetowania hasła. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Wprowadź poniżej numer telefonu komórkowego. Jeśli trzeba będzie zresetować hasło, na telefon komórkowy zostanie przesłany kod weryfikacyjny.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | Pole numeru telefonu komórkowego nie może być pusta.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Telefon komórkowy:                    |   |
| OTPSmsResetGateTitle                                  | Weryfikacja tożsamości: telefon komórkowy         |   |
| OTPSmsResetHeader                                     | Wprowadź poniżej kod zabezpieczeń. Kod zabezpieczeń został wysłany na telefon komórkowy zarejestrowany przez tę organizację.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Wprowadź poniżej swoje bieżące hasło, a następnie kliknij przycisk 'Dalej'.        |   |
| PasswordGateErrorMessagePasswordRequired              | Wprowadź bieżące hasło.                  |   |
| PasswordGateGateTitle                                 | Bieżące hasło               |   |
| PasswordGatePasswordLabelText                         | Hasło:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(zalogowany jako:`<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | Należy odpowiedzieć na co najmniej pytania {0}.        |   |
| QAGateIncorrectAnswer                                 | Twoje odpowiedzi są nieprawidłowe.       |   |
| QAGatePrivacyNotice                                   | Udzielane odpowiedzi są przechowywane przez organizację w usłudze Forefront Identity Manager.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Należy odpowiedzieć na co najmniej pytania {0} do zarejestrowania.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Co najmniej jedna odpowiedź nie są zgodne z zasadami.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | Tej odpowiedzi nie jest zgodne z zasadami.      |   |
| QAGateRegistrationTitle                               | Zarejestruj odpowiedzi         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | Musisz odpowiedzieć {0} z poniższych pytań {1}.   |   |
| QAGateResetTitle                                      | Weryfikacja tożsamości: Przesyłanie odpowiedzi                                  |   |



## <a name="customizing-the-logo-banner"></a>Dostosowywanie Baner logo

Transparent domyślny na stronach portalu można dostosować dla Twojej organizacji.
Aby dostosować Baner logo:

1.  Utwórz użytkownika niestandardowego transparentach i zapisać je jako pliki PNG. Pliki powinny spełniać następujące zalecenia:

 - Rozmiar: 490 X 50 pikseli.

 - Głębia bitowa: 32

2.  Skopiuj pliki do \\folderu dostosowań w każdym portalu, który chcesz dostosować.

3.  Utwórz plik Style.css w każdym folderze. Ma ona punktu w folderze dostosowania i nowe logo... Nazwa logo mogą ulec zmianie, jeśli ma to zastosowanie (tj. /Customizations/contosologo.png). Kod powinien wyglądać następująco:

   **Przykład 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Jeśli korzystasz z programu Internet Explorer 6.0, należy zapewnić alternatywne logo nieprzezroczystych i Dodaj następujący kod do Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Przykład 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Jeśli korzystasz z programu Internet Explorer 6.0, należy zapewnić alternatywne logo nieprzezroczystych i Dodaj następujący kod do Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Dostosowanie obrazu dla Smartfonów

Obraz dla Smartfonów można dostosować, korzystając z następujących czynności. Aby dostosować obraz SmartPhone:

1.  Tworzenie obrazu i zapisać je jako pliki PNG. Pliki powinny spełniać następujące zalecenia:

    - Rozmiar: 190 X 50 pikseli.
    - Głębia bitowa: 32

2.  Skopiuj pliki do \\folderu dostosowań w każdym portalu, który chcesz dostosować.

2.  Utwórz plik Style.css w każdym folderze. Ma ona punktu w folderze dostosowania i nowe logo... Nazwa logo mogą ulec zmianie, jeśli ma to zastosowanie (tj. /Customizations/contosologo.png). Kod powinien wyglądać następująco:

  **Przykład 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>Dostosowywanie arkusze stylów

Można zmodyfikować układu i stylu portali hasła przy użyciu dostosowanych kaskadowy arkusz stylów (CSS). Aby używać dostosowanego CSS:

1.  Utwórz niestandardowe pliki CSS i zapisać je jako Style.css.

2.  Skopiuj pliki do \\folderu dostosowań w każdym portalu, który chcesz dostosować.

Poniżej przedstawiono podstawowe przykładowy plik Style.css:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
Aby MIM do rozpoznawania niestandardowych zmiany należy ponownie uruchomić usługi IIS, uruchamiając polecenie iisreset.                                                                                                                                                                                       

Poniżej znajduje się bardziej zaawansowanych przykładowy plik Style.css. Ten plik zawiera smartphone i ipad informacji specyficznych dla wyświetlanie portali na tych urządzeniach.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Typowe problemy dostosowania

Poniższa tabela znajduje się lista znanych typowych problemów, które mogą wystąpić z Uaktualnianie portalu i usługi programu FIM. Ta tabela zawiera także rozwiązania dla tego problemu

|Problem |Rozwiązanie                                                                    |
|------|------------------------------|
| Wprowadzone dostosowania ciągu, ale go nie zostaną uwzględnione w Interfejsie użytkownika         | Ciąg dostosowań w strings.resources zawsze wymagają iisreset         |
| Po wprowadzeniu zmian strings.resources, nie widzę mojego zmiany ciąg już     | Strings.resources format jest prawdopodobnie uszkodzony i dlatego jest ignorowana przez portal. Sprawdź dziennik zdarzeń w obszarze dzienniki systemu Windows — Dzienniki aplikacji i usług — Forefront Identity Manager                        |
| Po raz pierwszy dodane Style.css, nie są wyświetlane w portalu zmiany wprowadzone w stylu            | Podczas pierwszego wprowadzenia plik Style.css, należy wykonać polecenie iisreset     |
| Nowe style są dodawane/zmodyfikowane w Style.css, ale zmiany nie są widoczne w przeglądarce      | Wyczyść pamięć podręczną przeglądarki i Odśwież stronę <br/> Sprawdzanie składni CSS    |
| Bezpośrednio po zmianie zawartości folderu CSS w path_to_sspr_portal\\css\\\*.css lub Baner logo w path_to_sspr_portal\\obrazów\\fimlogo.png, utratę tych zmian na uaktualnienie | Nigdy nie należy zmieniać te pliki rozpoczynać się od znaku. Tylko przy użyciu folder dostosowywania jako sposób zapewnić Baner logo i CSS styl dostosowań w Style.css. Folder dostosowania celowo nie zostanie zastąpiona ważne uaktualnienia <br/><br/>Nie należy próbować użyć narzędzia, takie jak ILSpy i reflektor, aby zmienić ciągi w zestawach portalu. Użyj strings.resources, aby przesłonić domyślne parametry. Zestawy są zamieniane na uaktualnienie  |
| Baner logo nie jest wyświetlany w portalach / nadal widoczne logo usługi FIM     | Nazwa obrazu/ścieżki w Style.css jest nieprawidłowy lub nie został wyczyszczony pamięci podręcznej przeglądarki          |
| Baner logo wygląda ugly w przeglądarkach IE6       | Konieczne będzie podanie nieprzezroczystych obrazu IE6 i stylu towarzyszący specjalne w style.css        |
