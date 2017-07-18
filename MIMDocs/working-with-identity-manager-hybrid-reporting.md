---
title: "Praca z funkcją raportowania hybrydowego na platformie Azure przy użyciu programu MIM 2016 | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć dane przechowywane lokalnie i w chmurze w raporty hybrydowe na platformie Azure oraz jak wyświetlać te raporty i jak nimi zarządzać."
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: df842309034ad68151dd8cc4151507e7ece6626d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# Praca z funkcją raportowania hybrydowego programu Identity Manager — publiczna wersja zapoznawcza (odświeżanie)
<a id="working-with-identity-manager-hybrid-reporting---public-preview-refresh" class="xliff"></a>

## Dostępne raporty hybrydowe
<a id="available-hybrid-reports" class="xliff"></a>
Pierwsze trzy raporty programu Microsoft Identity Manager dostępne w usłudze Azure AD to **Aktywność resetowania haseł**, **Rejestracja resetowania haseł** i **Aktywność grup samoobsługi**.

-   Raport aktywności resetowania haseł obejmuje każde wystąpienie resetowania hasła przez użytkownika przy użyciu usługi SSPR oraz bramy lub **metody** używane na potrzeby uwierzytelniania.

-   Raport rejestracji resetowania haseł obejmuje każdą rejestrację użytkownika w usłudze SSPR oraz **metody** używane na potrzeby uwierzytelniania, na przykład numer telefonu komórkowego lub pytania i odpowiedzi.
    W przypadku rejestracji resetowania haseł brama SMS i brama MFA są traktowane jak **telefon komórkowy**.

