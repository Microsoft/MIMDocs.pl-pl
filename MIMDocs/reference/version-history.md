---
title: Historia wersji programu Identity Manager | Microsoft Docs
description: W tym artykule opisano różne zmiany wprowadzone w ramach aktualizacji programu MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fa4e5ade14c1a3df94b868d149472c1b42d1c59c
ms.sourcegitcommit: d6178a67014d66d37056c13d10328ae03e3cd781
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/27/2020
ms.locfileid: "92761033"
---
# <a name="identity-manager-version-release-history"></a>Historia wersji programu Identity Manager

Zespół Microsoft Identity Manager regularnie publikuje aktualizacje. Ten artykuł został zaprojektowany z myślą o śledzeniu wersji, które zostały wydane. Następnie można określić, czy masz już najnowszą wersję dodatku Service Pack i poprawki, czy też trzeba przeprowadzić uaktualnienie. 

>[!NOTE]
>W tym artykule opisano tylko wersje składnika synchronizacji programu MIM, usługi i portalu, klienta, CM i składnika PAM.  Nie masz pewności, jakie składniki są potrzebne?  Zobacz [Microsoft Identity Manager Licencjonowanie i pobieranie](../microsoft-identity-manager-licensing.md).
>
>Historię wersji składników Microsoft pakietu BHOLD Suite można znaleźć w obszarze historia wersji [modułów pakietu BHOLD](version-bhold-history.md).
>
>Historię wersji ogólnych usług LDAP, ogólnych SQL, Web Services, PowerShell, Graph i Lotus Domino można znaleźć na stronie [historia wersji łącznika](microsoft-identity-manager-2016-connector-version-history.md).  

