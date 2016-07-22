---
title: Krok 2. Przygotowywanie kontrolera domeny PRIV | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/16/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 62d80222ea85fe5066cfa396b5e5a10bced4d3cd


---

# Krok 2. Przygotowywanie pierwszego kontrolera domeny PRIV

>[!div class="step-by-step"]
[« Krok 1](step-1-prepare-corp-domain.md)
[Krok 3 »](step-3-prepare-pam-server.md)

W tym kroku opisano tworzenie nowej domeny w celu udostępnienia środowiska bastionu na potrzeby uwierzytelniania administratorów.  W tym lesie będzie potrzebny co najmniej jeden kontroler domeny i jeden serwer członkowski. Serwer członkowski zostanie skonfigurowany w następnym kroku.

## Tworzenie nowego kontrolera domeny zarządzania dostępem uprzywilejowanym

W tej sekcji zostanie skonfigurowana maszyna wirtualna pełniąca funkcję kontrolera domeny dla nowego lasu.

### Instalowanie systemu Windows Server 2012 R2
Na innej nowej maszynie wirtualnej bez zainstalowanego oprogramowania zainstaluj system Windows Server 2012 R2, aby utworzyć komputer o nazwie „PRIVDC”.

1. Wybierz opcję wykonania niestandardowej instalacji (nie uaktualnienia) systemu Windows Server. Podczas instalacji wybierz opcję **Windows Server 2012 R2 Standard (serwer z graficznym interfejsem użytkownika) x64**. _Nie wybieraj opcji instalacji _**Data Center ani Server Core**.

2. Przeczytaj i zaakceptuj postanowienia licencyjne.

3. Ponieważ dysk będzie pusty, wybierz opcję **Niestandardowa: tylko zainstaluj system Windows** i użyj niezainicjowanego miejsca na dysku.

4. Po zainstalowaniu wersji systemu operacyjnego zaloguj się na nowym komputerze jako nowy administrator. Za pomocą Panelu sterowania ustaw nazwę komputera na *PRIVDC*, przypisz mu statyczny adres IP w sieci wirtualnej i skonfiguruj serwer DNS, ustawiając go na serwer DNS kontrolera domeny zainstalowanego w poprzednim kroku. Wymaga to ponownego uruchomienia serwera.

5. Po ponownym uruchomieniu serwera zaloguj się jako administrator. Za pomocą Panelu sterowania skonfiguruj komputer tak, aby sprawdzał dostępność aktualizacji, a następnie zainstaluj wszystkie wymagane aktualizacje. Może to wymagać ponownego uruchomienia serwera.

### Dodawanie ról
Dodaj role Usługi domenowe w usłudze Active Directory (AD DS) i Serwer DNS.

1. Uruchom program PowerShell jako administrator.

2. Wpisz poniższe polecenia w celu przygotowania instalacji usługi Active Directory systemu Windows Server.

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### Konfigurowanie ustawień rejestru dotyczących migracji historii identyfikatora SID

Uruchom program PowerShell i wpisz następujące polecenie, aby skonfigurować domenę źródłową pod kątem dostępu do bazy danych menedżera kont zabezpieczeń (Security Accounts Manager, SAM) za pośrednictwem zdalnego wywoływania procedur (Remote Procedure Call, RPC).

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## Tworzenie nowego lasu zarządzania dostępem uprzywilejowanym

Następnym krokiem jest podwyższenie poziomu serwera do poziomu kontrolera domeny nowego lasu.

Jako nazwa domeny nowego lasu w tym dokumencie jest używana nazwa priv.contoso.local.  Nazwa lasu nie ma znaczenia krytycznego i nie musi być podrzędna w stosunku do istniejącej nazwy lasu w organizacji. Jednak zarówno nazwa domeny, jak i nazwa NetBIOS nowego lasu muszą być unikatowe i różne od pozostałych domen w organizacji.  

### Tworzenie domeny i lasu

1. Aby utworzyć nową domenę, w oknie programu PowerShell wpisz następujące polecenia.  Spowoduje to również utworzenie delegowania DNS w domenie wyższego poziomu (contoso.local), która została utworzona w poprzednim kroku.  Jeśli zamierzasz później skonfigurować usługę DNS, pomiń parametry `CreateDNSDelegation -DNSDelegationCredential $ca`.

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. Gdy pojawi się okno podręczne, podaj poświadczenia administratora lasu CORP (np. nazwę użytkownika CONTOSO\\Administrator i hasło skonfigurowane w kroku 1).

