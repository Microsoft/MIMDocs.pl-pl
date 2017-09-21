---
title: Instalacja analizy BHOLD | Dokumentacja firmy Microsoft
description: "Moduł analizy BHOLD zapewnia oparte na regułach testowanie dostępu do danych"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 631e08667e5d1535d8f63cc297aad360080f8b20
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-analytics-installation"></a>Instalacja analizy BHOLD

Moduł analizy BHOLD zapewnia oparte na regułach testowanie dostępu do danych, aby efektywnie kontrolować dostęp do danych organizacji i spełnia wymagania dotyczące dostępu do wewnętrznych i zewnętrznych. Analiza wpływu zautomatyzowane, generowane przez moduł analizy BHOLD zapewnia przegląd, w którym jest wyświetlana liczba użytkowników, którzy może niekorzystnie wpływać stosowania reguły proponowanych, zarówno tych, którzy będą zgodne z regułą, jak i tych, którzy naruszyłoby reguły. Moduł analizy BHOLD oferuje również uzyskać szczegółową listę użytkowników, którzy będą zgodne z reguły oraz tych, którzy naruszyłoby reguły.

## <a name="bhold-analytics-installation-requirements"></a>Wymagania dotyczące instalacji analizy BHOLD

Przed zainstalowaniem modułu analizy BHOLD, należy zainstalować moduł BHOLD Core na serwerze, na którym planujesz zainstalować moduł analizy BHOLD. Informacje o instalowaniu modułu BHOLD Core, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem instalowania modułu analizy BHOLD, musisz być przygotowana do dostarczania informacji, Kreator instalacji analizy BHOLD wymagany do ukończenia instalacji. Następujący arkusz pomoże Ci Zarejestruj te informacje, więc wszystko będzie gotowe do dostarczenia go w razie potrzeby.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, że zabezpieczenia usług domenowych w usłudze Active Directory będzie kontrolował dostęp do podstawowych BHOLD.                                                                                                                | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konta usługi, utworzony podczas instalowania BHOLD Core. Aby uzyskać więcej informacji, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest nieprawidłowe. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (short), a nie w pełni kwalifikowaną nazwę (FQDN). Na przykład jeśli nazwa FQDN domeny, to fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika usługi BHOLD Core.                                                                                                                                                          | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Zapisać hasło tutaj: **ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Instalacja analizy BHOLD

Aby zainstalować moduł analizy BHOLD, zaloguj się jako członek grupy Administratorzy domeny, Pobierz następującego pliku i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł analizy BHOLD na:

- BholdAnalytics*\<wersji\>*\_Release.msi

Zastąp * \<wersji\> * z numerem wersji wersji analizy BHOLD, który jest instalowany.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

# <a name="next-steps"></a>Następne kroki

- [Instalacja Core BHOLD](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)
- [Przewodnik instalacji BHOLD](bhold-installation-guide.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)