---
title: "Korzystanie z usługi synchronizacji z usługami AD w programie Microsoft Identity Manager | Dokumentacja firmy Microsoft"
description: "Korzystając z agentów zarządzania i usługi synchronizacji programu MIM, można synchronizować bazy danych usług Active Directory i MIM."
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 31cc9a61bbcb309dae4ee4d09654432d08bf1e28
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="install-mim-2016-synchronize-active-directory-and-mim-service"></a>Instalacja programu MIM 2016: synchronizowanie usług Active Directory i MIM

>[!div class="step-by-step"]
[« Usługa i portal MIM](install-mim-service-portal.md)

> [!NOTE]
> W tym przewodniku zastosowano przykładowe nazwy i wartości dotyczące firmy o nazwie Contoso. Należy je zastąpić własnymi danymi. Na przykład:
> - Nazwa kontrolera domeny — **nazwa_serwera_mim**
> - Nazwa domeny — **contoso**
> - Hasło — **Pass@word1**

Domyślnie żadne łączniki usługi synchronizacji programu MIM (Sync) nie są skonfigurowane.  Typowym pierwszym krokiem jest użycie usługi synchronizacji programu MIM do wypełniania bazy danych usługi MIM informacjami z istniejących kont usługi Active Directory. W tym celu używana jest aplikacja usługi synchronizacji programu MIM.

## <a name="create-the-mim-management-agent"></a>Tworzenie agenta zarządzania programu MIM
Agent zarządzania (MA) programu MIM jest łącznikiem usługi synchronizacji programu MIM z usługą MIM. Ten łącznik można utworzyć przy użyciu kreatora tworzenia agenta zarządzania.

Podczas konfigurowania agenta zarządzania programu MIM należy określić konto użytkownika. W niniejszym dokumencie użyto nazwy **MIMMA** dla tego konta.

> [!NOTE]
> Konto używane dla agenta zarządzania programu MIM musi być tym samym kontem, które określono podczas instalacji usługi MIM.

###<a name="to-create-the-mim-ma"></a>Aby utworzyć agenta zarządzania programu MIM

1.  Otwórz menedżera usługi synchronizacji.

2.  Aby otworzyć kreatora tworzenia agenta zarządzania, zmień stronę na **Managment Actions** (Akcje zarządzania), a następnie w menu **Actions** (Akcje) kliknij pozycję **Create** (Utwórz).

3.  Na stronie **Create Management Agent** (Tworzenie agenta zarządzania) skonfiguruj następujące ustawienia, a następnie kliknij przycisk **Next** (Dalej).

    -   Management agent for (Agent zarządzania dla): FIM Service management agent (agent zarządzania usługi FIM)

    -   Name (Nazwa): MIMMA

4.  Na stronie **Connect to Database** (Połączenie z bazą danych) skonfiguruj następujące ustawienia, a następnie kliknij przycisk **Next** (Dalej).

    -   Server (Serwer): localhost

    -   Database (Baza danych): FIMService

    -   MIM Service base address (Adres podstawowy usługi MIM): http://localhost:5725

    -   Authentication mode (Tryb uwierzytelniania): Windows integrated authentication (zintegrowane uwierzytelnianie systemu Windows)

    -   User name (Nazwa użytkownika): mimma

    -   Password (Hasło): Pass@word

    -   Domain (Domena): contoso

5.  Na stronie **Selected Object Types** (Wybrane typy obiektów) sprawdź, czy wybrano typy obiektów uwzględnione na poniższej liście, a następnie kliknij przycisk **Next** (Dalej).

    -   DetectedRuleEntry

    -   ExpectedRuleEntry

    -   Grupa

    -   Person

    -   SynchronizationRule

6.  Na stronie **Selected Attributes** (Wybrane atrybuty) zaznacz pole wyboru **Show All** (Wyświetl wszystkie) i sprawdź, czy wybrano wszystkie atrybuty uwzględnione na liście, a następnie kliknij przycisk **Next** (Dalej).

7.  Na stronie **Configure Connector Filter** (Konfigurowanie filtru łącznika) kliknij przycisk **Next** (Dalej).