3. W oknie programu PowerShell pojawi się monit o użycie hasła administratora trybu awaryjnego. Wprowadź dwa razy nowe hasło. Zostaną wyświetlone komunikaty ostrzegawcze dotyczące delegowania DNS i ustawień kryptograficznych. Jest to normalne działanie.

Po utworzeniu lasu serwer zostanie automatycznie uruchomiony ponownie.

### Tworzenie kont użytkowników i usług
Utwórz konta użytkowników i usługi w ramach konfigurowania usługi i portalu MIM. Te konta zostaną umieszczona w kontenerze Użytkownicy domeny priv.contoso.local.

1. Po ponownym uruchomieniu serwera zaloguj się do komputera PRIVDC jako administrator domeny (PRIV\\Administrator).

2. W programie PowerShell wpisz poniższe polecenia. Hasło „Pass@word1” jest przykładowe. Użyj innego hasła dla kont.

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### Konfigurowanie praw do inspekcji i logowania

Skonfigurowanie inspekcji pozwoli określić konfigurację usługi PAM między lasami.  

1. Sprawdź, czy zalogowano się jako administrator domeny (PRIV\\Administrator).

2. Wybierz kolejno pozycje **Start** > **Narzędzia administracyjne** > **Zarządzanie zasadami grupy**.

3. Wybierz kolejno pozycje **Las: priv.contoso.local** > **Domeny** > **priv.contoso.local** > **Kontrolery domeny** > **Domyślne zasady kontrolerów domeny**. Zostanie wyświetlony komunikat ostrzegawczy.

4. Kliknij prawym przyciskiem myszy pozycję **Domyślne zasady kontrolerów domeny** i wybierz polecenie **Edytuj**.

5. W drzewie konsoli Edytor zarządzania zasadami grupy wybierz kolejno pozycje **Konfiguracja komputera** > **Zasady** > **Ustawienia systemu Windows** > **Ustawienia zabezpieczeń** > **Zasady lokalne** > **Zasady inspekcji**.

6. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Przeprowadź inspekcję zarządzania kontami**, a następnie wybierz polecenie **Właściwości**. Kliknij pozycję **Definiuj następujące ustawienia zasad**, zaznacz pole wyboru **Sukces**, zaznacz pole wyboru **Niepowodzenie**, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**.

7. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Przeprowadź inspekcję dostępu do usługi katalogowej**, a następnie wybierz polecenie **Właściwości**. Kliknij pozycję **Definiuj następujące ustawienia zasad**, zaznacz pole wyboru **Sukces**, zaznacz pole wyboru **Niepowodzenie**, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**.

8. Wybierz kolejno pozycje **Konfiguracja komputera** > **Zasady** > **Ustawienia systemu Windows** > **Ustawienia zabezpieczeń** > **Zasady konta** > **Zasady protokołu Kerberos**.

9. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Maksymalny okres istnienia biletu użytkownika** i wybierz polecenie **Właściwości**. Kliknij pozycję **Definiuj następujące ustawienia zasad**, ustaw liczbę godzin na *1*, kliknij przycisk **Zastosuj**, a następnie kliknij przycisk **OK**. Zwróć uwagę na to, że inne ustawienia wyświetlane w tym oknie również się zmienią.

10. W oknie Zarządzanie zasadami grupy wybierz pozycję **Domyślne zasady domeny**, kliknij prawym przyciskiem myszy i wybierz polecenie **Edytuj**.

11. Rozwiń kolejno węzły **Konfiguracja komputera** > **Zasady** > **Ustawienia systemu Windows** > **Ustawienia zabezpieczeń** > **Zasady lokalne** i wybierz pozycję **Przypisywanie praw użytkownika**.

12. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Odmowa logowania w trybie wsadowym** i wybierz polecenie **Właściwości**.

13. Zaznacz pole wyboru **Definiuj następujące ustawienia zasad**, kliknij pozycję **Dodaj użytkownika lub grupę**, w polu 	Nazwy użytkowników i grup wpisz *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* i kliknij przycisk **OK**.

14. Kliknij przycisk **OK**, aby zamknąć okno.

