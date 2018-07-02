---
title: Agent zarządzania programu Microsoft Identity Manager dla programu Microsoft Graph | Dokumentacja firmy Microsoft
author: fimguy
description: Program Microsoft Graph (wersja zapoznawcza) jest zarządzanie cyklem życia AD konta użytkownika zewnętrznego. W tym scenariuszu organizacja zaprosiła gości do swojego katalogu usługi Azure AD i chce udostępnić te gości lokalnymi zintegrowanego uwierzytelniania systemu Windows lub aplikacji opartych na protokołu Kerberos
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a66d424e8388005855ac8e64623f5a00f89682e9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479370"
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>Agent zarządzania programu Microsoft Identity Manager dla programu Microsoft Graph (publicznej wersji zapoznawczej)
=======================================================================================

<a name="summary"></a>Podsumowanie 
=======

[Agenta zarządzania programu Microsoft Identity Manager dla programu Microsoft Graph (wersja zapoznawcza)](http://go.microsoft.com/fwlink/?LinkId=717495) umożliwia obsługę scenariuszy integracji dodatkowych dla klientów usługi Azure AD Premium.

[Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) integruje katalogów lokalnych z usługą Azure AD i zapewnia użytkownikom wspólną tożsamość i spójne uwierzytelniania w usługach AD DS, usługi Office 365, Azure i aplikacji SaaS zintegrowanych z usługą Azure AD za synchronizowanie użytkowników i grupy z katalogów lokalnych z usługą Azure AD.   Ten agent zarządzania można wdrożyć specjalne tożsamości i dostępu do operacji zarządzania poza użytkowników i grupy synchronizacji z usługą Azure AD.  Ten agent zarządzania udostępnia w MIM synchronizacji metaverse dodatkowe obiekty uzyskane z [interfejsu API programu Microsoft Graph](https://developer.microsoft.com/en-us/graph/) v1 i w wersji beta. 

<a name="scenarios-covered"></a>Omówione scenariusze
=================

<a name="b2b-account-lifecycle-management"></a>Zarządzanie cyklem życia B2B konta
--------------------------------

Scenariusz początkowej w wersji zapoznawczej dla agenta zarządzania programu Microsoft Identity Manager dla programu Microsoft Graph (wersja zapoznawcza) jest zarządzanie cyklem życia konta użytkownika zewnętrznego AD. W tym scenariuszu organizacja zaprosiła gości do swojego katalogu usługi Azure AD i chce udostępnić te gości lokalnymi zintegrowanego uwierzytelniania systemu Windows lub aplikacji opartych na protokołu Kerberos, za pomocą [aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)serwera proxy lub innych mechanizmów bramy. Serwer proxy aplikacji usługi Azure AD wymaga od każdego użytkownika ma swoje własne konta usług AD DS, dla celów identyfikacji i delegowanie

Dodatkowe scenariusze, które mogą być dodane w przyszłości i [opisanych tutaj](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>Określanie topologii wdrożenia
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>Przygotowanie do użycia Agent(MA) zarządzania dla programu Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Autoryzowanie MA zarządzać katalogiem usługi Azure AD
----------------------------------------------------

1.  Agent zarządzania wykres wymaga aplikacji sieci Web / aplikacja interfejsu API mogą być tworzone w AzureAD.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Rysunek 1. Nowej rejestracji aplikacji

2.  Otwórz utworzonej aplikacji i użyj Identyfikatora aplikacji, takich jak identyfikator klienta na stronie łączności MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Rysunek 2. Identyfikator aplikacji

2.  Generowanie nowego klucza tajnego klienta przez otwarcie wszystkich ustawień-\> kluczy. Opis niektórych klucza i wybraniu needful czasu trwania. Zapisz zmiany. Wartość tajna nie będzie dostępna po opuszczeniu strony.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Rysunek 3. Nowy klucz tajny klienta

3.  Dodaj "Interfejsu API programu Graph Microsoft" do aplikacji przez otwarcie "Wymagane uprawnienia."

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Rysunek 4. Dodaj nowy interfejs API

"Microsoft Graph API" dodaje następujących uprawnień:

| Operacja z obiektem | Wymagane uprawnienia                                                                  | Typ uprawnienia |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Grupa importu          | Group.Read.All lub Group.ReadWrite.All                                                | Aplikacji     |
| Importowanie użytkowników           | User.Read.All lub User.ReadWrite.All lub Directory.Read.All lub Directory.ReadWrite.All | Aplikacji     |

Więcej informacji na temat wymaganych uprawnień można odnaleźć [tutaj](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  Tworzenie łącznika z Identyfikatorem aplikacji i wygenerowanego agenta zarządzania Secret.Each klienta musi mieć własną aplikację w AzureAD, aby zapobiegać uruchamianiu importu równolegle dla tej samej aplikacji. Wykres łącznik obsługuje następujące listy typów obiektów:

-   Użytkownik

    -   Import pełnej/zmian

    -   Eksportuj (Dodawanie, aktualizowanie i usuwanie)

-   Grupa

    -   Import pełnej/zmian

    -   Eksportuj (Dodawanie, aktualizowanie i usuwanie)


Lista typów atrybutów, które są obsługiwane:

-   Typem Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (ciąg przestrzeni łącznika)

-   microsoft.graph.directoryObject (odwołanie w przestrzeni łącznika do wszystkich obsługiwanych obiektów)

-   Microsoft.Graph.Contact

Atrybutów wielowartościowych (kolekcja) są również obsługiwane dla żadnej z formularza typu listy wcześniej.

Wykres łącznik używa atrybutu 'id' zakotwiczenia i nazwę Wyświetlaną dla wszystkich obiektów.

Zmienianie nazw jest nieobsługiwane w tej chwili, ponieważ GraphAPI nie zezwala na zmiany obiektu do atrybutu 'id' dla obiektu istniał już.

<a name="access-token-lifetime"></a>Okres istnienia tokenu dostępu
=====================

Aplikacja Graph wymaga tokenu dostępu do uzyskiwania dostępu do GraphAPI. Łącznik będzie żądać nowy token dostępu dla każdej iteracji importu (import iteracji zależy od rozmiaru strony). Przykład:

-   AzureAD zawiera obiekty 10000

-   Rozmiar strony skonfigurowany w łączniku jest 5000

W takim przypadku będzie dwa iteracji podczas importowania, każdego z nich będzie zwracać 5 000 obiekty do synchronizacji. Tak nowy token dostępu będzie żądania dwa razy.

Należy pamiętać, że podczas eksportowania nowy token dostępu będzie wymagane dla każdego obiektu, który należy dodać/zaktualizować/usunąć.

<a name="installing-the-connector"></a>Instalowanie łącznika
========================

Przed użyciem łącznika, upewnij się, masz następujące czynności na serwerze synchronizacji: Microsoft .NET 4.5.2 ani nowszych Microsoft Identity Manager 2016 z dodatkiem SP1 musi używać poprawki 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) lub nowszym.

Łączników programu Microsoft Identity Manager 2016 z dodatkiem SP1, łącznik, wykres jest dostępny do pobrania z [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Konfiguracja łącznika
=======================

Strona połączenia:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Rysunek 5. Strona łączności

Strona łączności (obraz 1) zawiera wersję interfejsu API programu Graph jest używany i nazwa dzierżawy. Identyfikator klienta i klucz tajny klienta reprezentuje identyfikator aplikacji i wartości klucza muszą być tworzone w AzureAD aplikacji WebAPI.

Strona parametry globalne:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Rysunek 6. Strona parametrów globalnych

Parametry globalne strona zawiera następujące ustawienia:

Format daty i godziny — format używany dla każdego atrybutu z typem Edm.DateTimeOffset. Wszystkie daty są konwertowane na ciąg, używając tego formatu podczas importowania. Format zestawu jest stosowany dla każdego atrybutu, który zapisuje daty.

HTTP limit czasu (w sekundach) — limit czasu w sekundach, które będą używane podczas każdego wywołania HTTP WebAPI aplikacji.

Wymuś zmiany hasła dla utworzonego użytkownika w następnej znak — ta opcja jest używana dla nowego użytkownika, który zostanie utworzony podczas eksportowania. Jeśli opcja jest włączona, następnie [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) właściwość zostanie ustawiona wartość true, w przeciwnym razie będzie ona wartość false.

<a name="troubleshooting"></a>Rozwiązywanie problemów
===============

**Włączanie dzienników**

Jeśli występują problemy w wykresie, dzienniki można użyć do zlokalizowania problem. Łącznik wykres korzysta z tego samego źródła, tak jak wszystkie łączniki ogólnego. Tak, ślady może zostać włączona w [taki sam sposób jak ogólnego łączników można znaleźć w witrynie typu wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Lub po prostu dodając następujące miiserver.exe.config (wewnątrz sekcji system.diagnostics/sources):

\<Nazwa źródła = switchValue "ConnectorsLog" = "Pełne"\>

\<obiekty nasłuchujące\>

>   \<Dodaj initializeData — = "ConnectorsLog" type="System.Diagnostics.EventLogTraceListener systemu, wersja 4.0.0.0, Culture = neutral, PublicKeyToken = = b77a5c561934e089" Nazwa = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack,   Stos wywołań DateTime, Timestamp,"/\>

\<Usuń name = "Default" /\>

\</Listeners\>

\</ Source\>

Uwaga: Jeśli "Uruchom ten agent zarządzania w oddzielnych procesach" włączone, a następnie dllhost.exe.config powinna być używana zamiast funkcji miiserver.exe.config.

**Błąd wygasły token dostępu**

Łącznik może zwrócić HTTP 401 nieautoryzowane, komunikat "token dostępu upłynął.":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Rysunek 7. "Wygasł token dostępu". Error

Przyczyną tego problemu może być konfiguracja okres istnienia tokenu dostępu, ze strony systemu Azure. Domyślnie wygaśnięcia tokenu dostępu po godzinie. Aby zwiększyć czas wygaśnięcia, zobacz [w tym artykule](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Przykład użycia tego [Azure AD PowerShell modułu publicznej wersji zapoznawczej](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Nowe AzureADPolicy — definicja \@("{"TokenLifetimePolicy": {"Version": 1, **"AccessTokenLifetime":" 5: 00:00 "**}}") — Nazwa wyświetlana "OrganizationDefaultPolicyScenario" - IsOrganizationDefault \$true - typu "TokenLifetimePolicy"

<a name="next-steps"></a>Następne kroki
----------
- [Wykres Explorer (wielkiej problemów wywołania HTTP)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Przechowywanie wersji, obsługę i fundamentalne zmiany zasad dla programu Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Pobierz agenta zarządzania programu Microsoft Identity Manager dla programu Microsoft Graph (wersja zapoznawcza)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Przewodniki obsługiwanych określonego scenariusza
----------------------------------
[Wdrożenie pełnego B2B MIM]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
