---
title: PAKIETU BHOLD raportowania instalacji | Microsoft Docs
description: Moduł raportowania pakietu BHOLD umożliwia generowanie raportów dotyczących ról i zasad autoryzacji
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7bac40105aa53add914599c59e812587b6be9585
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042037"
---
# <a name="bhold-reporting-installation"></a>PAKIETU BHOLD raportowanie

Moduł raportowania pakietu BHOLD umożliwia generowanie raportów dotyczących ról i innych zasad autoryzacji w programie pakietu BHOLD. Te raporty są często przydatne do inspekcji lub w celu zapewnienia zgodności z wymaganiami prawnymi. Ponadto ten moduł rozszerza możliwości zarządzania autoryzacją w organizacji przez umożliwienie użytkownikom informacji potrzebnych do analizowania członkostwa ich ról. Raporty mogą mieć ograniczone widoki, które zapewniają, że użytkownicy, którzy tworzą raporty, są wyświetlani tylko te informacje, które mogą zobaczyć.

## <a name="bhold-reporting-installation-requirements"></a>Wymagania dotyczące instalacji raportowania pakietu BHOLD

Przed zainstalowaniem modułu raportowania pakietu BHOLD należy zainstalować moduł pakietu BHOLD Core na serwerze, na którym planujesz zainstalować moduł raportowania pakietu BHOLD. Aby uzyskać informacje na temat instalowania modułu pakietu BHOLD Core, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Jeśli instalujesz zarówno pakietu BHOLD raportowanie, jak i zaświadczanie o pakietu BHOLD, musisz zainstalować raportowanie pakietu BHOLD przed zainstalowaniem zaświadczania pakietu BHOLD.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem instalacji modułu raportowania pakietu BHOLD należy przygotować się do podania informacji wymaganych przez Kreatora instalacji raportowania pakietu BHOLD do ukończenia instalacji. Poniższy arkusz pomoże Ci zarejestrować te informacje, dzięki czemu będzie można je w razie potrzeby udostępnić.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartościami**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Używanie dostawcy zabezpieczeń na komputerze/domenie** | Po wybraniu określa, że zabezpieczenia Active Directory Domain Services będą kontrolować dostęp do pakietu BHOLD rdzeń.                                                                                                                | zaznacz pole wyboru. </br>**Ważne:** Instalacja nie powiedzie się, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domain**                                  | Określa domenę zawierającą konto usługi, które zostało utworzone podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest ona niepoprawna. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (krótkiej), a nie w pełni kwalifikowanej nazwy domeny (FQDN). Na przykład, jeśli nazwa FQDN domeny to fabrikam.com, określ nazwę domeny jako FABRIKAM. |
| **Użytkownik**                                    | Określa nazwę logowania konta użytkownika usługi pakietu BHOLD Core.                                                                                                                                                          | W tym miejscu wpisz nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Napisz hasło tutaj: </br>**Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>PAKIETU BHOLD raportowanie

Aby zainstalować moduł raportowania pakietu BHOLD, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, na którym zamierzasz zainstalować moduł raportowania pakietu BHOLD:

- BholdReporting<em>\<wersja\></em>\_pliku MSI

Zastąp * \<wersję\> * numerem wersji programu pakietu BHOLD Reporting, którą instalujesz.

Aby uruchomić plik programu jako administrator, kliknij plik prawym przyciskiem myszy, a następnie kliknij polecenie **Uruchom jako administrator**.

## <a name="next-steps"></a>Następne kroki

- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