8.  Na stronie **Configure Object Type Mappings** (Konfigurowanie mapowań typów obiektów) dodaj następujące mapowanie, a następnie kliknij przycisk **Next** (Dalej).

    - Wybierz pozycję **Person** (Osoba) na liście **Data Source Object Type** (Typ obiektu źródła danych).
    - Kliknij pozycję **Add Mapping** (Dodaj mapowanie), aby otworzyć okno dialogowe Mapping (Mapowanie).
    - Wybierz pozycję **Person** (Osoba) na liście **Metaverse object type** (Typ obiektu Metaverse).
    - Kliknij przycisk **OK**, aby zamknąć okno dialogowe Mapping (Mapowanie).
    - Wybierz pozycję **Group** (Grupa) na liście **Data Source Object Type** (Typ obiektu źródła danych).
    - Kliknij pozycję **Add Mapping** (Dodaj mapowanie), aby otworzyć okno dialogowe Mapping (Mapowanie).
    - Wybierz pozycję **Group** (Grupa) na liście **Metaverse object type** (Typ obiektu Metaverse).
    - Kliknij przycisk **OK**, aby zamknąć okno dialogowe Mapping (Mapowanie).

9.  Na stronie **Configure Attribute Flow** (Konfigurowanie przepływu atrybutów) utwórz mapowanie przepływu atrybutów, tak jak pokazano poniżej, a następnie kliknij przycisk **Next** (Dalej).

    -   Wybierz pozycję **Person** (Osoba) jako typ obiektu źródła danych i typ obiektu Metaverse.

    -   Wybierz pozycję **Direct** (Bezpośredni) jako typ mapowania.

    -   Dla każdego wiersza w poniższej w tabeli wykonaj następujące czynności:

        -   Wybierz wartość ustawienia **Flow direction** (Kierunek przepływu) wyświetlanego dla tego wiersza w tabeli.

        -   Wybierz wartość ustawienia **Data source attribute** (Atrybut źródła danych) wyświetlanego dla tego wiersza w tabeli.

        -   Wybierz wartość ustawienia **Metaverse attribute** (Atrybut Metaverse) wyświetlanego dla tego wiersza w tabeli.

        -   Aby zastosować mapowanie przepływu, kliknij pozycję **New** (Nowy).

    | **Atrybut źródła danych** | **Kierunek przepływu** | **Atrybut Metaverse** |
    |-|-|-|
    | AccountName | Eksportowanie | accountName |
    | Nazwa wyświetlana | Eksportowanie | displayName |
    | Domain | Eksportowanie | domain |
    | Poczta e-mail | Eksportowanie | Poczta |
    | Identyfikator pracownika | Eksportowanie | employeeID |
    | EmployeeType | Eksportowanie | employeeType |
    | FirstName | Eksportowanie | firstName |
    | LastName | Eksportowanie | lastName |
    | ObjectSID | Eksportowanie | objectSid |

    -   Wybierz pozycję **Group** (Grupa) jako typ źródła danych i typ obiektu Metaverse.

    -   Wybierz pozycję **Direct** (Bezpośredni) jako typ mapowania.

    -   Dla każdego wiersza w poniższej w tabeli wykonaj następujące czynności:

        -   Wybierz wartość ustawienia **Flow direction** (Kierunek przepływu) wyświetlanego dla tego wiersza w tabeli.

        -   Wybierz wartość ustawienia **Data source attribute** (Atrybut źródła danych) wyświetlanego dla tego wiersza w tabeli.

        -   Wybierz wartość ustawienia **Metaverse attribute** (Atrybut Metaverse) wyświetlanego dla tego wiersza w tabeli.

        -   Aby zastosować mapowanie przepływu, kliknij pozycję **New** (Nowy).

    | **Atrybut źródła danych** | **Kierunek przepływu** | **Atrybut Metaverse** |
    |-|-|-|
    | AccountName | Eksportowanie | accountName |
    | Nazwa wyświetlana | Eksportowanie | displayName |
    | Domain | Eksportowanie | domain |
    | Poczta e-mail | Eksportowanie | Poczta |
    | MailNickName | Eksportowanie | mailNickName |
    | Element członkowski | Eksportowanie | członek |
    | ObjectSID | Eksportowanie | objectSid |
    | Zakres | Eksportowanie | zakres |
    | Typ | Eksportowanie | typ |
    | MembershipAddWorkflow | Eksportowanie | membershipAddWorkflow |
    | MembershipLocked | Eksportowanie | membershipLocked |
    | AccountName | Importuj | accountName |
    | DisplayedOwner | Importuj | displayedOwner |
    | Nazwa wyświetlana | Importuj | displayName |
    | MailNickName | Importuj | mailNickName |
    | Element członkowski | Importuj | członek |
    | Zakres | Importuj | zakres |
    | Typ | Importuj | typ |

