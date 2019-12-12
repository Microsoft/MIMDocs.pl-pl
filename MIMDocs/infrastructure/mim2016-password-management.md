---
title: Zarządzanie hasłami za pomocą programu Microsoft Identity Manager 2016 | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/01/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 45b46ed10f7eda506fe1fc1af94c4be06a1a37b9
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516580"
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Zarządzanie hasłami za pomocą programu Microsoft Identity Manager 2016

Zarządzanie hasłami dla wielu kont użytkowników jest jedną ze złożonych kwestii związanych z zarządzaniem środowiskiem przedsiębiorstwa z wieloma źródłami danych. Program Microsoft Identity Manager 2016 zapewnia dwa rozwiązania w zakresie zarządzania hasłami:

-   Synchronizacja haseł — wykorzystuje usługę powiadamiania o zmianie hasła (PCNS, password change notification service) do przechwytywania zmian haseł z usługi Active Directory i propagowania ich do innych połączonych źródeł danych.

-   Zarządzanie zmianą haseł oparte na użytkownikach — wykorzystuje usługę Instrumentacja zarządzania Windows (WMI, Windows Management Instrumentation) za pośrednictwem pomocy technicznej w sieci Web i aplikacji do samoobsługowego resetowania haseł.

Za pomocą rozwiązań do synchronizacji haseł i zarządzania zmianą haseł opartą na użytkownikach możliwe jest:

-   Zmniejszenie liczby różnych haseł, które użytkownicy muszą pamiętać.

-   Jednoczesne ustawianie lub zmienianie haseł na to samo hasło w ramach wielu kont użytkowników.

-   Umożliwienie użytkownikom zmiany swoich haseł w usłudze Active Directory i wypchnięcie zmiany hasła do innych systemów.

-   Wyeliminowanie ryzyka związanego z tworzeniem dodatkowego magazynu haseł lub poświadczeń.

-   Przeprowadzanie synchronizacji haseł w wielu źródłach danych za pomocą usługi Active Directory jako źródła autorytatywnego.

-   Wykonywanie operacji zarządzania hasłami w czasie rzeczywistym, niezależnie od operacji wykonywanych przez program MIM.

## <a name="password-extensions"></a>Rozszerzenia haseł

Agenci zarządzania dla serwerów katalogowych domyślnie obsługują operacje zmiany i ustawiania hasła. W przypadku agentów opartych na plikach, agentów bazy danych i agentów zarządzania łącznika Extensible Connectivity nieobsługujących domyślnie operacji zmiany i ustawiania hasła można utworzyć bibliotekę dołączaną dynamicznie (DLL) na potrzeby rozszerzenia haseł platformy .NET.
Biblioteka DLL rozszerzenia haseł platformy .NET jest wywoływana przy każdym wywołaniu zmiany lub ustawienia hasła dla dowolnego z tych agentów zarządzania. Ustawienia rozszerzenia haseł zostały skonfigurowane dla tych agentów zarządzania za pomocą narzędzia Synchronization Service Manager. Więcej informacji o konfigurowaniu rozszerzeń haseł zawiera dokumentacja dla deweloperów programu FIM.

