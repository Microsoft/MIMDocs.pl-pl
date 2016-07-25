---
title: "Instalacja usługi synchronizacji programu MIM | Microsoft Identity Manager"
description: "Rozpoczęcie pracy ze składnikami programu MIM 2016 przez instalację i konfigurację usługi synchronizacji."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 2dfc89d32ef3b615201f1eb57ed3b8f5daed513e


---

# Instalacja usługi synchronizacji programu MIM 2016

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[Portal i usługa programu MIM »](install-mim-service-portal.md)

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **nazwa_serwera_mim**
> - Nazwa domeny — **contoso**
> - Hasło — **Has@lo1**

Aby zainstalować składniki programu Microsoft Identity Manager 2016, należy najpierw skonfigurować pakiet instalacyjny.

1. Zaloguj się na konto *contoso\Administrator* na serwerze używanym do zarządzania tożsamościami.

2. Rozpakuj pakiet instalacyjny programu MIM lub zamontuj dysk DVD z obrazem programu MIM.

## Zainstaluj usługę synchronizacji programu MIM 2016

1. W rozpakowanym folderze instalacyjnym programu MIM przejdź do folderu **Synchronization Service** (Usługa synchronizacji).

2. Uruchom **Instalatora usługi synchronizacji programu MIM**. Postępuj zgodnie z wytycznymi instalatora i ukończ instalację.

3. Na ekranie powitalnym kliknij przycisk **Dalej**.

    ![Obraz powitania w kreatorze instalatora programu MIM](media/MIM-Install1.png)

4. Przejrzyj postanowienia licencyjne i kliknij przycisk **Dalej**, aby je zaakceptować.

5. Na ekranie **Instalacja niestandardowa** kliknij przycisk **Dalej**.

    ![Obraz z instalacją niestandardową](media/MIM-Install2.png)

6.  Na ekranie konfiguracji bazy danych synchronizacji wybierz:

    1.  Program SQL Server znajduje się na: **Ten komputer**.

    2.  Wystąpienie programu SQL Server: **Wystąpienie domyślne**.

    ![Obraz łączenia z bazą danych](media/MIM-Install3.png)

7.  Skonfiguruj konto usługi synchronizacji zgodnie z kontem utworzonym wcześniej:

    1.  Konto usługi: *MIMSync*

    2.  Hasło: *Has@lo1*

    3.  Domena konta usługi lub nazwa komputera lokalnego: *contoso*

    ![Obraz konta usługi](media/MIM-Install4.png)

8.  W instalatorze synchronizacji programu MIM określ następujące grupy zabezpieczeń:

    1. Administrator = *contoso\MIMSyncAdmins*

    2. Operator = *contoso\MIMSyncOperators*

    3. Węzły przyłączające = *contoso\MIMSyncJoiners*

    4. Przeglądanie łączników = *contoso\MIMSyncBrowse*

    5. Zarządzanie hasłami WMI = *contoso\MIMSyncPasswordReset*

    ![Obraz grup zabezpieczeń](media/MIM-Install5.png)

9. Na ekranie ustawień zabezpieczeń zaznacz pozycję **Włącz reguły zapory dla przychodzącej komunikacji zdalnego wywołania procedur** i kliknij przycisk **Dalej**.

10. Kliknij przycisk **Instaluj**, aby rozpocząć instalację usługi synchronizacji programu MIM.

    1. Może zostać wyświetlone ostrzeżenie dotyczące konta usługi synchronizacji programu MIM — kliknij przycisk **OK**.

    2. Zostanie zainstalowana usługa synchronizacji programu MIM.

    3. Zostanie wyświetlone powiadomienie dotyczące tworzenia kopii zapasowej klucza szyfrowania — kliknij przycisk **OK** i wybierz folder, w którym zostanie zapisana kopia zapasowa klucza szyfrowania.

        ![Obraz uwagi dotyczącej kopii zapasowej klucza szyfrowania usługi synchronizacji programu MIM](media/MIM-Install7.png)

    4. Gdy instalator pomyślnie ukończy instalację, kliknij przycisk **Zakończ**.

    5. Aby zmiany członkostwa w grupach zostały uwzględnione, musisz wylogować się i zalogować ponownie. Kliknij przycisk **Tak**, aby się wylogować.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[Portal i usługa programu MIM »](install-mim-service-portal.md)



<!--HONumber=Jul16_HO3-->


