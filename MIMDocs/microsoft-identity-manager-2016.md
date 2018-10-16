---
title: Microsoft Identity Manager 2016 | Dokumentacja firmy Microsoft
description: Program MIM obejmuje funkcje zarządzania dostępem programu FIM 2010 oraz pomaga w zarządzaniu użytkownikami, poświadczeniami, zasadami i dostępem w organizacji.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de00cc6da8c431519c9bdef3a2e97a8333cbb4ac
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332293"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Program Microsoft Identity Manager (MIM) 2016 korzysta z funkcji zarządzania tożsamościami i dostępem, uwzględnionych w programie [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Podobnie jak jego poprzednik, program MIM ułatwia zarządzanie użytkownikami, poświadczeniami, zasadami i dostępem w organizacji.  Ponadto program MIM 2016 zapewnia obsługę hybrydową, funkcje zarządzania dostępem uprzywilejowanym i obsługę nowych platform.

Oprócz istniejące funkcje zarządzania tożsamościami, objęte [FIM](https://technet.microsoft.com/library/jj133868). Program MIM 2016 zawiera nowe funkcje i ulepszenia, takich jak:

- Zarządzanie tożsamościami uprzywilejowanymi
- Nowa funkcja Zarządzanie certyfikatami
  - [Dokumentacja interfejsu API REST zarządzania certyfikatami](./reference/certificate-management-rest-api-reference.md)
  - Obsługę topologii z wieloma lasami.
  - [Aplikacja Windows, dla wirtualnej karty inteligentnej](working-with-mim-certificate-manager.md)
  - Zaktualizowane zdarzenia oraz funkcje do rozwiązywania problemów. 
- [Scenariusze samoobsługi](working-with-self-service-password-reset.md) uwzględniają teraz odblokowywanie kont i usługi Azure MFA bramy (uwierzytelniania wieloskładnikowego) do resetowania hasła.

## <a name="hybrid-experience"></a>Obsługa hybrydowa

Microsoft Identity Manager 2016 działa równolegle [usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) aby zapewnić kontrolę nad całym środowiskiem użytkownika. Raportowanie hybrydowe w usłudze Azure AD umożliwia wyświetlanie danych z chmury i danych lokalnych w jednym miejscu. Ponadto [portalu samoobsługowego resetowania hasła](working-with-self-service-password-reset.md) obsługuje usługa Azure Multi-Factor authentication (MFA).

## <a name="privileged-identity-management"></a>Zarządzanie tożsamościami uprzywilejowanymi

Zarządzanie tożsamościami uprzywilejowanymi umożliwia kontrolowanie dostępu administracyjnego i zarządzanie nim dzięki tymczasowemu, opartemu na zadaniach dostępowi do ważnych zasobów. Oznacza to, że można udzielać użytkownikom tylko niezbędnych uprawnień, co ogranicza ryzyko uzyskania pełnego dostępu administracyjnego przez osobę atakującą. Ponadto zarządzanie tożsamościami uprzywilejowanymi umożliwia wyodrębnianie i izolowanie kont administracyjnych z istniejących lasów usługi Active Directory.

Program MIM obsługuje lokalne rozwiązanie zarządzania tożsamościami uprzywilejowanymi do zarządzania usługą Active Directory. Aby rozpocząć, [skorzystaj z funkcji zarządzania dostępem uprzywilejowanym](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Tematy pokrewne

- Program Microsoft Identity Manager jest nadal ściśle związany ze swoim poprzednikiem, programem Forefront Identity Manager. Jeśli nadal korzystasz z programu FIM lub chcesz zapoznać się z dodatkową dokumentacją, zobacz [plan dokumentacji programu FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Zagadnienia dotyczące wdrażania programu MIM topologii](topology-considerations.md) w tym artykule przedstawiono różne topologie wdrożenia, które można rozważyć wykonania.
- [Przewodnik planowania pojemności](capacity-planning-guide.md) można użyć tego przewodnika oraz środowisk testowych, aby zrozumieć odpowiedni zakres dla danego wdrożenia.