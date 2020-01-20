---
title: Konfigurowanie łącznika Microsoft Identity Manager dla Microsoft Graph dla usługi B2B | Microsoft Docs
author: billmath
description: Łącznik Microsoft Graph jest zarządzaniem cyklem życia konta usługi AD użytkownika zewnętrznego. W tym scenariuszu Organizacja odprosiła Gości do swojego katalogu usługi Azure AD i chce udzielić tych Gościom dostępu do lokalnego uwierzytelniania zintegrowanego systemu Windows lub aplikacji opartych na protokole Kerberos.
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: ba70cd299f2ebec31555bb40b935a6b54779d198
ms.sourcegitcommit: 1ca298d61f6020623f1936f86346b47ec5105d44
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2020
ms.locfileid: "76256635"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Współpraca między firmami (B2B) w usłudze Azure AD z Microsoft Identity Manager (MIM) 2016 z dodatkiem SP1 przy użyciu serwera proxy aplikacji platformy Azure
============================================================================================================================

Scenariusz początkowy to zarządzanie cyklem życia konta usługi AD użytkownika zewnętrznego.   W tym scenariuszu Organizacja odprosiła Gości do swojego katalogu usługi Azure AD i chce udzielić tych Gościom dostępu do lokalnego uwierzytelniania zintegrowanego systemu Windows lub aplikacji opartych na protokole Kerberos, za pośrednictwem [serwera proxy aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) lub innych mechanizmów bramy. Serwer proxy aplikacji usługi Azure AD wymaga, aby każdy użytkownik miał własne konto AD DS na potrzeby identyfikacji i delegowania.

## <a name="scenario-specific-guidance"></a>Wskazówki dotyczące scenariusza

Kilka założeń w konfiguracji B2B przy użyciu programu MIM i usługi Azure serwer proxy aplikacji usługi Azure AD:

-   Lokalna usługa AD została już wdrożona, a Microsoft Identity Manager jest zainstalowana i Podstawowa konfiguracja usługi MIM, portalu programu MIM, agenta zarządzania Active Directory (AD MA) i agenta zarządzania programu FIM (FIM).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Wykonano już instrukcje zawarte w artykule na temat pobierania i instalowania [łącznika grafu](microsoft-identity-manager-2016-connector-graph.md).

-   Azure AD Connect skonfigurowany do synchronizowania użytkowników i grup w usłudze Azure AD.

-   Już skonfigurowano łączniki serwera proxy aplikacji i grupy łączników, jeśli nie możesz odwiedzić [tutaj](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) , aby zainstalować i skonfigurować

-   Opublikowano już co najmniej jedną aplikację, która polega na zintegrowanym uwierzytelnianiu systemu Windows lub poszczególnych kontach usługi AD za pośrednictwem aplikacja usługi Azure AD proxy

-   Zaprosiłeś lub zapraszasz co najmniej jednego gościa, co spowodowało utworzenie co najmniej jednego użytkownika w usłudze Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Przykładowy scenariusz między kompleksowym wdrożeniem B2B

Ten przewodnik kompiluje się w następującym scenariuszu:

Środki farmaceutyczne contoso współdziałają z Trey Research Inc. jako część ich działu R & D. Pracownicy naukowo-badawczy Trey muszą uzyskać dostęp do aplikacji do raportowania badań dostarczonych przez środki farmaceutyczne firmy Contoso.

-   Środki farmaceutyczne firmy Contoso znajdują się w własnej dzierżawie, aby można było skonfigurować domenę niestandardową.

-   Ktoś zaprosił użytkownika zewnętrznego do dzierżawy środków farmaceutycznych firmy Contoso.
    Ten Użytkownik zaakceptował zaproszenie i może uzyskać dostęp do udostępnionych zasobów.

-   Środki farmaceutyczne firmy Contoso opublikowały aplikację za pośrednictwem serwera proxy aplikacji. W tym scenariuszu Przykładowa aplikacja jest portalem programu MIM. Pozwoli to użytkownikom-Gościom na uczestnictwo w procesach programu MIM, na przykład w scenariuszach pomocy technicznej lub zażądać dostępu do grup w programie MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Konfigurowanie usług AD i Azure AD Connect, aby wykluczyć użytkowników dodanych z usługi Azure AD

