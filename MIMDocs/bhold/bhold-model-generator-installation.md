---
title: Instalacja generator modelu BHOLD | Dokumentacja firmy Microsoft
description: BHOLD model umożliwia struktury danych z różnych źródeł
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 90e7da2a1e39b802723ff0714bd0caccf9649440
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289139"
---
# <a name="bhold-model-generator-installation"></a>Instalacja Generator modelu BHOLD

Przy użyciu modułu BHOLD Generator modeli, można tworzyć struktury danych ze źródeł autorytatywny, zawierający użytkowników i informacje organizacyjne, wraz z listy kontroli dostępu (ACL) w modelu, który może być używana w administrowaniu BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Wymagania dotyczące instalacji Generator modeli BHOLD 

Przed zainstalowaniem modułu BHOLD Generator modeli, należy zainstalować następujące czynności:

1. Moduł BHOLD Core na serwerze, na którym planujesz zainstalować moduł BHOLD Generator modeli. Informacje o instalowaniu modułu BHOLD Core, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. Dostawca Microsoft OLE DB dla programu Microsoft Jet musi być zainstalowany. Aby uzyskać więcej informacji, zobacz [w tym artykule](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Nie należy instalować Generator modeli BHOLD w sieci produkcyjnej. Generator modeli BHOLD jest przeznaczony do użycia w trybie offline w środowisku przemieszczania do utworzenia znormalizowane modelu roli, który można zaimportować do modelu roli przedsiębiorstwa. Uruchamianie Generator modeli BHOLD w sieci produkcyjnej może spowodować utratę istniejącego modelu roli.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed zainstalowaniem modułu BHOLD Generator modeli, należy przygotować się do zawierają informacje, z Kreatora instalacji Generator modelu BHOLD wymaga, aby zakończyć instalację. Następujący arkusz pomoże Ci Zarejestruj te informacje, więc wszystko będzie gotowe do dostarczenia go w razie potrzeby. Należy również upewnić się, że

Aparat 2010 Redistributable bazy danych programu Microsoft Access

 

*Z \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Ustawienia konta**

| **Element**                                    | **Opis**                                                                                                                                                                                                           | **Wartość**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj dostawcy zabezpieczeń na komputerze/domeny** | Po wybraniu Określa, że zabezpieczenia usług domenowych w usłudze Active Directory będzie kontrolował dostęp do podstawowych BHOLD.                                                                                                                | Zaznacz pole wyboru. **Ważne:** instalacja zakończy się niepowodzeniem, jeśli to pole wyboru nie jest zaznaczone.                                                                                                                                                                                                                   |
| **Domeny**                                  | Określa domenę zawierającą konta usługi, utworzony podczas instalowania BHOLD Core. Aby uzyskać więcej informacji, zobacz [instalacji Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Nazwa domeny jest ona dostarczana automatycznie przez kreatora. Zmień nazwę tylko wtedy, gdy jest nieprawidłowe. **Ważne:** Określ nazwę domeny przy użyciu nazwy NetBIOS (short), a nie w pełni kwalifikowaną nazwę (FQDN). Na przykład jeśli nazwa FQDN domeny, to fabrikam.com, należy określić nazwę domeny jako firmy FABRIKAM. |
| **User**                                    | Określa nazwę logowania konta użytkownika usługi BHOLD Core.                                                                                                                                                          | Napisz tutaj nazwę konta użytkownika:                                                                                                                                                                                                                                                                                    |
| **Hasło**                                | Określa hasło konta użytkownika usługi.                                                                                                                                                                       | Zapisać hasło tutaj: **ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                  |

**Ustawienia tworzenia kopii zapasowej bazy danych**

| Element                                        | Opis                                                                                                                                                                                                                                                                                                                                                                                                                  | Wartość                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Użyj zintegrowanych zabezpieczeń**                 | Określa, że dostęp do bazy danych jest używane uwierzytelnianie systemu Windows.                                                                                                                                                                                                                                                                                                                                                        | Zaznacz pole wyboru, jeśli uwierzytelnianie systemu Windows są używane do łączenia się z serwerem SQL. Wyczyść pole wyboru, jeśli jest używane uwierzytelnianie programu SQL Server. Bazy danych muszą być utworzone przed systemem BHOLD podstawowe ustawienia jeśli uwierzytelniania programu SQL Server jest używany. **Uwaga:** Jeśli używane jest uwierzytelnianie systemu Windows, użytkownik musi być zalogowany przy użyciu konta, które ma roli serwera sysadmin na serwerze bazy danych. **Ważne:** Użyj uwierzytelniania programu SQL Server tylko w środowiskach testowych. Firma Microsoft zaleca używanie uwierzytelniania systemu Windows w przypadku wdrożeń produkcyjnych. |
| **Baza danych użytkownika** i **hasła bazy danych** | Określa nazwę użytkownika i hasło użytkownika z roli serwera sysadmin na serwerze bazy danych. Te wartości są określane tylko wtedy, gdy jest używane uwierzytelnianie programu SQL Server.                                                                                                                                                                                                                                                  | Zapis tutaj nazwę użytkownika programu SQL Server: zapis w tym miejscu hasło użytkownika programu SQL Server: </br></br> **Ważne:** należy zachować to hasło w ukrytym, bezpiecznej lokalizacji.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Serwer bazy danych** i **Nazwa bazy danych**   | Określa nazwę NetBIOS serwera bazy danych i nazwa kopii zapasowej bazy danych, który spowoduje utworzenie konfiguracji Generator modelu BHOLD. Jeśli nie używasz domyślnego wystąpienia serwera bazy danych, określ wystąpienie serwera bazy danych w postaci  *\<serwera\>*\\*\<wystąpienia\>* .  Firma Microsoft zaleca się, że nazwa kopii zapasowej bazy danych przy użyciu nazwy bazy danych BHOLD Core następuje \_kopii zapasowej, na przykład B1_BACKUP. | Nazwa serwera (lub serwera i wystąpienia) w tym miejscu zapisu: </br> Wpisz nazwę bazy danych w tym miejscu:

## <a name="bhold-model-generator-setup"></a>Instalator Generator modeli BHOLD

Aby zainstalować moduł Generator modeli BHOLD, zaloguj się jako członek grupy Administratorzy domeny, Pobierz następującego pliku i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł BHOLD Core na:

- BholdModelGenerator  *\<wersji\>*\_Release.msi

Zastąp *\<wersji\>* z numerem wersji instalowanej wersji BHOLD Generator modeli.

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje na temat tworzenia plików wejściowych [dokumentacja techniczna pakietu Microsoft BHOLD](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Przewodnik instalacji BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)