---
title: Współpraca z raportowaniem hybrydowym na platformie Azure przy użyciu programu Identity Manager 2016 | Microsoft Docs
description: Dowiedz się, jak połączyć dane przechowywane lokalnie i w chmurze w raporty hybrydowe na platformie Azure oraz jak wyświetlać te raporty i jak nimi zarządzać.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: fd0efd3e3d5c42f4b67d0abd42f6dab8254573e5
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044348"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Współpraca z raportowaniem hybrydowym w programie Identity Manager

W tym artykule omówiono sposób łączenia danych lokalnych i w chmurze w raporty hybrydowe na platformie Azure oraz sposób zarządzania tymi raportami i ich wyświetlania.

## <a name="available-hybrid-reports"></a>Dostępne raporty hybrydowe
Pierwsze trzy Microsoft Identity Manager raporty dostępne w usłudze Azure Active Directory (Azure AD) są następujące:

- **Działanie resetowania hasła**: wyświetla każde wystąpienie, gdy użytkownik wykonał Resetowanie hasła przy użyciu funkcji samoobsługowego resetowania hasła (SSPR) i udostępnia bramy lub metody używane do uwierzytelniania.

- **Rejestracja resetowania hasła**: wyświetla się za każdym razem, gdy użytkownik rejestruje się w celu SSPR oraz metod używanych do uwierzytelniania. Przykładami metod mogą być numer telefonu komórkowego lub pytania i odpowiedzi.
   > [!NOTE]
   > W przypadku raportów dotyczących *rejestracji resetowania haseł* nie są wykonywane żadne rozróżnienia między bramą SMS i bramą MFA. Oba są uznawane za metody telefonii komórkowej.

