---
title: "Skrypty wdrażania usługi PAM w programie MIM2016 SP1"
description: "Ta strona należy do serii artykułów na temat konfigurowania programu Privileged Identity Manager za pomocą skryptów. Zawiera listę założeń dotyczących środowiska."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 77a222c0a36f4e244a5114eddfc0edadb168d1cd
ms.sourcegitcommit: 06add1a636720f74bc0c0f25b4100b19f1bd31da
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Skrypty wdrażania usługi PAM w programie MIM2016 SP1

W tym dodatku Service Pack wprowadziliśmy szereg skryptów wdrażania w celu ułatwienia wdrażania usługi PAM. Skrypty te są dostępne w centrum pobierania. Przed podjęciem próby użycia skryptów ważne jest, że należy upewnić się, że spełniają poniższe wymagania:

1. System operacyjny na wszystkich serwerach jest co najmniej Windows Server 2012 R2.
2. Aby włączyć rozpoznawanie nazw między kontrolerami domeny i serwery składników należy skonfigurować DNS.
3. Dane binarne instalacji muszą być dostępne lokalnie na wskazanych serwerach na potrzeby instalacji programów SQL, SharePoint i MIM.
4. Środowisko ma trzy dedykowane maszyny (fizyczne lub wirtualne), na których są uruchomione w sposób niezależny kontrolery domeny CORPDC i PRIVDC oraz serwer PAMSERVER.
5. Dla opcji weryfikacji, musisz mieć dedykowanych stacji roboczej.

>[!NOTE]
>Jeśli napotkasz problem z wykonaniem skryptu, przyjrzyj się dziennikom. Wszystkie dzienniki skryptu są zapisywane w folderze %AppData%\MIMPAMInstall. Skompresuj ten folder do postaci pliku ZIP i dołącz go wraz ze szczegółami operacji i błędu do opisu problemu.

Chcesz rozpocząć pracę ze skryptami wdrażania usługi PAM? Zacznij [konfigurowanie usługi PAM za pomocą skryptów](./pam/sp1-pam-configure-using-scripts.md).
