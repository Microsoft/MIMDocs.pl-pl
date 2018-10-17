---
title: Instalowanie usługi i portalu programu Microsoft Identity Manager | Dokumentacja firmy Microsoft
description: Pobierz kroki konfigurowania i instalowania usługi i portalu programu MIM dla programu Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/30/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 535c80fa2ff1b6250ae9a3f340cb514e58f390a9
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358622"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Instalacja programu MIM 2016: usługa i portal programu MIM

> [!div class="step-by-step"]
> [« Usługa synchronizacji programu MIM](install-mim-sync.md)
> [Bazy danych synchronizacji »](install-mim-sync-ad-service.md)
> 
> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Przykład:
> - Nazwa kontrolera domeny — **nazwa_serwera_mim**
> - Nazwa domeny — **contoso**
> - Hasło — <strong>Pass@word1</strong>
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

7. Na **Skonfiguruj połączenie z serwerem poczty**, wprowadź nazwę serwera programu Exchange jako **serwera poczty** lub użyć **skrzynki pocztowej usługi O365**. Jeśli nie masz skonfigurowanego serwera poczty, użyj ciągu **localhost** jako nazwy serwera poczty i usuń zaznaczenie dwóch najwyższych pól wyboru. Kliknij przycisk **Dalej**.

    ![Obraz przedstawiający konfigurowanie połączenia z serwerem poczty](media/install-mim-service-portal/MIM_Install11.png)

8. Określ, że chcesz generować nowy certyfikat z podpisem własnym, lub wybierz odpowiedni certyfikat.

9. Określ nazwę konta usługi, które ma być używane (np. *usługa_MIM*), hasło konta usługi (np. <em>Pass@word1</em>), domenę konta usługi (np. *contoso*) i konto e-mail usługi (np. *contoso*).

    ![Obraz przedstawiający konfigurowanie konta usługi programu MIM](media/install-mim-service-portal/MIM_Install12.png)

10. Należy pamiętać, że może zostać wyświetlone ostrzeżenie informujące o tym, że konto usługi nie jest zabezpieczone w bieżącej konfiguracji.

11. Zaakceptuj wartości domyślne dla lokalizacji serwera synchronizacji i określ konto agenta zarządzania programu MIM jako *contoso\MIMMA*.

    ![Obraz konfigurowania usługi i portalu programu MIM](media/install-mim-service-portal/MIM_Install13.png)

12. Podaj wartość *CORPIDM* (nazwa tego komputera) jako adres serwera usługi programu MIM dla portalu programu MIM.

13. Określ *http://mim.contoso.com* programu SharePoint adres URL zbioru witryn.

14. Określ *http://passwordregistration.contoso.com* jako adres URL rejestracji haseł port 80, zaleca się zaktualizowanie później za pomocą certyfikatu SSL na porcie 443.

15. Określ *http://passwordreset.contoso.com* jako adres URL resetowania hasła port 80, zaleca się zaktualizowanie później za pomocą certyfikatu SSL na porcie 443.

16. Zaznacz pole wyboru umożliwiające otwarcie portów 5725 i 5726 w zaporze i pole wyboru umożliwiające zezwolenie wszystkim uwierzytelnionym użytkownikom na dostęp do portalu programu MIM.

## <a name="configure-mim-password-registration-portal"></a>Konfigurowanie portalu rejestracji haseł programu MIM

1. Ustaw wartość *contoso\MIMSSPR* jako nazwę konta usługi dla rejestracji SSPR i <em>Pass@word1</em> jako jego hasło.

2. Określ *passwordregistration.contoso.com* jako nazwę hosta dla rejestracji haseł programu MIM i Ustaw port **80**. Włącz opcję **Otwórz port w zaporze**.

   ![Obraz wprowadzania informacji konfiguracyjnych używanych przez usługi IIS](media/install-mim-service-portal/MIM_Install14.png)

3. Zostanie wyświetlone ostrzeżenie — przeczytaj je i kliknij przycisk **Dalej**.

4. Na następnym ekranie konfiguracji portalu rejestracji haseł programu MIM określ *mim.contoso.com* jako adres serwera usługi MIM dla portalu rejestracji haseł.

## <a name="configure-mim-password-reset-portal"></a>Konfigurowanie portalu resetowania haseł programu MIM

1. Ustaw nazwę konta usługi dla rejestracji SSPR *Contoso\MIMSSPR* i jego hasło <em>Pass@word1</em>.

2. Określ *passwordreset.contoso.com* jako nazwę hosta dla portalu resetowania haseł programu MIM i Ustaw port **80**. Włącz opcję **Otwórz port w zaporze**.

   ![Obraz wprowadzania informacji konfiguracyjnych używanych przez usługi IIS](media/install-mim-service-portal/MIM_Install15.png)

3. Zostanie wyświetlone ostrzeżenie — przeczytaj je i kliknij przycisk **Dalej**.

4. Na następnym ekranie konfiguracji portalu rejestracji haseł programu MIM określ *mim.contoso.com* jako adres serwera usługi MIM dla portalu resetowania haseł.

## <a name="install-mim-service-and-portal"></a>Instalacja portalu i usługi programu MIM

Gdy wszystkie definicje przedinstalacyjne będą gotowe, kliknij przycisk **Zainstaluj**, aby rozpocząć instalowanie wybranych składników z grupy **Usługa i portal**.

Po zakończeniu instalacji sprawdź, czy portal programu MIM jest aktywny.

1. Uruchom program Internet Explorer i połączyć się z portalem programu MIM na *http://mim.contoso.com/identitymanagement*. Podczas pierwszej wizyty na tej stronie może wystąpić krótkie opóźnienie.

    - Jeśli to konieczne, Uwierzytelnij się jako *contoso\miminstall* do programu Internet Explorer.

2. W programie Internet Explorer otwórz okno **Opcje internetowe**, wyświetl kartę **Zabezpieczenia** i dodaj witrynę do strefy **Lokalny intranet**, jeśli nie została jeszcze tam dodana.  Zamknij okno dialogowe **Opcje internetowe**.

3. Zezwól użytkownikom na wyświetlanie własnych wpisów w programie MIM.

    1.  Korzystając z programu Internet Explorer, w **portalu programu MIM** kliknij pozycję **Reguły zasad zarządzania**.

    2.  Wyszukaj regułę zasad zarządzania **Zarządzanie użytkownikami: użytkownicy mogą odczytywać atrybuty we własnym zakresie**.

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
> 
> [!div class="step-by-step"]  
> [« Usługa synchronizacji programu MIM](install-mim-sync.md)
> [Bazy danych synchronizacji »](install-mim-sync-ad-service.md)
