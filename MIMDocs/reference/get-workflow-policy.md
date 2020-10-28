---
title: Pobierz zasady przepływu pracy | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1928bfd4dff75b1484a8a232e41c742842fa01b9
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760595"
---
# <a name="get-workflow-policy"></a>Pobierz zasady przepływu pracy
Pobiera zasady szablonu profilu dla określonego przepływu pracy. Dane są używane podczas tworzenia żądania. Zasady przepływu pracy określają, które dane są konieczne przez klienta w celu utworzenia żądania. Dane mogą obejmować różne elementy kolekcji danych, komentarze żądań i jednokrotne zasady haseł.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

### <a name="url-parameters"></a>Parametry URL

Parametr| Opis
--------|-------------
identyfikator| Wymagane. Identyfikator GUID odpowiadający szablonowi profilu, z którego mają zostać wyodrębnione zasady.
typ| Wymagane. Typ zasad, które są żądane. Możliwe wartości to "Zarejestruj", "Duplikuj", "OfflineUnblock", "OnlineUpdate", "Renew", "Recover" "RecoverOnBehalf", "" Przywracanie "," wycofywanie "," Odbierz "," TemporaryEnroll "i" Odblokuj ".

### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądanie HTTP i odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w obszarze *Szczegóły usługi interfejsu API REST* .

### <a name="request-body"></a>Treść żądania
Brak.

## <a name="response"></a>Reakcja
W tej sekcji opisano odpowiedź.

### <a name="response-codes"></a>Kody odpowiedzi

Kod  |Opis  
---------|---------
200 | OK
403 | Forbidden
204 | Brak zawartości
500 | Błąd wewnętrzny

### <a name="response-headers"></a>Nagłówki odpowiedzi
W przypadku typowych nagłówków odpowiedzi zobacz [nagłówki żądań i odpowiedzi HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST* .

### <a name="response-body"></a>Treść odpowiedzi
Po powodzeniu funkcja zwraca obiekt zasad, który jest oparty na obiekcie [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) . Co najmniej obiekt zasad zawiera właściwości w poniższej tabeli, ale może zawierać dodatkowe właściwości w zależności od żądanych zasad. Na przykład żądanie rejestracji zasad zwraca obiekt [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy.aspx) . Aby uzyskać więcej informacji, zobacz dokumentację obiektu zasad skojarzonego z parametrem {Type} w żądaniu. Dokumentację dla różnych typów obiektów zasad można znaleźć w dokumentacji dotyczącej [przestrzeni nazw Microsoft. CLM. Shared. ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx) .

Właściwość | Opis
---------|------------
ApprovalsNeeded | Liczba zatwierdzeń wymaganych przez żądania zarządzania certyfikatami programu Forefront Identity Manager (FIM) dla zasad.
AuthorizedApprover | Deskryptor zabezpieczeń dla użytkowników, którzy mają uprawnienia do zatwierdzania żądań programu FIM CM dla zasad.
AuthorizedEnrollmentAgent | Deskryptor zabezpieczeń dla użytkowników, którzy mogą pełnić rolę agentów rejestracji dla zasad.
AuthorizedInitiator | Deskryptor zabezpieczeń dla użytkowników, którzy mogą inicjować żądania programu FIM CM dla zasad.
CollectComments | Wartość logiczna wskazująca, czy dla tych zasad jest włączona kolekcja komentarzy dla żądań programu FIM CM.
CollectRequestPriority | Wartość logiczna wskazująca, czy dla tych zasad włączono zbieranie informacji o priorytecie żądania.
DefaultRequestPriority | Domyślny priorytet żądań programu FIM CM dla zasad.
Dokumenty | Dokumenty zasad, które są skonfigurowane dla zasad.
Enabled (Włączony) | Wartość logiczna wskazująca, czy zasady są włączone.
EnrollAgentRequired | Wartość logiczna wskazująca, czy agenci rejestracji są zobowiązani do żądań programu FIM CM dla zasad.
OneTimePasswordPolicy | Metoda dystrybucji dla haseł jednorazowych dla żądań programu FIM CM dla zasad.
Personalizacja | Opcje personalizacji karty inteligentnej dla zasad.
PolicyDataCollection | Elementy kolekcji danych, które są skojarzone z zasadami.
SelfServiceEnabled | Wartość logiczna wskazująca, czy dla zasad włączono samoobsługowe inicjowanie żądań programu FIM CM.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład pobrania zasad szablonu profilu dla przepływu pracy. 

### <a name="example-request-1"></a>Przykład: żądanie 1

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```

### <a name="example-response-1"></a>Przykład: odpowiedź 1

```
HTTP/1.1 200 OK

... body coming soon
```       

### <a name="example-request-2"></a>Przykład: żądanie 2

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```

### <a name="example-response-2"></a>Przykład: odpowiedź 2

```
HTTP/1.1 200 OK

... body coming soon
```       

## <a name="see-also"></a>Zobacz także

- [Microsoft. CLM. Shared. ProfileTemplates. ProfileTemplatePolicy — Klasa](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Przestrzeń nazw Microsoft. CLM. Shared. ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
