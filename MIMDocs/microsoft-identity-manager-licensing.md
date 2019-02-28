---
title: Licencjonowanie programu Microsoft Identity Manager i pliki do pobrania | Dokumentacja firmy Microsoft
description: W tym artykule opisano metody licencjonowania programu Microsoft Identity Manager (MIM) 2016 r., za pomocą wskaźników o tym, gdzie można pobrać oprogramowanie.
keywords: ''
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 9e74b7ccf332c1572ce395fad18b8629673c756a
ms.sourcegitcommit: 4f0b2883922bcb8fbef6b4284c35c6ca62c11565
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56954549"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Licencjonowanie programu Microsoft Identity Manager 2016 i pliki do pobrania

W tym artykule opisano metody licencjonowania programu Microsoft Identity Manager (MIM) 2016 r., za pomocą wskaźników o tym, gdzie można pobrać oprogramowanie.

## <a name="licensing-mim-for-your-organization"></a>Licencjonowanie programu MIM dla Twojej organizacji

Microsoft Identity Manager 2016 jest licencjonowana na poszczególnych użytkowników.  Szczegółowe informacje dotyczące licencjonowania są uwzględnione w postanowieniach dotyczących produktu i powiązanych dokumentów, które można pobrać z [postanowienia licencyjne](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx) strony.

### <a name="licensing-for-azure-ad-premium-customers"></a>Licencjonowanie dla klientów usługi Azure AD Premium

Microsoft Identity Manager 2016 jest dołączone do usługi Azure Active Directory — wersja Premium (P1 i P2), który jest częścią pakietu Enterprise Mobility + Security.

