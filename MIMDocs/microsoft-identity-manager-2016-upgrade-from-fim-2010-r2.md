---
title: Uaktualnienie z programu FIM 2010 R2 do programu Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft
description: Informacje o sposobie uaktualniania składników programu FIM 2010 R2, a następnie instalowania składników, które są nowe w programie MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7df1a4ca7a33aa2a00ac0b2c3ef24056c9720b5b
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043243"
---
# <a name="upgrade-from-forefront-identity-manager-2010-r2"></a>Uaktualnienie z programu Forefront Identity Manager 2010 R2

Jeśli masz środowisko Forefront Identity Manager (FIM) 2010 R2, a chcesz wypróbować program Microsoft Identity Manager (MIM) 2016, skorzystaj z tego artykułu jako przewodnika. To uaktualnienie składa się z trzech etapów:

1.  Zainstaluj usługę synchronizacji programu MIM (Sync) na serwerze przyłączonym do Twojej domeny usługi Active Directory (AD). Spowoduje to zastąpienie wystąpienia składnika Sync środowiska FIM 2010 R2.

2.  Zainstaluj portal i usługę programu MIM. W tym momencie możesz również zainstalować portal rejestracji samoobsługowego resetowania haseł (SSPR) i portal usługi. Zostaną one zainstalowane z wyłączeniem zestawu funkcji zarządzania dostępem uprzywilejowanym.

3.  Wdróż dodatki i rozszerzenia programu MIM na każdym komputerze klienckim oddzielnie. Należy do nich między innymi zintegrowany klient logowania systemu Windows SSPR.


W tym przewodniku założono, że masz już skonfigurowane następujące elementy:
- Program FIM 2010 R2 wdrożony w środowisku testowym
- Serwery z systemem Windows Server 2012, Windows Server 2012 R2 lub Windows Server 2008 R2
- Lokalne i środowiskowe wymagania wstępne (SQL Server, Exchange Server, SharePoint Services itp.) skonfigurowane na potrzeby programu FIM 2010 R2.


## <a name="preparation"></a>Przygotowywanie

1.  Utwórz kopię zapasową bazy danych usługi programu FIM, bazy danych usługi synchronizacji programu FIM oraz oprogramowania i konfiguracji usługi synchronizacji programu FIM i usługi programu FIM.

2.  Na każdym serwerze, na którym są zainstalowane składniki programu FIM 2010 R2 — np. *CORPIDM* — zaloguj się jako użytkownik Contoso\Administrator. W tym przykładzie wdrożenia do uaktualnienia programu FIM 2010 R2 do programu **MIM** niezbędne są prawa administracyjne.

3.  Pobierz i rozpakuj pliki oprogramowania MIM.  Jeśli nie masz tego oprogramowania, zobacz [Microsoft Identity Manager Licencjonowanie i pobieranie](microsoft-identity-manager-licensing.md).

## <a name="upgrade-the-synchronization-service"></a>Uaktualnianie usługi synchronizacji

1.  Zaloguj się jako administrator do serwera, na którym wdrożono usługę synchronizacji programu FIM 2010 R2.

2.  Przed rozpoczęciem tej procedury koniecznie utwórz kopię zapasową bazy danych.

3.  Otwórz konsolę **Usługi**, odszukaj pozycję **Usługa synchronizacji programu Forefront Identity Manager** i zatrzymaj ją.

    ![Obraz przedstawiający konsolę Usługi](media/MIM-UpgFIM1.PNG)

4.  Uruchom **Instalatora usługi synchronizacji programu MIM**. Instalator wykryje istniejącą wersję usługi synchronizacji i zaproponuje uaktualnienie. Kliknij przycisk **Aktualizuj**, aby kontynuować.

5.  Jeśli akceptujesz postanowienia licencyjne, kliknij przycisk **Dalej**, aby kontynuować.

6.  Wprowadź hasło dla konta usługi używanego przez usługę synchronizacji i kliknij pozycję **Dalej**.

    ![Obraz przedstawiający konfigurowanie konta usługi synchronizacji programu MIM](media/MIM-UpgFIM3.png)

7.  Zweryfikuj nazwy grup zabezpieczeń i kliknij pozycję **Dalej**.

    ![Obraz przedstawiający konfigurowanie grup zabezpieczeń usługi synchronizacji programu MIM](media/MIM-UpgFIM4.png)

8.  Pozostaw bez zmian pole wyboru dla reguł zapory dotyczących przychodzącej komunikacji RPC.

9. Instalator jest gotowy do uaktualnienia usługi synchronizacji programu FIM 2010 R2 do programu MIM. Kliknij polecenie **Uaktualnij**, aby uruchomić proces uaktualniania.

10. Uaktualnianie jest w toku. Nie zamykaj instalatora ani nie uruchamiaj ponownie komputera w trakcie wykonywania uaktualnienia.

    ![Obraz przedstawiający stan instalacji usługi synchronizacji programu MIM](media/MIM-UpgFIM7.png)

