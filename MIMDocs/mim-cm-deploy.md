---
title: Wdrożenie Menedżera certyfikatów programu Microsoft Identity Manager | Dokumentacja firmy Microsoft
description: Zainstaluj Menedżera certyfikatów programu Microsoft Identity Manager 2016
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/19/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 25a511dc590b02019c65a688c9b2c8dc821fff50
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290088"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Wdrożenie Menedżera certyfikatów programu Microsoft Identity Manager 2016 (MIM CM)

Instalacja Menedżera certyfikatów programu Microsoft Identity Manager 2016 (MIM CM) obejmuje kilka kroków. Aby uprościć proces możemy są podziału rzeczy. Brak wstępne kroki, które należy podjąć przed żadnych czynności rzeczywisty zarządzania Certyfikatami programu MIM. Bez wstępnego pracy prawdopodobnie instalacja nie powiedzie się.

Na poniższym diagramie przedstawiono przykład typu środowiska, które mogą być używane. Systemy z numerami znajdują się na liście poniżej diagramu i są wymagane do pomyślnego ukończenia kroki omówione w tym artykule. Na koniec systemów Windows Server Datacenter 2016 służą:

![Diagram środowiska](media/mim-cm-deploy/image001.png)

1. CORPDC — kontroler domeny
2. CORPCM — serwera MIM CM
3. CORPCA — urząd certyfikacji
4. CORPCMR — sieci Web interfejsu API Rest zarządzania certyfikatami w usłudze MIM — CM Portal dla interfejsu API Rest — używane dla później
5. CORPSQL1 — DODATEK SP1 DLA PROGRAMU SQL 2016
6. CORPWK1 — przyłączone do domeny systemu Windows 10

## <a name="deployment-overview"></a>Przegląd wdrożenia

- Instalacja podstawowego systemu operacyjnego

    Laboratorium składa się z serwerów z systemem windows 2016 centrum danych.

    >[!NOTE]
    >Dla więcej szczegółów na platformach obsługiwanych przez program MIM 2016 Spójrz na artykuł [platformy obsługiwane przez program MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)

