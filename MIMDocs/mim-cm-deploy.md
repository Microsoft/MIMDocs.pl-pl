---
title: Wdrażanie Menedżera certyfikatów programu Microsoft Identity Manager | Dokumentacja firmy Microsoft
description: Zainstaluj Menedżera certyfikatów programu Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 9a9e00f7dca118627a5140967a104d13273cbc26
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690801"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Wdrażanie Menedżera certyfikatów programu Microsoft Identity Manager 2016 (MIM CM)

Instalacja Menedżera certyfikatów programu Microsoft Identity Manager 2016 (MIM CM) obejmuje kilka kroków. Aby uprościć proces możemy są potężne rzeczy. Brak wstępne kroki, które należy podjąć przed wszystkie rzeczywiste kroki MIM CM. Bez konieczności pracy wstępnej instalacji prawdopodobnie się nie powiedzie się.

Na poniższym diagramie przedstawiono przykład typ środowiska, która może być używana. Systemy z międzynarodowymi numerami identyfikującymi są uwzględnione w wymienionych poniżej diagramu i są wymagane do pomyślnego ukończenia kroki omówione w tym artykule. Na koniec są używane serwery centrów danych systemu Windows 2016:

![Diagram środowiska](media/mim-cm-deploy/image001.png)

1. CORPDC – Domain Controller
2. CORPCM — serwer zarządzania Certyfikatami programu MIM
3. CORPCA — urząd certyfikacji
4. CORPCMR — sieci Web interfejsu API Rest zarządzania Certyfikatami programu MIM — CM Portal dla interfejsu API Rest — używane do późniejszego
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10 Domain Joined

## <a name="deployment-overview"></a>Przegląd wdrożenia