## <a name="mim-version-462630"></a>4.6.263.0 wersja programu MIM
- Stan: 7 sierpnia 2020
- [Pobieranie poprawki](https://www.microsoft.com/download/details.aspx?id=101612)
- [Artykuł KB 4576473](https://support.microsoft.com/help/4576473)

Ta poprawka zawiera aktualizacje zarządzanie certyfikatami w usłudze MIM, Menedżera synchronizacji programu MIM i składników usługi PAM.


## <a name="mim-version-462580"></a>4.6.258.0 wersja programu MIM
- Stan: 22 maja 2020
- [Pobieranie poprawki](https://www.microsoft.com/download/details.aspx?id=101305)
- [Artykuł KB 4560335](https://support.microsoft.com/help/4560335)

Ta poprawka zawiera aktualizacje usługi programu MIM, portalu programu MIM i składników PAM.


## <a name="mim-version-46340"></a>4.6.34.0 wersja programu MIM
* Stan: MIM 2016 z dodatkiem Service Pack 2 (SP2) z października, 2019
* Odpowiedni numer wersji pakietu BHOLD: 6.0.62.0
- [Pobieranie poprawki](https://www.microsoft.com/download/details.aspx?id=100412)
- [Artykuł KB 4512924](https://support.microsoft.com/help/4512924)
- Plik ISO można pobrać [z VLSC](https://www.microsoft.com/Licensing/servicecenter/default.aspx) lub za pomocą  [programu Visual Studio downloads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202)

> [!IMPORTANT]
>- .NET Framework 4,6 jest wymagana<br>
>- Wymagany jest [pakiet redystrybucyjny Visual C++ 2013 (vcredist_x64.exe lub vcredist_x86.exe)](https://www.microsoft.com/download/details.aspx?id=40784)<br>

>[!NOTE]
> Zapoznaj się z artykułem [Przewodnik wdrażania programu MIM](../microsoft-identity-manager-deploy.md) , aby uzyskać zaktualizowaną listę wymagań wstępnych i ograniczeń związanych z protokołem TLS 1,2, obsługa kont usług zarządzanych przez grupę i [ścieżka uaktualnienia z wcześniejszych wersji programu MIM i programu FIM](../microsoft-identity-manager-2016-service-pack-2-upgrade-path.md).

Pakiet zbiorczy programu Service Pack 2 (SP2) (build 4.6.34.0) jest dostępny dla Microsoft Identity Manager (MIM) 2016. Jest to aktualizacja zbiorcza, która zastępuje wcześniejsze aktualizacje programu MIM 2016 SP1 4.4.1302.0 przez kompilację 4.5.412.0.
### <a name="updates-in-mim-2016-service-pack-2"></a>Aktualizacje w programie MIM 2016 z dodatkiem Service Pack 2

#### <a name="mim-client-addons"></a>Dodatki klienta programu MIM
- Dodano obsługę dodatku MIM programu Outlook do załadowania do programu Outlook w celu Microsoft 365 wersji, którą można uruchomić.

#### <a name="service-and-portal"></a>Usługa i portal
- Dodano obsługę usługi i portalu programu MIM do zainstalowania w systemie Windows Server 2019 i używają SQL Server 2017, Exchange Server 2019, SharePoint 2019, System Center Service Manager magazynu danych 2019
- Włączono instalację portalu i usługi w programie MIM w środowiskach TLS 1,2.
- Włączono instalację usługi programu MIM, resetowania haseł i witryn sieci Web rejestracji haseł, usługi monitorowania PAM, usługi składnika PAM do używania kont usług zarządzanych przez grupę.
- Dodano parametr Instalatora "keepSQLjobs".
- Zadania danych czasowych agenta programu MIM SQL Server nie zaczynają już działać w ramach pomocniczych replik grup dostępności Always-On SQL.
- Atrybuty wirtualne "ExplicitMember. Add" i "ExplicitMember. Remove" włączone dla niestandardowych typów obiektów w formularzach RCDC do pracy ze zmianami różnicowymi.

#### <a name="synchronization-service"></a>Usługa synchronizacji
- Dodano obsługę usługi synchronizacji programu MIM, która ma zostać zainstalowana w systemie Windows Server 2019, i użyj SQL Server 2017, Exchange Server 2019
- Włączono instalację usługi synchronizacji programu MIM w środowiskach tylko TLS 1,2.
- Włączono instalację usługi synchronizacji programu MIM do korzystania z konta usługi zarządzanego przez grupę.
- Dodano opcję "Użyj konta MIMSync" dla agenta zarządzania usługą programu MIM.
 
#### <a name="privilege-access-management"></a>Zarządzanie dostępem do uprawnień 
- Polecenie cmdlet "Get-PAMRequest" programu PowerShell zwraca dodatkową właściwość "FIMRequestID".


## <a name="mim-version-454120"></a>4.5.412.0 wersja programu MIM
> [!IMPORTANT]
>- Nie można renderować formularza RCDC dla obiektów grupy, gdy wartość atrybutu "displayedOwner" nie jest wypełniona. Po wystąpieniu tego błędu nie będzie można edytować/przeglądać grup.<br>

- Data dostępności: 10 maja 2019
- [Pobieranie poprawki](https://www.microsoft.com/en-us/download/details.aspx?id=58213)
- [Artykuł KB 4489646](https://support.microsoft.com/en-us/help/4489646/hotfix-rollup-4-5-412-0-available-for-mim-2016-sp1)

Ta poprawka zawiera aktualizacje dotyczące synchronizacji programu MIM, usługi MIM, portalu programu MIM, zarządzanie certyfikatami w usłudze MIM i składników PAM.  Jest to aktualizacja zbiorcza, która zastępuje wcześniejsze aktualizacje programu MIM 2016 SP1 4.4.1302.0 przez kompilację 4.5.286.0.

> [!IMPORTANT]
>- Dla Instalatora wymagane są również .NET Framework 4,6 <br>
>- Wymagane [pakiety redystrybucyjne Visual C++ 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe)<br>

## <a name="mim-hybrid-reporting-agent-version-31410"></a>Hybrydowy Agent raportowania programu MIM w wersji 3.1.41.0
- Data dostępności: 22 kwietnia 2019
- [Pobieranie hybrydowego agenta raportowania](https://www.microsoft.com/en-us/download/details.aspx?id=55112)

Hybrydowy Agent raportowania programu MIM w wersji 3.1.41.0 zawiera aktualizacje protokołu TLS 1,2 i poprawkę błędu.  Agenta raportowania hybrydowego można używać z programem MIM 2016 SP1 w wersji 4.4.1302.0 lub nowszej oraz subskrypcją Azure AD — wersja Premium.


## <a name="mim-version-452860"></a>4.5.286.0 wersja programu MIM
- Stan: 19 listopada 2018
- [Pobieranie poprawki](https://www.microsoft.com/en-us/download/details.aspx?id=57596)
- [Artykuł KB 4469694](https://support.microsoft.com/en-us/help/4469694/hotfixrolluppackagebuild452860isavailableformicrosoftidentitymanager20)


Ta poprawka zawiera aktualizacje usługi programu MIM, portalu programu MIM i składników PAM.  Dla Instalatora jest wymagany .NET Framework 4,6.


## <a name="version-452020"></a>4.5.202.0 wersja
- Stan: 30 sierpnia 2018
- [Wersja KB KB](https://support.microsoft.com/en-us/help/4346632)

> [!IMPORTANT]
>- Dla Instalatora wymagane są również .NET Framework 4,6 <br>
>- Wymagane [pakiety redystrybucyjne Visual C++ 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe)<br>
>- Zaktualizowano obsługiwane ustawienia regionalne do nowych standardów ISO ([tutaj](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Oznacza nowe ulepszenie 

#### <a name="mim-service"></a>Usługa MIM
- Integracja z serwerem usługi Azure MFA — alternatywny dostawca Multi-Factor Authentication

#### <a name="privilege-access-management"></a>Zarządzanie dostępem do uprawnień 
- Nie można uruchomić interfejsu API REST usługi PAM, ponieważ nie można załadować pliku lub zestawu

#### <a name="microsoft-identity-portal"></a>Portal tożsamości firmy Microsoft
- W portalu są wyświetlane nieprawidłową długość tabeli
- Okno dialogowe wyszukiwania zaawansowanego w portalu, paski przewijania nie są wyświetlane prawidłowo
- Weryfikacja podpisu silnej nazwy pakietu Language Pack nie powiodła się

#### <a name="certificate-management"></a>Zarządzanie certyfikatami
- Instrukcja redirect powiązania dla interfejsu API REST

## <a name="version-45260"></a>4.5.26.0 wersja
- Stan: 30 czerwca 2018
- [KB4073679 wydania KB](https://support.microsoft.com/en-us/help/4073679)

> [!IMPORTANT]
>- Dla Instalatora wymagane są również .NET Framework 4,6 <br>
>- Wymagane [pakiety redystrybucyjne Visual C++ 2013 x64 (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe)<br>
>- Zaktualizowano obsługiwane ustawienia regionalne do nowych standardów ISO ([tutaj](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support))<br>
>- * Oznacza nowe ulepszenie 

#### <a name="synchronization-service"></a>Usługa synchronizacji
- * Obsługa kont usług zarządzanych przez grupę
- * Obsługa programu Visual Studio (Visual Studio 2013, Visual Studio 2015, Visual Studio 2017)
- Aktualizacje MIISACTIVATE.EXE, dodano obsługę gMSA 
    - nie gMSA: Miisactivate.exe c:\configBU\ miiserver_01. bin "contoso\mimSyncService" *
    - gMSA: Miisactivate.exe c:\configBU\ miiserver_01. bin "contoso\mimSyncService"
- Aktualizacje MIISKMU.exe, dodano obsługę gMSA 
    - non-gMSA:MIISKMU.exe/e c:\configBU\ miiserver_02. bin "/u:" contoso\mimSyncService "
    - gMSA:MIISKMU.exe/e c:\configBU\ miiserver_02. bin "/u:" contoso\mimSyncService "*
- Zaktualizowane informacje o partycji są zapisywane zgodnie z oczekiwaniami po kliknięciu przycisku Odśwież, OK
- Podczas indeksowania atrybutu ciągu z indeksem jest zbyt długi, został zwrócony nieoczekiwany błąd, zostanie zwrócony bardziej opisowy komunikat o błędzie.
- Tworzenie agenta zarządzania plikiem tekstowym, gdy usługa synchronizacji programu MIM jest zainstalowana w systemie Windows Server 2016, niektóre opcje kodowania tekstu, w tym Unicode, były niedostępne
- Usługa MIM MA wtedy, gdy komunikat o błędzie eksportu zawiera nieprawidłowy znak, spowoduje to uszkodzenie wpisów historii uruchamiania. Ta kompilacja została usunięta z komunikatu o błędzie przed zapisaniem jej w obiekcie przestrzeni łącznika i historii uruchamiania

#### <a name="mim-service"></a>Usługa MIM
- * Obsługa kont usług zarządzanych przez grupę
- * Ulepszona obsługa języka dla nowego zdefiniowanego standardu
- * FIMAutomation Export-FIMConfig polecenie cmdlet programu PowerShell jest dostępny argument "-PamConfig", aby wymusić wyeksportowanie obiektów konfiguracji PAM
- * FIMAutomation Export-FIMConfig polecenia cmdlet programu PowerShell — dodano parametr "-Request"
- * Atrybuty logiczne są zawsze ustawione na wartość NULL po utworzeniu powiązania, Poprzednia wartość logiczna nie zostanie zaktualizowana
> [!IMPORTANT]
>Może to być istotna zmiana w przypadku wstępnego przetworzenia migracji konfiguracji. Konfiguracja powinna zostać oceniona i zaktualizowana pod kątem nowej funkcji, ponieważ migracja konfiguracji jest traktowana jako nowa 
    - Zaimplementowane Inicjowanie nowych atrybutów wartości logicznych programu MIM do wartości false podczas tworzenia nowego obiektu
    - Zaimplementowano Inicjowanie nowych atrybutów logicznych programu MIM na wartość false przy dodawaniu nowego powiązania atrybutu logicznego do zasobu
- Ustawienie Program poprawy jakości obsługi klienta jest obsługiwane w przypadku wartości false 
- Instalacja usługi MIM nie powiodła się z powodu błędu uaktualniania bazy danych: nie można wstawić wartości NULL do kolumny "name", jeśli nie jest używana domyślna nazwa bazy danych
- W przypadku poprawek Microsoft 365 ustawienie zostanie wyczyszczone, szyfrowane hasło dla skrzynki pocztowej usługi Exchange Online w usłudze MIM nie zostanie zmienione
- * Nie ma limitu dla utworzonego pliku dziennika usługi MIM, Zaktualizowano domyślne ustawienie rejestrowania i zaimplementowano funkcję rejestrowania cyklicznego

#### <a name="privileged-access-management"></a>Privileged Access Management (Ochrona systemu Windows i usługi Active Directory platformy Microsoft Azure przy użyciu programu Privileged Access Management) 
- * Obsługa kont usług zarządzanych przez grupę
- * Ulepszona obsługa języka dla nowego zdefiniowanego standardu
- Obiekty korzystające z zasobów niezarządzanych nie są wyczyszczone w czasie.  te obiekty zostaną prawidłowo oczyszczone
- * Polecenie cmdlet New-PAMRole programu PowerShell "-disableAutoApproveIfOwner" Odmów samozatwierdzania dla roli
- * Get-PamRequest polecenie cmdlet programu PowerShell "-CreatedFrom" zezwala na filtrowanie od określonego żądania PAM
- * Dodatki modułu PAM
    - Get-PAMSet
    - Add-PAMSetMember
    - Remove-PAMSetMember
- Ostrzeżenie (wyjątek: System. ObjectDisposedException: nie można uzyskać dostępu do usuniętego obiektu) nie będzie dłużej wyświetlane w dzienniku zdarzeń PAM
- Set-PAMUser polecenie cmdlet może zmienić PrivAccountName bez usuwania
- New-PamRole teraz sprawdza, czy data "dostępne do" jest większa od daty "dostępne od"
- Wartości "dostępne z" i "dostępne dla" są zwracane przez polecenie cmdlet programu Get-PAMRole PowerShell
- Filtr polecenia cmdlet Get-PamRequest jest teraz prawidłowy
- * Polecenie cmdlet Set-PamGroup może teraz aktualizować obiekt grupy głównej w tle Active Directory
- Remove-PamUser polecenie cmdlet programu PowerShell kończy się niejasnym komunikatem o błędzie, jeśli użytkownik jest połączony z rolą jako kandydat. Teraz Walidacja po stronie klienta została dodana do polecenia cmdlet, a zwrócony wyjątek został wyjaśniony
- Konta usługi PAM trybu zmiany nie są ujawniane w konfiguracji
    - Konto interfejsu API REST usługi PAM
    - Konto usługi składnika PAM
    - Konto usługi monitorowania PAM

#### <a name="microsoft-identity-portal"></a>Portal tożsamości firmy Microsoft
- * Obsługa kont usług zarządzanych przez grupę
- * Ulepszona obsługa języka dla nowego zdefiniowanego standardu
- Kontrolka selektora tożsamości, kontrolka wydaje się dynamicznie zwiększać jej szerokość zamiast zawijania tekstu
- Okna podręczne nie są wyświetlane prawidłowo podczas wyświetlania w programie Internet Explorer 10
- Symbole cyrylicy na pasku tytułu są wyświetlane prawidłowo
- Okna podręczne nie mają już wyświetlania dodatkowego paska przewijania, gdy jest wyświetlany w programie Internet Explorer
- Niepowodzenie "Importowanie definicji przepływu pracy" prawidłowo zgłasza wyjątek i przywraca, umożliwiając dodanie działania reguły synchronizacji do definicji przepływu pracy
- <httpRuntime enableVersionHeader="false" /> dodano do domyślnego web.config
- Znaki specjalne w polu odróżnionyname nie uniemożliwiają już resetowania hasła Self-Service użytkownika w Active Directory
- Ulepszenia w zdaniach są prawidłowo zlokalizowane na ekranie
- Dodatek MIM dla programu Outlook zawiera kopię brakujących plików binarnych programu Outlook Interop

#### <a name="certificate-management"></a>Zarządzanie certyfikatami
- Odnawianie wirtualnej karty inteligentnej za pomocą aplikacji zarządzanie certyfikatami w usłudze MIM Modern użytkownik otrzymuje wyjątek zabroniony
- * Ulepszona obsługa języka dla nowego zdefiniowanego standardu
- Narzędzie do PRZYPINAnia "CLM napotkało błąd podczas próby zmiany numeru PIN karty inteligentnej.  Niewłaściwa liczba argumentów lub nieprawidłowe przypisanie właściwości. "
- Aktualizacja do modułów urzędu certyfikacji programu MIM z 4.4.1302.0 na kompilację późniejszą niż 4.4.1459, instalacja kończy się niepowodzeniem
- Nowoczesnej aplikacji do operacji odnawiania, rejestrowania i zamieniania historia żądań nie zawiera wszystkich elementów stanu żądania, które są rejestrowane
- Aktualizacja online nie została ukończona i zwraca wyjątek "rekord został zaktualizowany lub usunięty przez innego użytkownika".  
- Link "Pobierz certyfikat" w portal zarządzania certyfikatu Pobieranie certyfikatu (plik. cer) jest zbyt duże
- Klient zbiorczy zarządzania certyfikatami programu MIM będzie współpracujący z protokołami TLS 1,1 i TLS 1,2.  

 
## <a name="version-4417490"></a>4.4.1749.0 wersja

- Stan: 30 listopada 2017
- [Pobieranie](https://www.microsoft.com/download/details.aspx?id=56271)

### <a name="fixed-issues"></a>Naprawione problemy
W programie MIM w wersji 4.4.1749.0 zostały rozwiązane następujące problemy.

#### <a name="synchronization-service"></a>Usługa synchronizacji

- Procedura resetowania hasła nie powiedzie się, gdy domena serwera synchronizacji nie ma relacji zaufania z domeną docelową.

#### <a name="mim-service"></a>Usługa MIM

- Niepowodzenie uaktualniania bazy danych. Wyjątek naruszenia ograniczenia klucza obcego jest rejestrowany w dzienniku uaktualniania bazy danych.
- W przypadku wykonywania żądań samoobsługowego resetowania hasła usługa MIM zostaje losowo zatrzymana.
- Ustawianie obliczeń nie powiedzie się w celu odzwierciedlenia poprawnego członkostwa. Nie można usunąć powiązania dla atrybutu, jeśli jest on przywoływany w zestawie dynamicznym lub filtrze grupy.
- Usługa MIM nie działała w scenariuszu zatwierdzania żądań w usłudze Exchange Online, w której odpowiedzi na zatwierdzenia są udzielane za pomocą dodatku MIM dla programu Outlook.
- Atrybut msidmPhoneGatePhoneNumber bez kodu kraju nie używa wartości DefaultCountryCode w MFASettings.xml.
- Zestawy członkostw można aktualizować dynamicznie bez konieczności polegania na FIM_TemporalEventsJob.
- Reguły synchronizacji nie obsługują tworzenia reguł przepływu atrybutów dla atrybutów, których nazwy zawierają skrót lub symbol funta (#).
  
#### <a name="privilege-access-management"></a>Zarządzanie dostępem do uprawnień 

- New-PAMDomainConfiguration polecenie cmdlet programu PowerShell ustawia nieprawidłową wartość dla konfiguracji zaufania domeny, co spowodowało błąd (nie można przetworzyć tego nieznanego parametru żądania).

#### <a name="microsoft-identity-portal"></a>Portal tożsamości firmy Microsoft

- Wyjątek jest wyświetlany na ekranie głównym portal zarządzania tożsamości i pojawia się również przycisk Zamknij.
- Przyciski są nieprawidłowo wyświetlane w oknie usuwania elementu.Ten problem wystąpił w programie Internet Explorer, Firefox i Chrome. 
- Przycisk wyszukiwania nakłada się na przycisk wyboru zasobów w oknie działania zatwierdzania w przepływie pracy autoryzacji. Ten problem wystąpił w programie Internet Explorer, Firefox i Chrome. 
- W menu podręcznym właściwości grupy obszar przycisku nakłada się na kontrolki nawigacji ListView w kontrolce Usuń członków.Ten problem wystąpił w programie Internet Explorer, Firefox i Chrome.
- Wiele elementów interfejsu użytkownika nie jest wyświetlanych prawidłowo. Następujące elementy zostały naprawione:

    - Strzałki w górę i w dół w niektórych arkuszach właściwości.
    - Puste miejsce w dolnej części niektórych stron i okien dialogowych.
    - Brak nakładek okna podręcznego.

- W przypadku korzystania z konstruktora filtrów w różnych obszarach produktu (takich jak wyszukiwanie zaawansowane), Konstruktor filtrów uzyska "zablokowany", jeśli przycisk OK w oknie dialogowym wybierz wartość zostanie kliknięty bez obiektu wybranego w obszarze Dodawanie instrukcji.
- Nowe okno podręczne przepływu atrybutów w regule synchronizacji nie działa zgodnie z oczekiwaniami w przeglądarce Chrome.
- Na ekranie zarządzania obiektami (na przykład grupy dystrybucji), jeśli wybrano wiele obiektów przy użyciu pola wyboru, a obiekty mają długie nazwy wyświetlane. Teraz okno dialogowe zmienia rozmiar w pionie, tak aby formant nie rozciągał się poza końcem ekranu przeglądarki.
- W celu zarządzania obiektami lub ekranu listy (takich jak grupy dystrybucyjne) kontrolka wybrane elementy może przesunąć ekran w górę, aby znajdował się bezpośrednio w ostatnim obiekcie wymienionym na liście tabeli.
- Konstruktor filtrów (na przykład wyszukiwanie zaawansowane) w przeglądarce Safari nie działa.
- okna dialogowe portalu, w których są wyświetlane wartości atrybutów, krótsze słowa są dystrybuowane w całej komórce z dużą ilością białych znaków, a nie są wyrównane do lewej. 
- W niektórych wersjach przeglądarki wybrane elementy nie są aktualizowane, gdy zaznaczenie elementu zostanie zmienione.
- Karty okna dialogowego i kopiowania do schowka wyróżniają się podczas przejścia do programu przy użyciu klawisza Tab.  
- W programie Internet Explorer 10, gdy oglądasz Wyświetlanie siatki obiektów (takich jak grupy dystrybucyjne), zamiast wyświetlania w środku okna dialogowego znajduje się część "Znajdź grupy dystrybucji, których chcesz użyć w powyższym polu wyszukiwania".  
- Po zainstalowaniu aktualizacji w portalu programu MIM wyświetlanie portalu w programie Internet Explorer kończy się niepowodzeniem.
- Gdy używasz wyszukiwania zaawansowanego w przeglądarce Firefox, naciśnięcie klawisza ENTER w polu wartość atrybutu zwraca błąd.  

#### <a name="certificate-management"></a>Zarządzanie certyfikatami

- Nadawca żądania (Menedżer certyfikatów) nie może porzucić żądania, które zostało zduplikowane lub zapomniane przez użytkownika, który ma uprawnienia do wykonywania.
- Podczas próby odnowienia wirtualnej karty inteligentnej modułu TPM z nowoczesnej aplikacji zwracany jest wyjątek zabroniony.
- Podczas niektórych działań kart inteligentnych istniejące połączenia z bazą danych CertificateManagement są nieoczekiwanie otwarte.  
- Jeśli instalacja aktualizacji do zarządzania certyfikatami programu MIM (CM) zostanie podjęta przed uruchomieniem Kreatora konfiguracji zarządzanie certyfikatami w usłudze MIM, aktualizacja kończy się niepowodzeniem z wyjątkiem, który wydaje się niezwiązany z problemem.
- W Kreatorze konfiguracji zarządzanie certyfikatami w usłudze MIM są wyświetlane nieprawidłowe informacje o wersji produktu, a logo nie jest wyświetlane poprawnie.  
- Eksportowane dane raportu zarządzania certyfikatami programu MIM różnią się od danych raportu.Dane kolumny nie zawsze pasują do nagłówków kolumn.

## <a name="version-4416420"></a>4.4.1642.0 wersja

- Stan: 29 sierpnia 2017
- [Pobieranie](https://www.microsoft.com/download/details.aspx?id=55794)

### <a name="fixed-issues"></a>Naprawione problemy
W programie MIM w wersji 4.4.1642.0 zostały rozwiązane następujące problemy.

#### <a name="synchronization-service"></a>Usługa synchronizacji

- Procedura resetowania hasła nie powiedzie się, gdy domena serwera synchronizacji nie ma relacji zaufania z domeną docelową.
- Zadeklarowany Filtr importu (Sprawdzanie nazwy wyróżniającej) nie działa prawidłowo, gdy obiekt jest przenoszony z jednostki organizacyjnej, w którym ma zostać przefiltrowany do innej lokalizacji, gdzie nie powinien być spowodowany użyciem starej nazwy obiektu fantomu.
- Projektant agenta zarządzania zawiesza się na stronie "Konfigurowanie partycji i hierarchii".
- Pierwszeństwo nie przenosi do następnego obiektu, gdy poprzedni z pierwszeństwem jest odłączony.
- Usunięcie jednego z agentów zarządzania wyszukiwanie kontenerów podrzędnych na serwerze LDAP przy użyciu stronicowania powoduje błąd, jeśli serwer nie obsługuje stronicowania.
- Typ obiektu metaverse został zmieniony dynamicznie powodując awarię.

#### <a name="mim-service"></a>Usługa MIM

- Funkcja słowa nie zwraca pustego ciągu, jeśli ciąg zawiera mniej niż liczbę wyrazów.
- Interfejs użytkownika Wyświetl elementy członkowskie pokazuje nieprawidłowe członkowstwo, gdy kryteria zawierają dereferencję i jeśli dereferencja występuje poniżej innego elementu kryteriów. 
- Przepływ pracy autoryzacji odmówił żądania z komunikatem o błędzie "nie znaleziono przepływu pracy w magazynie Stanów trwałości".
- Przepływ pracy uruchamia działanie wyliczania zasobów w celu zbadania zapytania w programie MIM i sporadycznie kończy się niepowodzeniem.

#### <a name="privilege-access-management"></a>Zarządzanie dostępem do uprawnień

- Get-PAMRequestToApprove polecenia cmdlet nie zwraca kandydatów, jeśli osoba zatwierdzająca nie znajduje się w roli do zatwierdzenia.
- Pokrewny węzeł pasek nawigacyjny reguł MPR i PAM jest włączony nawet wtedy, gdy nie zainstalowano funkcji PAM.
- Usługa MIM zgłasza wyjątek i dzienniki ostrzeżenie z synchronizatora konfiguracji domeny dla usługi PAM.

#### <a name="microsoft-identity-portal"></a>Portal tożsamości firmy Microsoft

- W przypadku korzystania z programu Firefox w celu uzyskania dostępu do konstruktora filtrów w portalu programu MIM nie można użyć wewnętrznych formantów (pól listy rozwijanej, pól tekstowych itd.). Nie można ich wybrać do momentu kliknięcia prawym przyciskiem myszy (czyli kliknięcia menu kontekstowego).
- Wyszukiwanie w portalu jest renderowane nieprawidłowo na niektórych rozdzielczościach ekranu. 
- Kontrolka Calendar w wyszukiwaniu zaawansowanym została obcięta.
- Interfejs użytkownika konstruktora filtrów został przerwany — w trybie edycji (elementy w nieumieszczonym miejscu).
- Wyskakujące okienko miało stały rozmiar, a kontrolki edycji nie mają odpowiedniego rozmiaru.
- Ciąg kawałków dla Norwegii i innych języków w menu głównym.
- Skopiowany adres URL z menu podręcznego nie działa.

#### <a name="certificate-management"></a>Zarządzanie certyfikatami

- Portale haseł samoobsługowych programu MIM.

#### <a name="reporting"></a>Raportowanie

- Polecenie cmdlet Import-FIMReportingSchemaDefinition dla raportowania kończy się niepowodzeniem z powodu błędu.

#### <a name="client-add-in"></a>Dodatek klienta

- Język interfejsu użytkownika nie sprawdza równości kodu kraju.

### <a name="new-features-and-improvements"></a>Nowe funkcje i ulepszenia
W programie MIM w wersji 4.4.1642.0 dodano następujące funkcje i ulepszenia.

#### <a name="mim-service"></a>Usługa MIM

- Dodano ponowną próbę w najdłuższych operacjach przetwarzania żądania (etap walidacji). Nie gwarantuje to, że przetwarzanie żądania zostało ukończone, ale powoduje, że żądania są bardziej stabilne. Poprawka jest domyślnie wyłączona. Aby włączyć poprawkę dodawaną alwaysOnRetryRequestProcessingTransaction = "true" w sekcji resourceManagementService pliku konfiguracji usługi FIMService

#### <a name="certificate-management"></a>Zarządzanie certyfikatami

- Nowsze wersje programu CM Server mogą współpracowały ze starszymi BulkClient (ale nie starszymi niż 4.4. xxxx. 0). 

#### <a name="mim-self-service-password-portals"></a>Portale haseł samoobsługowych programu MIM 

- Pokaż Ostrzeżenie dla elementu QAGate, jeśli jest używana, zawiera odpowiedź zawierającą znak dwubajtowy
- Dodaj konfigurowalną właściwość, aby umożliwić włączanie/wyłączanie użycia edytora IME w formularzu rejestracji SSPR 
- SSPR maskuje pytania dotyczące rejestracji haseł na kliencie portalu

## <a name="version-4415490"></a>4.4.1549.0 wersja
 
* Stan: wcześniejsza aktualizacja poprawki 2016 programu MIM z dodatkiem SP1, 27 marca 2017
* Odpowiedni numer wersji pakietu BHOLD: 6.0.36.0
* Więcej informacji można znaleźć pod adresem [https://support.microsoft.com/en-us/help/4012498](https://support.microsoft.com/en-us/help/4012498)


## <a name="version-4413020"></a>4.4.1302.0 wersja

* Stan: program MIM 2016 z dodatkiem Service Pack 1 (SP1) z listopada 9, 2016
* Odpowiedni numer wersji pakietu BHOLD: 5.0.3355.0

### <a name="updates-in-this-service-pack"></a>Aktualizacje w tym dodatku Service Pack


#### <a name="mim"></a>MIM

- **Możliwość korzystania z portalu MIM w różnych przeglądarkach na potrzeby samoobsługi użytkowników końcowych:** w tym dodatku Service Pack wprowadzamy obsługę większości popularnych przeglądarek. Użytkownicy mogą teraz uzyskać dostęp do portalu MIM i korzystać z niego w celu samoobsługowego zarządzania grupą i profilem, korzystając z przeglądarek Microsoft Edge, Chrome i Safari.

- **Obsługa usługi MIM dla usługi Exchange Online:** usługa MIM przez długi czas obsługiwała wysyłanie i odbieranie wiadomości e-mail z zatwierdzeniami i powiadomieniami. Przed wersją z dodatkiem SP1 program MIM obsługiwał tylko serwer Exchange lub SMTP. Z dodatkiem Service Pack 1 usługa MIM może wysyłać i odbierać żądania oraz powiadomienia e-mail przy użyciu konta usługi Exchange online Microsoft 365.

- **Sprawdzanie poprawności formatu pliku obrazu podczas przesyłania:** usługa MIM może teraz sprawdzić poprawność formatu plików obrazów podczas ich przesyłania do portalu.

#### <a name="privileged-access-managementpam"></a>Usługa Privileged Access Management (PAM)

- **Obsługa lasu „PRIV” (bastionu) usługi PAM dla poziomu funkcjonalności systemu Windows Server 2016:** usługa PAM programu MIM może zostać skonfigurowana w środowisku z kontrolerami domeny uruchomionymi na poziomie funkcjonalności lasu Usług domenowych Active Directory systemu Windows Server 2016. Jeśli bilet protokołu Kerberos użytkownika zostanie skonfigurowany, będzie on ograniczony w czasie do pozostałego czasu aktywacji roli.

    >[!Note]
    Jeśli zdecydujesz się zachować dla lasu poziom funkcjonalności systemu Windows Server 2012 R2 w domenie CORP, zalecamy zainstalowanie aktualizacji [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) i [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) na kontrolerze domeny CORP.

- **Podniesienie uprawnień konta z dostępem uprzywilejowanym do grup ograniczonych do lasu „PRIV” (bastionu):** teraz administratorzy mogą informować usługę MIM o grupach i użytkownikach ograniczonych wyłącznie do lasu „PRIV”. W ten sposób te grupy i ci użytkownicy mogą zostać uwzględnieni w rolach usługi PAM.  Następnie można ich aktywować dla roli i przypisać im członkostwo w grupach w lesie „PRIV”.

- **Skrypty wdrażania usługi PAM:** skrypty wdrażania usługi PAM umożliwiają administratorom uproszczenie instalacji środowiska PAM.

- **Polecenia cmdlet usługi PAM do konfiguracji silosów zasad uwierzytelniania:** z dodatkiem Service Pack 1 wprowadzono nowe polecenia cmdlet pozwalające wzmocnić zabezpieczenia lasu bastionu. Te polecenia cmdlet automatycznie tworzą silos zasad uwierzytelniania powiązany z szablonem zasady uwierzytelniania.

    >[!Note]
    Wspomniane polecenia cmdlet są uruchamiane automatycznie w ramach skryptów wdrażania.



### <a name="platform-support"></a>Obsługa platform 

Zaktualizowane informacje o obsłudze platform można znaleźć w artykule [Platformy obsługiwane przez program MIM 2016](../microsoft-identity-manager-2016-supported-platforms.md).  Nowe platformy obsługiwane w tym dodatku Service Pack obejmują SQL Server 2016, SharePoint 2016.


### <a name="issues-fixed-in-this-release"></a>Problemy rozwiązane w tej wersji


#### <a name="pam"></a>PAM
- Polecenie New-PAMGroup nie utworzyło obiektów MIM dla lokalnych grup domeny w lesie PRIV.
- Polecenie New-PAMDomainConfiguration kończyło się niepowodzeniem i był wyświetlany komunikat błędu „netdom”.
- Usługa monitorowania PAM rejestrowała ostrzeżenia dla grup w lesie PRIV.


### <a name="how-to-upgrade"></a>Jak uaktualnić

Klienci chcący przeprowadzić uaktualnienie do wersji Microsoft Identity Manager 2016 z dodatkiem Service Pack 1 powinni zastosować się do poniższych wytycznych w zakresie wszystkich usług mających zastosowanie do ich wdrożenia.

>[!Note]
>Klienci korzystający z programu Forefront Identity Manager 2010 R2 z dodatkiem SP1 lub wcześniejszych jego wersji muszą najpierw uaktualnić swoje środowisko do wersji Microsoft Identity Manager 2016 udostępnionej w sierpniu 2015 roku, a następnie wykonać poniższe kroki.

Przed rozpoczęciem

Przed uaktualnieniem usługi i portalu MIM należy uaktualnić aparat synchronizacji MIM.
Należy utworzyć kopię zapasową baz danych MIMService i MIM Sync.

1. Odinstaluj składnik Microsoft Identity Manager, który chcesz uaktualnić.
2. Po zakończeniu dezinstalacji otwórz stronę powitalną, korzystając z nośnika instalacyjnego „FIMSplash.htm”.
3. Wybierz składnik MIM do uaktualnienia.
4. Kontynuuj instalację, wykonując wyświetlane polecenia.
   * Instalacja usługi i portalu MIM: Jeśli chcesz wybrać Exchange Online jako konto poczty, wpisz adres e-mail i poświadczenia konta usługi Exchange Online na następnym ekranie.

## <a name="version-4322660"></a>4.3.2266.0 wersja


* Status: Poprawka 2016 programu MIM z 15 lipca 2016; Klienci muszą przeprowadzić uaktualnienie do nowszej wersji, aby nadal były obsługiwane
* Więcej informacji można znaleźć pod adresem [https://support.microsoft.com/en-us/kb/3171342](https://support.microsoft.com/en-us/kb/3171342)

## <a name="version-4321950"></a>4.3.2195.0 wersja

* Stan: hotfix 2016 poprawka z 20 marca 2016, klienci muszą przeprowadzić uaktualnienie do nowszej wersji, aby nadal były obsługiwane
* Odpowiedni numer wersji pakietu BHOLD: 5.0.3355.0
* Więcej informacji można znaleźć pod adresem [https://support.microsoft.com/kb/3134725](https://support.microsoft.com/kb/3134725)
    
## <a name="version-4320640"></a>4.3.2064.0 wersja

* Stan MIM 2016 poprawka z 11 grudnia 2015; Klienci muszą przeprowadzić uaktualnienie do nowszej wersji, aby nadal były obsługiwane
* Odpowiedni numer wersji pakietu BHOLD: 5.0.3176.0
* Więcej informacji można znaleźć pod adresem [https://support.microsoft.com/kb/3092179](https://support.microsoft.com/kb/3092179)

## <a name="version-4319350"></a>4.3.1935.0 wersja

* Stan MIM 2016 GA z 6 sierpnia 2015; Klienci muszą przeprowadzić uaktualnienie do nowszej wersji, aby nadal były obsługiwane
* Odpowiedni numer wersji pakietu BHOLD: 5.0.3079.0

