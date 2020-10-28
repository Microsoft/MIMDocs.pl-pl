---
title: Program MIM instaluje narzędzie do współużytkowania usługi sieci Web | Microsoft Docs
description: W tym artykule opisano procedurę instalowania narzędzia konfiguracji usługi sieci Web.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4b9d2463ea30839c2ea4e2a3427d057c925183e8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760535"
---
# <a name="install-the-web-service-configuration-tool"></a>Instalowanie narzędzia konfiguracji usługi sieci Web

Łącznik usługi sieci Web i projekty domyślne są dostępne w [Centrum pobierania Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

Plik **MSI łącznika usługi sieci Web** udostępnia dwie funkcje:

- Środowisko uruchomieniowe łącznika usługi sieci Web: instaluje łącznik podstawowy, zależności łącznika i pakiet spakowanego łącznika.
- Narzędzie konfiguracji usługi sieci Web: instaluje narzędzie konfiguracji usługi sieci Web.

![Opcje łącznika Kreatora instalacji](media/microsoft-identity-manager-2016-ma-ws-install/connector-installation-options.png)

Narzędzie konfiguracji można zainstalować bez zainstalowanej usługi synchronizacji. Pozwala to na konfigurację na osobnym komputerze.

## <a name="default-projects"></a>Projekty domyślne

Dodatkowe projekty domyślne są dostarczane z łącznikiem usług sieci Web. Są one dostępne jako pliki WYKONYWALNe samowyodrębniający się. Możesz pobrać projekt łącznika usługi sieci Web zgodnie z potrzebami.

Po zakończeniu instalacji, różne składniki zawierające pliki binarne są instalowane w następującej lokalizacji folderu w systemie.

| Zawartość | Lokalizacja |
|---|---|
| Środowisko uruchomieniowe łącznika usługi sieci Web           | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010 \\ — &nbsp; rozszerzenia usługi synchronizacji \\ |
| Projekt łącznika usługi sieci Web           | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010 \\ — &nbsp; rozszerzenia usługi synchronizacji \\ |
| Spakowany łącznik                      | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010 \\ Synchronization &nbsp; Service \\ UIShell \\ dane XML \\ PackagedMAs |
| Narzędzie konfiguracji usługi sieci Web          | % Program Files% \\ Microsoft Forefront Identity Management \\ 2010 \\ Synchronizacja &nbsp; Service \\ UIShell \\ &nbsp; Konfiguracja usługi sieci Web &nbsp; <br/>**Uwaga** : jest to domyślna lokalizacja instalacji. Tę lokalizację można zmienić podczas instalacji. |
| Plik projektu usługi sieci Web                | Użytkownik może wybrać dowolny folder docelowy, do którego zostanie wyodrębniony ten plik, ale wyodrębniony plik projektu (. WsConfig) jest widoczny dla interfejsu użytkownika synchronizacji FIM tylko wtedy, gdy plik projektu jest wyodrębniany do folderu **rozszerzeń** programu FIM. Wyodrębniony plik projektu jest widoczny dla narzędzia konfiguracji usługi sieci Web w dowolnej lokalizacji. |


## <a name="additional-permissions"></a>Dodatkowe uprawnienia

Plik projektu można zapisać i otworzyć z dowolnej lokalizacji (z odpowiednimi uprawnieniami dostępu do jego wykonawcy); jednak tylko pliki projektu, które są zapisywane w `Synchronization Service\Extension` folderze, można wybrać w Kreatorze łącznika usługi sieci Web dostępnym za pomocą interfejsu użytkownika synchronizacji FIM.

Użytkownik uruchamiający narzędzie konfiguracji usługi sieci Web musi mieć następujące uprawnienia:

- Uprawnienia do odczytu/zapisu do folderu rozszerzenia usługi synchronizacji.
- Dostęp do odczytu do klucza rejestru **HKLM \\ system \\ CurrentControlSet \\ Services \\ FIMSynchronizationService \\ Parameters** .
