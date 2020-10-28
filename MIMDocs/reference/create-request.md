---
title: Utwórz żądanie | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 51016951f9dec22e2a40b44a2b76a840192b518f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760667"
---
# <a name="create-request"></a>Utwórz żądanie
Utwórz żądanie zarządzania certyfikatami Microsoft Identity Manager (MIM).

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

### <a name="url-parameters"></a>Parametry URL
Brak.

### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądanie HTTP i odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w obszarze *Szczegóły usługi interfejsu API REST* .

### <a name="request-body"></a>Treść żądania
Treść żądania zawiera następujące właściwości:

Właściwość | Opis
---------|-----------
profiletemplateuuid | Wymagane. Identyfikator GUID szablonu profilu, dla którego użytkownik tworzy żądanie.
DataCollection | Wymagane. Kolekcja par nazwa-wartość reprezentujących dane, które mają być dostarczone przez stronie rejestrującej. Kolekcję niezbędnych danych, które należy podać, można pobrać z zasad przepływu pracy szablonu profilu. Można określić pustą kolekcję.
obiektów | Opcjonalny. Identyfikator GUID użytkownika docelowego, dla którego ma zostać utworzone żądanie. Jeśli nie zostanie określony, docelowym wartością domyślną jest bieżący użytkownik.
typ | Wymagane. Typ tworzonego żądania. Dostępne typy żądań to "Zarejestruj", "Duplikuj", "" OfflineUnblock "," OnlineUpdate "," Renew "," Recover "" RecoverOnBehalf "," "Przywracanie", "wycofywanie", "Odbierz", "TemporaryCards" i "Odblokowywanie".<br/><br/>**Uwaga** : nie wszystkie typy żądań są obsługiwane we wszystkich szablonach profilów. Nie można na przykład określić operacji odblokowywania dla szablonu profilu oprogramowania.
komentarz | Wymagane. Wszelkie komentarze, które mogą zostać wprowadzone przez użytkownika. Zasady przepływu pracy określają, czy właściwość komentarz jest konieczna. Można określić pusty ciąg.
priority | Opcjonalny. Priorytet żądania. Jeśli nie zostanie określony, używany jest domyślny priorytet żądania określony przez ustawienia szablonu profilu.


## <a name="response"></a>Reakcja
W tej sekcji opisano odpowiedź.

### <a name="response-codes"></a>Kody odpowiedzi

Kod  |Opis  
---------|---------
201 | Utworzone
403 | Forbidden
500 | Błąd wewnętrzny

### <a name="response-headers"></a>Nagłówki odpowiedzi
W przypadku typowych nagłówków odpowiedzi zobacz [nagłówki żądań i odpowiedzi HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST* .

### <a name="response-body"></a>Treść odpowiedzi
Po powodzeniu zwraca identyfikator URI nowo utworzonego żądania.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład tworzenia żądań rejestracji i odblokowywania.

### <a name="example-request-1"></a>Przykład: żądanie 1

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```

### <a name="example-response-1"></a>Przykład: odpowiedź 1

```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```

### <a name="example-request-2"></a>Przykład: żądanie 2

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```

### <a name="example-response-2"></a>Przykład: odpowiedź 2

```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

### <a name="example-request-3"></a>Przykład: żądanie 3

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```

## <a name="see-also"></a>Zobacz także

- [Microsoft.Clm.Provision.RequestOperations.IniMetoda tiateEnroll](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.IniMetoda tiateOfflineUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.IniMetoda tiateRecover](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.IniMetoda tiateRetire](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.IniMetoda tiateUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
