---
title: Microsoft Identity Manager 2016 Inicjowanie obsługi administracyjnej użytkowników w usłudze AD | Microsoft Docs
description: Przejdź przez proces tworzenia użytkowników w usłudze ADDS przy użyciu programu Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 149339a6e1029f01378a518a98029c1d588de6f9
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044178"
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>Jak aprowizować użytkowników do usług AD DS

Dotyczy: programu Microsoft Identity Manager 2016 SP1 (MIM)

Jednym z podstawowych wymagań dotyczących systemu zarządzania tożsamościami jest możliwość aprowizacji zasobów do systemu zewnętrznego.

Ten przewodnik przeprowadzi Cię przez główne bloki konstrukcyjne związane z procesem aprowizacji użytkowników z programu Microsoft® Identity Manager (MIM) 2016 do usług Active Directory® Domain Services (AD DS). Ponadto przewodnik opisuje sposób weryfikacji prawidłowego działania scenariusza, zapewnia sugestie dotyczące zarządzania użytkownikami usługi Active Directory przy użyciu programu MIM 2016 i zawiera listy dodatkowych źródeł informacji.

## <a name="before-you-begin"></a>Zanim rozpoczniesz


W tej części znajdziesz informacje dotyczące zakresu tego dokumentu. Ogólnie rzecz biorąc, przewodniki „Jak mogę” są kierowane do czytelników, którzy mają już podstawowe doświadczenie z procesem synchronizacji obiektów z programem MIM zgodnie z informacjami zawartymi w powiązanych [przewodnikach wprowadzających](https://go.microsoft.com/FWLink/p/?LinkId=190486).

### <a name="audience"></a>Grupy odbiorców


Ten przewodnik jest przeznaczony dla profesjonalistów z dziedziny IT, którzy mają już podstawową wiedzę o tym, jak działa proces synchronizacji MIM, zainteresowanych uzyskaniem bezpośredniego doświadczenia i obszerniejszych informacji koncepcyjnych związanych z konkretnymi scenariuszami.

### <a name="prerequisite-knowledge"></a>Wymagana wiedza


Dokument ten zakłada, że masz dostęp do uruchomionego wystąpienia programu MIM oraz że masz doświadczenie w konfigurowaniu prostych scenariuszy synchronizacji zgodnie z informacjami podanymi w następujących dokumentach:

-   [Wprowadzenie do synchronizacji danych wejściowych](https://go.microsoft.com/FWLink/p/?LinkId=189652)

-   [Wprowadzenie do synchronizacji danych wyjściowych](https://go.microsoft.com/FWLink/p/?LinkId=189653)

Zawartość tego dokumentu została opracowana jako rozszerzenie tych dokumentów wprowadzających.

### <a name="scope"></a>Zakres


Scenariusz opisany w tym dokumencie został uproszczony, aby sprostać wymaganiom podstawowego środowiska laboratoryjnego. Głównym celem jest zapewnienie zrozumienia omawianych pojęć i technologii.

Dokument ten pomoże w opracowaniu rozwiązania, które obejmuje grupy zarządzania w usługach AD DS, przy użyciu programu MIM.

### <a name="time-requirements"></a>Wymagania czasowe


Zakończenie procedur opisanych w tym dokumencie wymaga od 90 do 120 minut.

Szacunkowe wymagania czasowe zakładają, że środowisko testowe jest już skonfigurowane, i nie uwzględniają czasu wymaganego na konfigurację środowiska testowego.

### <a name="getting-support"></a>Uzyskiwanie pomocy technicznej


Jeśli masz pytania dotyczące zawartości niniejszego dokumentu lub masz ogólne opinie, które chcesz omówić, możesz opublikować wiadomość na [forum programu Forefront Identity Manager 2010](https://go.microsoft.com/FWLink/p/?LinkId=189654).

## <a name="scenario-description"></a>Opis scenariusza


Fabrikam, fikcyjna firma, planuje użyć programu MIM do zarządzania kontami użytkowników w usługach AD DS korporacji. W ramach tego procesu firma Fabrikam potrzebuje aprowizować użytkowników do usług AD DS. Aby rozpocząć wstępne testowanie, firma Fabrikam zainstalowała podstawowe środowisko laboratoryjne składające się z programu MIM i usług AD DS.
W tym środowisku laboratoryjnym firma Fabrikam testuje scenariusz obejmujący użytkownika, który został ręcznie utworzony w portalu MIM. Celem tego scenariusza jest aprowizacja użytkownika jako włączonego użytkownika ze wstępnie zdefiniowanym hasłem do usług AD DS.

## <a name="scenario-design"></a>Projekt scenariusza


Aby korzystać z tego przewodnika, potrzebujesz trzech składników architektury:

-   Kontroler domeny usługi Active Directory

-   Komputer z uruchomioną usługą FIM Synchronization Service
-   Komputer z uruchomionym portalem programu FIM

Poniższa ilustracja przedstawia wymagane środowisko.

![Wymagane środowisko](media/how-provision-users-adds/image001.png)


Możesz uruchomić wszystkie składniki na jednym komputerze.

> [!NOTE]
> Aby uzyskać więcej informacji dotyczących konfigurowania programu MIM, zobacz [Przewodnik instalacji programu FIM](https://go.microsoft.com/FWLink/p/?LinkId=165845).

## <a name="scenario-components-list"></a>Lista składników scenariusza


W poniższej tabeli zawarto listę składników, które stanowią część scenariusza w tym przewodniku.

| ![Jednostka organizacyjna](media/how-provision-users-adds/image005.jpg)   | Jednostka organizacyjna                | Obiekty MIM — jednostka organizacyjna (OU), której używa się jako celu dla aprowizowanych użytkowników.                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![Konta użytkowników](media/how-provision-users-adds/image006.jpg)   | Konta użytkowników                      | &#183; **ADMA** — konto użytkownika usługi Active Directory z odpowiednimi uprawnieniami do nawiązywania połączenia z usługami AD DS.<br/> &#183; **FIMMA** — konto użytkownika usługi Active Directory z odpowiednimi uprawnieniami do nawiązywania połączenia z programem MIM.
                                                                 |
| ![Agenci zarządzania i profile przebiegu](media/how-provision-users-adds/image007.jpg)  | Agenci zarządzania i profile przebiegu | &#183; **Fabrikam ADMA** — agent zarządzania, który wymienia dane z usługami AD DS. <br/> &#183; Fabrikam FIMMA — agent zarządzania, który wymienia dane z programem MIM.                                                                                 |
| ![Reguły synchronizacji](media/how-provision-users-adds/image008.jpg)  | Reguły synchronizacji              | Reguła synchronizacji ruchu wychodzącego firmy Fabrikam — reguła synchronizacji ruchu wychodzącego, która aprowizuje użytkowników do usług AD DS.                                     |
| ![Zestawy](media/how-provision-users-adds/image009.jpg)   | Zestawy                               | Wszyscy wykonawcy — zestaw z dynamicznym członkostwem dla wszystkich obiektów z wartością Contractor dla atrybutu EmployeeType.                                |
| ![Przepływy pracy](media/how-provision-users-adds/image010.jpg)  | Przepływy pracy                          | Przepływ pracy aprowizacji usługi AD — przepływ pracy przenoszący użytkownika programu MIM do zakresu reguły synchronizacji ruchu wychodzącego usługi AD.                                |
| ![Reguły zasad zarządzania](media/how-provision-users-adds/image011.jpg)   | Reguły zasad zarządzania            | Reguła zasad zarządzania aprowizacji usługi AD — reguła zasad zarządzania (MPR), która zostaje wyzwolona, gdy zasób zostaje elementem członkowskim zestawu Wszyscy wykonawcy. |
| ![Użytkownicy programu MIM](media/how-provision-users-adds/image012.jpg) | Użytkownicy programu MIM                          | Britta Simon — użytkownik programu MIM, którego należy aprowizować do usług AD DS.                                                                                             |



## <a name="scenario-steps"></a>Kroki w scenariuszu


Scenariusz opisany w tym przewodniku składa się z bloków konstrukcyjnych pokazanych na poniższym rysunku.

![Etapy scenariusza](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>Konfiguracja systemów zewnętrznych


W tej sekcji znajdziesz instrukcje dotyczące zasobów spoza środowiska programu MIM, które należy utworzyć.

### <a name="step-1-create-the-ou"></a>Krok 1. Tworzenie jednostki organizacyjnej


Potrzebujesz jednostki organizacyjnej jako kontenera dla aprowizowanego przykładowego użytkownika. Aby uzyskać więcej informacji o tworzeniu jednostek organizacyjnych, zobacz [Tworzenie nowej jednostki organizacyjnej](https://go.microsoft.com/FWLink/p/?LinkId=189655).

Utwórz jednostkę organizacyjną o nazwie MIMObjects w usługach AD DS.

### <a name="step-2-create-the-active-directory-user-accounts"></a>Krok 2. Tworzenie kont użytkowników usługi Active Directory

W ramach scenariusza opisywanego w tym przewodniku potrzebujesz dwóch kont użytkowników usługi Active Directory:

- **ADMA** — używanego przez agenta zarządzania usługi Active Directory;

- **FIMMA** — używanego przez agenta zarządzania usługi FIM Service.

W obu przypadkach wystarczy utworzyć regularne konta użytkowników. Więcej informacji o konkretnych wymaganiach dla obu kont można znaleźć w dalszej części tego dokumentu. Aby uzyskać więcej informacji o tworzeniu użytkowników, zobacz [Tworzenie nowego konta użytkownika](https://go.microsoft.com/FWLink/p/?LinkId=189656).


## <a name="configuring-the-fim-synchronization-service"></a>Konfiguracja usługi FIM Synchronization Service


W ramach kroków konfiguracyjnych opisanych w tej sekcji musisz uruchomić menedżera usługi FIM Synchronization Service.

### <a name="creating-the-management-agents"></a>Tworzenie agentów zarządzania

W ramach scenariusza opisywanego w tym przewodniku musisz utworzyć dwóch agentów zarządzania:

-   **Fabrikam ADMA** — agenta zarządzania dla usług AD DS;

-   **Fabrikam FIMMA** — agenta zarządzania dla agenta zarządzania usług FIM Service.

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>Krok 3. Tworzenie agenta zarządzania Fabrikam ADMA

Podczas konfigurowania agenta zarządzania dla usług AD DS należy określić konto, które jest wykorzystywane przez agenta zarządzania w ramach wymiany danych z usługami AD DS. Należy użyć normalnego konta użytkownika. Niemniej jednak, aby zaimportować dane z usług AD DS, konto musi mieć uprawnienia do sondowania zmian z kontroli DirSync. Jeśli chcesz, aby agent zarządzania eksportował dane do usług AD DS, musisz przyznać odpowiednie uprawnienia dla konta w docelowych jednostkach organizacyjnych. Aby uzyskać więcej informacji na ten temat, zobacz [Konfigurowanie konta ADMA](https://go.microsoft.com/FWLink/p/?LinkId=189657).

Aby utworzyć użytkownika w usługach AD DS, wymaga się przeniesienia nazwy wyróżniającej obiektu na zewnątrz. Ponadto dobrym rozwiązaniem jest przeniesienie imienia, nazwiska i nazwy wyświetlanej, aby zapewnić możliwość wykrycia obiektów.

W usługach AD DS nadal typowe dla użytkowników jest korzystanie z atrybutu sAMAccountName do logowania do usługi katalogu. Jeśli nie określisz wartości dla tego atrybutu, usługa katalogu wygeneruje dla niego losową wartość. Niemniej te losowe wartości nie są przyjazne dla użytkownika, dlatego zazwyczaj przyjazna dla użytkownika wersja tego atrybutu jest częścią eksportu do usług AD DS. Aby umożliwić użytkownikowi logowanie się do usług AD DS, musisz również uwzględnić utworzone hasło przy użyciu atrybutu unicodePwd w logice eksportu.

> [!Note]
> Upewnij się, że wartość określona jako unicodePwd jest zgodna z zasadami haseł docelowych usług AD DS.

Po ustawieniu hasła dla kont AD DS należy również utworzyć konto jako włączone konto. Możesz to zrobić, ustawiając atrybut userAccountControl. Aby uzyskać więcej informacji o atrybucie userAccountControl, zobacz [Using FIM to Enable or Disable Accounts in Active Directory](https://go.microsoft.com/FWLink/p/?LinkId=189658) (Korzystanie z programu FIM do włączania lub wyłączania kont w usłudze Active Directory).

W poniższej tabeli zawarto listę najważniejszych ustawień właściwych dla scenariusza, które należy skonfigurować.

| Strona projektanta agenta zarządzania                          | Konfiguracja                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| Utwórz agenta zarządzania                                 | 1. **agent zarządzania dla:** AD DS  <br/> 2. **Nazwa:** Fabrikam adma |
| Łączenie z lasem usługi Active Directory                      | 1. **Wybierz partycje katalogu:** "DC = FABRIKAM, DC = com"   <br/>   2. kliknij pozycję **Containers (kontenery** ), aby otworzyć okno dialogowe **Wybieranie kontenerów** i upewnić się, że **MIMObjects** jest jedyną wybraną jednostką organizacyjną.        |
| Wybierz typy obiektów                                     | Poza już wybranymi typami obiektów wybierz **użytkownika**. |
| Wybierz atrybuty                                       | 1. kliknij przycisk **Pokaż wszystko.** <br/>   2. Wybierz następujące atrybuty: <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

Więcej informacji znajduje się w następujących tematach pomocy:
- Tworzenie agenta zarządzania
- Łączenie z lasem usługi Active Directory
- Korzystanie z agenta zarządzania dla usługi Active Directory
- Konfiguruj partycje katalogu

> [!Note]
> Upewnij się, że skonfigurowano regułę przepływu atrybutu importowania dla atrybutu ExpectedRulesList.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>Krok 4. Tworzenie agenta zarządzania Fabrikam FIMMA

Podczas konfigurowania agenta zarządzania usługi FIM Service musisz określić konto, które jest wykorzystywane przez agenta zarządzania w wymianie danych z usługą FIM Service.

Należy użyć normalnego konta użytkownika. Konto musi być tym samym kontem, które zostało określone podczas instalacji programu MIM. Aby uzyskać informacje o skrypcie, którego możesz użyć do określenia nazwy konta FIMMA podanego podczas instalacji i do przetestowania, czy to konto jest nadal ważne, zobacz Using Windows PowerShell to Do a [FIM MA Account Configuration Quick Test](https://go.microsoft.com/FWLink/p/?LinkId=189659) (Korzystanie z programu Windows PowerShell do wykonania szybkiego testu konfiguracji konta FIM MA).

W poniższej tabeli zawarto listę najważniejszych ustawień właściwych dla scenariusza, które należy skonfigurować. Utwórz agenta zarządzania w oparciu o informacje podane w poniższej tabeli.  

| Strona projektanta agenta zarządzania | Konfiguracja |
|------------|------------------------------------|
| Utwórz agenta zarządzania | 1. **agent zarządzania dla:** agent zarządzania usługą FIM Service <br/> 2. **Nazwa** Fabrikam FIMMA |
| Łączenie z bazą danych     | Użyj następujących ustawień: <br/> &#183; **Serwer:** localhost <br/> &#183; **Baza danych:** FIMService <br/> **Adres podstawowy usługi &#183; FIM:**http://localhost:5725 <br/> <br/> Podaj informacje o koncie utworzonym dla tego agenta zarządzania |
| Wybierz typy obiektów                                     | Poza już wybranymi typami obiektów wybierz **osobę**.   |
| Skonfiguruj mapowania typów obiektów                          | Poza już istniejącymi mapowaniami typów obiektów dodaj mapowanie dla osoby **Typ obiektu źródła danych** do osoby typu obiektu **Metaverse**. |
| Konfiguruj przepływ atrybutów                                | Poza już istniejącymi mapowaniami przepływu atrybutów dodaj następujące mapowania przepływu atrybutów: <br/><br/> ![Przepływ atrybutów](media/how-provision-users-adds/image018.jpg) |




Więcej informacji znajduje się w następujących tematach pomocy:
-   Tworzenie agenta zarządzania

-   Łączenie z bazą danych usługi Active Directory

-   Korzystanie z agenta zarządzania dla usługi Active Directory

-   Konfiguruj partycje katalogu

> [!NOTE]
>  Upewnij się, że skonfigurowano regułę przepływu atrybutu importowania dla atrybutu ExpectedRulesList.

### <a name="step-5-create-the-run-profiles"></a>Krok 5. Tworzenie profilów przebiegu

W poniższej tabeli wymieniono profile przebiegu, które należy utworzyć w ramach scenariusza opisywanego w tym przewodniku.

| Agent zarządzania  | Profil przebiegu     |
|-------------------|--------------------------------------|
| Fabrikam ADMA     | 1. pełny import  <br/> 2. Pełna synchronizacja <br/> 3. import Delta <br/> 4. Synchronizacja różnicowa <br/> 5. Eksportowanie                                                                    |
| Fabrikam FIMMA   | 1. pełny import <br> 2. Pełna synchronizacja <br/> 3. import Delta <br/> 4. Synchronizacja różnicowa <br/> 5. Eksportowanie|                                                                                                                                                                                   

Utwórz profile przebiegu dla każdego agenta zarządzania zgodnie z poprzednią tabelą.


> [!Note]
> Aby uzyskać więcej informacji, zobacz Create a Management Agent Run Profile (Tworzenie profilu przebiegu agenta zarządzania) w Pomocy programu MIM.                                                                                                                  
> 
> 
> [!Important]
>  Upewnij się, że aprowizacja jest włączona w Twoim środowisku. Można to zrobić, uruchamiając skrypt przy użyciu programu Windows PowerShell w celu włączenia aprowizacji (https://go.microsoft.com/FWLink/p/?LinkId=189660).


## <a name="configuring-the-fim-service"></a>Konfigurowanie usługi FIM Service


W ramach scenariusza opisanego w tym przewodniku należy skonfigurować zasady aprowizacji zgodnie z poniższym rysunkiem.

![Zasady zastrzegania](media/how-provision-users-adds/image019.png)

Celem tych zasad aprowizacji jest wprowadzenie grup do zakresu reguły synchronizacji ruchu wychodzącego użytkowników usługi AD. Przenosząc zasób do zakresu reguły synchronizacji, włączasz aparat synchronizacji, aby aprowizować zasób do usług AD DS zgodnie z konfiguracją.

Aby skonfigurować usługę FIM, przejdź do http://localhost/identitymanagementprogramu Windows Internet Explorer®. Na stronie portalu MIM przejdź do powiązanych stron w sekcji Administracja, aby utworzyć zasady aprowizacji. Aby sprawdzić konfigurację, należy uruchomić skrypt opisany w artykule [Using Windows PowerShell to document your provisioning policy configuration](https://go.microsoft.com/FWLink/p/?LinkId=189661) (Korzystanie z programu Windows PowerShell do udokumentowania konfiguracji zasad aprowizacji).

### <a name="step-6-create-the-synchronization-rule"></a>Krok 6. Tworzenie reguły synchronizacji

W poniższych tabelach przedstawiono konfigurację wymaganej reguły synchronizacji aprowizacji firmy Fabrikam. Utwórz regułę synchronizacji zgodnie z danymi w następujących tabelach.

| Konfiguracja reguły synchronizacji                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Nazwa                                                                                                       | Reguła synchronizacji ruchu wychodzącego użytkowników usługi Active Directory                         |                                                          
| Opis                                                                                               |                                                                             |                                                           
| Pierwszeństwo                                                                                                | 2                                                                           |                                                           
| Kierunek przepływu danych   | Wychodzący             |       
| Zależność       |         |                                         


| Zakres |                                                                             |                                                           
|--------|-------|
| Typ zasobu Metaverse | osoba |                                                         
| System zewnętrzny                   |Fabrikam ADMA                                                               |                                                       
| Typ zasobu systemu zewnętrznego                                                                              | użytkownik      



| Relacja ||
|------------|---------|
| Utwórz zasób w systemie zewnętrznym                                                                         | True                                                                        |                                                           
| Włącz anulowanie aprowizacji                                                                                      | Fałsz                                                                       |                                                           

| Kryteria relacji                                                                                      | |
|------------|----------|
| Atrybut ILM     | Atrybut źródła danych                                                       |
| Atrybut źródła danych         | sAMAccountName    |

| Początkowe przepływy atrybutów wychodzących        | |                                                             |
|-------------------|---------------------- |---------------|
| Zezwalaj na wartości null                 | Element docelowy                                                                 | Element źródłowy                                                    |
| false                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| false                       | userAccountControl                                                          | **Stała:** 512                                         |
| false                                                                     | unicodePwd                    | Stała: P\@\$\$W0rd                                    |

| Trwałe przepływy atrybutów wychodzących  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| Zezwalaj na wartości null                                                                                                | Element docelowy                                                                 | Element źródłowy                                                    |
| false                                                                                                      | sAMAccountName                                                              | accountName                                               |
| false                                                                                                      | displayName                                                                 | displayName                                               |
| false                                                                                                      | givenName                                                                   | firstName                                                 |
| false                                                                                                      | sn                                                                          | lastName                                                  |



> [!NOTE]
>  Ważne: sprawdź, czy wybrano opcję Tylko przepływ początkowy dla przepływu atrybutu, którego miejscem docelowym jest DN.                                                                          

### <a name="step-7-create-the-workflow"></a>Krok 7. Tworzenie przepływu pracy

Celem przepływu pracy aprowizacji AD jest dodanie reguły synchronizacji aprowizacji firmy Fabrikam do zasobu. W poniższych tabelach przedstawiono konfigurację.  Utwórz przepływ pracy zgodnie z danymi w poniższych tabelach.

| Konfiguracja przepływu pracy               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Nazwa                                 | Przepływ pracy aprowizacji użytkownika usługi Active Directory                     |
| Opis                          |                                                                 |
| Typ przepływu pracy                        | Akcja                                                          |
| Uruchom podczas aktualizacji zasad                 | Fałsz                                                           |

| Reguła synchronizacji                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Nazwa                                 | Reguła synchronizacji ruchu wychodzącego użytkowników usługi Active Directory             |
| Akcja                               | Dodaj                                                             |




### <a name="step-8-create-the-mpr"></a>Krok 8. Tworzenie reguły MPR

Wymagana reguła MPR to reguła typu Przejście między zestawami i jest wyzwalana, gdy zasób staje się elementem członkowskim zestawu Wszyscy wykonawcy. W poniższych tabelach przedstawiono konfigurację.  Utwórz regułę MPR zgodnie z danymi w poniższych tabelach.

| Konfiguracja reguły MPR                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Nazwa                                 | Reguła zasad zarządzania aprowizacją użytkownika usługi AD                 |
| Opis                          |                                                             |
| Typ                                 | Przejście między zestawami                                              |
| Przyznaje uprawnienia                   | Fałsz                                                       |
| Disabled (Wyłączony)                             | Fałsz                                                       |

| Definicja przejścia                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Typ przejścia                      | Przejście do                                               |
| Zestaw przejścia                       | Wszyscy wykonawcy                                             |

| Przepływy pracy zasad                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Typ                                 | Akcja                                                      |
| Nazwa wyświetlana                         | Przepływ pracy aprowizacji użytkownika usługi Active Directory                 |




## <a name="initializing-your-environment"></a>Inicjowanie środowiska


Cele fazy inicjowania są następujące:

-   przeniesienie reguły synchronizacji do obiektu Metaverse;

-   przeniesienie struktury usługi Active Directory do obszaru łącznika usługi Active Directory.

### <a name="step-9-run-the-run-profiles"></a>Krok 9. Uruchomienie profilów przebiegu

W poniższej tabeli zawarto listę profilów przebiegu, które są częścią fazy inicjowania.  Uruchom profile przebiegu zgodnie z poniższą tabelą.

| Run                                                                                                           | Agent zarządzania                                      | Profil przebiegu          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | Fabrikam FIMMA                                        | Pełny import          |
| 2                                                                                                             |                                                       | Pełna synchronizacja |
| 3                                                                                                             |                                                       | Eksportowanie               |
| 4                                                                                                             |                                                       | Import zmian         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | Fabrikam ADMA                                         | Pełny import          |
| 6                                                                                                             |                                                       | Pełna synchronizacja |



> [!NOTE]
> Należy sprawdzić, czy reguła synchronizacji ruchu wychodzącego została pomyślnie przekazana do obiektu Metaverse.

## <a name="testing-the-configuration"></a>Testowanie konfiguracji


Celem tej sekcji jest przetestowanie faktycznej konfiguracji. Aby przetestować konfigurację, możesz:

1.  utworzyć przykładowego użytkownika w portalu programu FIM;

2.  zweryfikować wymagania dotyczące aprowizacji przykładowego użytkownika;

3.  aprowizować przykładowego użytkownika do usług AD DS;

4.  sprawdzić, czy użytkownik istnieje w usługach AD DS.

### <a name="step-10-create-a-sample-user-in-mim"></a>Krok 10. Tworzenie przykładowego użytkownika w programie MIM


Poniższa tabela zawiera listę właściwości przykładowego użytkownika. Utwórz przykładowego użytkownika zgodnie z danymi w poniższej tabeli.

| Atrybut                              | Wartość                                                          |
|----------------------------------------|----------------------------------------------------------------|
| Imię                             | Britta                                                         |
| Nazwisko                              | Simon                                                          |
| Nazwa wyświetlana                           | Britta Simon                                                   |
| Nazwa konta                           | BSimon                                                         |
| Domain                                 | Fabrikam                                                       |
| Typ pracownika                          | Contractor                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>Weryfikacja wymagań dotyczących aprowizacji przykładowego użytkownika


Aby aprowizować przykładowego użytkownika do usług AD DS, należy spełnić dwa wymagania wstępne:

1.  Użytkownik musi być członkiem zestawu Wszyscy wykonawcy.

2.  Ustawiony użytkownik musi znajdować się w zakresie reguły synchronizacji ruchu wychodzącego.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>Krok 11. Sprawdzenie, czy użytkownik jest członkiem zestawu Wszyscy wykonawcy

Aby sprawdzić, czy użytkownik jest członkiem zestawu Wszyscy wykonawcy, otwórz zestaw i kliknij pozycję Wyświetl członków.

![Sprawdzenie, czy użytkownik jest członkiem zestawu Wszyscy wykonawcy](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>Krok 12. Sprawdzenie, czy użytkownik znajduje się w zakresie reguły synchronizacji ruchu wychodzącego

Aby sprawdzić, czy użytkownik znajduje się w zakresie reguły synchronizacji, Otwórz stronę właściwości użytkownika i przejrzyj oczekiwany atrybut lista reguł na karcie aprowizacji. Atrybut listy oczekiwanych reguł powinien być listą użytkowników usługi AD

użytkownika usługi AD. Poniższy zrzut ekranu przedstawia przykład atrybutu lista oczekiwanych reguł.

![Stan reguły synchronizacji](media/how-provision-users-adds/image023.jpg)

Na tym etapie procesu stan reguły synchronizacji to Oczekująca. Oznacza to, że reguła synchronizacji nie została jeszcze zastosowana wobec użytkownika.



### <a name="step-13-synchronize-the-sample-group"></a>Krok 13. Synchronizacja przykładowej grupy


Przed rozpoczęciem pierwszego cyklu synchronizacji dla obiektu testowego należy prześledzić oczekiwany stan obiektu po każdym profilu przebiegu uruchamianym w planie testowania. Plan testowania powinien obejmować ogólny stan obiektu (utworzony, zaktualizowany lub usunięty) oraz oczekiwane wartości atrybutów.
Użyj planu testowania, aby zweryfikować oczekiwania dotyczące planu testowania. Jeśli krok nie zwraca oczekiwanych wyników, nie przechodź do następnego kroku przed rozwiązaniem rozbieżności między oczekiwanymi i faktycznymi wynikami.

Aby zweryfikować oczekiwania, możesz użyć jako pierwszego wskaźnika statystyk synchronizacji. Przykładowo jeśli oczekujesz, że nowe obiekty zostaną umieszczone w obszarze łącznika, ale statystyka importu nie zwraca żadnych elementów „Dodaje”, oznacza to, że jakiś element środowiska nie działa zgodnie z oczekiwaniami.

![Statystyki synchronizacji](media/how-provision-users-adds/image024.jpg)

Statystyki synchronizacji mogą stanowić pierwsze wskazanie prawidłowego działania scenariusza, niemniej należy użyć funkcji wyszukiwania obszaru łącznika i wyszukiwania obiektu Metaverse w menedżerze usługi synchronizacji, aby zweryfikować oczekiwane wartości atrybutów.

Aby zsynchronizować użytkownika z usługami AD DS:

1.  zaimportuj użytkownika do obszaru łącznika FIM MA;

2.  odwzoruj użytkownika w obiekcie Metaverse;

3.  aprowizuj użytkownika do obszaru łącznika usługi Active Directory;

4.  wyeksportuj informacje o stanie do programu FIM;

5.  wyeksportuj użytkownika do usług AD DS;

6.  potwierdź utworzenie użytkownika.

Aby wykonać te zadania, uruchamia się następujące profile przebiegu.

| Agent zarządzania | Profil przebiegu  |
|------------------|--------------|
| Fabrikam FIMMA   | 1. import Delta <br/> 2. Synchronizacja różnicowa <br/> 3. Eksportuj <br/> 4. import Delta |
| Fabrikam FIMMA   | 1. eksport <br/> 2. import Delta       |


Po zaimportowaniu z bazy danych usługi FIM, Britta Simon i obiekt ExpectedRuleEntry, który łączy Britta z regułą synchronizacji wychodzącej użytkownika usługi AD, są przemieszczane w obszarze łącznika Fabrikam FIMMA. Podczas przeglądania właściwości Britta w obszarze łącznika obok wartości atrybutów, które zostały skonfigurowane w portalu programu FIM, można również znaleźć prawidłowe odwołanie do oczekiwanego obiektu wpisu reguły. Poniższy zrzut ekranu przedstawia przykład tego elementu.

![Właściwości obiektu obszaru łącznika](media/how-provision-users-adds/image025.jpg)

Celem przebiegu synchronizacji zmian łącznika Fabrikam FIMMA jest wykonanie kilku operacji:

-   Projekcja — obiekt nowego użytkownika oraz powiązany obiekt oczekiwanego wpisu reguły zostają odwzorowane w obiekcie Metaverse.

-   Aprowizacja — nowo odzwierciedlony obiekt Britta Simon jest aprowizowany do obszaru łącznika Fabrikam ADMA.

-   Eksportowanie przepływów atrybutów — eksportowanie przepływów atrybutów ma miejsce w obu agentach zarządzania. W łączniku Fabrikam ADMA nowo aprowizowany obiekt Britta Simon jest uzupełniany nowymi wartościami atrybutów. W łączniku Fabrikam FIMMA istniejący obiekt Britta Simon i powiązany obiekt ExpectedRuleEntry zostają zaktualizowane o wartości atrybutów, które są wynikiem projekcji.

![Statystyki synchronizacji](media/how-provision-users-adds/image026.jpg)

Zgodnie ze wskazaniem statystyk synchronizacji działanie aprowizacji zostało wykonane w obszarze łącznika Fabrikam ADMA. Jeśli zweryfikujesz właściwości obiektu Metaverse w pozycji Britta Simon, okaże się, że działanie jest wynikiem atrybutu ExpectedRulesList uzupełnionego prawidłowym odwołaniem.

![właściwości obiektu Metaverse](media/how-provision-users-adds/image027.jpg)

Podczas następującej operacji eksportowania Fabrikam FIMMA stan reguły synchronizacji obiektu Britta Simon jest aktualizowany z Oczekujące na Zastosowano, co wskazuje, że reguła synchronizacji ruchu wychodzącego jest teraz aktywna dla obiektu w obiekcie Metaverse.

![Zastosowana reguła synchronizacji](media/how-provision-users-adds/image028.jpg)

Ponieważ nowy obiekt został aprowizowany do obszaru łącznika ADMA, w tym agencie zarządzania powinna znaleźć się jedna operacja eksportowania oczekująca na dodanie. 

![Oczekujące operacje eksportowania dla agenta zarządzania](media/how-provision-users-adds/image029.jpg)

W programie FIM każdy przebieg eksportowania wymaga zakończenia następującego importu zmian przed operacją eksportowania. Import zmian uruchamiany po poprzednim przebiegu eksportowania zwany jest importem potwierdzającym. Importy potwierdzające są wymagane, aby usługa FIM Synchronization Service mogła spełniać odpowiednie wymagania w zakresie aktualizacji podczas kolejnych przebiegów synchronizacji.


Uruchom profile przebiegu zgodnie z instrukcjami w tej sekcji.

> [!IMPORTANT]
> Każde uruchomienie profilu przebiegu musi zakończyć się bez błędów.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>Krok 14. Zweryfikowanie aprowizowanego użytkownika w usługach AD DS

Aby upewnić się, że przykładowy użytkownik został aprowizowany do usług AD DS, otwórz jednostkę organizacyjną FIMObjects. W jednostce organizacyjnej FIMObjects powinna znajdować się Britta Simon.

![weryfikowanie użytkownika odbywa się w jednostce organizacyjnej FIMObjects](media/how-provision-users-adds/image033.jpg)

## <a name="summary"></a>Podsumowanie


Celem tego dokumentu jest wprowadzenie do głównych bloków konstrukcyjnych związanych z synchronizacją użytkownika w programie MIM za pomocą usług AD DS. W początkowej fazie testowania należy najpierw zacząć od minimalnych atrybutów wymaganych do zakończenia zadania i dodawać więcej atrybutów do scenariusza, gdy ogólne etapy będą działać zgodnie z oczekiwaniami. Utrzymywanie minimalnej złożoności upraszcza proces rozwiązywania problemów.

Podczas testowania konfiguracji bardzo prawdopodobne jest, że dojdzie do usunięcia i ponownego utworzenia nowych obiektów testowych. Dla obiektów z

uzupełnionym atrybutem ExpectedRulesList może to spowodować powstanie oddzielonych obiektów ERE.
Aby uzyskać opis sposobu usuwania tych obiektów ze środowiska testowego, zobacz [A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](https://go.microsoft.com/FWLink/p/?LinkId=189667) (Metoda usuwania oddzielonych obiektów ExpectedRuleEntry ze środowiska).

W typowym scenariuszu synchronizacji, obejmującym usługi AD DS jako cel synchronizacji, program MIM nie jest autorytatywny dla wszystkich atrybutów obiektu. Na przykład podczas zarządzania obiektami użytkowników w usługach AD DS przy użyciu programu FIM należy jako minimum przekazać atrybuty domeny i objectSID przez agenta zarządzania usług AD DS.
Atrybuty nazwy konta, domeny i objectSID są wymagane, jeśli chcesz umożliwić użytkownikowi logowanie do portalu programu FIM. Aby uzupełnić te atrybuty z usługi AD DS, wymagana jest dodatkowa reguła synchronizacji ruchu przychodzącego dla obszaru łącznika usług AD DS. W przypadku zarządzania obiektami z wieloma źródłami wartości atrybutów należy upewnić się, że pierwszeństwo przepływu atrybutów zostało skonfigurowane prawidłowo. Jeśli pierwszeństwo przepływu atrybutów nie jest skonfigurowane prawidłowo, aparat synchronizacji zablokuje uzupełnianie wartości atrybutów. Więcej informacji o pierwszeństwie przepływu atrybutów znajduje się w artykule [About Attribute Flow Precedence](https://go.microsoft.com/FWLink/p/?LinkId=189675) (Informacje o pierwszeństwie przepływu atrybutów).

## <a name="next-steps"></a>Następne kroki

[Using FIM to Enable or Disable Accounts in Active Directory](https://go.microsoft.com/FWLink/p/?LinkId=189670) (Korzystanie z programu FIM do włączania lub wyłączania kont w usłudze Active Directory)

[Informacje o przetwarzaniu atrybutów odwołania](https://go.microsoft.com/FWLink/p/?LinkId=189671)

[Jak zarządzać kontem usługi FIM](https://go.microsoft.com/FWLink/p/?LinkId=189672)

[Wykrywanie kont nieautorytatywnych — część 1: Planowanie](https://go.microsoft.com/FWLink/p/?LinkId=189673)

[Jak wykryć łączniki](https://go.microsoft.com/FWLink/p/?LinkId=189674)

[Jak skonfigurować konto ADMA](https://go.microsoft.com/FWLink/p/?LinkId=189657)

[About Attribute Flow Precedence](https://go.microsoft.com/FWLink/p/?LinkId=189675) (Informacje o pierwszeństwie przepływu atrybutów)

[Informacje o eksportach](https://social.technet.microsoft.com/wiki/contents/articles/1861.understanding-exports-in-ilm-2007.aspx)
