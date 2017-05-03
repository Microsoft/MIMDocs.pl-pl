---
title: Co to jest raportowanie hybrydowe | Dokumentacja firmy Microsoft
description: "Raporty hybrydowe działania inspekcji w usłudze Azure Active Directory pozwalają przeglądać poddane inspekcji zdarzenia w chmurze i lokalne."
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Raporty hybrydowe inspekcji zarządzania tożsamościami w usłudze Azure Active Directory
Dzięki raportom działania inspekcji usługi Azure Active Directory (AD) można przeglądać jeden raport umożliwiający monitorowanie działań związanych z zarządzaniem tożsamościami, podejmowanych lokalnie lub w chmurze. Ta funkcja umożliwia zarządzanie wszystkimi tożsamościami i danymi dostępu w jednym miejscu, oszczędzając czas i obniżając ogólne koszty.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Co to jest raportowanie hybrydowe usługi Azure Active Directory?
Raportowanie hybrydowe inspekcji ułatwia specjalistom IT rozwiązywanie typowych problemów z raportami związanymi z zarządzaniem tożsamościami.

1. **Zbieranie informacji dotyczących działań związanych z zarządzaniem tożsamościami w różnych systemach.** Raporty hybrydowe zawierają informacje dotyczące działań związanych z zarządzaniem tożsamościami pochodzące z usługi Azure AD i programu Identity Manager.

2. **Eksportowanie danych raportów i tworzenie raportów niestandardowych.** Oprócz wyświetlania raportów w portalu platformy Azure można również eksportować dane w celu generowania własnych widoków niestandardowych.

3. **Zmniejszenie kosztów infrastruktury systemu raportowania.** Raporty hybrydowe w chmurze umożliwiają eliminację lokalnej infrastruktury magazynu danych raportowania.

## <a name="how-does-it-work"></a>Jak to działa?

Aby zbierać dane lokalne, należy najpierw zainstalować agenta raportowania na serwerze programu Identity Manager 2016. Agenta raportowania można pobrać z Centrum pobierania Microsoft [tutaj](https://www.microsoft.com/en-us/download/details.aspx?id=####/).

Proces raportowania hybrydowego obejmuje następujące kroki:
1. Po zainstalowaniu agenta raportowania dane o aktywności są przesyłane z programu Identity Manager do dziennika zdarzeń systemu Windows.
2. Agent raportowania przetwarza zdarzenia w dzienniku zdarzeń systemu Windows i przekazuje je do witryny Azure Portal.
3. Dane o aktywności są przechowywane na platformie Azure przez jeden miesiąc.
4. Gdy użytkownik żąda raportu, zdarzenia związane z aktywnością są analizowane i filtrowane pod kątem wymaganych raportów.
5. Witryna Azure Portal pobiera dane raportowania inspekcji i renderuje je jako inspekcję w ramach raportu działania.

## <a name="see-also"></a>Zobacz także
- Dodatkowe informacje na temat [Pracy z raportowaniem hybrydowym programu Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)
- Uzyskaj więcej szczegółowych informacji na temat [raportów działania inspekcji w portalu usługi Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)