| Zarządzanie hasłami jest obsługiwane domyślnie w ramach agentów zarządzania na potrzeby: | Dzięki użyciu rozszerzenia haseł zarządzanie hasłami jest również obsługiwane w ramach agentów zarządzania dla następujących zasobów: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Usługi domenowe                                                          | Pliki tekstowe z parami atrybut-wartość                                                                    |
| Usługi LDS Active Directory (ADLDS)                   | Rozdzielane pliki tekstowe                                                                               |
| IBM Directory Server                                                      | Directory Services Markup Language (DSML)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | Pliki tekstowe stałej szerokości                                                                             |
| Serwery katalogowe firmy Sun i Netscape                                        | IBM DB2 Universal Database                                                                         |
|                                                                           | Format wymiany danych LDAP (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Oracle Database                                                                                    |

## <a name="password-synchronization"></a>Synchronizacja haseł


Synchronizacja haseł współpracuje z usługą powiadamiania o zmianie hasła (PCNS) w domenie usługi Active Directory i umożliwia automatyczne propagowanie zmian haseł pochodzących z usługi Active Directory do innych połączonych źródeł danych. Program MIM umożliwia wykonanie tej operacji przez uruchomienie serwera zdalnego wywołania procedur (RPC, Remote Procedure Call), który nasłuchuje powiadomień o zmianie hasła z kontrolera domeny usługi Active Directory. Po odebraniu i uwierzytelnieniu żądanie zmiany hasła jest przetwarzane przez program MIM i propagowane do odpowiednich agentów zarządzania.

> [!IMPORTANT]
> Dwukierunkowa synchronizacja haseł nie jest obsługiwana przez program MIM. Skonfigurowanie dwukierunkowej synchronizacji haseł może spowodować powstanie pętli, która będzie zużywać zasoby serwera i mieć negatywny wpływ na usługę Active Directory oraz program MIM.

Usługa PCNS jest uruchamiana na każdym kontrolerze domeny usługi Active Directory. Systemy otrzymujące powiadomienia dotyczące haseł są określane jako elementy docelowe. Przed wysłaniem powiadomień dotyczących haseł serwer MIM musi zostać skonfigurowany jako element docelowy usługi PCNS w usłudze Active Directory. W konfiguracji usługi PCNS musi zostać zdefiniowania grupa dołączania i opcjonalnie grupa wykluczania. Te grupy są używane do ograniczenia przepływu poufnych haseł z domeny. Na przykład w celu wysłania haseł dla wszystkich użytkowników, bez uwzględniania haseł administracyjnych, można wybrać grupę Użytkownicy domeny jako grupę dołączania, a grupę Administratorzy domeny jako grupę wykluczania. Aby uzyskać więcej informacji o konfigurowaniu usługi powiadamiania o zmianie hasła, zobacz [Using Password Synchronization](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx) (Korzystanie z synchronizacji haseł)

Składniki biorące udział w procesie synchronizacji haseł to:

-   **Usługa powiadamiania o zmianie hasła (Pcnssvc.exe)** — usługa powiadamiania o zmianie hasła jest uruchamiana na kontrolerze domeny i jest odpowiedzialna za odbieranie powiadomień o zmianie haseł z lokalnego filtru haseł, kolejkowanie tych powiadomień na potrzeby docelowego serwera z uruchomionym programem MIM oraz używanie usługi RPC do dostarczania powiadomień. Usługa szyfruje hasło i zapewnia, że hasło pozostanie bezpieczne, dopóki nie zostanie pomyślnie dostarczone na serwer docelowy, na którym działa program MIM.

-   **Nazwa jednostki usługi (SPN, Service Principal Name)** — nazwa SPN to właściwość w ramach obiektu konta w usłudze Active Directory, która jest używana przez protokół Kerberos do wzajemnego uwierzytelnienia usługi PCNS i elementu docelowego. Nazwa SPN służy do zapewnienia, że usługa PCNS jest uwierzytelniana na odpowiednim serwerze, na którym działa program MIM, a żadna inna usługa nie będzie otrzymywać powiadomień o zmianie haseł. Nazwa SPN jest tworzona i przypisywana za pomocą narzędzia setspn.exe. Aby uzyskać więcej informacji o konfigurowaniu nazwy SPN, zobacz Using Password Synchronization (Korzystanie z synchronizacji haseł).

-   **Filtr powiadamiania o zmianie hasła (Pcnsflt.dll)** — filtr haseł służy do uzyskiwania z usługi Active Directory haseł w postaci zwykłego tekstu. Ten filtr jest ładowany przez urząd zabezpieczeń lokalnych (LSA, Local Security Authority) na każdym kontrolerze domeny systemu Windows Server biorącym udział w dystrybucji haseł do serwera docelowego, na którym działa program MIM. Po zainstalowaniu filtru i ponownym uruchomieniu kontrolera domeny filtr rozpoczyna odbieranie powiadomień o zmianie haseł, które pochodzą z tego kontrolera domeny. Filtr powiadomień dotyczących haseł jest uruchamiany jednocześnie z pozostałymi filtrami, które działają w ramach kontrolera domeny.

-   **Narzędzie do konfiguracji usługi powiadamiania o zmianie hasła (Pcnscfg.exe)** — narzędzie pcnscfg.exe jest używane do obsługi parametrów konfiguracji usługi powiadamiania o zmianie hasła przechowywanych w usłudze Active Directory i zarządzania nimi. Te parametry konfiguracji, na przykład służące do definiowania serwerów docelowych, interwału ponawiania kolejki haseł oraz włączania lub wyłączania serwera docelowego, są używane podczas uwierzytelniania i wysyłania powiadomień dotyczących haseł na serwer docelowy, na którym działa program MIM.
    Konfiguracja usługi jest przechowywana w usłudze Active Directory, więc wymagane jest tylko zaktualizowanie konfiguracji na jednym kontrolerze domeny. Usługa Active Directory replikuje zmianę na wszystkich innych kontrolerach domeny.

-   **Serwer zdalnego wywołania procedur na serwerze (RPC), na którym działa program MIM** — po włączeniu synchronizacji haseł uruchamiany jest serwer RPC na serwerze, na którym działa program MIM, co umożliwia otrzymywanie powiadomień z usługi powiadamiania o zmianie hasła. Serwer RPC dynamicznie wybiera zakres portów do użycia. Jeśli program MIM jest wymagany do komunikowania się z lasem usługi Active Directory przez zaporę, konieczne jest otworzenie zakresu portów.

-   **Biblioteka DLL rozszerzenia haseł** — biblioteka DLL rozszerzenia haseł zapewnia sposób implementacji operacji ustawiania lub zmiany hasła za pomocą rozszerzeń reguł dla dowolnej bazy danych, łącznika Extensible Connectivity lub agenta zarządzania opartego na plikach.
    Jest to realizowane przez utworzenie zaszyfrowanego atrybutu tylko do eksportu o nazwie „export_password”, który w rzeczywistości nie istnieje w połączonym katalogu, ale można do niego uzyskać dostęp i ustawić go w rozszerzeniach zasad aprowizacji lub użyć podczas eksportowania przepływu atrybutu. Więcej informacji o konfigurowaniu rozszerzeń haseł zawiera [dokumentacja dla deweloperów programu FIM](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx).

## <a name="preparing-for-password-synchronization"></a>Przygotowywanie synchronizacji haseł

Przed rozpoczęciem konfigurowania synchronizacji haseł dla programu MIM i środowiska usługi Active Directory sprawdź, czy:

-   Program MIM został zainstalowany zgodnie z instrukcjami instalacji.

-   Zostali utworzeni agenci zarządzania połączonych źródeł danych, którzy będą zarządzani na potrzeby synchronizacji haseł, a obiekty zostały pomyślnie dołączone i zsynchronizowane.

Aby skonfigurować synchronizację haseł:

-   Rozszerz schemat usługi Active Directory w celu dodania klas i atrybutów niezbędnych do zainstalowania i uruchomienia usługi powiadamiania o zmianie hasła (PCNS).

-   Zainstaluj usługę PCNS na każdym kontrolerze domeny.

-   Skonfiguruj nazwę jednostki usługi (SPN) w usłudze Active Directory dla konta usługi MIM.

-   Skonfiguruj usługę PCNS pod kątem komunikacji z docelową usługą MIM.

-   Skonfiguruj agentów zarządzania dla połączonych źródeł danych, które mają być zarządzane na potrzeby synchronizacji haseł.

-   Włącz synchronizację haseł w programie MIM.

Aby uzyskać więcej informacji na temat konfigurowania synchronizacji haseł, zobacz Using Password Synchronization (Korzystanie z synchronizacji haseł).

## <a name="password-synchronization-process"></a>Proces synchronizacji haseł

Proces synchronizacji żądania zmiany hasła pochodzącego z kontrolera domeny usługi Active Directory z innymi połączonymi źródłami danych przedstawiono na poniższym diagramie:

1.  Użytkownik inicjuje żądanie zmiany hasła przez naciśnięcie klawiszy Ctrl + Alt + Del. Żądanie zmiany hasła, łącznie z nowym hasłem, jest wysyłane do najbliższego kontrolera domeny.

2.  Kontroler domeny rejestruje żądanie zmiany hasła i wysyła powiadomienie do filtru powiadamiania o zmianie hasła (Pcnsflt.dll).

3.  Filtr powiadamiania o zmianie hasła przekazuje żądanie do usługi powiadamiania o zmianie hasła (PCNS).

4.  Usługa PCNS weryfikuje żądanie zmiany hasła, uwierzytelnia nazwę jednostki usługi (SPN) przy użyciu protokołu Kerberos, a następnie przekazuje żądanie zmiany hasła w zaszyfrowanym wywołaniu RPC do docelowego serwera MIM.

5.  Program MIM weryfikuje źródłowy kontroler domeny, używa nazwy domeny w celu zlokalizowania agenta zarządzania obsługującego tę domenę, a następnie używa informacji o koncie użytkownika w żądaniu zmiany hasła do zlokalizowania odpowiedniego obiektu w przestrzeni łącznika.

6.  Za pomocą informacji z tabeli złączeń program MIM określa agentów zarządzania, którzy odbierają zmianę hasła, a następnie wypycha do nich tę zmianę hasła.

## <a name="password-synchronization-security"></a>Zabezpieczenia synchronizacji haseł

Rozwiązano następujące problemy z zabezpieczeniami dotyczące synchronizacji haseł:

-   Uwierzytelnianie ze źródła hasła — po odebraniu powiadomienia o zmianie hasła ma miejsce uwierzytelnianie Kerberos w programie MIM i źródłowym kontrolerze domeny w celu sprawdzenia, czy adresat i nadawca są poprawni. Po odebraniu powiadomienia o zmianie hasła program MIM sprawdza, czy obiekt wywołujący ma konto w kontenerze kontrolerów domeny, do której należy.

-   Synchronizacja haseł zakończona niepowodzeniem w docelowym źródle danych z powodu niezabezpieczonego połączenia — jeśli agent zarządzania został skonfigurowany, aby wymagać bezpiecznego połączenia, ale takie połączenie nie zostało wykryte, synchronizacja zakończy się niepowodzeniem.
    Synchronizacja będzie nadal miała miejsce, jeśli agenta zarządzania skonfigurowano pod kątem zezwalania na niezabezpieczone połączenia. Zezwolenie na niezabezpieczone połączenia powinno zostać włączone tylko po przeanalizowaniu i zrozumieniu związanego z tym ryzyka.

-   Zabezpieczanie przechowywania haseł — w programie MIM zaszyfrowane hasła są przechowywane tylko tymczasowo. Wszystkie hasła odebrane przez program MIM podczas operacji związanej z powiadamianiem o zmianie hasła są szyfrowane od razu po wprowadzaniu ich do procesu MIM.
    Po ich pomyślnym wysłaniu do docelowego połączonego źródła danych są one odszyfrowywane, a pamięć, w której przechowywane było hasło, jest natychmiast czyszczona. W przypadku, gdy operacja zapisu do docelowego połączonego źródła danych nie powiedzie się, zaszyfrowane hasło będzie przechowywane aż do momentu przeprowadzenia wszystkich ponownych prób, po czym zostanie usunięte z pamięci.

-   Zabezpieczenia kolejek haseł — hasła przechowywane w kolejkach haseł usługi PCNS pozostają zaszyfrowane do momentu ich dostarczenia.

## <a name="password-synchronization-error-recovery-scenarios"></a>Scenariusze odzyskiwania po wystąpieniu błędu synchronizacji haseł

Idealna sytuacja ma miejsce, gdy użytkownik zmieni hasło, a zmiana zostanie zsynchronizowana bez błędów. W poniższych scenariuszach opisano sposób odzyskiwania sprawności programu MIM po wystąpieniu typowych błędów synchronizacji:

-   **Nie powiodło się przesłanie powiadomienia o haśle z usługi Active Directory do programu MIM** — może to mieć miejsce, gdy sieć nie działa lub serwer, na którym działa program MIM, jest niedostępny. Powiadomienie o zmianie hasła pozostaje w kolejce na kontrolerze domeny, umieszczone tam przez usługę PCNS. Usługa PCNS ponawia próbę wysłania powiadomienia zgodnie z konfiguracją interwału ponawiania.

-   **Nie powiodła się synchronizacja haseł z docelowym źródłem danych** — może to również mieć miejsce, gdy sieć nie działa lub docelowe źródło danych jest niedostępne.
    Powiadomienie o zmianie hasła jest umieszczane w kolejce i ponawiane zgodnie z konfiguracją agenta zarządzania dotyczącą liczby ponowień i interwału ponawiania. Wszystkie hasła pozostają zaszyfrowane, gdy są one przechowywane na potrzeby ponawiania, i usuwane, gdy operacja powiedzie się lub zostanie osiągnięty limit ponowień.

-   **Aktywowanie serwera rezerwy aktywnej z uruchomionym programem MIM po wystąpieniu awarii** — w przypadku awarii podstawowego serwera, na którym działa program MIM, można skonfigurować serwer rezerwy aktywnej na potrzeby synchronizacji haseł i aktywować go bez utraty zmian haseł. Aby uzyskać więcej informacji, zobacz [MIISactivate: Server Activation Tool](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx) (MIISactivate: narzędzie do aktywacji serwera)

Niektóre błędy są na tyle poważne, że żadna liczba ponownych prób nie spowoduje pomyślnego uruchomienia operacji. W takich przypadkach zostanie zarejestrowane zdarzenie błędu, a proces zostanie zatrzymany. Następujące zdarzenia nie są ponawiane:

| Zdarzenie | Ważność    | Description                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Informacje | Operacja ustawienia synchronizacji haseł nie została wykonana, ponieważ sygnatura czasowa jest nieaktualna.                                                                      |
| 6921  | Error       | Operacja ustawienia synchronizacji haseł nie została przetworzona, ponieważ zarządzanie hasłami nie jest włączone dla docelowego agenta zarządzania.                                |
| 6922  | Error       | Operacja ustawienia synchronizacji haseł nie została przetworzona, ponieważ zarządzanie hasłami nie jest skonfigurowane dla docelowego agenta zarządzania.                             |
| 6923  | Ostrzeżenie     | Operacja ustawienia synchronizacji haseł nie została przetworzona, ponieważ nie odnaleziono docelowego obiektu przestrzeni łącznika w połączonym katalogu.                  |
| 6927  | Error       | Operacja ustawienia synchronizacji haseł nie powiodła się, ponieważ hasło nie spełnia zasad dotyczących haseł systemu docelowego.                                      |
| 6928  | Error       | Operacja ustawienia synchronizacji haseł nie powiodła się, ponieważ rozszerzenie haseł dla docelowego agenta zarządzania nie jest skonfigurowane pod kątem obsługi operacji ustawiania haseł. |

## <a name="user-based-password-change-management"></a>Zarządzanie zmianą haseł oparte na użytkownikach

Program MIM udostępnia dwie aplikacje internetowe używające usługi Instrumentacja zarządzania Windows (WMI) do resetowania hasła. Podobnie jak w przypadku synchronizacji haseł zarządzanie hasłami jest aktywowane podczas konfigurowania agenta zarządzania w projektancie agenta zarządzania. Informacje dotyczące zarządzania hasłami i usługi WMI zawiera dokumentacja dla deweloperów programu MIM.

Podczas instalacji program MIM tworzy dwie grupy zabezpieczeń, które są przeznaczone do obsługi operacji zarządzania hasłami:

-   FIMSyncBrowse — członkowie tej grupy mają uprawnienia do zbierania informacji o kontach użytkowników podczas wykonywania operacji wyszukiwania za pomocą zapytań usługi WMI.

-   FIMSyncPasswordSet — członkowie tej grupy mają uprawnienia do wykonywania operacji wyszukiwania kont, ustawiania haseł i zmiany haseł za pomocą interfejsów zarządzania hasłami przy użyciu usługi WMI.
