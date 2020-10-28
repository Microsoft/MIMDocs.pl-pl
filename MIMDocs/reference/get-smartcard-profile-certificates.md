---
title: Uzyskiwanie certyfikatów kart inteligentnych lub profilów | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e089caa78dd9a52517f2cae3fa42176225c73b5
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760391"
---
# <a name="get-smart-card-or-profile-certificates"></a>Uzyskiwanie certyfikatów kart inteligentnych lub profilów
Pobiera listę certyfikatów skojarzonych z konkretną kartą inteligentną lub profilem oprogramowania.

>[!NOTE]
>Adresy URL w tym artykule odnoszą się do nazwy hosta wybranej podczas wdrażania interfejsu API, takich jak `https://api.contoso.com` .

## <a name="request"></a>Żądanie

Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/certificates <br/>/CertificateManagement/api/v1.0/smartcards/{id}/certificates

### <a name="url-parameters"></a>Parametry URL

Parametr | Opis
---------|------------
identyfikator | Identyfikator (GUID) profilu lub karty inteligentnej.

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
Po pomyślnym zwraca listę obiektów [Microsoft. CLM. Shared. Certificates. X509ClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx) z następującymi właściwościami:

Nazwa | Opis
-----|------------
ArchivedOnCa | Wartość logiczna wskazująca, czy certyfikat jest archiwizowany w urzędzie certyfikacji (CA).
CertificateType | Typ certyfikatu.
IsKeyHistory | Wartość logiczna wskazująca, czy certyfikat jest certyfikatem historii klucza.
SerialNumber | Numer seryjny certyfikatu.
TemplateCommonName | Nazwa pospolita szablonu certyfikatu dla certyfikatu.
Odcisk palca | Odcisk palca certyfikatu.

## <a name="example"></a>Przykład
Ta sekcja zawiera przykład uzyskiwania certyfikatów skojarzonych z kartą inteligentną lub profilem oprogramowania.

### <a name="example-request"></a>Przykład: żądanie

```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```

### <a name="example-response"></a>Przykład: odpowiedź

```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01052AFA01313FB77AC00010000B010",
        "Thumbprint":"C52B0C5FB8AAD31A5B239FF2712ED14122D67D30"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B011AB48AE7D664ED5D900010000B011",
        "Thumbprint":"E7C4324896271BE869544FF28AA2B1BF3B3BDFCF"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01ABCF2D38A0CCEAD8F00010000B01A",
        "Thumbprint":"D9293B5414C644888444541B64631E90F2612425"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B1F02865CF2C3A9DD96D00010000B1F0",
        "Thumbprint":"6615DDC8603DBA789D724502627682F37D0FC2D0"
    }
]
```       

## <a name="see-also"></a>Zobacz także

- [Microsoft. CLM. prowizja. FindOperations. FindCertificates — Metoda](https://msdn.microsoft.com/library/microsoft.clm.provision.findoperations.findcertificates.aspx)
- [Microsoft. CLM. Shared. Certificates. X509ClmCertificate — Klasa](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx)
