---
title: "Utwórz żądanie PAM | Dokumentacja firmy Microsoft"
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>Utwórz żądanie PAM
Używane przez uprzywilejowane konto do podniesienia uprawnień do roli PAM.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `http://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
POST     |/API/pamresources/pamrequests

###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
--------|-------------
Justowanie | Opcjonalny. Dostarczone przez użytkownika przyczynę żądania podniesienia uprawnień.
RoleId| Wymagany. Unikatowy identyfikator (globalny GUID) do podniesienia uprawnień do roli PAM.
RequestedTTL| Wymagany. Wygaśnięcie żądany czas w sekundach.
RequestedTime | Optoinal. Czas do podniesienia uprawnień.  
v | Opcjonalny. Wersja interfejsu API. Jeśli nie zostanie uwzględniony, będzie używany bieżący (najbardziej ostatnio) wersji interfejsu API. Aby uzyskać więcej informacji, zobacz [przechowywania wersji szczegóły usługi interfejsu API REST PAM](privileged-access-management-rest-api-service-details.md#versioning)

**Uwaga**: można określić *uzasadnienie*, *RoleId*, *RequestedTTL*, i *RequestedTime* parametrów jako właściwości w treści żądania, a nie jako parametry zapytania. *v* parameteer można określić tylko jako parametr zapytania.

###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST PAM*.
###<a name="request-body"></a>Treść żądania
Opcjonalny. Jak wspomniano powyżej, *uzasadnienie*, *RoleId*, *RequestedTTL*, i *RequestedTime* parametrów można określić jako ciągu zapytania właściwości treści żądania, zamiast określania je w adresie URL.

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
Odpowiedź oznaczająca Powodzenie zawiera obiekt żądania PAM z następującymi właściwościami.

Właściwość | Opis
--------|-------------
Identyfikator żądania | Unikatowy identyfikator (globalny GUID) dla żądania PAM.
CreatorID | Unikatowy identyfikator (globalny GUID) w usłudze MIM dla konta, który utworzył żądanie.
Justowanie | Powód podniesienia uprawnień.
CreationTime | Czas utworzenia żądania.
CreationMethod | Metoda użyta do utworzenia żądania.
ExpirationTime | Czas wygaśnięcia żądania.
RoleID| Unikatowy identyfikator (globalny GUID) roli PAM.
RequestedTTL | Żądany limit czasu wygaśnięcia w sekundach.
RequestedTime | Żądana godzina podniesienia uprawnień.
Stan żądania | Stan żądania. Możliwe wartości to: "Przetwarzanie", "Active", "Zamknięte", "Zamknięcie", "Wygasł", "Oczekuje na zatwierdzenie", "PendingMFA" i "Odrzucone".

##<a name="example"></a>Przykład

###<a name="request-1"></a>Żądanie 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Odpowiedź 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>Żądanie 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Odpowiedź 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
