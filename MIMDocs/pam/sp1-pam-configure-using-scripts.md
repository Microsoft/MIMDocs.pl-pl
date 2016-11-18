---
title: "Konfiguracja usługi PAM za pomocą skryptów"
description: "Przygotowanie domeny CORP z istniejącymi lub nowymi tożsamościami, które mają być zarządzane za pomocą programu Privileged Identity Manager, z użyciem skryptów"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/04/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 3aca2fb513280f118e760bdbc2ba471151c41b17


---

# <a name="configure-pam-using-scripts"></a>Konfiguracja usługi PAM za pomocą skryptów

Jeśli programy SQL i SharePoint zostaną zainstalowane na oddzielnych serwerach, muszą zostać skonfigurowane z użyciem poniższych instrukcji. Jeśli składniki SQL, SharePoint i PAM zostaną zainstalowane na tym samym komputerze, poniższe kroki należy przeprowadzić z poziomu tego komputera.

Poniższy opis kroków zakłada, że domena PRIV jest już skonfigurowana. Instrukcje dotyczące konfigurowania domeny PRIV można znaleźć w uzupełnieniu na końcu dokumentu.

kroki:

1. Rozpakuj skompresowany plik „PAMDeploymentScripts.zip” do folderu %SYSTEMDRIVE%\PAM na wszystkich komputerach.
2. Na dowolnym z komputerów otwórz plik **PAMDeploymentConfig.xml** i zaktualizuj szczegółowe dane przy użyciu poniższej tabeli lub w oparciu o wskazówki zawarte w samym pliku XML. Jeśli lasy CORP i PRIV są już skonfigurowane, wystarczy zaktualizować pozycje **DNSName** i **NetbiosName**.
3. W sekcji Role zaktualizuj dane: **konto usługi**, **szczegóły komputera** i **lokalizacja plików binarnych instalacji** dla ról SQL, SharePoint i MIM.
    1. Lokalizacja plików binarnych MIM musi wskazywać katalog zawierający folder „Service and Portal”. Lokalizacja plików binarnych klienta musi wskazywać katalog zawierający plik „Add-ins and Extensions.msi”.

4. W przypadku środowiska PRIVOnly tag PRIVOnly musi mieć wartość Prawda.
    1. W środowiskach PRIVOnly należy zaktualizować dane **DNSName** i **NetbiosName** domeny PRIV, tak aby pasowały do domeny CORP. Upewnij się, że sufiksy maszyny są poprawne dla maszyn, na których zostaną zainstalowane składniki SQL, SharePoint i MIM, jako że domyślny plik szablonu zakłada konfigurację CORP i PRIV.
    2. Kliknij tutaj, aby uzyskać więcej informacji na temat środowisk PRIVOnly.

5. Skopiuj ten sam plik PAMDeploymentConfig.xml do folderu %SYSTEMDRIVE%\PAM na wszystkich maszynach, kontrolerach domeny CORPDC i PRIVDC oraz serwerach składników PAM, SQL Server i SharePoint.


## <a name="deployment-worksheet"></a>Arkusz wdrażania

Przed przejściem do kolejnych czynności zaktualizuj plik PAMDeploymentConfig.xml i umieść zaktualizowaną kopię na wszystkich komputerach.

### <a name="setup"></a>Setup

|Maszyna   | Do uruchomienia jako   |Polecenia   |
|---|---|---|
|  Kontroler domeny PRIVDC |Administrator domeny PRIV   | .\PAMDeployment.ps1 Wybierz opcję menu 1 (PRIV Forest Configuration (Konfiguracja lasu PRIV)).   |
|   |   |  Powyższy krok pozwala wygenerować plik SIDs.txt. Plik ten należy przed przejściem do następnego kroku skopiować do lokalizacji $envDrive:PAM kontrolera domeny CORPDC. |
| Kontroler domeny CORPDC  |Administrator domeny CORP   | .\PAMDeployment.ps1 Wybierz opcję menu 2 (CORP Forest Configuration (Konfiguracja lasu CORP)).   |
| PAMServer (lub SQL Server)   |Administrator domeny CORP   |  .\PAMDeployment.ps1 Wybierz opcję menu 2 (CORP Forest Configuration (Konfiguracja lasu CORP)).  |
|  PAMServer |  Administrator lokalny (po przyłączeniu do domeny: administrator MIM) |  .\PAMDeployment.ps1 Wybierz opcję menu 4 (SharePoint Setup (Instalator programu SharePoint)).  |
| PAMServer  | Administrator lokalny (po przyłączeniu do domeny: administrator MIM)  | .\PAMDeployment.ps1 Wybierz opcję menu 5 (MIM PAM Setup (Konfiguracja usługi PAM programu MIM)).   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Wybierz opcję menu 6 (PAM Trust Setup (Konfiguracja zaufania usługi PAM)). |

### <a name="validation"></a>Sprawdzanie poprawności

|  Maszyna | Do uruchomienia jako   | Polecenia   |
|---|---|---|
| CORPClient  | Użytkownik domeny CORP (administrator lokalny)  |   .\PAMDeployment.ps1 Wybierz opcję menu 7 (MIM PAM Client Setup (Konfiguracja klienta usługi PAM programu MIM)).  |
| Kontroler domeny CORPDC  | Administrator domeny CORP   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | Użytkownik domeny CORP (administrator lokalny)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | Użytkownik <PRIV>\PRIV.pamRequestor lub — w przypadku środowiska PRIVOnly — użytkownik <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


>[!div class="step-by-step"]
[Początek »](sp1-step1-configuring-priv-domain.md)



<!--HONumber=Nov16_HO2-->


