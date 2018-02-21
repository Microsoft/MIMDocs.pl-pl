---
title: "Praca z raportowanie hybrydowe na platformie Azure przy użyciu programu Identity Manager 2016 | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć dane przechowywane lokalnie i w chmurze w raporty hybrydowe na platformie Azure oraz jak wyświetlać te raporty i jak nimi zarządzać."
keywords: 
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: e135cc5066220765d97568b3a1e1b984a876b2a2
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/20/2018
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Praca z raportowaniem w Identity Manager hybrydowym

W tym artykule omówiono sposób łączenia lokalnej i danych w chmurze w raporty hybrydowe na platformie Azure i sposobu zarządzania i wyświetlać te raporty.

## <a name="available-hybrid-reports"></a>Dostępne raporty hybrydowe
Pierwsze trzy raporty programu Microsoft Identity Manager dostępne w usłudze Azure Active Directory (Azure AD), są następujące:

- **Aktywność resetowania haseł**: Wyświetla każde wystąpienie użytkownika resetowania hasła przy użyciu samodzielnego resetowania hasła (SSPR) oraz bramy lub metody używane do uwierzytelniania.

- **Rejestracja resetowania haseł**: wyświetla za każdym razem, użytkownik rejestruje SSPR i metody używane do uwierzytelniania. Przykładami metod może być numer telefonu komórkowego lub pytania i odpowiedzi.
   > [!NOTE]
   > Aby uzyskać *rejestracji resetowania haseł* raporty rozróżnienia staje się brama SMS i brama MFA. Są traktowane jak telefon komórkowy metody.

