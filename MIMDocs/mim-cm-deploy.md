---
title: Wdrażanie Microsoft Identity Manager Menedżerze certyfikatów | Microsoft Docs
description: Zainstaluj Menedżera certyfikatów Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1885f01d1010565cef9ce50028ae1c86e8769003
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492544"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Wdrażanie Microsoft Identity Manager Menedżerze certyfikatów 2016 (zarządzanie certyfikatami w usłudze MIM)

Instalacja programu Microsoft Identity Manager Certificate Manager 2016 (zarządzanie certyfikatami w usłudze MIM) obejmuje kilka kroków. W celu uproszczenia procesu, w którym są one przerywane. Istnieją wstępne kroki, które należy wykonać przed wszelkimi rzeczywistymi zarządzanie certyfikatami w usłudze MIMmi krokami. Bez wstępnej pracy instalacja prawdopodobnie nie powiedzie się.

Na poniższym diagramie przedstawiono przykładowy typ środowiska, który może być używany. Systemy z numerami są zawarte na liście poniżej diagramu i są wymagane do pomyślnego wykonania kroków opisanych w tym artykule. Na koniec są używane serwery z systemem Windows 2016 Datacenter:

![Diagram środowiska](media/mim-cm-deploy/image001.png)

1. CORPDC — kontroler domeny
2. CORPCM — serwer zarządzanie certyfikatami w usłudze MIM
3. CORPCA — urząd certyfikacji
4. CORPCMR zarządzanie certyfikatami w usłudze MIM — Portal Web API REST dla interfejsu API REST — używany przez nowszy
5. CORPSQL1 — SQL 2016 Z DODATKIEM SP1
6. CORPWK1 — przyłączone do domeny systemu Windows 10

## <a name="deployment-overview"></a>Omówienie wdrażania

