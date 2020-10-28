---
title: Pobierz dane profilu | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62b4c336122376cd7da836b9ec6c6e09eac24729
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760450"
---
# <a name="get-profile-data"></a>Pobierz dane profilu
Pobiera listę profilów certyfikatów oprogramowania dla użytkownika. Lista zawiera możliwe operacje, które mogą być wykonywane przez bieżącego użytkownika. Żądanie można następnie zainicjować dla którejkolwiek z określonych operacji.

>[!IMPORTANT]
>Serwer ustawia numer PIN tylko wtedy, gdy zasady szablonu profilu wskazują, że należy to zrobić. W przeciwnym razie użytkownik powinien podać kod PIN.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
---------|------------
identyfikator | Identyfikator (GUID) profilu do zwrócenia.
IdentyfikatorŻądania | Identyfikator żądania, dla którego mają zostać zwrócone profile.

### <a name="query-parameters"></a>Parametry zapytania

Parametr | Opis
---------|------------
status | Opcjonalny. Wskazuje stan profilów, dla których mają zostać pobrane dane. Możliwymi typami stanu są "aktywne", "zatwierdzone", "anulowane", "ukończone", "odmowa", "" Executing "," "nie powiodło się," Brak "i" oczekujące ". <br/>Jeśli stan nie zostanie określony, zostaną zwrócone wszystkie profile, niezależnie od stanu.

### <a name="request-headers"></a>Nagłówki żądań
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądanie HTTP i odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w obszarze *Szczegóły usługi interfejsu API REST* .

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
W przypadku typowych nagłówków odpowiedzi zobacz [nagłówki żądań i odpowiedzi HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegółach usługi interfejsu API REST* .

### <a name="response-body"></a>Treść odpowiedzi
Po pomyślnym zwraca listę obiektów [Microsoft. CLM. Shared. profiles. profil](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) o następujących właściwościach:

Właściwość | Opis
---------|------------
AssignedUserUuid | Identyfikator użytkownika, do którego przypisany jest profil.
Komentarz | Komentarz opisujący profil.
Flagi | Flagi opisujące profil.
ParentProfileUuid | Identyfikator starego profilu, który został zastąpiony przez profil.
PrimaryProfileUuid | Identyfikator profilu podstawowego.
ProfileOperations | Lista możliwych operacji, które mogą być wykonywane przez bieżącego użytkownika w profilu.
ProfileTemplateUuid | Identyfikator szablonu profilu, który zawiera zasady i ustawienia zarządzające profilem.
ProfileTemplateVersion | Wersja szablonu profilu w momencie utworzenia profilu.
Stan | Stan profilu.
Identyfikator UUID | Identyfikator profilu.


## <a name="example"></a>Przykład
Ta sekcja zawiera przykład pobierania danych profilu dla użytkownika.

### <a name="example-request"></a>Przykład: żądanie

```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

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

## <a name="see-also"></a>Zobacz także

- [Microsoft. CLM. Shared. profile. profil — Klasa](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
