---
title: Zamknij żądanie PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: be7c2ff956f928a32de343fcf47b5398fdd303f0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760757"
---
# <a name="close-pam-request"></a>Zamknij żądanie PAM
Używany przez konto uprzywilejowane do zamykania żądania, które zainicjowano w celu podniesienia uprawnień do roli PAM.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
POST     |/API/pamresources/pamrequests ({Numer_id_żądania)/Close

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
----------|-----------
IdentyfikatorŻądania | Identyfikator (GUID) żądania PAM do zamknięcia, określony jako `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
----------|--------------
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
Brak.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład zamykania żądania.

### <a name="example-request"></a>Przykład: żądanie

```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK
```       
