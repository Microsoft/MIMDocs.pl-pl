---
title: Instalacja analizy pakietu BHOLD | Dokumentacja firmy Microsoft
description: Moduł analizy BHOLD zapewnia oparte na regułach testowanie dostępu do danych
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ec7069156aa033b33a139ae83e26abcdea7b482a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358333"
---
# <a name="bhold-analytics-installation"></a>Instalacja analizy pakietu BHOLD

Moduł analizy BHOLD zapewnia oparte na regułach testowanie dostępu do danych, aby upewnić się, czy Twoja organizacja skutecznie kontrolować dostęp do danych i jest zgodna z wymaganiami dotyczącymi dostępu do wewnętrznych i zewnętrznych. Analiza wpływu automatycznych, generowane przez moduł analizy BHOLD zawiera przegląd pokazujący liczbę użytkowników, którzy mogą być naruszone za wymuszania proponowaną regułę, zarówno tych, którzy będą zgodne z regułą, jak i tych, którzy naruszyłoby reguły. Moduł analizy BHOLD również zapewniają szczegółową listę użytkowników, którzy będą zgodne z regułą oraz tych, którzy naruszyłoby reguły.

## <a name="bhold-analytics-installation-requirements"></a>Wymagania dotyczące instalacji analizy BHOLD

Przed zainstalowaniem modułu analizy BHOLD, należy zainstalować moduł Core pakietu BHOLD na serwerze, na którym planujesz zainstalować moduł analizy BHOLD. Aby dowiedzieć się, jak instalowanie modułu BHOLD Core, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem Zainstaluj moduł analizy BHOLD, musisz być przygotowana do podawania, Kreator instalacji analizy BHOLD wymaga, aby ukończyć instalację. Następujący arkusz pomoże Zarejestruj te informacje będą gotowe do dostarczenia go, gdy jest to konieczne.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, czy zabezpieczeń Active Directory Domain Services będzie kontrolować dostęp do pakietu BHOLD Core.                                                                                                                | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konto usługi, który został utworzony podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę, tylko wtedy, gdy będzie ona nieprawidłowa. **Ważne:** Podaj nazwę domeny przy użyciu nazwy NetBIOS (krótki), a nie w pełni kwalifikowana nazwa domeny (FQDN). Na przykład jeśli nazwę FQDN domeny fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika BHOLD podstawowe usługi.                                                                                                                                                          | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Zapisz hasło w tym miejscu: **ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Instalacja analizy pakietu BHOLD

Aby zainstalować moduł analizy BHOLD, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł analizy pakietu BHOLD na:

- BholdAnalytics<em>\<wersji\></em>\_Release.msi

Zastąp *\<wersji\>* numerem wersji, wersji analizy BHOLD, którym ją instalujesz.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

# <a name="next-steps"></a>Kolejne kroki

- [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
