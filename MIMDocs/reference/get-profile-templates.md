---
title: Pobierz profil szablony | Dokumentacja firmy Microsoft
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Pobierz profil szablony
Pobiera listę szablonami profilów, które można zarejestrować określonego użytkownika. Ta metoda zwraca ograniczony widok szablonu profilu. Zwrócono dane szablonu profilu powinny być wystarczające, aby umożliwić użytkownika żądającego podjęcie które szablonu profilu, jeśli istnieją, należy zarejestrować. Jeśli podano nr przepływu pracy i uprawnienia zostaną zwrócone wszystkie szablony profilu widoczny dla użytkownika.

**Uwaga**: adresy URL wyświetlany w tym temacie są względem nazwy hosta podczas wdrażania interfejsu API; na przykład: `https://api.contoso.com`.
##<a name="request"></a>Żądanie


Metoda  |Adres URL żądania  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates? \[użytkownik docelowy\] 

###<a name="url-parameters"></a>Parametry adresu URL
Parametr| Opis
--------|-------------
Użytkownik docelowy| Opcjonalny. Określa docelowy użytkownika o powrót szablony profilów dla. Jeśli nie zostanie określony, będzie używany tożsamości bieżącego użytkownika. <br/>**Uwaga**: aktualnie obsługiwana jest tylko bieżącego użytkownika.

###<a name="request-headers"></a>Nagłówki żądania
Zobacz temat usługi dla typowych nagłówki żądania
###<a name="request-body"></a>Treść żądania
brak

##<a name="response"></a>Odpowiedź
###<a name="response-codes"></a>Kody odpowiedzi
Kod  |Opis  
---------|---------
200     | OK
204 | Brak zawartości
500 | Błąd wewnętrzny

###<a name="response-headers"></a>Nagłówki odpowiedzi
W temacie service wspólnych nagłówków odpowiedzi.
###<a name="response-body"></a>Treść odpowiedzi
W przypadku powodzenia zwraca listę obiektów ProfileTemplateLimitedView z następującymi właściwościami.

Właściwość| Typ| Opis
--------|-----|--------
Nazwa| ciąg| Nazwa wyświetlana szablonu profilu.
Opis| ciąg| Opis szablonu profilu.
Identyfikator UUID| Identyfikator GUID| Identyfikator szablonu profilu.
IsSmartcardProfileTemplate| wartość logiczna| Wskazuje, czy szablon jest szablonu profilu karty inteligentnej.
IsVirtualSmartcardProfileTemplate| wartość logiczna| Wskazuje, czy szablon profilu wymaga wirtualnej karty inteligentnej.

##<a name="example"></a>Przykład

###<a name="request"></a>Żądanie
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Odpowiedź
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