11. Podczas uaktualniania jest wyświetlane ostrzeżenie dotyczące uaktualniania bazy danych usługi synchronizacji. Zaleca się utworzenie kopii zapasowej bazy danych przed rozpoczęciem uaktualnienia.

12. Po pomyślnym zakończeniu uaktualniania kliknij pozycję **Zakończ**.

    ![Obraz przedstawiający powodzenie instalacji usługi synchronizacji programu MIM](media/MIM-UpgSP1.png)

13. Pamiętaj, że **Usługa synchronizacji** została uruchomiona ponownie.

## <a name="upgrade-the-service-and-portal"></a>Uaktualnianie usługi i portalu

1.  Zaloguj się jako administrator do serwera, na którym wdrożono portal i usługę programu FIM 2010 R2.

2.  Otwórz konsolę **Usługi**, odszukaj pozycję **Usługa programu Forefront Identity Manager** i zatrzymaj ją.

    ![Obraz przedstawiający konsolę Usługi](media/MIM-UpgFIM9.PNG)

3.  Uruchom instalatora portalu i usługi programu MIM. Kliknij przycisk **Zainstaluj**, aby kontynuować.

    ![Obraz przedstawiający instalowanie portalu i usługi programu MIM](media/MIM-UpgSP2.png)

4.  Jeśli akceptujesz postanowienia licencyjne, kliknij przycisk **Dalej**, aby kontynuować.

5.  Na ekranie Program poprawy jakości obsługi klienta programu MIM kliknij przycisk **Dalej**, aby kontynuować.

6.  Wybierz funkcje i składniki programu MIM, które chcesz zainstalować, a następnie kliknij przycisk **Dalej**.

    ![Obraz z instalacją niestandardową](media/MIM-UpgSP4.png)

    1.  **Usługa programu MIM:** ta funkcja jest wymagana na co najmniej jednym serwerze i wymaga serwera bazy danych programu SQL Server na tym samym lub na innym serwerze.

    2.  **Portal programu MIM:** ta funkcja jest wymagana na co najmniej jednym serwerze i wymaga programu SharePoint Foundation 2013.

    3.  **Portal rejestracji haseł programu MIM:** ta funkcja jest niezbędna do samoobsługowego resetowania hasła.

    4.  **Portal resetowania haseł programu MIM:** ta funkcja jest niezbędna do resetowania hasła.

7.  Podaj szczegóły programu SQL Server, który jest używany na potrzeby bazy danych usługi programu FIM. Wybierz opcję ponownego użycia istniejącej bazy danych i zachowania danych. Kliknij przycisk **Dalej** , aby kontynuować.

8. Jeśli wybrano opcję ponownego użycia istniejącej bazy danych, zostanie wyświetlone przypomnienie o utworzeniu kopii zapasowej bazy danych.

9. Wprowadź szczegóły serwera poczty. Jeśli serwer poczty znajduje się na bieżącym serwerze, wprowadź wartość „localhost” jako lokalizację serwera poczty. Kliknij przycisk **Dalej** , aby kontynuować.

    ![Obraz przedstawiający konfigurowanie połączenia z serwerem poczty](media/MIM-UpgSP6.png)

10. Wybierz certyfikat, którego usługa ma używać do weryfikacji klientów. Należy użyć istniejącego certyfikatu z lokalnego magazynu certyfikatów, który był wcześniej używany przez usługę programu FIM.

    ![Obraz przedstawiający konfigurowanie certyfikatu usługi](media/MIM-UpgSP7.png)

    1.  Jeśli wybrano opcję lokalnego magazynu certyfikatów, kliknij przycisk **Wybierz certyfikat** i wybierz certyfikat z listy w oknie podręcznym. Kliknij pozycję **OK**, a następnie pozycję **Dalej**.

        ![Obraz przedstawiający okno podręczne Wybieranie certyfikatu](media/MIM-UpgSP8.PNG)

11. Skonfiguruj poświadczenia konta usługi programu MIM. Pamiętaj, że to konto usługi nie może być kontem usługi używanym przez usługę synchronizacji. Powinno to być to samo konto, które było używane przez usługę programu FIM.

    ![Obraz przedstawiający konfigurowanie konta usługi programu MIM](media/MIM-UpgSP9.png)

12. Skonfiguruj szczegóły serwera synchronizacji programu MIM zgodnie z wdrożeniem usługi programu MIM skonfigurowanym w poprzednim kroku.

    ![Obraz przedstawiający konfigurowanie portalu i usługi programu MIM](media/MIM-UpgSP10.png)

13. Podczas instalowania portalu programu MIM podaj adres serwera usługi programu MIM. Kliknij przycisk **Dalej**.

14. Podczas instalowania portalu programu MIM podaj adres URL zbioru witryn programu SharePoint, w którym jest obecnie hostowany portal programu FIM. Kliknij przycisk **Dalej**.

