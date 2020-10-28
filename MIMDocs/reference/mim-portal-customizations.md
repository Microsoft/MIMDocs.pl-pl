---
title: Dostosowania portalu Microsoft Identity Manager 2016 | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 144ce2344327f16f60269b4f505c30fd470e46da
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760816"
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Dostosowywanie portalu Microsoft Identity Manager 2016

W Microsoft Identity Manager 2016 (MIM) można dostosować wybrane elementy portali haseł, w tym logo transparentu, zasoby ciągów i kaskadowe arkusze stylów.

>[!WARNING]
>Zawsze czyść pamięć podręczną przeglądarki w przypadku dokonania dowolnych dostosowań CSS.

Do dostosowywania portali rejestracji i resetowania haseł programu MIM 2016 służą następujące elementy:

- Folder dostosowania: jest to folder, który program MIM 2016 sprawdza przed użyciem wartości domyślnych. Każdy Portal, który ma zostać dostosowany, wymaga folderu dostosowania. Dostosowania należy wykonać tylko w tym folderze, ponieważ proces instalacji nie zastępuje tego folderu w przypadku uaktualnień, instalacji w trybie zmiany lub instalacji w trybie naprawy.

- Ciągi. Resources: jest to plik oparty na języku XML, który umożliwia modyfikowanie ciągów, które pojawiają się w portalu. Ten plik musi znajdować się w folderze customizations.

- Style. CSS: jest to kaskadowy arkusz stylów używany przez portale do dostosowywania. Utwórz i zmodyfikuj ten arkusz stylów, aby zmienić logo. Możesz również zastąpić całą zawartość arkusza stylów własnymi dostosowaniami.

Aby uzyskać szczegółowe instrukcje krok po kroku dotyczące dostosowywania rejestracji haseł i portali resetowania haseł, zobacz Przewodnik po laboratorium testowym: wykazanie sposobu rejestracji i resetowania hasła w programie MIM 2016.

>[!WARNING]
>Aby program MIM rozpoznał wszelkie dostosowane zmiany, należy uruchomić ponownie usługi IIS, uruchamiając polecenie `iisreset` .


## <a name="customizations-folder"></a>Folder dostosowania

