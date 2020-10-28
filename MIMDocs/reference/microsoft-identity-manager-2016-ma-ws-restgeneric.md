---
title: Przewodnik przepływu pracy łącznika usługi sieci Web dla interfejsu API REST | Microsoft Docs
description: W tym artykule opisano sposób wdrażania przykładu interfejsu API REST.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e0c00972983d964a489d7c76e06e271bdf91b79e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760841"
---
# <a name="web-service-connector-workflow-guide-for-a-rest-api-sample"></a>Przewodnik przepływu pracy łącznika usługi sieci Web dla przykładu interfejsu API REST

W tym artykule opisano wdrażanie przykładowego interfejsu API REST w celu przechodzenia przez narzędzie konfiguracji usługi sieci Web ze źródłem danych sieci Web interfejsu API REST.

## <a name="prerequisites"></a>Wymagania wstępne

Do korzystania z przykładu wymagane są następujące wymagania wstępne:

- Narzędzie konfiguracji usługi sieci Web jest zainstalowane.
- Usługa przykładowego źródła danych REST została wdrożona. Pobierz i zainstaluj próbkę z (zobacz tutaj).

<!-- No link provided for "see here" -->
<!-- Should Note go with bullet point #2 -->

>[!NOTE]
>Dane JSON muszą zawierać pojedynczy obiekt z właściwością zawierającą tablicę.

<!-- Should JSON be exactly as-is or just a sample -->
<!-- Should JSON be part of Note content -->
```JSON
{

"EmployeeList":[

{"id":"1","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""},{"id":"2","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""}

]

}
```

## <a name="configure-rest-project-discovery-in-the-web-service-configuration-tool"></a>Konfigurowanie odnajdywania projektów REST w narzędziu konfiguracji usługi sieci Web
Poniższe kroki pokazują, jak utworzyć nowy projekt dla źródła danych w narzędziu konfiguracji usługi sieci Web.

1. Otwórz Narzędzie konfiguracji usługi sieci Web. Otwiera pusty projekt protokołu SOAP.

   ![Narzędzie konfiguracji usługi sieci Web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/web-service-configuration-tool.png)

2. Wybierz pozycję **plik**  >  **Nowy**  >  **projekt REST** .

   ![Utwórz nowy projekt REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/new-project.png)
   <!-- Image shows SOAP project selected, not REST project -->

3. Po lewej stronie wybierz pozycję **projekt REST** , a następnie wybierz pozycję **Dodaj** .

   ![Wybierz projekt REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-project.png)

4. Na następnej stronie podaj następujące informacje:

   - Nowa nazwa usługi sieci Web
   - Adres (ścieżka URL interfejsu API REST)
   - Przestrzeń nazw
   - Tryb zabezpieczeń (typ uwierzytelniania)

   ![Usługa REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service.png)
    
   Na poniższym ekranie przedstawiono przykłady dla tych wartości:
    
   ![Przykładowe wartości dla usługi REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/restsample.png)

   Ustaw **tryb zabezpieczeń** na _Brak_ . Ustaw **adres** na przykładowy serwer JSON, który jest hostowany na platformie Azure.

5. Wybierz przycisk **OK** . Projekt REST wymieniony w narzędziu konfiguracji usług sieci Web.

   ![Projekt REST w narzędziu konfiguracji usług sieci Web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-discovery.png)

6. Następnym krokiem jest zdefiniowanie wywołania interfejsu API REST i przetłumaczenie wywołania na wywołania Windows Communication Foundation (WCF).

   1. Rozwiń **projekt REST** i wybierz usługę _RESTSAMPLE_ .

   2. Wybierz pozycję **Dodaj** . Zostanie wyświetlony monit o dodanie dwóch wartości:
   
      ![Wprowadź wartości dla usługi REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service-highlights.png)
      
      1. Wprowadź **nazwę** . Ten krok jest oznaczony jako 3 na zrzutie ekranu.
      2. Wprowadź **adres** . Ten krok jest oznaczony jako 4 na zrzucie ekranu.
      3. Wybierz przycisk **OK** . Zasób REST jest dodawany do opisu usługi _RESTSAMPLE_ .

7. W polu **zasoby** wybierz właśnie dodany zasób Rest. Dodaj następującą metodę:

   ![Dodawanie metody REST do zasobu](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-method.png)
   <!-- How does this dialog appear, by selecting Edit? -->

