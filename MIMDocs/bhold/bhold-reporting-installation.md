---
title: Zgłoszenie instalacji BHOLD | Dokumentacja firmy Microsoft
description: Moduł raportowania BHOLD służy do generowania raportów dotyczących ról i zasad autoryzacji
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: deb43aeb9133d7eed958730b0eb2cbd22fe3a0ef
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289309"
---
# <a name="bhold-reporting-installation"></a>Zgłoszenie instalacji BHOLD

Moduł raportowania BHOLD daje możliwość generowania raportów na temat ról i innych zasad autoryzacji w BHOLD. Te raporty często są przydatne do inspekcji lub potrzeby demonstrowania zgodność w celu przepisami dotyczącymi działalności. Ponadto ten moduł rozszerza możliwości zarządzania autoryzacji w organizacji, zapewniając użytkownikom informacji potrzebnych do analizowania członkostwa ich ról. Raporty mogą mieć ograniczony widoki, które upewnij się, że użytkownicy, którzy tworzą raporty są wyświetlane tylko te informacje mogą się w temacie.

## <a name="bhold-reporting-installation-requirements"></a>Wymagania dotyczące instalacji raportowania BHOLD

Przed zainstalowaniem modułu BHOLD raportowania, należy zainstalować moduł BHOLD Core na serwerze, na którym planujesz zainstalować moduł raportowania BHOLD. Informacje o instalowaniu modułu BHOLD Core, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Jeśli instalujesz BHOLD raportowania i zaświadczania BHOLD należy zainstalować raportowania BHOLD przed zainstalowaniem BHOLD zaświadczania.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem zainstalować moduł raportowania BHOLD, musisz być przygotowana do dostarczania informacji, którego BHOLD Kreatora instalacji programu Reporting wymaga, aby ukończyć instalację. Następujący arkusz pomoże Ci Zarejestruj te informacje, więc wszystko będzie gotowe do dostarczenia go w razie potrzeby.

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, że zabezpieczenia usług domenowych w usłudze Active Directory będzie kontrolował dostęp do podstawowych BHOLD.                                                                                                                | Zaznacz pole wyboru. </br>**Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konta usługi, utworzony podczas instalowania BHOLD Core. Aby uzyskać więcej informacji, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest nieprawidłowe. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (short), a nie w pełni kwalifikowaną nazwę (FQDN). Na przykład jeśli nazwa FQDN domeny, to fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika usługi BHOLD Core.                                                                                                                                                          | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Zapisać w tym miejscu: </br>**Ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Instalację funkcji Raportowanie BHOLD

Aby zainstalować moduł raportowania BHOLD, zaloguj się jako członek grupy Administratorzy domeny, Pobierz następującego pliku i uruchom go jako administrator na serwerze, na którym chcesz zainstalować moduł raportowania BHOLD na:

- BholdReporting<em>\<wersji\></em>\_Release.msi

Zastąp *\<wersji\>* z numerem wersji BHOLD raportowania wersji, który jest instalowany.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

## <a name="next-steps"></a>Następne kroki

- [Przewodnik instalacji BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)