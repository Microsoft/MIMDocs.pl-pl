---
title: Łącznik Microsoft Identity Manager dla Microsoft Graph | Microsoft Docs
author: fimguy
description: Łącznik Microsoft Identity Manager dla Microsoft Graph włącza Zarządzanie cyklem życia konta usługi AD użytkownika zewnętrznego. W tym scenariuszu Organizacja odprosiła Gości do swojego katalogu usługi Azure AD i chce udzielić tych Gościom dostępu do lokalnego uwierzytelniania zintegrowanego systemu Windows lub aplikacji opartych na protokole Kerberos.
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2e376bcc88518b911f93ce9cd4ab920eb428815b
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64519371"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Łącznik Microsoft Identity Manager dla Microsoft Graph
=======================================================================================

<a name="summary"></a>Podsumowanie 
=======

[Łącznik Microsoft Identity Manager dla Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) umożliwia wykonywanie dodatkowych scenariuszy integracji dla klientów Azure AD — wersja Premium.  Powierzchnie IT w ramach dodatkowych obiektów usługi synchronizacji programu MIM uzyskanych z [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) V1 i beta.

<a name="scenarios-covered"></a>Omówione scenariusze
=================

<a name="b2b-account-lifecycle-management"></a>Zarządzanie cyklem życia konta B2B
--------------------------------

Początkowy scenariusz Microsoft Identity Manager łącznika Microsoft Graph jest jako łącznik ułatwiający automatyzację zarządzania cyklem życia konta AD DS dla użytkowników zewnętrznych. W tym scenariuszu Organizacja synchronizuje pracowników do usługi Azure AD z AD DS przy użyciu Azure AD Connect, a także zaprasza Gości do katalogu usługi Azure AD. Zapraszanie gościa powoduje, że zewnętrzny obiekt użytkownika znajduje się w katalogu usługi Azure AD organizacji, który nie znajduje się w AD DS tej organizacji. Następnie organizacja chce udzielić tym Gościom dostępu do lokalnego uwierzytelniania zintegrowanego systemu Windows lub aplikacji opartych na protokole Kerberos za pośrednictwem [serwera proxy aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) lub innych mechanizmów bramy. Serwer proxy aplikacji usługi Azure AD wymaga, aby każdy użytkownik miał własne konto AD DS na potrzeby identyfikacji i delegowania.  

Aby dowiedzieć się, jak skonfigurować program MIM Sync do automatycznego tworzenia i obsługiwania kont AD DS dla Gości, po przeczytaniu instrukcji z tego artykułu, przejdź do artykułu [Azure AD Business-to-Business (B2B) Współpraca z programem MIM 2016 z dodatkiem SP1 przy użyciu serwera proxy aplikacji platformy Azure](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  W tym artykule przedstawiono reguły synchronizacji, które są używane przez łącznik.

<a name="other-identity-management-scenarios"></a>Inne scenariusze zarządzania tożsamościami
---------------

Łącznik może służyć do innych scenariuszy związanych z zarządzaniem tożsamościami, w tym do tworzenia, odczytywania, aktualizowania i usuwania obiektów użytkowników, grup i kontaktów w usłudze Azure AD poza synchronizacją użytkowników i grup w usłudze Azure AD. Gdy oceniasz potencjalne scenariusze, weź pod uwagę: ten łącznik nie może być obsługiwany w scenariuszu, co spowodowałoby nakładanie się przepływu danych, rzeczywiste lub potencjalne skutki synchronizacji ze wdrożeniem Azure AD Connect.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) jest zalecanym podejściem do integrowania katalogów lokalnych z usługą Azure AD przez synchronizowanie użytkowników i grup z katalogów lokalnych z usługą Azure AD.  Azure AD Connect ma wiele funkcji synchronizacji i umożliwia wykonywanie takich scenariuszy jak hasło i zapisywanie zwrotne urządzeń, co nie jest możliwe w przypadku obiektów utworzonych przez program MIM. Jeśli dane są wprowadzane do AD DS, na przykład upewnij się, że są wyłączone z Azure AD Connect próba dopasowania tych obiektów z powrotem do katalogu usługi Azure AD.  Nie można używać tego łącznika do wprowadzania zmian w obiektach usługi Azure AD, które zostały utworzone przez Azure AD Connect.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Przygotowywanie do używania łącznika do Microsoft Graph
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autoryzowanie łącznika do pobierania i zarządzania obiektami w katalogu usługi Azure AD
----------------------------------------------------

