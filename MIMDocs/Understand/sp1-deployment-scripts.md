---
title: "Skrypty wdrażania usługi PAM w programie MIM2016 SP1"
description: "Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager, z użyciem skryptów"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 43a176ed2f1375eb98851064c460515ec09de132


---

# Skrypty wdrażania usługi PAM w programie MIM2016 SP1

W tym dodatku Service Pack wprowadziliśmy szereg skryptów wdrażania w celu ułatwienia wdrażania usługi PAM. Skrypty te są dostępne w centrum pobierania. Przed podjęciem próby użycia tych skryptów należy upewnić się, że poniższe założenia mają zastosowanie do danego środowiska.

Ważne założenia:
1. System operacyjny na wszystkich komputerach ma co najmniej wersję Windows Server 2012 R2. Jeśli testujesz system Windows Server 2016 Technical Preview 5, na kontrolerze domeny PRIV musi być zainstalowana kompilacja TP5.
2. Usługa DNS musi być skonfigurowana tak, aby rozpoznawanie nazw między kontrolerami domeny i serwerami składników odbywało się automatycznie.
3. Dane binarne instalacji muszą być dostępne lokalnie na wskazanych serwerach na potrzeby instalacji programów SQL, SharePoint i MIM.
4. Środowisko ma trzy dedykowane maszyny (fizyczne lub wirtualne), na których są uruchomione w sposób niezależny kontrolery domeny CORPDC i PRIVDC oraz serwer PAMSERVER.
5. W celu skorzystania z opcji sprawdzania poprawności zakłada się istnienie dedykowanego komputera klienckiego umożliwiającego przeprowadzenie tego kroku.

>[!NOTE] Jeśli napotkasz problem z wykonaniem skryptu, przyjrzyj się dziennikom. Wszystkie dzienniki skryptu są zapisywane w folderze %AppData%\MIMPAMInstall. Skompresuj ten folder do postaci pliku ZIP i wyślij go za pośrednictwem poczty e-mail na adres mim2016@microsoft.com wraz ze szczegółami operacji i błędu.



<!--HONumber=Sep16_HO4-->


