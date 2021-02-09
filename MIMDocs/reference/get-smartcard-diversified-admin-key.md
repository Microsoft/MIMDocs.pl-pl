---
title: Uzyskiwanie klucza administratora o zróżnicowanej karcie inteligentnej | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5e21cd92496fd31ec12044f3b69599bf96bef62a
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835613"
---
# <a name="get-smart-card-diversified-admin-key"></a>Uzyskiwanie klucza administratora o zróżnicowanej karcie inteligentnej
Pobiera zróżnicowany klucz administratora dla określonej karty inteligentnej.

>[!IMPORTANT]
>Klucz administratora powinien być zróżnicowany tylko wtedy, gdy zasady szablonu profilu wskazują, że powinno to być.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
---------|------------
reqid | Wymagane. Identyfikator żądania, który jest specyficzny dla programu Microsoft Identity Manager (MIM) zarządzania certyfikatami (CM).
scid | Wymagane. Identyfikator karty inteligentnej, który jest specyficzny dla zarządzanie certyfikatami w usłudze MIM. SCID jest uzyskiwany z obiektu [Microsoft. CLM. Shared. SmartCards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
---------|------------
ATR | Opcjonalny. Ciąg odpowiedzi na Resetowanie karty inteligentnej (ATR).
cardid | Wymagane. Identyfikator karty.

### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądanie HTTP i odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w obszarze *Szczegóły usługi interfejsu API REST*.

### <a name="request-body"></a>Treść żądania
Brak.

## <a name="response"></a>Reakcja
W tej sekcji opisano odpowiedź.

### <a name="response-codes"></a>Kody odpowiedzi

Kod  |Opis  
---------|---------
200 | OK
204 | Brak zawartości
403 | Forbidden
500 | Błąd wewnętrzny


### <a name="response-headers"></a>Nagłówki odpowiedzi
W przypadku typowych nagłówków odpowiedzi zobacz [nagłówki żądań i odpowiedzi HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST*.

### <a name="response-body"></a>Treść odpowiedzi
Po pomyślnym zwraca wartość typu Byte obiektu BLOB reprezentującą zróżnicowany klucz administratora.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład, aby uzyskać zróżnicowany klucz administratora dla karty inteligentnej.

### <a name="example-request"></a>Przykład: żądanie

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       

## <a name="see-also"></a>Zobacz też

- [Microsoft. CLM. RequestOperations. issmartcard — Metoda (String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
