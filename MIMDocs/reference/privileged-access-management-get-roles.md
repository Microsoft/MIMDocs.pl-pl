---
title: "Uzyskać ról PAM | Dokumentacja firmy Microsoft"
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>Uzyskać ról PAM
Używany przez uprzywilejowane konto, aby wyświetlić listę ról PAM, dla których to konto jest kandydatem.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `http://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/API/pamresources/pamroles

###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
----------|--------------
$filter | Opcjonalny. Określ właściwości roli PAM w wyrażeniu filtru, aby powrócić do listy filtrowane odpowiedzi. Aby uzyskać więcej informacji na temat obsługiwanych operatorów zobacz [filtrowanie w szczegółach usługi interfejsu API REST PAM](privileged-access-management-rest-api-service-details.md#filtering)
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
Odpowiedź oznaczająca Powodzenie zawiera kolekcję jednego lub większej liczby ról PAM, z których każdy ma następujące właściwości.

Właściwość | Opis
--------|-------------
RoleID | Unikatowy identyfikator (globalny GUID) roli PAM.
Nazwa wyświetlana | Nazwa wyświetlana roli PAM w usłudze MIM.
Opis | Opis roli PAM w usłudze MIM.
TTL | Rola dostępu praw wygaśnięcia maksymalny limit czasu w sekundach.
AvailableFrom | Najwcześniejsza pora dnia requst zostanie aktywowany.
AvailableTo | Najpóźniejsza godzina dnia, żądanie zostanie aktywowany.
MFAEnabled | Wartość logiczna wskazująca, czy żądania aktywacji dla tej roli wymagają żądanie uwierzytelniania MFA.
ApprovalEnabled | Wartość logiczna wskazująca, czy żądania aktywacji dla tej roli wymagają zatwierdzenia przez właściciela roli.
AvailibitlyWindowEnabled | Wartość logiczna wskazująca, czy rola może zostać uruchomiona tylko w określonym interwale.

##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
