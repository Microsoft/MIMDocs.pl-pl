---
title: "Rejestrowanie dynamiczne usługi MIM | Microsoft Docs"
description: "Włączanie rejestrowania dynamicznego usługi MIM bez konieczności ponownego uruchamiania usługi zarządzania"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.translationtype: Human Translation
ms.sourcegitcommit: 1ff73d0bdfcbcb4ab79d0d81feca9abdc33f9213
ms.openlocfilehash: 1e2fb9a9ae508ab601ebad1dec7acc21dc44d13e
ms.contentlocale: pl-pl
ms.lasthandoff: 07/10/2017



---
# Rejestrowanie dynamiczne usługi MIM z dodatkiem SP1 (4.4.1436.0)
<a id="mim-sp1-4414360--service-dynamic-logging" class="xliff"></a>
W wersji 4.4.1436.0 wprowadziliśmy nową możliwość rejestrowania. Dzięki niej administrator i inżynierowie pomocy technicznej mogą włączyć rejestrowanie bez konieczności ponownego uruchamiania usługi zarządzania.

Po zainstalowaniu w pliku Microsoft.ResourceManagement.Service.exe.config zobaczysz nowy wiersz o nazwie:

*    Wiersz 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*    Wiersz 8: ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*    Wiersz 266: ``</system.diagnostics> ``

![Wyróżnione sekcje zawierające nowe zapisy rejestrowania dynamicznego](media/mim-service-dynamic-logging/screen01.png)

Poziomy rejestrowania dynamicznego można znaleźć [tutaj](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Krytyczne = w przypadku domyślnego poziomu usługi są zapisywane tylko zdarzenia krytyczne
- Zaktualizuj wiersz 8 (dynamicLogging mode="true" loggingLevel="Critical") przy użyciu preferowanej wartości rejestrowania

Plik konfiguracji rejestrowania dynamicznego znajduje się w wierszu 266: Microsoft.ResourceManagement.Service.exe.config

![Wyróżnione sekcje zawierają wiersze z różnymi dostępnymi obszarami rejestrowania](media/mim-service-dynamic-logging/screen02.png)

Domyślnie lokalizacją rejestrowania będzie folder **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**. Konto usługi FIM będzie mieć uprawnienia zapisu w tej lokalizacji umożliwiające generowanie dziennika dynamicznego.

![Lokalizacja folderu dzienników](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 W przypadku nieoczekiwanych błędów (błędy składni w pliku konfiguracji Microsoft.ResourceManagement.Service.exe.config lub inne pomyłki) odpowiedni komunikat o błędzie zostanie zapisany w pliku Microsoft.ResourceManagement.Service.exe_Emergency.log z następującą ścieżką %TMP%, %TEMP% lub %USERPROFILE% (pierwszą istniejącą).  
1. „%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
2. „%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
3. „% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log”

Aby wyświetlić ślad, możesz użyć [narzędzia do przeglądania danych śledzenia usług](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Zrzut ekranu narzędzia do przeglądania danych śledzenia usług](media/mim-service-dynamic-logging/screen04.png)