10.  Na stronie **Configure Deprovisioning** (Konfigurowanie anulowania zastrzeżenia) kliknij przycisk **Next** (Dalej).

11.  Aby utworzyć agenta zarządzania, na stronie **Configure Extensions** (Konfigurowanie rozszerzeń) kliknij przycisk **Finish** (Zakończ).

## <a name="create-the-ad-management-agent"></a>Tworzenie agenta zarządzania usługi AD
Agent zarządzania usługi Active Directory jest łącznikiem dla usług domenowych w usłudze AD. Ten łącznik można utworzyć przy użyciu kreatora tworzenia agenta zarządzania.

1. Aby otworzyć kreatora tworzenia agenta zarządzania, w menu **Actions** (Akcje) kliknij pozycję **Create** (Utwórz).

2. Na stronie **Create Management Agent** (Tworzenie agenta zarządzania) skonfiguruj następujące ustawienia, a następnie kliknij przycisk **Next** (Dalej):

    - Management agent for (Agent zarządzania dla): Active Directory Domain Services (Usługi domenowe Active Directory)
    - Name (Nazwa): ADMA

3. Na stronie **Connect to Active Directory Forest** (Połączenie z lasem usługi Active Directory) skonfiguruj następujące ustawienia, a następnie kliknij przycisk **Next** (Dalej):

    - Forest name (Nazwa lasu): contoso.local
    - User name (Nazwa użytkownika): administrator
    - Password (Hasło): &lt;hasło konta&gt;
    - Domain (Domena): contoso

4. Na stronie **Configure Directory Partitions** (Konfigurowanie partycji katalogu) skonfiguruj następujące ustawienia, a następnie kliknij przycisk **Next** (Dalej):

    - Z listy **Select directory partitions** (Wybieranie partycji katalogu) wybierz pozycję **DC=CONTOSO, DC=local**.

    - Aby otworzyć okno dialogowe Select Containers (Wybieranie kontenerów), kliknij pozycję **Containers** (Kontenery).

    - Jeśli chcesz zmienić kontener, aby umieścić tylko obiekty zarządzane przez program MIM w określonym kontenerze, kliknij węzeł **DC = CONTOSO, DC = local**, a następnie kliknij węzeł odpowiedniego kontenera.

    - Aby zamknąć okno dialogowe Select Containers (Wybieranie kontenerów), kliknij przycisk **OK**.

5. Na stronie **Configure Provisioning Hierarchy** (Konfigurowanie hierarchii aprowizacji) kliknij przycisk **Next** (Dalej).

6. Na stronie **Select Object Types** (Wybieranie typów obiektów) skonfiguruj następujące ustawienia, a następnie kliknij przycisk **Next** (Dalej):

    - Z listy **Object types** (Typy obiektów) wybierz pozycje **user** (użytkownik) i **group** (grupa).

7. Na stronie **Select Attributes** (Wybieranie atrybutów), zaznacz pole **Show ALL** (Pokaż WSZYSTKO), wybierz poniższe atrybuty, a następnie kliknij przycisk **Next** (Dalej):

    -   company
    -   displayName
    -   employeeID
    -   employeeType
    -   givenName
    -   groupType
    -   managedBy
    -   manager
    -   członek
    -   objectSid
    -   sAMAccountName
    -   sAMAccountType
    -   sn
    -   unicodePwd
    -   userAccountControl

8. Na stronie **Configure Connector Filter** (Konfigurowanie filtru łącznika) kliknij przycisk **Next** (Dalej).

9. Na stronie **Configure Join and Projection Rules** (Konfigurowanie reguł dołączania i projekcji) kliknij przycisk **Next** (Dalej).

10. Na stronie **Configure Attribute Flow** (Konfigurowanie przepływu atrybutów) kliknij przycisk **Next** (Dalej).

11. Na stronie **Configure Deprovisioning** (Konfigurowanie anulowania zastrzeżenia) kliknij przycisk **Next** (Dalej).

