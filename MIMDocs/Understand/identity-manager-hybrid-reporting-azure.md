---
title: "Raporty hybrydowego zarządzania tożsamościami | Microsoft Identity Manager"
description: "Raportowanie hybrydowe usługi Active Directory na platformie Azure umożliwia tworzenie niestandardowych raportów, które obejmują zarówno zdarzenia w chmurze, jak i zdarzenia lokalne."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 7e61e201b277a2e8ec9fee785e9e34fca3b1cb29
ms.openlocfilehash: b3f3982ade46932b18fb730fe5c16d52cde188a1


---

# Raporty hybrydowego zarządzania tożsamościami na platformie Azure
Za pomocą usługi Azure Active Directory (AD) można utworzyć jeden raport umożliwiający monitorowanie działań związanych z zarządzaniem tożsamościami, podejmowanych lokalnie lub w chmurze. Ta funkcja umożliwia zarządzanie wszystkimi tożsamościami i danymi dostępu w jednym miejscu, oszczędzając czas i obniżając ogólne koszty.

## Co to jest raportowanie hybrydowe usługi Azure AD?
Raportowanie hybrydowe ułatwia specjalistom IT rozwiązywanie typowych problemów z raportami związanymi z zarządzaniem tożsamościami.

1. **Zbieraj informacje dotyczące działań związanych z zarządzaniem tożsamościami w różnych systemach.** Raporty hybrydowe zawierają informacje dotyczące działań związanych z zarządzaniem tożsamościami pochodzące z usługi Azure AD i programu Identity Manager.

2. **Eksportuj dane raportów i twórz raporty niestandardowe.** Oprócz wyświetlania raportów w portalu platformy Azure można również eksportować dane w celu generowania własnych widoków niestandardowych.

3. **Zmniejsz koszty infrastruktury systemu raportowania.** Raporty hybrydowe w chmurze umożliwiają eliminację lokalnej infrastruktury magazynu danych raportowania.

## Jak to działa?

Aby zbierać dane lokalne, należy najpierw zainstalować agenta raportowania na serwerze programu Identity Manager. Agent raportowania jest pobierany ze strony konfiguracji katalogu w [klasycznym portalu platformy Azure](https://manage.windowsazure.com/).

Proces raportowania hybrydowego obejmuje następujące kroki:
1. Po zainstalowaniu agenta raportowania dane o aktywności są przesyłane z programu Identity Manager do dziennika zdarzeń systemu Windows.
2. Agent raportowania przetwarza zdarzenia w dzienniku zdarzeń systemu Windows i przekazuje je do platformy Azure.
3. Dane o aktywności są przechowywane na platformie Azure przez jeden miesiąc.
4. Gdy użytkownik żąda raportu, zdarzenia związane z aktywnością są analizowane i filtrowane pod kątem wymaganych raportów.
5. Klasyczny portal platformy Azure pobiera dane raportowania i renderuje je jako raport aktywności.

## Zobacz też
- Dodatkowe informacje na temat [Pracy z raportowaniem hybrydowym programu Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Jun16_HO4-->