-   Raport aktywności grup samoobsługi obejmuje każdą próbę dodania użytkownika do grupy, usunięcia użytkownika z grupy i utworzenia grupy.

    ![Raportowanie hybrydowe platformy Azure — obraz aktywności resetowania haseł](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> Obecnie raporty zawierają dane maksymalnie sprzed miesiąca.</br>
> Poprzedniego agenta hybrydowego należy odinstalować.</br>
> Aby odinstalować raporty hybrydowe, odinstaluj agenta MIMreportingAgent.msi.

## Wymagania wstępne
<a id="prerequisites" class="xliff"></a>

1.  Zainstaluj program Microsoft Identity Manager 2016 RTM lub SP1 z usługą MIM.

2.  Upewnij się, że masz dzierżawę usługi Azure AD Premium z licencjonowanym administratorem w katalogu.

3.  Upewnij się, że masz wychodzące połączenie z Internetem między serwerem programu Microsoft Identity Manager i platformą Azure.

## Wymagania
<a id="requirements" class="xliff"></a>
Poniższa tabela zawiera listę wymagań dotyczących korzystania z funkcji raportowania hybrydowego programu Microsoft Identity Manager.

| Wymaganie | Opis |
| --- | --- |
| Azure AD Premium | Raportowanie hybrydowe to funkcja usługi Azure AD Premium, która wymaga usługi Azure AD Premium. </br></br>Aby uzyskać więcej informacji, zobacz artykuł [Wprowadzenie do usługi Azure Active Directory — wersja Premium](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium). </br>Aby uruchomić bezpłatną 30-dniową wersję próbną, zobacz sekcję [Włączanie wersji próbnej](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Aby rozpocząć pracę, musisz być administratorem globalnym usługi Azure AD |Domyślnie tylko administratorzy globalni mogą instalować i konfigurować agentów do rozpoczynania pracy, uzyskiwania dostępu do portalu i wykonywania operacji na platformie Azure. </br></br>**Ważne:** konto używane do instalowania agentów musi być kontem służbowym. Nie może to być konto Microsoft. Aby uzyskać więcej informacji, zobacz artykuł [Sign up for Azure as an organization](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization) (Tworzenie nowego konta platformy Azure jako organizacja). |
| Hybrydowy agent programu Microsoft Identity Manager jest instalowany na każdym serwerze docelowym usługi MIM | Raportowanie hybrydowe wymaga, aby agenci byli instalowani na serwerach docelowych i konfigurowani do odbierania danych i udostępniania możliwości monitorowania oraz analizy. </br>|
| Łączność wychodząca z punktami końcowymi usługi platformy Azure | Podczas instalacji i wykonywania agent musi mieć łączność z punktami końcowymi usługi platformy Azure. Jeśli łączność wychodząca jest blokowana przy użyciu zapór, upewnij się, że następujące punkty końcowe zostały dodane do listy dozwolonych elementów: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Łączność wychodząca oparta na adresach IP | W przypadku filtrowania opartego na adresach IP przy użyciu zapór zapoznaj się z artykułem [Azure IP Ranges](https://www.microsoft.com/en-us/download/details.aspx?id=41653) (Zakresy adresów IP platformy Azure).|
| Inspekcja protokołu SSL dla ruchu wychodzącego jest filtrowana lub została wyłączona | Wykonywanie kroku rejestracji agenta lub operacji przekazywania danych może zakończyć się niepowodzeniem w przypadku przeprowadzania inspekcji protokołu SSL lub przerwania ruchu wychodzącego w warstwie sieciowej. |
| Porty zapory na serwerze z uruchomionym agentem |Aby agent mógł komunikować się z punktami końcowymi usługi platformy Azure, poniższe porty zapory muszą być otwarte.</br></br><li>Port TCP 443</li><li>Port TCP 5671</li> |
| Zezwolenie na następujące witryny sieci Web w przypadku włączenia zwiększonych zabezpieczeń programu Internet Explorer |W przypadku włączenia zwiększonych zabezpieczeń programu Internet Explorer korzystanie z poniższych witryn sieci Web musi być dozwolone na serwerze, na którym będzie instalowany agent.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Serwer federacyjny organizacji traktowany jako zaufany przez usługę Azure Active Directory, na przykład: https://sts.contoso.com</li> |
</BR>

## Instalowanie agenta raportowania programu Microsoft Identity Manager w usłudze Azure AD
<a id="install-microsoft-identity-manager-reporting-agent-in-azure-ad" class="xliff"></a>
Po zainstalowaniu agenta raportowania dane aktywności programu Microsoft Identity Manager są eksportowane z programu MIM do dziennika zdarzeń systemu Windows. Agent raportowania programu MIM przetwarza zdarzenia i przekazuje je do platformy Azure. Na platformie Azure zdarzenia są analizowane, odszyfrowywane i filtrowane pod kątem wymaganych raportów.

1.  Zainstaluj program Microsoft Identity Manager 2016.

2.  Pobierz agentów raportowania programu Microsoft Identity Manager:

    1.  Zaloguj się w portalu zarządzania usługi Azure AD i kliknij ikonę usługi Active Directory.

    2.  Kliknij dwukrotnie katalog, którego jesteś administratorem globalnym i dla którego masz subskrypcję usługi Azure AD Premium.

    3.  Kliknij pozycję **Konfiguracja** i pobierz agenta raportowania.

3.  Zainstaluj agenta raportowania programu Microsoft Identity Manager:

    1.  Pobierz plik [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) na serwer usługi Microsoft Identity Manager.
    2.  Uruchom polecenie `MIMHReportingAgentSetup.exe` 
    3.  Uruchom instalatora agenta.

    4.  Upewnij się, że jest uruchomiona usługa agenta raportowania MIM

    5.  Uruchom ponownie usługę MIM.

4.  Upewnij się, że funkcja raportowania programu Microsoft Identity Manager działa na platformie Azure.

    Aby utworzyć dane raportu, można zresetować hasło użytkownika za pomocą portalu samoobsługowego resetowania haseł programu Microsoft Identity Manager. Upewnij się, że pomyślnie ukończono resetowanie hasła, a następnie sprawdź, czy dane są wyświetlane w portalu zarządzania usługi Azure AD.

## Wyświetlanie raportów hybrydowych w witrynie Azure Portal
<a id="view-hybrid-reports-in-the-azure-portal" class="xliff"></a>

1.  Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/) przy użyciu konta administratora globalnego dzierżawy.

2.  Kliknij ikonę **Azure Active Directory**.

3.  Wybierz katalog dzierżawy z listy katalogów dostępnych dla Twojej subskrypcji.

4.  Kliknij pozycję **Dzienniki inspekcji**.

5.  Pamiętaj, aby wybrać pozycję **Usługa MIM** z menu rozwijanego Kategoria.

> [!WARNING]
> Dane inspekcji programu Microsoft Identity Manager mogą pojawić się w witrynie Azure Portal dopiero po pewnym czasie.

## Zatrzymywanie tworzenia raportów hybrydowych
<a id="stop-creating-hybrid-reports" class="xliff"></a>
Aby zatrzymać przekazywanie danych raportowania inspekcji z programu Microsoft Identity Manager do usługi Azure Active Directory, odinstaluj agenta raportowania hybrydowego. Użyj narzędzia **Dodaj lub usuń programy** systemu Windows, aby odinstalować funkcję raportowania hybrydowego programu Microsoft Identity Manager.

## Zdarzenia systemu Windows używane na potrzeby raportowania hybrydowego
<a id="windows-events-used-for-hybrid-reporting" class="xliff"></a>
Zdarzenia generowane przez program Microsoft Identity Manager są rejestrowane w dzienniku zdarzeń systemu Windows i są widoczne w Podglądzie zdarzeń w obszarze Dzienniki aplikacji i usług -&gt; **Dziennik żądań programu Identity Manager**. Każde żądanie programu MIM jest eksportowane jako zdarzenie w dzienniku zdarzeń systemu Windows w strukturze JSON. Takie zdarzenia można wyeksportować do rozwiązania SIEM.

|Typ zdarzenia|ID|Szczegóły zdarzenia|
|--------------|------|-----------------|
|Informacje|4121|Dane zdarzeń programu MIM obejmujące wszystkie dane żądań.|
|Informacje|4137|Rozszerzenie zdarzenia 4121 programu MIM na wypadek zbyt dużej ilości danych dla jednego zdarzenia. Nagłówek w tym zdarzeniu ma następujący format: `"Request: <GUID> , message <xxx> out of <xxx>`|
