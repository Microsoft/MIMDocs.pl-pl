---
title: Instalowanie usługi synchronizacji programu MIM | Dokumentacja firmy Microsoft
description: Rozpoczęcie pracy ze składnikami programu MIM 2016 przez instalację i konfigurację usługi synchronizacji.
keywords: ''
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 05/01/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c68b33b2ff28d75b6f4e63fa8caf0c87727a5927
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289401"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Instalacja usługi synchronizacji programu MIM 2016

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [Portal i usługa programu MIM »](install-mim-service-portal.md)
> 
> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **corpdc**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi MIM — **corpservice**
> - Nazwa serwera synchronizacji MIM — **corpsync**
> - Nazwa programu SQL Server — **corpsql**
> - Hasło — <strong>Pass@word1</strong>

Aby zainstalować składniki programu Microsoft Identity Manager 2016, należy najpierw skonfigurować pakiet instalacyjny.

1. Zaloguj się jako *contoso\miminstall* na serwerze używanym do serwera synchronizacja zarządzania tożsamościami **corpsync**.

2. Rozpakuj pakiet instalacyjny programu MIM lub zamontuj dysk DVD z obrazem programu MIM.

## <a name="install-mim-2016-sp1-synchronization-service"></a>Zainstaluj usługę synchronizacji programu MIM 2016 z dodatkiem SP1

1. W rozpakowanym folderze instalacyjnym programu MIM przejdź do folderu **Synchronization Service** (Usługa synchronizacji).

2. Uruchom **Instalatora usługi synchronizacji programu MIM**. Postępuj zgodnie z wytycznymi instalatora i ukończ instalację.

3. Na ekranie powitalnym kliknij przycisk **Dalej**.

    ![Obraz powitania w kreatorze instalatora programu MIM](media/install-mim-sync/MIM_Install1.png)

4. Przejrzyj postanowienia licencyjne i kliknij przycisk **Dalej**, aby je zaakceptować.

5. Na ekranie **Instalacja niestandardowa** kliknij przycisk **Dalej**.

    ![Obraz z instalacją niestandardową](media/install-mim-sync/MIM_Install2.png)

6. Na ekranie konfiguracji bazy danych usługi synchronizacji wybierz:

   1.  Program SQL Server znajduje się na: **komputera zdalnego A** o nazwie **corpsql.contoso.com**.

   2.  Wystąpienie programu SQL Server: **domyślnego wystąpienia**

   ![Obraz łączenia z bazą danych](media/install-mim-sync/MIM_Install3.png)

7. Skonfiguruj konto usługi synchronizacji zgodnie z kontem utworzonym wcześniej:

   1. Konto usługi: *MIMSync*

   2. Hasło: <em>Pass@word1</em>

   3. Domena konta usługi lub nazwa komputera lokalnego: *contoso*

   ![Obraz konta usługi](media/install-mim-sync/MIM_Install4.png)

8. W instalatorze usługi synchronizacji programu MIM określ następujące grupy zabezpieczeń:

   1. Administrator = *contoso\MIMSyncAdmins*

   2. Operator = *contoso\MIMSyncOperators*

   3. Węzły przyłączające = *contoso\MIMSyncJoiners*

   4. Przeglądanie łączników = *contoso\MIMSyncBrowse*

   5. Zarządzanie hasłami WMI = *contoso\MIMSyncPasswordReset*

   ![Obraz grup zabezpieczeń](media/install-mim-sync/MIM_Install5.png)

9. Na ekranie ustawień zabezpieczeń zaznacz pozycję **Włącz reguły zapory dla przychodzącej komunikacji zdalnego wywołania procedur** i kliknij przycisk **Dalej**.

10. Kliknij przycisk **Instaluj**, aby rozpocząć instalację usługi synchronizacji programu MIM.

    1. Może zostać wyświetlone ostrzeżenie dotyczące konta usługi synchronizacji programu MIM — kliknij przycisk **OK**.

    2. Usługa synchronizacji programu MIM zostanie zainstalowana.

    3. Zostanie wyświetlone powiadomienie dotyczące tworzenia kopii zapasowej klucza szyfrowania — kliknij przycisk **OK** i wybierz folder, w którym zostanie zapisana kopia zapasowa klucza szyfrowania.

        ![Obraz uwagi dotyczącej kopii zapasowej klucza szyfrowania usługi synchronizacji programu MIM](media/MIM-Install7.png)

    4. Gdy instalator pomyślnie ukończy instalację, kliknij przycisk **Zakończ**.

    5. Aby zmiany członkostwa w grupach zostały uwzględnione, musisz wylogować się i zalogować ponownie. Kliknij przycisk **Tak**, aby się wylogować.

> [!div class="step-by-step"]  
> [« Exchange Server](prepare-server-exchange.md)
> [Portal i usługa programu MIM »](install-mim-service-portal.md)
