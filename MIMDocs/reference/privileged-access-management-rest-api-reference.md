---
title: Privileged Access Management REST API Reference | Dokumentacja firmy Microsoft
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Dokumentacja interfejsu API REST zarządzania uprzywilejowanym dostępem
Program Microsoft Identity Manager (MIM) 2016 dodaje nowy scenariusz o nazwie Privileged Access Management (PAM). PAM umożliwia większą kontrolę nad wysokiej uprzywilejowanych kont, takich jak Administratorzy systemu lub usługi, prawa dostępu do poufnych zasobów organizacji. PAM określa wysokim poziomie uprawnień dostępu do konta, zapewniając praw dostępu ograniczony czas, po prostu w time (JIT), gdy są potrzebne prawa dostępu.

Użytkownika można zadawać usługi MIM Privileged uprawnienia (podniesienia uprawnień) w jeden z dwóch sposobów:

- Za pomocą interfejsu API REST PAM
- Za pomocą polecenia cmdlet programu PowerShell PAM New-PAMRequest

Tematy w tym przewodniku opisano interfejsu API REST PAM. Aby uzyskać więcej informacji o korzystaniu z programu PowerShell polecenia cmdlet, zobacz Przewodnik po laboratorium testowym: prezentacja Privileged Access Management przy użyciu programu Microsoft Identity Manager dostępne w witrynie connect.

##<a name="pam-rest-api-resources-and-operations"></a>Zasoby interfejsu API PAM REST i operacje
Interfejs API REST PAM działa na następujące zasoby
- **Rola PAM**: roli A PAM kojarzy kolekcji użytkowników z kolekcją praw dostępu. Prawa dostępu są definiowane w odniesieniu do grup zabezpieczeń.  Każdej roli PAM zawiera listę kont użytkowników, nazywane obiektami, które są uprawnione do podniesienia uprawnień do roli PAM. Można wykonywać następujące operacje w odniesieniu do ról PAM:

    - [Pobieranie ról PAM](privileged-access-management-get-roles.md)

- **Żądanie PAM**: użytkownik, który chce podnieść poziom do uprawnień roli PAM ma przesyłać żądania PAM w celu uzyskania zatwierdzenia dla żądania w celu podniesienia uprawnień. Obiekt żądania PAM śledzi cyklem życia tego żądania w usłudze MIM. Można wykonywać następujące operacje dotyczące żądań PAM:

    - [Tworzenie żądania PAM](privileged-access-management-create-request.md)
    - [Pobieranie żądań PAM](privileged-access-management-get-requests.md)
    - [Zamykanie żądania PAM](privileged-access-management-close-request.md)

- **Oczekujące żądania PAM**: umożliwia zatwierdzanie lub odrzucanie żądań PAM, które zostały przesłane przez użytkowników. Oczekujące żądania PAM, można wykonywać następujące operacje na:

    - [Pobieranie oczekujących żądań PAM](privileged-access-management-get-pending-requests.md)
    - [Zatwierdzanie lub odrzucanie oczekującego żądania PAM](privileged-access-management-approve-reject-pending-request.md)

- **Sesja PAM**: korzystając z interfejsu API REST PAM, klienta (na przykład przeglądarki sieci web) nie ma sesji z punktem końcowym interfejsu API REST PAM. W tej sesji klienta jest uwierzytelniony do punktu końcowego interfejsu API REST. Można wykonywać następujące operacje na sesje PAM:

     - [Pobieranie informacji dotyczących sesji PAM](privileged-access-management-get-session-info.md)

Aby uzyskać szczegółowe informacje o usłudze, zobacz [szczegóły usługi interfejsu API REST PAM](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>Portal przykładowy PAM w witrynie GitHub
Jednym ze sposobów Dowiedz się, jak za pomocą interfejsu API REST PAM jest przy użyciu przykładowego portalu PAM, przykładowa aplikacja sieci web, który używa interfejsu API. Można znaleźć kod portal przykładowy PAM w [repozytorium przykładowy PAM w serwisie GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Można poznać sposoby wdrażania przykładowego portalu w przewodniku po laboratorium testowym PAM.
