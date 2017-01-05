---
title: "Skrypty wdrażania usługi PAM w programie MIM2016 SP1"
description: "Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager, z użyciem skryptów"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 2bf445f2103c2793d613e001f15e06bdc8b9ad54


---

# <a name="mim2016-sp1-pam-deployment-scripts"></a>Skrypty wdrażania usługi PAM w programie MIM2016 SP1

W tym dodatku Service Pack wprowadziliśmy szereg skryptów wdrażania w celu ułatwienia wdrażania usługi PAM. Skrypty te są dostępne w centrum pobierania. Przed podjęciem próby użycia tych skryptów należy upewnić się, że poniższe założenia mają zastosowanie do danego środowiska.

Ważne założenia:
1. System operacyjny na wszystkich komputerach ma co najmniej wersję Windows Server 2012 R2. Jeśli testujesz system Windows Server 2016 Technical Preview 5, na kontrolerze domeny PRIV musi być zainstalowana kompilacja TP5.
2. Usługa DNS musi być skonfigurowana tak, aby rozpoznawanie nazw między kontrolerami domeny i serwerami składników odbywało się automatycznie.
3. Dane binarne instalacji muszą być dostępne lokalnie na wskazanych serwerach na potrzeby instalacji programów SQL, SharePoint i MIM.
4. Środowisko ma trzy dedykowane maszyny (fizyczne lub wirtualne), na których są uruchomione w sposób niezależny kontrolery domeny CORPDC i PRIVDC oraz serwer PAMSERVER.
5. W celu skorzystania z opcji sprawdzania poprawności zakłada się istnienie dedykowanego komputera klienckiego umożliwiającego przeprowadzenie tego kroku.

>[!NOTE]
>Jeśli napotkasz problem z wykonaniem skryptu, przyjrzyj się dziennikom. Wszystkie dzienniki skryptu są zapisywane w folderze %AppData%\MIMPAMInstall. Skompresuj ten folder do postaci pliku ZIP i wyślij go za pośrednictwem poczty e-mail na adres mim2016@microsoft.com wraz ze szczegółami operacji i błędu.

Chcesz rozpocząć pracę ze skryptami wdrażania usługi PAM? Zacznij [konfigurowanie usługi PAM za pomocą skryptów](/microsoft-identity-manager/pam/sp1-pam-configure-using-scripts).



<!--HONumber=Jan17_HO1-->


