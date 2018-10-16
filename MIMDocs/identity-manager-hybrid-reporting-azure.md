---
title: Co to jest raportowanie hybrydowe w usłudze Azure AD? | Microsoft Docs
description: Raporty hybrydowe inspekcji aktywności, w usłudze Azure Active Directory umożliwia wyświetlanie zdarzenia inspekcji w chmurze i lokalnych.
keywords: ''
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: 459e1c0c04f28b1ecc1c74f5f672c6318684cc90
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332837"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Hybrydowe Zarządzanie tożsamościami inspekcji raportowania usługi Azure Active Directory
Za pomocą usługi Azure Active Directory (Azure AD) inspekcji raportowaniem aktywności, można monitorować działania związane z zarządzaniem tożsamościami w środowisku lokalnym lub w chmurze. Za zarządzanie wszystkimi danymi tożsamości i dostępu w ramach jednego raportu, można zaoszczędzić czas i zmniejszyć ogólne koszty.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Co to jest raportowanie hybrydowe usługi Azure Active Directory?
Hybrydowe inspekcji raportowania ułatwiająca informatykom wyzwania wspólne zarządzanie tożsamościami raportowania, takie jak:

* **Zbieranie działań związanych z zarządzaniem tożsamościami w różnych systemach**. Raporty hybrydowe zawierają informacje dotyczące działań związanych z zarządzaniem tożsamościami pochodzące z usługi Azure AD i programu Identity Manager.

* **Eksportowanie danych raportów i tworzenie raportów niestandardowych**. Oprócz wyświetlania raportów w witrynie Azure portal, możesz wyeksportować dane w celu generowania własnych widoków niestandardowych.

* **Zmniejszenie kosztów infrastruktury systemu raportowania**. Raportowanie hybrydowe na platformie w chmurze oznacza, że mogą pomóc wyeliminować koszty, które są skojarzone z lokalną, infrastruktury magazynu danych.

## <a name="how-does-it-work"></a>Jak to działa?

Aby zbierać dane lokalne, należy najpierw zainstalować agenta raportowania na serwerze programu Identity Manager 2016. [Pobierz agenta raportowania hybrydowego programu Microsoft Identity Manager](https://www.microsoft.com/download/details.aspx?id=55112).

Raportowanie hybrydowe ulega następujący proces:
1. Po instalacji agenta raportowania dane aktywności programu Identity Manager są wysyłane do dziennika zdarzeń Windows.
2. Agent raportowania przetwarza zdarzenia różnicowe co 10 minut lub po ponownym uruchomieniu usługi dziennika zdarzeń Windows. Agent następnie przekazuje zdarzenia do witryny Azure portal.
3. Witryna Azure portal przetwarza dane odebrane w ciągu godziny od otrzymania go.
4. Dane o aktywności są przechowywane na platformie Azure przez jeden miesiąc.
5. Witryny Azure portal pobiera dane raportowania inspekcji i wyświetla go w oknie raportowania inspekcji platformy Azure.

## <a name="next-steps"></a>Kolejne kroki
Dowiedz się więcej na następujące tematy:
- [Praca z raportowaniem hybrydowym programu Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Raporty dotyczące inspekcji w portalu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Zasady przechowywania raportów](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure log integration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Usługa Azure Active Directory, interfejsu API raportowania](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
