---
title: Szczegóły usługi interfejsu API REST zarządzania certyfikatami | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a2dd84e121217772a8831653b2e4790436c32ec
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492450"
---
# <a name="certificate-management-rest-api-service-details"></a>Szczegóły usługi interfejsu API REST zarządzania certyfikatami
W poniższych sekcjach opisano szczegółowe informacje o interfejsie API REST zarządzania certyfikatami w programie Microsoft Identity Manager (MIM).

## <a name="architecture"></a>Architektura 
Wywołania interfejsu API REST zarządzanie certyfikatami w usłudze MIM są obsługiwane przez kontrolery. W poniższej tabeli przedstawiono pełną listę kontrolerów i przykładów kontekstu, w którym mogą być używane.


|                  Kontroler                   |                                                                                                                                                           Przykładowa trasa                                                                                                                                                           |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           CertificateDataController           |                                                                                                                                         /api/v1.0/requests/{requestid}/certificatedata/                                                                                                                                          |
| CertificateRequestGenerationOptionsController |                                                                                                                                                  /api/v1.0/requests/{requestid}                                                                                                                                                  |
|            CertificatesController             |                                                                                                                /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates                                                                                                                 |
|             OperationsController              |                                                                                                                  /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations                                                                                                                   |
|              PoliciesController               |                                                                                                                                   /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                                                   |
|              ProfilesController               |                                                                                                                                                     /api/v1.0/profiles/{id}                                                                                                                                                      |
|          ProfileTemplatesController           |                                                                                               /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                |
|              RequestsController               |                                                                                                                                         /api/v1.0/requests/{id} <br/> /api/v1.0/requests                                                                                                                                         |
|             SmartcardsController              | /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards |
|       SmartcardsConfigurationController       |                                                                                                                             /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards                                                                                                                              |

## <a name="http-request-and-response-headers"></a>Nagłówki żądań i odpowiedzi HTTP
Żądania HTTP wysyłane do interfejsu API powinny zawierać następujące nagłówki (Ta lista nie jest wyczerpująca):

Nagłówek | Opis
-------|------------
Autoryzacja | Wymagane. Zawartość zależy od metody uwierzytelniania. Metoda jest konfigurowalna i może opierać się na zintegrowanym uwierzytelnianiu systemu Windows (WIA) lub Active Directory Federation Services (AD FS).
Content-Type | Wymagane, jeśli żądanie ma treść. Musi być `application/json` .
Długość zawartości | Wymagane, jeśli żądanie ma treść. 
Plików | Plik cookie sesji. Może być wymagana w zależności od metody uwierzytelniania.


Odpowiedzi HTTP zawierają następujące nagłówki (Ta lista nie jest wyczerpująca):

Nagłówek | Opis
-------|------------
Content-Type | Interfejs API zawsze zwraca wartość `application/json` .
Długość zawartości | Długość treści żądania (jeśli jest obecna) w bajtach.


## <a name="api-versioning"></a>Obsługa wersji interfejsu API 
Bieżąca wersja interfejsu API REST usługi CM to 1,0. Wersja jest określona w segmencie bezpośrednio po `/api` segmencie w identyfikatorze URI: `/api/v1.0` . Numer wersji zmienia się w przypadku dokonania poważnych modyfikacji interfejsu API.


## <a name="enable-the-api"></a>Włącz interfejs API 
`<ClmConfiguration>`Sekcja pliku Web.config została rozszerzona o nowy klucz:

```
<add key="Clm.WebApi.Enabled" value="false" />
```

Ten klucz określa, czy interfejs API REST programu CM jest narażony na klientów. Jeśli klucz jest ustawiony na wartość "false", mapowanie trasy dla interfejsu API nie jest wykonywane podczas uruchamiania aplikacji. Kolejne żądania do punktów końcowych interfejsu API zwracają kod błędu HTTP 404. Domyślnie klucz jest ustawiony na wartość "wyłączone".

>[!NOTE]
>Po zmianie tej wartości na wartość "true" Pamiętaj, aby uruchomić **polecenie iisreset** na serwerze.

## <a name="enable-tracing-and-logging"></a>Włącz śledzenie i rejestrowanie 
Interfejs API REST zarządzanie certyfikatami w usłudze MIM emituje dane śledzenia dla każdego wysłanego żądania HTTP. Można ustawić poziom szczegółowości informacji śledzenia emitowanych przez ustawienie następującej wartości konfiguracji:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```

## <a name="error-handling-and-troubleshooting"></a>Obsługa błędów i rozwiązywanie problemów 
Gdy podczas przetwarzania żądania występują wyjątki, interfejs API REST zarządzanie certyfikatami w usłudze MIM zwraca kod stanu HTTP do klienta sieci Web. W przypadku typowych błędów interfejs API zwraca odpowiedni kod stanu HTTP i kod błędu. 

Nieobsłużone wyjątki są konwertowane na `HttpResponseException` kod stanu HTTP 500 ("błąd wewnętrzny") i śledzony w dzienniku zdarzeń i pliku śledzenia zarządzanie certyfikatami w usłudze MIM. Każdy nieobsługiwany wyjątek jest zapisywana w dzienniku zdarzeń przy użyciu odpowiedniego identyfikatora korelacji. Identyfikator korelacji jest również wysyłany do konsumenta interfejsu API w komunikacie o błędzie. Aby rozwiązać ten problem, administrator może przeszukać w dzienniku zdarzeń odpowiedni identyfikator korelacji i szczegóły błędu.

>[!NOTE]
>Ślady stosu, które odnoszą się do błędów, które są generowane w wyniku używania interfejsu API REST zarządzanie certyfikatami w usłudze MIM, nie są odsyłane do klienta. Ślady nie są odsyłane do klienta z powodu problemów związanych z bezpieczeństwem.
