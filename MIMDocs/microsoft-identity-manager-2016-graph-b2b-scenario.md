---
title: Agent zarządzania programu Microsoft Identity Manager dla programu Microsoft Graph | Dokumenty Microsoft
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
ms.openlocfilehash: ac11a4dfb23944d50dbbcf0b0d70c915f186c159
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/25/2018
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Azure AD współpracy między firmami (B2B) z dodatkiem SP1 2016 Manager(MIM) tożsamość firmy Microsoft przy użyciu serwera Proxy aplikacji Azure (publicznej wersji zapoznawczej)
============================================================================================================================

Początkowa scenariusz w wersji zapoznawczej dla jest zarządzanie cyklem życia AD konta użytkownika zewnętrznego.   W tym scenariuszu organizacja zaprosiła gości do swojego katalogu usługi Azure AD i chce udostępnić te gości lokalnymi zintegrowanego uwierzytelniania systemu Windows lub aplikacji opartych na protokołu Kerberos, za pomocą [aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)serwera proxy lub innych mechanizmów bramy. Serwer proxy aplikacji usługi Azure AD wymaga od każdego użytkownika ma swoje własne konta usług AD DS, dla celów identyfikacji i delegowanie

## <a name="scenario-specific-supported-guidance"></a>Dokładne wskazówki obsługiwany scenariusz

W tym scenariuszu organizacja zaprosiła gości do swojego katalogu usługi Azure AD i chce zapewniają te gości dostęp do lokalnego systemu Windows. Zintegrowane uwierzytelnianie lub opartego na protokole Kerberos aplikacji za pośrednictwem [aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) serwera proxy lub innych mechanizmów bramy. Serwer proxy aplikacji usługi Azure AD wymaga od każdego użytkownika ma swoje własne konta usług AD DS, dla celów identyfikacji i delegowanie

Kilka założeniom w konfiguracji B2B MIM i serwer Proxy aplikacji Azure

-   Użytkownik zainstalował już [agenta zarządzania wykresu](microsoft-identity-manager-2016-connector-graph.md).

