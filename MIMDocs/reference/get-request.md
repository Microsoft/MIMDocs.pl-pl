---
title: Pobierz żądanie | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1bc42eb0fb1e54a3425586350ae5ad20495534c5
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835709"
---
# <a name="get-request"></a>Pobierz żądanie
Pobiera co najmniej jedno określone żądanie zarządzania certyfikatami Microsoft Identity Manager (MIM).

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>Parametry URL

Właściwość| Opis
---------|--------
identyfikator| Opcjonalny. Identyfikator GUID żądania do pobrania.

### <a name="query-parameters"></a>Parametry zapytania

Właściwość| Opis
---------|--------
targetuser| Opcjonalny. Użytkownik docelowy żądania. Jeśli nie określono elementu docelowego, wszystkie żądania, niezależnie od użytkownika docelowego są zwracane. <br/><br/>**Uwaga**: obecnie jest obsługiwana tylko wartość "Current".
status| Opcjonalny. Wskazuje stan żądania do pobrania. Możliwymi typami stanu są "zatwierdzone", "anulowane", "ukończone", "odmowa", "" Executing "," "nie powiodło się," "Brak" i "oczekujące". <br/>Jeśli stan nie zostanie określony, wszystkie żądania, niezależnie od stanu są zwracane.

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
Po pomyślnym zwraca jeden lub więcej obiektów [żądania](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) o następujących właściwościach:

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
Ta sekcja zawiera przykład pobierania określonych żądań zarządzanie certyfikatami w usłudze MIM.

### <a name="example-request-1"></a>Przykład: żądanie 1

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1
```

### <a name="example-response-1"></a>Przykład: odpowiedź 1

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
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

### <a name="example-request-2"></a>Przykład: żądanie 2

```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```

### <a name="example-response-2"></a>Przykład: odpowiedź 2

```
HTTP/1.1 200 OK

{
    "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc",
    "RequestType":5,
    "Status":3,
    "Flags":2,
    "DataCollection":[
        {
            "Name":"Sample Data Item",
            "Value":null
        }
    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:40:33.133Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```     

## <a name="see-also"></a>Zobacz też

- [Microsoft. CLM. prowizja. FindOperations. FindRequest — Metoda](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Wyliczenie Microsoft. CLM. Shared. RequestPermission](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft. CLM. Shared. Requests. Request — Klasa](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
