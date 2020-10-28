---
title: Przykład App Service interfejsu API REST łącznika usługi sieci Web | Microsoft Docs
description: Przewodnik ułatwiający implementację przykładowego serwera JSON REST na platformie Azure
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/28/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: deb743fcbe4bdd155c1b0c4a31e24af0e8da8a4e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760840"
---
# <a name="web-service-connector-rest-api-app-service-sample"></a>Przykład App Service interfejsu API REST łącznika usługi sieci Web

Ten przewodnik wdrażania ułatwi Ci wdrożenie przykładowego serwera JSON REST na platformie Azure. Możesz użyć tego przykładu, aby pomóc w konfiguracji i zrozumieniu łącznika usługi sieci Web.

- Przykład wymaga Microsoft Visual Studio 2017.
- Pakiety natywnej ścieżki modułu (nnazwa pakietu MP) w przykładzie używają tego [serwera JSON](https://github.com/typicode/JSON-server) w serwisie GitHub.
- Pobierz [przykładowy kod](https://github.com/fimguy/SAMPLEREST) z usługi GitHub i Wdróż przykładowy kod w celu Azure App Service.

## <a name="deploy-the-json-server-sample"></a>Wdróż przykład serwera JSON

1. Po pobraniu kodu Otwórz plik rozwiązania w programie [Visual Studio 2017](https://www.visualstudio.com/downloads/).

2. Wdróż rozwiązanie, wybierając projekt, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję **Publikuj** :

    ![Publikowanie rozwiązania](media/microsoft-identity-manager-2016-ma-ws-restsample/publish-project.png)

3. Wybierz App Service do użycia we wdrożeniu:

    ![Wybierz App Service](media/microsoft-identity-manager-2016-ma-ws-restsample/app-service.png)

4. Wybierz istniejącą grupę zasobów lub Utwórz nową grupę zasobów:

    ![Wybierz grupę zasobów](media/microsoft-identity-manager-2016-ma-ws-restsample/resource-group.png)

5. Użyj istniejących nazw dla App Service i wybierz pozycję **Utwórz** :

    ![Tworzenie usługi App Service](media/microsoft-identity-manager-2016-ma-ws-restsample/create.png)

    App Service jest tworzony.

6. Aby opublikować App Service, wybierz pozycję **Publikuj** :

    ![Publikowanie App Service](media/microsoft-identity-manager-2016-ma-ws-restsample/publish.png)

7. Po opublikowaniu App Service przykład interfejsu API REST i odpowiedniej witrynie sieci Web zostanie uruchomiona w domyślnej przeglądarce:

    ![Przykładowy interfejs API REST i witryna sieci Web](media/microsoft-identity-manager-2016-ma-ws-restsample/sample-rest-api.png)

Teraz można skonfigurować wdrożenie, zgodnie z opisem w [przewodniku wdrażania REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md).


## <a name="modify-the-sample"></a>Modyfikowanie przykładu

Aby zmodyfikować dane JSON i przykład interfejsu API REST, wprowadź zmiany w **db.JSw** pliku, a następnie zaktualizuj wdrożenie:

![Aktualizowanie db.JSw pliku](media/microsoft-identity-manager-2016-ma-ws-restsample/db-json.png)


## <a name="next-steps"></a>Następne kroki

- [Przegląd ogólnego łącznika usługi sieci Web](microsoft-identity-manager-2016-ma-ws.md)
- [Instalowanie narzędzia konfiguracji usługi sieci Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Przewodnik wdrażania protokołu SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Przewodnik wdrażania REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Konfiguracja usługi sieci Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