12. Na stronie **Configure Extensions** (Konfigurowanie rozszerzeń) kliknij przycisk **Finish** (Zakończ).


## <a name="create-run-profiles"></a>Tworzenie profilów uruchamiania

Można utworzyć profile uruchamiania dla łączników menedżerów zarządzania usług AD (ADMA) i MIM (MIMMA).

### <a name="create-run-profiles-for-the-adma-connector"></a>Tworzenie profilów uruchamiania dla łącznika menedżera zarządzania usługi AD (ADMA)

W poniższej tabeli zamieszczono pięć profilów uruchamiania tworzonych dla łącznika menedżera ADMA:

| Nazwa | Typ |
| ---- | ---- |
| Profil1 | Pełny import (tylko przemieszczanie) |
| Profil2 | Pełna synchronizacja |
| Profil3 | Import zmian (tylko przemieszczanie) |
| Profil4 | Synchronizacja zmian |
| Profil5 | Eksportowanie |

Aby utworzyć profile uruchamiania dla łącznika menedżera ADMA:

1. Otwórz menedżera usługi synchronizacji i w menu **Tools** (Narzędzia) kliknij polecenie **Management Agents** (Agenci zarządzania).

2. Na liście **Management Agents** (Agenci zarządzania) wybierz pozycję **ADMA**.

3. Aby otworzyć okno dialogowe Configure Run Profiles for (Konfigurowanie profilów uruchamiania dla), w menu **Actions** (Akcje) kliknij pozycję **Configure Run Profiles** (Konfiguruj profile uruchamiania).

4. Dla każdego profilu uruchamiania w tabeli wykonaj następujące czynności:

    - Aby otworzyć kreatora konfigurowania profilu uruchamiania, kliknij przycisk **New Profile** (Nowy profil).

    - W polu **Name** (Nazwa) wpisz nazwę profilu z tabeli, a następnie kliknij przycisk **Next** (Dalej).

    - Na liście **Type** (Typ) wybierz typ kroku z tabeli, a następnie kliknij przycisk **Next** (Dalej).

    - Kliknij przycisk **Finish** (Zakończ), aby utworzyć profil uruchamiania.

5. Aby zamknąć okno dialogowe Configure Run Profiles (Konfigurowanie profilów uruchamiania), kliknij przycisk **OK**.

### <a name="create-run-profiles-for-the-mimma-connector"></a>Tworzenie profilów uruchamiania dla łącznika menedżera zarządzania usługi MIM (MIMMA)

W poniższej tabeli zamieszczono pięć pasujących profilów uruchamiania dla łącznika MIMMA:

| Nazwa | Typ |
| -------- | -------- |
| Profil1 | Pełny import (tylko przemieszczanie) |
| Profil2 | Pełna synchronizacja |
| Profil3 | Import zmian (tylko przemieszczanie) |
| Profil4 | Synchronizacja zmian |
| Profil5 | Eksportowanie |

Aby utworzyć profile uruchamiania dla łącznika menedżera MIMMA:

1. Otwórz menedżera usługi synchronizacji i w menu **Tools** (Narzędzia) kliknij polecenie **Management Agents** (Agenci zarządzania).

2. Z listy **Management Agents** (Agenci zarządzania) wybierz pozycję **MIMMA**.

3. Aby otworzyć okno dialogowe Configure Run Profiles for (Konfigurowanie profilów uruchamiania dla), w menu **Actions** (Akcje) kliknij pozycję **Configure Run Profiles** (Konfiguruj profile uruchamiania).

4. Dla każdego profilu uruchamiania w tabeli wykonaj następujące czynności:

    - Aby otworzyć kreatora konfigurowania profilu uruchamiania, kliknij przycisk **New Profile** (Nowy profil).

    - W polu **Name** (Nazwa) wpisz nazwę profilu z tabeli, a następnie kliknij przycisk **Next** (Dalej).

    - Na liście **Type** (Typ) wybierz typ kroku z tabeli, a następnie kliknij przycisk **Next** (Dalej).

    - Kliknij przycisk **Finish** (Zakończ), aby utworzyć profil uruchamiania.

5. Aby zamknąć okno dialogowe Configure Run Profiles (Konfigurowanie profilów uruchamiania), kliknij przycisk **OK**.

## <a name="configure-the-mim-service"></a>Konfigurowanie usługi MIM

