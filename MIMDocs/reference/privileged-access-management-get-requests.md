---
title: Pobierz żądania PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0eeaee3a6355229de94bcc390ad07da3e00b829f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760733"
---
# <a name="get-pam-requests"></a>Pobierz żądania PAM
Używany przez konto uprzywilejowane do zwracania historii poprzednio ogłoszonych żądań PAM.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
----------|--------------
$filter | Opcjonalny. Określ dowolne właściwości żądania PAM w wyrażeniu filtru, aby zwrócić przefiltrowaną listę odpowiedzi. Aby uzyskać więcej informacji na temat obsługiwanych operatorów, zobacz [filtrowanie w obszarze interfejsu API REST usługi PAM](privileged-access-management-rest-api-service-details.md#filtering).
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
Pomyślna odpowiedź zawiera listę obiektów żądania PAM o następujących właściwościach:

Właściwość | Opis
--------|-------------
RequestID | Unikatowy identyfikator (GUID) żądania PAM.
CreatorID | Unikatowy identyfikator (GUID) dla konta Active Directory, które spowodowało utworzenie żądania PAM.
Uzasadnienie | Przyczyna podniesienia uprawnień.
Nazwa wyświetlana | Nazwa wyświetlana żądania PAM w programie MIM.
CreationTime | Czas utworzenia żądania.
CreationMethod | Metoda użyta do utworzenia żądania.
ExpirationTime | Czas wygaśnięcia żądania.
RoleID| Unikatowy identyfikator (GUID) roli PAM.
RequestedTTL | Żądany limit czasu wygaśnięcia (w sekundach).
RequestedTime | Żądany czas podniesienia uprawnień.
RequestedStatus | Stan żądania. Możliwe wartości to "aktywne", "zamknięte", "zamykające", "wygasłe", "PendingApproval" i "odrzucone".

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład uzyskiwania opublikowanych żądań PAM.

### <a name="example-request"></a>Przykład: żądanie

```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
