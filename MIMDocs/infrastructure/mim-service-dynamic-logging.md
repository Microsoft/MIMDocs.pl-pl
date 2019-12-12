---
title: Rejestrowanie dynamiczne usługi MIM | Microsoft Docs
description: Włączanie rejestrowania dynamicznego usługi MIM bez konieczności ponownego uruchamiania usługi zarządzania
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 90ef2ab63be3914d1d48c7319821177e7e62f9e0
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "68701295"
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

Konfiguracja rejestrowania dynamicznego znajdująca się w wierszu 266: Microsoft. ResourceManagement. Service. exe. config

![Wyróżnione sekcje zawierają wiersze z różnymi dostępnymi obszarami rejestrowania](media/mim-service-dynamic-logging/screen02.png)

Domyślnie lokalizacja rejestrowania będzie znajdować się w lokalizacji * * C:\Program Files\Microsoft Forefront Identity Manager\2010\Service. konto usługi FIM będzie wymagało uprawnienia do zapisu w tej lokalizacji w celu wygenerowania dziennika dynamicznego.

![Lokalizacja folderu dzienników](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  W przypadku nieoczekiwanych błędów (błędy składni w pliku konfiguracji Microsoft.ResourceManagement.Service.exe.config lub inne pomyłki) odpowiedni komunikat o błędzie zostanie zapisany w pliku Microsoft.ResourceManagement.Service.exe_Emergency.log z następującą ścieżką %TMP%, %TEMP% lub %USERPROFILE% (pierwszą istniejącą).  
> 1. „%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
> 2. „%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log”
> 3. „% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log”

Aby wyświetlić ślad, można użyć [narzędzia Podgląd śledzenia usług](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Zrzut ekranu narzędzia do przeglądania danych śledzenia usług](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Aktualizacje: kompilacja 4.5. x. x lub większa

W kompilacji 4.5. x. x Zaktualizowaliśmy funkcję rejestrowania, aby określić domyślny poziom rejestrowania to **"Warning" (ostrzeżenie**). Usługa zapisuje komunikaty w dwóch plikach (indeksy "00" i "01" są dodawane przed rozszerzeniem). Pliki znajdują się w katalogu "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service". Gdy plik przekracza maksymalny rozmiar, Usługa uruchamia zapis w innym pliku. Jeśli istnieje inny plik, zostanie on nadpisany. Domyślny maksymalny rozmiar pliku to 1 GB. Aby zmienić domyślny maksymalny rozmiar, należy dodać parametr **"maxOutputFileSizeKB"** z wartością maksymalnego rozmiaru pliku w kilobajtach do odbiornika (Zobacz przykład poniżej) i ponownie uruchomić usługę programu MIM. Gdy usługa zostanie uruchomiona, dołącza dzienniki w nowszej pliku (w przypadku przekroczenia limitu miejsca zastępuje najstarszy plik). 

> [!NOTE] 
> Gdy rozmiar pliku sprawdzania usługi zostanie zapisany przed zapisaniem wiadomości, rozmiar pliku może być większy niż maksymalny rozmiar jednego komunikatu. Domyślnie rozmiar dzienników może wynosić około 6 GB (trzy > detektory z dwoma plikami dla jednego rozmiaru GB).

> [!NOTE] 
> Konto usługi powinno mieć uprawnienia do zapisu w > katalogu "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" >. W przypadku, gdy konto usługi nie ma takich uprawnień, pliki > nie zostaną utworzone.

Przykład ustawiania maksymalnego rozmiaru pliku na 200 MB (200 * 1024 KB) dla plików svclog i 100 MB * (100 * 1024 KB) dla plików txt

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
