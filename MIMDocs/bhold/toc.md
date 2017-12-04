# [Poznawanie i eksplorowanie](../microsoft-identity-manager-2016.md)
## [Co to jest program MIM 2016?](../microsoft-identity-manager-2016.md)
## [Co nowego w dodatku Service Pack 1](../Microsoft-identity-manager-2016-sp1-release-notes.md)
### [Skrypty wdrażania usługi PAM w programie MIM2016 SP1](../sp1-deployment-scripts.md)
## [Zapoznanie się z usługą PAM](../pam/privileged-identity-management-for-active-directory-domain-services.md)
## [Raportowanie hybrydowe na platformie Azure](../identity-manager-hybrid-reporting-azure.md)
# [Planowanie i projektowanie](../microsoft-identity-manager-2016-supported-platforms.md)
## [Obsługiwane platformy](../microsoft-identity-manager-2016-supported-platforms.md)
## [Łączenie z katalogami](../supported-management-agents.md)
## [Planowanie pojemności](../capacity-planning-guide.md)
## [Topologia wdrożenia](../topology-considerations.md)
## [Planowanie wdrożenia usługi PAM](../pam/environment-overview.md)
# [Wdrażanie i użytkowanie](../microsoft-identity-manager-deploy.md)
## [Pierwsze wdrożenie](../microsoft-identity-manager-deploy.md)
### [Konfiguracja domeny](../preparing-domain.md)
### [Konfiguracja serwera: Windows Server](../prepare-server-ws2012r2.md)
### [Konfiguracja serwera: SQL](../prepare-server-sql2014.md)
### [Konfiguracja serwera: SharePoint](../prepare-server-sharepoint.md)
### [Konfiguracja serwera: Exchange](../prepare-server-exchange.md)
### [Instalowanie programu MIM: synchronizacja](../install-mim-sync.md)
### [Instalowanie programu MIM: usługa i portal](../install-mim-service-portal.md)
### [Instalowanie programu MIM: synchronizowanie baz danych](../install-mim-sync-ad-service.md)
## [Uaktualnianie z programu Forefront Identity Manager 2010 R2](../microsoft-identity-manager-2016-upgrade-from-fim-2010-R2.md)
## [Instalowanie zarządzania certyfikatami programu MIM](../mim-cm-deploy.md)
## [Tematy dotyczące instalacji pakietu BHOLD](bhold-installation-guide.md)
### [Instalacja podstawowa pakietu BHOLD](bhold-core-installation.md)
### [Instalacja integracji pakietu BHOLD](bhold-integration-installation.md)
### [Instalacja zaświadczania pakietu BHOLD](bhold-attestation-installation.md)
### [Instalacja generatora modeli pakietu BHOLD](bhold-model-generator-installation.md)
### [Instalacja raportowania pakietu BHOLD](bhold-reporting-installation.md)
### [Instalacja analizy pakietu BHOLD](bhold-analytics-installation.md)
### [Instalacja łącznika do zarządzania dostępem pakietu BHOLD](bhold-access-management-connector-install.md)
## [Usługa powiadamiania o zmianie hasła](../deploying-mim-password-change-notification-service-on-domain-controller.md)
## [Raportowanie hybrydowe programu Identity Manager](../working-with-identity-manager-hybrid-reporting.md)
## [Samoobsługowe resetowanie hasła](../working-with-self-service-password-reset.md)
## [Menedżer certyfikatów programu MIM](../working-with-mim-certificate-manager.md)
### [Rejestrowanie kart inteligentnych](../certificate-manager-for-non-administrators.md)
### [Tworzenie certyfikatów oprogramowania](../certificate-manager-for-software-certificates.md)
# [Korzystanie z funkcji zarządzania dostępem uprzywilejowanym](../pam/privileged-identity-management-for-active-directory-domain-services.md)
## [Konfigurowanie programu MIM dla usługi Privileged Access Management](../pam/configuring-mim-environment-for-pam.md)
## [Zrozumienie składników](../pam/principles-of-operation.md)
### [Omówienie środowiska](../pam/environment-overview.md)
### [Model warstwy](../pam/tier-model-for-partitioning-administrative-privileges.md)
### [Planowanie środowiska bastionu](../pam/planning-bastion-environment.md)
### [Definiowanie ról](../pam/defining-roles-for-pam.md)
### [Wysoka dostępność i odzyskiwanie po awarii](../pam/high-availability-disaster-recovery-considerations-bastion-environment.md)
### [Wymagania dotyczące sprzętu i oprogramowania](../pam/hardware-software-requirements.md)
### [Krok 1 — domena CORP](../pam/step-1-prepare-corp-domain.md)
### [Krok 2 — kontroler domeny PRIV](../pam/step-2-prepare-priv-domain-controller.md)
### [Krok 3 — serwer PAM](../pam/step-3-prepare-pam-server.md)
### [Krok 4 — instalowanie programu MIM na serwerze PAM](../pam/step-4-install-mim-components-on-pam-server.md)
### [Krok 5 — ustanowienie zaufania między elementami PRIV i CORP](../pam/step-5-establish-trust-between-priv-corp-forests.md)
### [Krok 6 — utworzenie kont uprzywilejowanych](../pam/step-6-transition-group-to-pam.md)
### [Krok 7 — podniesienie uprawnień dostępu użytkownika](../pam/step-7-elevate-user-access.md)
### [Wdrażanie usługi PAM programu MIM w systemie Windows Server 2016](../pam/deploy-pam-with-windows-server-2016.md)
### [Konfigurowanie usługi Azure MFA](../pam/use-azure-mfa-for-activation.md)
## [Konfiguracja usługi PAM za pomocą skryptów](../pam/sp1-pam-configure-using-scripts.md)
### [Krok 1 — Konfigurowanie domeny PRIV](../pam/sp1-step1-configuring-priv-domain.md)
### [Krok 2 — Konfigurowanie domeny CORP](../pam/sp1-step2-configuring-corp-domain.md)
### [Krok 3 — Konfigurowanie serwera SQL](../pam/sp1-step3-installing-configuring-sql.md)
### [Krok 4 — Konfigurowanie programu SharePoint](../pam/sp1-step4-configuring-sharepoint.md)
### [Krok 5 — Instalowanie/konfigurowanie usługi PAM](../pam/sp1-step5-configuring-pam.md)
### [Krok 6 — Konfigurowanie zaufania usługi PAM](../pam/sp1-step6-setup-pam-trust.md)
### [Krok 7 — Konfigurowanie historii/filtrowania identyfikatorów SID](../pam/sp1-step7-setup-sidhistory-sidfiltering.md)
### [Krok 8 — Weryfikacja wdrożenia usługi PAM](../pam/sp1-step8-pam-deployment-verification.md)
### [Dodatek](../pam/sp1-pam-deployment-addendum.md)
# Zarządzanie infrastrukturą
## [Usługa Best Practice Analyzer w programie Identity Manager](https://technet.microsoft.com/library/jj203402)
## [Usługa powiadamiania o zmianie hasła](https://technet.microsoft.com/library/e27c0bc6-c808-4fdb-9e59-58feeb419308)
## Zarządzanie certyfikatami
### [Narzędzie wiersza polecenia CLMUtil](https://technet.microsoft.com/library/cc720647)
### [Szablony profilu konfiguracji](https://technet.microsoft.com/library/cc708656)
### [Korzystanie z witryny sieci Web zarządzania certyfikatami](https://technet.microsoft.com/library/cc720560)
### [Zarządzanie aplikacjami karty inteligentnej](https://technet.microsoft.com/library/cc708681)
### [Tworzenie kopii zapasowej i przywracanie](https://technet.microsoft.com/library/dd883245)
## Samoobsługowe resetowanie hasła
### [Programowa rejestracja użytkownika](https://technet.microsoft.com/library/jj134294)
### [Dostosowywanie portalu](../reference/mim-portal-customizations.md)
## Usługa i portal
### [Protokół Kerberos](https://technet.microsoft.com/library/jj134299)
### [Rejestrowanie dynamiczne](../infrastructure/mim-service-dynamic-logging.md)
### [Przewodnik dotyczący wydajności eksportu](https://technet.microsoft.com/library/hh322883)
## Raportowanie
### [Raportowanie przy użyciu raportów niestandardowych oraz rozszerzalność](https://technet.microsoft.com/library/jj133861)
## [Oprogramowanie Microsoft Identity: publiczne wersje kompilacji](https://blogs.technet.microsoft.com/iamsupport/idmbuildversions/)
# [Tematy pomocy](../reference/microsoft-identity-manager-2016-developer-reference.md)
## Dokumentacja dla deweloperów
### [Dokumentacja dla deweloperów programu MIM 2016](../reference/microsoft-identity-manager-2016-developer-reference.md)
### BHOLD
#### [Dokumentacja dla deweloperów pakietu BHOLD](../reference/mim2016-bhold-developer-reference.md) 
### [Dokumentacja interfejsu API REST zarządzania certyfikatami](../reference/certificate-management-rest-api-reference.md)
#### [Szczegóły usługi interfejsu API REST zarządzania certyfikatami](../reference/certificate-management-rest-api-service-details.md)
#### [Przewodnik przykładowej rejestracji](../reference/sample-enrollment-walkthrough.md)
#### [Pobieranie szablonów profilu](../reference/get-profile-templates.md)
#### [Operacje dotyczące zasad](../reference/policy-operations.md)
#### [Pobieranie zasad dotyczących przepływu pracy](../reference/get-workflow-policy.md)
#### [Pobieranie zasad dotyczących karty inteligentnej](../reference/get-smartcard-policy.md)
#### [Operacje dotyczące żądań](../reference/request-operations.md)
##### [Tworzenie żądania](../reference/create-request.md)
##### [Pobieranie żądania](../reference/get-request.md)
##### [Anulowanie, porzucenie lub ukończenie żądania](../reference/cancel-abandon-complete-request.md)
#### [Operacje dotyczące żądania certyfikatu](../reference/certificate-request-operations.md)
##### [Pobieranie opcji generowania żądania certyfikatu](../reference/get-certificate-request-generation-options.md)
##### [Uzyskanie odpowiedzi na żądania certyfikatu](../reference/get-certificate-responses.md)
#### [Operacje dotyczące karty inteligentnej](../reference/smartcard-operations.md)
##### [Przypisanie karty inteligentnej do żądania](../reference/assign-smartcard-to-request.md)
##### [Pobieranie danych karty inteligentnej](../reference/get-smartcard-data.md)
##### [Uzyskanie odpowiedzi na żądanie uwierzytelniania przy użyciu karty inteligentnej](../reference/get-smartcard-authentication-response.md)
##### [Pobieranie zróżnicowanego klucza administratora dla karty inteligentnej](../reference/get-smartcard-diversified-admin-key.md)
##### [Pobieranie proponowanego numeru PIN karty inteligentnej](../reference/get-smartcard-proposed-pin.md)
##### [Aktualizacja stanu karty inteligentnej](../reference/update-smartcard-status.md)
#### [Operacje dotyczące profilu](../reference/profile-operations.md)
##### [Pobieranie danych profilów](../reference/get-profile-data.md)
##### [Pobieranie operacji dla stanu profilu](../reference/get-profile-state-operations.md)
#### [Operacje dotyczące certyfikatu](../reference/certificate-operations.md)
##### [Pobieranie certyfikatów karty inteligentnej lub profilu](../reference/get-smartcard-profile-certificates.md)
##### [Pobieranie certyfikatów użytkowników](../reference/get-user-certificates.md)
### [Dokumentacja interfejsu API REST zarządzania uprzywilejowanym dostępem](../reference/privileged-access-management-rest-api-reference.md)
#### [Szczegóły usługi interfejsu API REST usługi PAM](../reference/privileged-access-management-rest-api-service-details.md)
#### [Pobieranie ról PAM](../reference/privileged-access-management-get-roles.md)
#### [Tworzenie żądania PAM](../reference/privileged-access-management-create-request.md)
#### [Pobieranie żądań PAM](../reference/privileged-access-management-get-requests.md)
#### [Zamykanie żądania PAM](../reference/privileged-access-management-close-request.md)
#### [Pobieranie oczekujących żądań PAM](../reference/privileged-access-management-get-pending-requests.md)
#### [Zatwierdzanie lub odrzucanie oczekującego żądania PAM](../reference/privileged-access-management-approve-reject-pending-request.md)
#### [Pobieranie informacji dotyczących sesji PAM](../reference/privileged-access-management-get-session-info.md)
## Dokumentacja techniczna
### [Terminologia programu Microsoft Identity Manager 2016 SP1](../reference/mim-2016-sp1-terms.md)
### [Dokumentacja języka XML na potrzeby konfiguracji ekranu kontroli zasobów](../reference/rcd-configuration-xml-reference.md)
### [Kody błędów podczas uruchamiania Agenta zarządzania](../reference/maerrorcodes.md)
### [Wykaz funkcji programu Microsoft Identity Manager 2016](../reference/mim2016-functions-reference.md)
### [Informacje o zarządzaniu hasłami za pomocą programu Microsoft Identity Manager 2016](../infrastructure/mim2016-password-management.md)
### BHOLD
#### [Przewodnik po pojęciach pakietu BHOLD](bhold-concepts-guide.md)
## Historia wersji
### [Historia wersji programu MIM](../reference/version-history.md)
### [Historia wersji pakietu BHOLD](../reference/version-bhold-history.md)