W portalu MIM zostanie utworzona reguła synchronizacji ruchu przychodzącego użytkowników usługi AD dla usługi MIM.

Aby utworzyć regułę synchronizacji ruchu przychodzącego użytkowników usługi AD:

1. Na stronie głównej portalu MIM na pasku nawigacyjnym kliknij pozycję **Administration** (Administracja).

2. Aby otworzyć stronę Synchronization Rules (Reguły synchronizacji), kliknij pozycję **Synchronization Rules** (Reguły synchronizacji).

3. Aby otworzyć kreatora tworzenia reguły synchronizacji, kliknij przycisk **New** (Nowy) na pasku narzędzi.

4. Na karcie **General** (Ogólne) wprowadź następujące informacje, a następnie kliknij przycisk **Next** (Dalej):

    -   Display Name (Nazwa wyświetlana): AD User Inbound Synchronization Rule (Reguła synchronizacji ruchu przychodzącego użytkowników usługi AD)
    -   Data Flow Direction (Kierunek przepływu danych): Inbound (Przychodzący)

5. Na karcie **Scope** (Zakres) wprowadź następujące informacje, a następnie kliknij przycisk **Next** (Dalej):

    -   Metaverse Resource Type (Typ zasobu Metaverse): person (osoba)
    -   External System (System zewnętrzny): ADMA
    -   External System Resource Type (Typ zasobu systemu zewnętrznego): user (użytkownik)

6. Na karcie **Relationship** (Relacja) wprowadź następujące informacje, a następnie kliknij przycisk **Next** (Dalej):

    -   Aby skonfigurować kryteria relacji, wybierz pozycję **ObjectSID** (Identyfikator SID obiektu) z list MetaverseObject:person(Attribute) i ConnectedSystemObject:person(Attribute).

    -   Wybierz pozycję **Create Resource in MIM** (Utwórz zasób w usłudze FIM).

7. Na stronie **Inbound Attribute Flow** (Przepływ atrybutów ruchu przychodzącego) wprowadź następujące informacje, a następnie kliknij przycisk **Next** (Dalej):

    | Reguła przepływu | Źródło | Lokalizacja docelowa |
    |-|-|-|
    |Reguła 1|samAccountName|accountName|
    |Reguła 2|displayName|displayName|
    |Reguła 3|EmployeeType|employeeType|
    |Reguła 4|givenName|firstName|
    |Reguła 5|sn|lastName|
    |Reguła 6|Manager|manager|
    |Reguła 7|objectSid|ObjectSID|
    |Reguła 8|"Contoso"|domain|

    Dla każdego wiersza w tej tabeli wykonaj następujące czynności:

    - Aby otworzyć okno dialogowe Flow Definition (Definicja przepływu), kliknij pozycję **New Attribute Flow** (Nowy przepływ atrybutów).

    - Na karcie **Source** (Źródło) wybierz atrybut wyświetlany dla danego wiersza w tabeli.

    - Na karcie **Destination** (Lokalizacja docelowa) wybierz atrybut wyświetlany dla danego wiersza w tabeli.

    - Aby zastosować konfigurację przepływu atrybutów, kliknij przycisk **OK**.

8. Na karcie **Summary** (Podsumowanie) kliknij przycisk **Submit** (Prześlij).

## <a name="initialize-the-testing-environment"></a>Inicjowanie środowiska testowego
Aby przetestować konfigurację programu MIM z danymi usługi AD, należy wcześniej wykonać cztery kroki:

### <a name="enable-provisioning"></a>Włączanie zastrzegania

1. Otwórz menedżera usługi synchronizacji.

2. Aby otworzyć okno dialogowe Options (Opcje), w menu **Tools** (Narzędzia) kliknij polecenie **Options** (Opcje).

3. Zaznacz pole wyboru **Enable Synchronization Rule Provisioning** (Włącz aprowizację reguły synchronizacji).

4. Aby zamknąć okno dialogowe Options (Opcje), kliknij przycisk **OK**.

### <a name="initialize-the-mimma"></a>Inicjowanie menedżera MIMMA

Wykonaj pełny cykl synchronizacji dla tego łącznika. Kompletny cykl składa się z następujących profilów uruchamiania:

- Pełny import
- Pełna synchronizacja
- Eksportowanie
- Import zmian

