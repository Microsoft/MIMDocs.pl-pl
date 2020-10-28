---
title: Przegląd ogólnego łącznika usługi sieci Web | Microsoft Docs
description: Przegląd konfiguracji i wymagań dla ogólnego łącznika usługi sieci Web.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: b0d4cc9ac9b9ac080038d632df0c02c4c244e3d6
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760804"
---
# <a name="overview-of-the-generic-web-service-connector"></a>Przegląd ogólnego łącznika usługi sieci Web

Łącznik usługi sieci Web integruje tożsamości za pomocą operacji usługi sieci Web z Microsoft Identity Manager (MIM) 2016 z dodatkiem SP1. Łącznik wymaga pliku projektu usługi sieci Web, aby nawiązać połączenie z właściwym źródłem danych. Ten projekt można pobrać z [Centrum pobierania firmy Microsoft](https://go.microsoft.com/fwlink/?LinkID=235883) wraz z [dokumentacją](https://www.microsoft.com/en-us/download/details.aspx?id=29943) dotyczącą używania łącznika z oprogramowaniem Oracle eBusiness, Oracle PeopleSoft i SAP. Można go również utworzyć za pomocą narzędzia konfiguracji usługi sieci Web.

Gdy usługa synchronizacji programu MIM wywoła łącznik usługi sieci Web, ładuje swój skonfigurowany plik projektu (plik **WsConfig** ). Ten plik pomaga rozpoznać punkt końcowy źródła danych, który powinien zostać użyty do nawiązania połączenia. Plik informuje również, że przepływ pracy jest wykonywany w celu zaimplementowania operacji programu MIM. Aby można było wykonać skonfigurowane przepływy pracy, łącznik usługi sieci Web wykorzystuje aparat czasu wykonywania programu .NET 4.

![Przepływ pracy](media/microsoft-identity-manager-2016-ma-ws/workflow.png)

## <a name="web-service-layers"></a>Warstwy usługi sieci Web

Dwie główne warstwy są używane do implementowania rozwiązania agenta zarządzania usługami sieci Web (MA): 

- Narzędzie konfiguracji usługi sieci Web
- Łącznik czasu wykonywania zaimplementowany z programem Workflow .NET 4,0

## <a name="supported-data-sources-for-web-service-discovery"></a>Obsługiwane źródła danych na potrzeby odnajdywania usług sieci Web

Narzędzie konfiguracji usługi sieci Web implementuje następujące funkcje:

- Odnajdywanie protokołu SOAP: umożliwia administratorowi wprowadzanie ścieżki WSDL uwidocznionej przez docelową usługę sieci Web. Funkcja odnajdywania utworzy strukturę drzewa hostowanych usług sieci Web z ich wewnętrznymi punktami końcowymi/Operations wraz z opisem metadanych operacji. Nie ma limitu liczby operacji odnajdywania, które można wykonać (krok po kroku). Odnalezione operacje są używane później w celu skonfigurowania przepływu operacji, które implementują operacje łącznika względem źródła danych (Import/Export/hasło).

- Odnajdywanie REST: umożliwia administratorowi wprowadzanie szczegółów usługi RESTful, np. punktu końcowego usługi, ścieżki zasobu, metody i parametru. Użytkownik może dodać nieograniczoną liczbę usług RESTful. Informacje o usługach REST będą przechowywane w ```discovery.xml``` pliku ```wsconfig``` projektu. Zostaną one użyte później przez użytkownika w celu skonfigurowania działania usługi sieci Web REST w przepływie pracy.

- Konfiguracja schematu przestrzeni łącznika: umożliwia administratorowi skonfigurowanie schematu przestrzeni łącznika. Konfiguracja schematu będzie zawierać listę typów obiektów i atrybutów dla określonej implementacji. Administrator może określić typy obiektów, które będą obsługiwane przez usługę sieci Web MA. Administrator może również wybrać w tym miejscu atrybuty, które będą częścią schematu przestrzeni łącznika.

- Konfiguracja przepływu operacji: interfejs użytkownika projektanta przepływu pracy służący do konfigurowania implementacji operacji FIM (Import/Export/Password) dla każdego typu obiektu za pomocą funkcji dostępnych operacji usługi sieci Web, takich jak:

    - Przypisanie parametrów z obszaru łącznika do funkcji usługi sieci Web.
    - Przypisanie parametrów z funkcji usługi sieci Web do obszaru łącznika.

## <a name="resources-generated-by-the-web-service-configuration-tool"></a>Zasoby wygenerowane przez narzędzie konfiguracji usługi sieci Web

Narzędzie konfiguracji usługi sieci Web generuje niezbędne zasoby potrzebne do skonfigurowania w pełni funkcjonalnej usługi sieci Web, która obejmuje następujące funkcje:

- Schemat przestrzeni łącznika: plik binarny, który zawiera konfigurację schematu. Plik zostanie zaimportowany przez program MIM za pośrednictwem ```Get Schema``` interfejsu, gdy ma skonfigurowany interfejs użytkownika synchronizacji programu FIM. Następnie jest konwertowany na obiekt ECMA2 schematu format.

- Przepływy pracy: Seria definicji przepływu pracy. Są one używane przez usługę sieci Web w czasie wykonywania w celu wykonania odpowiedniej operacji.

- Plik konfiguracji WCF: plik konfiguracyjny, który jest generowany przez operację odnajdywania. Plik zawiera informacje o powiązaniach i punktach końcowych wymagane do wywołania operacji usługi sieci Web względem źródła danych.

- Zestaw kontraktu danych: ponieważ łącznik usługi sieci Web obsługuje teraz zarówno usługi SOAP, jak i REST, te kontrakty danych są inne w pliku generated.dll.

- Zestaw SOAP: podczas analizowania danych wejściowych WSDL narzędzie konfiguracji usługi sieci Web generuje typy kontraktów danych, które są strukturami danych używanymi przez operacje usługi sieci Web do komunikowania się z usługą zdalną. Te typy kontraktów są również używane do ujawniania zdalnych jednostek źródła danych na potrzeby mapowania atrybutów typu obiektu.

- Zestaw REST: podczas analizowania przykładowego żądania-odpowiedzi dla usługi sieci Web REST narzędzie konfiguracji generuje typy (klasy), które będą używane w przepływie pracy do komunikowania się z usługą sieci Web za pośrednictwem działania wywołania usługi sieci Web. Każde żądanie/odpowiedź zostanie zdefiniowane we własnej przestrzeni nazw. Przestrzeń nazw ma składnię jako \< ServiceName \> . \< ResourceName \> . \< MethodName \> . [ Żądanie/odpowiedź]. Zawijanie każdego żądania/odpowiedzi w oddzielnym obszarze nazw pomaga zmniejszyć liczbę problemów spowodowanych zduplikowaną nazwą typu (Klasa).

![Przepływ pracy](media/microsoft-identity-manager-2016-ma-ws/workflow2.png)

### <a name="project-file-type"></a>Typ pliku projektu

Usługa sieci Web ma zapisany plik skompresowany (ZIP format) o nazwie określonej przez użytkownika i rozszerzeniu pliku "WsConfig". Rozszerzenie pliku "WsConfig" jest zarejestrowane i skojarzone z narzędziem konfiguracji usługi sieci Web przez Instalatora. Istniejące i zmodyfikowane projekty można otwierać, modyfikować i zapisywać. Mogą być zapisane w folderze rozszerzeń usługi synchronizacji FIM lub w dowolnej innej lokalizacji. Zmiany związane z typem obiektu i atrybutami wymagają synchronizacji po stronie programu FIM.  Narzędzie konfiguracji jest aplikacją o wielu wystąpieniach, która została zaprojektowana w celu tworzenia i modyfikowania MA.

## <a name="supported-security-modes"></a>Obsługiwane tryby zabezpieczeń

Aplikację usługi sieci Web REST/SOAP można zabezpieczyć za pomocą serwera sieci Web, takiego jak usługi IIS. Aplikacja umożliwia użytkownikowi wybranie trybu zabezpieczeń, jak pokazano na poniższej ilustracji. Tryby zabezpieczeń to: Basic, Digest, Certificate, Windows lub None.

![Tryby zabezpieczeń](media/microsoft-identity-manager-2016-ma-ws/security-mode.png)

## <a name="supported-data-types"></a>Obsługiwane typy danych

Obsługiwane są następujące typy danych:

- SOAP (starsza wersja): typ danych protokołu SOAP jest obsługiwany zgodnie z opisem w tym [artykule w witrynie MSDN](https://msdn.microsoft.com/library/ms995800.aspx). Obsługa jest dostępna tylko dla stosu interfejsu programowania aplikacji biznesowej (BAPI). Przykładowe szablony protokołu SOAP są dostępne w [Centrum pobierania Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).
- REST (nie ODATA): Łącznik/sieć Web oparta na protokole HTTP.

## <a name="next-steps"></a>Następne kroki 

- [Instalowanie narzędzia konfiguracji usługi sieci Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Przewodnik wdrażania protokołu SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Przewodnik wdrażania REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Konfiguracja usługi sieci Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
