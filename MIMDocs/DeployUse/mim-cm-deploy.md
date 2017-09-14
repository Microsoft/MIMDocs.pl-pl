---
title: "Wdrożenie Menedżera certyfikatów programu Microsoft Identity Manager | Dokumentacja firmy Microsoft"
description: "Zainstaluj Menedżera certyfikatów programu Microsoft Identity Manager 2016"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/16/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 61987d5b259830be0cf0bc12832a853e24e9c282
ms.sourcegitcommit: f29f02fa8437fa55e86afd7b0b99a36d2306b96b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/08/2017
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Wdrożenie Menedżera certyfikatów programu Microsoft Identity Manager 2016 (MIM CM)

Instalacja Menedżera certyfikatów programu Microsoft Identity Manager 2016 (MIM CM) obejmuje kilka kroków. Aby uprościć proces możemy są podziału rzeczy. Brak wstępne kroki, które należy podjąć przed żadnych czynności rzeczywisty zarządzania Certyfikatami programu MIM. Bez wstępnego pracy prawdopodobnie instalacja nie powiedzie się. 

1. Przegląd wdrożenia
2. Kroki przed wdrożeniem
3. Co?

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
  - Laboratorium składa się z serwerów z systemem windows 2016 centrum danych.
       >[!NOTE]
Dla więcej szczegółów na platformach obsługiwanych przez program MIM 2016 Spójrz na artykuł [platformy obsługiwane przez program MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)
- Kroki przed wdrożeniem
  - [Rozszerzanie schematu](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)
  - Tworzenie konta usług
  - [Tworzenie szablonów certyfikatów](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)
  - IIS
  - Konfigurowanie protokołu Kerberos
  - Kroki związane z bazy danych
    - Wymagania dotyczące konfiguracji programu SQL
    - Uprawnienia dotyczące bazy danych
- wdrażania

## <a name="pre-deployment-steps"></a>Kroki przed wdrożeniem

Kreator konfiguracji zarządzania Certyfikatami programu MIM wymaga informacji dostarczanych wzdłuż sposobu, w celu pomyślnego wykonania. Kroki przed wdrożeniem będzie (NIEKOMPLETNE TRAKTOWAĆ tutaj)