15. W okienku szczegółów kliknij prawym przyciskiem myszy pozycję **Odmawiaj logowania za pomocą usług pulpitu zdalnego** i wybierz polecenie **Właściwości**.

16. Kliknij pole wyboru **Definiuj następujące ustawienia zasad**, kliknij pozycję **Dodaj użytkownika lub grupę**, w polu 	Nazwy użytkowników i grup wpisz *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* i kliknij przycisk **OK**.

17. Kliknij przycisk **OK**, aby zamknąć okno.

18. Zamknij okno Edytor zarządzania zasadami grupy i okno Zarządzanie zasadami grupy.

19. Uruchom okno programu PowerShell jako administrator i wpisz poniższe polecenie, aby zaktualizować kontroler domeny przy użyciu ustawień zasad grupy.

  ```
  gpupdate /force /target:computer
  ```

  Po upływie około minuty proces zostanie zakończony i zostanie wyświetlony komunikat „Aktualizacja zasad komputera została ukończona pomyślnie”.


### Konfigurowanie przekierowywania nazw DNS na komputerze PRIVDC

Przy użyciu programu PowerShell skonfiguruj przekierowywanie nazw DNS na komputerze PRIVDC, aby zapewnić rozpoznawanie innych istniejących lasów przez domenę PRIV.

1. Uruchom program PowerShell.

2. Dla domen znajdujących się na początku wszystkich istniejących lasów wpisz następujące polecenie, określając istniejącą domenę DNS (np. contoso.local) i adres IP serwera głównego tej domeny.  

  Jeśli w poprzednim kroku utworzono jedną domenę contoso.local, podaj wartość *10.1.1.31* jako adres IP komputera CORPDC w sieci wirtualnej.

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE] 
> Inne lasy również muszą mieć możliwość przesyłania zapytań DNS dotyczących lasu PRIV do tego kontrolera domeny.  Jeśli istnieje wiele lasów usługi Active Directory, musisz dodać usługę DNS warunkowego przesyłania dalej do każdego z tych lasów.

### Konfigurowanie protokołu Kerberos

1. Przy użyciu programu PowerShell dodaj główne nazwy usługi (Service Principal Name, SPN), aby umożliwić programowi SharePoint, interfejsowi API REST usługi PAM i usłudze MIM korzystanie z uwierzytelniania za pośrednictwem protokołu Kerberos.

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE] 
> W następnych krokach opisano instalację składników serwera programu MIM 2016 na pojedynczym komputerze. Jeśli planujesz dodanie kolejnego serwera w celu zwiększenia dostępności, musisz wykonać dodatkowe czynności w ramach konfigurowania protokołu Kerberos. Zostało to opisane w artykule [FIM 2010: Kerberos Authentication Setup](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx) (Program FIM 2010: konfigurowanie uwierzytelniania za pośrednictwem protokołu Kerberos).

### Konfigurowanie delegowania w celu przyznania dostępu kontom usługi MIM

Zaloguj się na komputerze PRIVDC jako administrator domeny i wykonaj następujące czynności.

1. Uruchom przystawkę **Użytkownicy i komputery usługi Active Directory**.  
2. Kliknij prawym przyciskiem myszy domenę **priv.contoso.local** i wybierz polecenie **Deleguj kontrolę**.  
3. Na karcie Wybrani użytkownicy i grupy kliknij przycisk **Dodaj**.  
4. W oknie Wybieranie: Użytkownicy, komputery lub grupy wpisz *mimcomponent; mimmonitor; mimservice* i kliknij pozycję **Sprawdź nazwy**. Gdy nazwy zostaną podkreślone, kliknij kolejno przyciski **OK** i **Dalej**.  
5. Na liście typowych zadań wybierz pozycje **Tworzenie i usuwanie kont użytkowników oraz zarządzanie nimi** i **Modyfikowanie członkostwa w grupie**, a następnie kliknij kolejno pozycje **Dalej** i **Zakończ**.