- Podstawowa instalacja systemu operacyjnego

    Laboratorium składa się z serwerów centrów danych systemu Windows 2016.

    >[!NOTE]
    >Więcej informacji na temat obsługiwanych platform dla programu MIM 2016 zapoznaj się z artykułem zatytułowanym [obsługiwane platformy dla programu mim 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. Kroki przed wdrożeniem

    - [Rozszerzanie schematu](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Tworzenie kont usług

    - [Tworzenie szablonów certyfikatów](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Konfigurowanie protokołu Kerberos

    - Kroki związane z bazą danych

        - Wymagania dotyczące konfiguracji SQL

        - Uprawnienia bazy danych

2. Wdrożenie

## <a name="pre-deployment-steps"></a>Kroki przed wdrożeniem

Aby można było pomyślnie zakończyć pracę, Kreator konfiguracji zarządzanie certyfikatami w usłudze MIM wymaga podania informacji.

![diagram](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Rozszerzanie schematu

Proces rozszerzania schematu jest prosty, ale należy zachować ostrożność z powodu jego nieodwracalnego charakteru.

>[!NOTE]
>Ten krok wymaga, aby używane konto miało uprawnienia administratora schematu.

1. Przejdź do lokalizacji nośnika programu MIM i przejdź do \\ folderu zarządzania certyfikatami \\ x64.

2. Skopiuj folder Schema do CORPDC, a następnie przejdź do niego.

    ![diagram](media/mim-cm-deploy/image005.png)

3. Uruchom skrypt resourceForestModifySchema.vbs scenariuszu dotyczącego jednego lasu. W scenariuszu lasu zasobów Uruchom skrypty:
   - Domena a — użytkownicy z systemem (userForestModifySchema.vbs)
   - ResourceForestB — lokalizacja instalacji programu CM (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Zmiany schematu są operacją jednokierunkową i wymagają odzyskania lasu do wycofania, dlatego należy się upewnić, że istnieją wymagane kopie zapasowe. Aby uzyskać szczegółowe informacje na temat zmian wprowadzonych w schemacie przez wykonanie tej operacji, zapoznaj się ze [schematem zarządzania certyfikatami programu Forefront Identity Manager 2010](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![diagram](media/mim-cm-deploy/image007.png)

4. Uruchom skrypt, a po zakończeniu działania skryptu powinien pojawić się komunikat o powodzeniu.

    ![Komunikat z informacją o powodzeniu](media/mim-cm-deploy/image009.png)

Schemat w usłudze AD został teraz rozszerzony do obsługi zarządzanie certyfikatami w usłudze MIM.

### <a name="creating-service-accounts-and-groups"></a>Tworzenie kont i grup usług

Poniższa tabela zawiera podsumowanie kont i uprawnień wymaganych przez zarządzanie certyfikatami w usłudze MIM. Zarządzanie certyfikatami w usłudze MIM można automatycznie utworzyć następujące konta lub można je utworzyć przed rozpoczęciem instalacji. Rzeczywiste nazwy kont można zmienić. W przypadku tworzenia kont należy rozważyć nadanie nazw kontom użytkowników w taki sposób, aby można było łatwo dopasować nazwę konta użytkownika do jego funkcji.

Użytkownikowi

![Diagram](media/mim-cm-deploy/image010.png)

![Diagram](media/mim-cm-deploy/image012.png)

| **Role**                   | **Nazwa logowania użytkownika** |
|----------------------------|---------------------|
| Agent zarządzanie certyfikatami w usłudze MIM               | MIMCMAgent          |
| Agent odzyskiwania kluczy zarządzanie certyfikatami w usłudze MIM  | MIMCMKRAgent        |
| Agent autoryzacji zarządzanie certyfikatami w usłudze MIM | MIMCMAuthAgent      |
| Agent menedżera urzędu certyfikacji zarządzanie certyfikatami w usłudze MIM    | MIMCMManagerAgent   |
| zarządzanie certyfikatami w usłudze MIM agenta puli sieci Web      | MIMCMWebAgent       |
| Agent rejestracji zarządzanie certyfikatami w usłudze MIM    | MIMCMEnrollAgent    |
| Usługa aktualizacji zarządzanie certyfikatami w usłudze MIM      | MIMCMService        |
| Konto instalacji programu MIM        | MIMINSTALL          |
| Agent pomocy technicznej            | CMHelpdesk1-2       |
| Menedżer CM                 | CMManager1-2        |
| Użytkownik subskrybenta            | CMUser1-2           |

Grupowania

| **Role**               | **Grupa**         |
|------------------------|-------------------|
| Członkowie pomocy technicznej CM    | MIMCM-Helpdesk    |
| Elementy członkowskie Menedżera CM     | MIMCM-Managers    |
| Użytkownicy subskrybentów CM | MIMCM-Subscribers |

PowerShell: konta agentów:

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

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Aktualizowanie zasad lokalnych serwera **CORPCM** dla kont agentów 

| **Nazwa logowania użytkownika** | **Opis i uprawnienia**   |
|------|---------------------|
| MIMCMAgent          | Program udostępnia następujące usługi: </br>— Pobiera z urzędu certyfikacji zaszyfrowane klucze prywatne. </br>-Chroni informacje o numerze PIN karty inteligentnej w bazie danych programu FIM CM. </br>— Chroni komunikację między programem FIM CM a urzędem certyfikacji. </br></br> To konto użytkownika wymaga następujących ustawień kontroli dostępu:</br>-   **Zezwalaj na logowanie lokalnego** użytkownika.</br>-   **Wystawianie i zarządzanie certyfikatami** użytkownika. </br>-Uprawnienia do odczytu i zapisu w folderze Temp systemu w następującej lokalizacji:% WINDIR% \\ temp.</br>-Podpis cyfrowy i certyfikat szyfrowania wystawione i zainstalowane w magazynie użytkownika.
|MIMCMKRAgent        | Odzyskuje zarchiwizowane klucze prywatne z urzędu certyfikacji. To konto użytkownika wymaga następujących ustawień kontroli dostępu:</br> -   **Zezwalaj na logowanie lokalnego** użytkownika.</br>— Członkostwo w lokalnej grupie **administratorzy** . </br>-Zarejestruj uprawnienie dla szablonu certyfikatu **KeyRecoveryAgent** . </br>— Certyfikat agenta odzyskiwania kluczy jest wystawiany i instalowany w magazynie użytkownika. Certyfikat należy dodać do listy agentów odzyskiwania kluczy w urzędzie certyfikacji. </br>-Uprawnienia do odczytu i uprawnienia do zapisu w folderze Temp systemu w następującej lokalizacji: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Określa prawa i uprawnienia użytkowników dla użytkowników i grup. To konto użytkownika wymaga następujących ustawień kontroli dostępu: </br>— Członkostwo w grupie domeny dostępu zgodnej z systemami starszymi niż Windows 2000. </br> -Udzielono praw użytkownika **Generuj inspekcje zabezpieczeń** .             |
| MIMCMManagerAgent   | Wykonuje działania związane z zarządzaniem urzędem certyfikacji. </br> Użytkownikowi musi być przypisane uprawnienie Zarządzanie urzędem certyfikacji.        |
| MIMCMWebAgent       | Zapewnia tożsamość puli aplikacji usług IIS. Program FIM CM jest uruchamiany w procesie interfejsu programowania aplikacji® Win32, który używa poświadczeń tego użytkownika. </br> To konto użytkownika wymaga następujących ustawień kontroli dostępu:</br> -Członkostwo w IIS_WPG lokalnym **, windows 2016 = IIS_IUSRS** Group. </br>— Członkostwo w lokalnej grupie **administratorzy** .</br>-Udzielono praw użytkownika **Generuj inspekcje zabezpieczeń** . </br>-Udzielono **działania w ramach prawa użytkownika systemu operacyjnego** . </br>-Przyznano prawo użytkownika **Zastąp token na poziomie procesu** .</br>-Przypisane jako tożsamość puli aplikacji IIS **CLMAppPool**. </br>-Udzielono uprawnienia do odczytu w    **programie HKEY_LOCAL_MACHINE klucz rejestru \\ \\ \\ \\ \\ \\ WebUser programu Microsoft CLM v 1.0 Server** . </br>— To konto musi być również zaufane na potrzeby delegowania.|
| MIMCMEnrollAgent    | Wykonuje rejestrację w imieniu użytkownika. To konto użytkownika wymaga następujących ustawień kontroli dostępu:</br>-Certyfikat agenta rejestracji, który został wystawiony i zainstalowany w magazynie użytkownika.</br>-   **Zezwalaj na logowanie lokalnego** użytkownika. </br>-Zarejestruj uprawnienie w szablonie certyfikatu **agenta rejestracji** (lub w szablonie niestandardowym, jeśli jest używany).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Tworzenie szablonów certyfikatów dla kont usługi zarządzanie certyfikatami w usłudze MIM

Trzy z kont usług używanych przez zarządzanie certyfikatami w usłudze MIM wymagają certyfikatu, a Kreator konfiguracji wymaga podania nazwy szablonów certyfikatów, które powinny być używane do żądania certyfikatów.

Konta usług, które wymagają certyfikatów:

- MIMCMAgent: to konto wymaga certyfikatu użytkownika

- MIMCMEnrollAgent: to konto wymaga certyfikatu agenta rejestracji

- MIMCMKRAgent: to konto wymaga certyfikatu **agenta odzyskiwania kluczy**

Istnieją już szablony w usłudze AD, ale musimy utworzyć własne wersje, aby współpracować z zarządzanie certyfikatami w usłudze MIM. Musimy wprowadzić modyfikację oryginalnych szablonów bazowych.

Wszystkie trzy z powyższych kont będą mieć podwyższony poziom uprawnień w organizacji i powinny być obsługiwane uważnie.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Tworzenie szablonu certyfikatu podpisywania zarządzanie certyfikatami w usłudze MIM

1. W obszarze **Narzędzia administracyjne** Otwórz pozycję **urząd certyfikacji**.

2. W konsoli **urząd certyfikacji** w drzewie konsoli rozwiń węzeł **contoso-CorpCA** , a następnie kliknij pozycję **Szablony certyfikatów**.

3. Kliknij prawym przyciskiem myszy pozycję **Szablony certyfikatów** , a następnie kliknij polecenie **Zarządzaj**.

4. W **konsoli szablony certyfikatów** w okienku **szczegółów** wybierz i kliknij prawym przyciskiem myszy pozycję **użytkownik** , a następnie kliknij polecenie **Duplikuj szablon**.

5. W oknie dialogowym **Duplikowanie szablonu** wybierz pozycję **Windows Server 2003 Enterprise** , a następnie kliknij przycisk **OK**.

    ![Pokaż wyniki zmian](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >Zarządzanie certyfikatami w usłudze MIM nie działa z certyfikatami opartymi na szablonach certyfikatów w wersji 3. Należy utworzyć szablon certyfikatu systemu Windows Server® 2003 Enterprise (w wersji 2). Aby uzyskać więcej informacji, zobacz [v3](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) .

6. W oknie dialogowym **właściwości nowego szablonu** na karcie **Ogólne** w polu **Nazwa wyświetlana szablonu** wpisz **Zarządzanie certyfikatami w usłudze MIM podpisywanie**. Zmień **okres ważności** na **2 lata** , a następnie wyczyść pole wyboru **Publikuj certyfikat w Active Directory** .

7. Na karcie **Obsługiwanie żądań** upewnij się, że pole wyboru **Zezwalaj na eksportowanie klucza prywatnego** jest zaznaczone, a następnie kliknij przycisk **Kryptografia karta**.

8. W oknie dialogowym **Wybieranie kryptografii** Wyłącz **rozszerzonego dostawcę usług kryptograficznych firmy Microsoft** , Włącz dostawcę **usług kryptograficznych RSA i AES firmy Microsoft** , a następnie kliknij przycisk **OK**.

9. Na karcie **nazwa podmiotu** Usuń zaznaczenie pola wyboru **Dołącz nazwę e-mail w polu Nazwa podmiotu** i **nazwa e-mail** .

10. Na karcie **rozszerzenia** na liście **rozszerzenia zawarte w tym szablonie** upewnij się, że **zasady aplikacji** są zaznaczone, a następnie kliknij przycisk **Edytuj**.

11. W oknie dialogowym **Edytowanie rozszerzenia zasad aplikacji** wybierz zarówno **System szyfrowania plików** jak i zasady bezpiecznego stosowania **poczty e-mail** . Kliknij przycisk **Usuń** , a następnie kliknij przycisk **OK**.

12. Na karcie **zabezpieczenia** wykonaj następujące czynności:

    - Usuń **administratora**.

    - Usuń **administratorów domeny**.

    - Usuń **użytkowników domeny**.

    - Przypisz uprawnienia do **odczytu** i **zapisu** dla **administratorów przedsiębiorstwa**.

    - Dodaj **MIMCMAgent.**

    - Przypisz uprawnienia **Odczytaj** i **zarejestruj** do **MIMCMAgent**.

13. W oknie dialogowym **właściwości nowego szablonu** kliknij przycisk **OK**.

14. Pozostaw otwartą **konsolę Szablony certyfikatów** .

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Tworzenie szablonu certyfikatu agenta rejestracji zarządzanie certyfikatami w usłudze MIM

1. W **konsoli szablony certyfikatów** w okienku **szczegółów** wybierz i kliknij prawym przyciskiem myszy pozycję **Agent rejestracji** , a następnie kliknij polecenie **Duplikuj szablon**.

2. W oknie dialogowym **Duplikowanie szablonu** wybierz pozycję **Windows Server 2003 Enterprise** , a następnie kliknij przycisk **OK**.

3. W oknie dialogowym **właściwości nowego szablonu** na karcie **Ogólne** w polu **Nazwa wyświetlana szablonu** wpisz **Zarządzanie certyfikatami w usłudze MIM Agent rejestracji**. Upewnij się, że **okres ważności** wynosi **2 lata**.

4. Na karcie **Obsługiwanie żądań** Włącz opcję **Zezwalaj na eksportowanie klucza prywatnego** , a następnie kliknij pozycję **CSP lub kartę Kryptografia.**

5. W oknie **dialogowym wybór dostawcy usług kryptograficznych** Wyłącz **dostawcę usług** kryptograficznych firmy Microsoft w wersji 1.0, wyłącz **rozszerzonego dostawcę usług kryptograficznych** firmy Microsoft w wersji 1.0, Włącz **rozszerzonego dostawcę usług kryptograficznych RSA i AES** , a następnie kliknij przycisk **OK**.

6. Na karcie **zabezpieczenia** wykonaj następujące czynności:

    - Usuń **administratora**.

    - Usuń **administratorów domeny**.

    - Przypisz uprawnienia do **odczytu** i **zapisu** dla **administratorów przedsiębiorstwa**.

    - Dodaj **MIMCMEnrollAgent**.

    - Przypisz uprawnienia **Odczytaj** i **zarejestruj** do **MIMCMEnrollAgent**.

7. W oknie dialogowym **właściwości nowego szablonu** kliknij przycisk **OK**.

8. Pozostaw otwartą **konsolę Szablony certyfikatów** .

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Utwórz szablon certyfikatu agenta odzyskiwania kluczy zarządzanie certyfikatami w usłudze MIM

1. W konsoli **Szablony certyfikatów** w okienku **szczegółów** wybierz i kliknij prawym przyciskiem myszy pozycję **agent odzyskiwania kluczy** , a następnie kliknij polecenie **Duplikuj szablon**.

2. W oknie dialogowym **Duplikowanie szablonu** wybierz pozycję **Windows Server 2003 Enterprise** , a następnie kliknij przycisk **OK**.

3. W oknie dialogowym **właściwości nowego szablonu** na karcie **Ogólne** w polu **Nazwa wyświetlana szablonu** wpisz **Zarządzanie certyfikatami w usłudze MIM agent odzyskiwania kluczy**. Upewnij się, że **okres ważności** wynosi **2 lata** na **karcie Kryptografia.**

4. W oknie dialogowym **Wybór dostawców** Wyłącz **rozszerzonego dostawcę usług kryptograficznych firmy Microsoft** , Włącz dostawcę **usług kryptograficznych RSA i AES** , a następnie kliknij przycisk **OK**.

5. Na karcie **Wymagania wystawiania** upewnij się, że **zatwierdzenie Menedżera certyfikatów urzędu certyfikacji** jest **wyłączone**.

6. Na karcie **zabezpieczenia** wykonaj następujące czynności:

    - Usuń **administratora**.

    - Usuń **administratorów domeny**.

    - Przypisz uprawnienia do **odczytu** i **zapisu** dla **administratorów przedsiębiorstwa**.

    - Dodaj **MIMCMKRAgent**.

    - Przypisz uprawnienia **Odczytaj** i **zarejestruj** do **KRAgent**.

7. W oknie dialogowym **właściwości nowego szablonu** kliknij przycisk **OK**.

8. Zamknij okno **Konsola szablonów certyfikatów**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publikowanie wymaganych szablonów certyfikatów w urzędzie certyfikacji

1. Przywróć konsolę **urząd certyfikacji** .

2. W konsoli **urząd certyfikacji** w drzewie konsoli kliknij prawym przyciskiem myszy pozycję **Szablony certyfikatów** , wskaż polecenie **Nowy** , a następnie kliknij pozycję **szablon certyfikatu do wystawienia**.

3. W oknie dialogowym **Włączanie szablonu certyfikatu** wybierz pozycję **Zarządzanie certyfikatami w usłudze MIM agent rejestracji** , **Zarządzanie certyfikatami w usłudze MIM agent odzyskiwania kluczy** i **podpisywanie zarządzanie certyfikatami w usłudze MIM**. Kliknij przycisk **OK**.

4. W drzewie konsoli kliknij pozycję **Szablony certyfikatów**.

5. Sprawdź, czy trzy nowe szablony są wyświetlane w okienku **szczegółów** , a następnie zamknij pozycję **urząd certyfikacji**.

    ![Podpisywanie zarządzanie certyfikatami w usłudze MIM](media/mim-cm-deploy/image016.png)

6. Zamknij wszystkie otwarte okna i wyloguj się.

### <a name="iis-configuration"></a>Konfiguracja usług IIS

Aby hostować witrynę sieci Web w wersji CM, zainstaluj i Skonfiguruj usługi IIS.

#### <a name="install-and-configure-iis"></a>Instalowanie i Konfigurowanie usług IIS

1. Zaloguj się, aby **CORLog** jako konto **MIMINSTALL**

    >[!IMPORTANT]
    >Konto instalacji programu MIM powinno być kontem administratora lokalnego

2. Otwórz program PowerShell i uruchom następujące polecenie

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Witryna o nazwie domyślna witryna sieci Web jest instalowana domyślnie z usługami IIS 7. Jeśli nazwa witryny została zmieniona lub została usunięta, należy udostępnić witrynę z nazwą domyślna witryna sieci Web, aby można było zainstalować zarządzanie certyfikatami w usłudze MIM.

#### <a name="configuring-kerberos"></a>Konfigurowanie protokołu Kerberos

Na koncie MIMCMWebAgent będzie uruchomiony Portal zarządzanie certyfikatami w usłudze MIM. Domyślnie w usługach IIS i uwierzytelnianie Tryb jądra jest domyślnie używany w usługach IIS. Należy wyłączyć uwierzytelnianie w trybie jądra Kerberos i skonfigurować nazwy SPN na koncie MIMCMWebAgent. Niektóre polecenia będą wymagały podwyższonego poziomu uprawnień w usłudze Active Directory i serwerze CORPCM.

![Diagram](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Aktualizowanie usług IIS na CORPCM**

![diagram](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Konieczne będzie dodanie rekordu DNS A dla "cm.contoso.com" i wskazanie adresu IP CORPCM

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Wymaganie protokołu SSL w portalu zarządzanie certyfikatami w usłudze MIM

Zdecydowanie zaleca się wymaganie protokołu SSL w portalu zarządzanie certyfikatami w usłudze MIM. Jeśli nie, Kreator będzie nawet ostrzegał o tym.

1. Zarejestruj się w certyfikacie sieci Web dla **cm.contoso.com** Przypisz do domyślnej lokacji

2. Otwórz **Menedżera usług IIS** i przejdź do **zarządzania certyfikatami**

3. W obszarze Widok funkcji kliknij dwukrotnie pozycję Ustawienia protokołu SSL.

4. Na stronie Ustawienia protokołu SSL wybierz opcję **Wymagaj protokołu SSL**.

5. W okienku Akcje kliknij pozycję **Zastosuj.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>**CORPSQL** konfiguracji bazy danych dla zarządzanie certyfikatami w usłudze MIM

1. Upewnij się, że nawiązano połączenie z serwerem CORPSQL01.

2. Upewnij się, że użytkownik jest zalogowany jako administrator bazy danych SQL.

3. Uruchom następujący skrypt T-SQL, aby umożliwić \\ kontu contoso MIMINSTALL Tworzenie bazy danych po przejściu do kroku konfiguracji

    >[!NOTE]
    >Będziemy musieli wrócić do programu SQL, gdy wszystko będzie gotowe do modułu zasad & zakończenia

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Komunikat o błędzie Kreatora konfiguracji zarządzanie certyfikatami w usłudze MIM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Wdrażanie zarządzania certyfikatami Microsoft Identity Manager 2016

1. Upewnij się, że nawiązano połączenie z serwerem CORPCM i że konto **MIMINSTALL** jest członkiem **lokalnej grupy administratorów** .

2. Upewnij się, że użytkownik jest zalogowany jako contoso \\ MIMINSTALL.

3. Zainstaluj Microsoft Identity Manager 2016 SP1 lub nowszy dodatek Service Pack ISO.

4. **Otwórz** katalog **Zarządzanie certyfikatami \\ x64** .

5. W oknie **x64** kliknij prawym przyciskiem myszy pozycję **Instalator** , a następnie kliknij polecenie **Uruchom jako administrator**.

6. Na stronie Witamy w Kreatorze instalacji zarządzanie certyfikatami w usłudze Microsoft Identity Manager kliknij przycisk **Dalej.**

7. Na stronie Umowa licencyjna na End-User zapoznaj się z umową, **zaznacz pole wyboru** Akceptuję warunki w umowie licencyjnej, a następnie kliknij przycisk Dalej.

8. Na stronie Instalacja niestandardowa upewnij się, że w **portalu zarządzanie certyfikatami w usłudze MIM** i **składniki usługi aktualizacji zarządzanie certyfikatami w usłudze MIM** zostały ustawione na zainstalowanie, a następnie **kliknij przycisk Dalej**.

9. Na stronie wirtualny folder sieci Web upewnij się, że nazwa folderu wirtualnego to **CertificateManagement** , a następnie **kliknij przycisk Dalej**.

10. Na stronie Instalowanie zarządzanie certyfikatami w usłudze Microsoft Identity Manager **kliknij przycisk Instaluj**.

11. Na stronie **ukończono** kreatora instalacji zarządzanie certyfikatami w usłudze Microsoft Identity Manager **kliknij przycisk Zakończ**.

![Zakończono działanie kreatora zarządzanie certyfikatami w usłudze MIM](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Kreator konfiguracji zarządzania certyfikatami w Microsoft Identity Manager 2016

Przed zalogowaniem się do CORPCM Dodaj MIMINSTALL do grupy **Administratorzy domeny, Administratorzy schematu i grupę Administratorzy lokalni** dla Kreatora konfiguracji. Tę możliwość można później usunąć po zakończeniu konfiguracji.

![Komunikat o błędzie](media/mim-cm-deploy/image028.png)

1. W menu **Start** kliknij pozycję **Kreator konfiguracji zarządzania certyfikatami**. I Uruchom jako **administrator**

2. Na stronie **Witamy w Kreatorze konfiguracji** kliknij przycisk **dalej**.

3. Na stronie **Konfiguracja urzędu certyfikacji** upewnij się, że wybrany urząd certyfikacji jest **contoso-CORPCA-CA** , i upewnij się, że wybrany serwer to     **CORPCA. CONTOSO.COM** , a następnie kliknij przycisk **dalej**.

4. Na stronie **Konfigurowanie bazy danych programu Microsoft® SQL Server®** w polu **nazwa SQL Server** wpisz **CORPSQL1** , Włącz pole wyboru **Użyj moich poświadczeń do utworzenia bazy danych** , a następnie kliknij przycisk **dalej**.

5. Na stronie **Ustawienia bazy danych** zaakceptuj domyślną nazwę bazy danych **FIMCertificateManagement** , upewnij się, że jest zaznaczone pole wyboru **uwierzytelnianie zintegrowane SQL** , a następnie kliknij przycisk **dalej**.

6. Na stronie **konfigurowanie Active Directory** Zaakceptuj nazwę domyślną podaną dla punktu połączenia z usługą, a następnie kliknij przycisk **dalej**.

7. Na stronie **Metoda uwierzytelniania** Potwierdź **uwierzytelnianie zintegrowane systemu Windows** , a następnie kliknij przycisk **dalej**.

8. Na stronie **agenci – FIM cm** wyczyść pole wyboru **Użyj ustawień domyślnych programu FIM cm** , a następnie kliknij przycisk **konta niestandardowe**.

9. W oknie dialogowym **agenci — FIM cm** z obsługą wielodostępności na każdej karcie wpisz następujące informacje:

   - Nazwa użytkownika: **Aktualizuj**

   - Hasło: **Pass \@ word1**

   - Potwierdź hasło: **Pass \@ word1**

   - Użyj istniejącego użytkownika: **włączone**

     >[!NOTE]
     >Te konta zostały utworzone wcześniej. Upewnij się, że procedury w kroku 8 są powtórzone dla wszystkich sześciu kart konta agentów.

     ![Konta zarządzanie certyfikatami w usłudze MIM](media/mim-cm-deploy/image030.png)

10. Po zakończeniu wszystkich informacji o koncie agenta kliknij przycisk **OK**.

11. Na stronie **agenci — zarządzanie certyfikatami w usłudze MIM** kliknij przycisk **dalej**.

12. Na stronie **Konfigurowanie certyfikatów serwera** Włącz następujące szablony certyfikatów:

    - Szablon certyfikatu, który ma być używany dla certyfikatu agenta odzyskiwania klucza agenta odzyskiwania: **MIMCMKeyRecoveryAgent**.

    - Szablon certyfikatu, który ma być używany dla certyfikatu agenta programu FIM CM: **MIMCMSigning**.

    - Szablon certyfikatu, który ma być używany dla certyfikatu agenta rejestracji: **FIMCMEnrollmentAgent**.

13. Na stronie **Konfigurowanie certyfikatów serwera** kliknij przycisk **dalej**.

14. Na stronie **Konfigurowanie serwera poczty e-mail, drukowania dokumentu** w polu **Określ nazwę serwera SMTP, który ma być używany do powiadamiania o rejestracji wiadomości E-mail** , a następnie kliknij przycisk **Dalej.**

15. Na stronie **Wszystko gotowe do skonfigurowania** kliknij pozycję **Konfiguruj**.

16. W oknie dialogowym **Kreator konfiguracji — ostrzeżenie programu Microsoft Forefront Identity Manager 2010 R2** kliknij przycisk **OK** , aby potwierdzić, że protokół SSL nie jest włączony w katalogu wirtualnym usług IIS.

    ![Multimedia/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Nie klikaj przycisku Zakończ, dopóki nie zakończy się wykonywanie Kreatora konfiguracji. Rejestrowanie w kreatorze można znaleźć tutaj: **% ProgramFiles% \\ Microsoft Forefront Identity Management \\ 2010 \\ Certificate Management \\ config. log**

17. Kliknij przycisk **Finish** (Zakończ).

    ![Zakończono działanie kreatora zarządzanie certyfikatami w usłudze MIM](media/mim-cm-deploy/image033.png)

18. Zamknij wszystkie otwarte okna.

19. Dodaj `https://cm.contoso.com/certificatemanagement` do lokalnej strefy intranetowej w przeglądarce.

20. Odwiedź witrynę z serwera CORPCM `https://cm.contoso.com/certificatemanagement`  

    ![diagram](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Weryfikowanie usługi izolacji klucza CNG

1. W obszarze **Narzędzia administracyjne** Otwórz pozycję **usługi**.

2. W okienku **szczegółów** kliknij dwukrotnie pozycję **izolacja klucza CNG**.

3. Na karcie **Ogólne** Zmień **Typ uruchamiania** na **automatyczny**.

4. Na karcie **Ogólne** Uruchom usługę, jeśli nie jest w stanie uruchomienia.

5. Na karcie **Ogólne** kliknij przycisk **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalowanie i Konfigurowanie modułów urzędu certyfikacji:

W tym kroku będziemy instalować i konfigurować moduły urzędu certyfikacji w programie FIM CM w urzędzie certyfikacji.

1. Konfigurowanie programu FIM CM w celu sprawdzenia tylko uprawnień użytkowników do operacji zarządzania

2. W oknie **C: \\ Program Files \\ Microsoft Forefront Identity Manager \\ 2010 \\ Certyfikaty w \\ sieci web** Utwórz kopię **web.config** nazwy **web.1.config** kopii.

3. W oknie **sieci Web** kliknij prawym przyciskiem myszy **Web.config** , a następnie kliknij polecenie **Otwórz**.

    >[!Note]
    >Plik Web.config zostanie otwarty w Notatniku

4. Gdy plik zostanie otwarty, naciśnij klawisze CTRL + F.

5. W oknie dialogowym **Znajdź i Zamień** w polu **Znajdź** , wpisz **UseUser** , a następnie kliknij przycisk **Znajdź dalej** trzy razy.

6. Zamknij okno dialogowe **Znajdowanie i zamienianie** .

7. Powinien znajdować się w wierszu **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\>** . Zmień wiersz na odczytany **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>** .

8. Zamknij plik, zapisując wszystkie zmiany.

9. Utwórz konto dla komputera urzędu certyfikacji w programie SQL Server \<no script\>

10. Upewnij się, że nawiązano połączenie z serwerem **CORPSQL01** .

11. Upewnij się, że użytkownik jest zalogowany jako **administrator**

12. Z menu **Start** Uruchom **SQL Server Management Studio**.

13. W oknie dialogowym **łączenie z serwerem** w polu **Nazwa serwera** wpisz **CORPSQL01,** a następnie kliknij przycisk **Połącz**.

14. W drzewie konsoli rozwiń węzeł **zabezpieczenia** , a następnie kliknij przycisk **logowania**.

15. Kliknij prawym przyciskiem myszy pozycję **logowania** , a następnie kliknij pozycję **Nowa nazwa logowania**.

16. Na stronie **Ogólne** w polu **Nazwa logowania** wpisz **contoso \\ CORPCA \$**. Wybierz pozycję **uwierzytelnianie systemu Windows**. Domyślna baza danych to **FIMCertificateManagement**.

17. W lewym okienku wybierz pozycję **Mapowanie użytkownika**. W prawym okienku kliknij pole wyboru w kolumnie **Mapa** obok **FIMCertificateManagement**. Na liście **członkostwo w roli bazy danych dla: FIMCertificateManagement** Włącz rolę **clmApp** .

18. Kliknij przycisk **OK**.

19. Zamknij **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalowanie modułów urzędu certyfikacji programu FIM CM w urzędzie certyfikacji

1. Upewnij się, że nawiązano połączenie z serwerem **CORPCA** .

2. W systemie Windows **x64** kliknij prawym przyciskiem myszy **Setup.exe** , a następnie kliknij polecenie **Uruchom jako administrator**.

3. Na stronie **Witamy w Kreatorze instalacji zarządzanie certyfikatami w usłudze Microsoft Identity Manager** kliknij przycisk **dalej**.

4. Na stronie **Umowa licencyjna użytkownika oprogramowania** zapoznaj się z umową. Zaznacz pole wyboru **Akceptuję warunki umowy licencyjnej** , a następnie kliknij przycisk **dalej**.

5. Na stronie **Konfiguracja niestandardowa** wybierz pozycję **Portal zarządzanie certyfikatami w usłudze MIM** , a następnie kliknij pozycję **Ta funkcja będzie niedostępna**.

6. Na stronie **Instalacja niestandardowa** wybierz pozycję **Zarządzanie certyfikatami w usłudze MIM Update Service** , a następnie kliknij pozycję **Ta funkcja będzie niedostępna**.

    >[!Note]
    >Spowoduje to pozostawienie zarządzanie certyfikatami w usłudze MIM plików urzędu certyfikacji jako jedynej funkcji włączonej dla tej instalacji.

7. Na stronie **Instalacja niestandardowa** kliknij przycisk **dalej**.

8. Na stronie **instalowanie zarządzanie certyfikatami w usłudze Microsoft Identity Manager** kliknij przycisk **Instaluj**.

9. Na stronie **ukończono Kreatora instalacji zarządzanie certyfikatami w usłudze Microsoft Identity Manager** kliknij przycisk **Zakończ**.

10. Zamknij wszystkie otwarte okna.

### <a name="configure-the-mim-cm-exit-module"></a>Konfigurowanie modułu zarządzanie certyfikatami w usłudze MIM Exit

1. W obszarze **Narzędzia administracyjne** Otwórz pozycję **urząd certyfikacji**.

2. W drzewie konsoli kliknij prawym przyciskiem myszy pozycję **contoso-CORPCA-CA** , a następnie kliknij pozycję  **Właściwości**.

3. Na karcie **moduł zakończenia** wybierz pozycję **moduł zakończenia programu FIM cm** , a następnie kliknij przycisk  **Właściwości**.

4. W polu **Określanie parametrów połączenia z bazą danych Menedżera** konfiguracji wpisz **limit czasu połączenia = 15; Utrwalanie informacji o zabezpieczeniach = true; Integrated Security = SSPI; Initial Catalog = FIMCertificateManagement; Data Source = CORPSQL01**. Pozostaw pole wyboru  **Szyfruj parametry połączenia** włączone, a następnie kliknij przycisk **OK**.
5. W oknie komunikatu **zarządzania certyfikatami w usłudze Microsoft FIM** kliknij przycisk **OK**.

6. W oknie dialogowym **contoso-CORPCA — właściwości urzędu certyfikacji** kliknij przycisk **OK**.

7. Kliknij prawym przyciskiem myszy pozycję **contoso-CORPCA-CA** *,* wskaż pozycję **wszystkie zadania** , a następnie kliknij pozycję **Zatrzymaj usługę**. Zaczekaj, aż Active Directory usługi certyfikatów zostaną zatrzymane.

8. Kliknij prawym przyciskiem myszy pozycję **contoso-CORPCA-CA** *,* wskaż pozycję **wszystkie zadania** , a następnie kliknij pozycję **Uruchom usługę**.

9. Zminimalizuj konsolę **urząd certyfikacji** .

10. W obszarze **Narzędzia administracyjne** Otwórz **Podgląd zdarzeń**.

11. W drzewie konsoli rozwiń węzeł **Dzienniki aplikacji i usług** , a następnie kliknij pozycję **Zarządzanie certyfikatami programu FIM**.

12. Na liście zdarzeń Sprawdź, czy najnowsze zdarzenia *nie* uwzględniają żadnych **ostrzeżeń** ani zdarzeń **błędów** od momentu ostatniego ponownego uruchomienia usług certyfikatów.

    >[!NOTE] 
    >Ostatnie zdarzenie powinno określać, że moduł zakończenia został załadowany przy użyciu ustawień z: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Zminimalizuj **Podgląd zdarzeń**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka systemu Windows®

1. Przywróć konsolę **urząd certyfikacji** .

2. W drzewie konsoli rozwiń węzeł **contoso-CORPCA-CA** , a następnie kliknij pozycję **wystawione certyfikaty**.

3. W okienku **szczegółów** kliknij dwukrotnie certyfikat z **contoso \\ MIMCMAgent** w kolumnie **Nazwa osoby żądającej** i z **podpisem programu FIM cm** w kolumnie **szablon certyfikatu** .

4. Na karcie **Szczegóły** wybierz pole **Odcisk palca**.

5. Wybierz odcisk palca, a następnie naciśnij klawisze CTRL + C.

    >[!NOTE]
    >**Nie** Uwzględniaj spacji wiodącej na liście znaków odcisku palca.

6. W oknie dialogowym **certyfikat** kliknij przycisk **OK**.

7. Z menu **Start** , w polu **Wyszukaj programy i pliki** wpisz **Notepad** , a następnie naciśnij klawisz ENTER.

8. W **Notatnik** , z menu **Edycja** , kliknij **Wklej**.

9. W menu **Edycja** kliknij **Zamień**.

10. W polu **Znajdź** wpisz spację, a następnie kliknij pozycję **Zamień wszystkie**.

    >[!Note]
    >Spowoduje to usunięcie wszystkich spacji między znakami w odcisku palca.

11. W oknie dialogowym **Zamień** kliknij przycisk **Anuluj**.

12. Wybierz przekonwertowane *thumbprintstring* , a następnie naciśnij klawisze CTRL + C.

13. Zamknij **Notatnik** bez zapisywania zmian.

### <a name="configure-the-fim-cm-policy-module"></a>Konfigurowanie modułu zasad programu FIM CM

1. Przywróć konsolę **urząd certyfikacji** .

2. Kliknij prawym przyciskiem myszy pozycję **contoso-CORPCA-CA** , a następnie kliknij pozycję **Właściwości**.

3. W oknie dialogowym **contoso-CORPCA — właściwości urzędu certyfikacji** na karcie **moduł zasad** kliknij pozycję **Właściwości**.

    - Na karcie **Ogólne** upewnij się, że wybrano opcję **Przekaż żądania nie korzystające z programu FIM cm do domyślnego modułu zasad do przetwarzania** .

    - Na karcie **certyfikaty podpisywania** kliknij pozycję **Dodaj**.

    - W oknie dialogowym certyfikat kliknij prawym przyciskiem myszy pole **skrótu certyfikat zakodowany szesnastkowo** , a następnie kliknij przycisk **Wklej**.

    - W oknie dialogowym **certyfikat** kliknij przycisk **OK**.

        >[!Note]
        >Jeśli przycisk **OK** nie jest włączony, przypadkowo dołączono ukryty znak w ciągu odcisku palca w przypadku skopiowania odcisku palca z certyfikatu clmAgent. Powtórz wszystkie kroki zaczynające się od **zadania 4: Skopiuj odcisk palca certyfikatu MIMCMAgent do Schowka systemu Windows** w tym ćwiczeniu.

4. W oknie dialogowym **Właściwości konfiguracji** upewnij się, że odcisk palca pojawia się na liście **prawidłowych certyfikatów podpisywania** , a następnie kliknij przycisk **OK**.

5. W oknie komunikatu **zarządzania certyfikatami usługi FIM** kliknij przycisk **OK**.

6. W oknie dialogowym **contoso-CORPCA — właściwości urzędu certyfikacji** kliknij przycisk **OK**.

7. Kliknij prawym przyciskiem myszy pozycję **contoso-CORPCA-CA** *,* wskaż pozycję **wszystkie zadania** , a następnie kliknij pozycję **Zatrzymaj usługę**.

8. Zaczekaj, aż Active Directory usługi certyfikatów zostaną zatrzymane.

9. Kliknij prawym przyciskiem myszy pozycję **contoso-CORPCA-CA** *,* wskaż pozycję **wszystkie zadania** , a następnie kliknij pozycję **Uruchom usługę**.

10. Zamknij konsolę **urząd certyfikacji** .

11. Zamknij wszystkie otwarte okna, a następnie wyloguj się.

**Ostatnim krokiem we wdrożeniu** jest upewnienie się, że firma Contoso \\ serverfqdn określa — menedżerowie mogą wdrażać i tworzyć szablony oraz konfigurować system bez administratorów schematu i domeny. Następny skrypt będzie listą ACL uprawnień do szablonów certyfikatów przy użyciu narzędzia Dsacls. Uruchom z kontem z pełnymi uprawnieniami, aby zmienić uprawnienia do odczytu i zapisu zabezpieczeń w każdym istniejącym szablonie certyfikatu w lesie.

Pierwsze kroki: **Konfigurowanie uprawnień punktu połączenia z usługą i grupy docelowej & delegowania zarządzania szablonami profilu**

1. Skonfiguruj uprawnienia do punktu połączenia z usługą (SCP).

2. Skonfiguruj delegowane Zarządzanie szablonami profilów.

3. Skonfiguruj uprawnienia do punktu połączenia z usługą (SCP). **\<no script\>**

4.   Upewnij się, że nawiązano połączenie z serwerem wirtualnym **CORPDC** .

5. Zaloguj się jako **contoso \\ corpadmin**

6. W obszarze **Narzędzia administracyjne** Otwórz **Active Directory Użytkownicy i komputery**.

7. W **Active Directory Użytkownicy i komputery** , w menu **Widok** , upewnij się, że **funkcje zaawansowane** są włączone.

8. W drzewie konsoli rozwiń węzeł **contoso.com** \| **system** \| **Microsoft** \| **cykl życia certyfikatów** firmy Microsoft, a następnie kliknij przycisk **CORPCM**.

9. Kliknij prawym przyciskiem myszy pozycję **CORPCM** , a następnie kliknij pozycję **Właściwości**.

10. W oknie dialogowym **Właściwości CORPCM** na karcie **zabezpieczenia** Dodaj następujące grupy z odpowiednimi uprawnieniami:

    | Grupa          | Uprawnienia      |
    |----------------|------------------|
    | serverfqdn określa — menedżerowie | Odczyt </br> Inspekcja programu FIM CM</br> Agent rejestracji programu FIM CM</br> Rejestrowanie żądań programu FIM CM</br> Odzyskiwanie żądania programu FIM CM</br> Odnowienie żądania FIM CM</br> Żądanie odwołania FIM CM </br> Odblokowywanie karty inteligentnej przez żądanie programu FIM CM |
    | serverfqdn określa — pomoc techniczna | Odczyt</br> Agent rejestracji programu FIM CM</br> Żądanie odwołania FIM CM</br> Odblokowywanie karty inteligentnej przez żądanie programu FIM CM |

11. W oknie dialogowym **Właściwości CORPDC** kliknij przycisk **OK**.

12. Pozostaw otwarte **Active Directory Użytkownicy i komputery** .

**Konfigurowanie uprawnień do obiektów podrzędnych użytkownika**

1. Upewnij się, że nadal znajdują się w konsoli **Active Directory Użytkownicy i komputery** .

2. W drzewie konsoli kliknij prawym przyciskiem myszy pozycję **contoso.com** , a następnie kliknij polecenie **Właściwości**.

3. Na **zabezpieczeń** kliknij pozycję **Zaawansowane**.

4. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla contoso** kliknij przycisk **Dodaj**.

5. W oknie dialogowym **Wybieranie użytkowników, komputerów, kont usług lub grup** w polu **Wprowadź nazwę obiektu do wybrania** wpisz **serverfqdn określa-** Managers, a następnie kliknij przycisk **OK**.

6. W oknie dialogowym **wpis uprawnienia dla contoso** , na liście **Zastosuj do** wybierz pozycję **obiekty zależnych użytkowników** , a następnie Włącz pole wyboru **Zezwalaj** dla następujących **uprawnień** :

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Inspekcja programu FIM CM**

    - **Agent rejestracji programu FIM CM**

    - **Rejestrowanie żądań programu FIM CM**

    - **Odzyskiwanie żądania programu FIM CM**

    - **Odnowienie żądania FIM CM**

    - **Żądanie odwołania FIM CM**

    - **Odblokowywanie karty inteligentnej przez żądanie programu FIM CM**

7. W oknie dialogowym **wpis uprawnienia dla contoso** kliknij przycisk **OK**.

8. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla contoso** kliknij przycisk **Dodaj**.

9. W oknie dialogowym **Wybieranie użytkowników, komputerów, kont usług lub grup** w polu **Wprowadź nazwę obiektu do wybrania** wpisz **serverfqdn określa-** help, a następnie kliknij przycisk **OK**.

10. W oknie dialogowym **wpis uprawnienia dla contoso** , na liście **Zastosuj do** wybierz pozycję **obiekty zależnych użytkowników** , a następnie zaznacz pole wyboru **Zezwalaj** dla następujących **uprawnień** :

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Agent rejestracji programu FIM CM**

    - **Żądanie odwołania FIM CM**

    - **Odblokowywanie karty inteligentnej przez żądanie programu FIM CM**

11. W oknie dialogowym **wpis uprawnienia dla contoso** kliknij przycisk **OK**.

12. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla contoso** kliknij przycisk **OK**.

13. W oknie dialogowym **właściwości contoso.com** kliknij przycisk **OK**.

14. Pozostaw otwarte **Active Directory Użytkownicy i komputery** .

**Konfigurowanie uprawnień do obiektów podrzędnych użytkownika \<no script\>**

1. Upewnij się, że nadal znajdują się w konsoli **Active Directory Użytkownicy i komputery** .

2. W drzewie konsoli kliknij prawym przyciskiem myszy pozycję **contoso.com** , a następnie kliknij polecenie **Właściwości**.

3. Na **zabezpieczeń** kliknij pozycję **Zaawansowane**.

4. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla contoso** kliknij przycisk **Dodaj**.

5. W oknie dialogowym **Wybieranie użytkowników, komputerów, kont usług lub grup** w polu **Wprowadź nazwę obiektu do wybrania** wpisz **serverfqdn określa-** Managers, a następnie kliknij przycisk **OK**.

6. W oknie dialogowym **wpis uprawnienia dla contoso** , na liście **Zastosuj do** wybierz pozycję **obiekty zależnych użytkowników** , a następnie Włącz pole wyboru **Zezwalaj** dla następujących **uprawnień** :

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Inspekcja programu FIM CM**

    - **Agent rejestracji programu FIM CM**

    - **Rejestrowanie żądań programu FIM CM**

    - **Odzyskiwanie żądania programu FIM CM**

    - **Odnowienie żądania FIM CM**

    - **Żądanie odwołania FIM CM**

    - **Odblokowywanie karty inteligentnej przez żądanie programu FIM CM**

7. W oknie dialogowym **wpis uprawnienia dla contoso** kliknij przycisk **OK**.

8. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla contoso** kliknij przycisk **Dodaj**.

9. W oknie dialogowym **Wybieranie użytkowników, komputerów, kont usług lub grup** w polu **Wprowadź nazwę obiektu do wybrania** wpisz **serverfqdn określa-** help, a następnie kliknij przycisk **OK**.

10. W oknie dialogowym **wpis uprawnienia dla contoso** , na liście **Zastosuj do** wybierz pozycję **obiekty zależnych użytkowników** , a następnie zaznacz pole wyboru **Zezwalaj** dla następujących **uprawnień** :

    - **Odczyt wszystkich właściwości**

    - **Uprawnienia do odczytu**

    - **Agent rejestracji programu FIM CM**

    - **Żądanie odwołania FIM CM**

    - **Odblokowywanie karty inteligentnej przez żądanie programu FIM CM**

11. W oknie dialogowym **wpis uprawnienia dla contoso** kliknij przycisk **OK**.

12. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla contoso** kliknij przycisk **OK**.

13. W oknie dialogowym **właściwości contoso.com** kliknij przycisk **OK**.

14. Pozostaw otwarte **Active Directory Użytkownicy i komputery** .

Druga procedura: **delegowanie uprawnień \<script\> zarządzania szablonami certyfikatów**

- Delegowanie uprawnień do kontenera szablonów certyfikatów.

- Delegowanie uprawnień do kontenera OID.

- Delegowanie uprawnień do istniejących szablonów certyfikatów.

Zdefiniuj uprawnienia do kontenera szablonów certyfikatów:

1. Przywróć konsolę **Active Directory Lokacje i usługi** .

2. W drzewie konsoli rozwiń węzeł **usługi** , rozwiń węzeł **usługi klucza publicznego** , a następnie kliknij pozycję **Szablony certyfikatów**.

3. W drzewie konsoli kliknij prawym przyciskiem myszy pozycję **Szablony certyfikatów** , a następnie kliknij polecenie **Deleguj kontrolę**.

4. W kreatorze **delegowania kontroli** kliknij przycisk **dalej**.

5. Na stronie **Użytkownicy lub grupy** kliknij przycisk **Dodaj**.

6. W oknie dialogowym **Wybieranie użytkowników, komputerów lub grup** w polu **Wprowadź nazwy obiektów do wybrania** wpisz **serverfqdn określa-** Managers, a następnie kliknij przycisk **OK**.

7. Na stronie **Użytkownicy lub grupy** kliknij przycisk **dalej**.

8. Na stronie **zadania do delegowania** kliknij pozycję **Utwórz niestandardowe zadanie do delegowania** , a następnie kliknij przycisk **dalej**.

9.  Na stronie **Typ obiektu Active Directory** upewnij się, że **ten folder, istniejące obiekty w tym folderze i Utwórz nowe obiekty w tym** folderze jest zaznaczone, a następnie kliknij przycisk **dalej**.

10. Na stronie **uprawnienia** na liście **uprawnienia** zaznacz pole wyboru **pełna kontrola** , a następnie kliknij przycisk **dalej**.

11. Na stronie **Kończenie pracy Kreatora delegowania kontroli** kliknij przycisk **Zakończ**.

Zdefiniuj uprawnienia do kontenera OID:

1. W drzewie konsoli kliknij prawym przyciskiem myszy pozycję **OID** , a następnie kliknij polecenie **Właściwości**.

2. W oknie dialogowym **Właściwości identyfikatora OID** na karcie **zabezpieczenia** kliknij przycisk **Zaawansowane**.

3. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla identyfikatora OID** kliknij przycisk **Dodaj**.

4. W oknie dialogowym **Wybieranie użytkowników, komputerów, kont usług lub grup** w polu **Wprowadź nazwę obiektu do wybrania** wpisz **serverfqdn określa-** Managers, a następnie kliknij przycisk **OK**.

5. W oknie dialogowym **wpis uprawnień dla identyfikatora OID** upewnij się, że uprawnienia dotyczą **tego obiektu i wszystkich obiektów zależnych** , kliknij przycisk **pełna kontrola** , a następnie kliknij przycisk **OK**.

6. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla identyfikatora OID** kliknij przycisk **OK**.

7. W oknie dialogowym **Właściwości identyfikatora OID** kliknij przycisk **OK**.

8. Zamknij **Active Directory Lokacje i usługi**.

**Skrypty: uprawnienia do identyfikatorów OID, szablon profilu & kontener szablonów certyfikatów**

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

**Skrypty: delegowanie uprawnień do istniejących szablonów certyfikatów.**  

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
