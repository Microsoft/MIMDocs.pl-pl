---
title: "Utwórz żądanie | Dokumentacja firmy Microsoft"
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Utwórz żądanie
Utwórz żądanie zarządzania Certyfikatami programu MIM.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
POST     |/CertificateManagement/API/V1.0/Requests

###<a name="url-parameters"></a>Parametry adresu URL
brak

###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="request-body"></a>Treść żądania
Treść żądania zawiera następujące właściwości.

Właściwość | Opis
---------|-----------
profiletemplateuuid | Wymagany. Identyfikator GUID szablonu profilu, który tworzy żądanie dla użytkownika.
Klasa datacollection | Wymagany. Kolekcja par nazwa wartość reprezentujący dane, które mają zostać podane w zarejestrowany. Kolekcja niezbędnych danych, które należy podać można pobrać z szablonu profilu zasad przepływu pracy. Można określić pustej kolekcji.
docelowy | Opcjonalny. Identyfikator GUID użytkownika docelowego, który ma zostać utworzony dla żądanie. Jeśli nie zostanie określony, wartością domyślną dla bieżącego użytkownika.
typ | Wymagany. Typ żądania, która jest tworzona. Żądanie dostępne typy to: "Zarejestruj", "Duplikat", "OfflineUnblock", "OnlineUpdate", "Odnawiania", "Odzyskać", "RecoverOnBehalf", "Przywróć", "wycofania", "Odwołać", "TemporaryCards" i "Odblokuj".<br/>**Uwaga**: nie wszystkie typy żądań są obsługiwane na wszystkich szablonów profilów. Na przykład nie można określić operacji odblokowania w szablonie profilu oprogramowania.
komentarz | Wymagany. Wszelkie komentarze, które mogą być wprowadzane przez użytkownika. Zasady przepływu pracy określi, czy jest to konieczne. Można określić pustego ciągu.
Priorytet | Opcjonalny. Priorytet żądania. Jeśli nie zostanie określony, domyślny priorytet żądania, zgodnie z ustawieniami szablonu profilu, będą obowiązywać.


##<a name="response"></a>Odpowiedź
###<a name="response-codes"></a>Kody odpowiedzi
Kod  |Opis  
---------|---------
201     | Utworzono
403 | Dostęp zabroniony
500 | Błąd wewnętrzny

###<a name="response-headers"></a>Nagłówki odpowiedzi
Dla wspólnych nagłówków odpowiedzi, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="response-body"></a>Treść odpowiedzi
W przypadku powodzenia zwraca identyfikator URI dla nowo utworzonego żądania.
##<a name="example"></a>Przykład

###<a name="request-1"></a>Żądanie 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Odpowiedź 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Żądanie 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Odpowiedź 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Żądanie 3
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
##<a name="see-also"></a>Zobacz też

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll — metoda](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock — metoda](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover — metoda](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire — metoda](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock — metoda](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
