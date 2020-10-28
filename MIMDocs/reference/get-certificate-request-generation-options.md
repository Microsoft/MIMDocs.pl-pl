---
title: Pobierz opcje generowania żądania certyfikatu | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d8baab788c07dfb8c009857b45e38eb662f98199
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760486"
---
# <a name="get-certificate-request-generation-options"></a>Pobierz opcje generowania żądania certyfikatu
Pobiera parametry dla generowania żądania certyfikatu po stronie klienta.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

| Metoda |                                       Adres URL żądania                                        |
|--------|------------------------------------------------------------------------------------------|
|  GET   | /CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions |

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
--------|--------------
IdentyfikatorŻądania| Wymagane. Identyfikator GUID żądania zarządzanie certyfikatami w usłudze MIM, dla którego mają zostać pobrane parametry generacji żądania certyfikatu.

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
W przypadku typowych nagłówków żądań zapoznaj się z [nagłówkami żądanie HTTP i odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w obszarze *Szczegóły usługi interfejsu API REST* .

### <a name="response-body"></a>Treść odpowiedzi
Po powodzeniu funkcja zwraca listę obiektów CertificateRequestGenerationOptions. Każdy obiekt CertificateRequestGenerationOptions odnosi się do pojedynczego żądania certyfikatu, które klient musi wygenerować. Każdy obiekt ma następujące właściwości:

Właściwość| Opis
--------|-----------
Można eksportować | Wartość określająca, czy klucz prywatny utworzony dla żądania można wyeksportować.
FriendlyName | Nazwa wyświetlana zarejestrowanego certyfikatu.
HashAlgorithmName | Algorytm wyznaczania wartości skrótu używany podczas tworzenia sygnatury żądania certyfikatu.
Algorytm algorytmu | Algorytm klucza publicznego.
Parametru ProtectionLevel | Poziom silnej ochrony klucza.
KeySize | Rozmiar w bitach klucza prywatnego, który ma zostać wygenerowany.
KeyStorageProviderNames | Lista akceptowalnych dostawców magazynu kluczy (KSP), których można użyć do wygenerowania klucza prywatnego. Gdy pierwszy dostawcy magazynu kluczy nie może zostać użyty do wygenerowania żądania certyfikatu, można użyć dowolnego z określonych KSP, dopóki jeden z nich nie powiedzie się.
Użycie użycia | Operacja, którą można wykonać przy użyciu klucza prywatnego utworzonego dla tego żądania certyfikatu. Wartością domyślną jest podpisywanie.
Temat | Nazwa podmiotu.

>[!NOTE]
>Więcej informacji o tych właściwościach jest dostępnych w opisie [klasy Windows. Security. Cryptography. Certificates. CertificateRequestProperties](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) . Należy pamiętać, że nie istnieje taka sama zgodność między tą klasą a obiektami CertificateRequestGenerationOptions.
>

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład uzyskiwania opcji generacji dla żądania certyfikatu.

### <a name="example-request"></a>Przykład: żądanie

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```  