-   Masz lokalnej usługi AD i Azure AD połączyć z zestawu do synchronizacji użytkowników i grup do usługi Azure AD.

    -   Grupy usługi Office kontrolowanie aplikacji uzyskiwać dostęp za pomocą [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Już skonfigurowano serwer Proxy aplikacji łączników i grup łącznika, jeśli nie możesz odwiedzić [tutaj](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) do instalowania i konfigurowania

-   Opublikowane jeden lub więcej aplikacji, które zależą od konta indywidualne AD za pomocą serwera Proxy aplikacji usługi Azure AD lub zintegrowane uwierzytelnianie systemu Windows

-   Musisz mieć zaproszenie lub zaprosić gości co najmniej jednego, które są tworzone w usłudze Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>

-   Program Microsoft Identity Manager jest zainstalowany i podstawowych konfiguracji usługi i portalu oraz Agent zarządzania usługi Active Directory.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>B2B pełnego wdrożenia

Scenariusz

Firmy Contoso Pharmaceuticals działa Trey Research Inc. jako część ich R & D działu. Trey Research pracownicy muszą research raportowanie aplikacji udostępniane przez firmy Contoso Pharmaceuticals dostępu.

-   Firmy Contoso Pharmaceuticals są niezależne dzierżawy, aby skonfigurowano domeny niestandardowej.

-   Zaproszenie użytkownika zewnętrznego: do firmy Contoso Pharmaceuticals dzierżawy.
    Ten użytkownik zaakceptował zaproszenia i dostęp do zasobów, które są udostępniane.

-   Opublikowane aplikacji za pośrednictwem serwera Proxy aplikacji, a w tym scenariuszu, na przykład na potrzeby uczestniczą w procesie MIM, scenariusze pomocy technicznej przykład usługi MIM i portalu dla użytkownika gościa.

## <a name="create-the-graph-management-agent"></a>Tworzenie agenta zarządzania programu Graph

Uwaga: Przed utworzeniem łącznika upewnij się, należy przejrzeć [agenta zarządzania wykresu](microsoft-identity-manager-2016-connector-graph.md).

W Interfejsie użytkownika Menedżera usługi synchronizacji, wybierz **łączniki** i **Utwórz**.
Wybierz **Graph (Microsoft)** i nadaj mu nazwę opisową

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Łączność

Na stronie Łączność należy określić jest gotowy produkcyjnej wersji interfejsu API Graph **V 1.0**, jest inne niż produkcji **w wersji Beta**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Możliwości

Na stronie parametry globalne można skonfigurować DN dziennik zmian różnicowych i dodatkowe funkcje LDAP. Strona jest wstępnie wypełniony z informacjami pochodzącymi z serwera LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Parametry globalne

Na stronie parametry globalne można skonfigurować DN dziennik zmian różnicowych i dodatkowe funkcje LDAP. Strona jest wstępnie wypełniony z informacjami pochodzącymi z serwera LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Konfigurowanie hierarchii zastrzegania

Ta strona służy do mapowania składnik nazwy domeny, na przykład jednostki Organizacyjnej, typ obiektu powinna być przygotowana, na przykład jednostka organizacyjna. Pozostaw ten domyślny, kliknij Dalej

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfiguruj partycje i hierarchie

Na stronie partycje i hierarchie zaznacz wszystkie przestrzenie nazw z obiektami, które chcesz importować i eksportować.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Wybierz typy obiektów

Na stronie partycje i hierarchie zaznacz wszystkie przestrzenie nazw z obiektami, które chcesz importować i eksportować.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Wybierz atrybuty

Na ekranie Wybierz atrybuty wybierz atrybuty wymagane do zarządzania użytkownikami B2B. Wymagany jest atrybut "id"

-   Identyfikator

-   displayName

-   Poczta

-   givenName

-   nazwisko

-   userPrincipalName

-   UserType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Konfiguruj zakotwiczenia

Zakotwiczenia skonfigurować Konfiguruj zakotwiczenia jest wymagane kroku. Domyślnie używa atrybutu id mapowania.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Konfiguruj filtr łącznika

W konfiguracji strony filtr łącznika umożliwia filtrowanie oparte na filtr atrybutów obiektów. W tym scenariuszu B2B celem jest tylko przenieść użytkowników z userType, równą gościa i nie jest to element członkowski.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Konfiguruj reguły dołączania i projekcji

Konfigurowanie dołączania i projekcji reguły są obsługiwane przez regułę synchronizacji, nie jest potrzebna, musi wskazywać dołączania i projekcji na łącznik sam. Pozostaw domyślne i kliknij przycisk ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Konfiguruj przepływ atrybutów

Jak dołączania i projekcji nie jest wymagana do definiowania przepływu atrybutu, ponieważ jest on dojścia przez regułę synchronizacji, który można utworzyć później. Pozostaw domyślne i kliknij przycisk ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Skonfiguruj Deprovision

Skonfiguruj deprovision umożliwiają usunięcie obiektu usuniętego obiektu metaverse. W tym teście wykonujemy ich disconnectors zgodnie z celem jest pozostaw je na platformie Azure także możemy są nie eksportowanie niczego na platformie azure jako jest tylko importowanie.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Konfigurowanie rozszerzeń

Konfigurowanie rozszerzeń w tym zarządzania agenta jest opcją, ale nie są wymagane jako używamy reguły synchronizacji. Jeśli zdecydowaliśmy się do wcześniejszego reguły przepływu atrybutu wcześniej powinien to być opcję, aby określić.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Tworzenie reguły synchronizacji usługi MIM

w poniższych krokach Zaczniemy mapowania konta gościa B2B i przepływ atrybutów. Niektóre założenia zostały wprowadzone w tym miejscu masz już Active Directory Management Agent skonfigurowany, a agenta zarządzania usługą FIM skonfigurowany do komunikowania się z usługą MIM i portalu.

Przed utworzeniem reguły synchronizacji, należy utworzyć atrybutu o nazwie powiązane z obiektem osoby przy użyciu narzędzia Projektant MV userPrincipalName.

W kliencie synchronizacji wybierz Metaverse Designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Następnie wybierz typ obiektu osoby

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Następnie w obszarze Akcje kliknij przycisk Dodaj atrybut

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Na koniec wszystkie następujące szczegóły

Nazwa atrybutu: **userPrincipalName**

Typ atrybutu: **ciąg (Index)**

Indeksowane = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Następne kroki wymaga minimalnego konfiguracja agenta zarządzania usługi FIM i domeny usługi zarządzania Agent usługi Active Directory.

Więcej informacji można znaleźć tutaj konfiguracji <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> — jak udostępnić użytkownikom usług AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Reguła synchronizacji: Importowanie użytkownika gościa do MV do środowiska Metaverse usługi synchronizacji z usługą Azure Active Directory<br>

Przejdź do usługi MIM i portalu i wybierz zasady Sycronization i kliknij przycisk Nowy.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Wybierz opcję "Utwórz zasób w programie FIM" ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Reguły przepływu:

| **Tylko początkowego przepływu** | **Używany jako Test istnienia** | **Przepływ (atrybut docelowy ⇒ wartość FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName] (javascript:void(0);)                        |
|                       |                           | [Lewej (identyfikator, 20) ⇒accountName] (javascript:void(0);)                        |
|                       |                           | [id⇒uid] (javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType] (javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [surname⇒sn] (javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
|                       |                           | [id⇒cn] (javascript:void(0);)                                          |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone] (javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Reguła synchronizacji: Tworzenie konta użytkownika gościa do usługi Active Directory 

Ta reguła synchronizacji tworzy użytkownika w usłudze active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Reguły przepływu:

| **Tylko początkowego przepływu** | **Używany jako Test istnienia** | **Przepływ (atrybut docelowy ⇒ wartość FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName] (javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [sn⇒sn] (javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", OU = B2BGuest, DC = scontoso, DC = com" ⇒dn] (javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd] (javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl] (javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Reguła synchronizacji: Import B2B gościa użytkownika obiekty SID umożliwiające logowanie do programu MIM 

Ta reguła synchronizacji tworzy użytkownika w usłudze active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Ponadto firma Microsoft zaprosić użytkownika, a następnie uruchom Zarządzanie w następującej kolejności:

-   Pełny Import i synchronizacji na agencie zarządzania MIMMA

-   Pełny Import i synchronizacji ADMA_SCONTOSO_B2B agenta zarządzania

-   Pełny Import i synchronizacji na agencie zarządzania wykres B2B

-   Export, Import zmian i synchronizacji ADMA_SCONTOSO_B2B agenta zarządzania

-   Export, Import zmian i synchronizacji na agencie zarządzania MIMMA

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Na koniec: Serwer Proxy aplikacji z gościa B2B i rejestrowanie do MIM

Teraz, gdy utworzono zasady synchronizacji w programie MIM. W konfiguracji serwera Proxy aplikacji, należy zdefiniować użycie chmury umożliwiają KCD serwera proxy aplikacji.
Ponadto obok dodany ręcznie do Zarządzaj użytkownikami i grupami. Opcje, aby nie wyświetlać użytkownika do czasu podczas tworzenia w programie MIM, aby dodać gościa do grupy office raz elastycznie wymaga nieco większej konfiguracji nie zostały omówione w tym dokumencie.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Tylko początkowego przepływu** | **Używany jako Test istnienia** | **Przepływ (atrybut docelowy ⇒ wartość FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName] (javascript:void(0);)                     |
|                       |                           | ["CONTOSO" ⇒domain] (javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid] (javascript:void(0);)                                      |

Po skonfigurowania wszystkich

Na koniec B2B użytkownika logowania i mogą być wyświetlane aplikacji

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Następne kroki
----------

[Jak aprowizować użytkowników do usług AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Dokumentacja funkcji programu FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Jak zapewnić bezpieczny zdalny dostęp do aplikacji lokalnych](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Pobierz agenta zarządzania programu Microsoft Identity Manager dla programu Microsoft Graph (wersja zapoznawcza)](http://go.microsoft.com/fwlink/?LinkId=717495)
