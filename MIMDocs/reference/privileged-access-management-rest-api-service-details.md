---
title: Szczegóły usługi interfejsu API REST PAM | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00a2f4d9c44747d50139655d368e42b11fbd388c
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/14/2020
ms.locfileid: "92760985"
---
# <a name="pam-rest-api-service-details"></a>Szczegóły usługi interfejsu API REST PAM
W poniższych sekcjach omówiono szczegółowe informacje o interfejsie API REST usługi Microsoft Identity Manager (MIM) Privileged Access Management (PAM).

<h2 id="http-request-and-response-headers">Nagłówki żądań HTTP</h2>

Żądania HTTP wysyłane do interfejsu API powinny zawierać następujące nagłówki (Ta lista nie jest wyczerpująca):

Header | Opis
-------|------------
Autoryzacja | Wymagane. Zawartość zależy od metody uwierzytelniania, która jest konfigurowalna i może opierać się na użyciu usługi WIA (uwierzytelnianie zintegrowane systemu Windows) lub usług AD FS.
Content-Type | Wymagane, jeśli żądanie ma treść. Musi być ustawiony na `application/json` .
Długość zawartości | Wymagane, jeśli żądanie ma treść. 
Plików | Plik cookie sesji. Może być wymagana w zależności od metody uwierzytelniania.

## <a name="http-response-headers"></a>Nagłówki odpowiedzi HTTP

Odpowiedzi HTTP powinny zawierać następujące nagłówki (Ta lista nie jest wyczerpująca):

Header | Opis
-------|------------
Content-Type | Interfejs API zawsze zwraca wartość `application/json` .
Długość zawartości | Długość treści żądania (jeśli jest obecna) w bajtach.

## <a name="versioning"></a>Przechowywanie wersji 
Bieżąca wersja interfejsu API to 1. Wersję interfejsu API można określić za pomocą parametru zapytania w adresie URL żądania, jak w poniższym przykładzie: `http://localhost:8086/api/pamresources/pamrequests?v=1` Jeśli wersja nie zostanie określona w żądaniu, żądanie jest wykonywane względem ostatnio wydanej wersji interfejsu API. 

## <a name="security"></a>Zabezpieczenia 
Dostęp do interfejsu API wymaga zintegrowanego uwierzytelniania systemu Windows (IWA). Należy to skonfigurować ręcznie w usługach IIS przed instalacją Microsoft Identity Manager (MIM).

Protokół HTTPS (TLS) jest obsługiwany, ale powinien być skonfigurowany ręcznie w usługach IIS. Aby uzyskać więcej informacji, zobacz: **implementowanie SSL (SSL) dla portalu programu FIM** w [kroku 9: wykonywanie zadań po instalacji w programie FIM 2010 R2](https://technet.microsoft.com/library/hh322875.aspx) w przewodniku po laboratorium testowym programu FIM 2010 R2. 

Nowy certyfikat serwera SSL można wygenerować, uruchamiając następujące polecenie w wierszu polecenia programu Visual Studio:

```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Polecenie tworzy certyfikat z podpisem własnym, który może służyć do testowania aplikacji sieci Web, która używa protokołu SSL na serwerze sieci Web, na którym znajduje się adres URL `test.cwap.com` . Identyfikator OID zdefiniowany przez `-eku` opcję służy do identyfikowania certyfikatu jako certyfikatu serwera SSL. Certyfikat jest przechowywany **w magazynie i jest dostępny na poziomie** komputera. Certyfikat można wyeksportować z przystawki Certyfikaty w mmc.exe.

## <a name="cross-domain-access-cors"></a>Dostęp do wielu domen (CORS) 
Mechanizm CORS jest obsługiwany, ale powinien być konfigurowany ręcznie w usługach IIS. Dodaj następujące elementy do wdrożonego pliku web.config API, aby skonfigurować interfejs API do zezwalania na wywołania między domenami: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```

## <a name="error-handling"></a>Obsługa błędów 
Interfejs API zwraca odpowiedzi na błędy HTTP w celu wskazania warunków błędu. Błędy są zgodne ze standardem OData. W poniższej tabeli przedstawiono kody błędów, które mogą zostać zwrócone do klienta:

Kod stanu HTTP | Opis
-----------------|------------
401 | Brak autoryzacji 
403 | Forbidden 
408 | Limit czasu żądania   
500 | Wewnętrzny błąd serwera 
503 | Usługa jest niedostępna 

## <a name="filtering"></a>Filtrowanie 
Żądania interfejsu API REST usługi PAM mogą zawierać filtry, aby określić właściwości, które powinny być uwzględnione w odpowiedzi. Składnia filtru jest oparta na wyrażeniach OData.

Filtry mogą określać dowolne właściwości żądań PAM, ról PAM. lub oczekujące żądania PAM. Na przykład: *ExpirationTime* , *DisplayName* lub jakakolwiek inna prawidłowa właściwość żądania PAM, roli PAM lub oczekującego żądania.

Interfejs API obsługuje następujące operatory w wyrażeniach filtru: *and* , *EQUAL* , *NotEqual* , *GreaterThan* , *LessThan* , *GreaterThenOrEqueal* i *LessThanOrEqual* . 

Następujące przykładowe żądania obejmują filtry:

- To żądanie zwraca wszystkie żądania PAM między określonymi datami: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime'2015-01-09T08:26:49.721Z' and ExpirationTime lt datetime'2015-02-10T08:26:49.722Z' `
 
- To żądanie zwraca rolę PAM o nazwie wyświetlanej "dostęp do plików SQL": `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq 'SQL File Access' `
