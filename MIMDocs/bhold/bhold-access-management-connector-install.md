---
title: Instalacja łącznika zarządzania dostępem pakietu BHOLD | Microsoft Docs
description: Moduł łącznika pakietu BHOLD obsługuje początkową i ciągłą synchronizację danych
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 60886a84c6105e94a2cd3d42f17b86b2d69c8c0a
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516057"
---
# <a name="access-management-connector-installation"></a>Instalacja łącznika zarządzania dostępem

Moduł łącznika zarządzania dostępem do pakietu BHOLD Suite obsługuje początkową i ciągłą synchronizację danych w pakietu BHOLD. Łącznik zarządzania dostępem współpracuje z usługą synchronizacji Microsoft Identity Manager (MIM), aby przenosić dane między podstawową bazą danych pakietu BHOLD, Metaverse programu FIM 2010 oraz aplikacjami docelowymi i magazynami tożsamości. Po zainstalowaniu modułu łącznika zarządzania dostępem można utworzyć agentów zarządzania FIM, którzy kontrolują przepływ danych między pakietu BHOLD i MIM.

## <a name="access-management-connector-software-requirements"></a>Wymagania programowe łącznika zarządzania dostępem

Przed zainstalowaniem modułu łącznika zarządzania dostępem należy zainstalować program Microsoft .NET Framework 4. Aby uzyskać więcej informacji na temat .NET Framework 4 i instrukcje dotyczące instalacji, zobacz [stronę główną Microsoft .NET](http://www.microsoft.com/net).
Łącznik zarządzania dostępem należy zainstalować na komputerze z uruchomioną usługą synchronizacji FIM programu MIM.

## <a name="access-management-connector-setup"></a>Konfiguracja łącznika zarządzania dostępem

Aby zainstalować moduł kontroli zarządzania dostępem, zaloguj się jako członek grupy Administratorzy domeny, pobierz następujący plik i uruchom go jako administrator na serwerze, na którym ma zostać zainstalowany moduł integracji programu pakietu BHOLD FIM:

- AccessManagementConnector. msi

Aby uruchomić plik programu jako administrator, kliknij plik prawym przyciskiem myszy, a następnie kliknij polecenie **Uruchom jako administrator**.

## <a name="next-steps"></a>Następne kroki

- [Pakietu BHOLD z integracją](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) z usługą FIM Aby włączyć samoobsługowe role użytkowników końcowych, można zainstalować moduł integracji programu pakietu BHOLD FIM
- [Przewodnik instalacji pakietu BHOLD](bhold-installation-guide.md)
- [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)