- Instalacja podstawowego systemu operacyjnego

    Laboratorium składa się serwerów z systemem windows 2016 Datacenter.

    >[!NOTE]
    >Aby uzyskać więcej informacji na platformach obsługiwanych przez program MIM 2016, zapoznaj się z artykuł [platformy obsługiwane przez program MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. Kroki przed wdrożeniem

    - [Rozszerzanie schematu](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Tworzenie konta usług

    - [Tworzenie szablonów certyfikatów](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Konfigurowanie protokołu Kerberos

    - Kroki związane z bazy danych

        - Wymagania dotyczące konfiguracji programu SQL

        - Uprawnienia bazy danych

2. Wdrożenie

## <a name="pre-deployment-steps"></a>Kroki przed wdrożeniem

Kreator konfiguracji programu MIM CM wymaga podania informacji o po drodze w kolejności na jego zakończenie pomyślnie.

![diagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Rozszerzanie schematu

Proces rozszerzania schematu jest prosty, ale musi być skontaktowali się ostrożnie ze względu na charakter nieodwracalne.

>[!NOTE]
>Ten krok wymaga, że używane konto ma uprawnienia administratora schematu.

1. Przejdź do lokalizacji nośnika programu MIM i przejdź do \\zarządzania certyfikatami\\x64 folderu.

2. Skopiuj folder schematu do kontrolera domeny CORPDC, a następnie przejdź do niego.

    ![diagram](media/mim-cm-deploy/image005.png)

3. Uruchom skrypt resourceForestModifySchema.vbs pojedynczego lasu scenariusza. W scenariuszu lasu zasobów, uruchamianie skryptów:
   - DomenaA — użytkownicy znajdujący się (userForestModifySchema.vbs)
   - ResourceForestB — lokalizacja instalacji programu CM (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Zmiany schematu są jednym ze sposobów operacji i wymagają lasu odzyskiwania, aby wycofać więc upewnij się, niezbędne kopie zapasowe. Dla informacji na temat zmian wprowadzonych w schemacie przez wykonanie tej operacji zapoznaj się z artykułem [zmiany schematu programu Forefront Identity Manager 2010 certyfikatu zarządzania](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![diagram](media/mim-cm-deploy/image007.png)

4. Uruchom skrypt i powinien zostać wyświetlony sukcesu raz komunikat, że ukończeniu działania skryptu.

    ![Komunikat o powodzeniu](media/mim-cm-deploy/image009.png)

Teraz rozszerzania schematu w AD do obsługi zarządzania Certyfikatami programu MIM.

### <a name="creating-service-accounts-and-groups"></a>Tworzenie konta usługi i grupy

Poniższa tabela zawiera podsumowanie konta i uprawnienia wymagane przez program MIM CM. Możesz zezwolić MIM CM automatycznie Utwórz następujące konta lub można je utworzyć przed instalacją. Można zmienić nazwy konta rzeczywistego. Jeśli tworzysz kont samodzielnie, należy wziąć pod uwagę nazewnictwa kont użytkowników w takich natychmiast jest łatwo jest zgodna z nazwą konta użytkownika do jego funkcji.

Użytkownicy:

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Rola**                   | **Nazwa logowania użytkownika** |
|----------------------------|---------------------|
| Agent Menedżera konfiguracji programu MIM               | MIMCMAgent          |
| Agent odzyskiwania kluczy zarządzania Certyfikatami programu MIM  | MIMCMKRAgent        |
| Program MIM CM autoryzacji agenta | MIMCMAuthAgent      |
| Agent Menedżera urzędu certyfikacji zarządzania Certyfikatami programu MIM    | MIMCMManagerAgent   |
| Agent puli sieci Web zarządzania Certyfikatami programu MIM      | MIMCMWebAgent       |
| Agent rejestrowania zarządzania Certyfikatami programu MIM    | MIMCMEnrollAgent    |
| Usługa aktualizacji Menedżer certyfikatów programu MIM      | MIMCMService        |
| Konto instalacji programu MIM        | MIMINSTALL          |
| Pomoc techniczna agenta            | CMHelpdesk1-2       |
| Menedżer CM                 | CMManager1-2        |
| Użytkownik subskrybenta            | CMUser1-2           |

Grupy:

| **Rola**               | **Grupa**         |
|------------------------|-------------------|
| Elementy członkowskie CM działu pomocy technicznej    | Pomoc techniczna Menedżera    |
| Elementy członkowskie Menedżera CM     | Menedżerowie Menedżera    |
| Członkowie-subskrybenci CM | Subskrybenci Menedżera |

Program PowerShell: Konta agenta:

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

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Aktualizacja **CORPCM** serwera lokalne zasady dotyczące kont agentów 

| **Nazwa logowania użytkownika** | **Opis i uprawnienia**   |
|------|---------------------|
| MIMCMAgent          | Udostępnia następujące usługi: </br>-Pobiera zaszyfrowane klucze prywatne z urzędu certyfikacji. </br>— Zapewnia ochronę informacji o numerach PIN karty inteligentnej w bazie danych programu FIM CM. </br>-Zabezpiecza komunikację między CM usługi FIM i urzędu certyfikacji. </br></br> To konto użytkownika wymaga następujących ustawień kontroli dostępu:</br>-   **Zezwalaj na logowanie lokalne** prawa użytkownika.</br>-   **Wystawianie i zarządzanie certyfikatami** prawa użytkownika. </br>— Odczytywanie i zapisywanie uprawnienia do folderu tymczasowego systemu w następującej lokalizacji: % WINDIR %\\Temp.</br>-Cyfrowy podpis i szyfrowanie certyfikat wydany i zainstalowany w magazynie użytkownika.
|MIMCMKRAgent        | Odzyskuje archiwizacji kluczy prywatnych z urzędu certyfikacji. To konto użytkownika wymaga następujących ustawień kontroli dostępu:</br> -   **Zezwalaj na logowanie lokalne** prawa użytkownika.</br>-Członkostwo w lokalnej **Administratorzy** grupy. </br>— Uprawnienia do rejestracji na **KeyRecoveryAgent** szablonu certyfikatu. </br>— Certyfikat agenta odzyskiwania kluczy jest wydany i zainstalowany w magazynie użytkownika. Certyfikat należy dodać do listy agentów odzyskiwania kluczy urzędu certyfikacji. </br>— Uprawnienia do odczytu i zapisu uprawnienia do folderu tymczasowego systemu w następującej lokalizacji: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Określa prawa i uprawnienia dla użytkowników i grup. To konto użytkownika wymaga następujących ustawień kontroli dostępu: </br>-Członkostwo w grupie domeny dostęp zgodny z systemami starszymi niż Windows 2000. </br> -Przyznanych **Generuj inspekcje zabezpieczeń** prawa użytkownika.             |
| MIMCMManagerAgent   | Wykonuje działania związane z zarządzaniem urzędu certyfikacji. </br> Ten użytkownik musi posiadać uprawnienie do zarządzania urzędu certyfikacji.        |
| MIMCMWebAgent       | Zawiera tożsamość puli aplikacji usług IIS. Zarządzania certyfikatami w usłudze FIM jest uruchamiany w ramach Microsoft Win32® aplikacji programowania interfejsu procesu, który używa poświadczeń tego użytkownika. </br> To konto użytkownika wymaga następujących ustawień kontroli dostępu:</br> -Członkostwo w lokalnej **IIS_WPG, system windows 2016 = IIS_IUSRS** grupy. </br>-Członkostwo w lokalnej **Administratorzy** grupy.</br>-Przyznanych **Generuj inspekcje zabezpieczeń** prawa użytkownika. </br>-Przyznanych **działanie jako część systemu operacyjnego** prawa użytkownika. </br>-Przyznanych **Zamień token na poziomie procesu** prawa użytkownika.</br>-Przypisane jako tożsamość puli aplikacji usług IIS, **CLMAppPool**. </br>-Przyznane uprawnienia do odczytu **HKEY_LOCAL_MACHINE\\oprogramowania\\Microsoft\\CLM\\v1.0\\serwera\\WebUser** klucza rejestru. </br>— To konto musi być także zaufany dla celów delegacji.|
| MIMCMEnrollAgent    | Wykonuje rejestrowanie w imieniu użytkownika. To konto użytkownika wymaga następujących ustawień kontroli dostępu:</br>-Certyfikat agenta rejestracji, który jest wydany i zainstalowany w magazynie użytkownika.</br>-   **Zezwalaj na logowanie lokalne** prawa użytkownika. </br>— Uprawnienia do rejestracji na **agenta rejestracji** szablonu certyfikatu (lub szablonu niestandardowego, jeśli jest on używany).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Tworzenie szablonów certyfikatów dla konta usługi MIM CM

Trzy z konta usługi używane przez Menedżera certyfikatów programu MIM jest wymagany certyfikat, a Kreator konfiguracji wymaga podania nazwy szablonów certyfikatów, które należy używać do żądania certyfikatów dla nich.

Konta usług, które wymagają certyfikatów są:

- MIMCMAgent: To konto musi mieć certyfikat użytkownika

- MIMCMEnrollAgent: To konto musi mieć certyfikat agenta rejestrowania

- MIMCMKRAgent: To konto musi mieć **agenta odzyskiwania kluczy** certyfikatu

Istnieją już w AD szablony, ale musimy utworzyć własną wersji do pracy z programu MIM CM. Jak należy wprowadzić modyfikacji przy użyciu oryginalnego szablonów linii bazowej.

Wszystkie trzy powyższe kont będzie kontem z podniesionymi uprawnieniami w Twojej organizacji i powinno zostać obsłużone ostrożnie.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Tworzenie szablonu certyfikatu podpisywania programu MIM CM

1. Z **narzędzia administracyjne**, otwórz **urzędu certyfikacji**.

2. W **urzędu certyfikacji** konsoli w drzewie konsoli rozwiń **Contoso CorpCA**, a następnie kliknij przycisk **szablonów certyfikatów**.

3. Kliknij prawym przyciskiem myszy **szablonów certyfikatów**, a następnie kliknij przycisk **Zarządzaj**.

4. W **konsolę Szablony certyfikatów**w **szczegóły** okienku, wybierz i kliknij prawym przyciskiem myszy **użytkownika**, a następnie kliknij przycisk **Duplikuj szablon** .

5. W **Duplikuj szablon** okno dialogowe, wybierz opcję **systemu Windows Server 2003 Enterprise**, a następnie kliknij przycisk **OK**.

    ![Pokaż wynikowe zmiany](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >Menedżer certyfikatów programu MIM nie działa z certyfikatów opartych na wersji 3 szablonów certyfikatów. Należy utworzyć szablon certyfikatu systemu Windows Server® 2003, Enterprise (wersja 2). Zobacz [szczegóły V3](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) Aby uzyskać więcej informacji.

6. W **właściwości nowego szablonu** dialogowym **ogólne** na karcie **nazwę wyświetlaną szablonu** wpisz **podpisywania programu MIM CM**. Zmiana **okres ważności** do **2 lata**, a następnie wyczyść **Publikuj certyfikat w usłudze Active Directory** pole wyboru.

7. Na **obsługiwanie żądań** kartę, upewnij się, że **Zezwalaj na eksportowanie klucza prywatnego** pole wyboru jest zaznaczone, a następnie kliknij przycisk **karta Kryptografia**.

8. W **wybór kryptografii** okno dialogowe, wyłącz **Microsoft Enhanced Cryptographic Provider 1.0**, Włącz **Microsoft Enhanced RSA and AES Cryptographic Provider**, a następnie kliknij przycisk **OK**.

9. Na **nazwy podmiotu** kartę, usuń zaznaczenie **Dołącz nazwę e-mail do nazwy podmiotu** i **nazwa E-mail** pola wyboru.

10. Na **rozszerzenia** na karcie **rozszerzenia zawarte w tym szablonie** listy, upewnij się, że **zasady aplikacji** jest zaznaczone, a następnie kliknij przycisk **edycji** .

11. W **Edytowanie rozszerzenia zasad aplikacji** okno dialogowe, wybierz **systemu szyfrowania plików** i **bezpieczna poczta E-mail** zasady aplikacji. Kliknij przycisk **Usuń**, a następnie kliknij przycisk **OK**.

12. Na **zabezpieczeń** karcie należy wykonać następujące czynności:

    - Usuń **administratora**.

    - Usuń **Administratorzy domeny**.

    - Usuń **użytkownicy domeny**.

    - Przypisz tylko **odczytu** i **zapisu** uprawnień do **Administratorzy przedsiębiorstwa**.

    - Dodaj **MIMCMAgent.**

    - Przypisz **odczytu** i **Zarejestruj** uprawnień do **MIMCMAgent**.

13. W **właściwości nowego szablonu** okno dialogowe, kliknij przycisk **OK**.

14. Pozostaw **konsolę Szablony certyfikatów** Otwórz.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Tworzenie szablonu certyfikatu agenta rejestracji menedżera konfiguracji programu MIM

1. W **konsolę Szablony certyfikatów**w **szczegóły** okienku, wybierz i kliknij prawym przyciskiem myszy **agenta rejestracji**, a następnie kliknij przycisk **Duplikuj szablon**.

2. W **Duplikuj szablon** okno dialogowe, wybierz opcję **systemu Windows Server 2003 Enterprise**, a następnie kliknij przycisk **OK**.

3. W **właściwości nowego szablonu** dialogowym **ogólne** na karcie **nazwę wyświetlaną szablonu** wpisz **agenta rejestracji menedżera konfiguracji programu MIM**. Upewnij się, że **okres ważności** jest **2 lata**.

4. Na **obsługiwanie żądań** kartę, należy włączyć **Zezwalaj na eksportowanie klucza prywatnego**, a następnie kliknij przycisk **dostawców usług kryptograficznych lub karta kryptografii.**

5. W **Wybieranie dostawcy CSP** okno dialogowe, wyłącz **Microsoft Base Cryptographic Provider 1.0**, wyłącz **Microsoft Enhanced Cryptographic Provider 1.0**, Włącz  **Microsoft Enhanced RSA and AES Cryptographic Provider**, a następnie kliknij przycisk **OK**.

6. Na **zabezpieczeń** karcie, wykonaj następujące czynności:

    - Usuń **administratora**.

    - Usuń **Administratorzy domeny**.

    - Przypisz tylko **odczytu** i **zapisu** uprawnień do **Administratorzy przedsiębiorstwa**.

    - Dodaj **MIMCMEnrollAgent**.

    - Przypisz **odczytu** i **Zarejestruj** uprawnień do **MIMCMEnrollAgent**.

7. W **właściwości nowego szablonu** okno dialogowe, kliknij przycisk **OK**.

8. Pozostaw **konsolę Szablony certyfikatów** Otwórz.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Tworzenie szablonu certyfikatu agenta odzyskiwania kluczy zarządzania Certyfikatami programu MIM

1. W **szablonów certyfikatów** konsoli programu **szczegóły** okienku, wybierz i kliknij prawym przyciskiem myszy **agenta odzyskiwania kluczy**, a następnie kliknij przycisk **Duplikuj szablon**.

2. W **Duplikuj szablon** okno dialogowe, wybierz opcję **systemu Windows Server 2003 Enterprise**, a następnie kliknij przycisk **OK**.

3. W **właściwości nowego szablonu** dialogowym **ogólne** na karcie **nazwę wyświetlaną szablonu** wpisz **agenta odzyskiwania kluczy MIM CM**. Upewnij się, że **okres ważności** jest **2 lata** na **karta kryptografii.**

4. W **wyboru dostawcy** okno dialogowe, wyłącz **Microsoft Enhanced Cryptographic Provider 1.0**, Włącz **Microsoft Enhanced RSA and AES Cryptographic Provider**, a następnie kliknij przycisk **OK**.

5. Na **wymagania wystawiania** kartę, upewnij się, że **Zgoda menedżera certyfikatów urzędu certyfikacji** jest **wyłączone**.

6. Na **zabezpieczeń** karcie, wykonaj następujące czynności:

    - Usuń **administratora**.

    - Usuń **Administratorzy domeny**.

    - Przypisz tylko **odczytu** i **zapisu** uprawnień do **Administratorzy przedsiębiorstwa**.

    - Dodaj **MIMCMKRAgent**.

    - Przypisz **odczytu** i **Zarejestruj** uprawnień do **KRAgent**.

7. W **właściwości nowego szablonu** okno dialogowe, kliknij przycisk **OK**.

8. Zamknij okno **Konsola szablonów certyfikatów**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publikowanie szablonów wymaganego certyfikatu w urzędzie certyfikacji

1. Przywróć **urzędu certyfikacji** konsoli.

2. W **urzędu certyfikacji** konsoli w drzewie konsoli kliknij prawym przyciskiem myszy **szablonów certyfikatów**, wskaż polecenie **New**, a następnie kliknij przycisk **certyfikatu Szablon do wystawienia**.

3. W **Włączanie szablonu certyfikatu** okno dialogowe, wybierz opcję **agenta rejestracji menedżera konfiguracji programu MIM**, **agenta odzyskiwania kluczy zarządzania Certyfikatami programu MIM**, i **MIM CM podpisywania**. Kliknij przycisk **OK**.

4. W drzewie konsoli kliknij **szablonów certyfikatów**.

5. Sprawdź, czy trzy nowe szablony są wyświetlane w **szczegóły** okienka, a następnie Zamknij **urzędu certyfikacji**.

    ![Podpisywanie zarządzania Certyfikatami programu MIM](media/mim-cm-deploy/image016.png)

6. Zamknij wszystkie otwarte okna i Wyloguj.

### <a name="iis-configuration"></a>Konfiguracja programu IIS

W celu hostowania witryny sieci Web do zarządzania certyfikatami w usłudze, należy zainstalować i skonfigurować usługi IIS.

#### <a name="install-and-configure-iis"></a>Instalowanie i konfigurowanie usług IIS

1. Zaloguj się do **CORLog w** jako **MIMINSTALL** konta

    >[!IMPORTANT]
    >Konto instalacji programu MIM należy mieć uprawnienia administratora lokalnego

2. Otwórz program PowerShell i uruchom następujące polecenie

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Witryny o nazwie domyślnej witryny sieci Web jest zainstalowana domyślnie w usługach IIS 7. Jeśli danej lokacji została zmieniona lub usunięta witryną o nazwie domyślnej witryny sieci Web musi być dostępny przed zainstalowaniem programu MIM CM.

#### <a name="configuring-kerberos"></a>Konfigurowanie protokołu Kerberos

Konto MIMCMWebAgent będą uruchomione w portalu MIM CM. Domyślnie w usługach IIS oraz jego nowszymi wersjami jądra tryb uwierzytelniania jest używany w usługach IIS domyślnie. Będzie Wyłącz uwierzytelnianie trybu jądra protokołu Kerberos i skonfigurować nazwy SPN na koncie MIMCMWebAgent zamiast tego. Niektóre polecenia będzie wymagać podwyższonym poziomem uprawnień w usłudze active directory i CORPCM server.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Aktualizowanie usługi IIS na CORPCM**

![diagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Musisz dodać DNS rekord A "cm.contoso.com", a następnie wskaż CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Wymaganie protokołu SSL w portalu MIM CM

Zdecydowanie zaleca się wymagać protokołu SSL w portalu MIM CM. Jeśli jeszcze nie zostanie Kreator ostrzeżenie o nim.

1. Zarejestruj się w sieci web certyfikatu dla **cm.contoso.com** przypisać do domyślnej witryny

2. Otwórz **Menedżera usług IIS** i przejdź do **zarządzania certyfikatami**

3. W widoku funkcji kliknij dwukrotnie pozycję Ustawienia protokołu SSL.

4. Na stronie Ustawienia protokołu SSL, zaznacz **Wymagaj protokołu SSL**.

5. W okienku Akcje kliknij polecenie **Zastosuj.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Konfiguracja bazy danych **CORPSQL** dla zarządzania Certyfikatami programu MIM

1. Upewnij się, czy masz połączenie z serwerem CORPSQL01.

2. Upewnij się, że użytkownik jest zalogowany jako administrator programu SQL.

3. Uruchom poniższy skrypt języka T-SQL, aby umożliwić firmie CONTOSO\\MIMINSTALL konto, aby utworzyć bazę danych, gdy firma Microsoft, przejdź do kroku konfiguracji

    >[!NOTE]
    >Firma Microsoft będzie należy możesz wrócić do bazy danych SQL, gdy firma Microsoft będzie gotowa do zakończenia i zasady dotyczące modułu

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Komunikat o błędzie Kreatora konfiguracji programu MIM CM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Wdrożenie zarządzania certyfikatami programu Microsoft Identity Manager 2016

1. Upewnij się, czy masz połączenie z serwerem CORPCM i który **MIMINSTALL** konto jest członkiem **Administratorzy lokalni** grupy.

2. Upewnij się, użytkownik jest zalogowany jako Contoso\\MIMINSTALL.

3. Zainstaluj plik ISO z dodatku SP1 dla programu Microsoft Identity Manager.

4. **Otwórz** **zarządzania certyfikatami\\x64** katalogu.

5. W **x64** okna, kliknij prawym przyciskiem myszy **instalacji**, a następnie kliknij przycisk **Uruchom jako administrator**.

6. Na stronie Kreatora instalacji zarządzania certyfikatów Microsoft Identity Manager-Zapraszamy kliknij **dalej.**

7. Na stronie postanowienia licencyjne przeczytaj umowę, Włącz akceptuję warunki umowy licencyjnej **pole wyboru**, a następnie kliknij przycisk Dalej.

8. Upewnij się, na stronie Instalacja niestandardowa **portalu programu MIM CM** i **składniki programu MIM CM aktualizacji usługi** jest ustawiona do zainstalowania, a następnie **kliknij przycisk Dalej,** .

9. Na stronie Folder wirtualny sieci Web, upewnij się, że nazwa folderu wirtualnej **CertificateManagement**, a następnie **kliknij przycisk Dalej,** .

10. Na stronie Instalowanie zarządzania certyfikatami Menedżer tożsamości firmy Microsoft **kliknij przycisk Instaluj**.

11. Na **Ukończono** stronie Kreatora instalacji zarządzania certyfikatów Microsoft Identity Manager **kliknij przycisk Zakończ**.

![Ukończono działanie kreatora MIM CM](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Kreator konfiguracji programu Microsoft Identity Manager 2016 certyfikatu zarządzania

Przed zalogowaniem się do CORPCM Dodaj MIMINSTALL do **domeny Administratorzy, Administratorzy schematu i Administratorzy lokalni** dla Kreatora konfiguracji. Może to zostać usunięte później po zakończeniu konfiguracji.

![Komunikat o błędzie](media/mim-cm-deploy/image028.png)

1. Z **Start** menu, kliknij przycisk **Kreator konfiguracji usług certyfikatów zarządzania**. I uruchomić **administratora**

2. Na **Witamy w Kreatorze konfiguracji** kliknij **dalej**.

3. Na **konfiguracji urzędu certyfikacji** strony, upewnij się, że wybrany urząd certyfikacji jest **Contoso-CORPCA-CA**, upewnij się, że wybrany serwer **CORPCA. CONTOSO.COM**, a następnie kliknij przycisk **dalej**.

4. Na **Konfigurowanie bazy danych programu Microsoft® SQL Server®** stronie **nazwa programu SQL Server** wpisz **CORPSQL1** , Włącz **umożliwia utworzenie moje poświadczenia Baza danych** pole wyboru, a następnie kliknij przycisk **dalej**.

5. Na **ustawienia bazy danych** strona, zaakceptuj domyślną nazwę bazy danych **FIMCertificateManagement**, upewnij się, że **zintegrowane uwierzytelnianie SQL** jest zaznaczone, a następnie Kliknij przycisk **dalej**.

6. Na **Skonfiguruj usługi Active Directory** strony, zaakceptuj nazwę domyślną, podany dla punktu połączenia usługi, a następnie kliknij przycisk **dalej**.

7. Na **metodę uwierzytelniania** strony, potwierdź **uwierzytelnianie zintegrowane systemu windows** jest zaznaczone, a następnie kliknij przycisk **dalej**.

8. Na **agentów — programu FIM CM** strony, wyczyść **Użyj ustawień domyślnych programu FIM CM** pole wyboru, a następnie kliknij przycisk **kont niestandardowe**.

9. W **agentów — programu FIM CM** okno dialogowe z wieloma kartami, na każdej karcie wpisz następujące informacje:

   - Nazwa użytkownika: **Aktualizacja**

   - Hasło: **Przekaż\@word1**

   - Potwierdź hasło: **Przekaż\@word1**

   - Użyj istniejącego użytkownika: **Włączone**

     >[!NOTE]
     >Te konta utworzony wcześniej. Upewnij się, że procedur opisanych w kroku 8 są powtarzane dla wszystkich kart konta sześć agenta.

     ![Konta programu MIM CM](media/mim-cm-deploy/image030.png)

10. Po zakończeniu wszystkich informacji o koncie agenta, kliknij przycisk **OK**.

11. Na **agentów — MIM CM** kliknij **dalej**.

12. Na **Konfigurowanie certyfikatów serwera** strony, należy włączyć następujące szablony certyfikatów:

    - Szablon certyfikatu, który ma być używany dla certyfikatu agenta odzyskiwania kluczy agenta odzyskiwania: **MIMCMKeyRecoveryAgent**.

    - Szablon certyfikatu, który ma być używany dla certyfikatu agenta programu FIM CM: **MIMCMSigning**.

    - Szablon certyfikatu, który ma być używany dla certyfikatu agenta rejestracji: **FIMCMEnrollmentAgent**.

13. Na **certyfikaty serwera konfiguracji** kliknij **dalej**.

14. Na **ustawienia serwera poczty E-mail, drukowanie dokumentów** stronie **Określ nazwę serwera SMTP, którego chcesz użyć do rejestracji powiadomień e-mail** pole, a następnie kliknij przycisk **dalej.**

15. Na **wszystko gotowe do skonfigurowania** kliknij **Konfiguruj**.

16. W **Kreator konfiguracji — Microsoft Forefront Identity Manager 2010 R2** okno dialogowe ostrzeżenia kliknij **OK** można potwierdzić, że protokół SSL nie jest włączone w katalogu wirtualnym programu IIS.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Nie klikaj przycisk Zakończ, aż do zakończenia wykonywania Kreatora konfiguracji. Rejestrowanie dla kreatora można znaleźć tutaj: **%programfiles%\\Microsoft Forefront Identity Management\\2010\\zarządzania certyfikatami\\config.log**

17. Kliknij przycisk **Zakończ**.

    ![Ukończono działanie kreatora MIM CM](media/mim-cm-deploy/image033.png)

18. Zamknij wszystkie otwarte okna.

19. Dodaj `https://cm.contoso.com/certificatemanagement` do strefy Lokalny intranet w przeglądarce.

20. Odwiedź witrynę z serwera CORPCM `https://cm.contoso.com/certificatemanagement`  

    ![diagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Sprawdzić, czy Usługa izolacji klucza CNG

1. Z **narzędzia administracyjne**, otwórz **usług**.

2. W **szczegóły** okienku kliknij dwukrotnie **Izolacja klucza CNG**.

3. Na **ogólne** kartę, zmień **uruchamiana** do **automatyczne**.

4. Na **ogólne** kartę, uruchom usługę, jeśli nie jest w stanie uruchomionym.

5. Na **ogólne** kliknij pozycję **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalowanie i Konfigurowanie modułów urzędu certyfikacji:

W tym kroku będziemy instalowanych i konfigurowanych modułów programu FIM CM urzędu certyfikacji w urzędzie certyfikacji.

1. Konfigurowanie programu FIM CM, aby tylko sprawdzić uprawnienia użytkownika do operacji zarządzania

2. W **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\zarządzania certyfikatami\\web** okna, Utwórz kopię  **plik Web.config** nazewnictwa kopii **web.1.config**.

3. W **Web** okna, kliknij prawym przyciskiem myszy **Web.config**, a następnie kliknij przycisk **Otwórz**.

    >[!Note]
    >Plik Web.config jest otwarty w Notatniku

4. Po otwarciu pliku, naciśnij klawisze CTRL + F.

5. W **Znajdź i Zamień** dialogowym **Znajdź** wpisz **UseUser**, a następnie kliknij przycisk **Znajdź następny** trzy razy.

6. Zamknij **Znajdź i Zamień** okno dialogowe.

7. Należy w wierszu  **\<Dodaj key="Clm.RequestSecurity.Flags" wartość = "UseUser UseGroups" /\>** . Zmień wiersz do odczytania  **\<Dodaj key="Clm.RequestSecurity.Flags" wartość = "UseUser" /\>** .

8. Zamknij wszystkie zmiany zapisywania pliku.

9. Utwórz konto komputera urzędu certyfikacji na serwerze SQL \<bez skryptu\>

10. Upewnij się, że nawiązano **CORPSQL01** serwera.

11. Upewnij się, użytkownik jest zalogowany jako **DBA**

12. Z **Start** menu, uruchamiania **SQL Server Management Studio**.

13. W **Połącz z serwerem** dialogowym **nazwy serwera** wpisz **CORPSQL01,** a następnie kliknij przycisk **Connect**.

14. W drzewie konsoli rozwiń **zabezpieczeń**, a następnie kliknij przycisk **logowania**.

15. Kliknij prawym przyciskiem myszy **logowania**, a następnie kliknij przycisk **nowy identyfikator logowania**.

16. Na **ogólne** stronie **nazwa logowania** wpisz **contoso\\CORPCA\$** . Wybierz **uwierzytelniania Windows**. Domyślna baza danych jest **FIMCertificateManagement**.

17. W okienku po lewej stronie wybierz **mapowania użytkowników**. W okienku po prawej stronie, kliknij pole wyboru w **mapy** kolumnę obok **FIMCertificateManagement**. W **członkostwo roli dla bazy danych: FIMCertificateManagement** listy, należy włączyć **clmApp** roli.

18. Kliknij przycisk **OK**.

19. Zamknij **programu Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Zainstaluj moduły programu FIM CM urzędu certyfikacji w urzędzie certyfikacji

1. Upewnij się, że nawiązano **CORPCA** serwera.

2. W **X64** systemu windows, kliknij prawym przyciskiem myszy **Setup.exe**, a następnie kliknij przycisk **Uruchom jako administrator**.

3. Na **Kreatora instalacji zarządzania certyfikatów Microsoft Identity Manager — Zapraszamy** kliknij **dalej**.

4. Na **Umowa licencyjna użytkownika oprogramowania** strony, zapoznaj się z umową. Wybierz **akceptuję warunki umowy licencyjnej** pole wyboru, a następnie kliknij przycisk **dalej**.

5. Na **Instalacja niestandardowa** wybierz opcję **portalu programu MIM CM**, a następnie kliknij przycisk **tej funkcji nie będą dostępne**.

6. Na **Instalacja niestandardowa** wybierz opcję **usługi aktualizacji programu MIM CM**, a następnie kliknij przycisk **ta funkcja nie będzie dostępna**.

    >[!Note]
    >Spowoduje to pozostawienie plików urzędu certyfikacji zarządzania Certyfikatami programu MIM jako funkcja tylko jest włączona podczas instalacji.

7. Na **Instalacja niestandardowa** kliknij **dalej**.

8. Na **instalowanie zarządzania certyfikatami Menedżer tożsamości firmy Microsoft** kliknij **zainstalować**.

9. Na **programu Microsoft Identity Manager certyfikatu zarządzania Kreator instalacji zakończył pracę** kliknij **Zakończ**.

10. Zamknij wszystkie otwarte okna.

### <a name="configure-the-mim-cm-exit-module"></a>Skonfiguruj moduł zakończenia zarządzania Certyfikatami programu MIM

1. Z **narzędzia administracyjne**, otwórz **urzędu certyfikacji**.

2. W drzewie konsoli kliknij prawym przyciskiem myszy **contoso-CORPCA-CA**, a następnie kliknij przycisk **właściwości**.

3. Na **moduł zakończenia** zaznacz **moduł zakończenia programu FIM CM**, a następnie kliknij przycisk **właściwości**.

4. W **Określ parametry połączenia bazy danych CM** wpisz **Connect Timeout = 15; Utrwal informacje zabezpieczające = True; Integrated Security = sspi; Initial Catalog = FIMCertificateManagement; źródło danych = CORPSQL01**. Pozostaw **szyfrowania parametrów połączenia** włączone pole wyboru, a następnie kliknij przycisk **OK**.
5. W **zarządzania certyfikatami programu FIM Microsoft** , kliknij przycisk pola **OK**.

6. W **właściwości urzędu certyfikacji CORPCA contoso** okno dialogowe, kliknij przycisk **OK**.

7. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA** *,* wskaż **wszystkie zadania**, a następnie kliknij przycisk **Zatrzymaj usługę**. Zaczekaj, aż zatrzymuje usług certyfikatów Active Directory.

8. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA** *,* wskaż **wszystkie zadania**, a następnie kliknij przycisk **Uruchom usługę**.

9. Minimalizuj **urzędu certyfikacji** konsoli.

10. Z **narzędzia administracyjne**, otwórz **Podgląd zdarzeń**.

11. W drzewie konsoli rozwiń **Dzienniki aplikacji i usług**, a następnie kliknij przycisk **zarządzania certyfikatami w programie FIM**.

12. Na liście zdarzeń, sprawdź, czy najnowszych zdarzeń, wykonaj *nie* zawierać **ostrzeżenie** lub **błąd** zdarzenia od czasu ostatniego ponownego uruchomienia usług certyfikatów.

    >[!NOTE] 
    >Ostatnie zdarzenie powinien określać, że moduł zakończenia załadowany, użyciem ustawień z: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimalizuj **Podgląd zdarzeń**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka Windows®

1. Przywróć **urzędu certyfikacji** konsoli.

2. W drzewie konsoli rozwiń **contoso-CORPCA-CA**, a następnie kliknij przycisk **wystawione certyfikaty**.

3. W **szczegóły** okienku kliknij dwukrotnie certyfikat za pomocą **CONTOSO\\MIMCMAgent** w **Nazwa żądającego** kolumny i **programu FIM CM Podpisywanie** w **szablonu certyfikatu** kolumny.

4. Na **szczegóły** zaznacz **odcisk palca** pola.

5. Wybierz odcisk palca, a następnie naciśnij klawisze CTRL + C.

    >[!NOTE]
    >Czy **nie** zawierają spacje wiodące na liście znaków odcisku palca.

6. W **certyfikatu** okno dialogowe, kliknij przycisk **OK**.

7. Z **Start** menu w **Wyszukaj programy i pliki** wpisz **Notatnik**, a następnie naciśnij klawisz ENTER.

8. W **Notatnik**, z **Edytuj** menu, kliknij przycisk **Wklej**.

9. Z **Edytuj** menu, kliknij przycisk **Zastąp**.

10. W **Znajdź** wpisz znak spacji, a następnie kliknij przycisk **Zamień wszystkie**.

    >[!Note]
    >Spowoduje to usunięcie wszystkich odstępów między znakami w odcisku palca.

11. W **Zastąp** okno dialogowe, kliknij przycisk **anulować**.

12. Wybierz skonwertowany *thumbprintstring*, a następnie naciśnij klawisze CTRL + C.

13. Zamknij **Notatnik** bez zapisywania zmian.

### <a name="configure-the-fim-cm-policy-module"></a>Konfigurowanie modułu zasad zarządzania certyfikatami w usłudze FIM

1. Przywróć **urzędu certyfikacji** konsoli.

2. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA**, a następnie kliknij przycisk **właściwości**.

3. W **właściwości urzędu certyfikacji CORPCA contoso** dialogowym **modułu zasad** kliknij pozycję **właściwości**.

    - Na **ogólne** kartę, upewnij się, że **przekazywać żądania CM usługi FIM do domyślnego modułu zasad w celu przetwarzania** jest zaznaczone.

    - Na **certyfikaty podpisywania** kliknij pozycję **Dodaj**.

    - W oknie dialogowym certyfikat, kliknij prawym przyciskiem myszy **Określ skrót certyfikatu zakodowanego Szesnastkowo** , a następnie kliknij przycisk **Wklej**.

    - W **certyfikatu** okno dialogowe, kliknij przycisk **OK**.

        >[!Note]
        >Jeśli **OK** przycisk nie jest włączona, możesz przypadkowo włączone ukryty znak ciąg odcisku palca podczas kopiowania odcisk palca z certyfikatu clmAgent. Powtórz kroki wszystkie, począwszy od **zadanie 4: Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka Windows** w tym ćwiczeniu.

4. W **właściwości konfiguracji** okna dialogowego pole, upewnij się, że odcisk palca jest wyświetlana w **prawidłowe certyfikaty podpisywania** listy, a następnie kliknij przycisk **OK**.

5. W **zarządzania certyfikatami w programie FIM** , kliknij przycisk pola **OK**.

6. W **właściwości urzędu certyfikacji CORPCA contoso** okno dialogowe, kliknij przycisk **OK**.

7. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA** *,* wskaż **wszystkie zadania**, a następnie kliknij przycisk **Zatrzymaj usługę**.

8. Zaczekaj, aż zatrzymuje usług certyfikatów Active Directory.

9. Kliknij prawym przyciskiem myszy **contoso-CORPCA-CA** *,* wskaż **wszystkie zadania**, a następnie kliknij przycisk **Uruchom usługę**.

10. Zamknij **urzędu certyfikacji** konsoli.

11. Zamknij wszystkie otwarte okna, a następnie wylogować.

**Ostatni krok w ramach wdrożenia** jest chcemy upewnić się, że CONTOSO\\menedżerów Menedżera można wdrożyć i tworzenie szablonów i skonfigurować system bez schematu oraz Administratorzy domeny. Następny skrypt będzie listy ACL uprawnień szablonów certyfikatów przy użyciu dsacls. Uruchom przy użyciu konta, które ma pełne uprawnienia do zmiany zabezpieczeń uprawnienia odczytu i zapisu do każdego istniejącego szablonu certyfikatu w lesie.

Pierwsze kroki: **Konfigurowanie punktu połączenia usługi i uprawnienia grupy docelowej i delegowanie zarządzania szablonu profilu**

1. Skonfiguruj uprawnienia dla punktu połączenia usługi (SCP).

2. Skonfiguruj profil delegowane Zarządzanie szablonami.

3. Skonfiguruj uprawnienia dla punktu połączenia usługi (SCP). **\<skrypt nie jest\>**

4.   Upewnij się, że nawiązano **kontrolera domeny CORPDC** serwera wirtualnego.

5. Zaloguj się jako **contoso\\konta corpadmin**

6. Z **narzędzia administracyjne**, otwórz **użytkownicy usługi Active Directory i komputery**.

7. W **użytkownicy usługi Active Directory i komputery**na **widoku** menu, upewnij się, że **funkcje zaawansowane** jest włączona.

8. W drzewie konsoli rozwiń **Contoso.com** \| **systemu** \| **Microsoft** \| **cykl życia certyfikatu Menedżer**, a następnie kliknij przycisk **CORPCM**.

9. Kliknij prawym przyciskiem myszy **CORPCM**, a następnie kliknij przycisk **właściwości**.

10. W **właściwości CORPCM** dialogowym **zabezpieczeń** kartę, Dodaj następujące grupy przy użyciu odpowiednich uprawnień:

    | Grupa          | Uprawnienia      |
    |----------------|------------------|
    | Menedżerowie Menedżera | Odczyt </br> Inspekcja zarządzania certyfikatami w usłudze FIM</br> Agenta rejestracji programu FIM CM</br> Rejestrowanie programu FIM CM żądania</br> Odzyskaj żądania zarządzania certyfikatami w usłudze FIM</br> Odnów żądania zarządzania certyfikatami w usłudze FIM</br> Odwołaj żądania zarządzania certyfikatami w usłudze FIM </br> FIM CM żądania odblokowania karty inteligentnej |
    | Pomoc techniczna Menedżera | Odczyt</br> Agenta rejestracji programu FIM CM</br> Odwołaj żądania zarządzania certyfikatami w usłudze FIM</br> FIM CM żądania odblokowania karty inteligentnej |

11. W **właściwości kontrolera domeny CORPDC** okno dialogowe, kliknij przycisk **OK**.

12. Pozostaw **użytkownicy usługi Active Directory i komputery** Otwórz.

**Skonfiguruj uprawnienia do obiektów podrzędnych użytkownika**

1. Upewnij się, że jesteś nadal w **użytkownicy usługi Active Directory i komputery** konsoli.

2. W drzewie konsoli kliknij prawym przyciskiem myszy **Contoso.com**, a następnie kliknij przycisk **właściwości**.

3. Na **zabezpieczeń** kliknij pozycję **Zaawansowane**.

4. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.

5. W **wybierz użytkownika, komputera, konto usługi lub grupy** dialogowym **wprowadź nazwę obiektu do wybrania** wpisz **menedżerów Menedżera**, a następnie kliknij przycisk **OK**.

6. W **wpis uprawnienia dla Contoso** okno dialogowe, **dotyczą** listy wybierz **obiektów użytkowników podrzędny** , a następnie Włącz **Zezwalaj**pole wyboru dla następujących **uprawnienia**:

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Inspekcja zarządzania certyfikatami w usłudze FIM**

    - **Agenta rejestracji programu FIM CM**

    - **Rejestrowanie programu FIM CM żądania**

    - **Odzyskaj żądania zarządzania certyfikatami w usłudze FIM**

    - **Odnów żądania zarządzania certyfikatami w usłudze FIM**

    - **Odwołaj żądania zarządzania certyfikatami w usłudze FIM**

    - **FIM CM żądania odblokowania karty inteligentnej**

7. W **wpis uprawnienia dla Contoso** okno dialogowe, kliknij przycisk **OK**.

8. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.

9. W **wybierz użytkownika, komputera, konto usługi lub grupy** dialogowym **wprowadź nazwę obiektu do wybrania** wpisz **pomocy Menedżera**, a następnie kliknij przycisk **OK**.

10. W **wpis uprawnienia dla Contoso** okno dialogowe, **dotyczą** listy wybierz **obiektów użytkowników podrzędny** , a następnie wybierz **Zezwalaj**pole wyboru dla następujących **uprawnienia**:

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Agenta rejestracji programu FIM CM**

    - **Odwołaj żądania zarządzania certyfikatami w usłudze FIM**

    - **FIM CM żądania odblokowania karty inteligentnej**

11. W **wpis uprawnienia dla Contoso** okno dialogowe, kliknij przycisk **OK**.

12. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **OK**.

13. W **contoso.com właściwości** okno dialogowe, kliknij przycisk **OK**.

14. Pozostaw **użytkownicy usługi Active Directory i komputery** Otwórz.

**Skonfiguruj uprawnienia do obiektów podrzędnych użytkownika \<bez skryptu\>**

1. Upewnij się, że jesteś nadal w **użytkownicy usługi Active Directory i komputery** konsoli.

2. W drzewie konsoli kliknij prawym przyciskiem myszy **Contoso.com**, a następnie kliknij przycisk **właściwości**.

3. Na **zabezpieczeń** kliknij pozycję **Zaawansowane**.

4. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **Dodaj**.

5. W **wybierz użytkownika, komputera, konto usługi lub grupy** dialogowym **wprowadź nazwę obiektu do wybrania** wpisz **menedżerów Menedżera**, a następnie kliknij przycisk **OK**.

6. W **wpis uprawnienia dla CONTOSO** okno dialogowe, **dotyczą** listy wybierz **obiektów użytkowników podrzędny** , a następnie Włącz **Zezwalaj**pole wyboru dla następujących **uprawnienia**:

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Inspekcja zarządzania certyfikatami w usłudze FIM**

    - **Agenta rejestracji programu FIM CM**

    - **Rejestrowanie programu FIM CM żądania**

    - **Odzyskaj żądania zarządzania certyfikatami w usłudze FIM**

    - **Odnów żądania zarządzania certyfikatami w usłudze FIM**

    - **Odwołaj żądania zarządzania certyfikatami w usłudze FIM**

    - **FIM CM żądania odblokowania karty inteligentnej**

7. W **wpis uprawnienia dla CONTOSO** okno dialogowe, kliknij przycisk **OK**.

8. W **Zaawansowane ustawienia zabezpieczeń dla CONTOSO** okno dialogowe, kliknij przycisk **Dodaj**.

9. W **wybierz użytkownika, komputera, konto usługi lub grupy** dialogowym **wprowadź nazwę obiektu do wybrania** wpisz **pomocy Menedżera**, a następnie kliknij przycisk **OK**.

10. W **wpis uprawnienia dla CONTOSO** okno dialogowe, **dotyczą** listy wybierz **obiektów użytkowników podrzędny** , a następnie wybierz **Zezwalaj**pole wyboru dla następujących **uprawnienia**:

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Agenta rejestracji programu FIM CM**

    - **Odwołaj żądania zarządzania certyfikatami w usłudze FIM**

    - **FIM CM żądania odblokowania karty inteligentnej**

11. W **wpis uprawnienia dla contoso** okno dialogowe, kliknij przycisk **OK**.

12. W **Zaawansowane ustawienia zabezpieczeń dla Contoso** okno dialogowe, kliknij przycisk **OK**.

13. W **contoso.com właściwości** okno dialogowe, kliknij przycisk **OK**.

14. Pozostaw **użytkownicy usługi Active Directory i komputery** Otwórz.

Druga procedura: **Delegowanie uprawnień do zarządzania szablonów certyfikatów \<skryptu\>**

- Delegowanie uprawnień w kontenerze szablonów certyfikatów.

- Delegowanie uprawnień w kontenerze OID.

- Delegowanie uprawnień do istniejących szablonów certyfikatów.

Definiowanie uprawnień w kontenerze szablonów certyfikatów:

1. Przywróć **Lokacje usługi Active Directory i usługi** konsoli.

2. W drzewie konsoli rozwiń **usług**, rozwiń węzeł **Public Key Services**, a następnie kliknij przycisk **szablonów certyfikatów**.

3. W drzewie konsoli kliknij prawym przyciskiem myszy **szablonów certyfikatów**, a następnie kliknij przycisk **Deleguj kontrolę**.

4. W **delegowania kontroli** kreatora, kliknij polecenie **dalej**.

5. Na **użytkownikom lub grupom** kliknij **Dodaj**.

6. W **Wybieranie: użytkownicy, komputery lub grupy** dialogowym **wprowadź nazwy obiektów do wybrania** wpisz **menedżerów Menedżera**, a następnie kliknij przycisk **OK**.

7. Na **użytkownikom lub grupom** kliknij **dalej**.

8. Na **zadania do oddelegowania** kliknij **Utwórz zadanie niestandardowe do delegowania**, a następnie kliknij przycisk **dalej**.

9.  Na **typ obiektu usługi Active Directory** strony, upewnij się, że **ten folder istniejących obiektów w tym folderze i tworzenie nowych obiektów w tym folderze** jest zaznaczone, a następnie kliknij przycisk **dalej**.

10. Na **uprawnienia** strony w **uprawnienia** listy wybierz **Pełna kontrola** pole wyboru, a następnie kliknij przycisk **dalej**.

11. Na **Kończenie pracy Kreatora delegowania kontroli** kliknij **Zakończ**.

Definiowanie uprawnień w kontenerze OID:

1. W drzewie konsoli kliknij prawym przyciskiem myszy **OID**, a następnie kliknij przycisk **właściwości**.

2. W **właściwości identyfikatora OID** dialogowym **zabezpieczeń** kliknij pozycję **zaawansowane**.

3. W **Zaawansowane ustawienia zabezpieczeń dla identyfikatora OID** okno dialogowe, kliknij przycisk **Dodaj**.

4. W **wybierz użytkownika, komputera, konto usługi lub grupy** dialogowym **wprowadź nazwę obiektu do wybrania** wpisz **menedżerów Menedżera**, a następnie kliknij przycisk **OK**.

5. W **wpis uprawnienia dla identyfikatora OID** okna dialogowego pole, upewnij się, że uprawnienia mają zastosowanie do **ten obiekt i wszystkie obiekty zależne**, kliknij przycisk **Pełna kontrola**, a następnie kliknij przycisk  **OK**.

6. W **Zaawansowane ustawienia zabezpieczeń dla identyfikatora OID** okno dialogowe, kliknij przycisk **OK**.

7. W **właściwości identyfikatora OID** okno dialogowe, kliknij przycisk **OK**.

8. Zamknij **Lokacje i usługi Active Directory**.

**Skrypty: Uprawnień w kontenerze OID, szablon profilu i szablonów certyfikatów**

![diagram](media/mim-cm-deploy/image021.png)

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

![diagram](media/mim-cm-deploy/image039.png)

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
