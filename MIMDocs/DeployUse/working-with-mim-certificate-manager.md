---
title: "Praca z Menedżerem certyfikatów programu MIM | Microsoft Identity Manager"
description: "Dowiedz się, jak wdrożyć aplikację Menedżer certyfikatów, aby umożliwić użytkownikom zarządzanie swoimi prawami dostępu."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f9b01ac2cee2b96f64a9fda917f4f4146ca2eeda
ms.openlocfilehash: 3e0e6cea0b268836bb6347e81694deec93320ce3


---

# Praca z Menedżerem certyfikatów programu MIM
Po przygotowaniu programu MIM 2016 i Menedżera certyfikatów do pracy można wdrożyć aplikację Menedżer certyfikatów programu MIM ze Sklepu Windows, dzięki której użytkownicy będą mogli łatwo zarządzać fizycznymi kartami inteligentnymi, wirtualnymi kartami inteligentnymi i certyfikatami oprogramowania. Wdrażanie aplikacji Menedżer certyfikatów programu MIM obejmuje następujące czynności:

1.  Utworzenie szablonu certyfikatu.

2.  Utworzenie szablonu profilu.

3.  Przygotowanie aplikacji.

4.  Wdrożenie aplikacji za pomocą programu SCCM lub usługi Intune.

## Tworzenie szablonu certyfikatu
Tworzenie szablonu certyfikatu dla aplikacji Menedżer certyfikatów przebiega w normalny sposób. Jednak w tym przypadku musisz upewnić się, że szablon certyfikatu jest w wersji 3 lub nowszej.

1.  Zaloguj się do serwera z uruchomionymi usługami AD CS (serwera certyfikatów).

2.  Otwórz program MMC.

3.  Kliknij pozycję **Plik &gt; Dodaj/Usuń przystawkę**.

4.  Na liście dostępnych przystawek kliknij pozycję **Szablony certyfikatów**, a następnie kliknij przycisk **Dodaj**.

5.  Pozycja **Szablony certyfikatów** znajduje się teraz poniżej pozycji **Główny katalog konsoli** w programie MMC. Kliknij dwukrotnie tę pozycję, aby wyświetlić wszystkie dostępne szablony certyfikatów.

6.  Kliknij prawym przyciskiem myszy szablon **Logowanie karty inteligentnej**, a następnie kliknij polecenie **Duplikuj szablon**.

7.  Na karcie Zgodność wybierz pozycję Windows Server 2008 w obszarze Urząd certyfikacji i wybierz pozycję Windows 8.1/Windows Server 2012 R2 w obszarze Odbiorca certyfikatu.
    Ten krok jest niezwykle ważny, ponieważ pozwala sprawdzić, czy szablon certyfikatu jest w wersji 3 lub nowszej. Tylko wersja 3 współdziała z aplikacją Menedżer certyfikatów. Wersja jest ustawiana podczas pierwszego tworzenia i zapisywania szablonu certyfikatu, dlatego jeśli nie utworzono szablonu certyfikatu w ten sposób, nie ma możliwości zmiany wersji i należy utworzyć nowy szablon przed kontynuowaniem.

8.  Na karcie **Ogólne** w polu **Nazwa wyświetlana** wpisz nazwę, która ma być wyświetlana w interfejsie użytkownika aplikacji, taką jak **Logowanie wirtualnej karty inteligentnej**.

9. Na karcie **Obsługiwanie żądań** ustaw dla opcji **Przeznaczenie** wartość **Podpis i szyfrowanie**, a następnie w obszarze **Zrób tak…** zaznacz pole wyboru **Monituj użytkownika podczas rejestrowania**.

10. Na karcie **Kryptografia** w obszarze **Kategoria dostawcy** wybierz pozycje **Dostawca magazynu kluczy i Żądania mogą korzystać z każdego dostawcy dostępnego na komputerze podmiotu**.

    > [!NOTE]
    > Jeśli szablon jest w wersji 3, dostępna będzie tylko opcja Dostawca magazynu kluczy. Jeśli jej nie widzisz, prawdopodobnie oznacza to, że nie utworzono poprawnie szablonu certyfikatu w odpowiedniej wersji. Wróć do kroku 5 powyżej.

