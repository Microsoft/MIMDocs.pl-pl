---
title: Wdrożenie usługi PAM — krok 1 — domena CORP | Dokumentacja firmy Microsoft
description: Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0c2261ba908ecd3bef991d5b98efbcb6bf56bedd
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333568"
---
# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>Krok 1 — Przygotowanie hosta i domeny CORP

> [!div class="step-by-step"]
> [Krok 2 »](step-2-prepare-priv-domain-controller.md)

W tym kroku przygotujesz się do hostowania środowiska bastionu. Ponadto, jeśli jest to konieczne, utworzysz kontroler domeny i członkowską stację roboczą w nowej domenie i lesie (las *CORP*) z tożsamościami do zarządzania przy użyciu środowiska bastionu. Las CORP symuluje istniejący las, który ma zasoby do zarządzania. Ten dokument zawiera przykładowy zasób do ochrony: udział plików.

Jeśli masz już istniejącą domenę usługi Active Directory (AD) z kontrolerem domeny z systemem Windows Server 2012 R2 lub nowszym, w której jesteś administratorem domeny, możesz użyć tej domeny.  

## <a name="prepare-the-corp-domain-controller"></a>Przygotowanie kontrolera domeny CORP

Niniejsza sekcja opisuje sposób skonfigurowania kontrolera domeny dla domeny CORP. W domenie CORP użytkownicy administracyjni są zarządzani przez środowisko bastionu. Nazwa systemu nazw domen (DNS) domeny CORP używana w tym przykładzie to *contoso.local*.

### <a name="install-windows-server"></a>Instalacja systemu Windows Server

Zainstaluj system Windows Server 2012 R2 lub Windows Server 2016 Technical Preview 4 (lub nowszy) na maszynie wirtualnej, aby utworzyć komputer o nazwie *CORPDC*.

1. Wybierz **Windows Server 2012 R2 Standard (serwer z graficznym interfejsem użytkownika) x64** lub **Windows Server 2016 Technical Preview (serwer ze środowiskiem pulpitu)**.

2. Przeczytaj i zaakceptuj postanowienia licencyjne.

3. Ponieważ dysk będzie pusty, wybierz opcję **Niestandardowa: tylko zainstaluj system Windows** i użyj niezainicjowanego miejsca na dysku.

4. Zaloguj się na nowym komputerze jako administrator. Przejdź do Panelu sterowania. Ustaw nazwę komputera na *CORPDC* i nadaj mu statyczny adres IP w sieci wirtualnej. Uruchom ponownie serwer.

5. Po ponownym uruchomieniu serwera zaloguj się jako administrator. Przejdź do Panelu sterowania. Skonfiguruj komputer tak, aby sprawdzał dostępność aktualizacji, a następnie zainstaluj wszystkie wymagane aktualizacje. Uruchom ponownie serwer.

### <a name="add-roles-to-establish-a-domain-controller"></a>Dodawanie ról w celu ustanowienia kontrolera domeny

W tej sekcji dodasz role usług domenowych Active Directory (AD DS), serwera DNS i serwera plików (część sekcji Usługi plików i magazynu) i podwyższysz poziom tego serwera do kontrolera domeny nowego lasu contoso.local.