Usługa Azure AD Premium jest dostępna za pośrednictwem [umowy Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), [programu licencjonowania zbiorowego Open](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx)i [dostawców rozwiązań w chmurze](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) program. Subskrybenci platformy Azure i usługi Office 365 mogą też kupić usługę Azure Active Directory Premium P1 i P2 online.  Dowiedz się więcej o [cennik usługi Azure Active Directory](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### <a name="mim-cals"></a>Licencje CAL programu MIM

Jeśli nie masz subskrypcji usługi Azure Active Directory — wersja Premium dla użytkowników i korzystają z więcej możliwości programu MIM poza synchronizacji, a następnie [licencji dostępu klienta (CAL)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) jest wymagana dla każdego użytkownika, którego tożsamość jest zarządzana w programie MIM. Jeśli chcesz, aby użytkownicy zewnętrzni — np. partnerom biznesowym, zewnętrznych wykonawców lub klientów — aby umożliwić dostęp do programu MIM, można uzyskać licencje CAL dla każdego z użytkowników zewnętrznych lub uzyskania licencji w zewnętrznych Connector (EC). Licencje CAL programu Microsoft Identity Manager 2016 nie są wymagane dla użytkowników, którego tożsamość jest tylko w przypadku usługi synchronizacji programu Microsoft Identity Manager i nie jest zarządzany w jakikolwiek inny składnik programu MIM.

### <a name="licenses-for-platform-components"></a>Licencji dla składników platformy

Wymagana jest licencja systemu Windows Server do użycia oprogramowania serwerowego firmy Microsoft Identity Manager 2016, jako dodatek systemu Windows Server. I wdrożenia programu MIM wymaga także instalacji programu SQL Server.  Licencje na system Windows Server i programu SQL Server nie są uwzględniane przy użyciu programu MIM.

## <a name="obtaining-mim-software"></a>Uzyskiwanie oprogramowania MIM

Przed rozpoczęciem nowej instalacji programu MIM lub uaktualnienia ze starszej wersji, upewnij się, że masz najnowszej wersji.

Jeśli rozpoczynasz zainstalować od nowa, należy pobrać pliki instalacyjne dla poszczególnych składników programu MIM, która jest odpowiednia dla danego scenariusza. Następnie należy pobrać wszystkie aktualizacje dla tych plików, a następnie Pobierz dodatkowe składniki, które są osobne pliki do pobrania z Centrum pobierania.


| Scenariusz | Składnik | Wymagane dla scenariusza? | Nazwa folderu ISO dysku DVD | Komentarze |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Synchronizacja| Synchronizowanie usługi (łącznie z łącznika usługi AD) | Tak | `Synchronization Service` | |
| Synchronizacja | PCNS | Nie | `Password Change Notification Service` |  Do zainstalowania na kontrolerach domeny |
| Synchronizacja | Wykres łączników na potrzeby LDAP, SQL, Web Services i programu PowerShell programu Lotus Domino | Nie | ND | Rozpowszechniane za pośrednictwem Centrum pobierania |
| Privileged Access Management (Ochrona systemu Windows i usługi Active Directory platformy Microsoft Azure przy użyciu programu Privileged Access Management) | Usługa MIM | Tak | `Service and Portal` | |
| Samoobsługa | Usługa MIM i portalu programu MIM | Tak | `Service and Portal` | |
| Samoobsługa | Dodatki i rozszerzenia | Nie | `Add-ins and extensions` | Do zainstalowania na komputerach użytkowników końcowych |
| Samoobsługa | Raportowanie programu SCSM | Nie | `Data Warehouse Support Scripts` | |
| Samoobsługa | Agenta raportowania hybrydowego | Nie | ND | Rozpowszechniane za pośrednictwem Centrum pobierania |
| Samoobsługa | Pakiety językowe | Nie | `LANGUAGE Packs` | |
| Zarządzanie certyfikatami | CM | Tak | `Certificate Management` | |
| Zarządzanie certyfikatami | Klient zarządzania certyfikatami w usłudze masowego | Nie | `CM Bulk Client` | |
| Zarządzanie certyfikatami | Klient zarządzania Certyfikatami | Nie | `CM Client`  | |
| Zarządzanie certyfikatami | Aplikacji Menedżer certyfikatów dla Windows | Nie | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Uzyskiwanie pakietów Instalatora Windows

W przypadku nowych instalacji większości organizacji pobranie pakietów instalacyjnych programu MIM z [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


Plik ISO dysku DVD zawiera jeden folder dla każdego składnika programu MIM: Synchronizacja usługi, usługi i portalu, itp. Jeśli zamierzasz zainstalować oprogramowanie na innym komputerze, z którego został pobrany, pamiętaj skopiować cały plik ISO lub folderu dla składnika: nie Kopiuj tylko po prostu plik MSI z folderu bez reszty pliki i podfoldery.

Jeśli nie masz dostępu do Centrum usługi licencjonowania zbiorowego, klientów dzięki subskrypcji dewelopera odpowiednie można również pobrać program MIM 2016 z dodatkiem SP1 jako plik ISO z [pobieranie Visual Studio Moje korzyści](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=).  Wyszukaj "Programu Microsoft Identity Manager 2016 z dodatkiem Service Pack 1".  

Jeśli nie masz dostępu do Centrum usługi licencjonowania zbiorowego i jedynie chcesz wypróbować oprogramowania MIM przez ograniczony czas, możesz pobrać [wersję ewaluacyjną programu MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). To oprogramowanie nie jest przeznaczony do użycia w środowisku produkcyjnym i przestanie działać 180 dni po pierwszej instalacji i nie może zostać uaktualniona. Wersja ewaluacyjna wymaga systemu Windows Server 2008 R2, Windows Server 2012 lub Windows Server 2012 R2 do instalacji.  Jeśli jesteś nowym użytkownikiem programu MIM i zapoznanie się z technologią, należy pamiętać o tym, że wszystkie scenariusze programu MIM wymaga domeny usługi Active Directory, Windows Server i SQL Server, aby być obecne. Jeśli nie masz system Windows Server lub SQL Server już istnieje, możesz spróbować [aprowizacji maszyny Wirtualnej przy użyciu programu SQL Server 2016 i Windows Server 2016](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Uzyskiwanie aktualizacji

Po zainstalowaniu programu MIM z pliku MSI, należy zainstalować obok niezbędnych poprawek.

Sprawdź [Historia wersji programu Identity Manager](./reference/version-history.md) dla najnowszej wersji aktualizacji, która zawiera link do witryny pobierania dla pliki poprawek Instalatora.

Aby określić, które pliki aktualizacji są niezbędne, ta tabela zawiera składniki i nazwę odpowiedniego pliku poprawki (MSP) do aktualizacji.

| Scenariusz | Składnik | Nazwa folderu ISO dysku DVD | Odpowiednia nazwa pliku poprawki aktualizacji |
|----------|-----------|-   |-------------------|----------|--------------|
|Synchronizacja| Usługa synchronizacji | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Samoobsługa | Usługa MIM i portalu programu MIM | `Service and Portal` | `FIMService_x64*msp` |
| Samoobsługa | Dodatki i rozszerzenia | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Samoobsługa | Pakiety językowe | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Zarządzanie dostępem (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Zarządzanie certyfikatami | CM |  `Certificate Management` | `FIMCM*.msp` |
| Zarządzanie certyfikatami | Klient zarządzania certyfikatami w usłudze masowego |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Zarządzanie certyfikatami | Klient zarządzania Certyfikatami | Klient zarządzania Certyfikatami |`FIMCMClient*msp` |

Się, że można odczytać informacje o wersji skojarzony z aktualizacją update przed zainstalowaniem plik MSP.

Aktualizacje [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) nie są dystrybuowane jako pliki MSP, tylko jako instalatory MSI.

### <a name="additional-downloads"></a>Dodatkowe pliki do pobrania

Następujące pliki do pobrania mogą być istotne:

- [Agent raportowania hybrydowego MIM](https://www.microsoft.com/download/details.aspx?id=55112)

- [Ogólny łącznik LDAP, ogólny łącznik SQL, wykres łącznika, łącznik programu Lotus Domino, łącznik programu PowerShell, Łącznik usług sieci Web](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Łącznik dla Store profilu użytkownika programu SharePoint](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Jeśli nie masz jeszcze domeny usługi Active Directory i konfigurowania scenariusza funkcji PAM na potrzeby doświadczeń, zobacz [skrypty wdrażania usługi PAM programu MIM 2016 z dodatkiem SP1](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej w scenariuszach dostarczane w [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Odczyt [przewodnik planowania pojemności](capacity-planning-guide.md).
- Wdrażanie programu MIM dla [scenariusza synchronizacji](microsoft-identity-manager-deploy.md) lub [scenariusz zarządzania dostępem uprzywilejowanym](./pam/privileged-identity-management-for-active-directory-domain-services.md).

