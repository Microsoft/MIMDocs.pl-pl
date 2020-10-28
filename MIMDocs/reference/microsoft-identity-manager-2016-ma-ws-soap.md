---
title: Przewodnik przepływu pracy łącznika usługi sieci Web dla protokołu SOAP | Microsoft Docs
description: W tym artykule opisano sposób tworzenia nowego projektu dla źródła danych protokołu SOAP przy użyciu narzędzia konfiguracji usługi sieci Web.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 54f9eb08ce8c400aac5c66467a797bcd3cb097a0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760852"
---
# <a name="web-service-connector-workflow-guide-for-soap"></a>Przewodnik przepływu pracy łącznika usługi sieci Web dla protokołu SOAP

W tym artykule opisano sposób tworzenia nowego projektu dla źródła danych w narzędziu konfiguracji usługi sieci Web. Wykonaj następujące kroki, aby utworzyć projekt.

1.  Otwórz Narzędzie konfiguracji usługi sieci Web. Otwiera pusty projekt.

    ![Narzędzie konfiguracji usługi sieci Web](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-01.png)

2.  Wybierz pozycję **projekt protokołu SOAP** , a następnie wybierz pozycję **Dodaj** .

    ![Projekt protokołu SOAP](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-02.png)

3.  Na następnej stronie podaj następujące informacje, a następnie wybierz pozycję **dalej** :

    - Nowa nazwa usługi sieci Web
    - Adres (ścieżka WSDL) do pobrania uwidocznionych usług, punktów końcowych i operacji
    - Przestrzeń nazw
    - Tryb zabezpieczeń (typ uwierzytelniania)
  
4.  W tym przykładzie zostanie wyświetlona strona **poświadczenia** z wymaganiami dotyczącymi *podstawowego* trybu zabezpieczeń (tryb, który został wybrany w poprzednim kroku). Jeśli dla trybu zabezpieczeń określono wartość "none", zostanie wyświetlona strona poświadczeń. Wybierz pozycję **Dalej** .

    ![Ekran usługi SOAP z nazwą użytkownika i hasłem](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service.png)

5.  Do pobrania informacji o usłudze jest uzyskiwany dostęp do ścieżki WSDL i zostanie wyświetlona lista uwidocznionych funkcji. Jeśli wprowadzona ścieżka WSDL jest nieprawidłowa, narzędzie konfiguracji nie może pobrać informacji o usłudze i zgłosi błąd.

    ![ekran postępu pobierania usługi sieci Web](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-progress.png)

6.  Po przeprowadzeniu odnajdywania program wyświetla listę znalezionych punktów końcowych i operacji. Wybierz pozycję **Zakończ** .

    ![Wykryto punkty końcowe usługi SOAP i operacje](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service-endpoints.png)

7.  Kompilacja jest wykonywana. Kompilacja jest procesem kompilowania zestawu kontraktu danych, który może być czasochłonną operacją. Użytkownik jest informowany o ewentualnych błędach kompilacji. Po przeprowadzeniu odnajdywania narzędzie wyświetli następującą stronę:

    ![Odnajdywanie protokołu SOAP](media/microsoft-identity-manager-2016-ma-ws-soap/soap-discovery.png)

8.  Rozwijanie **projektu protokołu SOAP** i wybieranie uwidocznionego punktu końcowego podane poniżej ekranu. Ten ekran zawiera listę operacji, które są zadeklarowane w punkcie końcowym.

    ![Operacje zadeklarowane w punkcie końcowym](media/microsoft-identity-manager-2016-ma-ws-soap/basic-http-binding.png)

9.  Rozwijanie punktu końcowego wyświetla listę operacji. Operacja jest funkcją zadeklarowaną przez punkt końcowy. Każda operacja dotyczy typu zadania, które można wykonać w ramach usługi. Ten ekran zawiera listę argumentów zadeklarowanych dla operacji. Te argumenty są następnie definiowane, gdy operacja jest używana podczas konfigurowania przepływów pracy.

    ![Rozwinięte punkty końcowe](media/microsoft-identity-manager-2016-ma-ws-soap/get-employee-byid.png)

