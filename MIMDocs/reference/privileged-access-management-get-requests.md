---
title: "Uzyskać żądań PAM | Dokumentacja firmy Microsoft"
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
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00467b6443d3e6e0511ee1817a945ffe569b99b0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-requests"></a>Uzyskać żądań PAM
Używany przez konta uprawnionego do zwrócenia historię poprzednio zaksięgowany PAM żądania.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `http://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/API/pamresources/pamrequests

###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
----------|--------------
$filter | Opcjonalny. Określ właściwości żądania PAM w wyrażeniu filtru, aby powrócić do listy filtrowane odpowiedzi. Aby uzyskać więcej informacji na temat obsługiwanych operatorów zobacz [filtrowanie w szczegółach usługi interfejsu API REST PAM](privileged-access-management-rest-api-service-details.md#filtering)
v | Opcjonalny. Wersja interfejsu API. Jeśli nie zostanie uwzględniony, będzie używany bieżący (najbardziej ostatnio) wersji interfejsu API. Aby uzyskać więcej informacji, zobacz [przechowywania wersji szczegóły usługi interfejsu API REST PAM](privileged-access-management-rest-api-service-details.md#versioning)

###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST PAM*.
###<a name="request-body"></a>Treść żądania
brak

##<a name="response"></a>Odpowiedź
###<a name="response-codes"></a>Kody odpowiedzi
Kod  |Opis  
---------|---------
200 | OK
401 | Nieupoważniony
403 | Dostęp zabroniony
408 | Limit czasu żądania   
500 | Wewnętrzny błąd serwera
503 | Usługa jest niedostępna

###<a name="response-headers"></a>Nagłówki odpowiedzi
Dla wspólnych nagłówków odpowiedzi, zobacz [żądania HTTP i nagłówków odpowiedzi](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST PAM*.
###<a name="response-body"></a>Treść odpowiedzi
Odpowiedź oznaczająca Powodzenie zawiera listę obiektów żądania PAM z następującymi właściwościami.

Właściwość | Opis
--------|-------------
Identyfikator żądania | Unikatowy identyfikator (globalny GUID) dla żądania PAM.
CreatorID | Unikatowy identyfikator (globalny GUID) dla konta usługi Active Directory, który utworzył żądanie PAM.
Justowanie | Powód podniesienia uprawnień.
Nazwa wyświetlana | Nazwa wyświetlana żądania PAM w programie MIM.
CreationTime | Czas utworzenia żądania.
CreationMethod | Metoda użyta do utworzenia żądania.
ExpirationTime | Czas wygaśnięcia żądania.
RoleID| Unikatowy identyfikator (globalny GUID) roli PAM.
RequestedTTL | Żądany limit czasu wygaśnięcia w sekundach.
RequestedTime | Żądana godzina podniesienia uprawnień.
RequestedStatus | Stan żądania. Możliwe wartości to: "Active", "Zamknięte", "Zamknięcie", "Wygasł", "Oczekuje na zatwierdzenie" i "Odrzucone".

##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

###<a name="response"></a>Odpowiedź
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