8. Wybierz metodę REST. Należy zauważyć, że można utworzyć wiele metod w tym samym zasobie i zdefiniować zapytania przesłane podczas wykonywania.

9. Dla metody GETALL nie są wymagane żadne zapytania. Pozostaw puste wartości parametrów. Podczas eksportowania lub importowania interfejsu API REST należy zdefiniować przykładową odpowiedź/or żądania w zależności od funkcji. Skopiuj i wklej zwrot JSON podczas przechodzenia do tego przykładu.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-samples.png)

10. Wybierz pozycję **Zapisz** . Zapisz projekt w `C:\Program Files\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions` . 

>[!NOTE]
>Po zapisaniu projektu zostanie wygenerowany plik WsConfig. Plik konfiguracji zawiera wiele plików, które są zdefiniowane wcześniej w przeglądzie usługi sieci Web.


## <a name="configure-object-types-in-the-web-service-configuration-tool"></a>Konfigurowanie typów obiektów w narzędziu konfiguracji usługi sieci Web
Poniższe kroki pokazują, jak skonfigurować typy obiektów dla źródła danych w narzędziu konfiguracji usługi sieci Web.

1. Następnym krokiem jest zdefiniowanie schematu przestrzeni łącznika. Jest to osiągane przez utworzenie typu obiektu i zdefiniowanie ich typów obiektów. W lewym okienku kliknij pozycję **typy obiektów** , a następnie kliknij przycisk **Dodaj** . Spowoduje to otwarcie ekranu poniżej. Dodaj nowy typ obiektu i podaj nazwę. Kliknij przycisk **OK** .

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-types.png)

2. Dodanie typu obiektu zapewnia Poniższy ekran.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-type-employee.png)

3. Prawe okienko odpowiadające typowi obiektu umożliwia zachowanie atrybutów i ich właściwości dla wybranego typu obiektu. Kliknięcie przycisku Dodaj umożliwia wyświetlenie ekranu poniżej, gdzie jeden może dodawać atrybuty.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employeeid-string.png)

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-id-string.png)

4. Poniższy ekran jest wyświetlany po dodaniu wszystkich wymaganych atrybutów.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-object-type-02.png)

5. Typ obiektu i atrybuty utworzone po utworzeniu zawierają puste przepływy pracy, które spełniają operacje wykonywane w Microsoft Identity Manager (MIM).


## <a name="configure-workflows-in-the-web-service-configuration-tool"></a>Konfigurowanie przepływów pracy w narzędziu konfiguracji usługi sieci Web

Następnym krokiem jest skonfigurowanie przepływów pracy dla typu obiektu. Pliki przepływu pracy to seria działań, które są używane przez łącznik usług sieci Web w czasie wykonywania. Przepływy pracy są używane do implementowania odpowiedniej operacji programu MIM. Narzędzie Konfiguracja usługi sieci Web ułatwia tworzenie czterech różnych przepływów pracy:

- Import: Importuj dane ze źródła danych dla następujących dwóch typów przepływów pracy:

    - Pełny import: pełny import, który można skonfigurować.
    - Import Delta: nie jest obsługiwany przez narzędzie konfiguracji usługi sieci Web.

- Eksportuj: Eksportuj dane z programu MIM do połączonego źródła danych. Poniższe trzy akcje są obsługiwane dla operacji. Te akcje można skonfigurować na podstawie wymagań.

    - Dodaj
    - Usuń
    - Zamień

- Hasło: wykonaj zarządzanie hasłami dla użytkownika (typ obiektu). Dla tej operacji są dostępne dwie akcje:

    - Ustaw hasło
    - Zmień hasło

- Testuj połączenie: Skonfiguruj przepływ pracy, aby sprawdzić, czy połączenie z serwerem źródła danych zostało pomyślnie ustanowione.

