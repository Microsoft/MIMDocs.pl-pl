---
title: Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft
description: "Program MIM obejmuje funkcje zarządzania dostępem programu FIM 2010 oraz pomaga w zarządzaniu użytkownikami, poświadczeniami, zasadami i dostępem w organizacji."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 21bb12a70850a5f835ca6715d9683558ac6fad1d
ms.sourcegitcommit: f2778c5fa5f0cd04e8a74fc15fa340cd118dded5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Program Microsoft Identity Manager (MIM) 2016 korzysta z funkcji zarządzania tożsamościami i dostępem, uwzględnionych w programie [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Podobnie jak jego poprzednik, program MIM ułatwia zarządzanie użytkownikami, poświadczeniami, zasadami i dostępem w organizacji.  Ponadto program MIM 2016 zapewnia obsługę hybrydową, funkcje zarządzania dostępem uprzywilejowanym i obsługę nowych platform.

Oprócz istniejące funkcje zarządzania tożsamościami objęte [FIM](https://technet.microsoft.com/library/jj133868). Program MIM 2016 udostępnia nowe funkcje i ulepszenia, takie jak:

- Zarządzanie tożsamościami uprzywilejowanymi
- Nowe funkcje w funkcji Zarządzanie certyfikatami
  - [Dokumentacja interfejsu API REST zarządzania certyfikatami](./reference/certificate-management-rest-api-reference.md)
  - Obsługę topologii z wieloma lasami.
  - Dla wirtualnej karty inteligentnej aplikacji systemu Windows
  - Zaktualizowane zdarzenia oraz funkcje do rozwiązywania problemów. 
- Scenariusze samoobsługi uwzględniają teraz odblokowywanie kont i bramę Azure MFA (uwierzytelniania wieloskładnikowego) do resetowania hasła.

## <a name="hybrid-experience"></a>Obsługa hybrydowa

Program Microsoft Identity Manager 2016 działa równolegle z [usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) umożliwiają kontroli nad całym środowiskiem użytkownika. Raportowanie hybrydowe w usłudze Azure AD umożliwia wyświetlanie danych z chmury i danych lokalnych w jednym miejscu. Ponadto [portalu samoobsługowego resetowania hasła](working-with-self-service-password-reset.md) obsługuje uwierzytelnianie wieloskładnikowe platformy Azure (MFA).

## <a name="privileged-identity-management"></a>Zarządzanie tożsamościami uprzywilejowanymi

Zarządzanie tożsamościami uprzywilejowanymi umożliwia kontrolowanie dostępu administracyjnego i zarządzanie nim dzięki tymczasowemu, opartemu na zadaniach dostępowi do ważnych zasobów. Oznacza to, że można udzielać użytkownikom tylko niezbędnych uprawnień, co ogranicza ryzyko uzyskania pełnego dostępu administracyjnego przez osobę atakującą. Ponadto zarządzanie tożsamościami uprzywilejowanymi umożliwia wyodrębnianie i izolowanie kont administracyjnych z istniejących lasów usługi Active Directory.

Program MIM obsługuje lokalne rozwiązanie zarządzania tożsamościami uprzywilejowanymi do zarządzania usługą Active Directory. Aby rozpocząć, [skorzystaj z funkcji zarządzania dostępem uprzywilejowanym](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Tematy pokrewne

- Program Microsoft Identity Manager jest nadal ściśle związany ze swoim poprzednikiem, programem Forefront Identity Manager. Jeśli nadal korzystasz z programu FIM lub chcesz zapoznać się z dodatkową dokumentacją, zobacz [plan dokumentacji programu FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Zagadnienia dotyczące topologii wdrożeń MIM](topology-considerations.md) w tym artykule przedstawiono różne topologie wdrożenia, które można zastosować.
- [Przewodnik planowania pojemności](capacity-planning-guide.md) można użyć tego przewodnika oraz środowisk testowych, aby zrozumieć odpowiedni zakres wdrożenia.