11. Na karcie **Zabezpieczenia** dodaj grupę zabezpieczeń, której chcesz zezwolić na dostęp z uprawnieniem **Rejestracja**. Jeśli na przykład chcesz zezwolić na dostęp wszystkim użytkownikom, wybierz grupę **Użytkownicy uwierzytelnieni**, a następnie wybierz dla nich uprawnienia **Rejestracja**.

12. Kliknij przycisk **OK**, aby zakończyć wprowadzanie zmian i utworzyć nowy szablon. Twój nowy szablon powinien zostać wyświetlony na liście Szablony certyfikatów.

13. Wybierz menu **Plik**, a następnie kliknij polecenie **Dodaj/Usuń przystawkę**, aby dodać przystawkę Urząd certyfikacji do konsoli programu MMC. Po wyświetleniu monitu o określenie komputera, którym chcesz zarządzać, wybierz pozycję **Komputer lokalny**.

14. W lewym okienku programu MMC rozwiń węzeł **Urząd certyfikacji (lokalny)**, a następnie rozwiń węzeł urzędu certyfikacji (CA) na liście Urząd certyfikacji.

15. Kliknij prawym przyciskiem myszy pozycję **Szablony certyfikatów**, a następnie kliknij pozycje **Nowy &gt; Szablon certyfikatu do wystawienia**.

16. Wybierz nowo utworzony szablon z listy, a następnie kliknij przycisk **OK**.

## Tworzenie szablonu profilu
Podczas tworzenia szablonu profilu ustaw go w celu utworzenia/zniszczenia wirtualnej karty inteligentnej i usunięcia kolekcji danych. Aplikacja Menedżer certyfikatów nie obsługuje zebranych danych, dlatego należy wyłączyć tę funkcję w opisany poniżej sposób.

1.  Zaloguj się do portalu zarządzania certyfikatami jako użytkownik z uprawnieniami administracyjnymi.

2.  Przejdź do pozycji Administracja &gt; Zarządzaj szablonami profilów, upewnij się, że zaznaczono pole wyboru Szablon profilu logowania za pomocą karty inteligentnej Menedżera certyfikatów programu MIM, a następnie kliknij przycisk Kopiuj wybrany szablon profilu.

3.  Wpisz nazwę szablonu profilu, a następnie kliknij przycisk **OK**.

4.  Na następnym ekranie kliknij pozycję **Dodaj nowy szablon certyfikatu** i koniecznie zaznacz pole wyboru obok nazwy urzędu certyfikacji.

5.  Zaznacz pole obok nazwy szablonu profilu **Logowanie** i kliknij pozycję **Dodaj**.

6.  Usuń szablon logowania za pomocą karty inteligentnej, zaznaczając pole wyboru obok tego szablonu, a następnie klikając pozycję **Usuń wybrane szablony certyfikatów** i klikając przycisk **OK**.

7.  Przewiń w dół i kliknij pozycję **Zmień ustawienia**.

8.  Zaznacz pola wyboru obok pozycji **Utwórz/zniszcz wirtualną kartę inteligentną** i **Zróżnicuj klucz administratora**.

9. W obszarze **Zasady numeru PIN użytkownika** wybierz pozycję **Podane przez użytkownika**.

10. W lewym okienku kliknij pozycje **Odnów zasady &gt; Zmień ustawienia ogólne**. Wybierz pozycję **Użyj karty ponownie przy odnawianiu** i kliknij przycisk **OK**.

11. Musisz wyłączyć elementy kolekcji danych dla poszczególnych zasad, klikając zasady w okienku po lewej stronie, a następnie zaznaczając pole wyboru obok pozycji **Element danych przykładowych** i klikając pozycję **Usuń elementy kolekcji danych**. Następnie kliknij przycisk **OK**.

## Przygotowywanie aplikacji Menedżer certyfikatów do wdrożenia

1.  W wierszu polecenia uruchom poniższe polecenie, aby rozpakować aplikację, a następnie wyodrębnij zawartość do nowego podfolderu o nazwie appx oraz utwórz kopię, aby nie modyfikować oryginalnego pliku.

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  W folderze appx zmień nazwę pliku CustomDataExample.xml na Custom.data.

