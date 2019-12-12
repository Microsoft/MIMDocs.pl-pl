---
title: Instalacja generatora modeli pakietu BHOLD | Microsoft Docs
description: Model pakietu BHOLD umożliwia tworzenie struktury danych z różnych źródeł
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d2510d6ea604dd88e56436812ed8bc975bc5c2b
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516745"
---
# <a name="bhold-model-generator-installation"></a>Instalacja generatora modeli pakietu BHOLD

Za pomocą modułu generatora modeli pakietu BHOLD można struktury danych z autorytatywnych źródeł, które zawierają informacje o użytkowniku i organizacji oraz listy kontroli dostępu (ACL) do modelu, który może być używany w administrowaniu pakietu BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Wymagania dotyczące instalacji generatora modeli pakietu BHOLD 

Przed zainstalowaniem modułu generatora modeli pakietu BHOLD należy zainstalować następujące elementy:

1. Moduł pakietu BHOLD Core na serwerze, na którym planujesz zainstalować moduł generatora modelu pakietu BHOLD. Aby uzyskać informacje na temat instalowania modułu pakietu BHOLD Core, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. Należy zainstalować dostawcę OLE DB firmy Microsoft dla programu Microsoft Jet. Aby uzyskać więcej informacji, zobacz [ten artykuł](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Nie należy instalować generatora modeli pakietu BHOLD w sieci produkcyjnej. Generator modelu pakietu BHOLD jest przeznaczony do użycia w trybie offline w środowisku przejściowym, aby utworzyć znormalizowany model ról, który można zaimportować do modelu roli przedsiębiorstwa. Uruchomienie generatora modelu pakietu BHOLD w sieci produkcyjnej może spowodować utratę istniejącego modelu roli.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed zainstalowaniem modułu generatora modeli pakietu BHOLD należy przygotować się, aby podać informacje wymagane przez Kreatora instalacji generatora modeli pakietu BHOLD do ukończenia instalacji. Poniższy arkusz pomoże Ci zarejestrować te informacje, dzięki czemu będzie można je w razie potrzeby udostępnić. Należy również upewnić się, że

Pakiet redystrybucyjny aparatu bazy danych programu Microsoft Access 2010

 

*Z \<* <http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems> *\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Ustawienia konta**

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Używanie dostawcy zabezpieczeń na komputerze/domenie** | Po wybraniu określa, że zabezpieczenia Active Directory Domain Services będą kontrolować dostęp do pakietu BHOLD rdzeń.                                                                                                                | zaznacz pole wyboru. **Ważne:** Instalacja nie powiedzie się, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konto usługi, które zostało utworzone podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [pakietu BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest ona niepoprawna. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (krótkiej), a nie w pełni kwalifikowanej nazwy domeny (FQDN). Na przykład, jeśli nazwa FQDN domeny to fabrikam.com, określ nazwę domeny jako FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika usługi pakietu BHOLD Core.                                                                                                                                                          | W tym miejscu wpisz nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Napisz hasło tutaj: **Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

**Ustawienia kopii zapasowej bazy danych**

| Element                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Korzystanie ze zintegrowanych zabezpieczeń**                 | Określa, że uwierzytelnianie systemu Windows jest używane w celu uzyskania dostępu do bazy danych.                                                                                                                                                                                                                                                                                                                                                        | Zaznacz to pole wyboru, jeśli uwierzytelnianie systemu Windows jest używane do nawiązywania połączenia z SQL Server. Usuń zaznaczenie tego pola wyboru, jeśli jest używane uwierzytelnianie SQL Server. Baza danych musi zostać utworzona przed uruchomieniem Instalatora pakietu BHOLD Core, jeśli jest używane uwierzytelnianie SQL Server. **Uwaga:** Jeśli jest używane uwierzytelnianie systemu Windows, użytkownik musi być zalogowany przy użyciu konta z rolą serwera sysadmin na serwerze bazy danych. **Ważne:** Uwierzytelniania SQL Server należy używać tylko w środowiskach testowych. Firma Microsoft zdecydowanie zaleca korzystanie z uwierzytelniania systemu Windows w ramach wdrożeń produkcyjnych. |
| Hasło **użytkownika bazy danych** i **bazy danych** | Określa nazwę użytkownika i hasło użytkownika z rolą serwera sysadmin na serwerze bazy danych. Te wartości są dostarczane tylko wtedy, gdy używane jest uwierzytelnianie SQL Server.                                                                                                                                                                                                                                                  | Napisz SQL Server nazwę użytkownika tutaj: Wpisz tutaj hasło użytkownika SQL Server: </br></br> **Ważne:** Pamiętaj, aby zachować to hasło w ukrytej, bezpiecznej lokalizacji.                                                                                                                                                                                                                                                                                                                                                                                                           |
| Nazwa **serwera bazy danych** i **bazy danych**   | Określa nazwę NetBIOS serwera bazy danych i nazwę bazy danych kopii zapasowej, która zostanie utworzona przez konfigurację generatora modeli pakietu BHOLD. Jeśli nie używasz domyślnego wystąpienia serwera bazy danych, Określ wystąpienie serwera bazy danych w formularzu *\<server\>* \\ *\<wystąpienia\>* .  Firma Microsoft zaleca, aby nazwać bazę danych kopii zapasowej przy użyciu nazwy podstawowej bazy danych pakietu BHOLD, a \_kopii ZAPASowej, na przykład B1_BACKUP. | Wpisz tutaj nazwę serwera (lub serwera i wystąpienia): </br> Wpisz tutaj nazwę bazy danych:

## <a name="bhold-model-generator-setup"></a>Konfiguracja generatora modeli pakietu BHOLD

Aby zainstalować moduł generatora modelu pakietu BHOLD, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, na którym zamierzasz zainstalować moduł pakietu BHOLD Core w:

- BholdModelGenerator *\<wersja\>* \_Release. msi

Zastąp *\<Version\>* numerem wersji instalowanego generatora modeli pakietu BHOLD.

Aby uruchomić plik programu jako administrator, kliknij plik prawym przyciskiem myszy, a następnie kliknij polecenie **Uruchom jako administrator**.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje na temat sposobu tworzenia plików wejściowych [Dokumentacja techniczna Microsoft pakietu BHOLD Suite](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
