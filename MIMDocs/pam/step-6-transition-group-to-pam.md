---
title: "Krok 6. Przeniesienie grupy do usługi Privileged Access Management | Microsoft Identity Manager"
description: 
keywords: 
author: 
manager: femila
ms.date: 06/16/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 01470689e862b47625346d5d5bc6bc7def11da9c
ms.openlocfilehash: b21e2fed4588572fd1b793c4942860871ae9a51c


---

# Krok 6. Przeniesienie grupy do usługi Privileged Access Management

>[!div class="step-by-step"] [« Krok 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Krok 7 »](step-7-elevate-user-access.md)

Tworzenie kont uprzywilejowanych w lesie PRIV odbywa się przy użyciu poleceń cmdlet programu PowerShell. Te polecenia cmdlet umożliwiają wykonanie następujących działań:

- Utworzenie nowej grupy w lesie PRIV z tym samym identyfikatorem zabezpieczeń (SID), który jest przypisany do grupy w lesie CORP.  
- Utworzenie obiektu w bazie danych usługi programu MIM odpowiadającego grupie w lesie PRIV.  
- Utworzenie dwóch obiektów w bazie danych usługi programu MIM dla każdego konta użytkownika, które odpowiadają użytkownikowi w lesie CORP i nowemu kontu użytkownika w lesie PRIV.  
- Utworzenie obiektu roli PAM w bazie danych usługi programu MIM.  

Te polecenia cmdlet należy uruchomić jeden raz dla każdej grupy i dla poszczególnych członków grupy. Polecenia cmdlet migracji nie powodują zmiany żadnych użytkowników ani grup w lesie CORP — odpowiednie modyfikacje zostaną wprowadzone ręcznie przez administratora usługi PAM.

1. Zaloguj się na serwerze PAMSRV jako *PRIV\MIMAdmin*, bezpośrednio lub przy użyciu stacji roboczej PRIV.

2.  W programie PowerShell wpisz poniższe polecenia.

    ```
    Import-Module MIMPAM
    Import-Module ActiveDirectory
    ```

3.  Dla celów demonstracyjnych utwórz konto użytkownika w lesie PRIV odpowiadające kontu użytkownika w istniejącym lesie.

    W programie PowerShell wpisz poniższe polecenia.  Jeśli do utworzenia użytkownika w domenie contoso.local nie użyto imienia *Jen*, odpowiednio zmień parametry polecenia. Hasło „Pass@word1” jest tylko przykładowe i należy je zmienić przy użyciu unikatowej wartości.

    ```
    $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
    $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
    Set-ADUser –identity priv.Jen –Enabled 1
    ```

4. Dla celów demonstracyjnych skopiuj grupę i jej członka, Jen, z domeny CONTOSO do domeny PRIV.

    Uruchom następujące polecenia, podając hasło administratora domeny CORP (CONTOSO\Administrator), jeśli zostanie wyświetlony monit:

        ```
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
        ```

    Polecenie **New-PAMGroup** przyjmuje następujące parametry:

        -   The CORP forest domain name in NetBIOS form  
        -   The name of the group to copy from that domain  
        -   The CORP forest Domain Controller NetBIOS name  
        -   The credentials of an domain admin user in the CORP forest  

5.  (Opcjonalnie) Na kontrolerze domeny CORPDC usuń konto użytkownika Jen z grupy **CONTOSO CorpAdmins**, jeśli jest ono nadal tam obecne.  To działanie ma na celu jedynie pokazanie, jak można kojarzyć uprawnienia z kontami utworzonymi w lesie PRIV.

    1.  Zaloguj się na kontrolerze domeny CORPDC jako *CONTOSO\Administrator*.

    2.  Uruchom program PowerShell, uruchom następujące polecenie i potwierdź zmianę.

        ```
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


Jeśli chcesz przekonać się, że prawa dostępu między lasami dla konta administratora działają, przejdź do kolejnego kroku.

>[!div class="step-by-step"] [« Krok 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Krok 7 »](step-7-elevate-user-access.md)



<!--HONumber=Jun16_HO3-->


