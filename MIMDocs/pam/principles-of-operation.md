---
title: Opis składników funkcji PAM | Dokumentacja firmy Microsoft
description: Usługa Privileged Access Management współużytkuje niektóre składniki z programem MIM, a także ma kilka własnych składników. Dowiedz się, jak te składniki współpracują ze sobą.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: accdda41616ba6c02959eda8549b6dbc322fc627
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043923"
---
# <a name="understand-the-components-of-pam"></a>Opis składników funkcji PAM

Funkcja zarządzania dostępem uprzywilejowanym (Privileged Access Management, PAM) oddziela dostęp administracyjny od zwykłych kont użytkowników. To rozwiązanie opiera się na równoległych lasach:

- *CORP*: las firmowy ogólnego przeznaczenia, zawierający co najmniej jedną domenę. Można mieć większą liczbę lasów CORP, natomiast w przykładach zawartych w tych artykułach dla uproszczenia przyjęto założenie, że występuje jeden taki las z jedną domeną.  
- *PRIV*: dedykowany las utworzony specjalnie na potrzeby tego scenariusza funkcji PAM. Ten las zawiera jedną domenę obsługującą grupy uprzywilejowane oraz konta kopiowane z co najmniej jednej domeny z lasu CORP.

Rozwiązanie MIM skonfigurowane na potrzeby funkcji PAM zawiera następujące składniki:  

- **Usługa MIM**: implementuje logikę biznesową wykonywania operacji zarządzania tożsamością i dostępem, w tym zarządzania kontami uprzywilejowanymi i obsługi żądań podniesienia uprawnień.
- **Portal MIM**: portal oparty na programie SharePoint i hostowany w programie SharePoint 2013, zapewniający administratorom interfejs użytkownika do zarządzania i konfiguracji.
- **Baza danych usługi MIM**: przechowywana w programie SQL Server 2012 lub 2014, zawierająca dane tożsamości i metadane wymagane przez usługę programu MIM.
- **Usługa monitorowania PAM** oraz **usługa składników PAM**: dwie usługi służące do zarządzania kontami uprzywilejowanymi i wspomagające usługę AD lasu PRIV w cyklu członkostwa w grupach.
- **Polecenia cmdlet programu PowerShell**: umożliwiają wprowadzanie do usługi MIM oraz usługi AD lasu PRIV użytkowników i grup odpowiadających użytkownikom i grupom w lesie CORP, dla administratorów funkcji PAM oraz użytkowników końcowych wymagających przywilejów konta administracyjnego na żądanie.
- **Interfejs API REST i portal przykładowy PAM**: dla deweloperów chcących włączyć program MIM do scenariusza funkcji PAM z uwzględnieniem klientów niestandardowych na potrzeby podnoszenia uprawnień, bez konieczności korzystania z programu PowerShell lub protokołu SOAP. Korzystanie z interfejsu API REST przedstawiono na podstawie przykładowej aplikacji internetowej.

Po zainstalowaniu i skonfigurowaniu każda grupa utworzona w lesie PRIV w procesie migracji jest grupą zabezpieczeń opartą na historii identyfikatora SID (lub w późniejszej aktualizacji systemu Windows Server vNext, grupą obcego podmiotu zabezpieczeń), dublującą grupę SID w oryginalnym lesie CORP. Ponadto członkowie dodani do tych grup w lesie PRIV przez usługę MIM zostaną objęci ograniczeniem czasowym.

W wyniku tego, gdy użytkownik zażąda podniesienia uprawnień za pomocą poleceń cmdlet programu PowerShell, a żądanie zostanie zatwierdzone, usługa MIM doda jego konto w lesie PRIV do grupy w lesie PRIV. Gdy użytkownik zaloguje się za pomocą konta uprzywilejowanego, jego token protokołu Kerberos będzie zawierał identyfikator zabezpieczeń (SID) identyczny z identyfikatorem SID grupy w lesie CORP. Ponieważ las CORP skonfigurowano tak, aby ufał lasowi PRIV, konto z podniesionymi uprawnieniami używane do uzyskania dostępu do zasobu w lesie CORP będzie traktowane przez zasób sprawdzający członkostwo w grupach protokołu Kerberos jako członek grup zabezpieczeń tego zasobu. Odbywa się to przez uwierzytelnianie między lasami za pośrednictwem protokołu Kerberos.

Ponadto członkostwo takie jest ograniczone czasowo, a zatem po upływie wstępnie skonfigurowanego okresu konto administracyjne danego użytkownika przestanie należeć do grupy w lesie PRIV. W związku z tym nie będzie już można używać tego konta w celu uzyskania dostępu do dodatkowych zasobów.
