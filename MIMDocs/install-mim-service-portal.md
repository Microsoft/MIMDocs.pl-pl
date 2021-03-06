---
title: Instalowanie usługi i portalu programu Microsoft Identity Manager | Dokumentacja firmy Microsoft
description: Pobierz kroki konfigurowania i instalowania usługi i portalu programu MIM dla programu Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: a85a8eeaf999c193a3e2bbd3f2cdf75cef65e574
ms.sourcegitcommit: 80507a128d2bc28ff3f1b96377c61fa97a4e7529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/13/2020
ms.locfileid: "83280001"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Instalacja programu MIM 2016: usługa i portal programu MIM

> [!div class="step-by-step"]
> [«Usługa](install-mim-sync.md) 
>  synchronizacji programu MIM [Synchronizuj bazy danych»](install-mim-sync-ad-service.md)
 
> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **nazwa_serwera_mim**
> - Nazwa domeny — **contoso**
> - Hasło<strong>Pass@word1</strong>
> - Nazwa konta usługi — **usługa_programu_MIM**

Jeśli pakiet instalacyjny programu MIM nie został skonfigurowany w ostatnim kroku, przed kontynuowaniem cofnij się i zainstaluj składniki programu Microsoft Identity Manager 2016.


## <a name="configure-mim-service-and-portal-for-installation"></a>Konfigurowanie usługi i portalu programu MIM pod kątem instalacji

1. Uruchom **instalatora usługi i portalu programu MIM** z rozpakowanego podfolderu **Service and Portal** (Usługa i portal).

2. Na ekranie powitalnym kliknij przycisk **Dalej**.

3. Przeczytaj Umowę Licencyjną Użytkownika Oprogramowania i kliknij przycisk **Dalej**, jeśli akceptujesz postanowienia licencyjne.

4. Na ekranie **Program poprawy jakości obsługi klienta programu MIM** kliknij przycisk **Dalej**.

5. Podczas wybierania funkcji składników dla tego wdrożenia koniecznie uwzględnij usługę programu MIM (z wyjątkiem raportowania programu MIM) i funkcje portalu programu MIM. Możesz również wybrać portal resetowania haseł programu MIM i usługę powiadamiania o zmianie hasła programu MIM.

6. Na stronie **Skonfiguruj połączenie bazy danych programu MIM** wybierz pozycję **Utwórz nową bazę danych**.

    ![Obraz konfigurowania połączenia z bazą danych programu MIM](media/install-mim-service-portal/MIM_Install10.png)

7. Na stronie **Konfigurowanie połączenia z serwerem poczty**wprowadź nazwę serwera Exchange Server jako **serwer poczty** lub Użyj **skrzynki pocztowej usługi O365**. Jeśli nie masz skonfigurowanego serwera poczty, użyj ciągu **localhost** jako nazwy serwera poczty i usuń zaznaczenie dwóch najwyższych pól wyboru. Kliknij przycisk **Dalej**.
    >[!NOTE]
    >Program MIM 2016 z dodatkiem SP2 i nowsze: Jeśli używasz kont usług zarządzanych przez grupę, musisz zaznaczyć pole wyboru **Użyj innego użytkownika dla programu Exchange** , nawet jeśli nie planujesz korzystać z programu Exchange.
    
    >[!NOTE]
    >W przypadku wybrania opcji **Użyj usługi Exchange Online** w celu umożliwienia usłudze MIM przetworzenia odpowiedzi na żądania zatwierdzenia z dodatku programu MIM w programie Outlook należy ustawić klucz rejestru HKLM\SYSTEM\CurrentControlSet\Services\FIMService wartość PollExchangeEnabled na 1 po instalacji.

    ![Obraz przedstawiający konfigurowanie połączenia z serwerem poczty](media/install-mim-service-portal/MIM_Install11.png)

8. Określ, że chcesz generować nowy certyfikat z podpisem własnym, lub wybierz odpowiedni certyfikat.

9. Określ nazwę konta usługi, które ma być używane (np. *usługa_MIM*), hasło konta usługi (np. <em>Pass@word1</em>), domenę konta usługi (np. *contoso*) i konto e-mail usługi (np. *contoso*).
    >[!NOTE]
    >Program MIM 2016 z dodatkiem SP2 lub nowszy: Jeśli używasz kont usług zarządzanych przez grupę, musisz upewnić się, że **$** znak jest na końcu nazwy konta usługi, np. MIMService $, i pozostaw puste pole hasło do konta usługi.

    ![Obraz przedstawiający konfigurowanie konta usługi programu MIM](media/install-mim-service-portal/MIM_Install12.png)

10. Należy pamiętać, że może zostać wyświetlone ostrzeżenie informujące o tym, że konto usługi nie jest zabezpieczone w bieżącej konfiguracji.

11. Zaakceptuj wartości domyślne dla lokalizacji serwera synchronizacji i określ konto agenta zarządzania programu MIM jako *contoso\MIMMA*.
    >[!NOTE]
    >MIM 2016 SP2 i nowszych: Jeśli planujesz używać konta usługi zarządzanej przez grupę usługi synchronizacji programu MIM w programie MIM Sync, a następnie Włącz funkcję "Użyj konta synchronizacji programu MIM", wprowadź nazwę gMSA usługi synchronizacji programu MIM jako konto w programie MIM, np. *contoso\MIMSync $*.

    ![Obraz konfigurowania usługi i portalu programu MIM](media/install-mim-service-portal/MIM_Install13.png)

12. Podaj wartość *CORPIDM* (nazwa tego komputera) jako adres serwera usługi programu MIM dla portalu programu MIM.