Domyślnie Azure AD Connect założono, że użytkownicy niebędący administratorami w Active Directory muszą zostać zsynchronizowani z usługą Azure AD.  Jeśli Azure AD Connect odnajdzie istniejącego użytkownika w usłudze Azure AD, który pasuje do użytkownika z lokalnej usługi AD, Azure AD Connect będzie pasować do tych dwóch kont i założono, że jest to wcześniejsza Synchronizacja użytkownika i że lokalna usługa Active Directory jest autorytatywna.  Jednak to zachowanie domyślne nie jest odpowiednie dla przepływu B2B, gdzie konto użytkownika pochodzi z usługi Azure AD. 

W związku z tym użytkownicy przeniesieni do AD DS przez program MIM z usługi Azure AD muszą być przechowywani w taki sposób, aby usługa Azure AD nie próbowała synchronizować tych użytkowników z powrotem z usługą Azure AD.
Jednym ze sposobów jest utworzenie nowej jednostki organizacyjnej w AD DS i skonfigurowanie Azure AD Connect do wykluczenia tej jednostki organizacyjnej.  

Więcej informacji można znaleźć w temacie [Azure AD Connect Sync: Configure Filter](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Tworzenie aplikacji usługi Azure AD 


Uwaga: przed utworzeniem w programie MIM zsynchronizuj agenta zarządzania dla łącznika grafu, upewnij się, że został on sprawdzony w celu wdrożenia [łącznika grafu](microsoft-identity-manager-2016-connector-graph.md)i utworzenia aplikacji z identyfikatorem klienta i wpisem tajnym.
Upewnij się, że aplikacja została autoryzowana dla co najmniej jednego z następujących uprawnień: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` lub `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Utwórz nowego agenta zarządzania


W interfejsie użytkownika Synchronization Service Manager wybierz pozycję **łączniki** i **Utwórz**.
Wybierz pozycję **Graph (Microsoft)**  i nadaj jej nazwę opisową.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Łączność

Na stronie łączność należy określić wersję interfejs API programu Graph. PAI produkcyjny to **V 1,0**, non-produkcyjny to **wersja beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Parametry globalne

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Skonfiguruj hierarchię aprowizacji

Ta strona służy do mapowania składnika nazwy wyróżniającej, na przykład jednostki organizacyjnej, na typ obiektu, który powinien być zainicjowany, na przykład organizationalUnit. Nie jest to wymaganie w tym scenariuszu, więc pozostaw to ustawienie domyślne i kliknij przycisk Dalej.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie

Na stronie partycje i hierarchie zaznacz wszystkie przestrzenie nazw z obiektami, które mają zostać zaimportowane i wyeksportowane.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Wybierz typy obiektów

Na stronie typy obiektów wybierz typy obiektów, które planujesz zaimportować. Musisz wybrać co najmniej "User".

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Wybierz atrybuty

Na ekranie Wybieranie atrybutów wybierz atrybuty z usługi Azure AD, które będą konieczne do zarządzania użytkownikami B2B w usłudze AD. Atrybut "ID" jest wymagany.  Atrybuty `userPrincipalName` i `userType` zostaną użyte w dalszej części tej konfiguracji.  Inne atrybuty są opcjonalne, włącznie z

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Konfiguruj zakotwiczenia

Na ekranie konfigurowania zakotwiczenia skonfigurowanie atrybutu zakotwiczenia jest wymaganym krokiem. Domyślnie Użyj atrybutu ID dla mapowania użytkownika.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Konfiguruj filtr łącznika

Na stronie Konfigurowanie filtru łącznika program MIM umożliwia odfiltrowanie obiektów na podstawie filtru atrybutów. W tym scenariuszu dla B2B celem jest dołączenie tylko do użytkowników z wartością atrybutu `userType`, który jest równy `Guest`, a nie dla użytkowników, które są równe `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Konfiguruj reguły dołączania i projekcji

W tym przewodniku przyjęto założenie, że zostanie utworzona reguła synchronizacji.  Ponieważ reguły przyłączania i projekcji są obsługiwane przez regułę synchronizacji, nie jest to konieczne, aby identyfikować sprzężenie i projekcję na samym łączniku. Pozostaw wartość domyślną i kliknij przycisk OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Konfiguruj przepływ atrybutów

W tym przewodniku przyjęto założenie, że zostanie utworzona reguła synchronizacji.  Projekcja nie jest wymagana do zdefiniowania przepływu atrybutów w synchronizacji programu MIM, ponieważ jest ona obsługiwana przez regułę synchronizacji utworzoną później. Pozostaw wartość domyślną i kliknij przycisk OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Konfigurowanie anulowania aprowizacji

Ustawienie anulowania aprowizacji umożliwia skonfigurowanie funkcji synchronizacji programu MIM do usuwania obiektów, jeśli obiekt Metaverse został usunięty. W tym scenariuszu firma Microsoft poprowadzi własne połączenia w celu opuszczenia ich w usłudze Azure AD. W tym scenariuszu nie eksportumy niczego do usługi Azure AD, a łącznik jest skonfigurowany tylko do importowania.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Konfigurowanie rozszerzeń

Konfigurowanie rozszerzeń na tym agencie zarządzania jest opcją, ale nie jest to wymagane, ponieważ jest używana reguła synchronizacji. Jeśli postanowiono użyć zaawansowanej reguły w przepływie atrybutów wcześniej, będzie można zdefiniować rozszerzenie reguł.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Rozszerzanie schematu Metaverse


Przed utworzeniem reguły synchronizacji musimy utworzyć atrybut o nazwie userPrincipalName powiązany z obiektem Person przy użyciu narzędzia MV Designer.

W kliencie synchronizacji wybierz pozycję Projektant Metaverse.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Następnie wybierz typ obiektu osoba

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Następnie w obszarze Akcje kliknij pozycję Dodaj atrybut.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Następnie wykonaj następujące szczegóły

Nazwa atrybutu: **userPrincipalName**

Typ atrybutu: **ciąg (z indeksem)**

Indeksowane = **prawda**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Tworzenie reguł synchronizacji usługi MIM

w poniższych krokach rozpocznie się mapowanie konta gościa B2B i przepływu atrybutów. Niektóre założenia zostały wykonane w tym miejscu: masz już skonfigurowaną Active Directory, a usługa FIM MA skonfigurowany do przenoszenia użytkowników do usługi i portalu programu MIM.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

W następnych krokach będzie wymagane dodanie minimalnej konfiguracji do usługi FIM i ma.

Więcej szczegółów można znaleźć tutaj dla konfiguracji <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> — jak zapewnić użytkownikom możliwość AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Reguła synchronizacji: zaimportuj użytkownika-gościa do funkcji MV usługi synchronizacji z Azure Active Directory<br>

Przejdź do portalu MIM, wybierz pozycję reguły synchronizacji, a następnie kliknij przycisk Nowy.  Utwórz regułę synchronizacji ruchu przychodzącego dla przepływu B2B za pośrednictwem łącznika Graf.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

W kroku kryteria relacji wybierz opcję "Utwórz zasób w programie FIM".
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Skonfiguruj następujące przychodzące reguły przepływu atrybutów.  Pamiętaj, aby wypełnić `accountName`, `userPrincipalName` i `uid` atrybutów, ponieważ zostaną one użyte w dalszej części tego scenariusza:

| **Tylko przepływ początkowy** | **Użyj jako testu istnienia** | **Flow (wartość źródłowa ⇒ atrybut FIM)**                          |
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

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Reguła synchronizacji: Utwórz konto użytkownika-gościa do Active Directory 

Ta reguła synchronizacji służy do tworzenia użytkownika w Active Directory.  Upewnij się, że przepływ dla `dn` musi umieścić użytkownika w jednostce organizacyjnej, która została wykluczona z Azure AD Connect.  Ponadto zaktualizuj przepływ pod kątem `unicodePwd` w celu spełnienia zasad haseł usługi AD — użytkownik nie musi znać hasła.  Zwróć uwagę na wartość `262656` dla `userAccountControl` koduje flagi `SMARTCARD_REQUIRED` i `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Reguły przepływu:

| **Tylko przepływ początkowy** | **Użyj jako testu istnienia** | **Flow (atrybut docelowy ⇒ wartości programu FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", OU = B2BGuest, DC = contoso, DC = com" ⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Opcjonalna reguła synchronizacji: Importowanie identyfikatora SID obiektów użytkownika gościa B2B w celu zezwolenia na logowanie do programu MIM 

Ta reguła synchronizacji ruchu przychodzącego umożliwia uzyskanie atrybutu SID użytkownika z Active Directory z powrotem do programu MIM, dzięki czemu użytkownik może uzyskać dostęp do portalu MIM.  Portal programu MIM wymaga, aby użytkownik miał atrybuty `samAccountName`, `domain` i `objectSid` wypełnione w bazie danych usługi MIM.

Skonfiguruj źródłowy system zewnętrzny jako `ADMA`, ponieważ atrybut `objectSid` zostanie automatycznie ustawiony przez usługi AD podczas tworzenia użytkownika przez program MIM.
 
Należy pamiętać, że w przypadku skonfigurowania użytkowników do utworzenia w usłudze MIM upewnij się, że nie są one w zakresie żadnych zestawów przeznaczonych dla reguł zasad zarządzania SSPRą pracowników.  Może zajść potrzeba zmiany definicji zestawu, aby wykluczyć użytkowników, którzy zostali utworzeni przez przepływ B2B. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Tylko przepływ początkowy** | **Użyj jako testu istnienia** | **Flow (wartość źródłowa ⇒ atrybut FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO" ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Uruchamianie reguł synchronizacji

Następnie Zapraszamy użytkownika, a następnie uruchamiasz reguły synchronizacji agenta zarządzania w następującej kolejności:

-   Pełny import i synchronizacja na `MIMMA` agenta zarządzania.  Dzięki temu synchronizacja programu MIM ma skonfigurowane najnowsze reguły synchronizacji.

-   Pełny import i synchronizacja na `ADMA` agenta zarządzania.  Zapewnia to spójność programu MIM i Active Directory.  W tym momencie nie będą jeszcze oczekujące eksporty dla Gości.

-   Pełny import i synchronizacja na agencie zarządzania grafem B2B.  Spowoduje to napełnienie użytkownikom-Gościom funkcji Metaverse.  W tym momencie co najmniej jedno konto będzie oczekuje na eksport dla `ADMA`.  Jeśli nie ma oczekujących eksportów, sprawdź, czy użytkownicy-Goście zostali zaimportowani do obszaru łącznika i czy reguły zostały skonfigurowane pod kątem kont usługi AD.

-   Eksportowanie, importowanie różnicowe i synchronizacja na `ADMA` agenta zarządzania.  Jeśli eksporty nie powiodły się, sprawdź konfigurację reguły i ustal, czy istnieją jakieś wymagania schematu. 

-   Eksportowanie, importowanie różnicowe i synchronizacja na `MIMMA` agenta zarządzania.  Po zakończeniu tego procesu nie powinno już istnieć żadne oczekujące eksporty.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Opcjonalnie: serwer proxy aplikacji dla Gości B2B logowanie do portalu MIM

Teraz, gdy utworzyliśmy reguły synchronizacji w programie MIM. W konfiguracji serwera proxy aplikacji Zdefiniuj użycie usługi Cloud, aby zezwolić na KCD na serwerze proxy aplikacji.
Dodatkowo dodano użytkownika ręcznie do pozycji Zarządzaj użytkownikami i grupami. Opcje, które nie pokazują użytkownika, dopóki nie nastąpi utworzenie w programie MIM, aby dodać gościa do grupy Office po zainicjowaniu obsługi administracyjnej wymaga nieco więcej konfiguracji, które nie zostały omówione w tym dokumencie.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Po skonfigurowaniu programu należy zalogować użytkownika B2B i zobaczyć aplikację.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Następne kroki
----------

[Jak aprowizować użytkowników do usług AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Dokumentacja funkcji programu FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Jak zapewnić bezpieczny, zdalny dostęp do aplikacji lokalnych](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Pobierz Łącznik Microsoft Identity Manager dla Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495)