10. Następnym krokiem jest zdefiniowanie schematu przestrzeni łącznika, który jest osiągany przez utworzenie typu obiektu i zdefiniowanie ich typów obiektów. Wybierz pozycję **typy obiektów** , a następnie wybierz pozycję **Dodaj** . W nowym oknie Dodaj nowy typ obiektu i podaj nazwę. Wybierz przycisk **OK** .

    ![Definiowanie typu obiektu](media/microsoft-identity-manager-2016-ma-ws-soap/object-types.png)

11. Dodanie typu obiektu zapewnia Poniższy ekran.

    ![Wyświetlanie nowo utworzonego typu obiektu](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-employee.png)

12. Prawe okienko odpowiadające typowi obiektu umożliwia zachowanie atrybutów i ich właściwości dla wybranego typu obiektu. Wybierz pozycję **Dodaj** . Nowe okno zostanie otwarte w celu dodania atrybutów:

    ![Atrybut i typ danych](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-firstname.png)

    ![Wybrano opcję atrybutu i typu danych z zakotwiczeniem](media/microsoft-identity-manager-2016-ma-ws-soap/employeeid-string.png)

13. Po dodaniu wszystkich wymaganych atrybutów zostanie wyświetlony następujący ekran:

    ![Typ obiektu z informacjami o atrybucie](media/microsoft-identity-manager-2016-ma-ws-soap/soap-project.png)

14. Typ obiektu i atrybuty utworzone po utworzeniu zawierają puste przepływy pracy, które spełniają operacje wykonywane w Microsoft Identity Manager 2016 (MIM).

    ![Typy obiektów przedstawiają operacje, które pracownik może wykonywać](media/microsoft-identity-manager-2016-ma-ws-soap/object-types-operations.png)


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

![Projektant przepływów pracy](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-workflow.png)

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


<h2 id="full-import-workflows">Konfigurowanie pełnego przepływu pracy w narzędziu konfiguracji usługi sieci Web</h2>
Poniższe kroki pokazują, jak skonfigurować pełne przepływy pracy dla protokołu SOAP przy użyciu narzędzia konfiguracji usługi sieci Web.

>[!WARNING]
>Ten przykład tworzy tylko przepływ pracy. Mogą być wymagane modyfikacje przepływu pracy, takie jak użycie logiki niestandardowej w interfejsie API.

1. Wybierz przepływ pracy pełnego importu do skonfigurowania. **Argumenty** i **Importy** są już zdefiniowane i są specyficzne dla działań. Aby uzyskać więcej informacji, zobacz następujące ekrany.

   ![Pełny import argumentów przepływu pracy](media/microsoft-identity-manager-2016-ma-ws-soap/arguments.png)
 
   ![Zaimportowane przestrzenie nazw](media/microsoft-identity-manager-2016-ma-ws-soap/imports.png)

   Po ponownym skonfigurowaniu wywołań Zmień nazwy atrybutów, które się zmieniają, Dodaj lub Zmień przestrzeń nazw na zmienne, które odwołują się do struktury powrotu dla typów interfejsów API i obiektów odnoszących się do starej przestrzeni nazw. Przybornik w okienku po prawej stronie zawiera wszystkie niestandardowe działania specyficzne dla przepływu pracy, które są wymagane do konfiguracji. Przypisz wartości do zmiennych, które będą używane dla logiki. Przejdź do dolnej części projektanta centralnego przepływu pracy i Zadeklaruj zmienne. Zmienne są deklarowane w następnym kroku.

2. Dodawanie działania sekwencji. Przeciągnij projektanta działania **sekwencji** z **przybornika** i upuść go na powierzchnię Projektant przepływu pracy systemu Windows. Zapoznaj się z poniższymi ekranami. Działanie [sekwencji](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) zawiera uporządkowaną kolekcję działań podrzędnych wykonywanych w określonej kolejności.
   
    ![Działanie sekwencji](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-sequence.png)