13. Określ `http://mim.contoso.com` jako adres URL zbioru witryn programu SharePoint.

14. Określ `http://passwordregistration.contoso.com` jako port 80 adresu URL rejestracji hasła, zalecamy późniejsze aktualizacje przy użyciu certyfikatu SSL na 443.

15. Określ `http://passwordreset.contoso.com` jako port 80 adresu URL resetowania hasła, zalecamy późniejsze aktualizacje przy użyciu certyfikatu SSL na 443.

16. Zaznacz pole wyboru umożliwiające otwarcie portów 5725 i 5726 w zaporze i pole wyboru umożliwiające zezwolenie wszystkim uwierzytelnionym użytkownikom na dostęp do portalu programu MIM.

## <a name="configure-mim-password-registration-portal"></a>Konfigurowanie portalu rejestracji haseł programu MIM

1. Ustaw wartość *contoso\MIMSSPR* jako nazwę konta usługi dla rejestracji SSPR i <em>Pass@word1</em> jako jego hasło.

2. Określ *passwordregistration.contoso.com* jako nazwę hosta dla rejestracji haseł programu MIM i Ustaw port na **80**. Włącz opcję **Otwórz port w zaporze**.

   ![Obraz wprowadzania informacji konfiguracyjnych używanych przez usługi IIS](media/install-mim-service-portal/MIM_Install14.png)

3. Zostanie wyświetlone ostrzeżenie — przeczytaj je i kliknij przycisk **Dalej**.

4. Na następnym ekranie konfiguracji portalu rejestracji haseł programu MIM Określ *MIM.contoso.com* jako adres serwera usługi programu MIM dla portalu rejestracji haseł.

## <a name="configure-mim-password-reset-portal"></a>Konfigurowanie portalu resetowania haseł programu MIM

1. W polu Nazwa konta usługi dla rejestracji SSPR ustaw wartość *wartość contoso\mimsspr jako* i hasło na <em>Pass@word1</em> .

2. Określ *PasswordReset.contoso.com* jako nazwę hosta dla portalu resetowania haseł programu MIM i Ustaw port na **80**. Włącz opcję **Otwórz port w zaporze**.

   ![Obraz wprowadzania informacji konfiguracyjnych używanych przez usługi IIS](media/install-mim-service-portal/MIM_Install15.png)

3. Zostanie wyświetlone ostrzeżenie — przeczytaj je i kliknij przycisk **Dalej**.

4. Na następnym ekranie konfiguracji portalu rejestracji haseł programu MIM Określ *MIM.contoso.com* jako adres serwera usługi programu MIM dla portalu resetowania haseł.

## <a name="install-mim-service-and-portal"></a>Instalacja portalu i usługi programu MIM

Gdy wszystkie definicje przedinstalacyjne będą gotowe, kliknij przycisk **Zainstaluj**, aby rozpocząć instalowanie wybranych składników z grupy **Usługa i portal**.

Po zakończeniu instalacji sprawdź, czy portal programu MIM jest aktywny.

1. Uruchom program Internet Explorer i Połącz się z portalem programu MIM w systemie `http://mim.contoso.com/identitymanagement` . Należy zauważyć, że podczas pierwszej wizyty na tej stronie może wystąpić krótkie opóźnienie.
    - W razie potrzeby należy uwierzytelnić się jako *contoso\miminstall* w programie Internet Explorer.

2. W programie Internet Explorer otwórz okno **Opcje internetowe**, wyświetl kartę **Zabezpieczenia** i dodaj witrynę do strefy **Lokalny intranet**, jeśli nie została jeszcze tam dodana.  Zamknij okno dialogowe **Opcje internetowe**.

3. Zezwól użytkownikom na wyświetlanie własnych wpisów w programie MIM.

    1.  Korzystając z programu Internet Explorer, w **portalu programu MIM** kliknij pozycję **Reguły zasad zarządzania**.

    2.  Wyszukaj regułę zasad zarządzania, **Zarządzanie użytkownikami: użytkownicy mogą odczytywać atrybuty własnych**.

    3.  Wybierz tę regułę zasad zarządzania, anulując zaznaczenie pozycji **Zasady są wyłączone**.

    4.  Kliknij przycisk **OK**, a następnie kliknij przycisk **Prześlij**.

4.  Sprawdź, czy zapora zezwala na połączenia przychodzące na portach TCP 5725 i 5726.

    1.  Uruchom aplet **Narzędzia administracyjne » Zapora systemu Windows** z **zabezpieczeniami zaawansowanymi**.

    2.  Kliknij pozycję **Reguły dla ruchu przychodzącego**.

    3.  Sprawdź, czy pojawiły się dwie następujące reguły:

    -   Forefront Identity Manager (STS).
    -   Forefront Identity Manager (usługa sieci Web).

    4.  Ukończ działanie kreatora i zamknij aplikację **Zapora systemu Windows**.

    5.  Uruchom aplet **Panel sterowania » Sieć i Internet » Wyświetl stan sieci i zadania**.

    6.  Sprawdź, czy aktywna sieć jest wyświetlana na liście jako contoso.local i sieć domeny.

    7.  Zamknij **Panel sterowania**.

> [!NOTE]
> Opcjonalnie: na tym etapie można zainstalować dodatki i rozszerzenia programu MIM.
 
> [!div class="step-by-step"]  
> [«Usługa](install-mim-sync.md) 
>  synchronizacji programu MIM [Synchronizuj bazy danych»](install-mim-sync-ad-service.md)
