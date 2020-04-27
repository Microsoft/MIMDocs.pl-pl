---
title: Konfiguracja usługi PAM za pomocą skryptów
description: Ten artykuł jest częścią serii dotyczącej konfigurowania usługi PAM za pomocą skryptów. Dotyczy on modyfikacji pliku XML, który będzie używany przez skrypty wdrażania usługi PAM.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 07/20/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 102754fc88af32cb9abed40716ba9168a041d58e
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043889"
---
# <a name="configure-pam-using-scripts"></a>Konfiguracja usługi PAM za pomocą skryptów

Jeśli programy SQL i SharePoint zostaną zainstalowane na oddzielnych serwerach, muszą zostać skonfigurowane z użyciem poniższych instrukcji. Jeśli składniki SQL, SharePoint i PAM zostaną zainstalowane na tym samym komputerze, poniższe kroki należy przeprowadzić z poziomu tego komputera.

Poniższy opis kroków zakłada, że domena PRIV jest już skonfigurowana. Instrukcje dotyczące konfigurowania domeny PRIV można znaleźć w uzupełnieniu na końcu dokumentu.

kroki:

1. Pobierać [Skrypty wdrażania usługi PAM](https://www.microsoft.com/download/details.aspx?id=53941)
2. Rozpakuj skompresowany plik „PAMDeploymentScripts.zip” do folderu %SYSTEMDRIVE%\PAM na wszystkich komputerach.
3. Na dowolnym z komputerów otwórz plik **PAMDeploymentConfig.xml** i zaktualizuj szczegółowe dane przy użyciu poniższej tabeli lub w oparciu o wskazówki zawarte w samym pliku XML. Jeśli lasy CORP i PRIV są już skonfigurowane, wystarczy zaktualizować pozycje **DNSName** i **NetbiosName**.
4. W sekcji Role zaktualizuj dane: **konto usługi**, **szczegóły komputera** i **lokalizacja plików binarnych instalacji** dla ról SQL, SharePoint i MIM.
    1. Lokalizacja plików binarnych MIM musi wskazywać katalog zawierający folder „Service and Portal”. Lokalizacja plików binarnych klienta musi wskazywać katalog zawierający plik „Add-ins and Extensions.msi”.

5. W przypadku środowiska PRIVOnly tag PRIVOnly musi mieć wartość Prawda.
    1. W środowiskach PRIVOnly należy zaktualizować dane **DNSName** i **NetbiosName** domeny PRIV, tak aby pasowały do domeny CORP. Upewnij się, że sufiksy maszyny są poprawne dla maszyn, na których zostaną zainstalowane składniki SQL, SharePoint i MIM, jako że domyślny plik szablonu zakłada konfigurację CORP i PRIV.
    2. Kliknij tutaj, aby uzyskać więcej informacji na temat środowisk PRIVOnly.

6. Skopiuj ten sam plik PAMDeploymentConfig.xml do folderu %SYSTEMDRIVE%\PAM na wszystkich maszynach, kontrolerach domeny CORPDC i PRIVDC oraz serwerach składników PAM, SQL Server i SharePoint.


## <a name="deployment-worksheet"></a>Arkusz wdrażania

Przed kontynuowaniem Zaktualizuj plik plik pamdeploymentconfig. XML i umieść zaktualizowaną kopię na wszystkich komputerach.

### <a name="setup"></a>Konfigurowanie

|Maszyna   | Do uruchomienia jako   |Polecenia   |
|---|---|---|
|  Kontroler domeny PRIVDC |Administrator domeny PRIV   | .\PAMDeployment.ps1 Wybierz opcję menu 1 (PRIV Forest Configuration (Konfiguracja lasu PRIV)).   |
|   |   |  Powyższy krok pozwala wygenerować plik SIDs.txt. Plik ten należy przed przejściem do następnego kroku skopiować do lokalizacji $envDrive:PAM kontrolera domeny CORPDC. |
| Kontroler domeny CORPDC  |Administrator domeny CORP   | .\PAMDeployment.ps1 Wybierz opcję menu 2 (CORP Forest Configuration (Konfiguracja lasu CORP)).   |
| PAMServer (lub SQL Server)   |Administrator domeny CORP   |  .\PAMDeployment.ps1 Wybierz opcję menu 2 (CORP Forest Configuration (Konfiguracja lasu CORP)).  |
|  PAMServer |  Administrator lokalny (po przyłączeniu do domeny: administrator MIM) |  .\PAMDeployment.ps1 Wybierz opcję menu 4 (SharePoint Setup (Instalator programu SharePoint)).  |
| PAMServer  | Administrator lokalny (po przyłączeniu do domeny: administrator MIM)  | .\PAMDeployment.ps1 Wybierz opcję menu 5 (MIM PAM Setup (Konfiguracja usługi PAM programu MIM)).   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Wybierz opcję menu 6 (PAM Trust Setup (Konfiguracja zaufania usługi PAM)). |

### <a name="validation"></a>Walidacja

|  Maszyna | Do uruchomienia jako   | Polecenia   |
|---|---|---|
| CORPClient  | Użytkownik domeny CORP (administrator lokalny)  |   .\PAMDeployment.ps1 Wybierz opcję menu 7 (MIM PAM Client Setup (Konfiguracja klienta usługi PAM programu MIM)).  |
| Kontroler domeny CORPDC  | Administrator domeny CORP   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | Użytkownik domeny CORP (administrator lokalny)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | Użytkownik <PRIV>\PRIV.pamRequestor lub — w przypadku środowiska PRIVOnly — użytkownik <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


> [!div class="step-by-step"]
> [Początek »](sp1-step1-configuring-priv-domain.md)
