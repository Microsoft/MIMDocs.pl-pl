---
title: Skonfiguruj kont gMSA dla Microsoft Identity Manager 2016 | Microsoft Docs
description: Konfigurowanie kont usług zarządzanych przez grupę w domenie dla Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: be5dc1e8615f56d3157a78891e80897e446eafab
ms.sourcegitcommit: 32c7a46b2f8ed3f2f9ebc6f79a4ecb0019fe62e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2020
ms.locfileid: "77527926"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>Skonfiguruj domenę dla scenariusza kont usług zarządzanych przez grupę (gMSA)

> [!div class="step-by-step"]
> [System Windows Server»](prepare-server-ws2016.md)

> [!IMPORTANT]
> Ten artykuł ma zastosowanie tylko do programu MIM 2016 SP2.

Program Microsoft Identity Manager (MIM) współpracuje z Twoją domeną usługi Active Directory (AD). Usługa AD powinna już być zainstalowana, a w środowisku musi istnieć kontroler dla domeny, którą możesz administrować.  W tym artykule opisano sposób konfigurowania kont usług zarządzanych przez grupę w tej domenie do użycia przez program MIM.

## <a name="overview"></a>Przegląd

Konta usług zarządzane przez grupę eliminują konieczność okresowego zmieniania haseł kont usług. W wersji programu MIM 2016 SP2 następujące składniki programu MIM mogą mieć skonfigurowane konta gMSA do użycia podczas procesu instalacji:

-   Usługa synchronizacji programu MIM (FIMSynchronizationService)
-   Usługa MIM (usługi FIMService)
-   Pula aplikacji witryny sieci Web do rejestracji haseł programu MIM
-   Pula aplikacji witryny sieci Web do resetowania hasła programu MIM
-   Pula aplikacji sieci Web interfejsu API REST usługi PAM
-   Usługa monitorowania PAM (PamMonitoringService)
-   Usługa składnika PAM (PrivilegeManagementComponentService)

Następujące składniki programu MIM nie obsługują uruchamiania jako konta gMSA:

