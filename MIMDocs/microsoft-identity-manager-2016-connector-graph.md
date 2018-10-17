---
title: Łącznik programu Microsoft Identity Manager dla programu Microsoft Graph | Dokumentacja firmy Microsoft
author: fimguy
description: Łącznik programu Microsoft Identity Manager dla programu Microsoft Graph umożliwia użytkownikowi zewnętrznemu zarządzania cyklem życia konta usługi AD. W tym scenariuszu organizacja ma zaproszenie gości do swojego katalogu usługi Azure AD i chce udzielić tych dostępu gości do uwierzytelniania Windows-Integrated lokalnego lub aplikacje oparte na protokołu Kerberos
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2e376bcc88518b911f93ce9cd4ab920eb428815b
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358656"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Łącznik programu Microsoft Identity Manager dla programu Microsoft Graph
=======================================================================================

<a name="summary"></a>Podsumowanie 
=======

[Łącznika programu Microsoft Identity Manager dla programu Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) umożliwia integracji dodatkowych scenariuszy dla klientów usługi Azure AD Premium.  Udostępnia on w MIM sync metaverse dodatkowe obiekty uzyskany z [interfejsu API Microsoft Graph](https://developer.microsoft.com/en-us/graph/) v1 i beta.

<a name="scenarios-covered"></a>Omówione scenariusze
=================

<a name="b2b-account-lifecycle-management"></a>Zarządzanie cyklem życia konta B2B
--------------------------------

Początkowa scenariusz dla łącznika programu Microsoft Identity Manager dla programu Microsoft Graph jest jako łącznik, które ułatwiają Automatyzowanie zarządzania cyklem życia konto usług AD DS dla użytkowników zewnętrznych. W tym scenariuszu organizacja synchronizuje pracowników do usługi Azure AD z usług AD DS za pomocą usługi Azure AD Connect, a także zaprosił gości do swojego katalogu usługi Azure AD. Zapraszanie gościa skutkuje obiektu użytkownika zewnętrznego w katalogu usługi Azure AD w organizacji, która znajduje się w usługach AD DS w organizacji. Następnie organizacja chce udostępnić te gości zintegrowane uwierzytelnianie Windows w środowisku lokalnym lub w przypadku aplikacji opartych na protokołu Kerberos za pomocą [serwera proxy aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) lub innych mechanizmów bramy. Serwer proxy aplikacji usługi Azure AD wymaga każdy użytkownik musi mieć własne konto usług AD DS na potrzeby identyfikacji i delegowania.  

Aby dowiedzieć się, jak skonfigurować automatyczne tworzenie i obsługa kont usług AD DS dla gości, po zapoznaniu się z instrukcjami w tym artykule Usługa synchronizacji programu MIM Czytaj dalej w artykule [współpracy business-to-business (B2B) usługi Azure AD przy użyciu programu MIM 2016 z dodatkiem SP1 za pomocą serwera Proxy aplikacji platformy Azure](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Ten artykuł przedstawia reguły synchronizacji wymagane dla łącznika.

<a name="other-identity-management-scenarios"></a>Inne scenariusze zarządzania tożsamościami
---------------

Łącznik może służyć do zarządzania określonej tożsamości, innych scenariuszy obejmujących tworzenie, Odczyt, aktualizowanie i usuwanie obiektów użytkowników, grup i skontaktuj się z pomocą w usłudze Azure AD, poza użytkowników i grup synchronizacji z usługą Azure AD. Podczas oceny potencjalnych scenariuszach, należy pamiętać: ten łącznik nie może działać w scenariuszu wynik w przepływie danych podsieć nakładałaby, rzeczywiste lub potencjalne synchronizacji w konflikcie z wdrożenia usługi Azure AD Connect.  [Program Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) przedstawia zalecane podejście do integracji katalogów lokalnych z usługą Azure AD przez synchronizację użytkowników i grup z katalogów lokalnych z usługą Azure AD.  Program Azure AD Connect ma wiele innych funkcji synchronizacji i umożliwia obsługę scenariuszy takich jak zapisywanie zwrotne haseł i urządzeń, które nie są możliwe dla obiektów utworzonych przez program MIM. Jeśli dane są przeznaczone do usług AD DS, na przykład, upewnij się, że jest ona wykluczana z usługi Azure AD Connect, próbując dopasować te obiekty do katalogu usługi Azure AD.  Ani ten łącznik służy do zmiany obiektów usługi Azure AD, które zostały utworzone przy użyciu usługi Azure AD Connect.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Przygotowanie do korzystania z łącznika dla programu Microsoft Graph
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autoryzowanie łącznika można pobrać lub zarządzania obiektami w katalogu usługi Azure AD
----------------------------------------------------

1.  Łącznik wymaga, aby aplikacja sieci Web lub aplikacji interfejsu API utworzony w usłudze Azure AD, dzięki czemu może być autoryzowane przy użyciu odpowiednich uprawnień do działania w usłudze Azure AD obiektów za pomocą programu Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Rysunek 1. Rejestrowanie nowej aplikacji

2.  W witrynie Azure portal Otwórz utworzonej aplikacji, a następnie zapisz identyfikator aplikacji, jako identyfikator klienta do użycia później MA łączność strony:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Rysunek 2. Identyfikator aplikacji

3.  Generowanie nowego klucza tajnego klienta, otwierając wszystkie ustawienia-\> kluczy. Ustaw miejsce na opis klucza, a następnie wybierz needful czasu trwania. Zapisz zmiany. Wartość wpisu tajnego nie będzie dostępna po opuszczeniu strony.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Rysunek 3. Nowy wpis tajny klienta

4.  Dodaj "Interfejsu API Microsoft Graph" do aplikacji, otwierając "Wymagane uprawnienia".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Rysunek 4. Dodawanie nowego interfejsu API

Następujące uprawnienia powinna być dodana do aplikacji, aby zezwolić na używanie "Microsoft interfejsu API programu Graph", w zależności od scenariusza:

| Operacja z obiektem | Wymagane jest uprawnienie                                                                  | Typ uprawnienia |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importuj grupę          | `Group.Read.All` lub `Group.ReadWrite.All`                                                | Aplikacja     |
| Importuj użytkowników           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` lub `Directory.ReadWrite.All` | Aplikacja     |

Więcej informacji na temat wymaganych uprawnień można odnaleźć [tutaj](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Przyznać aplikacji wymaganych uprawnień.


<a name="installing-the-connector"></a>Instalowanie łącznika
========================

6.  Przed zainstalowaniem łącznika, upewnij się, że masz następujące elementy na serwerze synchronizacji: 

 - Program Microsoft .NET 4.5.2 Framework lub nowszy
 - Microsoft Identity Manager 2016 z dodatkiem SP1, muszą one korzystać ze poprawkę 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) lub nowszej.

7. Łącznik dla programu Microsoft Graph, oprócz innych łączników dla programu Microsoft Identity Manager 2016 z dodatkiem SP1 jest dostępny do pobrania z [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Uruchom ponownie usługi synchronizacji programu MIM.
 
<a name="connector-configuration"></a>Konfiguracja łącznika
=======================


9.  W interfejsie użytkownika Menedżera usługi synchronizacji wybierz **łączników** i **Utwórz**.
Wybierz **wykresu (Microsoft)** , Tworzenie łącznika i nadaj mu nazwę opisową.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. Usługi synchronizacji programu MIM interfejsu użytkownika określ identyfikator aplikacji, a następnie wygenerowany klucz tajny klienta. Każdego agenta zarządzania skonfigurowano w synchronizacji programu MIM mają swoje własne aplikacji w usłudze Azure AD, aby zapobiegać uruchamianiu jej importu równolegle w tej samej aplikacji.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Rysunek 5. Strona łączności

Strona łączności (obraz 5) zawiera wersję interfejsu API programu Graph, która jest używana, a nazwa dzierżawcy. Identyfikator klienta oraz klucz tajny klienta reprezentuje identyfikator aplikacji i wartość klucza aplikacji WebAPI utworzonego w usłudze Azure AD.

11. Wprowadź niezbędne zmiany na stronie parametrów globalnych:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Rysunek 6. Strona parametrów globalnych

Parametry globalne strona zawiera następujące ustawienia:

- Format daty/godziny — formatu, który jest używany dla dowolnego atrybutu z typem Edm.DateTimeOffset. Wszystkie daty są konwertowane na ciąg przy użyciu tego formatu podczas importowania. Ustawianie formatu jest stosowany dla każdego atrybutu, który zapisuje daty.

 - HTTP przekroczenia limitu czasu (w sekundach) — limit czasu w sekundach, które będą używane podczas każdego wywołania HTTP do aplikacji WebAPI.

 - Wymuś zmienić hasło dla poświadczeń utworzonego użytkownika następny znak — ta opcja jest używana dla nowego użytkownika, który zostanie utworzony podczas eksportowania. Jeśli opcja zostanie włączona, następnie [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) właściwość zostanie ustawiona na wartość true, w przeciwnym razie będzie wartość false.

<a name="configuring-the-connector-schema-and-operations"></a>Konfigurowanie łącznika schematu i operacje
=========================

12.   Skonfiguruj schematu.  Łącznik obsługuje następujące typy obiektów:

-   Użytkownik

    -   Importuj pełnej/różnicowej

    -   Eksportuj (Dodawanie, aktualizowanie i usuwanie)

-   Grupa

    -   Importuj pełnej/różnicowej

    -   Eksportuj (Dodawanie, aktualizowanie i usuwanie)


Lista typów atrybutów, które są obsługiwane:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (string, w obszarze łącznika)

-   `microsoft.graph.directoryObject` (odwołanie w przestrzeni łącznika do dowolnego z obsługiwanych obiektów)

-   `microsoft.graph.contact`

Atrybuty wielowartościowe (kolekcję) są również obsługiwane dla dowolnego typu z listy powyżej.

Łącznik używa "`id`" atrybut zakotwiczenia i nazwy domeny dla wszystkich obiektów.  W związku z tym zmiana nazwy nie jest potrzebna, ponieważ interfejs API programu Graph nie zezwala na obiekt zmienić jego atrybutu 'id'.


<a name="access-token-lifetime"></a>Okres istnienia tokenu dostępu
=====================

Aplikacja Graph wymaga tokenu dostępu do uzyskiwania dostępu do interfejsu API programu Graph. Łącznik będzie żądać nowy token dostępu dla każdej iteracji importu (iteracji importu zależy od rozmiaru strony). Przykład:

-   Usługa Azure AD zawiera obiekty 10000

-   Rozmiar strony, które skonfigurowano w łączniku jest 5000

W tym przypadku będzie istnieć dwóch iteracji podczas importowania, każde z nich zwróci 5000 obiektów do synchronizacji. Dlatego nowy token dostępu będzie żądanie dwa razy.

Podczas eksportowania nowy token dostępu będzie wymagane dla każdego obiektu, który musi być dodany, zaktualizowane lub usunięte.

<a name="troubleshooting"></a>Rozwiązywanie problemów
===============

**Włączanie dzienników**

Jeśli występują problemy w programie Graph, dzienniki można użyć do zlokalizowania problemu. Dlatego śledzenia może zostać włączona w [taki sam sposób, takich jak ogólne łączników](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Lub po prostu, dodając następujące polecenie, aby `miiserver.exe.config` (wewnątrz `system.diagnostics/sources` sekcji):


\<Nazwa źródła = switchValue "ConnectorsLog" = "Pełne"\>

\<odbiorniki\>

>   \<Dodaj initializeData = "ConnectorsLog" type="System.Diagnostics.EventLogTraceListener systemu, wersja = 4.0.0.0, kultura = neutral, PublicKeyToken = b77a5c561934e089" name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack,   Data i godzina, Timestamp, stos wywołań"/\>

\<Usuń nazwę = "Default" /\>

\</Listeners\>

\</ Source\>

>[!NOTE]
>Gdy "Uruchom ten agent zarządzania w oddzielnym procesie" jest włączona, następnie `dllhost.exe.config` powinny być używane zamiast `miiserver.exe.config`.

**Błąd wygasłego tokenu dostępu**

Łącznik może zwrócić HTTP 401 nieautoryzowane, komunikat o błędzie "token dostępu wygasł.":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Rysunek 7. "Token dostępu wygasł." Error

Przyczyny tego problemu może być konfiguracja okres istnienia tokenu dostępu po stronie platformy Azure. Domyślnie po 1 godzinie wygaśnięcia ważności tokenu dostępu. Aby zwiększyć czas wygaśnięcia, zobacz [w tym artykule](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Przykładem tego przy użyciu [usługi Azure AD PowerShell Module publicznej wersji zapoznawczej](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Nowe AzureADPolicy — definicja \@("{"TokenLifetimePolicy": {"Wersja": 1, **"AccessTokenLifetime":" 5: 00:00 "**}}") - DisplayName "OrganizationDefaultPolicyScenario" - IsOrganizationDefault \$wartość true - typu "TokenLifetimePolicy"

<a name="next-steps"></a>Kolejne kroki
----------
- [Graph Explorer doskonale nadaje się do rozwiązywania problemów HTTP wywołania problemów]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Przechowywanie wersji, obsługi i przełomowe zmiany zasad dla programu Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Pobierz łącznik programu Microsoft Identity Manager dla programu Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>Przewodniki specyficzne dla scenariusza
----------------------------------
[Wdrożenia programu MIM na typu End to End B2B]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
