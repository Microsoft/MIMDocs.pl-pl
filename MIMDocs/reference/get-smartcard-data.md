---
title: Pobierz dane karty inteligentnej | Dokumentacja firmy Microsoft
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
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a84982971d169b5c9e7bd758cc74d795416975b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-data"></a>Pobierz dane za pomocą kart inteligentnych
Pobiera listę profile karty inteligentnej z listą możliwych działań, które mogą być wykonywane przez bieżącego użytkownika. Następnie można zainicjować żądanie dla każdej określonej operacji.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/API/V1.0/SmartCards <br/> / CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


###<a name="url-parameters"></a>Parametry adresu URL
Właściwość| Opis
---------|--------
smartcarduuid | Opcjonalny. Karta inteligentna UUID oznaczony zarządzania Certyfikatami programu MIM. To jest pole "uuid" w [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) obiektu.

###<a name="query-parameters"></a>Parametry zapytań
Właściwość| Opis
---------|--------
obiekt CardId. | Opcjonalny. Karta inteligentna UUID oznaczony zarządzania Certyfikatami programu MIM. To jest pole "uuid" w [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) obiektu.


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

##<a name="example"></a>Przykład

###<a name="request-1"></a>Żądanie 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###<a name="response-1"></a>Odpowiedź 1
```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
###<a name="request-2"></a>Żądanie 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###<a name="response-2"></a>Odpowiedź 2
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
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
##<a name="see-also"></a>Zobacz też

-[Klasa Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
