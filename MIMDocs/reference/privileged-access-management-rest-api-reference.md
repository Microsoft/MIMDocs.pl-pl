---
title: Informacje o interfejsie API REST Privileged Access Management | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8a318ae5f12fd49e2ee949d81aefd86a7221e120
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760726"
---
# <a name="privileged-access-management-rest-api-reference"></a>Dokumentacja interfejsu API REST Privileged Access Management
Microsoft Identity Manager (MIM) 2016 dodaje nowy scenariusz o nazwie Privileged Access Management (PAM). Usługa PAM umożliwia organizacjom uzyskanie większej kontroli nad prawami dostępu do kont użytkowników z wysokim poziomem uprawnień, takich jak Administratorzy systemu lub usługi, do poufnych zasobów. Moduł PAM kontroluje dostęp do konta o wysokim poziomie uprawnień, zapewniając ograniczone prawa dostępu, just in Time (JIT), gdy są potrzebne prawa dostępu.

Użytkownik może zażądać usługi MIM na potrzeby praw dostępu uprzywilejowanego (podniesienia uprawnień) na jeden z dwóch sposobów:

- Za pomocą interfejsu API REST usługi PAM.
- Za pomocą polecenia cmdlet programu PowerShell w usłudze PAM New-PAMRequest.

Tematy w tym przewodniku opisują interfejs API REST usługi PAM. Aby uzyskać więcej informacji na temat korzystania z polecenia cmdlet programu PowerShell, zobacz _Przewodnik po laboratorium testowym: demonstrowanie Privileged Access Management przy użyciu Microsoft Identity Manager_ , dostępne w witrynie Connect.

## <a name="pam-rest-api-resources-and-operations"></a>Zasoby i operacje interfejsu API REST usługi PAM
Interfejs API REST usługi PAM działa na następujących zasobach:
- **Rola PAM** : rola PAM kojarzy kolekcję użytkowników z kolekcją praw dostępu. Prawa dostępu są definiowane przez odwołanie do grup zabezpieczeń.  Każda rola PAM ma listę kont użytkowników o nazwie kandydaci, które są uprawnione do podniesienia uprawnień do roli PAM. W rolach PAM można wykonywać następujące operacje:

    - [Pobieranie ról PAM](privileged-access-management-get-roles.md)

- **Żądanie PAM** : użytkownik, który chce podnieść poziom uprawnień dostępu roli PAM, musi przesłać żądanie Pam i uzyskać zatwierdzenie żądania podniesienia uprawnień. Obiekt żądania PAM śledzi cykl życia tego żądania w usłudze MIM. W żądaniach PAM można wykonać następujące operacje:

    - [Tworzenie żądania PAM](privileged-access-management-create-request.md)
    - [Pobieranie żądań PAM](privileged-access-management-get-requests.md)
    - [Zamykanie żądania PAM](privileged-access-management-close-request.md)

- **Oczekujące żądanie PAM** : służy do zatwierdzania lub odrzucania żądań PAM przesyłanych przez użytkowników. W przypadku oczekujących żądań PAM można wykonać następujące operacje:

    - [Pobieranie oczekujących żądań PAM](privileged-access-management-get-pending-requests.md)
    - [Zatwierdzanie lub odrzucanie oczekującego żądania PAM](privileged-access-management-approve-reject-pending-request.md)

- **Sesja usługi PAM** : w przypadku korzystania z interfejsu API REST usługi PAM klient (na przykład przeglądarka sieci Web) ma sesję z punktem końcowym interfejsu API REST usługi PAM. W tej sesji klient jest uwierzytelniany w punkcie końcowym interfejsu API REST. W sesjach PAM można wykonywać następujące operacje:

     - [Pobieranie informacji dotyczących sesji PAM](privileged-access-management-get-session-info.md)

Aby uzyskać szczegółowe informacje na temat usługi, zobacz [Szczegóły usługi interfejsu API REST PAM](privileged-access-management-rest-api-service-details.md).

## <a name="pam-sample-portal-on-github"></a>Przykładowy Portal PAM w witrynie GitHub
Jednym ze sposobów, aby dowiedzieć się, jak używać interfejsu API REST usługi PAM, jest korzystanie z przykładowej aplikacji sieci Web, która korzysta z interfejsu API. Kod dla przykładowego portalu PAM można znaleźć w [przykładowym repozytorium PAM w witrynie GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Możesz dowiedzieć się, jak wdrożyć przykładowy Portal w przewodniku po laboratorium testowym PAM.
