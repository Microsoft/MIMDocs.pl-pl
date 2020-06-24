---
title: Aktualizacja Azure AD — wersja Premium klientów korzystających z Microsoft Identity Manager | Microsoft Docs
description: W tym artykule opisano, jak Azure AD — wersja Premium klienci mogą uzyskać pomoc techniczną po 21 stycznia 2021.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
reviewer: markwahl-msft
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 26dcaf121c4fd980d296ffee893af3ca66249c6c
ms.sourcegitcommit: ea16fea5d69aff58b862468d4bebfb05100d037a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2020
ms.locfileid: "84749245"
---
# <a name="support-update-for-azure-ad-premium-customers-using-microsoft-identity-manager"></a>Aktualizacja Azure AD — wersja Premium klientów korzystających z Microsoft Identity Manager

Dotyczy: Azure AD — wersja Premium, Microsoft Identity Manager (MIM)

W przypadku klientów korzystających z Azure AD — wersja Premium Standardowa pomoc techniczna jest dostępna od 1 czerwca 2020, kontynuowana po stycznia 2021, dla określonych składników programu [Microsoft Identity Manager 2016 Service Pack 2](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016)lub nowszych dodatkach, które umożliwiają integrację z usługą Azure AD. Jest to dodatek do istniejącej obsługi Microsoft Identity Manager już zapewniane przez [stałe zasady cyklu życia](https://docs.microsoft.com//lifecycle/policies/fixed) i plany [wsparcia dla firm](https://support.microsoft.com/help/4341255).

Składniki programu MIM, dla których jest dostępna pomoc techniczna Standard, obejmują:
- Usługa synchronizacji programu MIM i Usługa powiadamiania o zmianie hasła (PCNS)
- Usługa i Portal programu MIM, dodatki i rozszerzenia, skrypty obsługi magazynu danych i pakiety językowe
- Łączniki programu MIM

Te składniki programu MIM wypełniają Active Directory i rozszerzają usługę Azure AD za pośrednictwem Azure AD Connect, z użytkownikami i grupami, które są obsługiwane z lokalnego systemu kadr lub innego systemu źródeł rekordów. Dzięki temu klienci korzystający z Azure AD — wersja Premium z systemami lokalnymi mogą nadal być obsługiwani podczas migracji scenariuszy zarządzania tożsamościami z systemów lokalnych do usługi Azure AD. 

## <a name="opening-a-support-request-in-the-azure-portal"></a>Otwieranie żądania obsługi w Azure Portal

Jako dodatkowa opcja pomocy technicznej dla Microsoft Identity Manager klienci Azure AD — wersja Premium mogą zażądać pomocy technicznej dotyczącej powyższych składników programu Microsoft Identity Manager 2016 z dodatkiem Service Pack 2 lub nowszej poprawki lub aktualizacji za pomocą Azure Portal.

Klient może utworzyć żądanie pomocy technicznej platformy Azure, korzystając z instrukcji przedstawionych na stronie [jak utworzyć żądanie pomocy technicznej platformy Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request):
1. Wybierz *typ problemu: techniczne*
1. Przełącz, aby wyświetlić *wszystkie usługi*
1. na liście usługi w obszarze Azure Active Directory wybierz opcję *aprowizacji i synchronizacja użytkowników*
1. Wybierz *typ problemu: Microsoft Identity Manager (MIM)*
1. Wybierz *podtyp problemu*: *Łączniki*, *Usługa i Portal* lub *aparat synchronizacji*

![Utwórz Support request MIM](media/azure-active-directory-new-support-request.png)

Składniki programu MIM są wyświetlane jako typy problemów w *Azure Active Directory aprowizacji i synchronizacji użytkowników* w Azure Portal.

W przypadku żądań otwartych za pomocą Azure Portal pomoc techniczna Standard jest dostępna dla Azure AD — wersja Premium klientów w przypadku następujących składników programu Microsoft Identity Manager 2016 z dodatkiem Service Pack 2 lub [nowszej poprawki lub aktualizacji](reference/version-history.md): usługa synchronizacji, Usługa powiadamiania o zmianie hasła (PCNS), łączniki, usługa i portal, dodatki i rozszerzenia, skrypty obsługi magazynu danych i pakiety językowe.

## <a name="other-support-options"></a>Inne opcje pomocy technicznej

Program MIM 2016 z dodatkiem SP2, kompilacja 4.6.34.0, został opublikowany w październiku 2019. Zaleca się, aby klienci korzystali z w pełni obsługiwanego dodatku Service Pack, aby upewnić się, że znajdują się one w najnowszej i najbezpieczniejszej wersji produktu. Aby uzyskać więcej informacji, zapoznaj się z [zasadami cyklu życia dodatku Service Pack](https://support.microsoft.com/help/17138).

W przypadku klientów nadal korzystających ze starszej kompilacji programu MIM lub klientów, którzy nie mają pomocy technicznej lub subskrypcji platformy Azure w ramach zestawu, który zawiera Azure Active Directory — wersja Premium lub w przypadku problemów z innymi składnikami programu MIM, które nie są wymienione powyżej, pomoc techniczna będzie nadal dostępna. Zasady pomocy technicznej są opisane w [zasadach dotyczących stałych cyklów życia](https://docs.microsoft.com/lifecycle/policies/fixed) z określonymi datami w [cyklu pomocy technicznej dla Microsoft Identity Manager 2016](https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016).

Oprócz pomocy technicznej systemu Azure istnieje kilka innych opcji pomocy technicznej, które mogą być używane w celu uzyskania pomocy technicznej. Jeśli na przykład masz pomoc techniczną firmy Microsoft, możesz [utworzyć nowe żądanie pomocy technicznej](https://support.microsoft.com/supportforbusiness/productselection). Aby wybrać odpowiedni składnik programu MIM:
1. Wybierz *zabezpieczenia* rodziny produktów
1. Wybieranie produktu *Identity Manager 2016*
