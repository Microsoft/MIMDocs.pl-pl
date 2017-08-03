---
title: "Szczegóły usługi interfejsu API PAM REST | Dokumentacja firmy Microsoft"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>Szczegóły usługi interfejsu API PAM REST
W poniższych sekcjach omówiono szczegóły interfejsu API REST Privileged Access Management (PAM) programu Microsoft Identity Manager (MIM).

## <a name="http-request-and-response-headers"></a>Żądania HTTP i nagłówków odpowiedzi

Żądania HTTP wysyłane do interfejsu API powinny zawierać następujące nagłówki (Ta lista nie jest wyczerpująca):

Nagłówek | Opis
-------|------------
Autoryzacja | Wymagany. Zawartość będzie zależeć od metody uwierzytelniania, które można konfigurować i mogą być oparte na WIA (zintegrowane uwierzytelnianie systemu Windows) lub usług AD FS.
Typ zawartości | Wymagane, jeśli żądanie zawiera treść. Musi być "application/json".
Content-Length. | Wymagane, jeśli żądanie zawiera treść. 
Plik cookie | Plik cookie sesji. Może być wymagane w zależności od metody uwierzytelniania.
<br/>
Odpowiedzi HTTP będzie zawierał następujące nagłówki (Ta lista nie jest wyczerpująca):

Nagłówek | Opis
-------|------------
Typ zawartości | Interfejs API zawsze zwraca "application/json".
Content-Length. | Długość treści żądania, jeśli jest obecny w bajtach.

## <a name="versioning"></a>Przechowywanie wersji 
Bieżąca wersja interfejsu API to 1. Wersja interfejsu API można określić za pomocą parametru zapytania w adresie URL żądania, jak w poniższym przykładzie: `http://localhost:8086/api/pamresources/pamrequests?v=1` Jeśli wersja nie jest określony w żądaniu, żądanie jest wykonywane względem najbardziej nowszych wersji interfejsu API. 

## <a name="security"></a>Zabezpieczenia 
Dostęp do interfejsu API wymaga zintegrowane uwierzytelnianie systemu Windows (IWA). To należy skonfigurować ręcznie w usługach IIS przed rozpoczęciem instalacji programu Microsoft Identity Manager (MIM).

HTTPS (TLS) jest obsługiwana, ale należy skonfigurować ręcznie w usługach IIS. Aby uzyskać informacje, zobacz: **wdrożenie Secure Sockets Layer (SSL) dla portalu programu FIM** w [krok 9: wykonaj FIM 2010 R2 poinstalacyjne zadania] (https://technet.microsoft.com/library/hh322875 (v=ws.10%29.aspx) zainstalowanie programu FIM 2010 R2 Przewodnik po laboratorium testowym. 

Możesz wygenerować certyfikat serwera SSL, uruchamiając następujące polecenie w Visual Studio wierszu polecenia:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
To polecenie tworzy certyfikatu z podpisem własnym, który może służyć do testowania aplikacji sieci web, która używa protokołu SSL na serwerze sieci web, którego adres URL jest test.cwap.com. Identyfikator OID, określany przy użyciu opcji - eku identyfikuje certyfikatu jako certyfikatu serwera SSL. Certyfikat jest przechowywany w magazynie my i jest dostępna na poziomie komputera, więc możesz wyeksportować go z przystawki Certyfikaty w mmc.exe

## <a name="cross-domain-access-cors"></a>Krzyżowe dostępu do domeny (CORS) 
CORS jest obsługiwana, ale należy skonfigurować ręcznie w usługach IIS. Dodaj następujące elementy do wdrożonej interfejsu API plik web.config w celu skonfigurowania interfejsu API w celu zapewnienia obsługi wywołań obejmujące różne domeny: 

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
<br/>

## <a name="error-handling"></a>Obsługa błędów 
Interfejs API zwraca odpowiedzi, które wskazują na warunki błędu HTTP. Błędy są zgodne OData. W poniższej tabeli przedstawiono kody błędów, które mogą być zwrócone do klienta.

Kod stanu HTTP | Opis
-----------------|------------
401 | Nieupoważniony 
403 | Dostęp zabroniony 
408 | Limit czasu żądania   
500 | Wewnętrzny błąd serwera 
503 | Usługa jest niedostępna 
<br/>

## <a name="filtering"></a>Filtrowanie 
Żądania interfejsu API REST PAM mogą obejmować filtry, aby określić właściwości, które powinny być uwzględnione w odpowiedzi. Składnia filtru jest oparte na wyrażeniach OData.

Filtry można określić właściwości żądań PAM, ról PAM. lub oczekujących żądań PAM. Na przykład: *ExpirationTime*, *DisplayName*, lub inne prawidłowej właściwości żądania PAM, roli PAM lub oczekującego żądania.

Interfejs API obsługuje następujące operatory w wyrażeniach filtru: *i*, *równy*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*, i *LessThanOrEqual*. 

Następujące przykładowe żądania Uwzględnij filtry:

- To żądanie zwraca wszystkie żądania PAM między określonymi datami:`http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- To żądanie Zwraca rolę Pam o nazwie wyświetlanej "Dostęp do plików SQL":`http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