![](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Rozszerzanie schematu

Procesu rozszerzenia schematu jest proste, ale musi być skontaktowali się ostrożnie z powodu natury nieodwracalne.

>[!NOTE]
Ten krok wymaga, czy używane konto ma uprawnienia administratora schematu.

- Przejdź do lokalizacji nośnika MIM i przejdź do \\zarządzania certyfikatami\\x64 folderu.

- Skopiuj folder schematu na kontrolerze domeny CORPDC, a następnie przejdź do niego.

    ![](media/mim-cm-deploy/image005.png)

- Uruchom skrypt resourceForestModifySchema.vbs pojedynczego lasu scenariusza

- W scenariuszu lasu zasobów uruchomić skrypty:
  - DomenaA — użytkownicy znajdujący się (userForestModifySchema.vbs)
  - ResourceForestB — lokalizacja instalacji CM (resourceForestModifySchema.vbs)

>[!NOTE]
Zmiany schematu są operacji jednokierunkowej i wymagają lesie odzyskiwania, aby wycofać dlatego upewnij się, masz niezbędne kopie zapasowe. Aby uzyskać szczegółowe informacje o zmiany wprowadzone w schemacie po wykonaniu tej operacji zapoznaj się z artykułem [zmiany schematu programu Forefront Identity Manager 2010 certyfikatu zarządzania](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

![](media/mim-cm-deploy/image007.png)

Uruchom skrypt i powinien zostać wyświetlony sukcesu raz wiadomości, że skrypt zostało ukończone.

![Komunikat z potwierdzeniem](media/mim-cm-deploy/image009.png)

Teraz rozszerzania schematu w usłudze AD do obsługi zarządzania Certyfikatami programu MIM.

### <a name="creating-service-accounts-and-groups"></a>Tworzenie konta usługi i grupy

W poniższej tabeli przedstawiono kont i uprawnień wymaganych przez zarządzania Certyfikatami programu MIM.
Można zezwolić MIM CM automatycznie Utwórz następujące konta lub można je utworzyć przed instalacją. Można zmienić nazwy własnego konta. Utwórz konta użytkownika, należy rozważyć nazw kont użytkowników w sposób łatwy do dopasowania nazwy konta użytkownika dla jej funkcji.

Użytkownicy:

![](media/mim-cm-deploy/image010.png)

![](media/mim-cm-deploy/image012.png)

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

Programu PowerShell: Konta agenta

```
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
|MIMCMKRAgent        | Odzyskuje archiwizacji kluczy prywatnych z urzędu certyfikacji. To konto użytkownika wymagane są następujące ustawienia kontroli dostępu:</br> -   **Zezwalaj na logowanie lokalne** prawa użytkownika.</br>-Członkostwo w lokalnej **Administratorzy** grupy. </br>— Rejestrowanie uprawnień na **KeyRecoveryAgent** szablonu certyfikatu. </br>-Certyfikat agenta odzyskiwania kluczy wydane i zainstalowany w magazynie użytkownika. Certyfikat musi być dodany do listy agentów odzyskiwania kluczy urzędu certyfikacji. </br>-Uprawnienia odczytu i zapisu uprawnienia do folderu tymczasowego systemu w następującej lokalizacji:```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Określa prawa i uprawnienia dla użytkowników i grup. To konto użytkownika wymagane są następujące ustawienia kontroli dostępu: </br>-Członkostwo w grupie domeny dostęp zgodny z systemami starszymi niż Windows 2000. </br> -Przyznanych **Generuj inspekcje zabezpieczeń** prawa użytkownika.             |
| MIMCMManagerAgent   | Wykonuje działania związane z zarządzaniem urzędu certyfikacji. </br> Ten użytkownik należy przypisać uprawnienia do zarządzania urzędu certyfikacji.        |
| MIMCMWebAgent       | Zawiera tożsamość puli aplikacji usług IIS. FIM CM jest uruchamiany w ramach Microsoft Win32® aplikacji programowania interfejsu procesu, który używa poświadczeń użytkownika. </br> To konto użytkownika wymagane są następujące ustawienia kontroli dostępu:</br> -Członkostwo w lokalnej **IIS_WPG, windows 2016 = IIS_IUSRS** grupy. </br>-Członkostwo w lokalnej **Administratorzy** grupy.</br>-Przyznanych **Generuj inspekcje zabezpieczeń** prawa użytkownika. </br>-Przyznanych **działanie jako część systemu operacyjnego** prawa użytkownika. </br>-Przyznanych **Zastępowanie tokenu poziomu procesu** prawa użytkownika.</br>-Przypisany jako tożsamość puli aplikacji usług IIS **CLMAppPool**. </br>-Przyznane uprawnienia do odczytu **HKEY_LOCAL_MACHINE\\oprogramowania\\Microsoft\\CLM\\1.0\\serwera\\WebUser** klucza rejestru. </br>-Tego konta również musi być zaufany dla celów delegacji.|
| MIMCMEnrollAgent    | Wykonuje rejestrowanie w imieniu użytkownika. To konto użytkownika wymagane są następujące ustawienia kontroli dostępu:</br>-Certyfikatu Agent rejestrowania, który jest wystawiony zainstalowany w magazynie użytkownika.</br>-   **Zezwalaj na logowanie lokalne** prawa użytkownika. </br>— Rejestrowanie uprawnień na **agenta rejestracji** szablonu certyfikatu (lub szablonu niestandardowego, jeśli jest on używany).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Tworzenie szablonów certyfikatów dla konta usługi MIM CM

Trzy z konta usług używane przez zarządzania Certyfikatami programu MIM jest wymagany certyfikat, a następnie Kreatora konfiguracji wymaga podania nazwy szablonów certyfikatów, których należy używać do żądania certyfikatów dla nich.

Konta usług, które wymagają certyfikatów są:

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
    MIM CM does not work with certificates based on version 3 certificate templates. You must create a Windows Server® 2003 Enterprise (version 2)certificate template. See the following link for V3 details https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade

6. W **właściwości nowego szablonu** na okna dialogowego **ogólne** karcie **Nazwa wyświetlana szablonu** wpisz **MIM CM podpisywania**. Zmień **okres ważności** do **2 lata**, a następnie wyczyść **Publikuj certyfikat w usłudze Active Directory** pole wyboru.

7. Na **obsługiwanie żądań** karcie, upewnij się, że **Zezwalaj na eksportowanie klucza prywatnego** pole wyboru jest zaznaczone, a następnie kliknij przycisk **karta Kryptografia**.

8. W **wybór kryptografii** okno dialogowe, wyłącz **Microsoft Enhanced Cryptographic Provider 1.0**, Włącz **Microsoft Enhanced RSA and AES Cryptographic Provider**, a następnie kliknij przycisk **OK**.

Na **nazwa podmiotu** kartę, wyczyść **Dołącz nazwę e-mail do nazwy podmiotu** i **nazwa E-mail** pola wyboru.

Na **rozszerzenia** karcie **rozszerzenia zawarte w tym szablonie** listy, upewnij się, że **zasady aplikacji** jest zaznaczone, a następnie kliknij przycisk **edycji** .

W **Edytowanie rozszerzenia zasad aplikacji** okno dialogowe, wybierz **systemu szyfrowania plików** i **zabezpieczanie poczty E-mail** zasady aplikacji. Kliknij przycisk **Usuń**, a następnie kliknij przycisk **OK**.

Na **zabezpieczeń** kartę należy wykonać następujące czynności:

- Usuń **administratora**.

- Usuń **Administratorzy domeny**.

- Usuń **użytkownicy domeny**.

- Przypisz tylko **odczytu** i **zapisu** uprawnień do **Administratorzy przedsiębiorstwa**.

- Dodaj **MIMCMAgent.**

- Przypisz **odczytu** i **Zarejestruj** uprawnień do **MIMCMAgent**.

W **właściwości nowego szablonu** okno dialogowe, kliknij przycisk **OK**.

Pozostaw **konsolę Szablony certyfikatów** otworzyć.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Tworzenie szablonu certyfikatu agenta rejestracji MIM CM

-   W **konsolę Szablony certyfikatów**w **szczegóły** okienku, wybierz i kliknij prawym przyciskiem myszy **agenta rejestracji**, a następnie kliknij przycisk **Duplikuj szablon**.

W **Duplikuj szablon** okno dialogowe, wybierz opcję **systemu Windows Server 2003 Enterprise**, a następnie kliknij przycisk **OK**.

W **właściwości nowego szablonu** na okna dialogowego **ogólne** karcie **Nazwa wyświetlana szablonu** wpisz **agenta rejestracji MIM CM**. Upewnij się, że **okres ważności** jest **2 lata**.

Na **obsługiwanie żądań** pozycję Włącz **Zezwalaj na eksportowanie klucza prywatnego**, a następnie kliknij przycisk **dostawców usług kryptograficznych lub Tab kryptografii.**

W **Wybieranie dostawcy CSP** okno dialogowe, wyłącz **Microsoft Base Cryptographic Provider 1.0**, wyłącz **Microsoft Enhanced Cryptographic Provider 1.0**, Włącz  **Microsoft Enhanced RSA and AES Cryptographic Provider**, a następnie kliknij przycisk **OK**.

Na **zabezpieczeń** kartę, wykonaj następujące czynności:

- Usuń **administratora**.

- Usuń **Administratorzy domeny**.

- Przypisz tylko **odczytu** i **zapisu** uprawnień do **Administratorzy przedsiębiorstwa**.

- Dodaj **MIMCMEnrollAgent**.

- Przypisz **odczytu** i **Zarejestruj** uprawnień do **MIMCMEnrollAgent**.

W **właściwości nowego szablonu** okno dialogowe, kliknij przycisk **OK**.

Pozostaw **konsolę Szablony certyfikatów** otworzyć.

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

8. Zamknij **konsolę Szablony certyfikatów**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publikowanie szablonów wymaganego certyfikatu w urzędzie certyfikacji

1. Przywróć **urzędu certyfikacji** konsoli.

2. W **urzędu certyfikacji** konsoli, w drzewie konsoli kliknij prawym przyciskiem myszy **szablonów certyfikatów**, wskaż polecenie **nowy**, a następnie kliknij przycisk **certyfikatu Szablon do wystawienia**.
3. W **Włączanie szablonu certyfikatu** okno dialogowe, wybierz opcję **MIM CM rejestracji agenta**, **agenta odzyskiwania kluczy zarządzania certyfikatami w usłudze MIM**, i **MIM CM podpisywania**. Kliknij przycisk **OK**.
4. W drzewie konsoli kliknij **szablonów certyfikatów**.
5. Sprawdź, czy trzy nowe szablony są wyświetlane w **szczegóły** okienka, a następnie Zamknij **urzędu certyfikacji**.
    ![Podpisywanie zarządzania Certyfikatami programu MIM](media/mim-cm-deploy/image016.png)
6. Zamknij wszystkie otwarte okna i Wyloguj.

### <a name="iis-configuration"></a>Konfiguracja usług IIS 

W celu zamówienia hoIn do hostowania witryny sieci Web do zarządzania certyfikatami w usłudze miejsca i skonfiguruj program IIS

#### <a name="install-and-configure-iis"></a>Instalowanie i konfigurowanie usług IIS

1. Zaloguj się do ** CORLog w jako **MIMINSTALL** konta

>[!IMPORTANT]
Konto instalacji MIM powinien mieć uprawnienia administratora lokalnego

2. Otwórz program powershell i uruchom następujące polecenie

   - ```Install-WindowsFeature –ConfigurationFilePath```

>[!NOTE]
 Witryny o nazwie domyślnej witryny sieci Web jest zainstalowana domyślnie w usługach IIS 7. Jeśli tej lokacji została zmieniona lub usunął lokację o nazwie domyślnej witryny sieci Web musi być dostępny przed zainstalowaniem zarządzania Certyfikatami programu MIM.

#### <a name="configuring-kerberos"></a>Konfigurowanie protokołu Kerberos

Konto MIMCMWebAgent będzie uruchomiony w portalu zarządzania Certyfikatami programu MIM. Domyślnie w usługach IIS oraz jądra tryb uwierzytelniania jest używany w usługach IIS domyślnie. Zostanie Wyłącz uwierzytelnianie trybu jądra protokołu Kerberos i skonfiguruj nazwy SPN konta MIMCMWebAgent. Niektóre polecenia wymaga uprawnień z podwyższonym poziomem uprawnień w usłudze active directory i serwera CORPCM.

![](media/mim-cm-deploy/image020.png)

```
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}

```

** Aktualizowanie usług IIS na **CORPCM**


![](media/mim-cm-deploy/image022.png)

```
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true

```


>[!NOTE]
Musisz dodać DNS rekordu A dla "cm.contoso.com", a następnie wskaż CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Wymaganie protokołu SSL w portalu zarządzania Certyfikatami programu MIM

Zdecydowanie zaleca się wymagać protokołu SSL w portalu zarządzania Certyfikatami programu MIM. Jeśli jeszcze nie zostanie Kreator ostrzega użytkownika o nim.

1. Zarejestruj się w sieci web certyfikat dla **cm.contoso.com** przypisać do domyślnej witryny

2. Otwórz **Menedżera usług IIS** i przejdź do **zarządzania certyfikatami**

3. W widoku funkcje kliknij dwukrotnie pozycję Ustawienia SSL.

4. Na stronie Ustawienia protokołu SSL, wybierz **Wymagaj protokołu SSL**.

5. W okienku Akcje kliknij polecenie **Zastosuj.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Konfiguracja bazy danych **CORPSQL** dla zarządzania Certyfikatami programu MIM

1. Upewnij się, że nawiązano połączenie z serwerem CORPSQL01.

2. Upewnij się, że użytkownik jest zalogowany jako administrator SQL

3. Uruchom poniższy skrypt T-SQL umożliwiają firmie CONTOSO\\MIMINSTALL konta, aby utworzyć bazę danych, gdy firma Microsoft, przejdź do kroku konfiguracji

>[!NOTE]
Musimy wrócić do bazy danych SQL gdy firma Microsoft gotowe moduł zakończenia i zasady

```
create login [CONTOSO\\MIMINSTALL] from windows;
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
```

![Komunikat o błędzie Kreatora konfiguracji zarządzania Certyfikatami programu MIM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Wdrożenie programu Microsoft Identity Manager 2016 certyfikatu zarządzania

1. Upewnij się, czy masz połączenie, a serwer CORPCM **MIMINSTALL** konto jest członkiem **Administratorzy lokalni** grupy.

2. Upewnij się, użytkownik jest zalogowany jako Contoso\\MIMINSTALL.

3. Zainstaluj dodatek SP1 dla programu Microsoft Identity Manager ISO.

4. **Otwórz** **zarządzania certyfikatami\\x64** katalogu.

5. W **x64** okna, kliknij prawym przyciskiem myszy **Instalator**, a następnie kliknij przycisk **Uruchom jako administrator**.

6. Na stronie Kreatora instalacji zarządzania certyfikatów Microsoft Identity Manager-Zapraszamy kliknij **dalej.**

7. Na stronie umowę licencyjną użytkownika oprogramowania przeczytaj umowę, Włącz akceptuję warunki umowy licencyjnej **pole wyboru**, a następnie kliknij przycisk Dalej.

8. Na stronie Instalacja niestandardowa, upewnij się, **portalu programu MIM CM** i **składniki MIM CM aktualizacji usługi** jest ustawiona do zainstalowania, a następnie **kliknij przycisk Dalej**.

9. Na stronie Folder wirtualny sieci Web, upewnij się, że nazwa folderu wirtualnej jest ** CertificateManagement, a następnie **kliknij przycisk Dalej**.

10. Na stronie zainstalować Microsoft Identity Manager certyfikat Management **kliknij przycisk Instaluj**.

11. Na **Ukończono** strony Kreatora instalacji zarządzania certyfikatów Microsoft Identity Manager **kliknij przycisk Zakończ**.

![Ukończono działanie Kreatora zarządzania Certyfikatami programu MIM](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Kreator konfiguracji programu Microsoft Identity Manager 2016 certyfikatu zarządzania

Przed zalogowaniem się do CORPCM dodać MIMINSTALL do **domeny Administratorzy, Administratorzy schematu i Administratorzy lokalni** grupy Kreatora konfiguracji. Ten można usunąć później po zakończeniu konfiguracji.      
    
![Komunikat o błędzie](media/mim-cm-deploy/image028.png)

1. Z **Start** menu, kliknij przycisk **Kreatora konfiguracji zarządzania certyfikatów**. I Uruchom jako **administratora**
2. Na **Witamy w Kreatorze konfiguracji** kliknij przycisk **dalej**.
3. Na **konfiguracji urzędu certyfikacji** upewnij się, że wybrany urząd certyfikacji jest **Contoso-CORPCA-CA**, upewnij się, że wybrany serwer jest **CORPCA. CONTOSO.COM**, a następnie kliknij przycisk **dalej**.
4. Na **Konfigurowanie bazy danych programu Microsoft® SQL Server®** strony w **nazwa programu SQL Server** wpisz **CORPSQL1** , Włącz **umożliwiają utworzenie moich poświadczeń Baza danych** pole wyboru, a następnie kliknij przycisk **dalej**.
5. Na **ustawienia bazy danych** Zaakceptuj domyślną nazwę bazy danych **FIMCertificateManagement**, upewnij się, że **zintegrowane uwierzytelnianie SQL** jest zaznaczone, a następnie Kliknij przycisk **dalej**.

6. Na **— Konfiguracja usługi Active Directory** zaakceptuj nazwę domyślną podany dla punktu połączenia usługi, a następnie kliknij pozycję **dalej**.

7. Na **metodę uwierzytelniania** Potwierdź **uwierzytelnianie zintegrowane systemu windows** jest zaznaczone, a następnie kliknij przycisk **dalej**.

8. Na **agentów — FIM CM** wyczyść **Użyj ustawień domyślnych programu FIM CM** pole wyboru, a następnie kliknij przycisk **kont niestandardowe**.

9. W **agentów — FIM CM** okno dialogowe z wieloma kartami, na poszczególnych kartach, wpisz następujące informacje:
   - Nazwa użytkownika: **aktualizacji** 
   - Hasło: **przekazać\@word1**
   - Potwierdź hasło: **przekazać\@word1**
   - Użyj istniejącego użytkownika: **włączone**
>[!NOTE]
Te konta utworzony wcześniej. Upewnij się, że procedur opisanych w kroku 8 są powtarzane dla wszystkich kart konta sześciu agenta.

![Kont zarządzania Certyfikatami programu MIM](media/mim-cm-deploy/image030.png)

10. Po zakończeniu wszystkich informacji o koncie agenta, kliknij przycisk **OK**.

11. Na **agentów — MIM CM** kliknij przycisk **dalej**.

12. Na **Konfigurowanie certyfikatów serwera** pozycję Włącz następujące szablony certyfikatu:
    - Szablon certyfikatu do użycia dla agenta odzyskiwania kluczy certyfikat agenta odzyskiwania: **MIMCMKeyRecoveryAgent**.
    - Szablon certyfikatu do użycia dla agenta programu FIM CM certyfikatu: **MIMCMSigning**.
    - Szablon certyfikatu do użycia dla certyfikatu agenta rejestracji: **FIMCMEnrollmentAgent**.
13. Na **certyfikaty serwera konfiguracji** kliknij przycisk **dalej**.
14. Na **ustawienia serwera poczty E-mail, wydrukować dokument** strony w **Określ nazwę serwera SMTP, które chcesz użyć do rejestracji powiadomień e-mail** a następnie kliknij przycisk **dalej.**
15. Na **wszystko gotowe do skonfigurowania** kliknij przycisk **Konfiguruj**.
16. W **Kreator konfiguracji — Microsoft Forefront Identity Manager 2010 R2** okno dialogowe ostrzeżenia kliknij **OK** potwierdzić katalog wirtualny usług IIS nie jest włączony protokół SSL.

    ![nośnik/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    Nie klikaj przycisku do czasu ukończenia działania Kreatora konfiguracji. Rejestrowanie dla kreatora można znaleźć tutaj:**% programfiles %\\Microsoft Forefront Identity Management\\2010\\zarządzania certyfikatami\\config.log**
17. Kliknij przycisk **Finish** (Zakończ).

![Ukończono działanie Kreatora zarządzania Certyfikatami programu MIM](media/mim-cm-deploy/image033.png)

18. Zamknij wszystkie otwarte okna.

19. Dodaj https://cm.contoso.com/certificatemanagement do strefy Lokalny intranet w przeglądarce.

20. Odwiedź witrynę z https://cm.contoso.com/certificatemanagement CORPCM serwera  

    ![](media/mim-cm-deploy/image035.png)

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
    Plik Web.config jest otwarty w Notatniku

4. Po otwarciu pliku, naciśnij klawisze CTRL + F.

5. W **Znajdź i Zamień** okna dialogowego, **Znajdź** wpisz **UseUser**, a następnie kliknij przycisk **Znajdź następny** trzy razy.

6. Zamknij **Znajdź i Zamień** okno dialogowe.

7. Należy w wierszu  **\<dodać key="Clm.RequestSecurity.Flags" wartość = "UseUser UseGroups" /\>**. Zmień wiersz odczytać  **\<dodać key="Clm.RequestSecurity.Flags" wartość = "UseUser" /\>**.

8. Zamknij plik, zapisywanie wszystkich zmian.

9. Utwórz konto komputera urzędu certyfikacji na serwerze SQL \<skryptu\>

10. Upewnij się, że masz połączenie **CORPSQL01** serwera.

11. Upewnij się, użytkownik jest zalogowany jako **DBA**

12. Z **Start** menu, uruchom **programu SQL Server Management Studio**.

13. W **Połącz z serwerem** okna dialogowego, **nazwy serwera** wpisz **CORPSQL01,** , a następnie kliknij przycisk **Connect**.

14. W drzewie konsoli rozwiń węzeł **zabezpieczeń**, a następnie kliknij przycisk **logowania**.

15. Kliknij prawym przyciskiem myszy **logowania**, a następnie kliknij przycisk **nowe dane logowania**.

16. Na **ogólne** strony w **nazwa logowania** wpisz **contoso\\CORPCA\$**. Wybierz **uwierzytelniania systemu Windows**. Domyślna baza danych jest **FIMCertificateManagement**.

17. W okienku po lewej stronie wybierz **mapowania użytkowników**. W okienku po prawej stronie, kliknij pole wyboru w **mapy** kolumnę obok **FIMCertificateManagement**. W **członkostwo roli dla bazy danych: FIMCertificateManagement** listy, Włącz **clmApp** roli.

18. Kliknij przycisk **OK**.

19. Zamknij **programu Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalowanie modułów programu FIM CM urzędu certyfikacji w urzędzie certyfikacji

1. Upewnij się, że masz połączenie **CORPCA** serwera.

2. W **X64** systemu windows, kliknij prawym przyciskiem myszy **Setup.exe**, a następnie kliknij przycisk **Uruchom jako administrator**.

3. Na **Kreatora instalacji zarządzania certyfikatów Microsoft Identity Manager — Zapraszamy** kliknij przycisk **dalej**.

4. Na **umowę licencyjną użytkownika oprogramowania** strony, zapoznaj się z umową. Wybierz **akceptuję warunki umowy licencyjnej** pole wyboru, a następnie kliknij przycisk **dalej**.

5. Na **Instalacja niestandardowa** wybierz pozycję **portalu programu MIM CM**, a następnie kliknij przycisk **tej funkcji nie będą dostępne**.

6. Na **Instalacja niestandardowa** wybierz pozycję **usługi aktualizacji programu MIM CM**, a następnie kliknij przycisk **ta funkcja nie będzie dostępna**.

    >[!Note]
    Spowoduje to pozostawienie plików MIM CM urzędu certyfikacji jako funkcję tylko włączone dla instalacji.

7. Na **Instalacja niestandardowa** kliknij przycisk **dalej**.

8. Na **zainstalować Microsoft Identity Manager certyfikat Management** kliknij przycisk **zainstalować**.

9. Na **ukończyć Kreatora instalacji zarządzania certyfikatów Microsoft Identity Manager** kliknij przycisk **Zakończ**.

10. Zamknij wszystkie otwarte okna.

### <a name="configure-the-mim-cm-exit-module"></a>Skonfiguruj moduł zakończenia zarządzania Certyfikatami programu MIM

1. Z **narzędzia administracyjne**, otwórz **urzędu certyfikacji**.

2. W drzewie konsoli kliknij prawym przyciskiem myszy **contoso-CORPCA-CA**, a następnie kliknij przycisk **właściwości**.

3. Na **moduł zakończenia** wybierz opcję **moduł zakończenia FIM CM**, a następnie kliknij przycisk **właściwości**.

4. W **Określ parametry połączenia bazy danych zarządzania certyfikatami w usłudze** wpisz **Connect Timeout = 15; Utrzymuj informacje o zabezpieczeniach = True; Zabezpieczenia zintegrowane = sspi; Initial Catalog = FIMCertificateManagement; źródło danych = CORPSQL01**. Pozostaw **szyfrowania parametrów połączenia** włączone pole wyboru, a następnie kliknij przycisk **OK**.
5. W **zarządzanie certyfikatami programu FIM Microsoft** , kliknij przycisk pole **OK**.

6. W **właściwości contoso-CORPCA-CA** okno dialogowe, kliknij przycisk **OK**.

7. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA***,* wskaż **wszystkie zadania**, a następnie kliknij przycisk **Zatrzymaj usługę**. Zaczekaj, aż do zatrzymania usługi certyfikatów Active Directory.

8. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA***,* wskaż **wszystkie zadania**, a następnie kliknij przycisk **Uruchom usługę**.

9. Minimalizowanie **urzędu certyfikacji** konsoli.

10. Z **narzędzia administracyjne**, otwórz **Podgląd zdarzeń**.

11. W drzewie konsoli rozwiń węzeł **Dzienniki aplikacji i usług**, a następnie kliknij przycisk **zarządzanie certyfikatami programu FIM**.

12. Na liście zdarzeń, sprawdź, czy najnowsze zdarzeń czy *nie* zawierać **ostrzeżenie** lub **błąd** zdarzenia od czasu ostatniego ponownego uruchomienia usług certyfikatów.

    >[!NOTE] 
    Ostatnie zdarzenie powinny prezentować, że moduł zakończenia ładowane przy użyciu ustawień z```SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit```

13. Minimalizowanie **Podgląd zdarzeń**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka Windows®

1. Przywróć **urzędu certyfikacji** konsoli.

2. W drzewie konsoli rozwiń węzeł **contoso-CORPCA-CA**, a następnie kliknij przycisk **wystawione certyfikaty**.

3. W **szczegóły** okienku kliknij dwukrotnie certyfikat z **CONTOSO\\MIMCMAgent** w **Nazwa żądającego** kolumny i **programu FIM CM Podpisywanie** w **szablonu certyfikatu** kolumny.

4. Na **szczegóły** wybierz opcję **odcisk palca** pola.

5. Wybierz odcisk palca, a następnie naciśnij klawisze CTRL + C.

    >[!NOTE]
    Czy **nie** zawierają spacje wiodące na liście znaków odcisk palca.

6. W **certyfikatu** okno dialogowe, kliknij przycisk **OK**.

7. Z **Start** menu w **Wyszukaj programy i pliki** wpisz **Notatnik**, a następnie naciśnij klawisz ENTER.

8. W **Notatnik**, z **Edytuj** menu, kliknij przycisk **Wklej**.

9. Z **Edytuj** menu, kliknij przycisk **Zastąp**.

10. W **Znajdź** wpisz znak spacji, a następnie kliknij przycisk **Zamień wszystkie**.

    >[!Note]
    Spowoduje to usunięcie wszystkich odstępów między znakami w odcisk palca.
    
11. W **Zastąp** okno dialogowe, kliknij przycisk **anulować**.

12. Wybierz skonwertowany *thumbprintstring*, a następnie naciśnij klawisz CTRL + C.

13. Zamknij **Notatnik** bez zapisywania zmian.

### <a name="configure-the-fim-cm-policy-module"></a>Konfigurowanie modułu zasad zarządzania certyfikatami w usłudze FIM

1. Przywróć **urzędu certyfikacji** konsoli.

2. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA**, a następnie kliknij przycisk **właściwości**.

3.  W **właściwości contoso-CORPCA-CA** na okna dialogowego **modułu zasad** , kliknij pozycję **właściwości**.

- Na **ogólne** karcie, upewnij się, że **przekazywania żądań CM usługi FIM do domyślnego modułu zasad do przetwarzania** jest zaznaczone.
- Na **certyfikaty podpisywania** , kliknij pozycję **Dodaj**.
- W oknie dialogowym certyfikat, kliknij prawym przyciskiem myszy **Określ skrót certyfikatu hex** , a następnie kliknij przycisk **Wklej**.
- W **certyfikatu** okno dialogowe, kliknij przycisk **OK**.
    >[!Note]
    Jeśli **OK** przycisk nie jest włączone, możesz przypadkowo objęte ukrytych znaków ciągu odcisk palca podczas skopiowano odcisk palca certyfikatu clmAgent. Powtórz wszystkie kroki od **zadanie 4: Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka systemu Windows** w tym ćwiczeniu.

- W **właściwości konfiguracji** okno dialogowe pola, upewnij się, że odcisk palca zostanie wyświetlony w **prawidłowe certyfikaty podpisywania** , a następnie kliknij przycisk **OK**.

- W **zarządzanie certyfikatami programu FIM** , kliknij przycisk pole **OK**.

- W **właściwości contoso-CORPCA-CA** okno dialogowe, kliknij przycisk **OK**.

- Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA***,* wskaż **wszystkie zadania**, a następnie kliknij przycisk **Zatrzymaj usługę**.

- Zaczekaj, aż do zatrzymania usługi certyfikatów Active Directory.

- Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA***,* wskaż **wszystkie zadania**, a następnie kliknij przycisk **Uruchom usługę**.

- Zamknij **urzędu certyfikacji** konsoli.

- Zamknij wszystkie otwarte okna, a następnie wylogować.

- **Ostatni krok we wdrożeniu** jest chcemy upewnić się, że CONTOSO\\menedżerów określa można wdrożyć i utworzyć szablony i skonfigurować system bez schematu oraz Administratorzy domeny. Skrypt dalej będzie listy ACL uprawnienia w szablonach certyfikatów przy użyciu dsacls. Uruchom za pomocą konta, które ma pełne uprawnienia do zmiany zabezpieczeń uprawnienia odczytu i zapisu do każdego istniejącego szablonu certyfikatu w lesie.

- Pierwsze kroki: **konfigurowania punktu połączenia usługi i grupy docelowej uprawnienia & delegowanie zarządzania szablonu profilu**
  - Skonfiguruj uprawnienia w punkcie połączenia usługi (SCP).

  - Konfigurowanie zarządzania szablonu delegowanego profilem.

  - Skonfiguruj uprawnienia w punkcie połączenia usługi (SCP). **\<Brak skryptów\>**

        -   Upewnij się, że masz połączenie **CORPDC** serwera wirtualnego.

        -   Zaloguj się jako **contoso\\corpadmin**

        -   Z **narzędzia administracyjne**, otwórz **użytkownicy usługi Active Directory i komputery**.

        -   W **użytkownicy usługi Active Directory i komputery**na **widoku** menu, upewnij się, że **funkcje zaawansowane** jest włączona.

        -   W drzewie konsoli rozwiń węzeł **Contoso.com** \| **systemu** \| **Microsoft** \| **cykl życia certyfikatu Menedżer**, a następnie kliknij przycisk **CORPCM**.

        -   Kliknij prawym przyciskiem myszy **CORPCM**, a następnie kliknij przycisk **właściwości**.

        -   W **właściwości CORPCM** na okna dialogowego **zabezpieczeń** Dodaj następujące grupy przy użyciu odpowiednich uprawnień:

    | Grupa          | Uprawnienia                                                                                                                                                         |
    |----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Określa menedżerów | Odczyt </br> Inspekcja zarządzania certyfikatami w usłudze FIM</br> Agenta rejestracji programu FIM CM</br> Rejestrowanie programu FIM CM żądania</br> Odzyskaj żądania zarządzania certyfikatami w usłudze FIM</br> Odnów FIM CM żądania</br> Odwołaj żądanie zarządzania certyfikatami w usłudze FIM </br> FIM CM żądania odblokowania karty inteligentnej |
    | Pomoc techniczna określa | Odczyt</br> Agenta rejestracji programu FIM CM</br> Odwołaj żądanie zarządzania certyfikatami w usłudze FIM</br> FIM CM żądania odblokowania karty inteligentnej                                                                                |
- W **właściwości CORPDC** okno dialogowe, kliknij przycisk **OK**.

- Pozostaw **użytkownicy usługi Active Directory i komputery** otworzyć.

- **Konfigurowanie uprawnień na obiekty podrzędne użytkownika**
    - Upewnij się, że jesteś nadal w **użytkownicy usługi Active Directory i komputery** konsoli.
    - W drzewie konsoli kliknij prawym przyciskiem myszy **Contoso.com**, a następnie kliknij przycisk **właściwości**.
    - Na **zabezpieczeń** kliknij pozycję **Zaawansowane**.
    - W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.
    - W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **określa menedżerów**, a następnie kliknij przycisk **OK**.
    - W **wpis uprawnienia dla Contoso** okna dialogowego, **dotyczą** listy, wybierz **obiekty użytkownika podrzędnym** , a następnie włączyć **Zezwalaj**pole wyboru dla następujących **uprawnienia**:
        - **Odczyt wszystkich właściwości**
        - **Uprawnienia do odczytu**
        - **Inspekcja zarządzania certyfikatami w usłudze FIM**
        - **Agenta rejestracji programu FIM CM**
        - **Rejestrowanie programu FIM CM żądania**
        - **Odzyskaj żądania zarządzania certyfikatami w usłudze FIM**
        - **Odnów FIM CM żądania**
        - **Odwołaj żądanie zarządzania certyfikatami w usłudze FIM**
        - **FIM CM żądania odblokowania karty inteligentnej**
    - W **wpis uprawnienia dla Contoso** okno dialogowe, kliknij przycisk **OK**.
    - W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.
    - W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **pomoc techniczna Określa**, a następnie kliknij przycisk **OK**.
    - W **wpis uprawnienia dla Contoso** okna dialogowego, **dotyczą** listy, wybierz **obiekty użytkownika podrzędnym** , a następnie wybierz **Zezwalaj**pole wyboru dla następujących **uprawnienia**: - **odczytywanie wszystkich właściwości** - **uprawnienia do odczytu** - **Agenta rejestracji programu FIM CM** - **FIM CM żądania Revoke** - **FIM CM żądania odblokowania karty inteligentnej**
    - W **wpis uprawnienia dla Contoso** okno dialogowe, kliknij przycisk **OK**.
    — W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **OK**.
    - W **contoso.com właściwości** okno dialogowe, kliknij przycisk **OK**.
    - Pozostaw **użytkownicy usługi Active Directory i komputery** otworzyć.

    - **Konfigurowanie uprawnień na obiekty podrzędne użytkownika \<skryptu\>**
        - Upewnij się, że jesteś nadal w **użytkownicy usługi Active Directory i komputery** konsoli.
        - W drzewie konsoli kliknij prawym przyciskiem myszy **Contoso.com**, a następnie kliknij przycisk **właściwości**.
        - Na **zabezpieczeń** kliknij pozycję **Zaawansowane**.
        - W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.
        - W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **określa menedżerów**, a następnie kliknij przycisk **OK**.
        - W **wpis uprawnienia dla CONTOSO** okna dialogowego, **dotyczą** listy, wybierz **obiekty użytkownika podrzędnym** , a następnie włączyć **Zezwalaj**pole wyboru dla następujących **uprawnienia**:
            - **Odczyt wszystkich właściwości**
            - **Uprawnienia do odczytu**
            - **Inspekcja zarządzania certyfikatami w usłudze FIM**
            - **Agenta rejestracji programu FIM CM**
            - **Rejestrowanie programu FIM CM żądania**
            - **Odzyskaj żądania zarządzania certyfikatami w usłudze FIM**
            - **Odnów FIM CM żądania**
            - **Odwołaj żądanie zarządzania certyfikatami w usłudze FIM**
            - **FIM CM żądania odblokowania karty inteligentnej**
    - W **wpis uprawnienia dla CONTOSO** okno dialogowe, kliknij przycisk **OK**.
    - W **Zaawansowane ustawienia zabezpieczeń dla CONTOSO** okno dialogowe, kliknij przycisk **Dodaj**.
    - W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **pomoc techniczna Określa**, a następnie kliknij przycisk **OK**.
    - W **wpis uprawnienia dla CONTOSO** okna dialogowego, **dotyczą** listy, wybierz **obiekty użytkownika podrzędnym** , a następnie wybierz **Zezwalaj**pole wyboru dla następujących **uprawnienia**: - **odczytywanie wszystkich właściwości** - **uprawnienia do odczytu** - **Agenta rejestracji programu FIM CM** - **FIM CM żądania Revoke** - **FIM CM żądania odblokowania karty inteligentnej**
    - W **wpis uprawnienia dla contoso** okno dialogowe, kliknij przycisk **OK**.
    - W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **OK**.
    - W **contoso.com właściwości** okno dialogowe, kliknij przycisk **OK**.
    - Pozostaw **użytkownicy usługi Active Directory i komputery** otworzyć.
- Czynności drugiego: **uprawnień zarządzania szablonów certyfikatów delegowanie \<skryptu\>**
    - Delegowanie uprawnień w kontenerze szablonów certyfikatów.
    - Delegowanie uprawnień w kontenerze OID.
    - Delegowanie uprawnień do istniejących szablonów certyfikatów.
- Definiowanie uprawnień w kontenerze szablonów certyfikatów
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

- Definiowanie uprawnień w kontenerze OID
     1. W drzewie konsoli kliknij prawym przyciskiem myszy **OID**, a następnie kliknij przycisk **właściwości**.
     2. W **właściwości OID** na okna dialogowego **zabezpieczeń** , kliknij pozycję **zaawansowane**.
     3. W **Zaawansowane ustawienia zabezpieczeń dla identyfikatora OID** okno dialogowe, kliknij przycisk **Dodaj**.
     4. W **wybierz użytkownika, komputera, konto usługi lub grupy** okna dialogowego, **wprowadź nazwę obiektu do wybrania** wpisz **określa menedżerów**, a następnie kliknij przycisk **OK**.
     5. W **wpis uprawnienia dla identyfikatora OID** okno dialogowe pola, upewnij się, że uprawnienia dotyczą **ten obiekt i wszystkie obiekty zależne**, kliknij przycisk **Pełna kontrola**, a następnie kliknij przycisk  **OK**.
     6. W **Zaawansowane ustawienia zabezpieczeń dla identyfikatora OID** okno dialogowe, kliknij przycisk **OK**.
     7. W **właściwości OID** okno dialogowe, kliknij przycisk **OK**.
     8. Zamknij **Lokacje i usługi Active Directory**.

**Skrypty: Uprawnienia do kontenera identyfikator OID, szablon profilu i szablonów certyfikatów**

![](media/mim-cm-deploy/image021.png)

"" import-module activedirectory $adace = @{"OID" = "AD:\\CN identyfikator OID, CN = = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com"; "Wybierz" = "AD:\\CN = szablonów certyfikatów, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com"; "PT" = "AD:\\CN = szablonami profilów, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com"} $adace. GetEnumerator() | **Foreach-Object** {$acl = **listy Acl Get** *-ścieżka* $_. Wartość $sid = (**Get-ADGroup** "Określa menedżerowie"). Identyfikator SID $p = **nowy obiekt** System.Security.Principal.SecurityIdentifier($sid)
##<a name="httpsmsdnmicrosoftcomen-uslibrarysystemdirectoryservicesactivedirectorysecurityinheritancevvs110aspx"></a>https://msdn.microsoft.com/en-us/library/system.DirectoryServices.activedirectorysecurityinheritance (v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule ($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow, [ DirectoryServices.ActiveDirectorySecurityInheritance]::All) $acl. AddAccessRule($ace) **listy Acl zestaw** *-ścieżka* $_. Wartość *- AclObject* $acl}
```

**Scripts: Delegating permissions on the existing certificate templates.**

![](media/mim-cm-deploy/image039.png)

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
