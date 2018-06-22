---
title: Co to jest raportowanie hybrydowe w usłudze Azure AD? | Microsoft Docs
description: Raporty aktywności hybrydowe inspekcji w usłudze Azure Active Directory umożliwia przeglądanie zdarzeń inspekcji w chmurze i lokalnie.
keywords: ''
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: eb9725df484fb5ac2ee44bd9a0423bdb4fbe7e86
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/20/2018
ms.locfileid: "29370382"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Hybrydowe Zarządzanie tożsamościami inspekcji raportowania w usłudze Azure Active Directory
Z usługi Azure Active Directory (Azure AD) inspekcji raportowania aktywności, można monitorować działania związane z zarządzaniem tożsamościami lokalnie lub w chmurze. Zarządzając wszystkich danych tożsamościami i dostępem w jeden raport, można zaoszczędzić czas i obniżenie ogólnych kosztów.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Co to jest raportowanie hybrydowe usługi Azure Active Directory?
Hybrydowe inspekcji raportowania ułatwiająca informatykom wyzwań dotyczących wspólnych Zarządzanie tożsamościami raportowania, takie jak:

* **Zbieranie działania związane z zarządzaniem tożsamościami w różnych systemach**. Raporty hybrydowe zawierają informacje dotyczące działań związanych z zarządzaniem tożsamościami pochodzące z usługi Azure AD i programu Identity Manager.

* **Eksportowanie danych raportowania i tworzenia raportów niestandardowych**. Oprócz wyświetlania raportów w portalu Azure, możesz wyeksportować dane w celu generowania własnych widoków niestandardowych.

* **Zmniejsza koszty infrastruktury systemu raportowania**. Raporty hybrydowe w chmurze oznacza, że może pomóc wyeliminować koszty, które są skojarzone z lokalnymi, infrastruktury magazynu danych.

## <a name="how-does-it-work"></a>Jak to działa?

Aby zbierać dane lokalne, należy najpierw zainstalować agenta raportowania na serwerze programu Identity Manager 2016. [Pobierz agenta raportowania hybrydowego programu Microsoft Identity Manager](https://www.microsoft.com/download/details.aspx?id=55112).

Raportowanie hybrydowe ulega następujący proces:
1. Po zainstalowaniu agenta raportowania dane aktywności programu Identity Manager jest wysyłane do dziennika zdarzeń systemu Windows.
2. Agent raportowania przetwarza zdarzenia różnicowych, co 10 minut lub po ponownym uruchomieniu usługi dziennika zdarzeń systemu Windows. Agent wysyła następnie zdarzenia do portalu Azure.
3. Azure portal przetwarza odebranych danych w ciągu godziny od jego otrzymania.
4. Dane o aktywności są przechowywane na platformie Azure przez jeden miesiąc.
5. Azure portal pobiera dane raportowania inspekcji i wyświetla je w oknie raportów inspekcji usługi Azure.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na następujące tematy:
- [Praca z raportowaniem hybrydowym programu Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Raporty dotyczące działania inspekcji w portalu usługi Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Zasady przechowywania raportowania](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Integracja dziennika Microsoft Azure (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Usługa Azure Active Directory raportowania interfejsu API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
