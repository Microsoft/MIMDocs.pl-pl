---
title: Pobierz proponowane numeru PIN karty inteligentnej | Dokumentacja firmy Microsoft
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Pobierz proponowane numeru PIN karty inteligentnej
Pobiera użytkownika wygenerowana przez serwer numeru PIN.

**Uwaga**: serwer ustawi numer PIN, tylko wtedy, gdy zasady szablonu profilu wskazuje, że należy to zrobić. W przeciwnym razie użytkownik powinien podać go.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/CertificateManagement/API/V1.0/SmartCards/{ID}/serverproposedpin

###<a name="url-parameters"></a>Parametry adresu URL
Parametr | Opis
---------|------------
Identyfikator | Identyfikator karty inteligentnej (dotyczące zarządzania Certyfikatami programu MIM). Uzyskane z obiektu Microsft.Clm.Shared.Smartcard.
###<a name="query-parameters"></a>Parametry zapytań
Parametr | Opis
---------|------------
ATR | Karta atr.
obiekt CardId. | Identyfikator karty.
żądanie | Ciąg reprezentujący żądanie wystawiony przez kartę inteligentną algorytmem base-64.

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
W przypadku powodzenia zwraca ciąg reprezentujący kod PIN proponowane przez serwer.

##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

... body coming soon
```       