- **Działania grup samoobsługi**: zawiera każdą próbę dodania lub usunięcia jej z grupy i utworzenia grupy.

    ![Raportowanie hybrydowe platformy Azure — obraz aktywności resetowania haseł](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Raporty obecnie zawierają dane dla maksymalnie jednego miesiąca działania.
> * Poprzedni Agent raportowania hybrydowego musi zostać odinstalowany.
> * Aby odinstalować raporty hybrydowe, Odinstaluj agenta agenta mimreportingagent. msi.

## <a name="prerequisites"></a>Wymagania wstępne

* Usługa Identity Manager 2016 SP1 programu Identity Manager, zalecana kompilacja [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Dzierżawa Azure AD — wersja Premium z licencjonowanym administratorem w katalogu.

* Wychodzące połączenie z Internetem z serwera programu Identity Manager na platformę Azure.

## <a name="requirements"></a>Wymagania
Wymagania dotyczące korzystania z funkcji raportowania hybrydowego programu Identity Manager przedstawiono w poniższej tabeli:


|                                         Wymaganie                                         |                                                                                                                                                                                                                                                                                    Description                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Raportowanie hybrydowe jest funkcją Azure AD — wersja Premium i wymaga Azure AD — wersja Premium. </br>Aby uzyskać więcej informacji, zobacz [wprowadzenie do Azure AD — wersja Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Zapoznaj się z [bezpłatną 30-dniową wersją próbną Azure AD — wersja Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Musisz być administratorem globalnym usługi Azure AD                     |                                                                   Domyślnie tylko Administratorzy globalni mogą instalować i konfigurować agentów, aby rozpocząć pracę, uzyskiwać dostęp do portalu i wykonywać dowolne operacje na platformie Azure. </br>**Ważne**: konto używane podczas instalowania agentów musi być kontem służbowym. Nie może to być konto Microsoft. Aby uzyskać więcej informacji, zobacz [Rejestrowanie się w usłudze Azure jako organizacja](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Agent hybrydowy programu Identity Manager jest zainstalowany na każdym serwerze usługi programu Identity Manager |                                                                                                                                                                                                       Aby otrzymywać dane i zapewnić możliwości monitorowania i analizy, raportowanie hybrydowe wymaga zainstalowania i skonfigurowania agentów na serwerach kierowanych.  </br>                                                                                                                                                                                                       |
|                    Łączność wychodząca z punktami końcowymi usługi platformy Azure                     | Podczas instalacji i wykonywania agent musi mieć łączność z punktami końcowymi usługi platformy Azure. Jeśli łączność wychodząca jest blokowana przez zapory, należy się upewnić, że następujące punkty końcowe zostały dodane do listy dozwolonych:<ul><li>\*. blob.core.windows.net </li><li>\*. servicebus.windows.net-port: 5671 </li><li>\*. adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Łączność wychodząca oparta na adresach IP                         |                                                                                                                                                                                                                      W przypadku filtrowania na podstawie adresów IP na zaporach zapoznaj się z [zakresami adresów IP platformy Azure](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 Inspekcja protokołu SSL dla ruchu wychodzącego jest filtrowana lub wyłączona                 |                                                                                                                                                                                                               Wykonywanie kroku rejestracji agenta lub operacji przekazywania danych może zakończyć się niepowodzeniem, jeśli istnieje Inspekcja SSL lub zakończenie dla ruchu wychodzącego w warstwie sieciowej.                                                                                                                                                                                                                |
|                      Porty zapory na serwerze, na którym jest uruchomiony Agent programu                       |                                                                                                                                                                                                          Aby można było komunikować się z punktami końcowymi usługi platformy Azure, Agent wymaga otwarcia następujących portów zapory:<ul><li>Port TCP 443</li><li>Port TCP 5671</li></ul>                                                                                                                                                                                                          |
|          Zezwalaj na niektóre witryny sieci Web w przypadku włączenia zwiększonych zabezpieczeń programu Internet Explorer           |                                                                                W przypadku włączenia zwiększonych zabezpieczeń programu Internet Explorer na serwerze, na którym zainstalowano agenta, muszą być dozwolone następujące witryny sieci Web:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>Serwer federacyjny dla organizacji ufa Azure Active Directory (na przykład <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Zainstaluj agenta raportowania programu Identity Manager w usłudze Azure AD
Po zainstalowaniu agenta raportowania dane z działania programu Identity Manager są eksportowane z programu Identity Manager do dziennika zdarzeń systemu Windows. Agent raportowania programu Identity Manager przetwarza zdarzenia, a następnie przekazuje je do platformy Azure. Na platformie Azure zdarzenia są analizowane, odszyfrowywane i filtrowane pod kątem wymaganych raportów.

1.  Zainstaluj program Identity Manager 2016.

2.  Pobierz agenta raportowania programu Identity Manager, a następnie wykonaj następujące czynności:

    a. Zaloguj się do portalu zarządzania usługi Azure AD, a następnie wybierz pozycję **Active Directory**.

    b. Kliknij dwukrotnie katalog, dla którego jesteś administratorem globalnym i masz subskrypcję Azure AD — wersja Premium.

    c. Wybierz pozycję **Konfiguracja**, a następnie pobierz agenta raportowania.

3.  Zainstaluj agenta raportowania, wykonując następujące czynności:

    a.  Pobierz [plik MIMHReportingAgentSetup. exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) dla serwera usługi programu Identity Manager.

    b.  Uruchom polecenie `MIMHReportingAgentSetup.exe`. 

    c.  Uruchom instalatora agenta.

    d.  Upewnij się, że jest uruchomiona usługa agenta raportowania programu Identity Manager.

    e.  Uruchom ponownie usługę Identity Manager.

4.  Sprawdź, czy Agent raportowania programu Identity Manager działa na platformie Azure.

    Aby zresetować hasło użytkownika, można utworzyć dane raportu przy użyciu portalu funkcji samoobsługowego resetowania haseł programu Identity Manager. Upewnij się, że Resetowanie hasła zakończyło się pomyślnie, a następnie sprawdź, czy dane są wyświetlane w portalu zarządzania usługi Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Wyświetlanie raportów hybrydowych w Azure Portal

1.  Zaloguj się do [Azure Portal](https://portal.azure.com/) przy użyciu konta administratora globalnego dzierżawy.

2.  Wybierz pozycję **Azure Active Directory**.

3.  Na liście dostępnych katalogów dla subskrypcji wybierz katalog dzierżawy.

4.  Wybierz pozycję **dzienniki inspekcji**.

5.  Z listy rozwijanej **Kategoria** upewnij się, że wybrano opcję **Usługa MIM** .

> [!IMPORTANT]
> Dane inspekcji programu Identity Manager mogą pojawić się w Azure Portal.

## <a name="stop-creating-hybrid-reports"></a>Zatrzymywanie tworzenia raportów hybrydowych
Jeśli chcesz zatrzymać przekazywanie danych inspekcji raportowania z programu Identity Manager do usługi Azure AD, Odinstaluj agenta raportowania hybrydowego. Użyj narzędzia Dodaj lub usuń programy systemu Windows, aby odinstalować raportowanie hybrydowe programu Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Zdarzenia systemu Windows używane na potrzeby raportowania hybrydowego
Zdarzenia generowane przez program Identity Manager są przechowywane w dzienniku zdarzeń systemu Windows. Zdarzenia w **Podgląd zdarzeń** można wyświetlić, wybierając pozycję **Dzienniki aplikacji i usług** > **dzienniku żądań programu Identity Manager**. Każde żądanie programu Identity Manager jest eksportowane jako zdarzenie w dzienniku zdarzeń systemu Windows w strukturze JSON. Możesz wyeksportować wynik do systemu informacji o zabezpieczeniach i zarządzania zdarzeniami (SIEM).

|Typ zdarzenia|Identyfikator|Szczegóły zdarzenia|
|--------------|------|-----------------|
|Informacje|4121|Dane zdarzenia programu Identity Manager, które obejmują wszystkie dane żądania.|
|Informacje|4137|Rozszerzenie zdarzenia 4121 programu Identity Manager w przypadku zbyt dużej ilości danych dla jednego zdarzenia. Nagłówek w tym zdarzeniu jest wyświetlany w następującym formacie: `"Request: <GUID> , message <xxx> out of <xxx>`.|
