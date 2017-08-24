---
title: "Krok 8 — Weryfikacja wdrożenia usługi PAM"
description: "Wdrożenie usługi PAM inicjowane przez skrypty obejmuje skrypty weryfikacji, które umożliwiają wykonanie scenariusza PAM w celu potwierdzenia prawidłowego działania wdrożenia usługi PAM."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 28fcbe10366df749796be76f83f608561b5f39d3
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/19/2017
---
# <a name="step-8-pam-deployment-verification"></a>Krok 8 — Weryfikacja wdrożenia usługi PAM

>[!div class="step-by-step"]
[« Krok 7](sp1-step7-setup-sidhistory-sidfiltering.md)
[Dodatek »](sp1-pam-deployment-addendum.md)

Pakiet wdrożeniowy zawiera skrypty weryfikacji, które umożliwiają wykonanie scenariusza PAM w celu potwierdzenia prawidłowego działania wdrożenia usługi PAM.
Aby użyć funkcji weryfikacji wdrożenia, należy zmodyfikować sekcję <PamValidation/> pliku PAMDeploymentConfig.xml.

>[!NOTE]
>Weryfikacja wymaga komputera klienckiego przyłączonego do domeny CORP z zainstalowanymi składnikami usługi PAM po stronie klienta. Skrypty odnoszące się do instalacji klienta można znaleźć w sekcji Dodatek.

Nazwa komputera klienckiego musi zostać zaktualizowana w tagu <PAMValidationClient/> pliku PAMDeploymentConfig.xml. Pozostałe dane węzła <PAMValidation/> należy edytować tylko wtedy, gdy występują konflikty z istniejącymi użytkownikami/grupami, jako że sprawdzenie poprawności podejmie próbę ich utworzenia.
Aby przeprowadzić sprawdzenie poprawności, wykonaj następujące kroki:

Krok 1.

1. Zaloguj się do kontrolera domeny CORPDC jako administrator domeny CORP.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Spowoduje to utworzenie wymaganych grup i użytkowników potrzebnych do przeprowadzenia weryfikacji.

Krok 2:

1. Zaloguj się na serwerze PAM jako MIMAdmin.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Ten krok umożliwia przeprowadzenie migracji użytkowników i grup do środowiska PAM.

Krok 3:

1. Zaloguj się do klienta CORP jako administrator lokalny.
2. Uruchom program PowerShell jako administrator.
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


W tym kroku zostanie wyświetlony monit o poświadczenia konta CORPAdmin. Po ich podaniu następuje dodanie wymaganych użytkowników do grup „Użytkownicy pulpitu zdalnego” i „Użytkownicy zarządzania zdalnego”.
Na komputerze klienckim CORP użyj następujących poleceń do otwarcia programu PowerShell jako użytkownik PRIV, który ma zostać poddany procedurze weryfikacji. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
W oknie programu PowerShell wpisz polecenie:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Ta czynność spowoduje wyświetlenie stanu żądania.
  Początkowo użytkownik nie ma dostępu do zasobu. Użytkownik otrzyma dostęp, gdy zostanie dodany do roli w trybie Just-In-Time. Po wygaśnięciu czasu trwania żądania użytkownik ponownie traci dostęp.
  Skrypt wykorzystuje ustawienie domyślne (11 minut) czasu ważności żądania.

>[!div class="step-by-step"]
[« Krok 7](sp1-step7-setup-sidhistory-sidfiltering.md)
[Dodatek »](sp1-pam-deployment-addendum.md)
