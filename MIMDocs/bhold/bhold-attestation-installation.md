---
title: Instalacja zaświadczania pakietu BHOLD | Dokumentacja firmy Microsoft
description: Moduł zaświadczania pakietu BHOLD umożliwia wyznaczenie recenzentów i wykonywanie przeglądów
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 45b650c726ddf423bb6ebc5ec34d4a57e41bb82e
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334129"
---
# <a name="bhold-attestation-installation"></a>Instalacja zaświadczania pakietu BHOLD

Moduł zaświadczania pakietu BHOLD umożliwia wyznaczenie recenzentów i wykonywać przeglądy cykliczne relacje między użytkownikami i uprawnieniami poszczególnych aplikacji i kont.

## <a name="bhold-attestation-installation-requirements"></a>Wymagania dotyczące instalacji zaświadczania pakietu BHOLD

Przed zainstalowaniem modułu zaświadczania pakietu BHOLD, należy zainstalować moduł Core pakietu BHOLD na serwerze, na którym planujesz zainstalować moduł zaświadczania pakietu BHOLD. Aby dowiedzieć się, jak instalowanie modułu BHOLD Core, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Ponieważ zaświadczania pakietu BHOLD modułu Kontakty wysyła wiadomości e-mail do użytkowników, środowisko musi mieć serwer poczty e-mail Simple Mail Transfer Protocol (SMTP), takich jak Microsoft Exchange Server.

> [!IMPORTANT]
> Jeśli instalujesz raportowania pakietu BHOLD i zaświadczania pakietu BHOLD, należy zainstalować raportowania pakietu BHOLD przed zainstalowaniem pakietu BHOLD zaświadczania.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem Zainstaluj moduł zaświadczania pakietu BHOLD muszą być przygotowana do podawania, Kreator instalacji zaświadczania pakietu BHOLD wymaga, aby ukończyć instalację. Następujący arkusz pomoże Zarejestruj te informacje będą gotowe do dostarczenia go, gdy jest to konieczne.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, czy zabezpieczeń Active Directory Domain Services będzie kontrolować dostęp do pakietu BHOLD Core.                                                                                                                | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konto usługi, który został utworzony podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę, tylko wtedy, gdy będzie ona nieprawidłowa. **Ważne:** Podaj nazwę domeny przy użyciu nazwy NetBIOS (krótki), a nie w pełni kwalifikowana nazwa domeny (FQDN). Na przykład jeśli nazwę FQDN domeny fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika BHOLD podstawowe usługi.                                                                                                                                                          | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Zapisz hasło w tym miejscu: **ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalacja zaświadczania pakietu BHOLD

Aby zainstalować moduł zaświadczania pakietu BHOLD, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł zaświadczania pakietu BHOLD na:

- BholdAttestation<em>\<wersji\></em>\_Release.msi

Zastąp *\<wersji\>* numerem wersji, wersji zaświadczania pakietu BHOLD, którym ją instalujesz.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

## <a name="next-steps"></a>Kolejne kroki

- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)