---
title: Pobierz operacje na stanie profilu | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 35246570b52285f916b74785c17ce55f2967fb7d
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760654"
---
# <a name="get-profile-state-operations"></a>Pobierz operacje na stanie profilu
Pobiera listę możliwych operacji, które mogą być wykonywane przez bieżącego użytkownika w określonym profilu. Żądanie można następnie zainicjować dla którejkolwiek z określonych operacji.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{id}/operations

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
---------|------------
identyfikator | Identyfikator (GUID) profilu lub karty inteligentnej.

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
Po powodzeniu zwraca listę możliwych operacji, które mogą zostać wykonane przez użytkownika na karcie inteligentnej. Lista może zawierać dowolną liczbę następujących operacji: **OnlineUpdate** , **Renew** , **Recover** , **RecoverOnBehalf** , **wycofywania** , **odwoływania** i **odblokowywania** .

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład uzyskiwania operacji na stanie profilu dla bieżącego użytkownika.

### <a name="example-request"></a>Przykład: żądanie

```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
