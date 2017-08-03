---
title: "Przypisz karty inteligentnej na żądanie | Dokumentacja firmy Microsoft"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>Przypisz karty inteligentnej na żądanie
Wiąże określone żądanie określonej karty inteligentnej. Raz powiązany, żądanie można wykonać tylko z tej karty.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
POST     |/CertificateManagement/API/V1.0/SmartCards

###<a name="url-parameters"></a>Parametry adresu URL
Brak.
###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="request-body"></a>Treść żądania
Treść żądania zawiera następujące właściwości.

Właściwość | Opis
---------|-----------
Identyfikator żądania | Identyfikator żądania powinien być powiązana karty inteligentnej.
obiekt CardId. | Obiekt CardId karty inteligentnej.
ATR | Ciąg (ATR) odpowiedzi do resetowania karty inteligentnej.


##<a name="response"></a>Odpowiedź
###<a name="response-codes"></a>Kody odpowiedzi
Kod  |Opis  
---------|---------
201     | Utworzono
204 | Brak zawartości
403 | Dostęp zabroniony
500 | Błąd wewnętrzny

###<a name="response-headers"></a>Nagłówki odpowiedzi
Dla wspólnych nagłówków odpowiedzi, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="response-body"></a>Treść odpowiedzi
W przypadku powodzenia zwraca identyfikator URI do obiektu nowo utworzonej karty inteligentnej.
##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>Zobacz też

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard — metoda (ciąg, żądanie)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
