---
title: "Anuluj, Porzuć lub Ukończ żądanie | Dokumentacja firmy Microsoft"
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
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>Anuluj, Porzuć lub Ukończ żądanie
Oznacz żądania zarządzania Certyfikatami programu MIM jako zakończone, anulowane lub porzucone.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
UMIEŚĆ     |/ CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>Parametry adresu URL
Właściwość| Opis
---------|--------
Identyfikator| Wymagany. Identyfikator GUID na zakończenie żądania.


###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="request-body"></a>Treść żądania
Treść żądania zawiera następujące właściwości.

Właściwość | Opis
---------|-----------
Stan | Stan na żądanie. Możliwe wartości to: "Completed", "Anulowane" lub "Abandoned".


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
W przypadku powodzenia zwraca [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) obiekt z następującymi właściwościami żądania zarządzania Certyfikatami programu MIM, który został oznaczony jako zakończone:

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

###<a name="request"></a>Żądanie
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##<a name="see-also"></a>Zobacz też

- [Microsoft.Clm.Provision.ExecuteOperations.Complete — metoda](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