-   Portal programu MIM. Dzieje się tak, ponieważ Portal programu MIM jest częścią środowiska SharePoint. Zamiast tego można wdrożyć program SharePoint w trybie farmy i [skonfigurować automatyczną zmianę hasła w programie SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Wszyscy agenci zarządzania
-   Zarządzanie certyfikatami firmy Microsoft
-   BHOLD


Więcej informacji na temat gMSA można znaleźć w następujących artykułach:
-   [Omówienie kont usług zarządzanych przez grupę](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [Tworzenie klucza głównego KDS usług dystrybucji kluczy](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>Tworzenie grup i kont użytkowników

Wszystkie składniki wdrożenia programu MIM muszą mieć własną tożsamość w domenie. Dotyczy to między innymi składników programu MIM, takich jak Service i Sync, a także SharePoint i SQL.


> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **DC**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi programu MIM — **mimservice**
> - Nazwa serwera synchronizacji programu MIM — **mimsync**
> - Nazwa SQL Server — **SQL**
> - Hasło — <strong>Pass@word1</strong>

1. Zaloguj się do kontrolera domeny jako administrator domeny (*np. Contoso\Administrator*).

2. Utwórz następujące konta użytkowników dla usług programu MIM. Uruchom program PowerShell i wpisz następujący skrypt programu PowerShell, aby utworzyć nowych użytkowników domeny usługi Active Directory (nie wszystkie konta są obowiązkowe, chociaż skrypt jest dostarczany tylko do celów informacyjnych, najlepszym rozwiązaniem jest użycie dedykowanego konta *MIMAdmin* dla programu MIM i procesu instalacji programu SharePoint).

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Utwórz grupy zabezpieczeń dla wszystkich grup.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  Dodaj nazwy SPN, aby włączyć uwierzytelnianie Kerberos dla kont usług.

    ```PowerShell
    setspn -S http/mim.contoso.com contoso\svcMIMAppPool
    ```

5.  Upewnij się, że rejestrujesz następujące rekordy "A" DNS w celu rozpoznawania nazw (przy założeniu, że usługa MIM, Portal programu MIM, Resetowanie hasła i witryny sieci Web rejestracji haseł będą hostowane na tym samym komputerze)

    - adres IP usługi programu MIM i serwera portalu mim.contoso.com
    - adres IP usługi programu MIM i serwera portalu passwordreset.contoso.com
    - adres IP usługi programu MIM i serwera portalu passwordregistration.contoso.com

## <a name="create-key-distribution-service-root-key"></a>Utwórz klucz główny usługi dystrybucji kluczy

Upewnij się, że zalogowano się do kontrolera domeny jako administrator, aby przygotować usługę dystrybucji kluczy grupy.

Jeśli istnieje już klucz główny dla domeny (Użyj polecenie **Get-KdsRootKey** do sprawdzenia), a następnie przejdź do następnej sekcji.

6.  W razie konieczności Utwórz klucz główny usługi dystrybucji kluczy (KDS) (tylko raz na domenę). Klucz główny jest używany przez usługę KDS na kontrolerach domeny (wraz z innymi informacjami) do generowania haseł. Jako administrator domeny wpisz następujące polecenie programu PowerShell:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *– EffectiveImmediately* może wymagać opóźnienia do \~10 godzin, ponieważ będzie musiał replikować do wszystkich kontrolerów domeny. To opóźnienie miało około 1 godzinę w przypadku dwóch kontrolerów domeny.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >W środowisku laboratoryjnym lub testowym można uniknąć opóźnień replikacji o 10 godzin, uruchamiając następujące polecenie zamiast:
    ><br/>
    >Add-KDSRootKey-EffectiveTime ((Get-Date). Addgodz. (-10))

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>Utwórz konto usługi synchronizacji programu MIM, grupę i nazwę główną usługi
-----------------------

Upewnij się, że wszystkie konta komputerów, na których jest zainstalowane oprogramowanie MIM, są już przyłączone do domeny.  Następnie wykonaj te kroki w programie PowerShell jako administrator domeny.

7.  Utwórz grupę *MIMSync_Servers* i Dodaj do niej wszystkie serwery synchronizacji programu MIM.
    Wpisz następujące, aby utworzyć nową grupę usługi AD dla serwerów synchronizacji programu MIM. Następnie Dodaj serwer synchronizacji programu MIM Active Directory konta komputera, np. *contoso\MIMSync $* , do tej grupy.

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  Utwórz gMSA usługi synchronizacji programu MIM. Wpisz następujące środowisko PowerShell.

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Sprawdź szczegóły GSMA utworzonego przez wykonanie polecenia programu PowerShell *Get-ADServiceAccount* :

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  Jeśli planujesz uruchomić usługę powiadamiania o zmianie hasła, musisz zarejestrować nazwę główną usługi, wykonując następujące polecenie programu PowerShell:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. Uruchom ponownie serwer synchronizacji programu MIM, aby odświeżyć token Kerberos skojarzony z serwerem, ponieważ członkostwo w grupie "MIMSync_Server" zostało zmienione.

## <a name="create-mim-service-management-agent-service-account"></a>Utwórz konto usługi agenta zarządzania usługą MIM

11. Zwykle podczas instalowania usługi programu MIM zostanie utworzone nowe konto dla agenta zarządzania usługą programu MIM (konto usługi MIM).  W przypadku gMSA dostępne są dwie opcje:

- Użyj konta usługi zarządzanego przez grupę usługi synchronizacji programu MIM i nie twórz oddzielnego konta

    Możesz pominąć tworzenie konta usługi agenta zarządzania usługą programu MIM. W takim przypadku należy użyć nazwy gMSA usługi synchronizacji programu MIM, np. *contoso\MIMSyncGMSAsvc $* , zamiast konta w programie MIM podczas instalacji usługi MIM. Później w obszarze Konfiguracja agenta zarządzania usługą MIM Włącz opcję *Użyj konta MIMSync* .

    Nie włączaj "Odmowa logowania z sieci" dla usługi synchronizacji programu MIM gMSA, ponieważ uprawnienie "Zezwalaj na logowanie do sieci" w programie MIM jest wymagane.

- Użyj zwykłego konta usługi dla konta usługi agenta zarządzania usługą programu MIM

    Uruchom program PowerShell jako administrator domeny i wpisz następujące polecenia, aby utworzyć nowego użytkownika domeny usługi Active Directory:

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    Nie włączaj "Odmowa logowania z sieci" dla konta usługi MIM, ponieważ wymaga uprawnienia "Zezwalaj na logowanie do sieci".

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>Tworzenie kont usługi programu MIM, grup i nazwy głównej usługi

Kontynuuj korzystanie z programu PowerShell jako administrator domeny.
   
12. Utwórz grupę *MIMService_Servers* i Dodaj do niej wszystkie serwery usługi programu MIM.  Wpisz następujące polecenie programu PowerShell, aby utworzyć nową grupę usługi AD dla serwerów usługi programu MIM i dodać serwer usługi MIM Active Directory konto komputera, np. *contoso\MIMPortal $* , w tej grupie.

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  Utwórz usługę MIM gMSA.

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  Zarejestrowanie nazwy głównej usługi i włączenie delegowania protokołu Kerberos przez wykonanie tego polecenia programu PowerShell:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  W przypadku scenariuszy SSPR wymagane konto usługi programu MIM może komunikować się z usługą synchronizacji programu MIM, dlatego konto usługi programu MIM musi być członkiem usługi MIMSyncAdministrators lub resetowania hasła synchronizacji programu MIM i przeglądać grupy:

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. Uruchom ponownie serwer usługi programu MIM w celu odświeżenia tokenu Kerberos skojarzonego z serwerem, ponieważ członkostwo w grupie "MIMService_Servers" zostało zmienione.

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>W razie konieczności Utwórz inne konta i grupy programu MIM

W przypadku konfigurowania programu MIM SSPR, postępując zgodnie z tymi samymi wskazówkami opisanymi powyżej dla usługi synchronizacji programu MIM i usługi MIM, można utworzyć inne gMSA dla:

- Pula aplikacji witryny sieci Web do resetowania hasła programu MIM
- Pula aplikacji witryny sieci Web do rejestracji haseł programu MIM

W przypadku konfigurowania usługi PAM programu MIM, postępując zgodnie z tymi samymi wskazówkami, jak opisano powyżej w temacie usługa synchronizacji programu MIM i usługa MIM, można utworzyć inne gMSA dla:

- Pula aplikacji sieci Web interfejsu API REST usługi PAM programu MIM
- Usługa składnika PAM programu MIM
- Usługa monitorowania PAM programu MIM

## <a name="specifying-a-gmsa-when-installing-mim"></a>Określanie gMSA podczas instalowania programu MIM

Jako ogólna zasada, w większości przypadków podczas korzystania z Instalatora programu MIM, aby określić, że chcesz użyć gMSA zamiast zwykłego konta, Dołącz znak dolara do nazwy gMSA, np. **contoso\MIMSyncGMSAsvc $** , i pozostaw pole hasła puste. Jedynym wyjątkiem jest narzędzie *miisactivate. exe* , które akceptuje nazwę gMSA bez znaku dolara.
<br/>

> [!div class="step-by-step"]
> [System Windows Server»](prepare-server-ws2016.md)
