---
title: Konfigurowanie łącznika programu Microsoft Identity Manager dla programu Microsoft Graph dla modelu B2B | Dokumentacja firmy Microsoft
author: billmath
description: Łącznik programu Microsoft Graph jest użytkownikowi zewnętrznemu zarządzania cyklem życia konta usługi AD. W tym scenariuszu organizacja ma zaproszenie gości do swojego katalogu usługi Azure AD i chce udzielić tych dostępu gości do uwierzytelniania Windows-Integrated lokalnego lub aplikacje oparte na protokołu Kerberos
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 139c58510117ad422529a4ff0facd23040023713
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358775"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Współpraca business-to-business (B2B) usługi Azure AD za pomocą programu Microsoft Identity Manager(MIM) 2016 SP1 za pomocą serwera Proxy aplikacji usługi Azure
============================================================================================================================

Początkowa scenariusz polega na użytkownika zewnętrznego zarządzania cyklem życia konta usługi AD.   W tym scenariuszu organizacja ma zaproszenie gości do swojego katalogu usługi Azure AD i chce udzielić tych dostępu gości do uwierzytelniania Windows-Integrated w środowisku lokalnym lub w przypadku aplikacji opartych na protokołu Kerberos za pośrednictwem [serwera proxy aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) lub innych mechanizmów bramy. Serwer proxy aplikacji usługi Azure AD wymaga każdy użytkownik musi mieć własne konto usług AD DS na potrzeby identyfikacji i delegowania.

## <a name="scenario-specific-guidance"></a>Wskazówki specyficzne dla scenariusza

Kilka założeniom w konfiguracji B2B przy użyciu programu MIM i serwera Proxy aplikacji usługi Azure AD:

