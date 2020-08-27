---
title: Microsoft Identity Manager Licencjonowanie i pobieranie | Microsoft Docs
description: W tym artykule opisano podejścia do licencjonowania Microsoft Identity Manager (MIM) 2016 ze wskaźnikami na miejscu, w którym ma zostać pobrane oprogramowanie.
keywords: ''
author: markwahl-msft
ms.author: mwahl
manager: daveba
ms.date: 10/18/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: b7b0dd73e5a87f338dc8cd91e61ee6a19c84068a
ms.sourcegitcommit: d6178a67014d66d37056c13d10328ae03e3cd781
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970366"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016 Licencjonowanie i pobieranie

W tym artykule opisano podejścia do licencjonowania Microsoft Identity Manager (MIM) 2016 ze wskaźnikami na miejscu, w którym ma zostać pobrane oprogramowanie.

## <a name="licensing-mim-for-your-organization"></a>Licencjonowanie programu MIM dla Twojej organizacji

Microsoft Identity Manager 2016 jest licencjonowana na poszczególnych użytkowników.  Szczegółowe informacje na temat licencjonowania znajdują się w tematach dotyczących produktów i powiązane dokumenty, które można pobrać ze strony [postanowienia licencyjne](https://www.microsoft.com/licensing/product-licensing/products.aspx) .

### <a name="licensing-for-azure-ad-premium-customers"></a>Licencjonowanie dla Azure AD — wersja Premium klientów

Microsoft Identity Manager 2016 jest dołączony do Azure Active Directory — wersja Premium (P1 i P2), który jest częścią Enterprise Mobility + Security.

Azure AD — wersja Premium jest dostępna za pomocą usługi [Microsoft Umowa Enterprise](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx), [programu licencjonowania zbiorowego Open](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)i programu [dostawców rozwiązań w chmurze](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) . Subskrybenci platformy Azure i Microsoft 365 mogą również kupować Azure Active Directory — wersja Premium P1 i P2 w trybie online.  Więcej informacji można znaleźć pod adresem [Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="mim-cals"></a>Licencje CAL programu MIM

Jeśli nie masz subskrypcji Azure Active Directory — wersja Premium dla użytkowników i korzystasz z większej liczby możliwości programu MIM poza synchronizacją, w przypadku każdego użytkownika, którego tożsamość jest zarządzana w programie MIM, wymagana jest [licencja dostępu klienta (CAL)](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx) . Jeśli chcesz, aby użytkownicy zewnętrzni, tacy jak partnerzy biznesowi, zewnętrzni wykonawczy lub Klienci, mieli dostęp do programu MIM, możesz uzyskać licencje CAL dla każdego z użytkowników zewnętrznych lub uzyskać licencje na łącznik zewnętrzny (WE). Microsoft Identity Manager 2016 licencje CAL nie są wymagane dla użytkowników, których tożsamość działa tylko w ramach usługi synchronizacji Microsoft Identity Manager i nie są zarządzane w żadnym innym składniku programu MIM.

### <a name="licenses-for-platform-components"></a>Licencje dla składników platformy

Licencja systemu Windows Server jest wymagana do korzystania z oprogramowania serwera Microsoft Identity Manager 2016 jako dodatku systemu Windows Server. A wdrożenie programu MIM wymaga również instalacji SQL Server.  Licencje na system Windows Server i SQL Server nie są dołączone do programu MIM.

## <a name="obtaining-mim-software"></a>Uzyskiwanie oprogramowania MIM

Przed rozpoczęciem nowej instalacji programu MIM lub uaktualnieniem ze starszej wersji upewnij się, że masz najnowsze wersje.

W przypadku uruchamiania nowej instalacji należy pobrać pliki instalacyjne dla każdego składnika programu MIM, który jest istotny dla danego scenariusza. Następnie Pobierz wszystkie aktualizacje tych plików, a następnie Pobierz wszystkie dodatkowe składniki, które są oddzielnymi pobraniami z centrum pobierania.


| Scenariusz | Składnik | Wymagany dla scenariusza? | Nazwa folderu ISO dysku DVD | Komentarze |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Synchronizacja| Usługa synchronizacji (w tym Łącznik do usługi AD) | Tak | `Synchronization Service` | |
| Synchronizacja | PCNS | Nie | `Password Change Notification Service` |  Do zainstalowania na kontrolerach domeny |
| Synchronizacja | Łączniki dla protokołów LDAP, SQL, Web Services, PowerShell, Lotus Domino, Graph | Nie | Nie dotyczy | Dystrybuowane za pośrednictwem Centrum pobierania |
| Privileged Access Management (Ochrona systemu Windows i usługi Active Directory platformy Microsoft Azure przy użyciu programu Privileged Access Management) | Usługa MIM | Tak | `Service and Portal` | |
| Samoobsługa | Usługa MIM, Portal programu MIM | Tak | `Service and Portal` | |
| Samoobsługa | Dodatki i rozszerzenia | Nie | `Add-ins and extensions` | Do zainstalowania na komputerach użytkowników końcowych |
| Samoobsługa | Raportowanie SCSM | Nie | `Data Warehouse Support Scripts` | |
| Samoobsługa | Agent raportowania hybrydowego | Nie | Nie dotyczy | Dystrybuowane za pośrednictwem Centrum pobierania |
| Samoobsługa | Pakiety językowe | Nie | `LANGUAGE Packs` | |
| Zarządzanie certyfikatami | CM | Tak | `Certificate Management` | |
| Zarządzanie certyfikatami | Klient zbiorczy CM | Nie | `CM Bulk Client` | |
| Zarządzanie certyfikatami | Klient CM | Nie | `CM Client`  | |
| Zarządzanie certyfikatami | Aplikacja CM dla systemu Windows | Nie | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Uzyskiwanie pakietów Instalatora Windows

W przypadku nowej instalacji większość organizacji pobiera pakiety instalacyjne programu MIM z [centrum usługi licencjonowania zbiorowego](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


Plik ISO dysku DVD zawiera jeden folder dla każdego składnika programu MIM: usługa synchronizacji, usługa i Portal itp. Jeśli chcesz zainstalować oprogramowanie na innym komputerze, na którym został pobrany, pamiętaj o skopiowaniu całego pliku ISO lub folderu dla składnika: nie tylko Kopiuj plik MSI z folderu bez reszty plików i podfolderów.

Jeśli nie masz dostępu do centrum usługi licencjonowania zbiorowego, klienci z odpowiednią subskrypcją dewelopera mogą również pobrać program MIM 2016 z dodatkiem SP2 jako plik ISO z [programu Visual Studio moje korzyści](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202&pgroup=).  Wyszukaj ciąg "Microsoft Identity Manager 2016 z dodatkiem Service Pack 2".  

Jeśli nie masz dostępu do centrum usługi licencjonowania zbiorowego, a jedynie chcesz wypróbować oprogramowanie MIM przez ograniczony czas, możesz pobrać [wersję ewaluacyjną programu mim 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). To oprogramowanie nie jest przeznaczone do użytku produkcyjnego i nie będzie działać 180 dni po pierwszej instalacji i nie może zostać uaktualnione. Wersja ewaluacyjna wymaga systemu Windows Server 2008 R2, Windows Server 2012 lub Windows Server 2012 R2 do instalacji.  Jeśli dopiero zaczynasz korzystać z programu MIM i ucząsz technologię, pamiętaj, że wszystkie scenariusze programu MIM wymagają domeny Active Directory, systemu Windows Server i SQL Server. Jeśli nie masz już systemu Windows Server lub SQL Server, możesz spróbować [zainicjować swoją maszynę wirtualną za pomocą SQL Server 2016 i systemu Windows Server 2016](https://azure.microsoft.com/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Uzyskiwanie aktualizacji

Po zainstalowaniu programu MIM z pliku MSI należy zainstalować wymagane poprawki.

Sprawdź historię wersji programu [Identity Manager](./reference/version-history.md) dla najnowszej wersji aktualizacji, która ma link do witryny pobierania dla plików poprawek Instalatora.

Aby określić, które pliki aktualizacji są niezbędne, w tej tabeli wymieniono składniki i nazwę odpowiedniego pliku poprawki (MSP) w ramach aktualizacji.

| Scenariusz | Składnik | Nazwa folderu ISO dysku DVD | Nazwa pliku odpowiedniej poprawki aktualizacji |
|----------|-----------|-   |-------------------|----------|--------------|
|Synchronizacja| Usługa synchronizacji | `Synchronization Service` | `MIMSyncService_x64*.msp` |
| Samoobsługa | Usługa MIM, Portal programu MIM | `Service and Portal` | `MIMService_x64*msp` |
| Samoobsługa | Dodatki i rozszerzenia | `Add-ins and extensions` | `MIMAddinsExtensions*msp` |
| Samoobsługa | Pakiety językowe | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Zarządzanie dostępem (pakietu BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Zarządzanie certyfikatami | CM |  `Certificate Management` | `MIMCM*.msp` |
| Zarządzanie certyfikatami | Klient zbiorczy CM |  `CM Bulk Client` |`MIMCMBulkClient*msp` |
| Zarządzanie certyfikatami | Klient CM | `CM Client` |`MIMCMClient*msp` |

Przed zainstalowaniem pliku MSP należy zapoznać się z informacjami o wersji skojarzonych z aktualizacją.

Aktualizacje [pakietu BHOLD](https://www.microsoft.com/download/details.aspx?id=55950) nie są dystrybuowane jako pliki msp, tylko jako instalatorzy MSI.

### <a name="additional-downloads"></a>Dodatkowe materiały do pobrania

Mogą być również odpowiednie następujące pliki do pobrania:

- [Agent raportowania hybrydowego programu MIM](https://www.microsoft.com/download/details.aspx?id=55112)

- [Łącznik ogólny LDAP, ogólny łącznik SQL, łącznik wykresu, łącznik programu Lotus Domino, łącznik programu PowerShell, łącznik usług sieci Web](https://go.microsoft.com/fwlink/?LinkId=717495)

- [Łącznik dla magazynu profilów użytkowników programu SharePoint](https://www.microsoft.com/download/details.aspx?id=41164)

- Jeśli nie masz jeszcze domeny Active Directory i konfigurujemy scenariusz PAM na potrzeby eksperymentowania, zobacz [Skrypty wdrażania usługi PAM w programie MIM 2016 z dodatkiem SP1](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się więcej na temat scenariuszy dostarczonych w [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Zapoznaj się z [przewodnikiem planowania pojemności](capacity-planning-guide.md).
- Wdróż program MIM dla [scenariusza synchronizacji](microsoft-identity-manager-deploy.md) lub [scenariusza zarządzania dostępem uprzywilejowanym](./pam/privileged-identity-management-for-active-directory-domain-services.md).

