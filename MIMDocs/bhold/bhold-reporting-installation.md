---
title: Instalacja raportowania pakietu BHOLD | Dokumentacja firmy Microsoft
description: Moduł raportowania pakietu BHOLD umożliwia generowanie raportów dotyczących ról i zasad autoryzacji
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1f69f9f08cba24898509c771e4477b81c5ed272f
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358012"
---
# <a name="bhold-reporting-installation"></a>Instalacja raportowania pakietu BHOLD

Moduł raportowania pakietu BHOLD daje możliwość generowania raportów o rolach i innych zasad autoryzacji w BHOLD. Te raporty są często przydatne do inspekcji, lub potrzeby demonstrowania zgodności z wymaganiami prawnymi. Ponadto ten moduł rozszerza możliwości zarządzania autoryzacji w organizacji, zapewniając użytkownikom informacje, które są im potrzebne do analizowania członkostwa ich ról. Raporty mogą mieć ograniczony widoków, upewnij się, że użytkownicy, którzy tworzą raporty są wyświetlane tylko te informacje, które mogą zobaczyć.

## <a name="bhold-reporting-installation-requirements"></a>Wymagania dotyczące instalacji raportowania pakietu BHOLD

Przed zainstalowaniem modułu raportowania pakietu BHOLD, należy zainstalować moduł Core pakietu BHOLD na serwerze, na którym planujesz zainstalować moduł raportowania pakietu BHOLD. Aby dowiedzieć się, jak instalowanie modułu BHOLD Core, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Jeśli instalujesz raportowania pakietu BHOLD i zaświadczania pakietu BHOLD, należy zainstalować raportowania pakietu BHOLD przed zainstalowaniem pakietu BHOLD zaświadczania.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem zainstalować moduł raportowania pakietu BHOLD muszą być przygotowana do podawania, Kreator instalacji programu Reporting BHOLD wymaga, aby ukończyć instalację. Następujący arkusz pomoże Zarejestruj te informacje będą gotowe do dostarczenia go, gdy jest to konieczne.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, czy zabezpieczeń Active Directory Domain Services będzie kontrolować dostęp do pakietu BHOLD Core.                                                                                                                | Zaznacz pole wyboru. </br>**Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konto usługi, który został utworzony podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę, tylko wtedy, gdy będzie ona nieprawidłowa. **Ważne:** Podaj nazwę domeny przy użyciu nazwy NetBIOS (krótki), a nie w pełni kwalifikowana nazwa domeny (FQDN). Na przykład jeśli nazwę FQDN domeny fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika BHOLD podstawowe usługi.                                                                                                                                                          | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Zapisać hasło tutaj: </br>**Ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Instalacja raportowania pakietu BHOLD

Aby zainstalować moduł raportowania pakietu BHOLD, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł raportowania pakietu BHOLD na:

- BholdReporting<em>\<wersji\></em>\_Release.msi

Zastąp *\<wersji\>* numerem wersji, wersji pakietu BHOLD raportowania, którym ją instalujesz.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

## <a name="next-steps"></a>Kolejne kroki

- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
