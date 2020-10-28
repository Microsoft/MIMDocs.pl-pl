---
title: Pobieranie proponowanego numeru PIN karty inteligentnej | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9d4f1444efa490f980e29440342844ac246d8a20
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760606"
---
# <a name="get-smart-card-proposed-pin"></a>Pobieranie proponowanego numeru PIN karty inteligentnej
Pobiera numer PIN użytkownika wygenerowany przez serwer.

>[!IMPORTANT]
>Serwer ustawia tylko numer PIN, jeśli zasady szablonu profilu wskazują, że należy to zrobić. W przeciwnym razie użytkownik powinien podać kod PIN.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
---------|------------
identyfikator | Identyfikator karty inteligentnej, który jest specyficzny dla programu Microsoft Identity Manager (MIM) zarządzania certyfikatami (CM). Identyfikator jest uzyskiwany z obiektu Microsoft. CLM. Shared. SmartCard.

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
---------|------------
ATR | Ciąg odpowiedzi na Resetowanie karty inteligentnej (ATR).
cardid | Identyfikator karty inteligentnej.
Sprawdz | Zakodowany ciąg Base-64 reprezentujący wyzwanie, które jest wystawione przez kartę inteligentną.

### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądanie HTTP i odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w obszarze *Szczegóły usługi interfejsu API REST* .

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
W przypadku typowych nagłówków odpowiedzi zobacz [nagłówki żądań i odpowiedzi HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST* .

### <a name="response-body"></a>Treść odpowiedzi
Po powodzeniu funkcja zwraca ciąg, który reprezentuje numer PIN proponowany przez serwer.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład pobierania numeru PIN użytkownika generowanego przez serwer.

### <a name="example-request"></a>Przykład: żądanie

```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

... body coming soon
```       
