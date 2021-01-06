---
title: Historia wersji programu Identity Manager pakietu BHOLD | Microsoft Docs
description: W tym artykule opisano różne zmiany wprowadzone w ramach aktualizacji pakietu BHOLD w programie MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/1/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4e3c247370c6c78e7814d894e9f0a6166d2e397a
ms.sourcegitcommit: 8f81767ec92e1b80658aaebb9463aa4d62396d43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2021
ms.locfileid: "97927612"
---
# <a name="bhold-modules-version-release-history"></a>Historia wersji modułów pakietu BHOLD

Zespół Microsoft Identity Manager regularnie zwalnia aktualizacje wymienione w historii [wersji](version-history.md) . Ten artykuł pomaga śledzić wersje, które zostały wydane dla pakietu BHOLD, podskładnikowi Microsoft Identity Manager. Następnie możesz zdecydować, czy należy zaktualizować do najnowszej wersji.

## <a name="version-60620"></a>6.0.62.0 wersja

- Stan: październik 2018
- [Pobieranie](https://www.microsoft.com/download/details.aspx?id=55950)

## <a name="version-60360"></a>6.0.36.0 wersja

- Stan: 7 września 2017

### <a name="enhancements"></a>Ulepszenia  
PAKIETU BHOLD wersja 6.0.36.0 obejmuje następujące udoskonalenia:

- Aktualizacja platformy x86 do wersji x64.

### <a name="fixed-issues"></a>Naprawione problemy
Następujące problemy zostały rozwiązane w programie pakietu BHOLD w wersji 6.0.36.0.

#### <a name="bhold-core"></a>PAKIETU BHOLD rdzeń

- Instalator zaktualizował procesor x86 do wersji x64.
- Interfejs sieci Web nie pokazuje wszystkich uprawnień.
- Poprawki biblioteki B1Common.
- Użytkownik nie może przypisać roli do OrgUnit, gdy rola ma kilka uprawnień z kardynalnością.
- Kardynalność uprawnień nie działa w przypadku wartości maksymalnej. numery użytkowników.

#### <a name="access-management-connector"></a>Łącznik zarządzania dostępem

- n/d

#### <a name="bhold-integration-module"></a>Moduł integracji pakietu BHOLD

- Instalator zaktualizował procesor x86 do wersji x64.
- Nieprawidłowa lokalizacja folderu układu.

#### <a name="bhold-model-generator"></a>Generator modelu pakietu BHOLD

- Role modelu z wydziałów nadrzędnych nie są dziedziczone.

#### <a name="bhold-analytics"></a>Analiza pakietu BHOLD

- n/d

#### <a name="bhold-attestation"></a>Zaświadczanie pakietu BHOLD

- n/d

#### <a name="bhold-reporting"></a>Raportowanie pakietu BHOLD

- Atrybut niestandardowy "IDPracownika" jest niedostępny.
- Nie można prawidłowo załadować dużych ilości danych przy użyciu zestawu plików 3.

## <a name="next-steps"></a>Następne kroki

- [Przewodnik po pojęciach pakietu BHOLD](../bhold/bhold-concepts-guide.md)
- [Przewodnik instalacji pakietu BHOLD](../bhold/bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](mim2016-bhold-developer-reference.md)
- [Historia wersji programu MIM](version-history.md)