-   Masz już wdrożone w środowisku lokalnym AD i programu Microsoft Identity Manager jest zainstalowany i podstawowa konfiguracja agenta usługi MIM i portalu programu MIM do zarządzania Active Directory (AD MA) i agenta zarządzania usługi FIM (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Zostały już wykonane instrukcjami w artykule na temat sposobu pobierania i instalowania [łącznika programu Graph](microsoft-identity-manager-2016-connector-graph.md).

-   Masz usługi Azure AD Connect, skonfigurowany pod kątem synchronizowania użytkowników i grup do usługi Azure AD.

-   Masz skonfigurowany do synchronizacji grupy usługi Office do kontrolowania aplikacji Azure AD Connect [do lokalnych usług AD DS](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Już zdefiniowano łączników serwera Proxy aplikacji usługi i grupy łączników, jeśli nie możesz odwiedzić stronę [tutaj](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) do zainstalowania i skonfigurowania

-   Opublikowano już co najmniej jednej aplikacji, które zależą od zintegrowane uwierzytelnianie Windows albo poszczególne konta usługi AD za pośrednictwem serwera Proxy aplikacji usługi Azure AD

-   Masz zaproszenie lub zapraszać gości co najmniej jeden, które doprowadziły do co najmniej jednego użytkownika, do których tworzona w usłudze Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Scenariusz wdrożenia typu End to End B2B

Ten przewodnik jest oparta na następujący scenariusz:

Stacjonarnym współpracuje z Trey Research Inc. jako część ich R & D działu. Trey Research pracownicy muszą dostęp do badań raportowania aplikacji udostępniane przez stacjonarnym.

-   Stacjonarnym znajdują się w ich własnych dzierżawy skonfigurowano domenę niestandardową.

-   Ktoś zaprosił użytkownika zewnętrznego w dzierżawie stacjonarnym.
    Ten użytkownik zaakceptował zaproszenia, a dostęp do zasobów, które są udostępniane.

-   Stacjonarnym opublikował aplikacji za pośrednictwem serwera Proxy aplikacji. W tym scenariuszu przykładowej aplikacji jest portalem programu MIM. Spowoduje to włączenie użytkownika-gościa do wzięcia udziału w procesach programu MIM, na przykład na stronach pomocy technicznej scenariuszy albo w celu uzyskania dostępu do grup w programie MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Konfigurowanie usługi AD i Azure AD Connect, aby wykluczyć użytkowników dodanych z usługi Azure AD

Domyślnie program Azure AD Connect założy, że użytkownicy inni niż administratorzy w usłudze Active Directory muszą zostać zsynchronizowane w usłudze Azure AD.  Jeśli program Azure AD Connect znajdzie istniejącego użytkownika w usłudze Azure AD, zgodną użytkownika z lokalnej usługi AD, program Azure AD Connect będzie dopasować dwa konta i przyjęto, że zostało to wcześniej synchronizacji, użytkownika i utworzyć lokalną AD autorytatywne.  Jednak to zachowanie domyślne nie jest odpowiednia dla przepływu B2B inicjującego konta użytkownika w usłudze Azure AD. 

W związku z tym użytkownikom, dostosowane do usług AD DS przez program MIM, z usługi Azure AD muszą znajdować się w taki sposób, że usługa Azure AD nie będzie podejmować próby synchronizacji tych użytkowników z powrotem do usługi Azure AD.
Jednym ze sposobów, w tym celu jest tworzenie nowej jednostki organizacyjnej w usługach AD DS i skonfigurować program Azure AD Connect, które mają zostać wykluczone z tej jednostki organizacyjnej.  

Więcej informacji można znaleźć w [synchronizacji programu Azure AD Connect: Konfigurowanie filtrowania](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Tworzenie aplikacji usługi Azure AD 


Uwaga: Przed utworzeniem w synchronizacji programu MIM agenta zarządzania dla łącznika programu graph, upewnij się, przejrzeniu przewodnik wdrażania [łącznika programu Graph](microsoft-identity-manager-2016-connector-graph.md)i utworzyć aplikację za pomocą klienta, identyfikator i klucz tajny.
Upewnij się, że aplikacja ma autoryzację do co najmniej jednego z tych uprawnień: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` lub `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Tworzenie nowego agenta zarządzania


W interfejsie użytkownika Menedżera usługi synchronizacji wybierz **łączników** i **Utwórz**.
Wybierz **wykresu (Microsoft)** i nadaj mu nazwę opisową.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Łączność

Na stronie Łączność należy określić wersję interfejsu API programu Graph. Jest gotowy PAI produkcji **V 1.0**, jest nieprodukcyjne **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Parametry globalne

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Konfigurowanie hierarchii zastrzegania

Ta strona służy do mapowania składnik nazwy domeny, na przykład jednostki Organizacyjnej, typ obiektu powinny zostać aprowizowane, na przykład jednostka organizacyjna. Nie jest to konieczne, w tym scenariuszu, więc pozostaw tę wartość domyślna, a następnie kliknij przycisk Dalej.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie

Na stronie partycje i hierarchie zaznacz wszystkie przestrzenie nazw z obiektami, które planujesz importowanie i eksportowanie.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Wybierz typy obiektów

Na stronie typami obiektów wybierz typy obiektów, które chcesz zaimportować. Należy wybrać co najmniej "User".

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Wybierz atrybuty

Na ekranie Wybierz atrybuty wybierz atrybuty z usługi Azure AD, które będą potrzebne do zarządzania użytkownikami B2B w AD. Atrybut "ID" jest wymagany.  Atrybuty `userPrincipalName` i `userType` będzie używany w dalszej części tej konfiguracji.  Inne atrybuty są opcjonalne, w tym

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Konfiguruj zakotwiczenia

Na ekranie Konfigurowanie zakotwiczenia skonfigurowanie atrybutu zakotwiczenia jest to krok wymagany. Domyślnie należy użyć atrybutu ID dla mapowania użytkownika.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Konfiguruj filtr łącznika

Na stronie Konfiguruj filtr łącznika programu MIM można odfiltrować obiekty w oparciu o filtr atrybutu. W tym scenariuszu dla modelu B2B celem jest tylko użytkownicy z wartością `userType` atrybutu, która jest równa `Guest`i nie użytkowników z wartością userType, która jest równa `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Konfiguruj reguły dołączania i projekcji

W tym przewodniku założono, że zostanie utworzona reguła synchronizacji.  Jak Konfigurowanie reguł dołączania i projekcji są obsługiwane przez reguły synchronizacji, nie jest potrzebna musi wskazywać dołączania i projekcji na łącznik sam. Pozostaw domyślną, a następnie kliknij przycisk ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Konfiguruj przepływ atrybutów

W tym przewodniku założono, że zostanie utworzona reguła synchronizacji.  Jak odbywa się przez regułę synchronizacji, który jest tworzony w dalszej części projekcji nie jest potrzebna do definiowania przepływu atrybutów w synchronizacji programu MIM. Pozostaw domyślną, a następnie kliknij przycisk ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Konfigurowanie anulowania aprowizacji

Ustawienie, aby skonfigurować anulowania aprowizacji umożliwia konfigurowanie synchronizacji programu MIM, można usunąć obiektu usuniętego obiektu metaverse. W tym scenariuszu firma Microsoft były disconnectors zgodnie z celem jest zapewnienie pozostawić je w usłudze Azure AD. W tym scenariuszu firma Microsoft nie eksportujesz niczego do usługi Azure AD i skonfigurowaniu łącznika do importu tylko.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Konfigurowanie rozszerzeń

Konfigurowanie rozszerzeń w tym zarządzania agenta jest opcją, ale nie wymagane, ponieważ firma Microsoft korzysta z reguły synchronizacji. Jeśli firma zdecydowała się wcześniej przy użyciu zaawansowaną regułę przepływu atrybutu, będzie opcję, aby zdefiniować rozszerzenie reguły.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Rozszerzanie schematu metaverse


Przed utworzeniem reguły synchronizacji, należy utworzyć atrybutu o nazwie powiązany obiekt osoby za pomocą narzędzia Projektant MV userPrincipalName.

W kliencie synchronizacji należy wybrać Projektant magazynu Metaverse

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Następnie wybierz typ obiektu osoby

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Następnie w obszarze Akcje kliknij Dodawanie atrybutu

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Następnie wykonaj poniższe szczegóły

Nazwa atrybutu: **userPrincipalName**

Typ atrybutu: **ciąg (można indeksować)**

Indeksowane = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Tworzenie reguły synchronizacji usługi programu MIM

w poniższych krokach Zaczniemy mapowanie konta gościa B2B i przepływ atrybutów. Niektóre założenia zostały wprowadzone w tym miejscu: czy masz już usługi Active Directory MA skonfigurowane i FIM MA skonfigurowany tak, aby przenieść użytkowników do usługi i portalu MIM.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Następne kroki będą wymagać dodanie minimalnej konfiguracji FIM MA i agenta zarządzania usługą AD.

Więcej szczegółów można znaleźć tutaj dla konfiguracji <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> — jak Aprowizować użytkowników do usług AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Reguła synchronizacji: Importowanie użytkownika-gościa do MV do obiektu Metaverse usługi synchronizacji z usługą Azure Active Directory<br>

Przejdź do portalu programu MIM, wybierz reguły synchronizacji, a następnie kliknij pozycję Nowy.  Utwórz regułę synchronizacji ruchu przychodzącego przepływu B2B za pośrednictwem łącznika programu graph.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

Na etapie kryteriów relacji koniecznie wybierz pozycję "Utwórz zasób w programie FIM".
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Skonfiguruj następujące reguły przepływu atrybutów dla ruchu przychodzącego.  Pamiętaj wypełnić `accountName`, `userPrincipalName` i `uid` atrybutów, ponieważ zostaną użyte w dalszej części tego scenariusza:

| **Tylko początkowy przepływ** | **Użyj jako testu istnienia** | **Przepływ (atrybut FIM ⇒ wartość źródła)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Lewej (identyfikator, 20) ⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Reguła synchronizacji: Tworzenie konta użytkownika-gościa do usługi Active Directory 

Ta reguła synchronizacji tworzy użytkownika w usłudze Active Directory.  Pamiętaj, aby przepływ dla `dn` należy umieścić użytkownika w jednostce organizacyjnej, w której został wykluczony z usługi Azure AD Connect.  Ponadto Aktualizuj przepływ dla `unicodePwd` na zgodne z zasadami haseł usługi AD — użytkownik nie musi znać hasło.  Zanotuj wartość ustawienia `262656` dla `userAccountControl` koduje flagi `SMARTCARD_REQUIRED` i `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Reguły przepływu:

| **Tylko początkowy przepływ** | **Użyj jako testu istnienia** | **Przepływ (atrybut docelowy ⇒ wartości programu FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", OU = B2BGuest, DC = contoso, DC = com" ⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Opcjonalne reguły synchronizacji: Import B2B gościa obiektów identyfikator SID użytkownika umożliwiające logowanie do programu MIM 

Ta reguła synchronizacji ruchu przychodzącego wnosi atrybut identyfikatora SID użytkownika z usługi Active Directory powrót do programu MIM, dzięki czemu użytkownik może uzyskać dostęp do portalu programu MIM.  Portal programu MIM wymaga aby użytkownik miał atrybuty `samAccountName`, `domain` i `objectSid` wypełnione w bazie danych usługi MIM.

Konfigurowanie systemu zewnętrznego źródła jako `ADMA`, jako `objectSid` atrybutu zostanie ustawiona automatycznie przez usługi AD, gdy program MIM tworzy użytkownika.
 
Pamiętaj, że jeśli konfigurujesz użytkowników do utworzenia w usłudze MIM, upewnij się, ich nie są w zakresie wszystkie zestawy, przeznaczony dla reguły zasad zarządzania samoobsługowego resetowania HASEŁ dla pracowników.  Może być konieczna zmiana definicji zestawu do wykluczenia osób, które zostały utworzone przez usługę flow B2B. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Tylko początkowy przepływ** | **Użyj jako testu istnienia** | **Przepływ (atrybut FIM ⇒ wartość źródła)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO" ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Reguły synchronizacji

Następnie możemy zaprosić użytkownika, a następnie uruchom zarządzania agenta reguły synchronizacji w następującej kolejności:

-   Pełny Import i synchronizacja na `MIMMA` agenta zarządzania.  Dzięki temu usługa synchronizacji programu MIM ma najnowsze reguły synchronizacji skonfigurowane.

-   Pełny Import i synchronizacja na `ADMA` agenta zarządzania.  Dzięki temu program MIM i usługi Active Directory są spójne.  W tym momencie została jeszcze będzie wszelkie oczekujące operacje eksportowania dla gości.

-   Pełny Import i synchronizacja na agencie zarządzania wykres B2B.  Dzięki temu masz dostęp użytkowników-gości do obiektu metaverse.  W tym momencie co najmniej jedno konto będzie oczekujących operacji eksportu `ADMA`.  Jeśli nie istnieją żadne oczekujące operacje eksportowania, sprawdź, czy użytkownicy-goście zostały zaimportowane do obszaru łącznika i że reguły zostały skonfigurowane dla nich otrzyma kont usługi AD.

-   Eksport, Import zmian i synchronizacji na `ADMA` agenta zarządzania.  Jeśli polecenie eksportuje nie powiodła się, sprawdź konfigurację reguły i określić, czy wystąpiły wszelkie brakujące wymagania schematu. 

-   Eksport, Import zmian i synchronizacji na `MIMMA` agenta zarządzania.  Po zakończeniu tego procesu, powinien już istnieć wszelkie oczekujące operacje eksportowania.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Opcjonalne: Logując się do portalu MIM goście serwera Proxy aplikacji dla modelu B2B

Teraz, gdy utworzono reguły synchronizacji w programie MIM. W konfiguracji serwera Proxy aplikacji, należy zdefiniować użyć jednostki chmury, aby umożliwić dla ograniczonego delegowania protokołu Kerberos na serwerze proxy aplikacji.
Ponadto następnie dodany ręcznie do Zarządzaj użytkownikami i grupami. Opcje, aby nie wyświetlać użytkownika do momentu utworzenia wystąpił w programie MIM, aby dodać gościa do grupy usługi office po aprowizacji wymaga nieco większej liczby czynności konfiguracyjnych nieuwzględnione w tym dokumencie.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Gdy wszystkie skonfigurowane, mają nazwy logowania użytkownika B2B, a aplikacja.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Następne kroki
----------

[Jak aprowizować użytkowników do usług AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Dokumentacja funkcji programu FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Jak zapewnić bezpieczny dostęp zdalny do aplikacji lokalnych](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Pobierz łącznik programu Microsoft Identity Manager dla programu Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
