---
title: Co to jest raportowanie hybrydowe | Dokumentacja firmy Microsoft
description: "Raportowanie hybrydowe usługi Active Directory na platformie Azure umożliwia tworzenie niestandardowych raportów, które obejmują zarówno zdarzenia w chmurze, jak i zdarzenia lokalne."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: f21c15fdaa5fba9176cfc60a3c49017fa97fa935


---

# <a name="hybrid-identity-management-reports-in-azure"></a>Raporty hybrydowego zarządzania tożsamościami na platformie Azure
Za pomocą usługi Azure Active Directory (AD) można utworzyć jeden raport umożliwiający monitorowanie działań związanych z zarządzaniem tożsamościami, podejmowanych lokalnie lub w chmurze. Ta funkcja umożliwia zarządzanie wszystkimi tożsamościami i danymi dostępu w jednym miejscu, oszczędzając czas i obniżając ogólne koszty.

## <a name="what-is-azure-ad-hybrid-reporting"></a>Co to jest raportowanie hybrydowe usługi Azure AD?
Raportowanie hybrydowe ułatwia specjalistom IT rozwiązywanie typowych problemów z raportami związanymi z zarządzaniem tożsamościami.

1. **Zbieranie informacji dotyczących działań związanych z zarządzaniem tożsamościami w różnych systemach.** Raporty hybrydowe zawierają informacje dotyczące działań związanych z zarządzaniem tożsamościami pochodzące z usługi Azure AD i programu Identity Manager.

2. **Eksportowanie danych raportów i tworzenie raportów niestandardowych.** Oprócz wyświetlania raportów w portalu platformy Azure można również eksportować dane w celu generowania własnych widoków niestandardowych.

3. **Zmniejszenie kosztów infrastruktury systemu raportowania.** Raporty hybrydowe w chmurze umożliwiają eliminację lokalnej infrastruktury magazynu danych raportowania.

## <a name="how-does-it-work"></a>Jak to działa?

Aby zbierać dane lokalne, należy najpierw zainstalować agenta raportowania na serwerze programu Identity Manager. Agent raportowania jest pobierany ze strony konfiguracji katalogu w [klasycznym portalu platformy Azure](https://manage.windowsazure.com/).

Proces raportowania hybrydowego obejmuje następujące kroki:
1. Po zainstalowaniu agenta raportowania dane o aktywności są przesyłane z programu Identity Manager do dziennika zdarzeń systemu Windows.
2. Agent raportowania przetwarza zdarzenia w dzienniku zdarzeń systemu Windows i przekazuje je do platformy Azure.
3. Dane o aktywności są przechowywane na platformie Azure przez jeden miesiąc.
4. Gdy użytkownik żąda raportu, zdarzenia związane z aktywnością są analizowane i filtrowane pod kątem wymaganych raportów.
5. Klasyczny portal platformy Azure pobiera dane raportowania i renderuje je jako raport aktywności.

## <a name="see-also"></a>Zobacz także
- Dodatkowe informacje na temat [Pracy z raportowaniem hybrydowym programu Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Nov16_HO2-->


