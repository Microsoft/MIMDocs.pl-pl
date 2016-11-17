---
title: "Wdrożenie usługi PAM — krok 5 — połączenie lasów | Dokumentacja firmy Microsoft"
description: "Ustanowienie relacji zaufania między lasami PRIV i CORP, dzięki czemu uprzywilejowani użytkownicy w lesie PRIV będą mieli w dalszym ciągu dostęp do zasobów w lesie CORP."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 16208efe08c5a2c0f63ee121c64c45cad5a73909


---

# <a name="step-5-establish-trust-between-priv-and-corp-forests"></a>Krok 5 — ustanowienie zaufania między lasami PRIV i CORP

>[!div class="step-by-step"]
[« Krok 4](step-4-install-mim-components-on-pam-server.md)
[Krok 6 »](step-6-transition-group-to-pam.md)


Dla każdej domeny CORP, np. contoso.local, kontrolery domeny PRIV i CONTOSO muszą być związane relacją zaufania. Dzięki temu użytkownicy w domenie PRIV mogą uzyskać dostęp do zasobów w domenie CORP.

## <a name="connect-each-domain-controller-to-its-counterpart"></a>Łączenie każdego kontrolera domeny z jego odpowiednikiem

Zanim zostanie nawiązana relacja zaufania, każdy kontroler domeny musi zostać skonfigurowany do rozpoznawania nazwy DNS swojego odpowiednika, w oparciu o kontroler domeny/adres IP serwera DNS innej domeny.

1.  Jeśli kontrolery domeny lub serwer z oprogramowaniem MIM są wdrażane jako maszyny wirtualne, upewnij się, że nie istnieją inne serwery DNS zapewniające usługi nazw domeny tym komputerom.
    - Jeśli maszyny wirtualne mają wiele interfejsów sieciowych, w tym interfejsy sieciowe podłączone do sieci publicznych, może być wymagane tymczasowe wyłączenie tych połączeń lub zastąpienie ustawień interfejsu sieciowego systemu Windows. Należy się upewnić, że adres serwera DNS podany przez protokół DHCP nie jest wykorzystywany przez żadną z maszyn wirtualnych.

2.  Sprawdź, czy każdy istniejący kontroler domeny CORP może przekazywać nazwy do lasu PRIV. Na każdym kontrolerze domeny spoza lasu PRIV, np. CORPDC, uruchom program PowerShell i wprowadź następujące polecenie:

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    Sprawdź, czy dane wyjściowe wskazują rekord serwera nazw dla domeny PRIV z poprawnym adresem IP.

3.  Jeśli kontroler domeny nie może przekierować domeny PRIV, użyj **Menedżera DNS** (znajdującego się w **Start** > **Narzędzia aplikacji** > **DNS**), aby skonfigurować przekierowywanie nazw DNS dla domeny PRIV do adresu IP PRIVDC. Jeśli jest to domena wyższego poziomu (np. contoso.local), rozwiń węzły dla tego kontrolera domeny i jego domeny, np. **CORPDC** > **Strefy wyszukiwania do przodu** > **contoso.local**, a następnie upewnij się, że klucz o nazwie **priv** jest obecny jako typ serwera nazw (NS).

    ![Struktura plików klucza prywatnego — zrzut ekranu](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>Ustalenie zaufania na serwerze PAMSRV

Na serwerze PAMSRV ustal jednokierunkowe zaufanie z każdą domeną, np. CORPDC, aby kontrolery domeny CORP miały zaufanie do lasu PRIV.

1. Zaloguj się do serwera PAMSRV jako administrator domeny PRIV (PRIV\Administrator).

2.  Uruchom program PowerShell.

3.  Wpisz poniższe polecenia programu PowerShell dla każdego istniejącego lasu. Wprowadź poświadczenia administratora domeny CORP (CONTOSO\Administrator) po wyświetleniu monitu.

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Wpisz poniższe polecenia programu PowerShell dla każdej domeny w istniejących lasach. Wprowadź poświadczenia administratora domeny CORP (CONTOSO\Administrator) po wyświetleniu monitu.

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>Zapewnianie dostępu do odczytu do usługi Active Directory w lassach

Dla każdego istniejącego lasu włącz dostęp do odczytu do usługi AD dla administratorów PRIV i usługi monitorowania.

1.  Zaloguj się do istniejącego kontrolera domeny lasu CORP (CORPDC) jako administrator domeny dla domeny najwyższego poziomu w tym lesie (Contoso\Administrator).  
2.  Uruchom przystawkę **Użytkownicy i komputery usługi Active Directory**.  
3.  Kliknij prawym przyciskiem myszy domenę **contoso.local** i wybierz polecenie **Deleguj kontrolę**.  
4.  Na karcie Wybrani użytkownicy i grupy kliknij przycisk **Dodaj**.  
5.  W oknie Wybieranie: Użytkownicy, komputery lub grupy kliknij pozycję **Lokalizacje**, a następnie zmień lokalizację na *priv.contoso.local*.  W polu nazwy obiektu wpisz *Administratorzy domeny*, a następnie kliknij pozycję **Sprawdź nazwy**. Po wyświetleniu okienka wyskakującego wprowadź nazwę użytkownika *priv\administrator* i hasło tego użytkownika.  
6.  Po łańcuchu Administratorzy domeny dodaj wpis „*; MIMMonitor*”. Po podkreśleniu nazw **Administratorzy domeny** i **MIMMonitor** kliknij przycisk **OK**, następnie kliknij przycisk **Dalej**.  
7.  Na liście typowych zadań wybierz pozycję **Odczytywanie wszystkich informacji o użytkowniku**, kliknij przycisk **Dalej**, a następnie kliknij przycisk **Zakończ**.  
8.  Zamknij stronę Użytkownicy i komputery usługi Active Directory.

9.  Otwórz okno programu PowerShell.  
10.  Użyj polecenia `netdom`, aby upewnić się, że historia SID jest włączona, a filtrowanie SID wyłączone. Typ:  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    Dane wyjściowe powinny przekazać komunikat **Włączanie historii SID dla tego zaufania** lub **Historia SID jest już włączona dla tego zaufania**.

    Dane wyjściowe powinny również wskazywać, że **Filtrowanie SID nie jest włączone dla tego zaufania**. Zobacz [Wyłączanie poddawania filtra SID kwarantannie](http://technet.microsoft.com/library/cc772816.aspx), aby uzyskać więcej informacji.

## <a name="start-the-monitoring-and-component-services"></a>Uruchamianie usług monitorowania i składowych

1.  Zaloguj się do serwera PAMSRV jako administrator domeny PRIV (PRIV\Administrator).

2.  Uruchom program PowerShell.

3.  Wpisz poniższe polecenia programu PowerShell.

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

W następnym kroku przeniesiesz grupę do systemu PAM.

>[!div class="step-by-step"]
[« Krok 4](step-4-install-mim-components-on-pam-server.md)
[Krok 6 »](step-6-transition-group-to-pam.md)



<!--HONumber=Nov16_HO2-->


