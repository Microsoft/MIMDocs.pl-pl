---
title: Pobierz dane profilu | Dokumentacja firmy Microsoft
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>Pobierz dane profilu
Pobiera listę profilów certyfikatów oprogramowania użytkownika z listy możliwych działań, które mogą być wykonywane przez bieżącego użytkownika. Następnie można zainicjować żądanie dla każdej określonej operacji.

**Uwaga**: serwer ustawi numer PIN, tylko wtedy, gdy zasady szablonu profilu wskazuje, że należy to zrobić. W przeciwnym razie użytkownik powinien podać go.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/API/V1.0/Profiles<br/>/ CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/API/V1.0/Requests/{RequestId}/Profiles

###<a name="url-parameters"></a>Parametry adresu URL
Parametr | Opis
---------|------------
Identyfikator | Identyfikator (GUID) profilu do zwrócenia.
Identyfikator żądania | Identyfikator żądania do zwrócenia profilów.

###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
---------|------------
Stan | Opcjonalny. Wskazuje stan profile można pobrać danych. Stan możliwe typy to: "Active", "Zatwierdzone", "Anulowane", "Ukończone", "Odmowa dostępu", "Wykonywanie", "Niepowodzenie", "None" i "Oczekujące". <br/>Jeśli stan nie zostanie określony, zostaną zwrócone wszystkie profile, niezależnie od stanu.
###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="request-body"></a>Treść żądania
brak

##<a name="response"></a>Odpowiedź
###<a name="response-codes"></a>Kody odpowiedzi
Kod  |Opis  
---------|---------
200 | OK
204 | Brak zawartości
403 | Dostęp zabroniony
500 | Błąd wewnętrzny

###<a name="response-headers"></a>Nagłówki odpowiedzi
Dla wspólnych nagłówków odpowiedzi, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="response-body"></a>Treść odpowiedzi
W przypadku powodzenia zwraca listę serializacji JSON [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) obiektów z następującymi właściwościami:

Właściwość | Opis
---------|------------
AssignedUserUuid | Identyfikator użytkownika, do którego jest przypisany profil.
Komentarz | Komentarz, który zawiera opis profilu.
Flagi | Flagi, które opisują profilu.
ParentProfileUuid | Identyfikator starego profil, który zastąpił profilu.
PrimaryProfileUuid | Identyfikator profilu podstawowego.
ProfileOperations | Lista możliwych działań, które mogą być wykonywane przez bieżącego użytkownika w profilu.
ProfileTemplateUuid | Identyfikator szablonu profilu, który zawiera zasady i ustawienia, które określają sposób profilu.
ProfileTemplateVersion | Wersja szablonu profilu w czasie profil został utworzony.
Stan | Stan profilu.
Identyfikator UUID | Identyfikator profilu.


##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##<a name="see-also"></a>Zobacz też

- [Klasa Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
