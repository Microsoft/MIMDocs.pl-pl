---
title: "Pobierz odpowiedź uwierzytelnienia karty inteligentnej | Dokumentacja firmy Microsoft"
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
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1c7a6f5e7ebd1e6e4cfc0992607adb91743f343e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-authentication-response"></a>Pobierz odpowiedź uwierzytelnienia karty inteligentnej
Pobiera odpowiedź na żądanie uwierzytelnienia podstawowego dostawcy usług Kryptograficznych.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/API/V1.0/Requests/{reqid}/SmartCards/{scid}/authenticationresponse

###<a name="url-parameters"></a>Parametry adresu URL
Parametr | Opis
---------|------------
reqid | Wymagany. Identyfikator żądania (dotyczące zarządzania Certyfikatami programu MIM).
scid | Wymagany. Identyfikator karty inteligentnej (dotyczące zarządzania Certyfikatami programu MIM). Uzyskane z [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) obiektu.
###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
---------|------------
ATR | Opcjonalny. Ciąg (ATR) odpowiedzi do resetowania karty inteligentnej.
obiekt CardId. | Wymagany. Identyfikator karty.
żądanie | Wymagany. Ciąg reprezentujący żądanie wystawiony przez kartę inteligentną algorytmem base-64.
zróżnicowane | Wymagany. Flaga wartości logicznej wskazująca pogody klucz administratora za pomocą kart inteligentnych został zróżnicowane.


###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="request-body"></a>Treść żądania
brak

##<a name="response"></a>Odpowiedź
###<a name="response-codes"></a>Kody odpowiedzi
Kod  |Opis  
---------|---------
200     | OK
204 | Brak zawartości
403 | Dostęp zabroniony
500 | Błąd wewnętrzny

###<a name="response-headers"></a>Nagłówki odpowiedzi
Dla wspólnych nagłówków odpowiedzi, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="response-body"></a>Treść odpowiedzi
W przypadku powodzenia zwraca bajt obiektu BLOB, który reprezentuje odpowiedź na żądanie.

##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##<a name="see-also"></a>Zobacz też

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse — metoda](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