3.  Otwórz plik Custom.data i zmodyfikuj odpowiednio parametry.

    |||
    |-|-|
    |MIMCM URL|Nazwa FQDN portalu, w którym skonfigurowano Menedżera certyfikatów. Przykład: https://adres_serwera_menedzera_certyfikatow_programu_MIM/zarzadzanie_certyfikatami|
    |ADFS URL|Jeśli będziesz używać usług AD FS, wstaw odpowiedni adres URL. Przykład: https://nazwa_serwera_uslug_adfs/adfs|
    |PrivacyUrl|Możesz podać adres URL strony sieci Web z informacjami o tym, co się dzieje z danymi użytkowników zebranymi na potrzeby rejestracji certyfikatu.|
    |SupportMail|Możesz podać adres e-mail pomocy technicznej.|
    |LobComplianceEnable|Możesz ustawić wartość true lub false. Domyślnie jest ustawiona wartość true.|
    |MinimumPinLength|Domyślnie jest ustawiona wartość 6.|
    |NonAdmin|Możesz ustawić wartość true lub false. Domyślnie jest ustawiona wartość false. To ustawienie należy modyfikować tylko wtedy, gdy użytkownicy niebędący administratorami na swoich komputerach mają mieć możliwość rejestrowania i odnawiania certyfikatów.|

4.  Zapisz plik i zamknij edytor.

5.  Podpisanie pakietu powoduje utworzenie pliku podpisywania, więc musisz usunąć oryginalny plik podpisywania o nazwie AppxSignature.p7x.

6.  Plik AppxManifest.xml określa nazwę podmiotu certyfikatu podpisywania. Otwórz ten plik, aby go edytować.

7.  Aby móc przejść do tej sekcji, musisz uzyskać certyfikat podpisywania. Zobacz krok 1 w poniższej sekcji Włączanie funkcji odnawiania kart inteligentnych dla użytkowników innych niż administratorzy w Menedżerze certyfikatów programu MIM 2016.

8.  W elemencie &lt;Identity&gt; zmodyfikuj wartość atrybutu Publisher, aby była taka sama jak wartość podmiotu certyfikatu podpisywania, np. „CN=PODMIOT”.

9. Zapisz plik i zamknij edytor.

10. W wierszu polecenia uruchom poniższe polecenie, aby ponownie spakować i podpisać plik appx.

    ```
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    Nazwa pakietu aplikacji jest taka sama jak nazwa użyta podczas tworzenia kopii.

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Powoduje to udostępnienie nowego plik appx. Plik pfx stanowi lokalizację certyfikatu podpisywania i udostępnia hasło dla pliku pfx.

11. Aby korzystać z uwierzytelniania usług AD FS:

    -   Otwórz aplikację wirtualnej karty inteligentnej. Ułatwi to znalezienie wartości potrzebnych podczas następnego kroku.

    -   Aby dodać aplikację jako klienta na serwerze usług AD FS i skonfigurować Menedżera certyfikatów na serwerze, na serwerze usług AD FS otwórz program Windows PowerShell i uruchom polecenie `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`

        Poniżej przedstawiono skrypt ConfigureMimCMClientAndRelyingParty.ps1:

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   Zaktualizuj wartości parametrów redirectUri i serverFQDN.

    -   Aby znaleźć parametr redirectUri , otwórz panel ustawień aplikacji wirtualnej karty inteligentnej, kliknij pozycję **Ustawienia**. Identyfikator URI przekierowania powinien być wyświetlany poniżej paska adresu serwera usług AD FS. Identyfikator URI jest wyświetlany tylko w sytuacji, gdy skonfigurowano adres serwera usług AD FS.

    -   Parametr serverFQDN określa tylko pełną nazwę komputera serwera Menedżera certyfikatów programu MIM.

    -   Aby uzyskać pomoc dotyczącą skryptu **ConfigureMIimCMClientAndRelyingParty.ps1**, uruchom polecenie `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`

## Wdrażanie aplikacji
Po skonfigurowaniu aplikacji Menedżer certyfikatów pobierz plik MIMDMModernApp_&lt;wersja&gt;_AnyCPU_Test.zip z Centrum pobierania i wyodrębnij całą jego zawartość. Instalatorem jest plik appx. Możesz wdrożyć aplikację tak jak zwykle wdrażasz aplikacje ze Sklepu Windows przy użyciu programu [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx) lub usługi [Intune](https://technet.microsoft.com/library/dn613839.aspx) w celu lokalnego pobrania aplikacji, aby użytkownicy musieli uzyskiwać do niej dostęp za pośrednictwem Portalu firmy. W przeciwnym razie zawartość zostanie wypchnięta bezpośrednio na maszyny użytkowników.



<!--HONumber=Jun16_HO4-->


