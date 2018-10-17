---
title: Instalacja łącznika zarządzania dostęp do pakietu BHOLD | Dokumentacja firmy Microsoft
description: Moduł łącznik pakietu BHOLD obsługuje wstępnych i bieżących synchronizacji danych
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 60886a84c6105e94a2cd3d42f17b86b2d69c8c0a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358605"
---
# <a name="access-management-connector-installation"></a>Instalacja łącznika do dostępu do zarządzania

Moduł BHOLD Suite dostępu do zarządzania łącznika obsługuje zarówno początkowy, jak i bieżące synchronizacji danych do pakietu BHOLD. Dostęp do łącznika zarządzania współpracuje z usługi synchronizacji programu Microsoft Identity Manager (MIM) do przenoszenia danych między baza danych BHOLD Core, metaverse programu FIM 2010 i aplikacjach docelowych i magazyny tożsamości. Po zainstalowaniu łącznika zarządzania dostęp do modułu, można utworzyć agentów zarządzania usługi FIM, które sterują przepływem danych między BHOLD i MIM.

## <a name="access-management-connector-software-requirements"></a>Wymagania dotyczące oprogramowania łącznika zarządzania dostęp

Przed zainstalowaniem modułu łącznika zarządzania dostępu należy zainstalować program Microsoft .NET Framework 4. Aby uzyskać więcej informacji na temat programu .NET Framework 4 i instrukcje dotyczące instalacji, zobacz [strona główna programu Microsoft .NET](http://www.microsoft.com/net).
Łącznik zarządzania dostępu należy zainstalować na komputerze z systemem usługi synchronizacji FIM MIM.

## <a name="access-management-connector-setup"></a>Konfigurowanie łącznika zarządzania dostępu do zasobów

Aby zainstalować moduł kontrolę dostępu, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, który ma zostać zainstalowany moduł integracji usługi FIM pakietu BHOLD na:

- AccessManagementConnector.msi

Aby uruchomić plik programu jako administrator, kliknij prawym przyciskiem myszy plik, a następnie kliknij przycisk **Uruchom jako administrator**.

## <a name="next-steps"></a>Kolejne kroki

- [Instalacja integracji pakietu BHOLD FIM](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) Aby włączyć samoobsługi użytkowników końcowych, ról, należy zainstalować moduł integracji usługi FIM pakietu BHOLD
- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
