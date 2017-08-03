---
title: Aktualizacja stanu karty inteligentnej | Dokumentacja firmy Microsoft
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
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cb7b04539b8568a8b2ae83172b2aa8cddbf6174d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="update-smartcard-status"></a>Stan aktualizacji za pomocą kart inteligentnych
Aktualizuje stan karty inteligentnej.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
## <a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/ CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>Parametry adresu URL
Parametr | Opis
---------|------------
reqid | Wymagany. Identyfikator żądania (dotyczące zarządzania Certyfikatami programu MIM).
scid | Wymagany. Identyfikator karty inteligentnej (dotyczące zarządzania Certyfikatami programu MIM). Jest to właściwość "uuid" w [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) obiektu.

### <a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
### <a name="request-body"></a>Treść żądania
Treść żądania zawiera następujące właściwości.

Właściwość | Opis
---------|-----------
Stan | Stan na żądanie. Na przykład "wycofany".


## <a name="response"></a>Odpowiedź
### <a name="response-codes"></a>Kody odpowiedzi
Kod  |Opis  
---------|---------
200     | OK
204 | Brak zawartości
403 | Dostęp zabroniony
500 | Błąd wewnętrzny

### <a name="response-headers"></a>Nagłówki odpowiedzi
Dla wspólnych nagłówków odpowiedzi, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
### <a name="response-body"></a>Treść odpowiedzi
W przypadku powodzenia zwraca serializacji JSON [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) obiekt z następującymi właściwościami:

Nazwa | Opis
-----|-----------
AssignedUserUuid | Identyfikator użytkownika, któremu przypisano karty inteligentnej.
ATR | Ciąg karty inteligentnej odpowiedzi do resetowania (ATR) dla karty, który jest obecnie zainicjowany.
Komentarz | Komentarz, który opisuje karty inteligentnej.
Flagi | Flagi, które opisują karty inteligentnej.
Oprogramowanie pośredniczące | Oprogramowanie pośredniczące dla karty inteligentnej.
ParentSmartcardUuid | Identyfikator starego karty inteligentnej, który zastąpił karty inteligentnej.
PermanentSmartcardUuid | Identyfikator stałe karty inteligentnej jest skojarzona z kartą inteligentną.
PrimarySmartcardUuid | Identyfikator podstawowej karty inteligentnej.
ProfileTemplateUuid | Identyfikator szablonu profilu, który zawiera zasady i ustawienia, które decydują karty inteligentnej.
ProfileTemplateVersion | Wersja szablonu profilu w czasie, który został utworzony profil karty inteligentnej.
SerialNumber | Numer seryjny karty inteligentnej.
Stan | Stan karty inteligentnej.
Identyfikator UUID | Identyfikator profilu karty inteligentnej.

## <a name="example"></a>Przykład

### <a name="request"></a>Żądanie
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### <a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
## <a name="see-also"></a>Zobacz też

- [Klasa Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))
