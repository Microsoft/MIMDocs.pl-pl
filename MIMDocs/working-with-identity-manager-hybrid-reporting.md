---
title: Praca z raportowanie hybrydowe na platformie Azure przy użyciu programu Identity Manager 2016 | Dokumentacja firmy Microsoft
description: Dowiedz się, jak połączyć dane przechowywane lokalnie i w chmurze w raporty hybrydowe na platformie Azure oraz jak wyświetlać te raporty i jak nimi zarządzać.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 18e4127b1d854a53734142bb58442627619491ef
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358537"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Praca z raportowaniem w programie Identity Manager hybrydowym

W tym artykule omówiono, jak połączyć lokalną i danymi w chmurze w raporty hybrydowe na platformie Azure i zarządzać oraz jak wyświetlać te raporty.

## <a name="available-hybrid-reports"></a>Dostępne raporty hybrydowe
Pierwsze trzy raporty programu Microsoft Identity Manager dostępne w usłudze Azure Active Directory (Azure AD), są następujące:

- **Aktywność resetowania haseł**: Wyświetla każde wystąpienie, gdy użytkownik wykona resetowania haseł za pomocą Samoobsługowego resetowania haseł (SSPR) oraz bramy lub metody używane do uwierzytelniania.

- **Rejestracja resetowania haseł**: wyświetla za każdym razem, użytkownik rejestruje dla funkcji samoobsługowego resetowania HASEŁ i metody używane do uwierzytelniania. Przykładami metod może być numer telefonu komórkowego lub pytania i odpowiedzi.
   > [!NOTE]
   > Aby uzyskać *rejestracji resetowania haseł* raporty i wykonano rozróżnienia między brama SMS i brama MFA. Oba są traktowane jako metody telefonu komórkowego.