> [!NOTE]  
> Jeśli masz już domenę do użycia w formie domeny CORP, a domena ta używa systemu Windows Server 2012 R2 lub nowszego jako kontrolera domeny, możesz przejść do kroku [Tworzenie dodatkowych użytkowników i grup demonstracyjnych](#create-additional-users-and-groups-for-demonstration-purposes).

1. Uruchom program PowerShell po zalogowaniu się na konto administratora.

2. Wpisz następujące polecenia.

   ```PowoerShell
   import-module ServerManager

   Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

   Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
   ```

   Spowoduje to wyświetlenie monitu o podanie hasła administratora trybu awaryjnego. Należy pamiętać, że pojawią się komunikaty ostrzegawcze dotyczące delegowania DNS i ustawień kryptograficznych. Jest to normalne zachowanie.

3. Wyloguj się po utworzeniu lasu. Serwer zostanie automatycznie uruchomiony ponownie.

4. Po ponownym uruchomieniu serwera zaloguj się do komputera CORPDC jako administrator domeny. Zazwyczaj jest to użytkownik CONTOSO\\Administrator, który korzysta z hasła utworzonego podczas instalacji systemu Windows na komputerze CORPDC.

### <a name="create-a-group"></a>Tworzenie grupy

Utwórz grupę na potrzeby inspekcji przy użyciu usługi Active Directory, jeśli grupa jeszcze nie istnieje. Nazwa grupy musi być nazwą NetBIOS domeny z trzema znakami dolara, np. *CONTOSO$$$*.

Dla każdej domeny zaloguj się do kontrolera domeny jako administrator domeny i wykonaj następujące czynności:

1. Uruchom program PowerShell.

2. Wpisz poniższe polecenia, zastępując „CONTOSO” nazwą NetBIOS domeny.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
   ```

W niektórych przypadkach grupa może już istnieć — jest to normalne, jeśli domena była już używana w scenariuszach związanych z migracją usługi AD.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>Tworzenie dodatkowych użytkowników i grup demonstracyjnych

Jeśli utworzono nową domenę CORP, należy utworzyć dodatkowych użytkowników i grupy na potrzeby demonstrowania scenariusza PAM. Użytkownicy i grupy demonstracyjne nie powinny być administratorami domeny lub użytkownikami i grupami kontrolowanymi przez ustawienia adminSDHolder w usłudze AD.

> [!NOTE]
> Jeśli masz już domenę do użycia w formie domeny CORP, a domena zawiera użytkownika i grupę, których można użyć w celach demonstracyjnych, możesz przejść do sekcji [Konfiguracja inspekcji](#configure-auditing).

Zamierzamy utworzyć grupę zabezpieczeń o nazwie *CorpAdmins* i użytkownika o nazwie *Jen*. W razie potrzeby możesz użyć innych nazw.

1. Uruchom program PowerShell.

2. Wpisz następujące polecenia. Zamień hasło „Pass@word1” na inne.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

   New-ADUser –SamAccountName Jen –name Jen

   Add-ADGroupMember –identity CorpAdmins –Members Jen

   $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   Set-ADAccountPassword –identity Jen –NewPassword $jp

   Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
   ```

### <a name="configure-auditing"></a>Konfiguracja inspekcji

Musisz włączyć inspekcję w istniejących lasach, aby ustalić konfigurację PAM w tych lasach.  

Dla każdej domeny zaloguj się do kontrolera domeny jako administrator domeny i wykonaj następujące czynności:

1. Przejdź do pozycji **Start** > **Narzędzia administracyjne** (lub w systemie Windows Server 2016 **Narzędzia administracyjne systemu Windows**), a następnie uruchom **Zarządzanie zasadami grupy**.

2. Przejdź do zasad kontrolerów domeny dla tej domeny.  Jeśli utworzono nową domenę dla lasu contoso.local, przejdź do pozycji **Forest: contoso.local** > **Domeny** > **contoso.local** > **Kontrolery domeny** > **Domyślne zasady kontrolerów domeny**. Zostanie wyświetlony komunikat informacyjny.

3. Kliknij prawym przyciskiem myszy pozycję **Domyślne zasady kontrolerów domeny** i wybierz polecenie **Edytuj**. Zostanie wyświetlone nowe okno.

4. W oknie Edytor zarządzania zasadami grupy w drzewie Domyślne zasady kontrolerów domeny wybierz pozycję **Konfiguracja komputera** > **Zasady** > **Ustawienia systemu Windows** > **Ustawienia zabezpieczeń** > **Zasady lokalne** > **Zasady inspekcji**.

5. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Przeprowadź inspekcję zarządzania kontami**, a następnie wybierz polecenie **Właściwości**. Wybierz pozycję **Definiuj następujące ustawienia zasad**, zaznacz pole wyboru obok pozycji **Powodzenie**, zaznacz pole wyboru obok pozycji **Niepowodzenie**, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**.

6. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Przeprowadź inspekcję dostępu do usługi katalogowej**, a następnie wybierz polecenie **Właściwości**. Wybierz pozycję **Definiuj następujące ustawienia zasad**, zaznacz pole wyboru obok pozycji **Powodzenie**, zaznacz pole wyboru obok pozycji **Niepowodzenie**, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**.

7. Zamknij okno Edytor zarządzania zasadami grupy i okno Zarządzanie zasadami grupy.

8. Zastosuj ustawienia inspekcji, otwierając okno programu PowerShell i wpisując następujące polecenie:

   ```cmd
   gpupdate /force /target:computer
   ```

Po upływie kilku minut powinien zostać wyświetlony komunikat **Pomyślnie ukończono aktualizowanie zasad komputera**.

### <a name="configure-registry-settings"></a>Konfiguracja ustawień rejestru

W tej sekcji skonfigurujesz ustawienia rejestru wymagane do migracji sIDHistory, co zostanie wykorzystane podczas tworzenia grupy zarządzania dostępem uprzywilejowanym.

1. Uruchom program PowerShell.

2. Wpisz następujące polecenia, aby skonfigurować domenę źródłową i zezwolić na zdalny dostęp do wywołania procedury (RPC) do bazy danych menedżera kont zabezpieczeń (SAM).

   ```PowerShell
   New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

   Restart-Computer
   ```

Operacja spowoduje ponowne uruchomienie kontrolera domeny, CORPDC. Aby uzyskać więcej informacji na temat tego ustawienia rejestru, zobacz temat [Sposób rozwiązywania problemów związanych z migracją sIDHistory między lasami przy użyciu narzędzia ADMTv2](http://support.microsoft.com/kb/322970).

## <a name="prepare-a-corp-workstation-and-resource"></a>Przygotowanie stacji roboczej CORP i zasobów

Jeśli nie masz jeszcze komputera stacji roboczej podłączonego do domeny, wykonaj poniższe instrukcje, aby przygotować komputer.  

> [!NOTE]
> Jeśli masz już stację roboczą dołączoną do domeny, przejdź do kroku [Tworzenie zasobu demonstracyjnego](#create-a-resource-for-demonstration-purposes).

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>Instalowanie systemu Windows 8.1 lub Windows 10 Enterprise jako maszyny wirtualnej

Na kolejnej, nowej maszynie wirtualnej bez zainstalowanego oprogramowania zainstaluj system Windows 8.1 Enterprise lub Windows 10 Enterprise, aby utworzyć komputer o nazwie *CORPWKSTN*.

1. Użyj ustawień ekspresowych podczas instalacji.

2. Być może podczas instalacji połączenie z Internetem nie będzie możliwe. Wybierz opcję **Utwórz konto lokalne**. Podaj inną nazwę użytkownika — nie używaj nazw „Administrator” ani „Jen”.

3. Przy użyciu Panelu sterowania przypisz statyczny adres IP w sieci wirtualnej do tego komputera i ustaw preferowany serwer DNS interfejsu na serwer DNS komputera CORPDC.

4. Przy użyciu Panelu sterowania podłącz komputer CORPWKSTN do domeny contoso.local. Należy podać poświadczenia administratora domeny Contoso. Następnie, po zakończeniu tego procesu, uruchom ponownie komputer CORPWKSTN.

### <a name="create-a-resource-for-demonstration-purposes"></a>Tworzenie zasobu demonstracyjnego

Potrzebujesz zasobu w celach demonstracyjnych związanych z kontrolą dostępu opartą na grupach zabezpieczeń przy użyciu programu PAM.  Jeśli nie masz jeszcze zasobu, możesz użyć folderu plików w celach demonstracyjnych.  Spowoduje to użycie obiektów „Jen” i „CorpAdmins” usługi AD utworzonych w domenie contoso.local.

1. Nawiąż połączenie ze stacją roboczą CORPWKSTN. Kliknij ikonę **Przełącz użytkownika**, a następnie kliknij przycisk **Inny użytkownik**. Upewnij się, że użytkownik CONTOSO\\Jen może zalogować się do stacji CORPWKSTN.

2. Utwórz nowy folder o nazwie *CorpFS* i udostępnij go grupie *CorpAdmins*.

3. Uruchom program Windows PowerShell jako administrator.

4. Wpisz następujące polecenia.

   ```PowerShell
   mkdir c:\corpfs

   New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

   $acl = Get-Acl c:\corpfs

   $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

   $acl.SetAccessRule($car)

   Set-Acl c:\corpfs $acl
   ```

W następnym kroku należy przygotować kontroler domeny PRIV.

> [!div class="step-by-step"]
> [Krok 2 »](step-2-prepare-priv-domain-controller.md)
