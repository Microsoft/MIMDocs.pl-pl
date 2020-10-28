---
title: Tworzenie żądania PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d58155232b3f99f32df9cb6b4c2e344d0cbad278
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760768"
---
# <a name="create-pam-request"></a>Utwórz żądanie PAM
Używane przez uprzywilejowane konto do podniesienia poziomu roli PAM.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
POST     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
--------|-------------
Uzasadnienie | Opcjonalny. Przyczyna żądania podniesienia uprawnień dostarczona przez użytkownika.
RoleId | Wymagane. Unikatowy identyfikator (GUID) roli PAM do podniesienia uprawnień.
RequestedTTL | Wymagane. Żądany czas wygaśnięcia (w sekundach).
RequestedTime | Opcjonalny. Czas do podniesienia uprawnień.  
v | Opcjonalny. Wersja interfejsu API. Jeśli ta wartość nie jest uwzględniona, używana jest bieżąca (ostatnio wydana) wersja interfejsu API. Aby uzyskać więcej informacji, zobacz [przechowywanie wersji w temacie Informacje o usłudze interfejsu API REST usługi PAM](privileged-access-management-rest-api-service-details.md#versioning).

>[!NOTE]
>Można określić parametry **just** , **RoleId** , **RequestedTTL** i **RequestedTime** jako właściwości w treści żądania, a nie jako parametry zapytania. Parametr **v** może być określony tylko jako parametr zapytania.

### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądań i odpowiedzi HTTP](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST PAM* .

### <a name="request-body"></a>Treść żądania
Opcjonalny. Parametry **just** , **RoleId** , **RequestedTTL** i **RequestedTime** można określić jako właściwości treści żądania, zamiast określać je w ciągu zapytania adresu URL.

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
Pomyślna odpowiedź zawiera obiekt żądania PAM o następujących właściwościach:

Właściwość | Opis
--------|-------------
RequestID | Unikatowy identyfikator (GUID) żądania PAM.
CreatorID | Unikatowy identyfikator (GUID) w usłudze MIM dla konta, które utworzyło żądanie.
Uzasadnienie | Przyczyna podniesienia uprawnień.
CreationTime | Czas utworzenia żądania.
CreationMethod | Metoda użyta do utworzenia żądania.
ExpirationTime | Czas wygaśnięcia żądania.
RoleID| Unikatowy identyfikator (GUID) roli PAM.
RequestedTTL | Żądany limit czasu wygaśnięcia (w sekundach).
RequestedTime | Żądany czas podniesienia uprawnień.
Stanem żądania | Stan żądania. Możliwe wartości to "Processing", "aktywne", "zamknięte", "zamykające", "wygasłe", "PendingApproval", "PendingMFA" i "odrzucone".

## <a name="example"></a>Przykład
Ta sekcja zawiera przykłady tworzenia żądania PAM.

### <a name="example-request-1"></a>Przykład: żądanie 1

```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```

### <a name="example-response-1"></a>Przykład: odpowiedź 1

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

### <a name="example-request-2"></a>Przykład: żądanie 2

```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```

### <a name="example-response-2"></a>Przykład: odpowiedź 2

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