3. Aby dodać zmienną, Znajdź pozycję **Utwórz zmienną** . Wpisz _wsResponse_ w polu **Nazwa** , wybierz listę rozwijaną **Typ zmiennej** , a następnie wybierz pozycję **Przeglądaj w poszukiwaniu typów** . Zostanie wyświetlone okno dialogowe. Wybierz **wygenerowaną**  >  **domyślną**  >  **odpowiedź** . Nie zaznaczaj **zakresu** i wartości **domyślnych** . Alternatywnie możesz ustawić te wartości przy użyciu widoku **Właściwości** .

   ![Domyślna odpowiedź](media/microsoft-identity-manager-2016-ma-ws-soap/default-response.png)

   ![Właściwości pełnego importu](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-properties.png)

4. Teraz Dodaj wszystkie inne zmienne i poniżej są ekranem końcowym.

   ![Wszystkie zmienne importu](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-variables.png)

5. Przeciągnij jeden więcej projektanta działań **sekwencji** z **przybornika** w ramach już dodanego działania sekwencji.

6. Przeciągnij **WebServiceCallActivity** w obszarze **wspólne.** To działanie służy do wywoływania operacji usługi sieci Web dostępnej po przeprowadzeniu odnajdywania. Jest to działanie niestandardowe i jest wspólne w różnych scenariuszach operacji. 

    ![Operacja nazwy usługi](media/microsoft-identity-manager-2016-ma-ws-soap/service-name-operation.png)

   Aby użyć operacji usługi sieci Web, ustaw następujące właściwości:
   
      - **Nazwa usługi** : Wprowadź nazwę usługi sieci Web.
      - **Nazwa punktu końcowego** : Określ nazwę punktu końcowego dla wybranej usługi.
      - **Nazwa operacji** : Określ odpowiednią operację dla usługi.
      - **Argument** : Wybierz **argumenty** . W następnym oknie dialogowym Przypisz wartości argumentów, jak pokazano na poniższym rysunku:
      
         ![Przypisz argumenty](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

         >[!IMPORTANT]
         >Nie zmieniaj **nazwy** , **kierunku** ani **typu** dla argumentu przy użyciu tego okna dialogowego. Jeśli dowolna z tych wartości ulegnie zmianie, działanie jest nieprawidłowe. Ustaw tylko **wartość** argumentu. Jak pokazano na rysunku, wartość *wsResponse* jest ustawiona.

7. Dodaj działanie **foreach** tuż poniżej **WebServiceCallActivity.** To działanie służy do iteracji wszystkich atrybutów (kotwic i niekotwiczenia) typu obiektu. Przeciągnięcie tego działania na powierzchnię Projektant przepływu pracy powoduje automatyczne wyliczenie wszystkich nazw atrybutów dla obiektu. Ustaw wymagane wartości zgodnie z poniższym ekranem:

   ![Działanie wywołania usługi sieci Web](media/microsoft-identity-manager-2016-ma-ws-soap/webservicecallactivity.png)

8. Przeciągnij działanie **CreateCSEntryChangeScope** w treści **foreach** . To działanie służy do tworzenia wystąpienia obiektu CSEntryChange w domenie przepływu pracy dla każdego odpowiedniego rekordu podczas pobierania danych z docelowego źródła danych. Przeciąganie tego działania zapewnia Poniższy ekran. Działania związane z **kotwicą** są automatycznie dziedziczone.

    ![Działanie tworzenia zakresu zmian wpisów CS](media/microsoft-identity-manager-2016-ma-ws-soap/createcsentrychangescope.png)

9.  Ustaw wartość wyrażenia DN jako `‘string.Concat ("Employee",item.EmployeeID)’` . Ustaw **AnchorValue** dla _IDPracownika_ na **"Convert. ToString (Item. IDPracownika) '** . Ustaw wartość **ObjectTypeName** jako _pracownika_ . Po wprowadzeniu tych zmian zobaczysz następujący ekran:

    ![Pobierz identyfikator pracownika](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

    >[!NOTE]
    >Wartości zakotwiczenia i nazwy obiektów różnią się w zależności od uwidocznionej usługi sieci Web. Na rysunku przedstawiono przykład.

10. Przeciągnij działanie **CreateAttributeChange** **poniżej działania elementu** . Liczba działań do przeciągnięcia jest równa liczbie atrybutów niezakotwiczonych. Zobacz poniższą ilustrację, aby uzyskać odwołanie.

    ![Utwórz kotwicę](media/microsoft-identity-manager-2016-ma-ws-soap/create-anchor.png)

11. Przeciągnij **CreateValueChangeActivity** w działaniu **CreateAttributeChange** i ustaw wartość atrybutu na ekran poniżej.

    ![Zmień atrybut](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change.png)

    >[!NOTE]
    >Aby użyć tego działania, wybierz i przypisz odpowiednie pola z listy rozwijanej i przypisz wartości. W przypadku atrybutów wielowartościowych Usuń wiele działań **CreateValueChangeActivity** wewnątrz działania **CreateAttributeChangeActivity** .

12. Aby dodać warunki dla atrybutu, należy dodać działanie **if** , jak pokazano na poniższym rysunku:

    ![Działanie if](media/microsoft-identity-manager-2016-ma-ws-soap/if.png)

13. Na koniec Dodaj działanie **Assign** i Ustaw wyrażenie, jak pokazano na poniższym rysunku:

    ![Przypisywanie działania i Ustawianie wyrażenia](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change-email.png)

14. Zapisz ten projekt w lokalizacji `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .
    
    Projekty domyślne należy pobrać i zapisać w lokalizacji `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` w systemie docelowym. Projekty są następnie widoczne w Kreatorze łącznika usługi sieci Web.
    
    Podczas uruchamiania pliku wykonywalnego zostanie wyświetlony monit o określenie lokalizacji instalacji. Wprowadź lokalizację zapisywania.
    
    >[!IMPORTANT]
    >Plik projektu można zapisać i otworzyć z dowolnej lokalizacji (z odpowiednimi uprawnieniami dostępu do jego wykonawcy). `Synchronization Service\Extension`W Kreatorze łącznika usługi sieci Web, dostępnym za pomocą interfejsu użytkownika synchronizacji programu MIM, można wybrać tylko pliki projektu, które są zapisywane w folderze.
    
    Użytkownik, który uruchomił narzędzie konfiguracji usługi sieci Web, musi mieć następujące uprawnienia:
    
       - Pełna kontrola do folderu rozszerzenia usługi synchronizacji.
       - Dostęp do odczytu do klucza rejestru `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` , za pomocą którego znajduje się ścieżka folderu rozszerzenia.


## <a name="configure-export-workflows-in-the-web-service-configuration-tool"></a>Konfigurowanie przepływów pracy eksportowania w narzędziu konfiguracji usługi sieci Web
W poniższych sekcjach pokazano, jak wyeksportować przepływy pracy za pomocą narzędzia konfiguracji usługi sieci Web.

<h3 id="attribute-change-anchor">Dodaj przepływy pracy</h3>
Dodaj przepływy pracy eksportowania, wykonując następujące kroki w narzędziu konfiguracji usługi sieci Web.

1. Wybierz przepływ pracy eksportowania do skonfigurowania. W obszarze **Eksportuj** wybierz pozycję **Dodaj** . **Argumenty** i **Importy** są już zdefiniowane i są specyficzne dla działań. Aby uzyskać informacje, zobacz następujące ekrany.

    ![Dodaj](media/microsoft-identity-manager-2016-ma-ws-soap/add.png)

2. Dodawanie działania **sekwencji** . Przeciągnij projektanta działania **sekwencji** z **przybornika** i upuść go na powierzchnię Projektant przepływu pracy systemu Windows. Działanie [sekwencji](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) zawiera uporządkowaną kolekcję działań podrzędnych wykonywanych w określonej kolejności. Wybierz pozycję **Utwórz zmienną** . Przypisz wartości do zmiennych, które będą używane dla logiki.

    ![Eksportowanie](media/microsoft-identity-manager-2016-ma-ws-soap/export-add.png)

    >[!NOTE]
    >Procedura dodawania zmiennej została opisana w sekcji dotyczącej tworzenia <a href="#full-import-workflows">pełnych przepływów pracy importu</a>.

3. Przeciągnij działanie **foreach** w ramach już dodanego działania **sekwencji** , aby wykonać iterację wartości atrybutów zakotwiczenia.

4. Wybierz pozycję **Właściwości** , a następnie ustaw **wartości** na ekranie poniżej. W tym miejscu **objectToExport** jest argumentem.

    ![Ustaw właściwości działania ForEach](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-sequence.png)

5. Ustaw wartość **DisplayName jako właściwość** **\< kotwicy \> foreach**

   ![Ustaw nazwę wyświetlaną](media/microsoft-identity-manager-2016-ma-ws-soap/add-sequence.png)

6. Ustaw **elementu TypeArgument** jako `Microsoft.MetadirectoryServices.AnchorAttribute` .

   ![Ustawianie argumentu typu](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor.png)

7. Dodawanie działania **Switch** **w treści** **foreachattribute** .

   ![Dodawanie działania Switch](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-types.png)

8. Dodaj wyrażenie jak na poniższym ekranie.

   ![Dodaj wyrażenie](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Wybierz pozycję **Dodaj nową wielkość liter** i wprowadź wartość dla **IDPracownika** . Przeciągnij działanie **sekwencji** i w ramach niego Dodaj działanie **Assign** .

    ![Dodaj nową wielkość liter i przypisz ją do sekwencji](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-employeeid.png)

10. Przypisz właściwości **do** i **wartość** dla działania **przypisywania** .

    ![Przypisywanie właściwości do i wartości](media/microsoft-identity-manager-2016-ma-ws-soap/anchor-attribute-name.png)

11. Działanie **foreach** jest używane na potrzeby wartości zakotwiczenia. Dodaj kolejną aktywność **foreach** , aby przypisać wartości niezakotwiczone. W tym przykładzie jest używana kotwica **AttributeChange** .

    ![Dodaj kolejną aktywność ForEach z zakotwiczeniem AttributeChange](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-change.png)

12. Dodawanie działania **Switch** w treści **foreach** zakotwiczenia **AttributeChange** .   

    ![Dodawanie działania Switch dla zakotwiczenia AttributeChange](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-name-wrapper.png)

13. Dodaj wyrażenie jak na poniższym ekranie.

    ![Dodaj wyrażenie dla działania Switch](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-expression.png)

14. Wybierz pozycję **Dodaj nową wielkość liter** i wprowadź wartość dla **FirstName** . Przeciągnij działanie **sekwencji** i w ramach niego Dodaj działanie **Assign** . Przypisz właściwości **do** i **wartość** dla działania **przypisywania** .

    ![Dodaj nowy przypadek dla sekwencji](media/microsoft-identity-manager-2016-ma-ws-soap/switch-firstname.png)

15. Dodaj wartości wymaganych atrybutów, takich jak **LastName** , **email** i tak dalej. 

    ![Dodawanie wartości wymaganych atrybutów](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

16. W obszarze **wspólne** przeciągnij **WebServiceCallActivity** i ustaw **wartości** dla jej **argumentów** .

    ![Dodawanie działania usługi sieci Web i ustawianie wartości](media/microsoft-identity-manager-2016-ma-ws-soap/add-employee-attribute.png)

    >[!IMPORTANT]
    >Nie zmieniaj **nazwy** , **kierunku** ani **typu** dla argumentu przy użyciu tego okna dialogowego. Jeśli dowolna z tych wartości ulegnie zmianie, działanie jest nieprawidłowe. Ustaw tylko **wartość** argumentu. Jak pokazano na rysunku, wartość *wsResponse* jest ustawiona.

17.  Na koniec Dodaj działanie **if** , aby sprawdzić odpowiedzi, które są zwracane z operacji usługi sieci Web.

Ukończono tworzenie przepływu pracy eksportu z operacją **dodawania** :

![Ukończony przepływ pracy eksportu](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

Zapisz ten projekt w lokalizacji `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="delete-workflows"></a>Usuń przepływy pracy
Usuń przepływy pracy eksportowania, wykonując następujące kroki w narzędziu konfiguracji usługi sieci Web.

1. Wybierz przepływ pracy eksportowania do skonfigurowania. W obszarze **Eksportuj** wybierz pozycję **Usuń** . **Argumenty** i **Importy** są już zdefiniowane i są specyficzne dla działań. Aby uzyskać informacje, zobacz następujące ekrany.

   ![Eksportuj przepływy pracy usuwania](media/microsoft-identity-manager-2016-ma-ws-soap/export-delete.png)

2. Dodawanie działania **sekwencji** . Wybierz pozycję **Utwórz zmienną** . Przypisz wartości do zmiennych, które będą używane dla logiki.

   ![Dodawanie działania sekwencji](media/microsoft-identity-manager-2016-ma-ws-soap/sequence-variables.png)

   >[!NOTE]
   >Procedura dodawania zmiennej została opisana w sekcji dotyczącej tworzenia <a href="#full-import-workflows">pełnych przepływów pracy importu</a>.

3. Przeciągnij działanie **foreach** w ramach już dodanego działania **sekwencji** , aby wykonać iterację wartości atrybutów zakotwiczenia.

4. Wybierz **Właściwości** i ustaw **wartości** na ekranie poniżej. W tym miejscu **objectToExport** jest argumentem.

   ![Ustaw właściwości działania ForEach](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-object-to-export.png)

5. Ustaw wartość **DisplayName** jako `ForEach\<AnchorAttribute\>` :

   ![Ustaw nazwę wyświetlaną](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor-type.png)

6. Ustaw **elementu TypeArgument** jako `Microsoft.MetadirectoryServices.AnchorAttribute` : 

   ![Ustawianie argumentu typu](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-type-argument.png)

7. Dodawanie działania **Switch** **w treści** **foreachattribute** .

   ![Dodawanie działania Switch](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-type.png)

8. Dodaj wyrażenie jak na poniższym ekranie.

   ![Dodaj wyrażenie](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. Wybierz pozycję **Dodaj nową wielkość liter** i wprowadź wartość dla **IDPracownika** . Przeciągnij działanie **sekwencji** i w ramach niego Dodaj działanie **Assign** .

   ![Dodaj nową wielkość liter i przypisz ją do sekwencji](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-default.png)

10. Przypisz właściwości **do** i **wartość** dla działania **przypisywania** .

    ![Przypisywanie właściwości do i wartości](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-attribute-flow.png)

11. W obszarze **wspólne** przeciągnij **WebServiceCallActivity** i ustaw **wartości** dla jej **argumentów** .

    ![Dodawanie działania usługi sieci Web i ustawianie wartości](media/microsoft-identity-manager-2016-ma-ws-soap/delete-employee.png)

    >[!IMPORTANT]
    >Nie zmieniaj **nazwy** , **kierunku** ani **typu** dla argumentu przy użyciu tego okna dialogowego. Jeśli dowolna z tych wartości ulegnie zmianie, działanie jest nieprawidłowe. Ustaw tylko **wartość** argumentu. Jak pokazano na rysunku, wartość *IDPracownika* jest ustawiona.

12. Na koniec Dodaj działanie **if** , aby sprawdzić odpowiedzi zwrócone przez operację usługi sieci Web.

Ukończono usuwanie przepływu pracy eksportu z operacją **usuwania** :

![Usunięty przepływ pracy eksportu](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

<!-- Image of completed Delete operation is missing from document -->

Zapisz ten projekt w lokalizacji `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


### <a name="replace-workflows"></a>Zastąp przepływy pracy
Zastąp eksport przepływów pracy, wykonując następujące kroki w narzędziu konfiguracji usługi sieci Web.

1. Wybierz przepływ pracy eksportowania do skonfigurowania. W obszarze **Eksportuj** wybierz pozycję **Zamień** . **Argumenty** i **Importy** są już zdefiniowane i są specyficzne dla działań. Aby uzyskać informacje, zobacz poniższy ekran.

   ![Zastępowanie przepływu pracy](media/microsoft-identity-manager-2016-ma-ws-soap/replace.png)

2. Dodawanie działania **sekwencji** .

3. Przeciągnij działanie **foreach** dla **\<> zakotwiczania.**

4. Dodaj kolejną **aktywność \<>foreach AttributeChange** , aby przypisać wartości niezakotwiczone.

5. Na koniec ekran wygląda tak, jak na poniższym rysunku. Instrukcje dotyczące konfigurowania tego działania znajdują się w sekcji dotyczącej <a href="#attribute-change-anchor">dodawania przepływów pracy eksportowania</a>.

   ![Instrukcja ForEach z działaniem Switch i atrybutem kotwicy](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

6. W obszarze **wspólne** przeciągnij **WebServiceCallActivity** i ustaw **wartości** dla jej **argumentów** .

   ![Dodawanie działania usługi sieci Web i ustawianie wartości](media/microsoft-identity-manager-2016-ma-ws-soap/wsresponse.png)

   >[!IMPORTANT]
   >Nie zmieniaj **nazwy** , **kierunku** ani **typu** dla argumentu przy użyciu tego okna dialogowego. Jeśli dowolna z tych wartości ulegnie zmianie, działanie jest nieprawidłowe. Ustaw tylko **wartość** argumentu. Jak pokazano na tym rysunku, wartość *Pracownik* jest ustawiony.

7. Na koniec Dodaj działanie **if** , aby sprawdzić odpowiedzi, które są zwracane z operacji usługi sieci Web.

Zastępowanie przepływu pracy eksportu z operacją **replace** zostało zakończone:

![Zamień przepływ pracy eksportu](media/microsoft-identity-manager-2016-ma-ws-soap/operation-name-update-employee.png)

Zapisz ten projekt w lokalizacji `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` .


## <a name="debug-activities"></a>Działania debugowania
Dostępne są następujące działania niestandardowe ułatwiające Debugowanie szablonu przepływu pracy.

### <a name="log-activity"></a>Aktywność dziennika
Działanie **log** służy do zapisywania wiadomości tekstowych w pliku dziennika. Aby uzyskać więcej informacji, zobacz [Rejestrowanie](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx).

>[!NOTE]
>Jeśli nie możesz łatwo debugować przepływu pracy, spróbuj debugować przepływ pracy w środowisku produkcyjnym.  

Aby użyć działania **log** , ustaw następujące właściwości. Właściwości są widoczne po wybraniu działania w Projektant przepływu pracy i wyświetleniu **Właściwości** działania.
<!-- Properties missing from document -->
<!-- Image of Log activity GUI missing from document -->

### <a name="writeline-activity"></a>Działanie WriteLine
Działanie **WriteLine** służy do zapisywania komunikatów tekstowych w składniku zapisywania dostawcy. Jeśli moduł zapisujący nie jest dostępny, działanie [WriteLine](http://127.0.0.1:47873/help/1-7016/ms.help?method=page&id=T%3ASYSTEM.ACTIVITIES.STATEMENTS.WRITELINE&product=VS&productVersion=100&topicVersion=100&locale=EN-US&topicLocale=EN-US&embedded=true) zapisuje tekst w oknie konsoli.

<!-- Image of WriteLine activity GUI missing from document -->

W polu tekstowym Napisz komunikat, który ma być widoczny w miejscu docelowym składnika zapisywania.

>[!IMPORTANT]
>Nie można użyć okna konsoli dla tego działania. Użyj innego składnika zapisywania danych wyjściowych okna dla tego zadania.

Aby użyć działania **WriteLine** , ustaw następujące właściwości. Właściwości są widoczne po wybraniu działania w Projektant przepływu pracy i wyświetleniu **Właściwości** działania.

- **Poziom dziennika** : określa ilość zawartości do zapisania w wartości dziennika. Możliwe wartości są następujące:

    - Wysoki: Zapisz komunikat **LogText** w pliku dziennika, Jeśli ważność dziennika jest ustawiona na wartość wysoki.
    - Pełne: Zapisz komunikat **LogText** w pliku dziennika, Jeśli ważność dziennika ma wartość verbose.
    - Wyłączone: nie zapisuj w pliku dziennika.
- **LogText** : określa zawartość tekstową do zapisu w dzienniku.
- **Tag** : dodaje tag do tekstu w celu zidentyfikowania typu zawartości, która jest zapisywana w dzienniku. Możliwe wartości to: Error, Trace lub Warning.

<!-- log severity is not defined in this document -->


## <a name="next-steps"></a>Następne kroki

- [Przegląd ogólnego łącznika usługi sieci Web](microsoft-identity-manager-2016-ma-ws.md)
- [Instalowanie narzędzia konfiguracji usługi sieci Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Przewodnik wdrażania protokołu SOAP](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Przewodnik wdrażania REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Konfiguracja usługi sieci Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
