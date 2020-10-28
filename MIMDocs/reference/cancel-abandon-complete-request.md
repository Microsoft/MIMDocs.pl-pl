---
title: Anulowanie, porzucanie lub Kończenie żądania | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2016
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e971309963968ddd52ef44644fb9319738df6242
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760487"
---
# <a name="cancel-abandon-or-complete-a-request"></a>Anulowanie, porzucanie lub Kończenie żądania
Oznacz żądanie zarządzania certyfikatami Microsoft Identity Manager (MIM) jako ukończone, anulowane lub porzucone.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>Parametry URL

Właściwość| Opis
---------|--------
identyfikator| Wymagane. Identyfikator GUID żądania do ukończenia.


### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądanie HTTP i odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w obszarze *Szczegóły usługi interfejsu API REST* .

### <a name="request-body"></a>Treść żądania
Treść żądania zawiera następujące właściwości:

Właściwość | Opis
---------|-----------
status | Stan, dla którego ma zostać ustawione żądanie. Możliwe wartości to "ukończone", "anulowane" lub "porzucone".


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
Po powodzeniu funkcja zwraca obiekt [Microsoft. CLM. Shared. Requests. Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) . Obiekt opisuje żądanie zarządzanie certyfikatami w usłudze MIM, które zostało oznaczone jako ukończone. Obiekt zawiera następujące właściwości:

Nazwa | Opis
-----|------------
Komentarz | Komentarz skojarzony z żądaniem zarządzanie certyfikatami w usłudze MIM.
Ukończone | Godzina ukończenia żądania zarządzanie certyfikatami w usłudze MIM.
DataCollection | Elementy kolekcji danych, które są skojarzone z żądaniem zarządzanie certyfikatami w usłudze MIM.
DataCollectionFlags | Opcje zbierania danych dla żądania zarządzanie certyfikatami w usłudze MIM.
Flagi | Opcje, które są skojarzone z żądaniem zarządzanie certyfikatami w usłudze MIM.
IsDataCollectionComplete | Wartość logiczna wskazująca, czy dla żądania zarządzanie certyfikatami w usłudze MIM zostało wykonane zbieranie danych.
IsEnrollmentAgent | Wartość logiczna wskazująca, czy Agent rejestracji jest wymagany do uruchomienia żądania zarządzanie certyfikatami w usłudze MIM.
Issmartcard | Wartość logiczna wskazująca, czy żądanie zarządzanie certyfikatami w usłudze MIM jest żądaniem karty inteligentnej, czy żądaniem profilu oprogramowania.
NewProfileUuid | Tożsamość nowego profilu oprogramowania wygenerowanego przez żądanie zarządzanie certyfikatami w usłudze MIM.
NewSmartcardUuid | Tożsamość nowej karty inteligentnej, która jest przypisana przez żądanie zarządzanie certyfikatami w usłudze MIM.
OldProfileUuid | Tożsamość profilu oprogramowania, dla którego utworzono żądanie zarządzanie certyfikatami w usłudze MIM.
OldSmartcardUuid | Tożsamość karty inteligentnej, dla której zostało utworzone żądanie zarządzanie certyfikatami w usłudze MIM.
OriginatorUserUuid | Tożsamość użytkownika, który wysłał żądanie zarządzanie certyfikatami w usłudze MIM.
Priorytet | Priorytet żądania zarządzanie certyfikatami w usłudze MIM.
ProfileTemplateUuid | Tożsamość szablonu profilu, dla którego utworzono żądanie zarządzanie certyfikatami w usłudze MIM.
RequestType | Typ żądania zarządzanie certyfikatami w usłudze MIM.
SecurityDescriptor | Deskryptor zabezpieczeń dla żądania zarządzanie certyfikatami w usłudze MIM.
Stan | Stan żądania zarządzanie certyfikatami w usłudze MIM.
Przesłane | Godzina, o której żądanie zarządzanie certyfikatami w usłudze MIM zostało przesłane.
TargetUserUuid | Tożsamość użytkownika docelowego dla żądania zarządzanie certyfikatami w usłudze MIM.
Identyfikator UUID | Identyfikator żądania zarządzanie certyfikatami w usłudze MIM.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład, aby oznaczyć żądanie jako zakończone.

### <a name="example-request"></a>Przykład: żądanie

```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    "status":"Completed"
}
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```

## <a name="see-also"></a>Zobacz także

- [Microsoft.Clm.Provision.ExecuteOperations. Complete — Metoda](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft. CLM. Shared. Requests. Request — Klasa](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
