---
title: Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft
description: "Program MIM obejmuje funkcje zarządzania dostępem programu FIM 2010 oraz pomaga w zarządzaniu użytkownikami, poświadczeniami, zasadami i dostępem w organizacji."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b3cdc1a71b6e9eb14a132429ea66bb4ab33fe3c4
ms.sourcegitcommit: 0a78e39976cd03225a8e24a508e9ee23585e67cc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/11/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016
Program Microsoft Identity Manager (MIM) 2016 korzysta z funkcji zarządzania tożsamościami i dostępem, uwzględnionych w programie [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Podobnie jak jego poprzednik, program MIM ułatwia zarządzanie użytkownikami, poświadczeniami, zasadami i dostępem w organizacji.  Ponadto program MIM 2016 zapewnia obsługę hybrydową, funkcje zarządzania dostępem uprzywilejowanym i obsługę nowych platform.

Ta wersja programu Microsoft Identity Manager udostępnia nowe funkcje, takie jak zarządzanie tożsamościami uprzywilejowanymi i obsługa zarządzania certyfikatami na potrzeby dostępu di interfejsu API REST. Zarządzanie certyfikatami ma teraz obsługę topologii z wieloma lasami, aplikacji dla systemu Windows dla wirtualnej karty inteligentnej i zarządzanie cyklem życia certyfikatów, zaktualizowane zdarzenia oraz funkcje do rozwiązywania problemów. Scenariusze samoobsługi uwzględniają teraz odblokowywanie kont i bramę Azure MFA (uwierzytelniania wieloskładnikowego) do resetowania hasła.

## <a name="hybrid-experience"></a>Obsługa hybrydowa
Program Microsoft Identity Manager 2016 działa równolegle z usługą Azure AD, aby zapewnić kontrolę nad całym środowiskiem użytkownika. Raportowanie hybrydowe w usłudze Azure AD umożliwia wyświetlanie danych z chmury i danych lokalnych w jednym miejscu. Ponadto portal samoobsługowego resetowania haseł obsługuje uwierzytelnianie wieloskładnikowe platformy Azure (MFA).

## <a name="privileged-identity-management"></a>Zarządzanie tożsamościami uprzywilejowanymi
Zarządzanie tożsamościami uprzywilejowanymi umożliwia kontrolowanie dostępu administracyjnego i zarządzanie nim dzięki tymczasowemu, opartemu na zadaniach dostępowi do ważnych zasobów. Oznacza to, że można udzielać użytkownikom tylko niezbędnych uprawnień, co ogranicza ryzyko uzyskania pełnego dostępu administracyjnego przez osobę atakującą. Ponadto zarządzanie tożsamościami uprzywilejowanymi umożliwia wyodrębnianie i izolowanie kont administracyjnych z istniejących lasów usługi Active Directory.

Program MIM obsługuje lokalne rozwiązanie zarządzania tożsamościami uprzywilejowanymi do zarządzania usługą Active Directory. Aby rozpocząć, [skorzystaj z funkcji zarządzania dostępem uprzywilejowanym](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Tematy pokrewne

- Program Microsoft Identity Manager jest nadal ściśle związany ze swoim poprzednikiem, programem Forefront Identity Manager. Jeśli nadal korzystasz z programu FIM lub chcesz zapoznać się z dodatkową dokumentacją, zobacz [plan dokumentacji programu FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Zagadnienia dotyczące topologii wdrożeń programu MIM](topology-considerations.md)
- [Przewodnik planowania pojemności](capacity-planning-guide.md)