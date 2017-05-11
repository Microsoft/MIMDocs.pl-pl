---
title: Co to jest raportowanie hybrydowe | Dokumentacja firmy Microsoft
description: "Raporty hybrydowe działania inspekcji w usłudze Azure Active Directory pozwalają przeglądać poddane inspekcji zdarzenia w chmurze i lokalne."
keywords: 
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 2869e908f3ff2d8038441576900e2f4ff5ac3201
ms.openlocfilehash: 30d20bf342a897179e120bc763f47106da631cb6
ms.contentlocale: pl-pl
ms.lasthandoff: 04/28/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh"></a>Raporty hybrydowe inspekcji zarządzania tożsamościami w usłudze Azure Active Directory — publiczna wersja zapoznawcza (odświeżanie)
Dzięki raportom działania inspekcji usługi Azure Active Directory (AD) można przeglądać jeden raport umożliwiający monitorowanie działań związanych z zarządzaniem tożsamościami, podejmowanych lokalnie lub w chmurze. Ta funkcja umożliwia zarządzanie wszystkimi tożsamościami i danymi dostępu w jednym miejscu, oszczędzając czas i obniżając ogólne koszty.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Co to jest raportowanie hybrydowe usługi Azure Active Directory?
Raportowanie hybrydowe inspekcji ułatwia specjalistom IT rozwiązywanie typowych problemów z raportami związanymi z zarządzaniem tożsamościami.

1. **Zbieranie informacji dotyczących działań związanych z zarządzaniem tożsamościami w różnych systemach.** Raporty hybrydowe zawierają informacje dotyczące działań związanych z zarządzaniem tożsamościami pochodzące z usługi Azure AD i programu Identity Manager.

2. **Eksportowanie danych raportów i tworzenie raportów niestandardowych.** Oprócz wyświetlania raportów w portalu platformy Azure można również eksportować dane w celu generowania własnych widoków niestandardowych.

3. **Zmniejszenie kosztów infrastruktury systemu raportowania.** Raporty hybrydowe w chmurze umożliwiają eliminację lokalnej infrastruktury magazynu danych raportowania.

## <a name="how-does-it-work"></a>Jak to działa?

Aby zbierać dane lokalne, należy najpierw zainstalować agenta raportowania na serwerze programu Identity Manager 2016. Agenta raportowania można pobrać z Centrum pobierania Microsoft [tutaj](https://www.microsoft.com/en-us/download/details.aspx?id=55112).

Proces raportowania hybrydowego obejmuje następujące kroki:
1. Po zainstalowaniu agenta raportowania dane o aktywności są przesyłane z programu Identity Manager do dziennika zdarzeń systemu Windows.
2. Agent raportowania przetwarza zdarzenia różnicowe co 10 minut lub po ponownym uruchomieniu usługi w dzienniku zdarzeń systemu Windows i przekazuje je do witryny Azure Portal.
3. Witryna Azure Portal przetwarza dane w ciągu 1 godziny od ich odebrania.
4. Dane o aktywności są przechowywane na platformie Azure przez jeden miesiąc.
5. Witryna Azure Portal pobiera dane raportowania inspekcji i renderuje je jako inspekcję w bloku raportowania inspekcji platformy Azure.

## <a name="see-also"></a>Zobacz także
- Dodatkowe informacje na temat [Pracy z raportowaniem hybrydowym programu Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)
- Uzyskaj więcej szczegółowych informacji na temat [raportów działania inspekcji w portalu usługi Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)
