---
title: Uzyskiwanie informacji o sesji PAM | Dokumentacja firmy Microsoft
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
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 150bc7e863fc679e785526935155611fb75cd69b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-session-info"></a>Uzyskiwanie informacji o sesji PAM
Używany przez konta uprzywilejowanego, można uzyskać nazwy użytkownika konta, na którym jest zalogowany do sesji.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `http://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/API/Session/sessioninfo

###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
----------|--------------
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
Odpowiedź oznaczająca Powodzenie zwraca PAM obiektu sesji z następującymi właściwościami.

Właściwość| Opis
--------|-------------
Nazwa użytkownika| Nazwa użytkownika konta, na którym jest zalogowany w tej sesji.

##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
