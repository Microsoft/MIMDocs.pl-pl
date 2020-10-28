---
title: Pobierz szablony profilów | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9f14853b3895dece1098270d35439d6c4999be2b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760426"
---
# <a name="get-profile-templates"></a>Pobierz szablony profilów
Pobiera listę szablonów profilu, dla których określony użytkownik może zarejestrować. Ta metoda zwraca ograniczony widok szablonu profilu. Zwrócone dane szablonu profilu powinny być wystarczające, aby umożliwić użytkownikowi żądającemu decydowanie o tym, który szablon profilu (jeśli istnieje) musi zarejestrować się w programie. Jeśli nie określono przepływu pracy i uprawnienia, zwracane są wszystkie szablony profilów widoczne dla użytkownika.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates? \[ targetuser\] 

### <a name="url-parameters"></a>Parametry URL

Parametr| Opis
--------|-------------
targetuser| Opcjonalny. Określa użytkownika docelowego, dla którego mają zostać zwrócone szablony profilów. Jeśli nie zostanie określony, zostanie użyta tożsamość bieżącego użytkownika. <br/><br/>**Uwaga** : obecnie obsługiwany jest tylko bieżący użytkownik.

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
204 | Brak zawartości
500 | Błąd wewnętrzny

### <a name="response-headers"></a>Nagłówki odpowiedzi
W przypadku typowych nagłówków odpowiedzi zobacz [nagłówki żądań i odpowiedzi HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST* .

### <a name="response-body"></a>Treść odpowiedzi
Po sukcesie zwraca listę obiektów ProfileTemplateLimitedView o następujących właściwościach:

Właściwość| Typ| Opis
--------|-----|--------
Nazwa| string| Nazwa wyświetlana szablonu profilu.
Opis| ciąg| Opis szablonu profilu.
Identyfikator UUID| Guid (identyfikator GUID)| Identyfikator szablonu profilu.
IsSmartcardProfileTemplate| bool| Wskazuje, czy szablon jest szablonem profilu karty inteligentnej.
IsVirtualSmartcardProfileTemplate| bool| Wskazuje, czy szablon profilu wymaga wirtualnej karty inteligentnej.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład pobrania listy szablonów profilów dla określonego użytkownika.

### <a name="example-request"></a>Przykład: żądanie

```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]
```       