Wykonaj następujące czynności, aby uruchomić wymienione cztery profile uruchamiania.

1. Otwórz menedżera usługi synchronizacji i w menu **Tools** (Narzędzia) kliknij polecenie **Management Agents** (Agenci zarządzania).

2. Z listy **Management Agents** (Agenci zarządzania) wybierz pozycję **MIMMA**.

3. Aby otworzyć okno dialogowe Run Management Agent (Uruchamianie agenta zarządzania), w menu **Actions** (Akcje) kliknij polecenie **Run** (Uruchom).

4. Dla każdego wymienionego powyżej profilu uruchamiania wykonaj następujące czynności:

    - Aby otworzyć okno dialogowe Run Management Agent (Uruchamianie agenta zarządzania), w menu **Actions** (Akcje) kliknij polecenie **Run** (Uruchom).

    - Na liście **Run profiles** (Profile uruchamiania) wybierz profil, który chcesz uruchomić.

    - Aby uruchomić profil, kliknij przycisk **OK**.

#### <a name="configure-attribute-flow-precedence"></a>Konfigurowanie pierwszeństwa przepływu atrybutów

Podczas inicjowania łącznika usługi MIM skonfigurowane reguły synchronizacji zostały przeniesione do środowiska metaverse.

Dostosuj pierwszeństwo przepływu atrybutów dla atrybutów wprowadzanych przez ten łącznik, aby umożliwić przepływ atrybutów znajdujących się już w usłudze AD do środowiska metaverse, a później do bazy danych usługi MIM.

### <a name="initialize-the-adma"></a>Inicjowanie menedżera ADMA

Aby zainicjować łącznik usługi Active Directory, należy wykonać na nim pełny import i pełną synchronizację. Pełny import powoduje przeniesienie istniejących obiektów z usługi AD do przestrzeni łącznika. Pełna synchronizacja powoduje zaktualizowanie reguł synchronizacji zgodnie z regułami łącznika usługi MIM.

1. Otwórz menedżera usługi synchronizacji i w menu **Tools** (Narzędzia) kliknij polecenie **Management Agents** (Agenci zarządzania).

2. Na liście **Management Agents** (Agenci zarządzania) wybierz pozycję **ADMA**.

3. Aby otworzyć okno dialogowe Run Management Agent (Uruchamianie agenta zarządzania), w menu **Actions** (Akcje) kliknij polecenie **Run** (Uruchom).

4. Dla każdego wymienionego powyżej profilu uruchamiania wykonaj następujące czynności:

    - Aby otworzyć okno dialogowe Run Management Agent (Uruchamianie agenta zarządzania), w menu **Actions** (Akcje) kliknij polecenie **Run** (Uruchom).

    - Na liście **Run profiles** (Profile uruchamiania) wybierz profil, który chcesz uruchomić.

    - Aby uruchomić profil, kliknij przycisk **OK**.

### <a name="populate-the-mim-service-database"></a>Wypełnianie bazy danych usługi MIM

Aby wypełnić obiektami bazę danych usługi MIM, należy wykonać cykl synchronizacji na łączniku menedżera MIMMA. Ten cykl składa się z następujących elementów:

- Eksportowanie
- Pełny import
- Pełna synchronizacja

Wykonaj następujące czynności, aby uruchomić wymienione trzy profile uruchamiania.

1. Otwórz menedżera usługi synchronizacji i kliknij polecenie **Management Agents** (Agenci zarządzania) w menu **Tools** (Narzędzia).

2. Wybierz pozycję **MIMMA** na liście **Management Agents** (Agenci zarządzania).

3. Kliknij polecenie **Run** (Uruchom) w menu **Actions** (Akcje), aby otworzyć okno dialogowe Run Management Agent (Uruchamianie agenta zarządzania).

4. Dla każdego wymienionego powyżej profilu uruchamiania wykonaj następujące czynności:

    - Kliknij polecenie **Run** (Uruchom) w menu **Actions** (Akcje), aby otworzyć okno dialogowe Run Management Agent (Uruchamianie agenta zarządzania).
    - Wybierz profil, który chcesz uruchomić, z listy **Run profiles** (Profile uruchamiania).
    - Kliknij przycisk **OK**, aby uruchomić profil.

>[!div class="step-by-step"]
[« Usługa i portal MIM](install-mim-service-portal.md)
