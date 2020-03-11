---
title: Wdrożenie usługi PAM — krok 6 — przeniesienie grupy | Dokumentacja firmy Microsoft
description: Migracja grupy do lasu PRIV, dzięki czemu można będzie nią zarządzać za pomocą usługi Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e88407ceb1c7ac99f1746f453b7e4a7a5d296e5a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043634"
---
# <a name="step-6--transition-a-group-to-privileged-access-management"></a>Krok 6. Przeniesienie grupy do usługi Privileged Access Management

> [!div class="step-by-step"]
> [« Krok 5 ](step-5-establish-trust-between-priv-corp-forests.md)
> [Krok 7 »](step-7-elevate-user-access.md)

Tworzenie kont uprzywilejowanych w lesie PRIV odbywa się przy użyciu poleceń cmdlet programu PowerShell. Te polecenia cmdlet umożliwiają wykonanie następujących działań:

- Utworzenie nowej grupy w lesie PRIV z tym samym identyfikatorem zabezpieczeń (SID), który jest przypisany do grupy w lesie CORP.  
- Utworzenie obiektu w bazie danych usługi programu MIM odpowiadającego grupie w lesie PRIV.  
- Utworzenie dwóch obiektów w bazie danych usługi programu MIM dla każdego konta użytkownika, które odpowiadają użytkownikowi w lesie CORP i nowemu kontu użytkownika w lesie PRIV.  
- Utworzenie obiektu roli PAM w bazie danych usługi programu MIM.  

Te polecenia cmdlet należy uruchomić jeden raz dla każdej grupy i dla poszczególnych członków grupy. Polecenia cmdlet migracji nie powodują zmiany żadnych użytkowników ani grup w lesie CORP — odpowiednie modyfikacje zostaną wprowadzone ręcznie przez administratora usługi PAM.

1. Zaloguj się na serwerze PAMSRV jako *PRIV\MIMAdmin*, bezpośrednio lub przy użyciu stacji roboczej PRIV.

2.  W programie PowerShell wpisz poniższe polecenia.

```PowerShell
   Import-Module MIMPAM
   Import-Module ActiveDirectory
```

3. Dla celów demonstracyjnych utwórz konto użytkownika w lesie PRIV odpowiadające kontu użytkownika w istniejącym lesie.

   W programie PowerShell wpisz poniższe polecenia.  Jeśli do utworzenia użytkownika w domenie contoso.local nie użyto imienia *Jen*, odpowiednio zmień parametry polecenia. Hasło 'Pass@word1' jest tylko przykładowe i należy je zmienić przy użyciu unikatowej wartości.

   ```PowerShell
       $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
       $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
       Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
       Set-ADUser –identity priv.Jen –Enabled 1
   ```

4. Dla celów demonstracyjnych skopiuj grupę i jej członka, Jen, z domeny CONTOSO do domeny PRIV.

    Uruchom następujące polecenia, podając hasło administratora domeny CORP (CONTOSO\Administrator), jeśli zostanie wyświetlony monit:

   ```PowerShell
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
   ```

    Polecenie **New-PAMGroup** przyjmuje następujące parametry:

     -   Nazwa domeny lasu CORP w formularzu NetBIOS  
     -   Nazwa grupy do skopiowania z tej domeny  
     -   Nazwa NetBIOS kontrolera domeny lasu CORP  
     -   Poświadczenia użytkownika administratora domeny w lesie CORP  

5. (Opcjonalnie) Na kontrolerze domeny CORPDC usuń konto użytkownika Jen z grupy **CONTOSO CorpAdmins**, jeśli jest ono nadal tam obecne.  To działanie ma na celu jedynie pokazanie, jak można kojarzyć uprawnienia z kontami utworzonymi w lesie PRIV.

   1.  Zaloguj się na kontrolerze domeny CORPDC jako *CONTOSO\Administrator*.

   2.  Uruchom program PowerShell, uruchom następujące polecenie i potwierdź zmianę.

       ```PowerShell
       Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
       ```


Jeśli chcesz przekonać się, że prawa dostępu między lasami dla konta administratora działają, przejdź do kolejnego kroku.

> [!div class="step-by-step"]
> [« Krok 5 ](step-5-establish-trust-between-priv-corp-forests.md)
> [Krok 7 »](step-7-elevate-user-access.md)
