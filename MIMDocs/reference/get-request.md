---
title: "Pobrać żądania | Dokumentacja firmy Microsoft"
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
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 56bb25f7282879943dc624f3c24b836c7ae4a58c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-request"></a>Pobrać żądania
Pobiera określony co najmniej jedno żądanie zarządzania Certyfikatami programu MIM.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/ CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>Parametry adresu URL
Właściwość| Opis
---------|--------
Identyfikator| Opcjonalny. Identyfikator GUID żądanie pobrania.
###<a name="query-parameters"></a>Parametry zapytań
Właściwość| Opis
---------|--------
Użytkownik docelowy| Opcjonalny. Użytkownik docelowy żądania. Jeśli docelowy nie zostanie określony, zostaną zwrócone wszystkie żądania, niezależnie od docelowego użytkownika. <br/> **Uwaga**: tylko "current" jest obecnie obsługiwany.
Stan| Opcjonalny. Wskazuje stan żądania pobrania. Stan możliwe typy to: *zatwierdzone*, *anulowane*, *Ukończono*, *odmowa*, *Executing*, **, *Brak*, i *oczekujące*. <br/>Jeśli stan nie zostanie określony, zostaną zwrócone wszystkie żądania, niezależnie od stanu.

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
W przypadku powodzenia zwraca co najmniej jeden [żądania](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) obiektów z następującymi właściwościami:

Nazwa | Opis
-----|------------
Komentarz | Komentarz, który jest skojarzony z tym żądaniem Menedżer certyfikatów programu MIM.
Ukończone | Czas zakończenia żądania zarządzania Certyfikatami programu MIM.
Klasa DataCollection | Elementy kolekcji danych, które są skojarzone z tym żądaniem Menedżer certyfikatów programu MIM.
DataCollectionFlags | Opcje zbierania danych dla żądania zarządzania Certyfikatami programu MIM.
Flagi | Opcje, które są skojarzone z tym żądaniem Menedżer certyfikatów programu MIM.
IsDataCollectionComplete | Wartość logiczna wskazująca, czy zakończeniu zbierania danych dla żądania zarządzania Certyfikatami programu MIM.
IsEnrollmentAgent | Wartość logiczna wskazująca, czy agenta rejestracji jest wymagana do uruchamiania żądania zarządzania Certyfikatami programu MIM.
IsSmartcard | Wartość logiczna wskazująca, czy żądanie zarządzania Certyfikatami programu MIM jest żądania karty inteligentnej lub żądanie profilu oprogramowania.
NewProfileUuid | Tożsamość nowy profil oprogramowania, który jest generowany przez żądanie zarządzania Certyfikatami programu MIM.
NewSmartcardUuid | Tożsamość Nowa karta inteligentna, który jest przypisywany przez żądanie zarządzania Certyfikatami programu MIM.
OldProfileUuid | Tożsamość profilu oprogramowania, dla którego utworzono żądanie zarządzania Certyfikatami programu MIM.
OldSmartcardUuid | Tożsamość karty inteligentnej, dla którego utworzono żądanie zarządzania Certyfikatami programu MIM.
OriginatorUserUuid | Tożsamość użytkownika, który wysłał zarządzania Certyfikatami programu MIM
Priorytet | Priorytet żądania zarządzania Certyfikatami programu MIM.
ProfileTemplateUuid | Tożsamość szablonu profilu, dla którego utworzono żądanie zarządzania Certyfikatami programu MIM.
RequestType | Typ żądania zarządzania Certyfikatami programu MIM.
SecurityDescriptor | Deskryptor zabezpieczeń dla żądania zarządzania Certyfikatami programu MIM.
Stan | Stan żądania zarządzania Certyfikatami programu MIM.
Przesłane | Podczas zarządzania Certyfikatami programu MIM żądanie zostało przesłane.
TargetUserUuid | Tożsamość użytkownika docelowego dla żądania zarządzania Certyfikatami programu MIM.
Identyfikator UUID | Identyfikator żądania zarządzania Certyfikatami programu MIM.

##<a name="example"></a>Przykład

###<a name="request-1"></a>Żądanie 1
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1

```
###<a name="response-1"></a>Odpowiedź 1
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
}```       
###Request 2
```
Pobierz /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```
###Response 2
```
HTTP/1.1 200 OK

{"Uuid": "0c96d73f-967b-420e-854a-43ad2a1504bc", "RequestType": 5, "Status": 3, "flag": 2, "DataCollection": [{"Name": "Element danych przykładowych", "Wartość": null}], "DataCollectionFlags": 32, "OriginatorUserUuid": "8f1590dc-d932-4b66-8e68-2e91c5880780", "TargetUserUuid": "8f1590dc-d932-4b66-8e68-2e91c5880780", "przesłane": "2015-07-07T23:40:33.133Z", "Ukończone": "0001-01-01T00:00:00", "NewProfileUuid": "b99ff38c-6653-471f-ae3c-325bb351a6bc", "OldProfileUuid": "b99ff38c-6653-471f-ae3c-325bb351a6bc", "NewSmartcardUuid": "17cf063d-e337-4aa9-a822-346554ddd3c9", "OldSmartcardUuid": "17cf063d-e337-4aa9-a822-346554ddd3c9", "Priority": 0, "Comment": "", "ProfileTemplateUuid": "a039b4d0-5ce8-4eff-8651-181c6576fda3", "SecurityDescriptor": "O:S-1-5-21-2127521184-1604012920-1887927527-14134865 D:(D;; LC;; S-1-5-21-2127521184-1604012920-1887927527-14995557) (A; SWRC;; S-1-5-21-2127521184-1604012920-1887927527-5094533) (A; RC;; WD) (A; CCDCLCSWSDRC;; S-1-5-21-2127521184-1604012920-1887927527-14134865) (A; D CSW;; S-1-5-21-2127521184-1604012920-1887927527-14995557) (A; CCDCSW;; S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000 ","IsSmartcard": wartość true,"IsEnrollmentAgent": wartość false,"IsDataCollectionComplete": false}
```       

##See Also

- [Microsoft.Clm.Provision.FindOperations.FindRequest Method](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft.Clm.Shared.RequestPermission Enumeration](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft.Clm.Shared.Requests.Request Class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