Po uruchomieniu program MIM szuka plików String. resources w folderze customizations przed użyciem wartości domyślnych. Utwórz folder dostosowania w katalogu dla portalu, który chcesz dostosować (czyli Portal rejestracji haseł lub portalu resetowania haseł). Jeśli chcesz dostosować oba portale, Utwórz folder dostosowania w każdej z następujących lokalizacji:

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`

Aby utworzyć folder dostosowania dla portalu rejestracji haseł:

1. Przejdź do folderu `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`.
   
2. Utwórz folder o nazwie **dostosowania** .

Aby utworzyć folder dostosowania dla portalu resetowania haseł:

1. Przejdź do folderu `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`.
   
2. Utwórz folder o nazwie **dostosowania** .


## <a name="custom-strings-in-the-stringsresources-file"></a>Niestandardowe ciągi w pliku String. resources

Wiele ciągów w interfejsie użytkownika portalu można dostosować, tworząc plik String. resources i dodając ten plik do folderu customizations. Utwórz plik String. resources dla każdego portalu, który chcesz dostosować.

Aby utworzyć niestandardowe ciągi w pliku String. Resources:

1. W Notatniku Skopiuj poniższy kod do pliku String. resources i wklej go. Zapisz plik w folderze dostosowania dla portalu.

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

2.  W `<!-- Customizations begin here -->` sekcji kodu Zmień nazwę danych tak, aby odpowiadała ciągom, które chcesz dostosować. Wprowadź wartość ciągu między `<value></value>` tagami. Zobacz następne sekcje dla ciągów, które mogą być dostosowane i ich wartości domyślne.

>[!NOTE]
>Plik String. Resources jest niezależny od języka. Aby utworzyć niestandardowe ciągi specyficzne dla języka, należy zainstalować ten pakiet językowy i zapisać plik w formacie ciągów `<language>-<culture>.resources` . Na przykład w przypadku kultury języka angielskiego nazwa pliku to **ciągi. pl-US. resources** .


## <a name="portal-strings"></a>Ciągi portalu
W poniższej tabeli przedstawiono ciągi portalu, które mogą być dostosowane:

<!-- The default values are actual UI strings and should not copy edited. -->

| Nazwa ciągu portalu | Wartość domyślna |
|---|---|
| AboutLinkText                                            | Informacje  |
| ButtonCancel                                             | Anuluj |
| ButtonFinish                                             | Zakończ |
| ButtonNext                                               | Następne   |
| ButtonOk                                                 | OK     |
| CancelFinishedMessage                                    | Sesja nie jest już aktywna. Możesz zamknąć okno lub można uruchomić ponownie, klikając poniższe łącze. |
| CancelFinishedTitle                                      | Sesja zakończona |
| ErrorDescription_3000                                    | Wystąpił błąd. Spróbuj ponownie, a jeśli problem będzie się powtarzał, skontaktuj się z pomocą techniczną lub administratorem systemu. (Błąd 3000) |
| ErrorDescription_3001                                    | Upewnij się, że nazwa użytkownika została wprowadzona prawidłowo. Jeśli nadal nie możesz zresetować hasła, skontaktuj się z pomocą techniczną w celu uzyskania pomocy. (Błąd 3001) |
| ErrorDescription_3002                                    | Sesja została zakończona. Wróć do strony głównej, aby zacząć od nowa. (Błąd 3002) |
| ErrorDescription_3003                                    | Konto bieżącego użytkownika nie jest rozpoznawane przez program Forefront Identity Manager. Skontaktuj się z działem pomocy technicznej lub administratorem systemu. (Błąd 3003) |
| ErrorDescription_3004                                    | Nie masz uprawnień do rejestracji w celu zresetowania hasła. Skontaktuj się z działem pomocy technicznej lub administratorem systemu. (Błąd 3004) |
| ErrorDescription_3005                                    | Co najmniej jedna podana odpowiedź nie jest zgodna z odpowiedziami podanymi podczas rejestracji hasła. W celu zresetowania hasła podane odpowiedzi muszą być zgodne z odpowiedziami podanymi podczas rejestracji. Możesz zacząć od strony głównej lub skontaktować się z pomocą techniczną lub administratorem systemu. (Błąd 3005) |
| ErrorDescription_3006                                    | Wprowadzone hasło jest niepoprawne. Aby zarejestrować się w celu zresetowania hasła, należy wprowadzić prawidłowe hasło. (Błąd 3006) |
| ErrorDescription_3007                                    | Użytkownik nie jest tymczasowo zabroniony do resetowania hasła. Spróbuj ponownie później lub skontaktuj się z pomocą techniczną lub administratorem systemu w celu uzyskania pomocy. (Błąd 3007) |
| ErrorDescription_3008                                    | Wystąpił błąd. Spróbuj ponownie, a jeśli problem będzie się powtarzał, skontaktuj się z pomocą techniczną lub administratorem systemu. (Błąd 3008) |
| ErrorDescription_3009                                    | Dane wejściowe zawierają tekst w formacie, który jest niedozwolony. Spróbuj ponownie, podając inne dane wejściowe, lub skontaktuj się z pomocą techniczną lub administratorem systemu. (Błąd 3009) |
| ErrorDescription_3010_Registration                       | Obsługa skryptów nie jest włączona w przeglądarce. Włącz obsługę skryptów i wróć do strony głównej rejestracji hasła lub skontaktuj się z działem pomocy technicznej w celu uzyskania pomocy. |
| ErrorDescription_3010_Reset                              | Obsługa skryptów nie jest włączona w przeglądarce. Włącz obsługę skryptów i wróć do strony głównej resetowania hasła lub skontaktuj się z działem pomocy technicznej w celu uzyskania pomocy. |
| ErrorDescription_3011                                    | Ta witryna używa plików cookie. Skonfiguruj swoje przeglądarki do akceptowania plików cookie i spróbuj ponownie lub skontaktuj się z działem pomocy technicznej w celu uzyskania pomocy. |
| ErrorDescription_3012                                    | Wprowadzone dane są niezgodne z kodem zabezpieczeń, który został wysłany do Ciebie. Możesz spróbować ponownie zresetować hasło lub skontaktować się z działem pomocy technicznej w celu uzyskania pomocy.  |
| ErrorDescription_3013                                    | Nie można wysłać kodu zabezpieczeń. Skontaktuj się z działem pomocy technicznej, aby uzyskać pomoc. |
| ErrorMessageDomainUsernameFormat                         | Wprowadź swoją nazwę użytkownika w poprawnym formacie. |
| ErrorMessageDomainUsernameRequired                       | Wprowadź nazwę użytkownika, aby kontynuować. |
| ErrorMessagePasswordRequired                             | Wprowadź hasło. |
| ErrorMessagePasswordsDoNotMatch                          | Upewnij się, że oba hasła pasują do siebie. |
| ErrorPageDefaultHeading                                  | Błąd aplikacji <br/>**Uwaga** : po znaku następuje znak "=" i komunikat o błędzie.
| ErrorPageServerTime                                      | Czas serwera: {0:T} <br/>**Uwaga** : {0} to godzina, o której został przechwycony wyjątek. "T" powoduje, że przeszedł czas do sformatowania jako "długi czas", pokazując godzinę, minutę i sekundę. Oznaczenie AM/PM jest również wyświetlane w zależności od bieżącej kultury. |
| ErrorPageTitle                                           | Forefront Identity Management — błąd hasła | 
| ErrorTitle_3000                                          | Błąd |
| ErrorTitle_3001                                          | Odmowa dostępu |
| ErrorTitle_3002                                          | Sesja zakończona |
| ErrorTitle_3003                                          | Nierozpoznany użytkownik |
| ErrorTitle_3004                                          | Nieautoryzowany użytkownik |
| ErrorTitle_3005                                          | Odpowiedzi nie są zgodne |
| ErrorTitle_3006                                          | Nieprawidłowe hasło |
| ErrorTitle_3007                                          | Tymczasowe odmowa dostępu |
| ErrorTitle_3008                                          | Błąd komunikacji |
| ErrorTitle_3009                                          | Zabronione dane wejściowe |
| ErrorTitle_3010                                          | Błąd konfiguracji przeglądarki |
| ErrorTitle_3011                                          | Błąd konfiguracji przeglądarki |
| ErrorTitle_3012                                          | Weryfikacja nie powiodła się |
| ErrorTitle_3013                                          | Nie można wysłać kodu zabezpieczeń |
| FinalizeRegistrationHeading1                             | Jeśli kiedykolwiek zajdzie potrzeba zresetowania hasła: |
| FinalizeRegistrationSubHeading1                          | Przejdź do portalu resetowania hasła |
| FinalizeRegistrationSubHeading2                          | Weryfikowanie tożsamości |
| FinalizeRegistrationSubHeading3                          | Wybierz nowe hasło |
| FinishingDescription                                     | Wybierz nowe hasło |
| FinishingTitle                                           | Resetowanie hasła: |
| GotoPortalPrefix                                         | Przejdź do |
| GotoPortalSuffix                                         | Strona główna |
| LabelTroubleshootingLinkText                             | Wyświetl szczegóły |
| LoadingText                                              | Trwa ładowanie... |
| NoScriptTagErrorMessage                                  | Obsługa skryptów nie jest włączona w przeglądarce. Włącz obsługę skryptów i wróć do strony głównej lub skontaktuj się z działem pomocy technicznej w celu uzyskania pomocy.  |
| PasswordResetOperationGeneralErrorMessage                | Wystąpił błąd podczas próby zresetowania hasła. |
| PasswordResetOperationPolicyViolationErrorMessage        | Hasło jest niezgodne z zasadami haseł obowiązującymi w organizacji. |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Wystąpił błąd podczas resetowania hasła, użytkownik nie może zmienić hasła. |
| PrivacyStatement                                         | Zasady zachowania poufności informacji |
| RegistrationDescription                                  | Rejestracja hasła Self-Service |
| RegistrationMission                                      | Jeśli kiedykolwiek zapomnisz hasła, możesz zresetować je samodzielnie bez wywoływania pomocy technicznej. |
| RegistrationPageTitle                                    | Forefront Identity Management — Rejestracja hasła |
| RegistrationSteps                                        | Kliknij przycisk "dalej", aby rozpocząć proces rejestracji. |
| RegistrationSuccessDescription                           | Jesteś teraz zarejestrowany |
| RegistrationSuccessTitle                                 | Ukończono: |
| RegistrationWelcomeTitle                                 | Rejestracja hasła: |
| ResetDescription                                         | Samoobsługowe resetowanie hasła |
| ResetEnterNamePrompt                                     | Wprowadź poniżej swoją nazwę użytkownika |
| ResetEnterPassword                                       | Wprowadź nowe hasło: |
| ResetExample1                                            | `contoso\mmeyers` |
| ResetExample2                                            | `mmeyers\@contoso.com` |
| ResetExamples                                            | Przykłady: |
| ResetPageTitle                                           | Forefront Identity Management — Resetowanie hasła |
| ResetReenterPassword                                     | Wprowadź ponownie hasło: |
| ResetSuccessDescription                                  | Hasło zostało zresetowane |
| ResetSuccessTitle                                        | Prawnego |
| ResetUseNewPassword                                      | Teraz możesz zalogować się przy użyciu nowego hasła. |
| ResetUsernameTextFormat                                  | (Resetowanie hasła dla {0} ) <br/>**Uwaga** : {0} czy użytkownik jest zalogowany. |
| ResetWelcomeTitle                                        | Resetowanie hasła: |
| TroubleshootingEmailSubject                              | Szczegóły błędu przetwarzania żądania programu FIM |
| TroubleshootingLabelAttributes                           | Atrybuty: |
| TroubleshootingLabelCloseButton                          | Zamknij |
| TroubleshootingLabelCopyToClipboard                      | Kopiuj do schowka |
| TroubleshootingLabelCorrelationId                        | Identyfikator korelacji: |
| TroubleshootingLabelDetails                              | Szczegóły: |
| TroubleshootingLabelPostCopyClipboardMessage             | Informacje zostały skopiowane do Schowka. |
| TroubleshootingLabelRequestId                            | Identyfikator żądania: |
| TroubleshootingLabelSendEmail                            | Wyślij informacje pocztą E-mail |
| TroubleshootingLabelSource                               | Przyczyna: |
| TroubleshootingLabelViewRequestDetails                   | Wyświetl szczegóły żądania |
| TroubleshootingLinkText                                  | Informacje dotyczące rozwiązywania problemów |


## <a name="authentication-gate-strings"></a>Ciągi bram uwierzytelniania
W poniższej tabeli przedstawiono ciągi bramy uwierzytelniania, które można dostosować:

<!-- The default values are actual UI strings and should not copy edited. -->

| Nazwa ciągu bramy uwierzytelniania | Wartość domyślna |
|---|---|
| OTPEmailRegistraionEmailTextboxLabel                  | Adres e-mail: |
| OTPEmailRegistrationEmailRequiredErrorMessage         | Pole adresu e-mail nie może być puste. |
| OTPEmailRegistrationFooterReadOnly                    | Aby zaktualizować adres e-mail, postępuj zgodnie z procesem zdefiniowanym przez organizację lub skontaktuj się z pomocą techniczną. |
| OTPEmailRegistrationFooterReadWrite                   | Adres e-mail jest przechowywany przez organizację w programie Forefront Identity Manager. |
| OTPEmailRegistrationGateTitle                         | Weryfikacja adresu e-mail |
| OTPEmailRegistrationHeaderReadOnly                    | Jeśli kiedykolwiek zajdzie potrzeba zresetowania hasła, do wiadomości e-mail zostanie wysłany kod zabezpieczający weryfikacyjny. Jeśli poniższy adres e-mail jest niepoprawny, należy go zaktualizować w celu użycia funkcji samoobsługowego resetowania hasła. |
| OTPEmailRegistrationHeaderReadWrite                   | Wprowadź poniżej swój adres e-mail. Jeśli kiedykolwiek zajdzie potrzeba zresetowania hasła, kod weryfikacyjny zostanie wysłany do wiadomości e-mail. |
| OTPEmailResetGateTitle                                | Weryfikuj swoją tożsamość: email |
| OTPEmailResetHeader                                   | Wprowadź poniżej kod zabezpieczeń. Kod zabezpieczający został wysłany na adres e-mail zarejestrowany w tej organizacji. |
| OTPRegularExpressionErrorMessage                      | Określona wartość nie jest zgodna z oczekiwanym formatem. |
| OTPResetOneTimePasswordRequiredErrorMessage           | Pole kodu zabezpieczeń nie może być puste. |
| OTPResetVerificationLabel                             | Kod zabezpieczający: |
| OTPSmsRegistrationFooterReadOnly                      | Aby zaktualizować numer telefonu komórkowego, postępuj zgodnie z procesem zdefiniowanym przez organizację lub skontaktuj się z pomocą techniczną. |
| OTPSmsRegistrationFooterReadWrite                     | Numer telefonu komórkowego jest przechowywany przez organizację w programie Forefront Identity Manager. |
| OTPSmsRegistrationGateTitle                           | Weryfikacja telefonu komórkowego |
| OTPSmsRegistrationHeaderReadOnly                      | Jeśli kiedykolwiek zajdzie potrzeba zresetowania hasła, do telefonu komórkowego zostanie wysłany kod zabezpieczający weryfikacyjny. Jeśli podany poniżej numer telefonu komórkowego jest niepoprawny, należy go zaktualizować, aby można było korzystać z funkcji samoobsługowego resetowania hasła. |
| OTPSmsRegistrationHeaderReadWrite                     | Wprowadź poniżej numer telefonu komórkowego. Jeśli kiedykolwiek zajdzie potrzeba zresetowania hasła, do telefonu komórkowego zostanie wysłany kod weryfikacyjny. |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | Pole numeru telefonu komórkowego nie może być puste. |
| OTPSmsRegistrationSMSTextBoxLabel                     | Telefon komórkowy: |
| OTPSmsResetGateTitle                                  | Weryfikowanie Twojej tożsamości: telefon komórkowy |
| OTPSmsResetHeader                                     | Wprowadź poniżej kod zabezpieczeń. Kod zabezpieczający został wysłany do telefonu komórkowego zarejestrowanego w tej organizacji. |
| PasswordGateDescriptionText                           | Wprowadź poniżej bieżące hasło, a następnie kliknij przycisk Dalej. |
| PasswordGateErrorMessagePasswordRequired              | Wprowadź bieżące hasło. |
| PasswordGateGateTitle                                 | Twoje bieżące hasło |
| PasswordGatePasswordLabelText                         | Hasło: |
| PasswordGateUsernameTextFormat                        | `<i>` (zalogowany jako: `<b>{0}</b>` ) `</i>` |
| QAGateErrorNotEnoughQuestionsAnswered                 | Musisz odpowiedzieć na co najmniej {0} pytania. |
| QAGateIncorrectAnswer                                 | Twoje odpowiedzi są niepoprawne. |
| QAGatePrivacyNotice                                   | Wprowadzone odpowiedzi są przechowywane przez organizację w programie Forefront Identity Manager. |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Musisz odpowiedzieć na co najmniej {0} pytania, aby się zarejestrować. |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Co najmniej jedna odpowiedź nie jest zgodna z zasadami. |
| QAGateRegistrationThisAnswerValidationFailed          | Ta odpowiedź nie jest zgodna z zasadami. |
| QAGateRegistrationTitle                               | Rejestrowanie odpowiedzi |
| QAGateResetNumberOfQuestionsExplanation_Format        | Musisz odpowiedzieć {0} na następujące {1} pytania. |
| QAGateResetTitle                                      | Weryfikuj swoją tożsamość: Prześlij odpowiedzi  |


## <a name="custom-logo-banners"></a>Transparenty niestandardowe logo

Domyślny transparent na stronach portalu można dostosować do potrzeb Twojej organizacji.

Aby dostosować transparent logo:

1. Utwórz niestandardowe transparenty i Zapisz je jako pliki. png. Pliki powinny spełniać następujące zalecenia:

   - Rozmiar: 490 x 50 pikseli.
   - Głębia bitowa: 32 pikseli.

2. Skopiuj pliki do folderu dostosowania w każdym portalu, który chcesz dostosować.

3. Utwórz plik style. CSS w każdym folderze. Wskaż plik do folderu dostosowania dla portalu i nowego logo. Możesz zmienić nazwę logo zgodnie z potrzebami, na przykład `/Customizations/contosologo.png` . Arkusz CSS powinien wyglądać podobnie do następującego kodu:

   `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4. Jeśli używasz programu Internet Explorer 6,0, musisz podać alternatywne logo nieprzezroczyste i dodać następujący kod do stylu. CSS:

   `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

   Arkusz CSS powinien wyglądać podobnie do następującego kodu:
   
   `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`


## <a name="custom-images-for-smartphones"></a>Obrazy niestandardowe dla telefonów Smartphone

Możesz dostosować obraz logo dla telefonów Smartphone. 

Aby dostosować obraz dla telefonu smartphone:

1. Utwórz obrazy i Zapisz je jako pliki. png. Pliki powinny spełniać następujące zalecenia:

   - Rozmiar: 190 X 50 pikseli.
   - Głębia bitowa: 32 pikseli.

2. Skopiuj pliki do folderu dostosowania w każdym portalu, który chcesz dostosować.

3. Utwórz plik style. CSS w każdym folderze. Wskaż plik do folderu dostosowania dla portalu i nowego logo. Możesz zmienić nazwę logo zgodnie z potrzebami, na przykład `/Customizations/contosologo.png` . Arkusz CSS powinien wyglądać podobnie do następującego kodu:

   ```css
   @media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="custom-style-sheets"></a>Niestandardowe arkusze stylów

