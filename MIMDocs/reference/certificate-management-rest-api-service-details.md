---
title: "Szczegóły usługi interfejsu API REST zarządzania certyfikatami w usłudze | Dokumentacja firmy Microsoft"
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
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>Szczegóły usługi interfejsu API REST zarządzania Certyfikatami
W poniższych sekcjach omówiono szczegóły interfejsu API REST zarządzania certyfikatu CM Microsoft Identity Manager (MIM).

## <a name="architecture"></a>Architektura 
Wywołania interfejsu API REST zarządzania Certyfikatami programu MIM są obsługiwane przez administratorów. W poniższej tabeli przedstawiono pełną listę kontrolerów i przykłady kontekst, w którym mogą być używane.

Kontrolera| Przykładowe trasy
----------|-------------
CertificateDataController| /API/V1.0/Requests/{RequestId}/certificatedata /
CertificateRequestGenerationOptionsController| /API/V1.0/Requests/{RequestId}/certificaterequestgenerationoptions
CertificatesController| /API/V1.0/SmartCards/{smartcardid}/Certificates <br/> /API/V1.0/Profiles/{ProfileId}/Certificates
OperationsController| /API/V1.0/SmartCards/{smartcardid}/Operations <br/> /API/V1.0/Profiles/{ProfileId}/Operations
PoliciesController| / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| / api/v1.0/profiles/{id} <br/> /API/V1.0/Profiles <br/> / api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| / api/v1.0/profiletemplates/{id} <br/> /API/V1.0/profiletemplates <br/> / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| / api/v1.0/requests/{id} <br/> /API/V1.0/Requests
SmartcardsController| /API/V1.0/Requests/{RequestId}/SmartCards/{ID}/diversifiedkey <br/> /API/V1.0/Requests/{RequestId}/SmartCards/{ID}/serverproposedpin <br/> /API/V1.0/Requests/{RequestId}/SmartCards/{ID}/authenticationresponse <br/> / api/v1.0/requests/{requestid}/smartcards/{id} <br/> / api/v1.0/smartcards/{id} <br/> /API/V1.0/SmartCards
SmartcardsConfigurationController| /API/V1.0/profiletemplates/{profiletemplateid}/Configuration/SmartCards
<br/>

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


## <a name="api-versioning"></a>Przechowywanie wersji interfejsu API 
Bieżąca wersja interfejsu API REST zarządzania Certyfikatami jest 1.0. Wersja jest określony w segmencie bezpośrednio po `/api` segmentów w identyfikatorze URI: `/api/v1.0`. W przyszłości zmieni numer wersji powinno być istotne zmiany do interfejsu API.


## <a name="enabling-the-api"></a>Włączanie interfejsu API 
`<ClmConfiguration>` Sekcji w pliku Web.config został rozszerzony za pomocą nowego klucza:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Ten klucz określa, czy CM interfejsu API REST jest widoczne dla klientów. Jeśli klucz jest ustawiona na wartość "false", mapowanie trasy dla interfejsu API nie jest wykonywane podczas uruchamiania aplikacji. Oznacza to, że wszystkie kolejne żądania dotyczące punkty końcowe interfejsu API zwróci błąd o kodzie 404 protokołu HTTP. Klucz jest ustawiony na "wyłączone" domyślnie.
Po zmianie tej wartości na wartość "true", pamiętaj, aby uruchomić **iisreset** na serwerze.

## <a name="enabling-tracing-and-logging"></a>Włączanie śledzenia i rejestrowania 
Interfejs API REST zarządzania certyfikatami w usłudze MIM emituje danych śledzenia dla każdego żądania HTTP wysyłane do niego. Można ustawić poziom szczegółowości informacji śledzenia emitowane przez ustawienie wartości następujących konfiguracji:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>Obsługa błędów i rozwiązywanie problemów 
Wystąpieniu wyjątku podczas przetwarzania żądania interfejsu API REST zarządzania certyfikatami w usłudze MIM zwraca kod stanu HTTP do klienta sieci web. Typowe błędy interfejsu API zwraca odpowiedni kod stanu HTTP i kod błędu. 

Nieobsługiwane wyjątki są konwertowane na `HttpResponseException` ze stanem HTTP Code 500 ("wewnętrzny błąd") i śledzić w dzienniku zdarzeń i zarządzania Certyfikatami programu MIM plików śledzenia. Każdy nieobsługiwany wyjątek są zapisywane w dzienniku zdarzeń przy użyciu odpowiedniego identyfikatora korelacji. Identyfikator korelacji jest również przesyłany do konsumenta interfejsu API w komunikacie o błędzie. Aby rozwiązać problem, administrator można wyszukiwać odpowiednie szczegóły Identyfikatora i błąd korelacji można znaleźć w dzienniku zdarzeń.

Ze względów bezpieczeństwa śladów stosu odpowiadający błędy wygenerowanego w wyniku korzystanie z interfejsu API REST zarządzania Certyfikatami programu MIM nie są wysyłane do klienta.
