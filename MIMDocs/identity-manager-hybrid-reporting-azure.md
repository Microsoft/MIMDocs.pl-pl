---
title: Co to jest raportowanie hybrydowe w usłudze Azure AD? | Dokumentacja firmy Microsoft
description: Raporty dotyczące działań inspekcji hybrydowej w programie Azure Active Directory umożliwiają wyświetlanie zdarzeń poddawanych inspekcji zarówno w chmurze, jak i lokalnie.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: dd87f00fb3faded60671a47a0ba1dab7e4c2a531
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516774"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Raportowanie inspekcji hybrydowej zarządzania tożsamościami w Azure Active Directory
Dzięki funkcji raportowania aktywności usługi Azure Active Directory (Azure AD) można monitorować aktywność zarządzania tożsamościami lokalnie lub w chmurze. Dzięki zarządzaniu wszystkimi tożsamościami i dostępem do danych w jednym raporcie można zaoszczędzić czas i zmniejszyć koszty ogólne.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Co to jest raportowanie hybrydowe usługi Azure Active Directory?
Raportowanie inspekcji hybrydowej pomaga specjalistom IT w rozwiązywaniu typowych wyzwań raportowania zarządzania tożsamościami, takich jak:

* **Zbieranie działań związanych z zarządzaniem tożsamościami w różnych systemach**. Raporty hybrydowe zawierają informacje dotyczące działań związanych z zarządzaniem tożsamościami pochodzące z usługi Azure AD i programu Identity Manager.

* **Eksportowanie danych raportowania i tworzenie raportów niestandardowych**. Oprócz wyświetlania raportów w Azure Portal można eksportować dane w celu wygenerowania własnych widoków niestandardowych.

* **Zmniejszenie kosztu infrastruktury systemu raportowania**. Raportowanie hybrydowe w chmurze oznacza, że można pomóc wyeliminować koszty związane z infrastrukturą magazynu danych lokalnych.

## <a name="how-does-it-work"></a>Jak to działa?

Aby zbierać dane lokalne, należy najpierw zainstalować agenta raportowania na serwerze programu Identity Manager 2016. [Pobierz Microsoft Identity Manager agenta raportowania hybrydowego](https://www.microsoft.com/download/details.aspx?id=55112).

Raportowanie hybrydowe jest następujące:
1. Po zainstalowaniu agenta raportowania dane o aktywności programu Identity Manager są wysyłane do dziennika zdarzeń systemu Windows.
2. Agent raportowania przetwarza zdarzenia różnicowe co 10 minut lub po ponownym uruchomieniu usługi dziennika zdarzeń systemu Windows. Następnie Agent przekazuje zdarzenia do Azure Portal.
3. Azure Portal przetwarza odebrane dane w ciągu godziny od momentu ich otrzymania.
4. Dane o aktywności są przechowywane na platformie Azure przez jeden miesiąc.
5. Azure Portal pobiera dane raportowania inspekcji i wyświetla je w oknie raportowanie inspekcji platformy Azure.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na następujące tematy:
- [Praca z raportowaniem hybrydowym programu Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Raporty dotyczące inspekcji w portalu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Raportowanie zasad przechowywania](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Integracja dzienników Microsoft Azure (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Interfejs API raportowania Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
