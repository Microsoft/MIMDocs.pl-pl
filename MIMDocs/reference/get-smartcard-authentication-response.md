---
title: Pobieranie odpowiedzi uwierzytelniania karty inteligentnej | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62811bd5d225f981ded6a6439584c2b7730251c1
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835595"
---
# <a name="get-smart-card-authentication-response"></a>Pobierz odpowiedź uwierzytelniania karty inteligentnej
Pobiera odpowiedź na wyzwanie uwierzytelniania podstawowego dostawcy usług kryptograficznych (CSP).

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
---------|------------
reqid | Wymagane. Identyfikator żądania, który jest specyficzny dla programu Microsoft Identity Manager (MIM) zarządzania certyfikatami (CM).
scid | Wymagane. Identyfikator karty inteligentnej, który jest specyficzny dla zarządzanie certyfikatami w usłudze MIM. SCID jest uzyskiwany z obiektu [Microsoft. CLM. Shared. SmartCards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
---------|------------
ATR | Opcjonalny. Ciąg odpowiedzi na Resetowanie karty inteligentnej (ATR).
cardid | Wymagane. Identyfikator karty inteligentnej.
Sprawdz | Wymagane. Zakodowany ciąg Base-64 reprezentujący wyzwanie, które jest wystawione przez kartę inteligentną.
zróżnicowanego | Wymagane. Flaga logiczna określająca, czy klucz administratora karty inteligentnej był zróżnicowany.

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
Po sukcesie zwraca obiekt BLOB typu Byte reprezentujący odpowiedź na żądanie.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład, aby uzyskać odpowiedź na wyzwanie podstawowego uwierzytelniania CSP.

### <a name="example-request"></a>Przykład: żądanie

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       

## <a name="see-also"></a>Zobacz też

- [Microsoft.Clm.Provision.ExecuteOperations. GetBaseCspResponse — Metoda](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
