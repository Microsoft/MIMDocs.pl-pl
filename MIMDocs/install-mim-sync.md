---
title: Instalowanie usługi synchronizacji programu MIM | Dokumentacja firmy Microsoft
description: Rozpoczęcie pracy ze składnikami programu MIM 2016 przez instalację i konfigurację usługi synchronizacji.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: ae291712f33b0a28d3d0f3f5a451c9ac9b8a76b2
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492314"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Instalacja usługi synchronizacji programu MIM 2016

> [!div class="step-by-step"]
> [«Serwer Exchange Server](prepare-server-exchange.md) 
>  [Usługa i Portal programu MIM»](install-mim-service-portal.md)
 
> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Na przykład:
> - Nazwa kontrolera domeny — **corpdc**
> - Nazwa domeny — **contoso**
> - Nazwa serwera usługi programu MIM — **corpservice**
> - Nazwa serwera synchronizacji programu MIM — **corpsync**
> - Nazwa SQL Server — **corpsql**
> - Hasło <strong>Pass@word1</strong>

Aby zainstalować składniki programu Microsoft Identity Manager 2016, należy najpierw skonfigurować pakiet instalacyjny.

1. Zaloguj się jako *contoso\miminstall* na serwerze używanym przez program Identity Management synchronizacja Server **corpsync**.

2. Rozpakuj pakiet instalacyjny programu MIM lub zamontuj dysk DVD z obrazem programu MIM.  Jeśli nie masz tego dysku DVD, zobacz [Microsoft Identity Manager Licencjonowanie i pobieranie](microsoft-identity-manager-licensing.md).

## <a name="install-mim-2016-sp1-or-later-synchronization-service"></a>Zainstaluj program MIM 2016 z dodatkiem SP1 lub nowszą usługę synchronizacji

1. W rozpakowanym folderze instalacyjnym programu MIM przejdź do folderu **Synchronization Service** (Usługa synchronizacji).

2. Uruchom **Instalatora usługi synchronizacji programu MIM**. Postępuj zgodnie z wytycznymi instalatora i ukończ instalację.

3. Na ekranie powitalnym kliknij przycisk **Dalej**.

    ![Obraz powitania w kreatorze instalatora programu MIM](media/install-mim-sync/MIM_Install1.png)

4. Przejrzyj postanowienia licencyjne i kliknij przycisk **Dalej** , aby je zaakceptować.

5. Na ekranie **Instalacja niestandardowa** kliknij przycisk **Dalej**.

    ![Obraz z instalacją niestandardową](media/install-mim-sync/MIM_Install2.png)

6. Na ekranie konfiguracji bazy danych usługi synchronizacji wybierz:

   1.  SQL Server znajduje się w lokalizacji: **maszyna zdalna** o nazwie **corpsql.contoso.com**.

   2.  Wystąpienie programu SQL Server: **Wystąpienie domyślne**.

   ![Obraz łączenia z bazą danych](media/install-mim-sync/MIM_Install3.png)

    3. *MIM 2016 SP2 i nowsze* : Skonfiguruj nazwę bazy danych usługi synchronizacji programu MIM

7. Skonfiguruj konto usługi synchronizacji zgodnie z kontem utworzonym wcześniej:

   1. Konto usługi: *MIMSync*

   2. Hasło <em>Pass@word1</em>

   3. Domena konta usługi lub nazwa komputera lokalnego: *contoso*

    >[!NOTE]
    >MIM 2016 SP2 i nowszych: dla kont usług zarządzanych przez grupę upewnij się, że **$** znak znajduje się na końcu nazwy konta usługi, np. MIMSync $, i pozostaw pole hasła puste.

    ![Obraz konta usługi](media/install-mim-sync/MIM_Install4.png)

8. W instalatorze usługi synchronizacji programu MIM określ następujące grupy zabezpieczeń:

   1. Administrator = *contoso\MIMSyncAdmins*

   2. Operator = *contoso\MIMSyncOperators*

   3. Węzły przyłączające = *contoso\MIMSyncJoiners*

   4. Przeglądanie łączników = *contoso\MIMSyncBrowse*

   5. Zarządzanie hasłami WMI = *contoso\MIMSyncPasswordReset*

   ![Obraz grup zabezpieczeń](media/install-mim-sync/MIM_Install5.png)

9. Na ekranie ustawień zabezpieczeń zaznacz pozycję **Włącz reguły zapory dla przychodzącej komunikacji zdalnego wywołania procedur** i kliknij przycisk **Dalej**.

10. Kliknij przycisk **Instaluj** , aby rozpocząć instalację usługi synchronizacji programu MIM.

    1. Może zostać wyświetlone ostrzeżenie dotyczące konta usługi synchronizacji programu MIM — kliknij przycisk **OK**.

    2. Usługa synchronizacji programu MIM zostanie zainstalowana.

    3. Zostanie wyświetlone powiadomienie dotyczące tworzenia kopii zapasowej klucza szyfrowania — kliknij przycisk **OK** i wybierz folder, w którym zostanie zapisana kopia zapasowa klucza szyfrowania.

    ![Obraz uwagi dotyczącej kopii zapasowej klucza szyfrowania usługi synchronizacji programu MIM](media/MIM-Install7.png)

    4. Gdy instalator pomyślnie ukończy instalację, kliknij przycisk **Zakończ**.

    5. Aby zmiany członkostwa w grupach zostały uwzględnione, musisz wylogować się i zalogować ponownie. Kliknij przycisk **Tak** , aby się wylogować.

> [!div class="step-by-step"]  
> [«Serwer Exchange Server](prepare-server-exchange.md) 
>  [Usługa i Portal programu MIM»](install-mim-service-portal.md)
