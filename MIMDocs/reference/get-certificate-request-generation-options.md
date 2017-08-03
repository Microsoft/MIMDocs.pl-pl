---
title: "Opcje generowania żądania certyfikatu | Dokumentacja firmy Microsoft"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Opcje generowania żądania certyfikatu

Pobiera parametry dla generowania żądania certyfikatu klienta.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/API/V1.0/Requests/{RequestId}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>Parametry adresu URL
Parametr | Opis
--------|--------------
Identyfikator żądania| Wymagany. Identyfikator GUID żądania zarządzania Certyfikatami programu MIM, dla którego mają zostać pobrane parametry generowania żądania certyfikatu.

###<a name="request-headers"></a>Nagłówki żądania
Dla typowych nagłówków żądań, zobacz [żądania HTTP i nagłówków odpowiedzi](certificate-management-rest-api-service-details.md#http-request-and-response-headers) w *szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze*.
###<a name="request-body"></a>Treść żądania
Brak.


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
W przypadku powodzenia zwraca listę obiektów CertificateRequestGenerationOptions. Każdy obiekt CertificateRequestGenerationOptions odpowiada na żądanie jeden certyfikat, że klient ma generować i ma następujące właściwości:

Właściwość| Opis
--------|-----------
Można eksportować | Wartość, która określa, czy można wyeksportować klucza prywatnego utworzone dla żądania.
FriendlyName | Nazwa wyświetlana w zarejestrowany certyfikat.
HashAlgorithmName | Algorytm wyznaczania wartości skrótu używany podczas tworzenia podpisu żądania certyfikatu.
KeyAlgorithmName | Algorytm klucza publicznego.
KeyProtectionLevel | Poziom silnej ochrony klucza.
KeySize | Rozmiar w bitach klucza prywatnego do wygenerowania.
KeyStorageProviderNames | Lista dostawców dopuszczalne magazynu kluczy (KSP), które mogą służyć do generowania klucza prywatnego. W przypadku że pierwszy KLUCZY nie można użyć do wygenerowania żądania certyfikatu, żadnego określonego dostawcy magazynów kluczy może służyć do czasu znalezienia właściwego konta.
Elementów Keyusage | Operacja, która może zostać wykonana przez klucz prywatny utworzone dla tego żądania certyfikatu. Wartość domyślna to podpisywania.
Temat | Nazwa podmiotu.

**Uwaga**: więcej informacji o tych właściwościach znajduje się w [klasy Windows.Security.Cryptography.Certificates.CertificateRequestProperties](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) opis, ale należy pamiętać, że nie ma odpowiednika między to klasa i obiekty CertificateRequestGenerationOptions.

##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Odpowiedź
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