1. Kroki przed wdrożeniem

    - [Rozszerzanie schematu](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Tworzenie konta usług

    - [Tworzenie szablonów certyfikatów](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Konfigurowanie protokołu Kerberos

    - Kroki związane z bazy danych

        - Wymagania dotyczące konfiguracji programu SQL

        - Uprawnienia dotyczące bazy danych

2. wdrażania

## <a name="pre-deployment-steps"></a>Kroki przed wdrożeniem

Kreator konfiguracji zarządzania Certyfikatami programu MIM wymaga informacji dostarczanych wzdłuż sposobu, w celu pomyślnego wykonania.

![Diagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Rozszerzanie schematu

Procesu rozszerzenia schematu jest proste, ale musi być skontaktowali się ostrożnie z powodu natury nieodwracalne.

>[!NOTE]
>Ten krok wymaga, czy używane konto ma uprawnienia administratora schematu.

1. Przejdź do lokalizacji nośnika MIM i przejdź do \\zarządzania certyfikatami\\x64 folderu.

2. Skopiuj folder schematu na kontrolerze domeny CORPDC, a następnie przejdź do niego.

    ![Diagram](media/mim-cm-deploy/image005.png)

3. Uruchom skrypt resourceForestModifySchema.vbs pojedynczego lasu scenariusza. W scenariuszu lasu zasobów uruchomić skrypty:
   - DomenaA — użytkownicy znajdujący się (userForestModifySchema.vbs)
   - ResourceForestB — lokalizacja instalacji CM (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Zmiany schematu są operacji jednokierunkowej i wymagają lesie odzyskiwania, aby wycofać dlatego upewnij się, masz niezbędne kopie zapasowe. Aby uzyskać szczegółowe informacje o zmiany wprowadzone w schemacie po wykonaniu tej operacji zapoznaj się z artykułem [zmiany schematu programu Forefront Identity Manager 2010 certyfikatu zarządzania](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![Diagram](media/mim-cm-deploy/image007.png)

4. Uruchom skrypt i powinien zostać wyświetlony sukcesu raz wiadomości, że skrypt zostało ukończone.

    ![Komunikat z potwierdzeniem](media/mim-cm-deploy/image009.png)

Teraz rozszerzania schematu w usłudze AD do obsługi zarządzania Certyfikatami programu MIM.

### <a name="creating-service-accounts-and-groups"></a>Tworzenie konta usługi i grupy

W poniższej tabeli przedstawiono kont i uprawnień wymaganych przez zarządzania Certyfikatami programu MIM. Można zezwolić MIM CM automatycznie Utwórz następujące konta lub można je utworzyć przed instalacją. Można zmienić nazwy własnego konta. Utwórz konta użytkownika, należy rozważyć nazw kont użytkowników w takich optymalizacji jest łatwy do dopasowania nazwy konta użytkownika dla jej funkcji.

Użytkownicy:

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Rola**                   | **Nazwy logowania użytkownika** |
|----------------------------|---------------------|
| Agent zarządzania Certyfikatami programu MIM               | MIMCMAgent          |
| Agent odzyskiwania kluczy zarządzania Certyfikatami programu MIM  | MIMCMKRAgent        |
| MIM CM autoryzacji agenta | MIMCMAuthAgent      |
| Agent Menedżera urzędu certyfikacji zarządzania Certyfikatami programu MIM    | MIMCMManagerAgent   |
| Agent puli zarządzania certyfikatami w usłudze sieci Web programu MIM      | MIMCMWebAgent       |
| Agent rejestrowania MIM CM    | MIMCMEnrollAgent    |
| Usługa MIM CM aktualizacji      | MIMCMService        |
| Konto instalacji MIM        | MIMINSTALL          |
| Pomoc techniczna agenta            | CMHelpdesk1-2       |
| Menedżer CM                 | CMManager1-2        |
| Subskrybent użytkownika            | CMUser1-2           |

Grupy:

| **Rola**               | **Grupa**         |
|------------------------|-------------------|
| Elementy członkowskie techniczną CM    | Pomoc techniczna określa    |
| Elementy członkowskie Menedżera CM     | Określa menedżerów    |
| Członkowie-subskrybenci CM | Określa subskrybentów |

Programu PowerShell: Konta agenta:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Aktualizacja **CORPCM** zasady lokalne serwera dotyczące kont agentów 

| **Nazwy logowania użytkownika** | **Opis i uprawnień**   |
|------|---------------------|
| MIMCMAgent          | Udostępnia następujące usługi: </br>-Pobiera zaszyfrowanych kluczy prywatnych z urzędu certyfikacji. </br>— Zapewnia ochronę informacji o numerach PIN karty inteligentnej w bazie danych programu FIM CM. </br>— Zapewnia ochronę komunikacji między CM usługi FIM i urzędu certyfikacji. </br></br> To konto użytkownika wymagane są następujące ustawienia kontroli dostępu:</br>-   **Zezwalaj na logowanie lokalne** prawa użytkownika.</br>-   **Wystawianie i zarządzanie certyfikatami** prawa użytkownika. </br>-Odczytu i zapisu uprawnienia do folderu tymczasowego systemu w następującej lokalizacji: % WINDIR %\\Temp.</br>-Cyfrowy podpis i szyfrowanie certyfikat wystawiony i zainstalowany w magazynie użytkownika.
|MIMCMKRAgent        | Odzyskuje archiwizacji kluczy prywatnych z urzędu certyfikacji. To konto użytkownika wymagane są następujące ustawienia kontroli dostępu:</br> -   **Zezwalaj na logowanie lokalne** prawa użytkownika.</br>-Członkostwo w lokalnej **Administratorzy** grupy. </br>— Rejestrowanie uprawnień na **KeyRecoveryAgent** szablonu certyfikatu. </br>-Certyfikat agenta odzyskiwania kluczy wydane i zainstalowany w magazynie użytkownika. Certyfikat musi być dodany do listy agentów odzyskiwania kluczy urzędu certyfikacji. </br>Zmień ```%WINDIR%\\Temp.```okres ważności do 2 lata, a następnie wyczyść Publikuj certyfikat w usłudze Active Directory pole wyboru. ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Na obsługiwanie żądań karcie, upewnij się, że Zezwalaj na eksportowanie klucza prywatnego pole wyboru jest zaznaczone, a następnie kliknij przycisk karta Kryptografia. To konto użytkownika wymagane są następujące ustawienia kontroli dostępu: </br>W wybór kryptografii okno dialogowe, wyłącz Microsoft Enhanced Cryptographic Provider 1.0, Włącz Microsoft Enhanced RSA and AES Cryptographic Provider, a następnie kliknij przycisk OK. </br> Na **nazwa podmiotu** kartę, wyczyść Dołącz nazwę e-mail do nazwy podmiotu i nazwa E-mail pola wyboru.             |
| MIMCMManagerAgent   | Na rozszerzenia karcie rozszerzenia zawarte w tym szablonie listy, upewnij się, że zasady aplikacji jest zaznaczone, a następnie kliknij przycisk edycji . </br> W Edytowanie rozszerzenia zasad aplikacji okno dialogowe, wybierz systemu szyfrowania plików i zabezpieczanie poczty E-mail zasady aplikacji.        |
| MIMCMWebAgent       | Kliknij przycisk Usuń, a następnie kliknij przycisk OK. Na zabezpieczeń kartę należy wykonać następujące czynności: </br> To konto użytkownika wymagane są następujące ustawienia kontroli dostępu:</br> Usuń **administratora**. </br>-Członkostwo w lokalnej **Administratorzy** grupy.</br>Na **nazwa podmiotu** kartę, wyczyść Dołącz nazwę e-mail do nazwy podmiotu i nazwa E-mail pola wyboru. </br>Usuń **Administratorzy domeny**. </br>Usuń **użytkownicy domeny**.</br>Przypisz tylko **odczytu** i zapisu uprawnień do Administratorzy przedsiębiorstwa. </br>Dodaj **MIMCMAgent. </br>Przypisz odczytu i Zarejestruj uprawnień do MIMCMAgent.|
| MIMCMEnrollAgent    | W właściwości nowego szablonu okno dialogowe, kliknij przycisk OK. To konto użytkownika wymagane są następujące ustawienia kontroli dostępu:</br>Pozostaw konsolę Szablony certyfikatów otworzyć.</br>-   **Zezwalaj na logowanie lokalne** prawa użytkownika. </br>Tworzenie szablonu certyfikatu agenta rejestracji MIM CM                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>W konsolę Szablony certyfikatóww szczegóły okienku, wybierz i kliknij prawym przyciskiem myszy agenta rejestracji, a następnie kliknij przycisk Duplikuj szablon.

W właściwości nowego szablonu na okna dialogowego ogólne karcie Nazwa wyświetlana szablonu wpisz agenta rejestracji MIM CM.

Upewnij się, że okres ważności jest 2 lata.

- MIMCMAgent: To konto wymaga certyfikatu użytkownika

- MIMCMEnrollAgent: To konto wymaga certyfikatu agenta rejestracji

- MIMCMKRAgent: Wymaga to konto **agenta odzyskiwania kluczy** certyfikatu

Istnieją szablony, które jeszcze nie istnieje w usłudze AD, ale należy utworzyć własne wersji do pracy z zarządzania Certyfikatami programu MIM. Jak należy dokonać modyfikacji z oryginalne szablony linii bazowej.

Wszystkie trzy powyższe konta będą mieć podwyższonym poziomem uprawnień w organizacji i powinien zostać obsłużony dokładnie.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Tworzenie szablonu certyfikatu podpisywania programu MIM CM

1. Z **narzędzia administracyjne**, otwórz **urzędu certyfikacji**.

2. W **urzędu certyfikacji** konsoli, w drzewie konsoli rozwiń węzeł **Contoso CorpCA**, a następnie kliknij przycisk **szablonów certyfikatów**.

3. Kliknij prawym przyciskiem myszy **szablonów certyfikatów**, a następnie kliknij przycisk **Zarządzaj**.

4. W **konsolę Szablony certyfikatów**w **szczegóły** okienku, wybierz i kliknij prawym przyciskiem myszy **użytkownika**, a następnie kliknij przycisk **Duplikuj szablon** .

5. W **Duplikuj szablon** okno dialogowe, wybierz opcję **systemu Windows Server 2003 Enterprise**, a następnie kliknij przycisk **OK**.

    ![Pokaż wynikowe zmiany](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >Menedżer certyfikatów programu MIM nie działa z certyfikaty oparte na wersji 3 szablonów certyfikatów. Należy utworzyć szablonu certyfikatu w systemie Windows Server® 2003 Enterprise (wersja 2). Zobacz [szczegóły V3](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) Aby uzyskać więcej informacji.

6. W **właściwości nowego szablonu** na okna dialogowego **ogólne** karcie **Nazwa wyświetlana szablonu** wpisz **MIM CM podpisywania**. Zmień **okres ważności** do **2 lata**, a następnie wyczyść **Publikuj certyfikat w usłudze Active Directory** pole wyboru.

7. Na **obsługiwanie żądań** karcie, upewnij się, że **Zezwalaj na eksportowanie klucza prywatnego** pole wyboru jest zaznaczone, a następnie kliknij przycisk **karta Kryptografia**.

8. W **wybór kryptografii** okno dialogowe, wyłącz **Microsoft Enhanced Cryptographic Provider 1.0**, Włącz **Microsoft Enhanced RSA and AES Cryptographic Provider**, a następnie kliknij przycisk **OK**.

9. Na **nazwa podmiotu** kartę, wyczyść **Dołącz nazwę e-mail do nazwy podmiotu** i **nazwa E-mail** pola wyboru.

10. Na **rozszerzenia** karcie **rozszerzenia zawarte w tym szablonie** listy, upewnij się, że **zasady aplikacji** jest zaznaczone, a następnie kliknij przycisk **edycji** .

11. W **Edytowanie rozszerzenia zasad aplikacji** okno dialogowe, wybierz **systemu szyfrowania plików** i **zabezpieczanie poczty E-mail** zasady aplikacji. Kliknij przycisk **Usuń**, a następnie kliknij przycisk **OK**.

12. Na **zabezpieczeń** kartę należy wykonać następujące czynności:

    - Usuń **administratora**.

    - Usuń **Administratorzy domeny**.

    - Usuń **użytkownicy domeny**.

    - Przypisz tylko **odczytu** i **zapisu** uprawnień do **Administratorzy przedsiębiorstwa**.

    - Dodaj **MIMCMAgent.**

    - Przypisz **odczytu** i **Zarejestruj** uprawnień do **MIMCMAgent**.

13. W **właściwości nowego szablonu** okno dialogowe, kliknij przycisk **OK**.

14. Pozostaw **konsolę Szablony certyfikatów** otworzyć.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Tworzenie szablonu certyfikatu agenta rejestracji MIM CM

1. W **konsolę Szablony certyfikatów**w **szczegóły** okienku, wybierz i kliknij prawym przyciskiem myszy **agenta rejestracji**, a następnie kliknij przycisk **Duplikuj szablon**.

2. W **Duplikuj szablon** okno dialogowe, wybierz opcję **systemu Windows Server 2003 Enterprise**, a następnie kliknij przycisk **OK**.

3. W **właściwości nowego szablonu** na okna dialogowego **ogólne** karcie **Nazwa wyświetlana szablonu** wpisz **agenta rejestracji MIM CM**. Upewnij się, że **okres ważności** jest **2 lata**.

4. Na **obsługiwanie żądań** pozycję Włącz **Zezwalaj na eksportowanie klucza prywatnego**, a następnie kliknij przycisk **dostawców usług kryptograficznych lub Tab kryptografii.**

5. W **Wybieranie dostawcy CSP** okno dialogowe, wyłącz **Microsoft Base Cryptographic Provider 1.0**, wyłącz **Microsoft Enhanced Cryptographic Provider 1.0**, Włącz  **Microsoft Enhanced RSA and AES Cryptographic Provider**, a następnie kliknij przycisk **OK**.

6. Na **zabezpieczeń** kartę, wykonaj następujące czynności:

    - Usuń **administratora**.

    - Usuń **Administratorzy domeny**.

    - Przypisz tylko **odczytu** i **zapisu** uprawnień do **Administratorzy przedsiębiorstwa**.

    - Dodaj **MIMCMEnrollAgent**.

    - Przypisz **odczytu** i **Zarejestruj** uprawnień do **MIMCMEnrollAgent**.

7. W **właściwości nowego szablonu** okno dialogowe, kliknij przycisk **OK**.

8. Pozostaw **konsolę Szablony certyfikatów** otworzyć.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Tworzenie szablonu certyfikatu agenta odzyskiwania kluczy zarządzania Certyfikatami programu MIM

1. W **szablonów certyfikatów** konsoli w **szczegóły** okienku, wybierz i kliknij prawym przyciskiem myszy **agenta odzyskiwania kluczy**, a następnie kliknij przycisk **Duplikuj szablon**.

2. W **Duplikuj szablon** okno dialogowe, wybierz opcję **systemu Windows Server 2003 Enterprise**, a następnie kliknij przycisk **OK**.

3. W **właściwości nowego szablonu** na okna dialogowego **ogólne** karcie **Nazwa wyświetlana szablonu** wpisz **agenta odzyskiwania kluczy MIM CM**. Upewnij się, że **okres ważności** jest **2 lata** na **karta kryptografii.**

4. W **wybór dostawców** okno dialogowe, wyłącz **Microsoft Enhanced Cryptographic Provider 1.0**, Włącz **Microsoft Enhanced RSA and AES Cryptographic Provider**, a następnie kliknij przycisk **OK**.

5. Na **wymagania wystawiania** karcie, upewnij się, że **Zgoda menedżera certyfikatów urzędu certyfikacji** jest **wyłączone**.

6. Na **zabezpieczeń** kartę, wykonaj następujące czynności:

    - Usuń **administratora**.

    - Usuń **Administratorzy domeny**.

    - Przypisz tylko **odczytu** i **zapisu** uprawnień do **Administratorzy przedsiębiorstwa**.

    - Dodaj **MIMCMKRAgent**.

    - Przypisz **odczytu** i **Zarejestruj** uprawnień do **KRAgent**.

7. W **właściwości nowego szablonu** okno dialogowe, kliknij przycisk **OK**.

8. Zamknij okno **Konsola szablonów certyfikatów**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publikowanie szablonów wymaganego certyfikatu w urzędzie certyfikacji

1. Przywróć **urzędu certyfikacji** konsoli.

2. W **urzędu certyfikacji** konsoli, w drzewie konsoli kliknij prawym przyciskiem myszy **szablonów certyfikatów**, wskaż polecenie **nowy**, a następnie kliknij przycisk **certyfikatu Szablon do wystawienia**.

3. W **Włączanie szablonu certyfikatu** okno dialogowe, wybierz opcję **MIM CM rejestracji agenta**, **agenta odzyskiwania kluczy zarządzania certyfikatami w usłudze MIM**, i **MIM CM podpisywania**. Kliknij przycisk **OK**.

4. W drzewie konsoli kliknij **szablonów certyfikatów**.

5. Sprawdź, czy trzy nowe szablony są wyświetlane w **szczegóły** okienka, a następnie Zamknij **urzędu certyfikacji**.

    ![Podpisywanie zarządzania Certyfikatami programu MIM](media/mim-cm-deploy/image016.png)

6. Zamknij wszystkie otwarte okna i Wyloguj.

### <a name="iis-configuration"></a>Konfiguracja usług IIS

Do obsługi witryny sieci Web do zarządzania Certyfikatami, instalowanie i konfigurowanie usług IIS.

#### <a name="install-and-configure-iis"></a>Instalowanie i konfigurowanie usług IIS

1. Zaloguj się do **CORLog w** jako **MIMINSTALL** konta

    >[!IMPORTANT]
    >Konto instalacji MIM powinien mieć uprawnienia administratora lokalnego

2. Otwórz program PowerShell i uruchom następujące polecenie

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Witryny o nazwie domyślnej witryny sieci Web jest zainstalowana domyślnie w usługach IIS 7. Jeśli tej lokacji została zmieniona lub usunął lokację o nazwie domyślnej witryny sieci Web musi być dostępny przed zainstalowaniem zarządzania Certyfikatami programu MIM.

#### <a name="configuring-kerberos"></a>Konfigurowanie protokołu Kerberos

Konto MIMCMWebAgent będzie uruchomiony w portalu zarządzania Certyfikatami programu MIM. Domyślnie w usługach IIS oraz jądra tryb uwierzytelniania jest używany w usługach IIS domyślnie. Zostanie Wyłącz uwierzytelnianie trybu jądra protokołu Kerberos i skonfiguruj nazwy SPN konta MIMCMWebAgent. Na — Konfiguracja usługi Active Directory zaakceptuj nazwę domyślną podany dla punktu połączenia usługi, a następnie kliknij pozycję dalej.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Na **metodę uwierzytelniania** Potwierdź uwierzytelnianie zintegrowane systemu windows jest zaznaczone, a następnie kliknij przycisk dalej.**

![Diagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Na agentów — FIM CM wyczyść Użyj ustawień domyślnych programu FIM CM pole wyboru, a następnie kliknij przycisk kont niestandardowe.

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>W agentów — FIM CM okno dialogowe z wieloma kartami, na poszczególnych kartach, wpisz następujące informacje:

Nazwa użytkownika: aktualizacji Hasło: przekazaćword1

1. Potwierdź hasło: **przekazać**word1

2. Użyj istniejącego użytkownika: **włączone**

3. Te konta utworzony wcześniej.

4. Upewnij się, że procedur opisanych w kroku 8 są powtarzane dla wszystkich kart konta sześciu agenta.

5. Kont zarządzania Certyfikatami programu MIM**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Po zakończeniu wszystkich informacji o koncie agenta, kliknij przycisk **OK**.

1. Na agentów — MIM CM kliknij przycisk dalej.

2. Na Konfigurowanie certyfikatów serwera pozycję Włącz następujące szablony certyfikatu:

3. Szablon certyfikatu do użycia dla agenta odzyskiwania kluczy certyfikat agenta odzyskiwania: \\MIMCMKeyRecoveryAgent.

    >[!NOTE]
    >Szablon certyfikatu do użycia dla agenta programu FIM CM certyfikatu: MIMCMSigning.

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Szablon certyfikatu do użycia dla certyfikatu agenta rejestracji: FIMCMEnrollmentAgent.](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Na certyfikaty serwera konfiguracji kliknij przycisk dalej.

1. Na **ustawienia serwera poczty E-mail, wydrukować dokument** strony w **Określ nazwę serwera SMTP, które chcesz użyć do rejestracji powiadomień e-mail** a następnie kliknij przycisk dalej.

2. Na \\wszystko gotowe do skonfigurowania kliknij przycisk Konfiguruj.

3. W Kreator konfiguracji — Microsoft Forefront Identity Manager 2010 R2 okno dialogowe ostrzeżenia kliknij OK potwierdzić katalog wirtualny usług IIS nie jest włączony protokół SSL.

4. **nośnik/image17.png

5. Nie klikaj przycisku do czasu ukończenia działania Kreatora konfiguracji.

6. Rejestrowanie dla kreatora można znaleźć tutaj: **% programfiles %** Microsoft Forefront Identity Management2010zarządzania certyfikatamiconfig.log**

7. Zamknij wszystkie otwarte okna.

8. Dodaj ** do strefy Lokalny intranet w przeglądarce.

9. Odwiedź witrynę z serwera CORPCM

10. Sprawdzić, czy Usługa izolacji klucza CNG

11. Z **narzędzia administracyjne**, otwórz **usług**.

![W szczegóły okienku kliknij dwukrotnie izolacji klucza CNG.](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Na ogólne Zmień uruchamiana do automatyczne.

Na **ogólne** karcie, uruchom usługę, jeśli nie jest w stanie uruchomionym. Na ogólne , kliknij pozycję OK.

![Komunikat o błędzie](media/mim-cm-deploy/image028.png)

1. Instalowanie i Konfigurowanie modułów urzędu certyfikacji: W tym kroku zainstalujemy i skonfigurowania modułów programu FIM CM urzędu certyfikacji w urzędzie certyfikacji.**

2. Konfigurowanie programu FIM CM tylko sprawdzić uprawnienia użytkownika dla operacji zarządzania

3. W **C:** Program Files**Microsoft Forefront Identity Manager**2010**zarządzania certyfikatami**web**okna, utworzyć kopię** plik Web.config nazewnictwa kopii web.1.config.

4. W **Web** okna, kliknij prawym przyciskiem myszy **Web.config**, a następnie kliknij przycisk **Otwórz**.

5. Plik Web.config jest otwarty w Notatniku

6. Po otwarciu pliku, naciśnij klawisze CTRL + F.

7. W **Znajdź i Zamień** okna dialogowego, **Znajdź** wpisz **UseUser**, a następnie kliknij przycisk Znajdź następny trzy razy.

8. Zamknij **Znajdź i Zamień** okno dialogowe.

9. Należy w wierszu **** dodać key="Clm.RequestSecurity.Flags" wartość = "UseUser UseGroups" /.

   - Zmień wiersz odczytać **** dodać key="Clm.RequestSecurity.Flags" wartość = "UseUser" /.**

   - Zamknij plik, zapisywanie wszystkich zmian.**

   - Utwórz konto komputera urzędu certyfikacji na serwerze SQL **skryptu**

   - Upewnij się, że masz połączenie **CORPSQL01** serwera.**

     >[!NOTE]
     >Upewnij się, użytkownik jest zalogowany jako DBA Z Start menu, uruchom programu SQL Server Management Studio.

     ![W Połącz z serwerem okna dialogowego, nazwy serwera wpisz CORPSQL01, , a następnie kliknij przycisk Connect.](media/mim-cm-deploy/image030.png)

10. W drzewie konsoli rozwiń węzeł **zabezpieczeń**, a następnie kliknij przycisk logowania.

11. Kliknij prawym przyciskiem myszy **logowania**, a następnie kliknij przycisk **nowe dane logowania**.

12. Na **ogólne** strony w nazwa logowania wpisz contosoCORPCA.

    - Wybierz **uwierzytelniania systemu Windows**.

    - Domyślna baza danych jest **FIMCertificateManagement**.

    - W okienku po lewej stronie wybierz **mapowania użytkowników**.

13. W okienku po prawej stronie, kliknij pole wyboru w **mapy** kolumnę obok **FIMCertificateManagement**.

14. W **członkostwo roli dla bazy danych: FIMCertificateManagement** listy, Włącz **clmApp** roli.**

15. Zamknij **programu Microsoft SQL Server Management Studio**.

16. Instalowanie modułów programu FIM CM urzędu certyfikacji w urzędzie certyfikacji

    ![nośnik/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Nie klikaj przycisku do czasu ukończenia działania Kreatora konfiguracji. Rejestrowanie dla kreatora można znaleźć tutaj: **% programfiles %\\Microsoft Forefront Identity Management\\2010\\zarządzania certyfikatami\\config.log**

17. Kliknij przycisk **Finish** (Zakończ).

    ![W szczegóły okienku kliknij dwukrotnie izolacji klucza CNG.](media/mim-cm-deploy/image033.png)

18. Zamknij wszystkie otwarte okna.

19. Dodaj https://cm.contoso.com/certificatemanagement do strefy Lokalny intranet w przeglądarce.

20. Odwiedź witrynę z serwera CORPCM https://cm.contoso.com/certificatemanagement  

    ![Diagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Sprawdzić, czy Usługa izolacji klucza CNG

1. Z **narzędzia administracyjne**, otwórz **usług**.

2. W **szczegóły** okienku kliknij dwukrotnie **izolacji klucza CNG**.

3. Na **ogólne** Zmień **uruchamiana** do **automatyczne**.

4. Na **ogólne** karcie, uruchom usługę, jeśli nie jest w stanie uruchomionym.

5. Na **ogólne** , kliknij pozycję **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalowanie i Konfigurowanie modułów urzędu certyfikacji:

W tym kroku zainstalujemy i skonfigurowania modułów programu FIM CM urzędu certyfikacji w urzędzie certyfikacji.

1. Konfigurowanie programu FIM CM tylko sprawdzić uprawnienia użytkownika dla operacji zarządzania

2. W **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\zarządzania certyfikatami\\web** okna, utworzyć kopię  **plik Web.config** nazewnictwa kopii **web.1.config**.

3. W **Web** okna, kliknij prawym przyciskiem myszy **Web.config**, a następnie kliknij przycisk **Otwórz**.

    >[!Note]
    >Plik Web.config jest otwarty w Notatniku

4. Po otwarciu pliku, naciśnij klawisze CTRL + F.

5. W **Znajdź i Zamień** okna dialogowego, **Znajdź** wpisz **UseUser**, a następnie kliknij przycisk **Znajdź następny** trzy razy.

6. Zamknij **Znajdź i Zamień** okno dialogowe.

7. Należy w wierszu  **\<dodać key="Clm.RequestSecurity.Flags" wartość = "UseUser UseGroups" /\>**. Z **narzędzia administracyjne\<, otwórz \>Podgląd zdarzeń**.

8. W drzewie konsoli rozwiń węzeł Dzienniki aplikacji i usług, a następnie kliknij przycisk zarządzanie certyfikatami programu FIM.

9. Na liście zdarzeń, sprawdź, czy najnowsze zdarzeń czy \<nie\> zawierać ostrzeżenie lub błąd zdarzenia od czasu ostatniego ponownego uruchomienia usług certyfikatów.\>

10. Ostatnie zdarzenie powinny prezentować, że moduł zakończenia ładowane przy użyciu ustawień:

11. Minimalizowanie **Podgląd zdarzeń**.**

12. Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka Windows®

13. W drzewie konsoli rozwiń węzeł **contoso-CORPCA-CA**, a następnie kliknij przycisk **wystawione certyfikaty**.

14. W **szczegóły** okienku kliknij dwukrotnie certyfikat z **CONTOSO**MIMCMAgent w Nazwa żądającego kolumny i programu FIM CM Podpisywanie w szablonu certyfikatu kolumny.

15. Na **szczegóły** wybierz opcję **odcisk palca** pola.

16. Wybierz odcisk palca, a następnie naciśnij klawisze CTRL + C. Czy **nie** zawierają spacje wiodące na liście znaków odcisk palca. W **certyfikatu** okno dialogowe, kliknij przycisk OK.

17. Z **Start** menu w Wyszukaj programy i pliki wpisz Notatnik, a następnie naciśnij klawisz ENTER. W **Notatnik**, z **Edytuj** menu, kliknij przycisk Wklej. Z **Edytuj** menu, kliknij przycisk **Zastąp**.

18. Kliknij przycisk **OK**.

19. W **Znajdź** wpisz znak spacji, a następnie kliknij przycisk Zamień wszystkie.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Spowoduje to usunięcie wszystkich odstępów między znakami w odcisk palca.

1. W **Zastąp** okno dialogowe, kliknij przycisk anulować.

2. Wybierz skonwertowany **thumbprintstring**, a następnie naciśnij klawisz CTRL + C.

3. Zamknij **Notatnik** bez zapisywania zmian.

4. Konfigurowanie modułu zasad zarządzania certyfikatami w usłudze FIM Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA**, a następnie kliknij przycisk **właściwości**.

5. W **właściwości contoso-CORPCA-CA** na okna dialogowego **modułu zasad** , kliknij pozycję **właściwości**.

6. Na **ogólne** karcie, upewnij się, że **przekazywania żądań CM usługi FIM do domyślnego modułu zasad do przetwarzania** jest zaznaczone.

    >[!Note]
    >Na certyfikaty podpisywania , kliknij pozycję Dodaj.

7. W oknie dialogowym certyfikat, kliknij prawym przyciskiem myszy **Określ skrót certyfikatu hex** , a następnie kliknij przycisk **Wklej**.

8. Jeśli **OK** przycisk nie jest włączone, możesz przypadkowo objęte ukrytych znaków ciągu odcisk palca podczas skopiowano odcisk palca certyfikatu clmAgent.

9. Powtórz wszystkie kroki od **zadanie 4: Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka systemu Windows** w tym ćwiczeniu.

10. Zamknij wszystkie otwarte okna.

### <a name="configure-the-mim-cm-exit-module"></a>W właściwości konfiguracji okno dialogowe pola, upewnij się, że odcisk palca zostanie wyświetlony w prawidłowe certyfikaty podpisywania , a następnie kliknij przycisk OK.

1. Z **narzędzia administracyjne**, otwórz **urzędu certyfikacji**.

2. W **zarządzanie certyfikatami programu FIM** , kliknij przycisk pole **OK**.

3. Zamknij **urzędu certyfikacji** konsoli.

4. Zamknij wszystkie otwarte okna, a następnie wylogować. Ostatni krok we wdrożeniu** jest chcemy upewnić się, że CONTOSO**menedżerów określa można wdrożyć i utworzyć szablony i skonfigurować system bez schematu oraz Administratorzy domeny.
5. Skrypt dalej będzie listy ACL uprawnienia w szablonach certyfikatów przy użyciu dsacls.

6. Uruchom za pomocą konta, które ma pełne uprawnienia do zmiany zabezpieczeń uprawnienia odczytu i zapisu do każdego istniejącego szablonu certyfikatu w lesie.

7. Pierwsze kroki: **konfigurowania punktu połączenia usługi i grupy docelowej uprawnienia & delegowanie zarządzania szablonu profilu Skonfiguruj uprawnienia w punkcie połączenia usługi (SCP).

8. Konfigurowanie zarządzania szablonu delegowanego profilem.

9. Brak skryptów

10. Upewnij się, że masz połączenie **CORPDC** serwera wirtualnego.

11. Zaloguj się jako **contoso**corpadmin

12. Z *narzędzia administracyjne*, otwórz **użytkownicy usługi Active Directory i komputery**.

    >[!NOTE] 
    >W `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`użytkownicy usługi Active Directory i komputeryna widoku menu, upewnij się, że funkcje zaawansowane jest włączona. `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. W drzewie konsoli rozwiń węzeł **Contoso.com**  systemu  Microsoft  cykl życia certyfikatu Menedżer, a następnie kliknij przycisk CORPCM.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Kliknij prawym przyciskiem myszy CORPCM, a następnie kliknij przycisk właściwości.

1. Przywróć **urzędu certyfikacji** konsoli.

2. W **właściwości CORPCM** na okna dialogowego **zabezpieczeń** Dodaj następujące grupy przy użyciu odpowiednich uprawnień:

3. Określa menedżerów

4. Inspekcja zarządzania certyfikatami w usłudze FIM

5. Agenta rejestracji programu FIM CM

    >[!NOTE]
    >Rejestrowanie programu FIM CM żądania

6. Odzyskaj żądania zarządzania certyfikatami w usłudze FIM

7. Odnów FIM CM żądania

8. Odwołaj żądanie zarządzania certyfikatami w usłudze FIM

9. FIM CM żądania odblokowania karty inteligentnej

10. Pomoc techniczna określa

    >[!Note]
    >W właściwości CORPDC okno dialogowe, kliknij przycisk OK.

11. Pozostaw **użytkownicy usługi Active Directory i komputery** otworzyć.

12. Konfigurowanie uprawnień na obiekty podrzędne użytkownika

13. Upewnij się, że jesteś nadal w **użytkownicy usługi Active Directory i komputery** konsoli.

### <a name="configure-the-fim-cm-policy-module"></a>W drzewie konsoli kliknij prawym przyciskiem myszy Contoso.com, a następnie kliknij przycisk właściwości.

1. Przywróć **urzędu certyfikacji** konsoli.

2. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA**, a następnie kliknij przycisk **właściwości**.

3. W **właściwości contoso-CORPCA-CA** na okna dialogowego **modułu zasad** , kliknij pozycję **właściwości**.

    - Na **ogólne** karcie, upewnij się, że **przekazywania żądań CM usługi FIM do domyślnego modułu zasad do przetwarzania** jest zaznaczone.

    - Na **certyfikaty podpisywania** , kliknij pozycję **Dodaj**.

    - W oknie dialogowym certyfikat, kliknij prawym przyciskiem myszy **Określ skrót certyfikatu hex** , a następnie kliknij przycisk **Wklej**.

    - Odzyskaj żądania zarządzania certyfikatami w usłudze FIM

        >[!Note]
        >Jeśli **OK** przycisk nie jest włączone, możesz przypadkowo objęte ukrytych znaków ciągu odcisk palca podczas skopiowano odcisk palca certyfikatu clmAgent. Powtórz wszystkie kroki od **zadanie 4: Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka systemu Windows** w tym ćwiczeniu.

4. W **właściwości konfiguracji** okno dialogowe pola, upewnij się, że odcisk palca zostanie wyświetlony w **prawidłowe certyfikaty podpisywania** , a następnie kliknij przycisk **OK**.

5. W **zarządzanie certyfikatami programu FIM** , kliknij przycisk pole **OK**.

6. Uruchom za pomocą konta, które ma pełne uprawnienia do zmiany zabezpieczeń uprawnienia odczytu i zapisu do każdego istniejącego szablonu certyfikatu w lesie.

7. Pierwsze kroki: **konfigurowania punktu połączenia usługi i grupy docelowej uprawnienia & delegowanie zarządzania szablonu profilu

8. Skonfiguruj uprawnienia w punkcie połączenia usługi (SCP).

9. Konfigurowanie zarządzania szablonu delegowanego profilem.

10. Zamknij **urzędu certyfikacji** konsoli.

11. Zamknij wszystkie otwarte okna, a następnie wylogować.

**Ostatni krok we wdrożeniu** jest chcemy upewnić się, że CONTOSO\\menedżerów określa można wdrożyć i utworzyć szablony i skonfigurować system bez schematu oraz Administratorzy domeny. Skrypt dalej będzie listy ACL uprawnienia w szablonach certyfikatów przy użyciu dsacls. Uruchom za pomocą konta, które ma pełne uprawnienia do zmiany zabezpieczeń uprawnienia odczytu i zapisu do każdego istniejącego szablonu certyfikatu w lesie.

Pierwsze kroki: **konfigurowania punktu połączenia usługi i grupy docelowej uprawnienia & delegowanie zarządzania szablonu profilu**

1. Skonfiguruj uprawnienia w punkcie połączenia usługi (SCP).

2. Konfigurowanie zarządzania szablonu delegowanego profilem.

3. Skonfiguruj uprawnienia w punkcie połączenia usługi (SCP). **\<Brak skryptów\>**

4.   Upewnij się, że masz połączenie **CORPDC** serwera wirtualnego.

5. Zaloguj się jako **contoso\\corpadmin**

6. Z **narzędzia administracyjne**, otwórz **użytkownicy usługi Active Directory i komputery**.

7. W **użytkownicy usługi Active Directory i komputery**na **widoku** menu, upewnij się, że **funkcje zaawansowane** jest włączona.

8. W drzewie konsoli rozwiń węzeł **Contoso.com** \| **systemu** \| **Microsoft** \| **cykl życia certyfikatu Menedżer**, a następnie kliknij przycisk **CORPCM**.

9. Kliknij prawym przyciskiem myszy **CORPCM**, a następnie kliknij przycisk **właściwości**.

10. W **właściwości CORPCM** na okna dialogowego **zabezpieczeń** Dodaj następujące grupy przy użyciu odpowiednich uprawnień:

    | Grupa          | Uprawnienia      |
    |----------------|------------------|
    | Określa menedżerów | Odczyt </br> Inspekcja zarządzania certyfikatami w usłudze FIM</br> Agenta rejestracji programu FIM CM</br> Rejestrowanie programu FIM CM żądania</br> Odzyskaj żądania zarządzania certyfikatami w usłudze FIM</br> Odnów FIM CM żądania</br> Odwołaj żądanie zarządzania certyfikatami w usłudze FIM </br> FIM CM żądania odblokowania karty inteligentnej |
    | Pomoc techniczna określa | Odczyt</br> Agenta rejestracji programu FIM CM</br> Odwołaj żądanie zarządzania certyfikatami w usłudze FIM</br> FIM CM żądania odblokowania karty inteligentnej |

11. W **właściwości CORPDC** okno dialogowe, kliknij przycisk **OK**.

12. Pozostaw **użytkownicy usługi Active Directory i komputery** otworzyć.

**Konfigurowanie uprawnień na obiekty podrzędne użytkownika**

1. Upewnij się, że jesteś nadal w **użytkownicy usługi Active Directory i komputery** konsoli.

2. W drzewie konsoli kliknij prawym przyciskiem myszy **Contoso.com**, a następnie kliknij przycisk **właściwości**.

3. Na **zabezpieczeń** kliknij pozycję **Zaawansowane**.

4. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.

5. W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **określa menedżerów**, a następnie kliknij przycisk **OK**.

6. W **wpis uprawnienia dla Contoso** okna dialogowego, **dotyczą** listy, wybierz **obiekty użytkownika podrzędnym** , a następnie włączyć **Zezwalaj**pole wyboru dla następujących **uprawnienia**:

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Inspekcja zarządzania certyfikatami w usłudze FIM**

    - **Agenta rejestracji programu FIM CM**

    - **Rejestrowanie programu FIM CM żądania**

    - **Odzyskaj żądania zarządzania certyfikatami w usłudze FIM**

    - **Odnów FIM CM żądania**

    - **Odwołaj żądanie zarządzania certyfikatami w usłudze FIM**

    - **FIM CM żądania odblokowania karty inteligentnej**

7. W **wpis uprawnienia dla Contoso** okno dialogowe, kliknij przycisk **OK**.

8. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.

9. W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **pomoc techniczna Określa**, a następnie kliknij przycisk **OK**.

10. W **wpis uprawnienia dla Contoso** okna dialogowego, **dotyczą** listy, wybierz **obiekty użytkownika podrzędnym** , a następnie wybierz **Zezwalaj**pole wyboru dla następujących **uprawnienia**:

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Agenta rejestracji programu FIM CM**

    - **Odwołaj żądanie zarządzania certyfikatami w usłudze FIM**

    - **FIM CM żądania odblokowania karty inteligentnej**

11. W **wpis uprawnienia dla Contoso** okno dialogowe, kliknij przycisk **OK**.

12. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **OK**.

13. W **contoso.com właściwości** okno dialogowe, kliknij przycisk **OK**.

14. Pozostaw **użytkownicy usługi Active Directory i komputery** otworzyć.

**Konfigurowanie uprawnień na obiekty podrzędne użytkownika \<skryptu\>**

1. Upewnij się, że jesteś nadal w **użytkownicy usługi Active Directory i komputery** konsoli.

2. W drzewie konsoli kliknij prawym przyciskiem myszy **Contoso.com**, a następnie kliknij przycisk **właściwości**.

3. Na **zabezpieczeń** kliknij pozycję **Zaawansowane**.

4. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.

5. W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **określa menedżerów**, a następnie kliknij przycisk **OK**.

6. W **wpis uprawnienia dla CONTOSO** okna dialogowego, **dotyczą** listy, wybierz **obiekty użytkownika podrzędnym** , a następnie włączyć **Zezwalaj**pole wyboru dla następujących **uprawnienia**:

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Inspekcja zarządzania certyfikatami w usłudze FIM**

    - **Agenta rejestracji programu FIM CM**

    - **Rejestrowanie programu FIM CM żądania**

    - **Odzyskaj żądania zarządzania certyfikatami w usłudze FIM**

    - **Odnów FIM CM żądania**

    - **Odwołaj żądanie zarządzania certyfikatami w usłudze FIM**

    - **FIM CM żądania odblokowania karty inteligentnej**

7. W **wpis uprawnienia dla CONTOSO** okno dialogowe, kliknij przycisk **OK**.

8. W **Zaawansowane ustawienia zabezpieczeń dla CONTOSO** okno dialogowe, kliknij przycisk **Dodaj**.

9. W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **pomoc techniczna Określa**, a następnie kliknij przycisk **OK**.

10. W **wpis uprawnienia dla CONTOSO** okna dialogowego, **dotyczą** listy, wybierz **obiekty użytkownika podrzędnym** , a następnie wybierz **Zezwalaj**pole wyboru dla następujących **uprawnienia**:

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Agenta rejestracji programu FIM CM**

    - **Odwołaj żądanie zarządzania certyfikatami w usłudze FIM**

    - **FIM CM żądania odblokowania karty inteligentnej**

11. W **wpis uprawnienia dla contoso** okno dialogowe, kliknij przycisk **OK**.

12. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **OK**.

13. W **contoso.com właściwości** okno dialogowe, kliknij przycisk **OK**.

14. Pozostaw **użytkownicy usługi Active Directory i komputery** otworzyć.

Czynności drugiego: **uprawnień zarządzania szablonów certyfikatów delegowanie \<skryptu\>**

- Delegowanie uprawnień w kontenerze szablonów certyfikatów.

- Delegowanie uprawnień w kontenerze OID.

- Delegowanie uprawnień do istniejących szablonów certyfikatów.

Definiowanie uprawnień w kontenerze szablonów certyfikatów:

1. Przywróć **Lokacje usługi Active Directory i usługi** konsoli.

2. W drzewie konsoli rozwiń węzeł **usług**, rozwiń węzeł **Public Key Services**, a następnie kliknij przycisk **szablonów certyfikatów**.

3. W drzewie konsoli kliknij prawym przyciskiem myszy **szablonów certyfikatów**, a następnie kliknij przycisk **Deleguj kontrolę**.

4. W **delegowania kontroli** kreatora, kliknij przycisk **dalej**.

5. Na **użytkowników lub grup** kliknij przycisk **Dodaj**.

6. W **Wybieranie: użytkownicy, komputery lub grupy** okna dialogowego, **wprowadź nazwy obiektów do wybrania** wpisz **określa menedżerów**, a następnie kliknij przycisk **OK**.

7. Na **użytkowników lub grup** kliknij przycisk **dalej**.

8. Na **zadania do oddelegowania** kliknij przycisk **Utwórz zadanie niestandardowe do delegowania**, a następnie kliknij przycisk **dalej**.

9.  Na **typ obiektu usługi Active Directory** strony, upewnij się, że **tego folderu, istniejące obiekty w tym folderze oraz tworzenie nowych obiektów w tym folderze** jest zaznaczone, a następnie kliknij przycisk **dalej**.

10. Na **uprawnienia** strony w **uprawnienia** listy, wybierz **Pełna kontrola** pole wyboru, a następnie kliknij przycisk **dalej**.

11. Na **Kończenie pracy Kreatora delegowania kontroli** kliknij przycisk **Zakończ**.

Definiowanie uprawnień w kontenerze identyfikatora OID:

1. W drzewie konsoli kliknij prawym przyciskiem myszy **OID**, a następnie kliknij przycisk **właściwości**.

2. W **właściwości OID** na okna dialogowego **zabezpieczeń** , kliknij pozycję **zaawansowane**.

3. W **Zaawansowane ustawienia zabezpieczeń dla identyfikatora OID** okno dialogowe, kliknij przycisk **Dodaj**.

4. W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **określa menedżerów**, a następnie kliknij przycisk **OK**.

5. W **wpis uprawnienia dla identyfikatora OID** okno dialogowe pola, upewnij się, że uprawnienia dotyczą **ten obiekt i wszystkie obiekty zależne**, kliknij przycisk **Pełna kontrola**, a następnie kliknij przycisk  **OK**.

6. W **Zaawansowane ustawienia zabezpieczeń dla identyfikatora OID** okno dialogowe, kliknij przycisk **OK**.

7. W **właściwości OID** okno dialogowe, kliknij przycisk **OK**.

8. Zamknij **Lokacje i usługi Active Directory**.

**Skrypty: Uprawnienia do kontenera identyfikator OID, szablon profilu i szablonów certyfikatów**

![Diagram](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Skrypty: Delegowanie uprawnień do istniejących szablonów certyfikatów.**  

![Diagram](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