- **Aktywność grup samoobsługi**: Wyświetla każdej próby dokonywane przez osobę, aby dodać lub usunąć siebie z grupy i utworzenia grupy.

    ![Raportowanie hybrydowe platformy Azure — obraz aktywności resetowania haseł](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Obecnie raporty zawierają dane przez maksymalnie miesiąc działania.
> * Poprzedniego agenta raportowania hybrydowego należy odinstalować.
> * Aby odinstalować raporty hybrydowe, odinstaluj agenta MIMreportingAgent.msi.

## <a name="prerequisites"></a>Wymagania wstępne

* Usługa programu Identity Manager programu Identity Manager 2016 z dodatkiem SP1, zalecane kompilacji [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Dzierżawy usługi Azure AD Premium z licencjonowanym administratorem w katalogu.

* Wychodzące połączenie z Internetem między serwerem programu Identity Manager na platformie Azure.

## <a name="requirements"></a>Wymagania
Wymagania dotyczące korzystania z raportowanie hybrydowe programu Identity Manager są wymienione w poniższej tabeli:


|                                         Wymaganie                                         |                                                                                                                                                                                                                                                                                    Opis                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Raportowanie hybrydowe to funkcja usługi Azure AD Premium i wymaga usługi Azure AD Premium. </br>Aby uzyskać więcej informacji, zobacz [wprowadzenie do usługi Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Pobierz [bezpłatnej 30-dniowej wersji próbnej usługi Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Musisz być administratorem globalnym usługi Azure AD                     |                                                                   Domyślnie tylko administratorzy globalni można zainstalować i skonfigurować agentów, aby rozpocząć pracę, uzyskać dostęp do portalu i wykonywać dowolne operacje w obrębie platformy Azure. </br>**Ważne**: konto, którego używasz, aby podczas instalowania agentów musi być konta firmowego lub szkolnego. Nie może to być konto Microsoft. Aby uzyskać więcej informacji, zobacz [konta na platformie Azure jako organizacja](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Agent hybrydowe programu Identity Manager jest zainstalowany na każdym serwerze docelowym usługi Identity Manager |                                                                                                                                                                                                       Aby odbierać dane i zapewniają możliwości monitorowania i analizy, raportowanie hybrydowe wymaga agentów, aby zainstalować i skonfigurować na serwerach docelowych.  </br>                                                                                                                                                                                                       |
|                    Łączność wychodząca z punktami końcowymi usługi platformy Azure                     | Podczas instalacji i wykonywania agent musi mieć łączność z punktami końcowymi usługi platformy Azure. Jeśli łączność wychodząca jest blokowana przez zapory, upewnij się, że następujące punkty końcowe są dodawane do listy lokalizacji dozwolonych:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Łączność wychodząca na podstawie adresów IP                         |                                                                                                                                                                                                                      Filtrowanie oparte na zaporach adresów IP, znaleźć [zakresów adresów IP Azure](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 Inspekcja połączenia SSL dla ruchu wychodzącego jest filtrowana lub wyłączona                 |                                                                                                                                                                                                               Agent rejestracji krok lub operacje przekazywania danych może zakończyć się niepowodzeniem w przypadku zakończenia dla ruchu wychodzącego w warstwie sieciowej lub inspekcji połączenia SSL.                                                                                                                                                                                                                |
|                      Porty zapory na serwerze z uruchomionym agentem                       |                                                                                                                                                                                                          Aby komunikować się z punktami końcowymi usługi platformy Azure, agent wymaga poniższe porty zapory, muszą być otwarte:<ul><li>Port TCP 443</li><li>Port TCP 5671</li></ul>                                                                                                                                                                                                          |
|          Zezwalaj na pewność, że witryny sieci Web, jeśli program Internet Explorer podnoszą poziom zabezpieczeń jest włączone.           |                                                                                Jeśli program Internet Explorer rozszerzony zabezpieczenia są włączone, muszą być dozwolone następujące witryny sieci Web na serwerze, na którym został zainstalowany agent:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>Serwer federacyjny w organizacji zaufany przez usługę Azure Active Directory (na przykład <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Zainstaluj agenta raportowania programu Identity Manager w usłudze Azure AD
Po zainstalowaniu agenta raportowania dane aktywności programu Identity Manager są eksportowane z programu Identity Manager do dziennika zdarzeń Windows. Agent raportowania programu Identity Manager przetwarza zdarzenia i przekazuje je do platformy Azure. Na platformie Azure zdarzenia są analizowane, odszyfrowywane i filtrowane pod kątem wymaganych raportów.

1.  Instalowanie programu Identity Manager 2016.

2.  Pobierz agenta raportowania programu Identity Manager, a następnie wykonaj następujące czynności:

    a. Zaloguj się do portalu zarządzania usługi Azure AD, a następnie wybierz **usługi Active Directory**.

    b. Kliknij dwukrotnie katalog, dla którego jesteś administratorem globalnym i masz subskrypcję usługi Azure AD Premium.

    c. Wybierz **konfiguracji**, a następnie Pobierz agenta raportowania.

3.  Zainstaluj agenta raportowania, wykonując następujące czynności:

    a.  Pobierz [pliku MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) dla serwera usługi programu Identity Manager.

    b.  Uruchom polecenie `MIMHReportingAgentSetup.exe`. 

    c.  Uruchom instalatora agenta.

    d.  Upewnij się, że usługa agenta raportowania programu Identity Manager jest uruchomiona.

    e.  Uruchom ponownie usługę programu Identity Manager.

4.  Sprawdź, czy Agent raportowania programu Identity Manager działa na platformie Azure.

    Można utworzyć raport resetowania danych za pomocą programu Identity Manager samoobsługowego hasła portalu można zresetować hasła użytkownika. Upewnij się, czy Resetowanie hasła zakończyło się pomyślnie, a następnie sprawdź, upewnij się, że dane są wyświetlane w portalu zarządzania usługi Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Wyświetlanie raportów hybrydowych w witrynie Azure portal

1.  Zaloguj się do [witryny Azure portal](https://portal.azure.com/) przy użyciu konta administratora globalnego dla dzierżawy.

2.  Wybierz **usługi Azure Active Directory**.

3.  Na liście dostępnych katalogów dla Twojej subskrypcji wybierz katalog dzierżawy.

4.  Wybierz **dzienniki inspekcji**.

5.  W **kategorii** listy rozwijanej liście, upewnij się, że **usługi MIM** jest zaznaczone.

> [!IMPORTANT]
> Może potrwać pewien czas programu Identity Manager inspekcji dane zostaną wyświetlone w witrynie Azure portal.

## <a name="stop-creating-hybrid-reports"></a>Zatrzymywanie tworzenia raportów hybrydowych
Jeśli chcesz zatrzymać przekazywanie danych raportowania inspekcji z programu Identity Manager do usługi Azure AD, odinstaluj agenta raportowania hybrydowego. Aby odinstalować raportowanie hybrydowe programu Identity Manager, należy użyć narzędzia Windows Dodaj lub usuń programy.

## <a name="windows-events-used-for-hybrid-reporting"></a>Zdarzenia systemu Windows używane na potrzeby raportowania hybrydowego
Zdarzenia, które są generowane przez programu Identity Manager są przechowywane w dzienniku zdarzeń Windows. Można wyświetlać zdarzenia w **Podgląd zdarzeń** , wybierając **Dzienniki aplikacji i usług** > **Dziennik żądań programu Identity Manager**. Każde żądanie programu Identity Manager są eksportowane jako zdarzenie w dzienniku zdarzeń Windows w strukturze JSON. Można wyeksportować wyniki do zabezpieczeń informacjami i zdarzeniami (SIEM) systemu zarządzania.

|Typ zdarzenia|ID|Szczegóły zdarzenia|
|--------------|------|-----------------|
|Informacje|4121|Dane zdarzenia programu Identity Manager obejmuje wszystkie dane żądania.|
|Informacje|4137|Rozszerzenie zdarzenia 4121 programu Identity Manager, jeśli ma zbyt dużej ilości danych dla jednego zdarzenia. Nagłówek w tym przypadku jest wyświetlany w następującym formacie: `"Request: <GUID> , message <xxx> out of <xxx>`.|