- **Aktywność grup samoobsługi**: Wyświetla każdą próbę dodania, usuń go lub siebie z grupy i utworzenia grupy.

    ![Raportowanie hybrydowe platformy Azure — obraz aktywności resetowania haseł](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Obecnie raporty zawierają dane przez maksymalnie miesiąc działania.
> * Należy odinstalować jego poprzednie agenta raportowania hybrydowego.
> * Aby odinstalować raporty hybrydowe, odinstaluj agenta MIMreportingAgent.msi.

## <a name="prerequisites"></a>Wymagania wstępne

* Usługa programu Identity Manager Identity Manager 2016 z dodatkiem SP1, zalecane kompilacji [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Dzierżawy usługi Azure AD Premium z licencjonowanym administratorem w katalogu.

* Wychodzące połączenie z Internetem między serwerem programu Identity Manager na platformie Azure.

## <a name="requirements"></a>Wymagania
W poniższej tabeli przedstawiono wymagania dotyczące korzystania z usługi Raportowanie hybrydowe programu Identity Manager:

| Wymaganie | Opis |
| --- | --- |
| Azure AD Premium | Raportowanie hybrydowe to funkcja usługi Azure AD Premium i Azure AD Premium wymaga. </br>Aby uzyskać więcej informacji, zobacz [wprowadzenie do korzystania z usługi Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Pobierz [wolne 30-dniową wersją próbną usługi Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Musi być administratorem globalnym usługi Azure AD |Domyślnie tylko administratorzy globalni można zainstalować i skonfigurować agentów, aby rozpocząć pracę, dostępu do portalu i wykonywać dowolne operacje w usłudze Azure. </br>**Ważne**: konto, który jest używany podczas instalowania agentów musi być konta firmowego lub szkolnego. Nie może to być konto Microsoft. Aby uzyskać więcej informacji, zobacz [tworzenia konta platformy Azure jako organizacja](https://docs.microsoft.com/azure/active-directory/sign-up-organization). |
| Agent hybrydowe programu Identity Manager jest zainstalowany na każdym serwerze docelowym Usługa programu Identity Manager | Aby odbierać dane i zapewniają możliwości monitorowania i analizy, raportowanie hybrydowe wymaga agentów należy zainstalować i skonfigurować na serwerach docelowych.  </br>|
| Łączność wychodząca z punktami końcowymi usługi platformy Azure | Podczas instalacji i wykonywania agent musi mieć łączność z punktami końcowymi usługi platformy Azure. Łączność wychodząca jest blokowany przez zapory, upewnij się, że następujące punkty końcowe są dodawane do listy dozwolonych:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li></ul> |
|Łączność wychodząca na podstawie adresów IP | Dla adresów IP na podstawie filtrowania na zaporach, zobacz [zakresów adresów IP Azure](https://www.microsoft.com/download/details.aspx?id=41653).|
| Inspekcji SSL dla ruchu wychodzącego jest filtrowana lub wyłączony | Agent rejestracji krok lub dane przekazywania operacje może zakończyć się niepowodzeniem w przypadku inspekcji SSL lub zakończenia dla ruchu wychodzącego w warstwie sieci. |
| Porty zapory na serwerze z uruchomioną agenta | Aby komunikować się z punktami końcowymi usług Azure, agent wymaga być otwarte poniższe porty zapory:<ul><li>Port TCP 443</li><li>Port TCP 5671</li></ul> |
| Zezwalaj na niektórych witryn sieci Web, jeśli program Internet Explorer zwiększonych zabezpieczeń jest włączona. |Jeśli program Internet Explorer rozszerzony zabezpieczeń jest włączona, może następujące witryny sieci Web na serwerze, na którym został zainstalowany agent:<ul><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Serwer federacyjny Twojej organizacji zaufany przez usługę Azure Active Directory (na przykład https://sts.contoso.com).</li></ul> |
</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Zainstaluj agenta raportowania programu Identity Manager w usłudze Azure AD
Po zainstalowaniu agenta raportowania dane aktywności programu Identity Manager są eksportowane z programu Identity Manager do dziennika zdarzeń systemu Windows. Agent raportowania programu Identity Manager przetwarza zdarzenia i przekazuje je do platformy Azure. Na platformie Azure zdarzenia są analizowane, odszyfrowywane i filtrowane pod kątem wymaganych raportów.

1.  Zainstaluj Identity Manager 2016.

2.  Pobierz agenta raportowania programu Identity Manager, a następnie wykonaj następujące czynności:

    a. Zaloguj się do portalu zarządzania usługi Azure AD, a następnie wybierz **usługi Active Directory**.

    b. Kliknij dwukrotnie katalog, dla którego jest administratorem globalnym i mieć subskrypcję usługi Azure AD Premium.

    c. Wybierz **konfiguracji**, a następnie Pobierz agenta raportowania.

3.  Zainstaluj agenta raportowania, wykonując następujące czynności:

    a.  Pobierz [pliku MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) dla serwera usługi programu Identity Manager.

    b.  Uruchom polecenie `MIMHReportingAgentSetup.exe`. 

    c.  Uruchom instalatora agenta.

    d.  Upewnij się, że usługa agenta raportowania programu Identity Manager jest uruchomiona.

    e.  Uruchom ponownie usługę programu Identity Manager.

4.  Sprawdź, czy Agent raportowania programu Identity Manager działa na platformie Azure.

    Można utworzyć raport danych przy użyciu hasła samoobsługi Identity Manager resetowania portalu, aby zresetować hasło użytkownika. Upewnij się, że resetowania hasła została ukończona pomyślnie, a następnie sprawdź, upewnij się, że dane są wyświetlane w portalu zarządzania usługi Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Wyświetlanie raportów hybrydowych w portalu Azure

1.  Zaloguj się do [portalu Azure](https://portal.azure.com/) przy użyciu konta administratora globalnego dla dzierżawcy.

2.  Wybierz **usługi Azure Active Directory**.

3.  Na liście katalogów dostępnych dla Twojej subskrypcji wybierz katalog dzierżawy.

4.  Wybierz **dzienniki inspekcji**.

5.  W **kategorii** rozwijania listy, upewnij się, że **usługi MIM** jest zaznaczone.

> [!IMPORTANT]
> Może upłynąć pewien czas Identity Manager dane inspekcji, które mają być widoczne w portalu Azure.

## <a name="stop-creating-hybrid-reports"></a>Zatrzymywanie tworzenia raportów hybrydowych
Jeśli chcesz zatrzymać przekazywanie inspekcji danych raportowania z programu Identity Manager do usługi Azure AD, odinstaluj agenta raportowania hybrydowego. Aby odinstalować raportowanie hybrydowe programu Identity Manager, należy użyć narzędzia Windows Dodaj lub usuń programy.

## <a name="windows-events-used-for-hybrid-reporting"></a>Zdarzenia systemu Windows używane na potrzeby raportowania hybrydowego
Zdarzenia, które są generowane przez Identity Manager są przechowywane w dzienniku zdarzeń systemu Windows. Można wyświetlać zdarzenia w **Podgląd zdarzeń** wybierając **Dzienniki aplikacji i usług** > **Dziennik żądań programu Identity Manager**. Każde żądanie programu Identity Manager są eksportowane jako zdarzenie w dzienniku zdarzeń systemu Windows w strukturze JSON. Wynik można wyeksportować do systemu zarządzania (SIEM) informacje i zdarzeń zabezpieczeń.

|Typ zdarzenia|ID|Szczegóły zdarzenia|
|--------------|------|-----------------|
|Informacje|4121|Dane zdarzeń programu Identity Manager obejmuje wszystkie dane żądań.|
|Informacje|4137|Rozszerzenie zdarzenia 4121 programu Identity Manager, jeśli istnieje za dużo danych dla pojedynczego zdarzenia. Nagłówek w tym przypadku jest wyświetlany w następującym formacie: `"Request: <GUID> , message <xxx> out of <xxx>`.|