>[!NOTE]
>Można skonfigurować te przepływy pracy dla projektu lub pobrać domyślny projekt z [Centrum pobierania Microsoft](http://www.microsoft.com/download/details.aspx?id=29944).


### <a name="workflow-designer"></a>Projektant przepływów pracy
Projektant przepływu pracy otwiera obszar roboczy, aby skonfigurować przepływ pracy zgodnie z wymaganiami. Dla każdego typu obiektu (New/Existing) Narzędzie konfiguracji udostępnia węzły dla przepływów pracy, które są obsługiwane przez narzędzie. 

![Projektant przepływów pracy](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-configuration-workflow.png)

Projektant przepływu pracy składa się z następujących elementów interfejsu użytkownika:

   - **Węzły w lewym okienku** : te ułatwienia umożliwiają wybranie projektu, który ma zostać zaprojektowany.

   - **Centralna Projektant przepływu pracy** : w tym miejscu można usunąć działania związane z konfigurowaniem przepływów pracy. Aby wykonać różne operacje programu MIM (eksportowanie, importowanie, zarządzanie hasłami), można użyć standardowych i niestandardowych działań przepływu pracy w programie .NET Workflow Framework 4. Narzędzie konfiguracji usługi sieci Web używa standardowych i niestandardowych działań przepływu pracy. Aby uzyskać więcej informacji o działaniach standardowych, zobacz [Korzystanie z projektantów działań](http://msdn.microsoft.com/library/ee829528.aspx).

      - W środkowym Projektant przepływu pracy czerwony okrąg z wykrzyknikiem obok dowolnego działania wskazuje, że operacja została porzucona i nie jest zdefiniowana prawidłowo i całkowicie. Umieść kursor nad czerwonym kółkiem, aby sprawdzić dokładny błąd. Po poprawnym zdefiniowaniu działania czerwony okrąg zmieni się na żółty znacznik informacji.
      
      - W Projektant przepływu pracy centralnym żółty trójkątny znacznik informacji obok dowolnego działania wskazuje, że działanie jest zdefiniowane, ale istnieje więcej możliwości wykonania działania. Umieść kursor na żółtym trójkącie, aby zobaczyć więcej informacji.

   - **Przybornik** : pakiety wszystkich narzędzi, w tym działania systemowe i niestandardowe oraz wstępnie zdefiniowane instrukcje, aby zaprojektować przepływ pracy. Aby uzyskać więcej informacji, zobacz [Przybornik](http://msdn.microsoft.com/library/aa480213.aspx).
   
   - **Sekcje przybornika** : Przybornik zawiera następujące sekcje i kategorie:
   
      - **Opis** : nagłówek przybornika. Jedna karta uzyskuje dostęp do przybornika i właściwości wybranego działania przepływu pracy. 

      - **Importowanie przepływu pracy** : niestandardowe działania w celu skonfigurowania przepływów pracy importu.
      
      - **Eksportuj przepływ pracy** : niestandardowe działania w celu skonfigurowania przepływów pracy eksportowania.
      
      - **Typowy** : działania niestandardowe w celu skonfigurowania dowolnego przepływu pracy.
      
      - **Debuguj** : działania systemowego przepływu pracy do debugowania zdefiniowane w przepływie pracy 4. Te działania umożliwiają śledzenie problemów dla przepływu pracy.
      
      - **Instrukcje** : działania przepływu pracy w systemie zdefiniowane w przepływie pracy 4. Aby uzyskać więcej informacji, zobacz [Korzystanie z projektantów działań](http://msdn.microsoft.com/library/ee829528.aspx).

   - **Właściwości** : na karcie właściwości są wyświetlane właściwości określonego działania przepływu pracy, które zostało usunięte w obszarze projektanta i wybrane. Na rysunku po lewej stronie są wyświetlane właściwości działania **przypisywania** . Dla każdego działania właściwości różnią się i są używane podczas konfigurowania niestandardowego przepływu pracy. Na tej karcie można zdefiniować atrybuty wybranego narzędzia, które zostało porzucone w centralnym Projektancie przepływu pracy. Aby uzyskać więcej informacji, zobacz [Właściwości](http://msdn.microsoft.com/library/ee342461.aspx).

   - **Pasek zadań:** Pasek zadań zawiera trzy elementy: **zmienne** , **argumenty** i **Importy** . Te elementy są używane razem z działaniami przepływu pracy. Aby uzyskać więcej informacji, zobacz [wprowadzenie do programu developer Windows Workflow Foundation (WF) w programie .NET 4](http://msdn.microsoft.com/library/ee342461.aspx).



## <a name="configure-a-full-import-workflow-in-the-web-service-configuration-tool"></a>Konfigurowanie pełnego przepływu pracy w narzędziu konfiguracji usługi sieci Web
Poniższe kroki pokazują, jak skonfigurować pełne przepływy pracy dla interfejsu API REST za pomocą narzędzia konfiguracji usługi sieci Web.

>[!WARNING]
>Ten przykład tworzy tylko przepływ pracy. Mogą być wymagane modyfikacje przepływu pracy, takie jak użycie logiki niestandardowej w interfejsie API.

1. Wybierz przepływ pracy pełnego importu do skonfigurowania. **Argumenty** i **Importy** są już zdefiniowane i są specyficzne dla działań. Aby uzyskać więcej informacji, zobacz następujące ekrany.

   ![Pełny import argumentów przepływu pracy](media/microsoft-identity-manager-2016-ma-ws-restgeneric/arguments.png)

   ![Zaimportowane przestrzenie nazw](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imported-name-spaces.png)

   Po ponownym skonfigurowaniu wywołań należy zmienić nazwy atrybutów, które zmieniają lub dodają przestrzeń nazw do zmiennych, które odwołują się do struktury powrotu interfejsów API i obiektów odwołujących się do starej przestrzeni nazw. Przybornik w okienku po prawej stronie zawiera wszystkie niestandardowe działania specyficzne dla przepływu pracy, które są wymagane do konfiguracji. Przypisz wartości do zmiennych, które będą używane dla logiki. Przejdź do dolnej części projektanta centralnego przepływu pracy i Zadeklaruj zmienne. Zmienne są deklarowane w następnym kroku.

2. Dodawanie działania sekwencji. Przeciągnij projektanta działania **sekwencji** z **przybornika** i upuść go na powierzchnię Projektant przepływu pracy systemu Windows. Zapoznaj się z poniższymi ekranami. Działanie [sekwencji](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) zawiera uporządkowaną kolekcję działań podrzędnych wykonywanych w określonej kolejności.
   
    ![Działanie sekwencji](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imports.png)

3. Aby dodać zmienną, Znajdź pozycję **Utwórz zmienną** . Wpisz _wsResponse_ w polu **Nazwa** , wybierz listę rozwijaną **Typ zmiennej** , a następnie wybierz pozycję **Przeglądaj w poszukiwaniu typów** . Zostanie wyświetlone okno dialogowe. Wybierz **wygenerowaną**  >  **GETALL**  >  **odpowiedź** GetAll. Nie zaznaczaj **zakresu** i wartości **domyślnych** . Alternatywnie możesz ustawić te wartości przy użyciu widoku **Właściwości** .

   ![Domyślna odpowiedź](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list.png)

4. Przeciągnij jeden więcej projektanta działań **sekwencji** z **przybornika** w ramach już dodanego działania sekwencji.

5. Przeciągnij **WebServiceCallActivity** w obszarze **wspólne.** To działanie służy do wywoływania operacji usługi sieci Web dostępnej po przeprowadzeniu odnajdywania. Jest to działanie niestandardowe i jest wspólne w różnych scenariuszach operacji. 

    ![Operacja nazwy usługi](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-operation-workflow.png)

   Aby użyć operacji usługi sieci Web, ustaw następujące właściwości:
   
      - **Nazwa usługi** : Wprowadź nazwę usługi sieci Web.
      - **Nazwa punktu końcowego** : Określ nazwę punktu końcowego dla wybranej usługi.
      - **Nazwa operacji** : Określ odpowiednią operację dla usługi.
      - **Argument** : Wybierz **argumenty** . W następnym oknie dialogowym Przypisz wartości argumentów, jak pokazano na poniższym rysunku:
      
         ![Przypisz argumenty](media/microsoft-identity-manager-2016-ma-ws-restgeneric/get-all.png)

         >[!IMPORTANT]
         >Nie zmieniaj **nazwy** , **kierunku** ani **typu** dla argumentu przy użyciu tego okna dialogowego. Jeśli dowolna z tych wartości ulegnie zmianie, działanie jest nieprawidłowe. Ustaw tylko **wartość** argumentu. Jak pokazano na rysunku, wartość *wsResponse* jest ustawiona.

6. Dodaj działanie **foreach** tuż poniżej **WebServiceCallActivity.** To działanie służy do iteracji wszystkich atrybutów (kotwic i niekotwiczenia) typu obiektu. Przeciągnięcie tego działania na powierzchnię Projektant przepływu pracy powoduje automatyczne wyliczenie wszystkich nazw atrybutów dla obiektu. Ustaw wymagane wartości zgodnie z poniższym ekranem:

   ![Działanie wywołania usługi sieci Web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach.png)

7. W niektórych przypadkach może być konieczne otwarcie generated.dll znajdującego się w pliku WsConfig. Skopiuj ten plik WsConfig i zmień jego nazwę na rozszerzenie zip. Otwórz i Wyodrębnij generated.dll przy użyciu preferowanego narzędzia reflektora platformy .NET.

   ![Plik konfiguracji](media/microsoft-identity-manager-2016-ma-ws-restgeneric/config-files.png)

8. Określ publiczną przestrzeń nazw dla _EmployeeList_ :

    ![Kod listy pracowników](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list-code.png)

    Następnie Dodaj ten zwrot do przepływu pracy **foreach** :

    ![Dodaj listę pracowników do przepływu pracy ForEach](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach-employee-list.png)

9. Przeciągnij działanie **CreateCSEntryChangeScope** w treści **foreach** . To działanie służy do tworzenia wystąpienia obiektu CSEntryChange w domenie przepływu pracy dla każdego odpowiedniego rekordu podczas pobierania danych z docelowego źródła danych. Przeciąganie tego działania zapewnia Poniższy ekran. Działania związane z **kotwicą** są automatycznie dziedziczone. Zaktualizuj wartość **DN** do preferowanej nazwy domeny.

    ![Działanie tworzenia zakresu zmian wpisów CS](media/microsoft-identity-manager-2016-ma-ws-restgeneric/createcsentry.png)

    >[!NOTE]
    >Wartości zakotwiczenia i nazwy obiektów różnią się w zależności od uwidocznionej usługi sieci Web. Na rysunku przedstawiono przykład.

10. Przeciągnij działanie **CreateAttributeChange** **poniżej działania elementu** . Liczba działań do przeciągnięcia jest równa liczbie atrybutów niezakotwiczonych. Zobacz poniższą ilustrację, aby uzyskać odwołanie.

    ![Utwórz kotwicę](media/microsoft-identity-manager-2016-ma-ws-restgeneric/create-anchor-attribute.png)

    >[!NOTE]
    >Aby użyć tego działania, wybierz i przypisz odpowiednie pola z listy rozwijanej i przypisz wartości. W przypadku atrybutów wielowartościowych Usuń wiele działań **CreateValueChangeActivity** wewnątrz działania **CreateAttributeChangeActivity** .

11. Zapisz ten projekt w lokalizacji `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` . Następnie skonfiguruj agenta zarządzania zgodnie z opisem w temacie [Konfiguracja usługi sieci Web](microsoft-identity-manager-2016-ma-ws-maconfig.md).

    ![Zapisz projekt REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/sample-rest.png)
    
    Projekty domyślne należy pobrać i zapisać w lokalizacji `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` w systemie docelowym. Projekty są następnie widoczne w Kreatorze łącznika usługi sieci Web.
    
    Podczas uruchamiania pliku wykonywalnego zostanie wyświetlony monit o określenie lokalizacji instalacji. Wprowadź lokalizację zapisywania.
    
    >[!IMPORTANT]
    >Plik projektu można zapisać i otworzyć z dowolnej lokalizacji (z odpowiednimi uprawnieniami dostępu do jego wykonawcy). `Synchronization Service\Extension`W Kreatorze łącznika usługi sieci Web, dostępnym za pomocą interfejsu użytkownika synchronizacji programu MIM, można wybrać tylko pliki projektu, które są zapisywane w folderze.
    
    Użytkownik, który uruchomił narzędzie konfiguracji usługi sieci Web, musi mieć następujące uprawnienia:
    
       - Pełna kontrola do folderu rozszerzenia usługi synchronizacji.
       - Dostęp do odczytu do klucza rejestru `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` , za pomocą którego znajduje się ścieżka folderu rozszerzenia.


## <a name="next-steps"></a>Następne kroki

- [Przegląd ogólnego łącznika usługi sieci Web](microsoft-identity-manager-2016-ma-ws.md)
- [Instalowanie narzędzia konfiguracji usługi sieci Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Przewodnik wdrażania protokołu SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Przewodnik wdrażania REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Konfiguracja usługi sieci Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
