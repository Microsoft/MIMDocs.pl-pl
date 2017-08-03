---
title: "Zatwierdź lub Odrzuć oczekujące żądania PAM | Dokumentacja firmy Microsoft"
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
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e5506f96a22d1918cff7d0c9b822babb0f6cfa9
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Zatwierdź lub Odrzuć oczekujące żądania PAM
Używany przez konta uprawnionego do zatwierdzenia, zamknij lub odrzucić żądanie do podniesienia uprawnień do roli PAM.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `http://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
POST     |/API/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/API/pamresources/pamrequeststoapprove({approvalId)/Reject

###<a name="url-parameters"></a>Parametry adresu URL
Parametr | Opis
----------|-----------
approvalId | Następujący identyfikator (GUID) obiektu zatwierdzenia w PAM`guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
----------|--------------
v | Opcjonalny. Wersja interfejsu API. Jeśli nie zostanie uwzględniony, będzie używany bieżący (najbardziej ostatnio) wersji interfejsu API. Aby uzyskać więcej informacji, zobacz [przechowywania wersji szczegóły usługi interfejsu API REST PAM](privileged-access-management-rest-api-service-details.md#versioning)


###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST PAM*.
###<a name="request-body"></a>Treść żądania
Brak.

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
brak
##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1


```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

```       
