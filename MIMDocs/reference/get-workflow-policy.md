---
title: "Pobierz zasady przepływu pracy | Dokumentacja firmy Microsoft"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6bc88fee88bdfeec3ce4ead60bc17fe2ea169402
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-workflow-policy"></a>Pobierz zasady przepływu pracy
Pobiera zasady szablonu profilu dla określonego przepływu pracy. Te dane są używane podczas tworzenia żądania. Zasady przepływu pracy określa dane, które są wymagane przez klienta w celu utworzenia żądania. Dane te mogą obejmować: różne elementy kolekcji danych, komentarze żądania i jedne zasady haseł czasu.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

###<a name="url-parameters"></a>Parametry adresu URL
Parametr| Opis
--------|-------------
Identyfikator| Wymagany. Identyfikator GUID odpowiadający szablonu profilu, który ma zostać wyodrębniony z zasad.
typ| Wymagany. Typ zasad jest wymagane. Możliwe wartości to: *Zarejestruj*, *zduplikowane*, *OfflineUnblock*, *OnlineUpdate*, *odnawiania*, *odzyskać*, *RecoverOnBehalf*, *przywrócenia*, *Wycofaj*, *odwołać*, *TemporaryEnroll*, *Odblokuj*.

###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="request-body"></a>Treść żądania
brak

##<a name="response"></a>Odpowiedź
###<a name="response-codes"></a>Kody odpowiedzi
Kod  |Opis  
---------|---------
200     | OK
403 | Dostęp zabroniony
204 | Brak zawartości
500 | Błąd wewnętrzny

###<a name="response-headers"></a>Nagłówki odpowiedzi
Dla wspólnych nagłówków odpowiedzi, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="response-body"></a>Treść odpowiedzi
W przypadku powodzenia zwraca obiekt zasad na podstawie [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) obiektu. Co najmniej obiektu zasad będzie zawierać właściwości w poniższej tabeli, ale może zawierać dodatkowe właściwości, w zależności od żądane zasady. Na przykład żądania zasad Zarejestruj zwróci [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy.aspx) obiektu. Aby uzyskać więcej informacji zobacz dokumentację dla obiektu zasady skojarzone z parametrem {type} w żądaniu. Dokumentacja dla różnych typów obiektów zasad można znaleźć w [Namespace Microsoft.Clm.Shared.ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx) dokumentacji.

Właściwość | Opis
---------|------------
ApprovalsNeeded | Liczba zatwierdzeń, które są wymagane dla żądań FIM CM zasad.
AuthorizedApprover | Deskryptor zabezpieczeń dla użytkowników, którzy mogą zatwierdzać żądania programu FIM CM zasad.
AuthorizedEnrollmentAgent | Deskryptor zabezpieczeń dla użytkowników, którzy mogą pełnić rolę agentów rejestracji zasad.
AuthorizedInitiator | Deskryptor zabezpieczeń dla użytkowników, którzy mogą inicjować żądań FIM CM zasad.
CollectComments | Wartość logiczna wskazująca, czy kolekcja komentarz jest włączony dla żądań FIM CM zasad.
CollectRequestPriority | Wartość logiczna wskazująca, czy kolekcja priorytet żądania jest włączony dla żądań FIM CM zasad.
DefaultRequestPriority | Domyślny priorytet żądań FIM CM zasad.
Dokumenty | Dokumenty zasad, które są skonfigurowane zasady.
Enabled | Wartość logiczna wskazująca, czy jest włączona.
EnrollAgentRequired | Wartość logiczna wskazująca, czy agenci rejestracji są wymagane dla żądań FIM CM zasad.
OneTimePasswordPolicy | Pobiera jak jednorazowe hasła dla żądań FIM CM dla zasad są dystrybuowane.
Personalizacja | Opcje personalizacji kart inteligentnych dla zasad.
PolicyDataCollection | Elementy kolekcji danych, które są skojarzone z zasadami.
SelfServiceEnabled | Wartość logiczna wskazująca, czy samoobsługi inicjowania żądań FIM CM jest włączony dla zasad.

##<a name="example"></a>Przykład

###<a name="request-1"></a>Żądanie 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Odpowiedź 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Żądanie 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Odpowiedź 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Zobacz też

- [Klasa Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Namespace Microsoft.Clm.Shared.ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
