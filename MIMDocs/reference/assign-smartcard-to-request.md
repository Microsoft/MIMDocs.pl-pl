---
title: Przypisywanie karty inteligentnej do żądania | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a8acf3defc2087fc6ea08bf8063579058465fcc8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760510"
---
# <a name="assign-a-smart-card-to-a-request"></a>Przypisywanie karty inteligentnej do żądania
Powiąże określoną kartę inteligentną z określonym żądaniem. Po powiązaniu karty inteligentnej żądanie można wykonać tylko z określoną kartą.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

### <a name="url-parameters"></a>Parametry URL
Brak.

### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądanie HTTP i odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w obszarze *Szczegóły usługi interfejsu API REST* .

### <a name="request-body"></a>Treść żądania
Treść żądania zawiera następujące właściwości:

Właściwość | Opis
---------|-----------
IdentyfikatorŻądania | Identyfikator żądania do powiązania z kartą inteligentną.
cardid | CardID karty inteligentnej.
ATR | Ciąg odpowiedzi na Resetowanie karty inteligentnej (ATR).


## <a name="response"></a>Reakcja
W tej sekcji opisano odpowiedź.

### <a name="response-codes"></a>Kody odpowiedzi

Kod  |Opis  
---------|---------
201 | Utworzone
204 | Brak zawartości
403 | Forbidden
500 | Błąd wewnętrzny

### <a name="response-headers"></a>Nagłówki odpowiedzi
W przypadku typowych nagłówków odpowiedzi zobacz [nagłówki żądań i odpowiedzi HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST* .

### <a name="response-body"></a>Treść odpowiedzi
Po powodzeniu zwraca identyfikator URI nowo utworzonego obiektu karty inteligentnej.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład powiązania karty inteligentnej.

### <a name="example-request"></a>Przykład: żądanie

```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```

## <a name="see-also"></a>Zobacz także

- [Microsoft. CLM. RequestOperations. issmartcard — Metoda (String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
