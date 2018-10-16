---
title: Instalacja generatora modeli pakietu BHOLD | Dokumentacja firmy Microsoft
description: BHOLD model umożliwia struktury danych z różnych źródeł
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: ddb49219b5f68ff060f9b15a9ab64cb85a035d98
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333483"
---
# <a name="bhold-model-generator-installation"></a>Instalacja generatora modeli pakietu BHOLD

Przy użyciu modułu generatora modeli pakietu BHOLD, tworzyć struktury danych ze źródeł autorytatywny, która zawiera użytkownika i informacji organizacyjnych wraz z listy kontroli dostępu (ACL) do modelu, który może służyć w administrowaniu pakietu BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Wymagania dotyczące instalacji generatora modeli pakietu BHOLD 

Przed zainstalowaniem modułu generatora modeli pakietu BHOLD, należy zainstalować następujące czynności:

1. Moduł Core pakietu BHOLD na serwerze, na którym planujesz zainstalować moduł generatora modeli pakietu BHOLD. Aby dowiedzieć się, jak instalowanie modułu BHOLD Core, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. Musi być zainstalowany dostawca Microsoft OLE DB dla programu Microsoft Jet. Aby uzyskać więcej informacji, zobacz [w tym artykule](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Nie należy instalować generatora modeli pakietu BHOLD w sieci produkcyjnej. Generator modeli pakietu BHOLD jest przeznaczony do użycia w trybie offline w środowisku przejściowym do utworzenia znormalizowane modelu roli, który można zaimportować do modelu roli przedsiębiorstwa. Uruchamianie generatora modeli pakietu BHOLD w sieci produkcyjnej może spowodować utratę istniejącego modelu roli.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed zainstalowaniem modułu generatora modeli pakietu BHOLD, musisz być przygotowany do podawania, Kreator instalacji generatora modeli pakietu BHOLD wymaga, aby ukończyć instalację. Następujący arkusz pomoże Zarejestruj te informacje będą gotowe do dostarczenia go, gdy jest to konieczne. Należy również upewnić się, że

Program Microsoft Access bazy danych aparatu 2010 Redistributable

 

*Z \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Ustawienia konta**

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, czy zabezpieczeń Active Directory Domain Services będzie kontrolować dostęp do pakietu BHOLD Core.                                                                                                                | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konto usługi, który został utworzony podczas instalowania pakietu BHOLD Core. Aby uzyskać więcej informacji, zobacz [Instalacja podstawowa pakietu BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę, tylko wtedy, gdy będzie ona nieprawidłowa. **Ważne:** Podaj nazwę domeny przy użyciu nazwy NetBIOS (krótki), a nie w pełni kwalifikowana nazwa domeny (FQDN). Na przykład jeśli nazwę FQDN domeny fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika BHOLD podstawowe usługi.                                                                                                                                                          | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Zapisz hasło w tym miejscu: **ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

**Ustawienia tworzenia kopii zapasowej bazy danych**

| Element                                        | Opis                                                                                                                                                                                                                                                                                                                                                                                                                  | Wartość                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj zintegrowanych zabezpieczeń**                 | Określa, że uwierzytelnianie Windows jest używane do dostępu do bazy danych.                                                                                                                                                                                                                                                                                                                                                        | Zaznacz pole wyboru, jeśli jest używane uwierzytelnianie Windows, aby połączyć się z serwerem SQL. Usuń zaznaczenie pola wyboru, jeśli jest używane uwierzytelnianie programu SQL Server. Baza danych muszą zostać utworzone przed systemem BHOLD podstawowych ustawień Jeśli uwierzytelnianie programu SQL Server jest używany. **Uwaga:** Jeśli używane jest uwierzytelnianie Windows, użytkownik musi być zalogowany przy użyciu konta które ma roli serwera sysadmin na serwerze bazy danych. **Ważne:** Użyj uwierzytelniania programu SQL Server tylko w środowiskach testowych. Firma Microsoft zaleca korzystanie z uwierzytelniania Windows we wdrożeniach produkcyjnych. |
| **Użytkownika bazy danych** i **hasła bazy danych** | Określa nazwę użytkownika i hasło użytkownika z roli serwera sysadmin na serwerze bazy danych. Wartości te są dostarczane tylko wtedy, gdy jest używane uwierzytelnianie programu SQL Server.                                                                                                                                                                                                                                                  | Zapis nazwę użytkownika w tym miejscu programu SQL Server: zapis w tym miejscu hasło użytkownika programu SQL Server: </br></br> **Ważne:** koniecznie Zapisz to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Serwer bazy danych** i **Nazwa bazy danych**   | Określa nazwę NetBIOS serwera bazy danych i nazwę pliku kopii zapasowej, który spowoduje utworzenie ustawienia generatora modeli pakietu BHOLD. Jeśli nie używasz domyślnego wystąpienia serwera bazy danych, określ wystąpienie serwera bazy danych w postaci  *\<serwera\>*\\*\<wystąpienia\>* .  Firma Microsoft zaleca, nadaj nazwę kopii zapasowej bazy danych przy użyciu nazwy podstawowej BHOLD bazę danych, a następnie \_kopii zapasowej, na przykład B1_BACKUP. | Zapis nazwy serwera (lub serwera i wystąpienia) w tym miejscu: </br> Napisz tutaj nazwę bazy danych:

## <a name="bhold-model-generator-setup"></a>Ustawienia generatora modeli pakietu BHOLD

Aby zainstalować moduł generatora modeli pakietu BHOLD, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł Core pakietu BHOLD na:

- BholdModelGenerator  *\<wersji\>*\_Release.msi

Zastąp *\<wersji\>* numerem wersji, wersji generatora modeli pakietu BHOLD, którym ją instalujesz.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

## <a name="next-steps"></a>Kolejne kroki

- Aby uzyskać informacje na temat tworzenia plików wejściowych [Microsoft dokumentacja techniczna pakietu BHOLD](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)