---
title: "Wdrażanie usługi Privileged Access Management programu MIM w systemie Windows Server 2016 | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące wdrażania usługi Privileged Access Management w systemie Windows Server 2016"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/08/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.openlocfilehash: fbdebd59249667a0e60d3a248f183bcb6a75085a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="deploy-mim-pam-with-windows-server-2016"></a>Wdrażanie usługi PAM programu MIM w systemie Windows Server 2016


W tym scenariuszu program MIM 2016 SP1 może korzystać z funkcji systemu Windows Server 2016 w roli kontrolera domeny dla lasu „PRIV”.  Po skonfigurowaniu tego scenariusza bilet protokołu Kerberos użytkownika zostanie ograniczony w czasie do pozostałego czasu aktywacji roli. 

>[!Note]
Z tą wersją programu MIM nie można używać wersji przeglądowych systemu Windows Server 2016 wcześniejszych od wersji Technical Preview 5.

## <a name="preparation"></a>Przygotowanie

Dla środowiska laboratorium wymagane są przynajmniej dwie maszyny wirtualne:

-   Maszyna wirtualna jest hostem dla kontrolera domeny PRIV z uruchomionym systemem Windows Server 2016

-   Maszyna wirtualna jest hostem dla usługi MIM z uruchomionym systemem Windows Server 2016 (zalecane) lub Windows Server 2012 R2

>[!NOTE]
Jeśli nie ma już domeny „CORP” w środowisku laboratorium, dla tej domeny wymagany jest dodatkowy kontroler. Kontroler domeny „CORP” może obsługiwać system Windows Server 2016 lub Windows Server 2012 R2.


Wykonaj instalację zgodnie z opisem w artykule [Przewodnik z wprowadzeniem](privileged-identity-management-for-active-directory-domain-services.md) **z wyjątkiem sytuacji wskazanych poniżej**:

-   Podczas tworzenia nowej domeny CORP w trakcie wypełniania instrukcji w sekcji [Krok 1 — Przygotowanie kontrolera domeny CORP](step-1-prepare-corp-domain.md) można wybrać opcjonalną konfigurację domeny CORP do działania na poziomie funkcjonalności systemu Windows Server 2016. **Jeśli wybierzesz tę opcję, wprowadź następujące zmiany**:

    -   Jeśli używasz nośników systemu Windows Server 2016, opcja instalacji będzie miała nazwę Windows Server 2016 (serwer ze środowiskiem pulpitu).

    -   Poziom funkcjonalności systemu Windows Server 2016 dla lasu i domeny CORP można określić, podając 7 jako numer wersji domeny i lasu w argumencie polecenia Install-ADDSForest w następujący sposób:
     ```
        Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
        ```
    -   W poleceniu „Utwórz nowych użytkowników i grupy” polecenie końcowe (New-ADGroup -name 'CONTOSO\$\$\$' …) **nie jest wymagane, gdy kontrolery domen CORP i PRIV działają na poziomie funkcjonalności domeny systemu Windows Server 2016**.

    -   Zmiany opisane w instrukcjach „Konfigurowanie inspekcji” (element nr 8) i „Konfigurowanie ustawień rejestru” (element nr 10) są **zalecane, ale nie wymagane**, gdy kontrolery domen CORP i PRIV działają na poziomie funkcjonalności domeny systemu Windows Server 2016.

