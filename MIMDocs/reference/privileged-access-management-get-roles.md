---
title: Pobierz role PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f802dc22019f1ad97fc5cce8c9ed2e000349dc58
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760889"
---
# <a name="get-pam-roles"></a>Pobierz role PAM
Używane przez uprzywilejowane konto do wyświetlania listy ról usługi PAM, dla których konto jest kandydatem.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/api/pamresources/pamroles

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
----------|--------------
$filter | Opcjonalny. Określ dowolne właściwości roli PAM w wyrażeniu filtru, aby zwrócić przefiltrowaną listę odpowiedzi. Aby uzyskać więcej informacji na temat obsługiwanych operatorów, zobacz [filtrowanie w obszarze interfejsu API REST usługi PAM](privileged-access-management-rest-api-service-details.md#filtering).
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
Pomyślna odpowiedź zawiera kolekcję co najmniej jednej roli PAM, z której każdy ma następujące właściwości:

Właściwość | Opis
--------|-------------
RoleID | Unikatowy identyfikator (GUID) roli PAM.
Nazwa wyświetlana | Nazwa wyświetlana roli PAM w usłudze MIM.
Opis | Opis roli PAM w usłudze MIM.
TTL | Maksymalny limit czasu wygaśnięcia praw dostępu roli (w sekundach).
AvailableFrom | Najwcześniejsza godzina aktywacji żądania.
AvailableTo | Najpóźniejsza godzina aktywacji żądania.
MFAEnabled | Wartość logiczna wskazująca, czy żądania aktywacji dla tej roli wymagają wyzwania usługi MFA.
ApprovalEnabled | Wartość logiczna wskazująca, czy żądania aktywacji dla tej roli wymagają zatwierdzenia przez właściciela roli.
AvailabilityWindowEnabled | Wartość logiczna wskazująca, czy rola może być aktywowana tylko w określonym przedziale czasu.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład pobierania ról PAM.

### <a name="example-request"></a>Przykład: żądanie

```
GET /api/pamresources/pamroles HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

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
