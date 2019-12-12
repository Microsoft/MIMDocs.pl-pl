---
title: PAKIETU BHOLD zaświadczania o instalacji | Microsoft Docs
description: Moduł zaświadczania pakietu BHOLD umożliwia wyznaczanie recenzentów i wykonywanie przeglądów
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4c3a6248585d55fddbbca3153f33734d7c5c429
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64519172"
---
# <a name="bhold-attestation-installation"></a>Instalacja zaświadczania pakietu BHOLD

Moduł zaświadczania pakietu BHOLD pozwala wyznaczyć recenzentów i wykonywać cykliczne przeglądy relacji między użytkownikami a uprawnieniami poszczególnych aplikacji i kontami.

## <a name="bhold-attestation-installation-requirements"></a>Wymagania dotyczące instalacji zaświadczania pakietu BHOLD

Przed zainstalowaniem modułu zaświadczania pakietu BHOLD należy zainstalować podstawowy moduł pakietu BHOLD na serwerze, na którym planujesz zainstalować moduł zaświadczania pakietu BHOLD. Aby uzyskać informacje na temat instalowania modułu pakietu BHOLD Core, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Ponieważ kontakty modułu zaświadczania pakietu BHOLD wysyłają wiadomości e-mail do użytkowników, środowisko musi mieć serwer poczty e-mail Simple Mail Transfer Protocol (SMTP), taki jak Microsoft Exchange Server.

> [!IMPORTANT]
> Jeśli instalujesz zarówno pakietu BHOLD raportowanie, jak i zaświadczanie o pakietu BHOLD, musisz zainstalować raportowanie pakietu BHOLD przed zainstalowaniem zaświadczania pakietu BHOLD.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem instalacji modułu zaświadczania pakietu BHOLD należy przygotować się do podania informacji wymaganych przez Kreatora instalacji zaświadczania o pakietu BHOLD do ukończenia instalacji. Poniższy arkusz pomoże Ci zarejestrować te informacje, dzięki czemu będzie można je w razie potrzeby udostępnić.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Używanie dostawcy zabezpieczeń na komputerze/domenie** | Po wybraniu określa, że zabezpieczenia Active Directory Domain Services będą kontrolować dostęp do pakietu BHOLD rdzeń.                                                                                                                | zaznacz pole wyboru. **Ważne:** Instalacja nie powiedzie się, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konto usługi, które zostało utworzone podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest ona niepoprawna. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (krótkiej), a nie w pełni kwalifikowanej nazwy domeny (FQDN). Na przykład, jeśli nazwa FQDN domeny to fabrikam.com, określ nazwę domeny jako FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika usługi pakietu BHOLD Core.                                                                                                                                                          | W tym miejscu wpisz nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Napisz hasło tutaj: **Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalacja zaświadczania pakietu BHOLD

Aby zainstalować moduł zaświadczania pakietu BHOLD, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, na którym ma zostać zainstalowany moduł zaświadczania pakietu BHOLD na:

- BholdAttestation<em>\<wersja\></em>\_Release. msi

Zastąp *\<wersja\>wersją* zainstalowaną wersji zaświadczania pakietu BHOLD.

Aby uruchomić plik programu jako administrator, kliknij plik prawym przyciskiem myszy, a następnie kliknij polecenie **Uruchom jako administrator**.

## <a name="next-steps"></a>Następne kroki

- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