-   Jeśli zdecydujesz się używać Windows Server 2012 R2 jako systemu operacyjnego dla kontrolera domeny CORP, musisz zainstalować poprawki 2919442, 2919355 [i zaktualizować poprawkę 3155495](http://support.microsoft.com/kb/3156418) na kontrolerze domeny CORP.

-   Wypełnij instrukcje w sekcji [Krok 2 — Przygotowanie kontrolera domeny PRIV](step-2-prepare-priv-domain-controller.md) z wyłączeniem następujących zmian:

    -   Zainstaluj przy użyciu nośników systemu Windows Server 2016. Opcja instalacji będzie miała nazwę Windows Server 2016 (serwer ze środowiskiem pulpitu).

    -   W instrukcjach „Dodawanie ról” (element nr 4) **należy w czwartym wierszu poleceń programu PowerShell określić numery wersji domeny i lasu, używając cyfry 7**, aby zezwolić na włączenie funkcji usługi AD systemu Windows Server opisanych w dalszej części.

        ```
        Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
        ```  

    -   Podczas konfigurowania uprawnień inspekcji i logowania należy pamiętać, że program Zarządzanie zasadami grupy będzie znajdować się w folderze Narzędzia administracyjne systemu Windows.

    -   Konfigurowanie ustawień rejestru wymaganych w przypadku migracji historii SID (element nr 8) **nie jest wymagane, jeśli domena PRIV działa na poziomie funkcjonalności domeny systemu Windows Server 2016**.

    -   Po skonfigurowaniu delegowania i przed ponownym uruchomieniem serwera włącz funkcje usługi Privileged Access Management w usłudze Windows Server 2016 Active Directory, uruchamiając okno programu PowerShell jako administrator i wpisując następujące polecenia.

    ```
    $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
    Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
    ```

  -   Po skonfigurowaniu delegowania i przed ponownym uruchomieniem serwera autoryzuj administratorów programu MIM oraz konto usługi programu MIM do tworzenia i aktualizowania podmiotów zabezpieczeń w tle.

     a. Uruchom okno programu Powershell i wpisz polecenie ADSIEdit.

     b. Otwórz menu Akcje i kliknij pozycję „Połącz z”. W ramach ustawienia punktu połączenia zmień kontekst nazewnictwa „Domyślny kontekst nazewnictwa” na wartość „Konfiguracja” i kliknij przycisk OK.

     c. Po nawiązaniu połączenia po lewej stronie okna pod pozycją „Edytor interfejsu ADSI” rozwiń węzeł konfiguracji, aby wyświetlić pozycje „CN=Konfiguracja,DC=priv,....”. Rozwiń pozycję CN=Konfiguracja, a następnie rozwiń pozycję CN=Usługi.

     d. Kliknij prawym przyciskiem myszy pozycję „CN=Konfiguracja podmiotu zabezpieczeń w tle” i kliknij pozycję Właściwości. Gdy zostanie wyświetlone okno dialogowe Właściwości, przejdź na kartę zabezpieczeń.

     e. Kliknij przycisk Dodaj. Określ konta jako „MIMService” oraz innych administratorów MIM, którzy później wykonają polecenie New-PAMGroup w celu utworzenia dodatkowych grup usługi PAM. Dla każdego użytkownika na liście dozwolonych uprawnień dodaj uprawnienia „Zapisywanie”, „Tworzenie wszystkich obiektów podrzędnych” i „Usuwanie wszystkich obiektów podrzędnych”. Dodaj uprawnienia.

     f. Zmień na zaawansowane ustawienia zabezpieczeń. W wierszu zezwalającym na dostęp MIMService kliknij przycisk Edytuj. Zmień ustawienie „Dotyczy” na „tego obiektu i wszystkich obiektów podrzędnych”. Zaktualizuj to ustawienie uprawnień i zamknij okno dialogowe zabezpieczeń.

     g. Zamknij Edytor interfejsu ADSI.

 -   Po skonfigurowaniu delegowania i przed ponownym uruchomieniem serwera autoryzuj administratorów programu MIM do tworzenia i aktualizowania zasad uwierzytelniania.

     a.  Uruchom **wiersz polecenia** z podwyższonym poziomem uprawnień i wpisz następujące polecenia, podstawiając we wszystkich czterech wierszach nazwę konta administratora programu MIM w miejsce elementu „mimadmin”:
    ```
       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicy

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


-   Wypełnij instrukcje w sekcji [Krok 3 — Przygotowanie serwera PAM](step-3-prepare-pam-server.md) i wprowadź te zmiany.

    -   W przypadku instalacji w systemie Windows Server 2016 należy pamiętać, że rola „ApplicationServer” nie jest dostępna.

    -   W przypadku instalowania programu MIM w systemie Windows Server 2016 **instalacja programu SharePoint 2013 nie jest możliwa**.

-   Wypełnij instrukcje w sekcji [Krok 4 – Instalowanie składników programu MIM na serwerze i stacji roboczej usługi PAM](step-4-install-mim-components-on-pam-server.md) i wprowadź te zmiany.

    -   Użytkownik instalujący usługę programu MIM i składniki usługi PAM **musi mieć dostęp do zapisu w domenie PRIV w usłudze AD**, gdyż instalacja programu MIM powoduje utworzenie nowej jednostki organizacyjnej usługi AD „Obiekty usługi PAM”.

    -   Jeśli nie zainstalowano programu SharePoint, nie instaluj portalu MIM.

-   Wypełnij instrukcje w sekcji [Krok 5 — Ustanowienie zaufania](step-5-establish-trust-between-priv-corp-forests.md) i wprowadź te zmiany:

    -   Podczas ustanawiania zaufania jednokierunkowego wykonaj tylko pierwsze dwa polecenia programu PowerShell (get-credential i New-PAMTrust), **nie wykonuj polecenia New-PAMDomainConfiguration**.

    -   Po ustanowieniu zaufania zaloguj się do kontrolera domeny PRIV jako PRIV\\Administrator, uruchom program PowerShell i wpisz następujące polecenia:
  ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1

     netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
     /usero:contoso\\administrator /passwordo:Pass\@word1  

     netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1
  ```

-   Element nr 5 (weryfikacja zaufania) **nie jest wymagany, gdy domeny CORP i PRIV działają na poziomie funkcjonalności domeny systemu Windows Server 2016**.

## <a name="more-information"></a>Więcej informacji

- [Privileged Access Management for Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) (Usługa Privileged Access Management dla usług Active Directory Domain Services)
- [Configure the MIM environment for Privileged Access Management](configuring-mim-environment-for-pam.md) (Konfigurowanie środowiska programu MIM na potrzeby usługi Privileged Access Management)
- [Konfiguracja usługi PAM za pomocą skryptów](sp1-pam-configure-using-scripts.md)
