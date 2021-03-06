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
ms.openlocfilehash: e3e248092f674a031e255eb368681ab1b6dc21df
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010697"
---
# <a name="understand-the-components-of-mim-pam"></a>Informacje o składnikach modułu PAM programu MIM

Privileged Access Management utrzymuje dostęp administracyjny niezależnie od codziennych kont użytkowników przy użyciu oddzielnego lasu.

> [!NOTE]
> Podejście PAM udostępnione przez program MIM jest przeznaczone do użycia w niestandardowej architekturze dla izolowanych środowisk, w których dostęp do Internetu nie jest dostępny, gdy ta konfiguracja jest wymagana przez regulację lub w środowiskach izolowanych o dużym wpływie, takich jak laboratoria badawcze w trybie offline i rozłączona technologia kontroli i środowiska pozyskiwania danych. Jeśli Active Directory jest częścią środowiska połączonego z Internetem, zapoznaj się z tematem [Zabezpieczanie uprzywilejowanego dostępu](/security/compass/overview) , aby uzyskać więcej informacji na temat lokalizacji do uruchomienia.

 To rozwiązanie opiera się na równoległych lasach:

- *CORP*: las firmowy ogólnego przeznaczenia, zawierający co najmniej jedną domenę. Można mieć większą liczbę lasów CORP, natomiast w przykładach zawartych w tych artykułach dla uproszczenia przyjęto założenie, że występuje jeden taki las z jedną domeną.  
- *PRIV*: dedykowany las utworzony specjalnie na potrzeby tego scenariusza funkcji PAM. Ten las zawiera jedną domenę obsługującą grupy uprzywilejowane oraz konta kopiowane z co najmniej jednej domeny z lasu CORP.

Rozwiązanie MIM skonfigurowane na potrzeby funkcji PAM zawiera następujące składniki:  

- **Usługa MIM**: implementuje logikę biznesową wykonywania operacji zarządzania tożsamością i dostępem, w tym zarządzania kontami uprzywilejowanymi i obsługi żądań podniesienia uprawnień.
- **Portal programu MIM**: Portal oparty na programie SharePoint hostowany przez program SharePoint 2013 lub nowszy, który udostępnia interfejs użytkownika służący do zarządzania i konfigurowania administratora.
- **Baza danych usługi MIM**: przechowywana w SQL Server 2012 lub nowszej oraz zawiera dane tożsamości i metadane wymagane przez usługę MIM.
- **Usługa monitorowania PAM** oraz **usługa składników PAM**: dwie usługi służące do zarządzania kontami uprzywilejowanymi i wspomagające usługę AD lasu PRIV w cyklu członkostwa w grupach.
- **Polecenia cmdlet programu PowerShell**: umożliwiają wprowadzanie do usługi MIM oraz usługi AD lasu PRIV użytkowników i grup odpowiadających użytkownikom i grupom w lesie CORP, dla administratorów funkcji PAM oraz użytkowników końcowych wymagających przywilejów konta administracyjnego na żądanie.
- **Interfejs API REST i portal przykładowy PAM**: dla deweloperów chcących włączyć program MIM do scenariusza funkcji PAM z uwzględnieniem klientów niestandardowych na potrzeby podnoszenia uprawnień, bez konieczności korzystania z programu PowerShell lub protokołu SOAP. Korzystanie z interfejsu API REST przedstawiono na podstawie przykładowej aplikacji internetowej.

Po zainstalowaniu i skonfigurowaniu każda grupa utworzona przez procedurę migracji w lesie PRIV jest zagraniczną grupą podmiotu zabezpieczeń dublowania grupy w oryginalnym lesie CORP. Obca Grupa główna zapewnia użytkownikom, którzy są członkami tej grupy, o takim samym identyfikatorze SID w tokenie Kerberos, jako identyfikator SID grupy w lesie CORP. Ponadto członkowie dodani do tych grup w lesie PRIV przez usługę MIM zostaną objęci ograniczeniem czasowym.

W wyniku tego, gdy użytkownik zażąda podniesienia uprawnień za pomocą poleceń cmdlet programu PowerShell, a żądanie zostanie zatwierdzone, usługa MIM doda jego konto w lesie PRIV do grupy w lesie PRIV. Gdy użytkownik zaloguje się za pomocą konta uprzywilejowanego, jego token protokołu Kerberos będzie zawierał identyfikator zabezpieczeń (SID) identyczny z identyfikatorem SID grupy w lesie CORP. Ponieważ las CORP skonfigurowano tak, aby ufał lasowi PRIV, konto z podniesionymi uprawnieniami używane do uzyskania dostępu do zasobu w lesie CORP będzie traktowane przez zasób sprawdzający członkostwo w grupach protokołu Kerberos jako członek grup zabezpieczeń tego zasobu. Odbywa się to przez uwierzytelnianie między lasami za pośrednictwem protokołu Kerberos.

Ponadto członkostwo takie jest ograniczone czasowo, a zatem po upływie wstępnie skonfigurowanego okresu konto administracyjne danego użytkownika przestanie należeć do grupy w lesie PRIV. W związku z tym nie będzie już można używać tego konta w celu uzyskania dostępu do dodatkowych zasobów.