## <a name="install-the-mim-password-registration-portal"></a>Instalowanie portalu rejestracji haseł programu MIM

1. Jeśli instalujesz portal rejestracji haseł programu MIM, podaj żądany adres URL portalu rejestracji haseł. Kliknij przycisk **Dalej**.

2. Skonfiguruj możliwość używania usługi i portalu przez klientów i użytkowników końcowych.

    1.  Za pomocą opcji **Otwórz porty 5725 i 5726 w zaporze** określ, czy te porty mają zostać otworzone.

    2.  Za pomocą opcji **Udziel uwierzytelnionym użytkownikom dostępu do witryny portalu programu MIM** określ, czy udzielić dostępu uwierzytelnionym użytkownikom.

    3.  Kliknij przycisk **Dalej**.

3. Podaj szczegóły dostępu i poświadczenia na potrzeby rejestracji haseł programu MIM.

    ![Obraz przedstawiający konfigurowanie portalu rejestracji haseł programu MIM](media/MIM-UpgSP15.png)

    1.  Podaj nazwę konta usługi (łącznie z domeną) i hasło na potrzeby rejestracji haseł programu MIM.

    2.  Podaj szczegóły hosta — nazwę i port (na przykład 8080) — portalu rejestracji haseł.

    3.  Zaznacz opcję **Otwórz port w zaporze**.

    4.  Kliknij przycisk **Dalej**.

4. Na następnym ekranie konfiguracji rejestracji haseł programu MIM:

    1.  Wskaż funkcji rejestracji haseł miejsce, w którym zainstalowano usługę programu MIM. Zazwyczaj jest to ten sam system.

    2.  Określ, czy dostęp do tego portalu mogą uzyskiwać użytkownicy zarówno ekstranetu, jak i intranetu, czy wyłącznie użytkownicy intranetu (zgodnie z wcześniejszą konfiguracją funkcji resetowania haseł programu FIM).

## <a name="install-the-mim-password-reset-portal"></a>Instalowanie portalu resetowania haseł programu MIM

1. Jeśli instalujesz portal resetowania haseł programu MIM, podaj szczegóły dostępu i poświadczenia na potrzeby resetowania haseł programu MIM.

    ![Obraz przedstawiający konfigurowanie portalu resetowania haseł programu MIM](media/MIM-UpgSP17.png)

    1.  Podaj nazwę konta usługi (łącznie z domeną) i hasło na potrzeby resetowania haseł programu MIM.

    2.  Podaj szczegóły hosta — nazwę i port (na przykład 8088) — portalu resetowania haseł.

    3.  Zaznacz opcję **Otwórz port w zaporze**.

    4.  Kliknij przycisk **Dalej**.

2. Na następnym ekranie konfiguracji resetowania haseł programu MIM:

    1.  Wskaż funkcji resetowania haseł programu MIM miejsce, w którym zainstalowano usługę programu MIM.

    2.  Określ, czy dostęp do tego portalu mogą uzyskiwać użytkownicy zarówno ekstranetu, jak i intranetu, czy wyłącznie użytkownicy intranetu.

## <a name="finish-installation-and-upgrade"></a>Zakończenie instalacji i uaktualniania

1. Po pomyślnym podaniu wszystkich definicji konfiguracji zostanie wyświetlona strona instalacji. Kliknij polecenie **Zainstaluj**, aby rozpocząć instalowanie oraz uaktualnianie portalu i usługi programu MIM.

2. Instalowanie oraz uaktualnianie portalu i usługi programu MIM jest w toku. Nie anuluj instalatora ani nie uruchamiaj ponownie komputera w trakcie instalowania.

3. Po pomyślnym ukończeniu instalowania (uaktualniania) portalu i usługi programu MIM zostanie wyświetlony ekran potwierdzenia. Kliknij przycisk **Zakończ**, aby zakończyć instalowanie i zamknąć instalatora.

4. Pamiętaj, że **Usługa programu Forefront Identity Manager** została uruchomiona ponownie.

Uwaga: jeśli na komputerach użytkowników są wdrożone dodatki i rozszerzenia programu FIM dla usługi SSPR, nie konfiguruj nowych bram telefonicznych usługi MFA na potrzeby resetowania haseł, dopóki wszystkie dodatki i rozszerzenia programu FIM nie zostaną uaktualnione do programu MIM 2016.  Ponieważ dodatki i rozszerzenia programu FIM 2010 i FIM 2010 R2 nie rozpoznają nowych bram, zwracają błąd uniemożliwiający użytkownikom ukończenie resetowania hasła.

Instrukcje dotyczące uaktualniania programu Microsoft Identity Manager 2016 SP1 można znaleźć w artykule [Microsoft Identity Manager 2016 Service Pack 1 update package](https://blogs.technet.microsoft.com/iamsupport/2016/11/08/microsoft-identity-manager-2016-service-pack-1-update-package/) (Pakiet aktualizacji programu Microsoft Identity Manager 2016 Service Pack 1)
