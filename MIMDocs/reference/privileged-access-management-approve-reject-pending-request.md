---
title: Zatwierdzanie lub odrzucanie oczekującego żądania PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 743c28b343c374d3406600751b379e8a4695c2d0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760780"
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Zatwierdzanie lub odrzucanie oczekującego żądania PAM
Używane przez uprzywilejowane konto do zatwierdzania, zamykania lub odrzucania żądania podniesienia uprawnień do roli PAM.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/api/pamresources/pamrequeststoapprove({approvalId)/Reject

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
----------|-----------
approvalId | Identyfikator (GUID) obiektu zatwierdzenia w usłudze PAM, określony jako `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

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
Ta sekcja zawiera przykład zatwierdzania żądania podniesienia uprawnień do roli PAM.

### <a name="example-request"></a>Przykład: żądanie

```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK
```       
