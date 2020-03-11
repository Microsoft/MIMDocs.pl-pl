---
title: Skrypty wdrażania usługi PAM w programie MIM2016 SP1
description: Ta strona należy do serii artykułów na temat konfigurowania programu Privileged Identity Manager za pomocą skryptów. Zawiera listę założeń dotyczących środowiska.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: eeee6473f7471d4c961a4f4d3113d1af73ddaffe
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044416"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Skrypty wdrażania usługi PAM w programie MIM2016 SP1

W tym dodatku Service Pack wprowadziliśmy szereg skryptów wdrażania w celu ułatwienia wdrażania usługi PAM. Skrypty te są dostępne w centrum pobierania. Przed podjęciem próby użycia skryptów ważne jest, aby upewnić się, że spełniasz poniższe wymagania:

1. System operacyjny na wszystkich serwerach ma co najmniej system Windows Server 2012 R2.
2. Usługa DNS musi być skonfigurowana w taki sposób, aby umożliwiała rozpoznawanie nazw między kontrolerami domeny i serwerami składników.
3. Dane binarne instalacji muszą być dostępne lokalnie na wskazanych serwerach na potrzeby instalacji programów SQL, SharePoint i MIM.
4. Środowisko ma trzy dedykowane maszyny (fizyczne lub wirtualne), na których są uruchomione w sposób niezależny kontrolery domeny CORPDC i PRIVDC oraz serwer PAMSERVER.
5. Dla opcji walidacji należy mieć dedykowaną stację roboczą.

>[!NOTE]
>Jeśli napotkasz problem z wykonaniem skryptu, przyjrzyj się dziennikom. Wszystkie dzienniki skryptu są zapisywane w folderze %AppData%\MIMPAMInstall. Skompresuj ten folder do postaci pliku ZIP i dołącz go wraz ze szczegółami operacji i błędu do opisu problemu.

Chcesz rozpocząć pracę ze skryptami wdrażania usługi PAM? Zacznij [konfigurowanie usługi PAM za pomocą skryptów](./pam/sp1-pam-configure-using-scripts.md).
