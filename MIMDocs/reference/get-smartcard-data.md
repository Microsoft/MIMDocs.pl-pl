---
title: Pobierz profile kart inteligentnych | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 605dfe1359b5706b27682a086f039f53a5ddaa05
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835692"
---
# <a name="get-smart-card-profiles"></a>Pobierz profile kart inteligentnych
Pobiera listę profilów kart inteligentnych dla użytkownika. Lista zawiera możliwe operacje, które mogą być wykonywane przez bieżącego użytkownika. Żądanie można następnie zainicjować dla którejkolwiek z określonych operacji.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


### <a name="url-parameters"></a>Parametry URL

Właściwość| Opis
---------|--------
smartcarduuid | Opcjonalny. Identyfikator UUID karty inteligentnej jest określany przez Microsoft Identity Manager (MIM) Zarządzanie certyfikatami (CM). Wartość odnosi się do pola "UUID" w obiekcie [Microsoft. CLM. Shared. SmartCards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

### <a name="query-parameters"></a>Parametry zapytania

Właściwość| Opis
---------|--------
cardid | Opcjonalny. Identyfikator UUID karty inteligentnej jest określany przez zarządzanie certyfikatami w usłudze MIM. Wartość odnosi się do pola "UUID" w obiekcie [Microsoft. CLM. Shared. SmartCards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) .

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
Po sukcesie zwraca obiekt JSON-Serialized [Microsoft. CLM. Shared. SmartCards. SmartCard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) o następujących właściwościach:

Nazwa | Opis
-----|-----------
AssignedUserUuid | Identyfikator użytkownika, do którego przypisano kartę inteligentną.
ATR | Ciąg odpowiedzi z karty inteligentnej (ATR) dla karty, która jest obecnie inicjowana.
Komentarz | Komentarz opisujący kartę inteligentną.
Flagi | Flagi opisujące kartę inteligentną.
Oprogramowanie pośredniczące | Oprogramowanie pośredniczące dla karty inteligentnej.
ParentSmartcardUuid | Identyfikator starej karty inteligentnej, która została zastąpiona przez kartę inteligentną.
PermanentSmartcardUuid | Identyfikator trwałej karty inteligentnej skojarzonej z kartą inteligentną.
PrimarySmartcardUuid | Identyfikator podstawowej karty inteligentnej.
ProfileTemplateUuid | Identyfikator szablonu profilu, który zawiera zasady i ustawienia zarządzające kartą inteligentną.
ProfileTemplateVersion | Wersja szablonu profilu w momencie utworzenia profilu karty inteligentnej.
SerialNumber | Numer seryjny karty inteligentnej.
Stan | Stan karty inteligentnej.
Identyfikator UUID | Identyfikator profilu karty inteligentnej.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład uzyskiwania profilów kart inteligentnych dla użytkownika.

### <a name="example-request-1"></a>Przykład: żądanie 1

```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1
```

### <a name="example-response-1"></a>Przykład: odpowiedź 1

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

### <a name="example-request-2"></a>Przykład: żądanie 2

```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response-2"></a>Przykład: odpowiedź 2

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

## <a name="see-also"></a>Zobacz też

- [Microsoft. CLM. Shared. SmartCards. SmartCard — Klasa](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
