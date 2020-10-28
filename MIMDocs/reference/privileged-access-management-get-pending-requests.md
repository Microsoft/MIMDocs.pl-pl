---
title: Pobierz oczekujące żądania PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0ec4388ce5d5f748d9f779819d0a2d0bdec06791
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760769"
---
# <a name="get-pending-pam-requests"></a>Pobierz oczekujące żądania PAM
Używane przez uprzywilejowane konto do zwrócenia listy oczekujących żądań, które wymagają zatwierdzenia.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
----------|--------------
$filter | Opcjonalny. Określ wszystkie oczekujące właściwości żądania PAM w wyrażeniu filtru, aby zwrócić przefiltrowaną listę odpowiedzi. Aby uzyskać więcej informacji na temat obsługiwanych operatorów, zobacz [filtrowanie w obszarze interfejsu API REST usługi PAM](privileged-access-management-rest-api-service-details.md#filtering).
v | Opcjonalny. Wersja interfejsu API. Jeśli ta wartość nie jest uwzględniona, używana jest bieżąca (ostatnio wydana) wersja interfejsu API. Aby uzyskać więcej informacji, zobacz [przechowywanie wersji w temacie Informacje o usłudze interfejsu API REST usługi PAM](privileged-access-management-rest-api-service-details.md#versioning).

### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądań i odpowiedzi HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST PAM* .

### <a name="request-body"></a>Treść żądania
Brak.

## <a name="response"></a>Reakcja
W tej sekcji opisano odpowiedź.

### <a name="response-codes"></a>Kody odpowiedzi

Kod  |Opis  
---------|---------
200 | OK
401 | Brak autoryzacji
403 | Forbidden
408 | Limit czasu żądania   
500 | Wewnętrzny błąd serwera
503 | Usługa jest niedostępna

### <a name="response-headers"></a>Nagłówki odpowiedzi
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądań i odpowiedzi HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST PAM* .

### <a name="response-body"></a>Treść odpowiedzi
Pomyślna odpowiedź zawiera listę obiektów zatwierdzenia żądania PAM o następujących właściwościach:

Właściwość | Opis
---------|-------------
RoleName | Nazwa wyświetlana roli, dla której jest wymagana zatwierdzenie.
Requestor | Nazwa użytkownika żądającego, który ma zostać zatwierdzony.
Uzasadnienie | Uzasadnienie podane przez użytkownika.
RequestedTTL | Żądany czas wygaśnięcia (w sekundach).
RequestedTime | Żądany czas podniesienia uprawnień.
CreationTime | Czas utworzenia żądania.
FIMRequestID | Zawiera pojedynczy element "value" z unikatowym identyfikatorem (GUID) żądania PAM.
RequestorID | Zawiera pojedynczy element "value" z unikatowym identyfikatorem (GUID) dla konta Active Directory, które spowodowało utworzenie żądania PAM.
ApprovalObjectID | Zawiera pojedynczy element "value" z unikatowym identyfikatorem (GUID) dla obiektu zatwierdzenia.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład pobierania oczekujących żądań PAM.

### <a name="example-request"></a>Przykład: żądanie

```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