6. Ponownie kliknij prawym przyciskiem myszy domenę **priv.contoso.local** i wybierz polecenie **Deleguj kontrolę**.  
7. Na karcie Wybrani użytkownicy i grupy kliknij przycisk **Dodaj**.  
8. W oknie Wybieranie: Użytkownicy, komputery lub grupy wpisz *MIMAdmin* i kliknij pozycję **Sprawdź nazwy**. Gdy nazwy zostaną podkreślone, kliknij kolejno przyciski **OK** i **Dalej**.  
9. Wybierz **zadanie niestandardowe** i zastosuj je do **tego folderu** z **uprawnieniami ogólnymi**.    
10. Na liście uprawnień wybierz następujące pozycje:  
  - **Odczyt**  
  - **Zapis**  
  - **Tworzenie wszystkich obiektów podrzędnych**  
  - **Usuwanie wszystkich obiektów podrzędnych**  
  - **Odczyt wszystkich właściwości**  
  - **Zapis wszystkich właściwości**  
  - **Migrowanie historii SID**  
  Kliknij kolejno pozycje **Dalej** i **Zakończ**.

11. Ponownie kliknij prawym przyciskiem myszy domenę **priv.contoso.local** i wybierz polecenie **Deleguj kontrolę**.  
12. Na karcie Wybrani użytkownicy i grupy kliknij przycisk **Dodaj**.  
13. W oknie Wybieranie: Użytkownicy, komputery lub grupy wpisz *MIMAdmin* i kliknij pozycję **Sprawdź nazwy**. Gdy nazwy zostaną podkreślone, kliknij kolejno przyciski **OK** i **Dalej**.  
14. Wybierz **zadanie niestandardowe**, zastosuj je do **tego folderu** i kliknij pozycję **tylko obiekty użytkownika**.    
15. Na liście uprawnień wybierz pozycje **Zmienianie hasła** i **Resetowanie hasła**. Kliknij przycisk **Dalej**, a następnie kliknij przycisk **Zakończ**.  
16. Zamknij stronę Użytkownicy i komputery usługi Active Directory.

17. Otwórz wiersz polecenia.  
18. Przejrzyj listę kontroli dostępu w obiekcie przechowującym deskryptor zabezpieczeń administratora w domenach PRIV. Na przykład jeśli nazwa domeny to „priv.contoso.local”, wpisz polecenie  
  ```  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"  
  ```  
19. Zaktualizuj odpowiednio listę kontroli dostępu, aby mieć pewność, że usługa MIM i usługa składnika MIM mogą aktualizować członkostwa grup chronionych przez tę listę.  Wpisz polecenie:  
  ```  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"  
  ```  
20. Uruchom ponownie serwer PRIVDC, aby zmiany zostały wprowadzone.

## Przygotowywanie stacji roboczej PRIV

Jeśli nie masz jeszcze stacji roboczej służącej do wykonywania konserwacji zasobów PRIV (na przykład usługi MIM), wykonaj te instrukcje, aby przygotować stację roboczą dołączoną do domeny PRIV.  

### Instalowanie systemu Windows 8.1 lub Windows 10 Enterprise

Na innej nowej maszynie wirtualnej bez zainstalowanego oprogramowania zainstaluj system Windows 8.1 Enterprise lub Windows 10 Enterprise, aby utworzyć komputer o nazwie *„PRIVWKSTN”*.

1. Użyj ustawień ekspresowych podczas instalacji.

2. Być może podczas instalacji połączenie z Internetem nie będzie możliwe. Kliknij opcję **Utwórz konto lokalne**. Podaj inną nazwę użytkownika — nie używaj nazw „Administrator” ani „Jen”.

3. Przy użyciu Panelu sterowania przypisz temu komputerowi statyczny adres IP w sieci wirtualnej i ustaw preferowany serwer DNS interfejsu na serwer DNS komputera PRIVDC.

4. Przy użyciu Panelu sterowania dołącz komputer PRIVWKSTN do domeny priv.contoso.local. Wymaga to podania poświadczeń administratora domeny PRIV. Po ukończeniu tego procesu uruchom ponownie komputer PRIVWKSTN.

Więcej szczegółów zawiera temat [securing privileged access workstations](https://technet.microsoft.com/en-us/library/mt634654.aspx) (Zabezpieczanie stacji roboczych z dostępem uprzywilejowanym).

W następnym kroku zostanie przygotowany serwer usługi PAM.

>[!div class="step-by-step"]
[« Krok 1](step-1-prepare-corp-domain.md)
[Krok 3 »](step-3-prepare-pam-server.md)



<!--HONumber=Jun16_HO5-->