Możesz zmodyfikować układ i styl portali haseł przy użyciu niestandardowego arkusza stylów kaskadowych (CSS).

Aby użyć niestandardowego kodu CSS:

1. Utwórz niestandardowe pliki CSS i Zapisz je jako style. css.

2. Skopiuj pliki do folderu dostosowania w każdym portalu, który chcesz dostosować.

Poniższy kod jest podstawowym przykładem pliku style. CSS:

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
>Aby program MIM rozpoznał wszelkie dostosowane zmiany, należy uruchomić ponownie usługi IIS, uruchamiając polecenie `iisreset` .

Poniższy kod stanowi bardziej zaawansowany przykład pliku style. css. Ten plik zawiera informacje specyficzne dla telefonu smartphone lub tabletu iPad firmy Apple do wyświetlania portali na tych urządzeniach.

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


## <a name="common-customization-issues"></a>Typowe problemy dotyczące dostosowywania

W poniższej tabeli wymieniono typowe problemy, które mogą wystąpić podczas uaktualniania usługi FIM i portalu programu MIM.

| Problem | Rozwiązanie |
|---|---|
| Przeprowadzono dostosowanie ciągu, ale nie zostało ono odzwierciedlone w interfejsie użytkownika.                       | Dostosowanie ciągu w ciągach. zasoby wymagają ponownego uruchomienia usług IIS przez uruchomienie `iisreset` . |
| Po wprowadzeniu ciągów. resources nie widzę żadnych zmian w ciągu.          | Format String. Resources jest prawdopodobnie nieprawidłowo sformułowany i może być ignorowany przez portal. Sprawdź dziennik zdarzeń w obszarze **Dzienniki systemu Windows**  >  **Dzienniki aplikacji i usług** programu  >  **Forefront Identity Manager** . |
| Po raz pierwszy dodano styl style. CSS w portalu.     | Przy pierwszym wprowadzeniu pliku style. css należy uruchomić polecenie `iisreset` . |
| Nowe style są dodawane lub modyfikowane w pliku style. CSS, ale zmiany nie są widoczne w przeglądarce. | Wyczyść pamięć podręczną przeglądarki i Odśwież stronę. Sprawdź składnię CSS. |
| Zmieniono bezpośrednio zawartość folderu CSS `<path_to_sspr_portal>\css\*.css` lub logo transparentu `<path_to_sspr_portal>\images\fimlogo.png` . Te zmiany zostały utracone po uaktualnieniu. | Użytkownicy nie mogą bezpośrednio zmieniać tych plików. Użyj folderu customizations, aby podać logo transparentu i tylko dostosowanie stylów CSS w Style. css. Folder dostosowania jest celowo zastępowany przez główne uaktualnienia. Nie używaj narzędzi takich jak ILSpy i reflektory, aby zmienić ciągi w zestawach portalu. Użyj ciągów. resources, aby zastąpić domyślne ciągi. Zestawy zostały zastąpione po uaktualnieniu.  |
| Logo transparentu nie jest wyświetlane w portalach. Nadal widzimy logo programu FIM.                  | Nazwa obrazu/ścieżka w pliku style. CSS jest nieprawidłowa lub pamięć podręczna przeglądarki nie została wyczyszczona.  |
| Logo transparentu wygląda nieładnego w programie Internet Explorer 6.                                          | Udostępnij nieprzezroczysty obraz dla programu Internet Explorer 6 z odpowiednim stylem dla obrazu w pliku style. css. |
