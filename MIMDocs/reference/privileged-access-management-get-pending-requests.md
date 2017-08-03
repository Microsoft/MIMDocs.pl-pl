---
title: "Pobrać żądań oczekujących PAM | Dokumentacja firmy Microsoft"
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>Uzyskać żądań PAM oczekujące
Używany przez uprzywilejowane konto, aby powrócić do listy oczekujących żądań, które wymagają zatwierdzenia.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `http://api.contoso.com`.
##<a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/API/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
----------|--------------
$filter | Opcjonalny. Określ właściwości żądania PAM Perding w wyrażeniu filtru, aby powrócić do listy filtrowane odpowiedzi. Aby uzyskać więcej informacji na temat obsługiwanych operatorów zobacz [filtrowanie w szczegółach usługi interfejsu API REST PAM](privileged-access-management-rest-api-service-details.md#filtering)
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
Odpowiedź oznaczająca Powodzenie zawiera listę obiektów zatwierdzenie żądania PAM z następującymi właściwościami.

Właściwość | Opis
---------|-------------
RoleName | Wyświetlana nazwa roli, dla którego zatwierdzenia jest wymagana.
Obiekt żądający | Nazwa użytkownika żądającego zatwierdzenia.
Justowanie | Uzasadnienie podanego przez użytkownika.
RequestedTTL | Wygaśnięcie żądany czas w sekundach.
RequestedTime | Żądana godzina podniesienia uprawnień.
CreationTime | Czas utworzenia żądania.
FIMRequestID | Zawiera pojedynczy element "Value" z Unikatowy identyfikator (globalny GUID) żądania PAM.
RequestorID | Zawiera pojedynczy element "Wartość" Unikatowy identyfikator (globalny GUID) dla konta usługi Active Directory, który utworzył żądanie PAM.
ApprovalObjectID | Zawiera pojedynczy element "Value" z Unikatowy identyfikator (globalny GUID) dla obiekt zatwierdzenia.

##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Odpowiedź
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
