---
title: Instalacja łącznika zarządzania dostępu BHOLD | Dokumentacja firmy Microsoft
description: Moduł łącznika BHOLD obsługuje wstępnych i bieżących synchronizacji danych
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
ms.locfileid: "24522332"
---
# <a name="access-management-connector-installation"></a>Instalacja łącznika zarządzania dostępu

Moduł łącznika zarządzania dostępu pakiet BHOLD obsługuje zarówno początkowy, jak i trwającej synchronizacji danych do BHOLD. Łącznik zarządzania dostępu działa usługa synchronizacji programu Microsoft Identity Manager (MIM) do przenoszenia danych między baza danych BHOLD Core metaverse programu FIM 2010 i aplikacji docelowej i magazyny tożsamości. Po zainstalowaniu modułu łącznika zarządzania dostępu, można utworzyć agenci zarządzania programu FIM, która sterowania przepływem danych między BHOLD i MIM.

## <a name="access-management-connector-software-requirements"></a>Wymagania dotyczące oprogramowania łącznika zarządzania dostępu

Przed zainstalowaniem modułu łącznika zarządzania dostępu należy zainstalować Microsoft .NET Framework 4. Aby uzyskać więcej informacji na temat platformy .NET Framework 4 i instrukcje dotyczące instalacji, zobacz [strona główna programu Microsoft .NET](http://www.microsoft.com/net).
Łącznik zarządzania dostępu należy zainstalować na komputerze obsługującym usługi synchronizacji FIM MIM.

## <a name="access-management-connector-setup"></a>Konfigurowanie łącznika zarządzania dostępu do zasobów

Aby zainstalować moduł kontrolę dostępu, zaloguj się jako członek grupy Administratorzy domeny, Pobierz następującego pliku i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł BHOLD FIM integracji na:

- AccessManagementConnector.msi

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

## <a name="next-steps"></a>Następne kroki

- [Instalacja integracji BHOLD FIM](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) Aby włączyć samoobsługi użytkownika końcowego ról, należy zainstalować moduł BHOLD FIM integracji
- [Przewodnik instalacji BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)