---
title: Konfigurowanie domeny pod kątem instalacji programu Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft
description: Przed zainstalowaniem programu MIM 2016 należy utworzyć kontroler domeny usługi Active Directory
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/26/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ddab5b1ab57d3d332d5cd36ecc5a29abd83222ec
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289034"
---
# <a name="set-up-a-domain"></a>Konfigurowanie domeny

> [!div class="step-by-step"]
> [Windows Server 2016»](prepare-server-ws2016.md)

Program Microsoft Identity Manager (MIM) współpracuje z Twoją domeną usługi Active Directory (AD). Usługa AD powinna już być zainstalowana, a w środowisku musi istnieć kontroler dla domeny, którą możesz administrować.

Ten artykuł zawiera szczegółowy opis czynności, które należy wykonać w celu przygotowania domeny do pracy obok programu MIM.

## <a name="create-user-accounts-and-groups"></a>Tworzenie grup i kont użytkowników

Wszystkie składniki wdrożenia programu MIM muszą mieć własną tożsamość w domenie. Dotyczy to między innymi składników programu MIM, takich jak Service i Sync, a także SharePoint i SQL.

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **corpdc**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi MIM — **corpservice**
> - Nazwa serwera synchronizacji MIM — **corpsync**
> - Nazwa programu SQL Server — **corpsql**
> - Hasło — <strong>Pass@word1</strong>

1. Zaloguj się do kontrolera domeny jako administrator domeny (*np. Contoso\Administrator*).

2. Utwórz następujące konta użytkowników dla usług programu MIM. Uruchom program PowerShell i wpisz poniższy skrypt PowerShell w celu zaktualizowania domeny.

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMINSTALL –name MIMMA
    Set-ADAccountPassword –identity MIMINSTALL –NewPassword $sp
    Set-ADUser –identity MIMINSTALL –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMpool –name BackupAdmin
    Set-ADAccountPassword –identity MIMPool –NewPassword $sp
    Set-ADUser –identity MIMPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Utwórz grupy zabezpieczeń dla wszystkich grup.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMInstall
    ```

4.  Dodaj nazwy SPN, aby włączyć uwierzytelnianie Kerberos dla kont usług.

    ```CMD
    setspn -S http/mim.contoso.com Contoso\mimpool
    setspn -S http/mim Contoso\mimpool
    setspn -S http/passwordreset.contoso.com Contoso\mimsspr
    setspn -S http/passwordregistration.contoso.com Contoso\mimsspr
    setspn -S FIMService/mim.contoso.com Contoso\MIMService
    setspn -S FIMService/corpservice.contoso.com Contoso\MIMService
    ```
5.  Podczas instalacji należy dodać następujące rekordy DNS "A" do mechanizmu rozpoznawania nazw

- mim.contoso.com punktu z adresem ip fizycznych corpservice
- passwordreset.contoso.com punktu z adresem ip fizycznych corpservice
- passwordregistration.contoso.com punktu z adresem ip fizycznych corpservice

> [!div class="step-by-step"]
> [Windows Server 2016»](prepare-server-ws2016.md)
