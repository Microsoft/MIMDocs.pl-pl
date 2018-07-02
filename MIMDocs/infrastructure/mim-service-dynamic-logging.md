---
title: Rejestrowanie dynamiczne usługi MIM | Microsoft Docs
description: Włączanie rejestrowania dynamicznego usługi MIM bez konieczności ponownego uruchamiania usługi zarządzania
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: 35d210b06a1e58b3b8f4f08677c2a4151f540246
ms.sourcegitcommit: 88d4e41d8d57f44f4c6c4468fdbd37c2d7e91fd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36957543"
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Rejestrowanie dynamiczne usługi MIM z dodatkiem SP1 (4.4.1436.0)
W wersji 4.4.1436.0 wprowadziliśmy nową możliwość rejestrowania. Dzięki niej administrator i inżynierowie pomocy technicznej mogą włączyć rejestrowanie bez konieczności ponownego uruchamiania usługi zarządzania.

Po zainstalowaniu w pliku Microsoft.ResourceManagement.Service.exe.config zobaczysz nowy wiersz o nazwie:

*   Wiersz 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Wiersz 8: ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Wiersz 266: ``</system.diagnostics> ``

![Wyróżnione sekcje zawierające nowe zapisy rejestrowania dynamicznego](media/mim-service-dynamic-logging/screen01.png)

Poziomy rejestrowania dynamicznego można znaleźć [tutaj](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Krytyczne = w przypadku domyślnego poziomu usługi są zapisywane tylko zdarzenia krytyczne
- Zaktualizuj wiersz 8 (dynamicLogging mode="true" loggingLevel="Critical") przy użyciu preferowanej wartości rejestrowania

Rejestrowanie dynamiczne konfiguracji znajdujących się w wierszu 266: Microsoft.ResourceManagement.Service.exe.config

![Wyróżnione sekcje zawierają wiersze z różnymi dostępnymi obszarami rejestrowania](media/mim-service-dynamic-logging/screen02.png)

Domyślna lokalizacja rejestrowania będą w ** C:\Program Files\Microsoft Forefront Identity Manager\2010\Service usługi FIM konta należy uprawnienie zapisu do tej lokalizacji, aby wygenerować dziennik dynamicznych.

![Lokalizacja folderu dzienników](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  W przypadku nieoczekiwanych błędów (błędy składni w pliku konfiguracji Microsoft.ResourceManagement.Service.exe.config lub inne pomyłki) odpowiedni komunikat o błędzie zostanie zapisany w pliku Microsoft.ResourceManagement.Service.exe_Emergency.log z następującą ścieżką %TMP%, %TEMP% lub %USERPROFILE% (pierwszą istniejącą).  
> 1. „%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
> 2. „%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
> 3. „% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log”

Aby wyświetlić dane śledzenia, można użyć [narzędzia podglądu śledzenia usługi](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Zrzut ekranu narzędzia do przeglądania danych śledzenia usług](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Aktualizacje: Tworzenie 4.5.x.x lub nowszej

W kompilacji jest 4.5.x.x poprawiona funkcji rejestrowania, aby określić domyślny poziom rejestrowania **"Ostrzeżenie"**. Zapisuje komunikaty w dwóch plikach ("00" i "01" indeksów są dodawane przed rozszerzeniem). Pliki znajdują się w katalogu "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service". Gdy rozmiar pliku przekracza maksymalny rozmiar usługa zostanie uruchomiona zapisu w innym pliku. Istnieje już inny plik, zostaną zastąpione. Domyślny maksymalny rozmiar pliku wynosi 1 GB. Aby zmienić domyślny maksymalny rozmiar, należy dodać **"maxOutputFileSizeKB"** parametru z wartością Maksymalny rozmiar w KB do odbiornika (zobacz poniższy przykład) i uruchom ponownie usługę MIM. Gdy usługa jest uruchomiona, dołącza go dzienniki w pliku nowszą (po przekroczeniu limitu miejsca go zastąpić plik najstarsze). 

> [!NOTE] Jako rozmiar pliku wyboru usługi przed zapisaniem komunikat rozmiar pliku może być większa niż maksymalny rozmiar dla rozmiaru jeden komunikat. Domyślnie dzienniki może mieć około 6 GB (trzy > odbiorników z dwóch plików dla rozmiaru jednego GB).

> [!NOTE] Konto usługi musi mieć uprawnienia do zapisu > "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" > katalogu. W przypadku, gdy konto usługi nie ma takich uprawnień > nie można utworzyć plików.

Przykład sposobu ustawiania do 200 MB (200 * 1024 KB) dla plików svclog i 100 MB maks * (100 * 1024 KB) dla plików txt

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
