---
title: Skrypty wdrażania PAM programu MIM
description: Ta strona jest częścią serii artykułów dotyczących konfigurowania Microsoft Identity Manager przy użyciu skryptów. Zawiera listę założeń dotyczących środowiska.
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
ms.openlocfilehash: e7eea2c72df3ca9893acc5f6989afa9c488f384e
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010459"
---
# <a name="mim-pam-deployment-scripts"></a>Skrypty wdrażania PAM programu MIM

W programie MIM 2016 z dodatkiem Service Pack 1 wprowadzono zestaw skryptów wdrażania, które ułatwiają wdrażanie usługi PAM. Te skrypty są dostępne w centrum pobierania. Przed podjęciem próby użycia skryptów należy upewnić się, że spełniasz poniższe wymagania:

1. System operacyjny na wszystkich serwerach ma co najmniej system Windows Server 2012 R2.
2. Usługa DNS musi być skonfigurowana w taki sposób, aby umożliwiała rozpoznawanie nazw między kontrolerami domeny i serwerami składników.
3. Dane binarne instalacji muszą być dostępne lokalnie na wskazanych serwerach na potrzeby instalacji programów SQL, SharePoint i MIM.
4. Środowisko ma trzy dedykowane maszyny (fizyczne lub wirtualne), na których są uruchomione w sposób niezależny kontrolery domeny CORPDC i PRIVDC oraz serwer PAMSERVER.
5. Dla opcji walidacji należy mieć dedykowaną stację roboczą.

>[!NOTE]
>Jeśli napotkasz problem z wykonaniem skryptu, przyjrzyj się dziennikom. Wszystkie dzienniki skryptu są zapisywane w folderze %AppData%\MIMPAMInstall. Skompresuj ten folder do postaci pliku ZIP i dołącz go wraz ze szczegółami operacji i błędu do opisu problemu.

Chcesz rozpocząć pracę ze skryptami wdrażania usługi PAM? Zacznij [konfigurowanie usługi PAM za pomocą skryptów](./pam/sp1-pam-configure-using-scripts.md).
