---
title: Raportowanie hybrydowe na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak połączyć dane przechowywane lokalnie i w chmurze w raporty hybrydowe na platformie Azure oraz jak wyświetlać te raporty i jak nimi zarządzać."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: ff0469da204a9bfa861273d66b04f5da51557c99


---

# <a name="working-with-identity-manager-hybrid-reporting"></a>Praca z raportowaniem hybrydowym programu Identity Manager

## <a name="available-hybrid-reports"></a>Dostępne raporty hybrydowe
Pierwsze trzy raporty programu Microsoft Identity Manager dostępne w usłudze Azure AD to **Aktywność resetowania haseł**, **Rejestracja resetowania haseł** i **Aktywność grup samoobsługi**.

-   Raport aktywności resetowania haseł obejmuje każde wystąpienie resetowania hasła przez użytkownika przy użyciu usługi SSPR oraz bramy lub **metody** używane na potrzeby uwierzytelniania.

    ![Raportowanie hybrydowe platformy Azure — obraz aktywności resetowania haseł](media/MIM-Hybrid-passwordreset.jpg)

-   Raport rejestracji resetowania haseł obejmuje każdą rejestrację użytkownika w usłudze SSPR oraz **metody** używane na potrzeby uwierzytelniania, na przykład numer telefonu komórkowego lub pytania i odpowiedzi.
    W przypadku rejestracji resetowania haseł brama SMS i brama MFA są traktowane jak **telefon komórkowy**.

-   Raport aktywności grup samoobsługi obejmuje każdą próbę dodania użytkownika do grupy, usunięcia użytkownika z grupy i utworzenia grupy.

> [!NOTE]
> Obecnie raporty zawierają dane maksymalnie sprzed miesiąca.
>
> Aby odinstalować raporty hybrydowe, odinstaluj agenta MIMreportingAgent.msi.

## <a name="prerequisites"></a>Wymagania wstępne

1.  Zainstaluj program Microsoft Identity Manager 2016 z usługą MIM.

2.  Upewnij się, że masz dzierżawę usługi Azure AD Premium z licencjonowanym administratorem w katalogu.

3.  Upewnij się, że masz wychodzące połączenie z Internetem między serwerem programu Microsoft Identity Manager i platformą Azure.

## <a name="install-microsoft-identity-manager-reporting-in-azure-ad"></a>Instalowanie funkcji raportowania programu Microsoft Identity Manager w usłudze Azure AD
Po zainstalowaniu agenta raportowania dane aktywności programu Microsoft Identity Manager są eksportowane z programu MIM do dziennika zdarzeń systemu Windows. Agent raportowania programu MIM przetwarza zdarzenia i przekazuje je do platformy Azure. Na platformie Azure zdarzenia są analizowane, odszyfrowywane i filtrowane pod kątem wymaganych raportów.

1.  Zainstaluj program Microsoft Identity Manager 2016.

2.  Pobierz agentów raportowania programu Microsoft Identity Manager:

    1.  Zaloguj się w portalu zarządzania usługi Azure AD i kliknij ikonę usługi Active Directory.

    2.  Kliknij dwukrotnie katalog, którego jesteś administratorem globalnym i dla którego masz subskrypcję usługi Azure AD Premium.

    3.  Kliknij pozycję **Konfiguracja** i pobierz agenta raportowania.

3.  Zainstaluj agenta raportowania programu Microsoft Identity Manager:

    1.  Utwórz katalog na komputerze.

    2.  Wyodrębnij do tego katalogu pliki `MIMHybridReportingAgent.msi` i `tenant.cert`.

    3.  Uruchom instalatora agenta.

    4.  Upewnij się, że jest uruchomiona usługa agenta raportowania MIM

    5.  Uruchom ponownie usługę MIM.

4.  Upewnij się, że funkcja raportowania programu Microsoft Identity Manager działa na platformie Azure.

    Aby utworzyć dane raportu, można zresetować hasło użytkownika za pomocą portalu samoobsługowego resetowania haseł programu Microsoft Identity Manager. Upewnij się, że pomyślnie ukończono resetowanie hasła, a następnie sprawdź, czy dane są wyświetlane w portalu zarządzania usługi Azure AD.

## <a name="view-hybrid-reports-in-the-azure-classic-portal"></a>Wyświetlanie raportów hybrydowych w klasycznym portalu platformy Azure

1.  Zaloguj się w [klasycznym portalu platformy Azure](https://manage.windowsazure.com/) przy użyciu konta administratora globalnego dzierżawy.

2.  Kliknij ikonę **Active Directory**.

3.  Wybierz katalog dzierżawy z listy katalogów dostępnych dla Twojej subskrypcji.

4.  Kliknij pozycję **Raporty**, a następnie pozycję **Aktywność resetowania haseł**.

5.  Upewnij się, że wybrano pozycję **Identity Manager** w menu rozwijanym źródła.

> [!WARNING]
> Dane programu Microsoft Identity Manager mogą być wyświetlane w usłudze Azure AD dopiero po pewnym czasie.

## <a name="stop-creating-hybrid-reports"></a>Zatrzymywanie tworzenia raportów hybrydowych
Aby zatrzymać przekazywanie danych raportowania z programu Microsoft Identity Manager do usługi Azure Active Directory, odinstaluj agenta raportowania hybrydowego. Użyj narzędzia **Dodaj lub usuń programy** systemu Windows, aby odinstalować funkcję raportowania hybrydowego programu Microsoft Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Zdarzenia systemu Windows używane na potrzeby raportowania hybrydowego
Zdarzenia generowane przez program Microsoft Identity Manager są rejestrowane w dzienniku zdarzeń systemu Windows i są widoczne w Podglądzie zdarzeń w obszarze Dzienniki aplikacji i usług -&gt; **Dziennik żądań programu Identity Manager**. Każde żądanie programu MIM jest eksportowane jako zdarzenie w dzienniku zdarzeń systemu Windows w strukturze JSON. Takie zdarzenia można wyeksportować do rozwiązania SIEM.

|Typ zdarzenia|ID|Szczegóły zdarzenia|
|--------------|------|-----------------|
|Informacje|4121|Dane zdarzeń programu MIM obejmujące wszystkie dane żądań.|
|Informacje|4137|Rozszerzenie zdarzenia 4121 programu MIM na wypadek zbyt dużej ilości danych dla jednego zdarzenia. Nagłówek w tym zdarzeniu ma następujący format: `"Request: <GUID> , message <xxx> out of <xxx>`|



<!--HONumber=Nov16_HO2-->


