---
title: Instalacja usługi pakietu BHOLD Analytics | Microsoft Docs
description: Moduł pakietu BHOLD Analytics zapewnia oparte na regułach testowanie dostępu do danych
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ec7069156aa033b33a139ae83e26abcdea7b482a
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516871"
---
# <a name="bhold-analytics-installation"></a>PAKIETU BHOLD Analytics

Moduł pakietu BHOLD Analytics zapewnia oparte na regułach testowanie dostępu do danych w celu zapewnienia, że organizacja może efektywnie kontrolować dostęp do danych i spełniać wymagania wewnętrzne i zewnętrzne. Analiza automatycznego wpływu generowana przez moduł pakietu BHOLD Analytics zawiera przegląd przedstawiający liczbę użytkowników, którzy mają wpływ na wymuszanie proponowanej reguły, zarówno te, które byłyby zgodne z regułą, jak i te, które naruszają tę regułę. Moduł pakietu BHOLD Analytics może również udostępnić szczegółową listę użytkowników, którzy byłyby zgodni z regułą i tych, które mogłyby naruszać regułę.

## <a name="bhold-analytics-installation-requirements"></a>Wymagania dotyczące instalacji pakietu BHOLD Analytics

Przed zainstalowaniem modułu pakietu BHOLD Analytics należy zainstalować moduł pakietu BHOLD Core na serwerze, na którym planujesz zainstalować moduł pakietu BHOLD Analytics. Aby uzyskać informacje na temat instalowania modułu pakietu BHOLD Core, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem instalacji modułu pakietu BHOLD Analytics należy przygotować się do podania informacji wymaganych przez Kreatora instalacji usługi pakietu BHOLD Analytics do ukończenia instalacji. Poniższy arkusz pomoże Ci zarejestrować te informacje, dzięki czemu będzie można je w razie potrzeby udostępnić.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Używanie dostawcy zabezpieczeń na komputerze/domenie** | Po wybraniu określa, że zabezpieczenia Active Directory Domain Services będą kontrolować dostęp do pakietu BHOLD rdzeń.                                                                                                                | zaznacz pole wyboru. **Ważne:** Instalacja nie powiedzie się, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konto usługi, które zostało utworzone podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest ona niepoprawna. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (krótkiej), a nie w pełni kwalifikowanej nazwy domeny (FQDN). Na przykład, jeśli nazwa FQDN domeny to fabrikam.com, określ nazwę domeny jako FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika usługi pakietu BHOLD Core.                                                                                                                                                          | W tym miejscu wpisz nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Napisz hasło tutaj: **Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>PAKIETU BHOLD Analytics

Aby zainstalować moduł pakietu BHOLD Analytics, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, na którym zamierzasz zainstalować moduł analizy pakietu BHOLD w:

- BholdAnalytics<em>\<wersja\></em>\_Release. msi

Zastąp *\<version\>wersją* wersji pakietu BHOLD Analytics, którą instalujesz.

Aby uruchomić plik programu jako administrator, kliknij plik prawym przyciskiem myszy, a następnie kliknij polecenie **Uruchom jako administrator**.

# <a name="next-steps"></a>Następne kroki

- [Instalacja pakietu BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
