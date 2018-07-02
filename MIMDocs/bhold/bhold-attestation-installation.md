---
title: Instalacja zaświadczania BHOLD | Dokumentacja firmy Microsoft
description: Moduł zaświadczania BHOLD umożliwia wyznaczanie osoby dokonujące przeglądu i wykonywanie przeglądów
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 6838355c05f7c19436d8a83839044ea5f4e2533d
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290343"
---
# <a name="bhold-attestation-installation"></a>Instalacja zaświadczania BHOLD

Moduł BHOLD zaświadczania umożliwia wyznaczanie osoby dokonujące przeglądu i wykonywanie przeglądami cykliczne relacje między użytkownikami i uprawnień dla poszczególnych aplikacji i kont.

## <a name="bhold-attestation-installation-requirements"></a>Wymagania dotyczące instalacji poświadczenia BHOLD

Przed zainstalowaniem modułu zaświadczania BHOLD, należy zainstalować moduł BHOLD Core na serwerze, na którym planujesz zainstalować moduł BHOLD zaświadczania. Informacje o instalowaniu modułu BHOLD Core, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Ponieważ zaświadczania BHOLD modułu Kontakty wysyła wiadomości e-mail do użytkowników, środowisko musi mieć serwera poczty e-mail Simple Mail Transfer Protocol (SMTP), takich jak Microsoft Exchange Server.

> [!IMPORTANT]
> Jeśli instalujesz BHOLD raportowania i zaświadczania BHOLD należy zainstalować raportowania BHOLD przed zainstalowaniem BHOLD zaświadczania.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem instalowania modułu BHOLD zaświadczania o należy przygotować się do zawierają informacje, z Kreatora instalacji zaświadczania BHOLD wymaga, aby zakończyć instalację. Następujący arkusz pomoże Ci Zarejestruj te informacje, więc wszystko będzie gotowe do dostarczenia go w razie potrzeby.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, że zabezpieczenia usług domenowych w usłudze Active Directory będzie kontrolował dostęp do podstawowych BHOLD.                                                                                                                | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konta usługi, utworzony podczas instalowania BHOLD Core. Aby uzyskać więcej informacji, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest nieprawidłowe. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (short), a nie w pełni kwalifikowaną nazwę (FQDN). Na przykład jeśli nazwa FQDN domeny, to fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika usługi BHOLD Core.                                                                                                                                                          | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Zapisać hasło tutaj: **ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalacja zaświadczania BHOLD

Aby zainstalować moduł zaświadczania BHOLD, zaloguj się jako członek grupy Administratorzy domeny, Pobierz następującego pliku i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł zaświadczania BHOLD na:

- BholdAttestation<em>\<wersji\></em>\_Release.msi

Zastąp *\<wersji\>* z numerem wersji instalowanej wersji BHOLD zaświadczania.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

## <a name="next-steps"></a>Następne kroki

- [Przewodnik instalacji BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)