1.  Łącznik wymaga utworzenia aplikacji sieci Web/interfejsu API w usłudze Azure AD, dzięki czemu może być autoryzowany z odpowiednimi uprawnieniami do działania w obiektach usługi Azure AD za pomocą Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Rysunek 1. Rejestrowanie nowej aplikacji

2.  W Azure Portal Otwórz utworzoną aplikację i Zapisz identyfikator aplikacji jako identyfikator klienta, który ma być używany później na stronie o łączności:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Rysunek 2. Identyfikator aplikacji

3.  Wygeneruj nowy klucz tajny klienta, otwierając wszystkie ustawienia — klucze\>. Ustaw Opis klucza i wybierz potrzebny czas trwania. Zapisz zmiany. Wartość wpisu tajnego nie będzie dostępna po opuszczeniu strony.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Rysunek 3. Nowy klucz tajny klienta

4.  Dodaj "Microsoft Graph API" do aplikacji, otwierając "wymagane uprawnienia".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Rysunek 4. Dodawanie nowego interfejsu API

Do aplikacji należy dodać następujące uprawnienia, aby umożliwić korzystanie z "Microsoft Graph API" w zależności od scenariusza:

| Operacja z obiektem | Wymagane uprawnienie                                                                  | Typ uprawnienia |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importuj grupę          | `Group.Read.All` lub `Group.ReadWrite.All`                                                | Aplikacja     |
| Importuj użytkownika           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` lub `Directory.ReadWrite.All` | Aplikacja     |

Więcej informacji o wymaganych uprawnieniach można znaleźć [tutaj](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Przyznaj aplikacji wymagane uprawnienia.


<a name="installing-the-connector"></a>Instalowanie łącznika
========================

6.  Przed zainstalowaniem łącznika upewnij się, że na serwerze synchronizacji znajdują się następujące elementy: 

 - Microsoft .NET 4.5.2 Framework lub nowszy
 - Microsoft Identity Manager 2016 z dodatkiem SP1 i musi używać poprawki 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) lub nowszej.

7. Łącznik dla Microsoft Graph, oprócz innych łączników dla Microsoft Identity Manager 2016 z dodatkiem SP1, jest dostępny jako pobranie z [Centrum pobierania Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Uruchom ponownie usługę synchronizacji programu MIM.
 
<a name="connector-configuration"></a>Konfiguracja łącznika
=======================


9.  W interfejsie użytkownika Synchronization Service Manager wybierz pozycję **łączniki** i **Utwórz**.
Wybierz pozycję **Graph (Microsoft)**  , Utwórz łącznik i nadaj mu nazwę opisową.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. W interfejsie użytkownika usługi synchronizacji programu MIM Określ identyfikator aplikacji i wygenerowany klucz tajny klienta. Każdy agent zarządzania skonfigurowany w ramach synchronizacji programu MIM powinien mieć własną aplikację w usłudze Azure AD, aby uniknąć jednoczesnego uruchamiania importowania dla tej samej aplikacji.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Obraz 5. Strona łączność

Strona łączność (obraz 5) zawiera używaną wersję interfejs API programu Graph i nazwę dzierżawy. Identyfikator klienta i klucz tajny klienta reprezentują identyfikator aplikacji i wartość klucza aplikacji WebAPI, które muszą zostać utworzone w usłudze Azure AD.

11. Wprowadź wszelkie niezbędne zmiany na stronie parametrów globalnych:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Ilustracja 6. Strona parametrów globalnych

Strona parametrów globalnych zawiera następujące ustawienia:

- Format daty i godziny — format używany dla dowolnego atrybutu z typem EDM. DateTimeOffset. Wszystkie daty są konwertowane na ciąg przy użyciu tego formatu podczas importowania. Format zestawu jest stosowany dla dowolnego atrybutu, który zapisuje datę.

 - Limit czasu protokołu HTTP (w sekundach) — limit czasu (w sekundach), który będzie używany podczas każdego wywołania protokołu HTTP do aplikacji WebAPI.

 - Wymuś zmianę hasła dla utworzonego użytkownika przy następnym podpisaniu — ta opcja jest używana dla nowego użytkownika, który zostanie utworzony podczas eksportowania. Jeśli opcja jest włączona, właściwość [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) zostanie ustawiona na wartość true, w przeciwnym razie ma wartość false.

<a name="configuring-the-connector-schema-and-operations"></a>Konfigurowanie schematu i operacji łącznika
=========================

12.   Skonfiguruj schemat.  Łącznik obsługuje następującą listę typów obiektów:

-   Użytkownik

    -   Import pełny/Delta

    -   Eksportuj (Dodawanie, aktualizowanie, usuwanie)

-   Grupa

    -   Import pełny/Delta

    -   Eksportuj (Dodawanie, aktualizowanie, usuwanie)


Lista typów atrybutów, które są obsługiwane:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (ciąg w przestrzeni łącznika)

-   `microsoft.graph.directoryObject` (odwołanie w obszarze łącznika do dowolnego z obsługiwanych obiektów)

-   `microsoft.graph.contact`

Atrybuty wielowartościowe (kolekcja) są również obsługiwane dla dowolnego typu z powyższej listy.

Łącznik używa atrybutu "`id`" dla zakotwiczenia i nazwy wyróżniającej dla wszystkich obiektów.  W związku z tym zmiana nazwy nie jest konieczna, ponieważ interfejs API programu Graph nie zezwala obiektowi na zmianę atrybutu "ID".


<a name="access-token-lifetime"></a>Okres istnienia tokenu dostępu
=====================

Aplikacja grafu wymaga tokenu dostępu do uzyskiwania dostępu do interfejs API programu Graph. Łącznik zażąda nowego tokenu dostępu dla każdej iteracji importu (zależą od rozmiaru strony). Przykład:

-   Usługa Azure AD zawiera 10000 obiektów

-   Rozmiar strony skonfigurowany w łączniku to 5000

W takim przypadku podczas importowania będą dostępne dwie iteracje, każda z nich zwróci 5000 obiektów do synchronizacji. W związku z tym nowy token dostępu zostanie pożądany dwa razy.

Podczas eksportowania zostanie zażądany nowy token dostępu dla każdego obiektu, który musi zostać dodany/zaktualizowany/usunięty.

<a name="troubleshooting"></a>Rozwiązywanie problemów
===============

**Włączanie dzienników**

W przypadku problemów z programem Graph dzienniki mogą być używane do lokalizowania problemu. Dlatego ślady mogą być włączone w taki [sam sposób jak w przypadku łączników generycznych](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Lub po prostu dodając następujące elementy do `miiserver.exe.config` (wewnątrz sekcji `system.diagnostics/sources`):


\<Source Name = "ConnectorsLog" switchValue = "verbose"\>

\<detektory\>

>   \<Add initializeData = "ConnectorsLog" Type = "System. Diagnostics. EventLogTraceListener, system, Version = 4.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089" name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack, DateTime, timestamp, Stack Call"/\>

\<usunąć Name = "default"/\>

\</Listeners\>

\</Source\>

>[!NOTE]
>Jeśli włączono opcję "Uruchom ten agent zarządzania w osobnym procesie", zamiast `miiserver.exe.config`należy użyć `dllhost.exe.config`.

**Błąd wygaśnięcia tokenu dostępu**

Łącznik może zwrócić błąd HTTP 401 bez autoryzacji, komunikat "token dostępu wygasł.":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Rysunek 7. "Token dostępu wygasł." Error

Przyczyną tego problemu może być konfiguracja okresu istnienia tokenu dostępu po stronie platformy Azure. Domyślnie token dostępu wygasa po 1 godzinie. Aby zwiększyć czas wygaśnięcia, zobacz [ten artykuł](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Przykład korzystania z [publicznej wersji zapoznawczej modułu programu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy-Definition \@("{" TokenLifetimePolicy ": {" Version ": 1, **" AccessTokenLifetime ":" 5:00:00 "** }}")-DisplayName "OrganizationDefaultPolicyScenario"-IsOrganizationDefault \$True-Type "TokenLifetimePolicy"

<a name="next-steps"></a>Następne kroki
----------
- [Eksplorator grafów, doskonały do rozwiązywania problemów z wywołaniem HTTP]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Zasady przechowywania wersji, pomocy technicznej i zmiany dotyczącej podziału dla Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Pobierz Łącznik Microsoft Identity Manager dla Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>Przewodniki dotyczące scenariusza
----------------------------------
[Kompleksowe wdrożenie programu MIM B2B]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
