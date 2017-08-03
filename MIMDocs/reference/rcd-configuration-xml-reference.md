---
title: "Odwołanie XML konfiguracji wyświetlania formantu zasobów | Dokumentacja firmy Microsoft"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>Odwołanie XML konfiguracji wyświetlania formantu zasobów

Zasób kontroli wyświetlania konfiguracji (RCDC) zasoby są zasoby zdefiniowane przez użytkownika, których można kontrolować, jak inne zasoby w magazynie danych programu Microsoft Identity Manager 2016 z dodatkiem SP1 (MIM) są wyświetlane w interfejsie użytkownika (UI) dla użytkownika końcowego. Każdy zasób RCDC zawiera plik konfiguracyjny XML, który można zmienić, aby dodać, zmodyfikować lub usunąć tekst interfejsu użytkownika i kontrolek interfejsu użytkownika. MIM 2016 z dodatkiem SP1 zawiera kilka domyślnej konfiguracji RCDC zasobów, natomiast można również utworzyć niestandardowe zasoby RCDC dla niestandardowych zasobów. Aby uzyskać więcej informacji o użyciu RCDC interfejsu użytkownika w portalu programu FIM, zobacz [wprowadzenie do konfigurowania i dostosowywania portalu programu FIM](http://go.microsoft.com/fwlink/?LinkID=165848) w dokumentacji programu FIM.


## <a name="known-issues"></a>Znane problemy

Wartość domyślna w wielu formantów konfiguracji wyświetlania formantów zasobów nie jest obsługiwana.

W tej wersji z wyjątkiem przycisk opcji nie jest obsługiwane wartości domyślne ustawienie formantów w formancie zasobów. Można obejść ten problem, na liście rozwijanej, określając wartość domyślną, który nie jest skojarzony z dowolną wartością, aby wymusić użytkownika, aby zmienić wybór. Aby obejść ten problem z innych formantów, należy użyć przepływu pracy autoryzacji umożliwiają określanie wartości domyślnej podczas przesyłania żądania.

## <a name="basic-structure"></a>Podstawowa struktura

Dane XML dla zasobu RCDC składa się z jednego elementu XML **ObjectControlConfiguration.**

>[!NOTE]
Pełna schematu XSD zobacz dodatek A: domyślne XSD schematu w dalszej części tego dokumentu.

Oto schemat XSD dla elementu ObjectControlConfiguration:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



**ObjectControlConfiguration** element zawiera następujące:

1.  **Element ObjectDataSource**: ten element określa TypeName klasy źródła danych, która używa w formancie zasobów (RC). Opis i definicji schematu Zobacz następującą sekcję źródeł danych w tym dokumencie. **ObjectControlConfiguration** element może zawierać maksymalnie 32 węzły **ObjectDataSource** elementu.

2.  **XmlDataSource**: jest to źródło danych proste jest najczęściej używana do określenia projekt strony podsumowania. Opis i definicji schematu Zobacz następującą sekcję źródeł danych w tym dokumencie. **ObjectControlConfiguration**: element może zawierać maksymalnie 32 węzły **XmlDataSource** elementu.

3.  **Panel**: Administrator może dostosować układ strony RCDC przez zmodyfikowanie elementów wewnątrz elementy panelu. Aby uzyskać więcej informacji zobacz sekcję panelu w dalszej części tego dokumentu. **ObjectControlConfiguration** element musi zawierać tylko jeden element panelu.

4.  **Zdarzenia**: Administratorzy nie może dostarczyć niestandardowe kodzie, ta funkcja jest ograniczona. To jest zdarzenie, które może emitować panelu lub formantu, oparte na zmianę stanu. Aby uzyskać więcej informacji zobacz sekcję zdarzenia w dalszej części tego dokumentu. **ObjectControlConfiguration** elementu mogą zawierać opcjonalnie jeden **zdarzeń** elementu. Ogólnie użycie niestandardowego **zdarzenia** nie jest obsługiwana, chyba że opracowane specjalnie w późniejszym ulepszenia.

## <a name="data-sources"></a>Źródła danych

Program Microsoft Identity Manager używa źródeł danych w sposób, aby powiązać dane składniki interfejsu użytkownika. Dzięki temu można ułatwić podział danych z warstwy prezentacji. Istnieją dwa rodzaje źródeł danych w konfiguracji RCDC danych konfiguracji zasobu: **ObjectDataSource** i **XmlDataSource**.

-   **ObjectDataSources** Określ klasę platformy Microsoft .NET, która dostarcza dane do wersji RC. Brak ustalony zbiór dostępnych typów ObjectDataSources, pod warunkiem, że administrator może wybrać wykorzystać podczas tworzenia RCDCs.

-   **XMLDataSources** zapewniają prosty sposób do struktury danych opartych na języku XML i mogą one używane przez administratorów danych niestandardowych. Dane XML należy określić bezpośrednio w konfiguracji RCDC, chyba że za pomocą wbudowanych, wstępnie zdefiniowanych w strukturze XML. Wbudowana Struktura XML służy do generowania strony podsumowania w wersji RC.

W konfiguracji RCDC atrybuty kontrolek interfejsu użytkownika, które są określone w konfiguracji RCDC do generowania interfejsu użytkownika można powiązać te źródła danych.

### <a name="objectdatasource"></a>Element ObjectDataSource

Program Microsoft Identity manager udostępnia typowe typy źródeł danych w poniższej tabeli, które są dostępne dla wszystkich typów zasobów (z wyjątkiem zaznaczono inaczej).

| Właściwość TypeName                        | Opis     | Obsługuje powiązanie dwukierunkowe | Obsługiwane powiązanie składni        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | Reprezentuje zasób FIM 2010, który jest tworzony, w edytowanym lub wyświetlane. Ścieżka w ciągu powiązania jest nazwą atrybutu. Należy pamiętać, że typ zasobu jest określony przez atrybut TargetObjectType RCDC, a nie w konfiguracji RCDC. Atrybut ConfigurationData. | Tak                     | [AttributeName] Wartość atrybutu obiektu podane według jego nazwy.    |
| PrimaryResourceDeltaDataSource  | To źródło danych tworzy delta XML, który porównuje oryginalnego stanu i bieżący stan zasobu FIM 2010. Wygenerowany delta XML jest używany przez RC kontrolki podsumowania do renderowania interfejsu użytkownika dla żądania przesyłania przez użytkownika.                                    | Nie                      | DeltaXml: </br> Służy to za pomocą formantu podsumowania do wyświetlenia delta.                                                 |
| PrimaryResourceRightsDataSource | To źródło danych zawiera prawa wbudowane dla każdego atrybutu zasobów FIM 2010. Dzięki temu RC w celu określenia klienta z wyprzedzeniem przesyłanie uprawnienia, które użytkownik ma dla tego atrybutu i odpowiednio renderowania interfejsu użytkownika dla tego atrybutu.                     | Nie                      | [AttributeName]                                                                                         |
| SchemaDataSource                | To źródło danych umożliwia dostęp do informacji związanych z schematu, takie jak nazwa wyświetlana, opis, czy ten atrybut jest wymagany, a także informacje o typie zasobu.                                                                                             | Nie                      | [AttributeName]. **Wymagane** wartość logiczna określająca, czy ten atrybut musi mieć wartość jest nieprawidłowy. <br/> [AttributeName]. **DisplayNameString** wartość wskazującą, nazwa wyświetlana wiązania <br/>[AttributeName]. **DescriptionString** wartość wskazującą opis powiązania <br/>[AttributeName]. Wartość StringRegexString, która wskazuje wyrażenie regularne ciągu powiązania. <br/>[AttributeName]. **Nazwa wyświetlana** <br/> [AttributeName]. **Opis** <br/> [AttributeName]. [AttributeName]. **IntergerValueMinimum** <br/>[AttributeName]. **IntergerValueMaximum** <br/>[AttributeName]. **LocalizedAllowedValues**|
| DomainDataSource                | To źródło danych zawiera wyliczenie domen, na podstawie zasobów konfiguracji domeny. Należy pamiętać, że to źródło danych może być używana tylko w RCDCs przeznaczone dla grupy zasobów i użytkownika.                                                                           | Tak                     | Domena           |

Poniżej przedstawiono fragment RCDC przykład, który wiąże trzech źródeł danych kontrolki UocTextBox do edycji atrybucie opis grupy:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

Za pomocą **XMLDataSource**, można określić niestandardowe dane, które RCDC, jaką może wykorzystać do określonego zasobu. W takim przypadku należy określić dane XML w konfiguracji RCDC. Alternatywnie można wbudowana struktura danych XML do renderowania interfejsu użytkownika dla strony podsumowania odwołanie do tego źródła danych. Możesz kontrolować, jaki typ **XMLDataSource** do użycia podczas definiowania w konfiguracji RCDC.


| Właściwość TypeName                 | Opis   | | |
|--------------------------|------------|
| **XMLDataSource**            | Źródło danych reprezentuje dane XML. Można go XSL lub XSL osadzonych formaty: <br/>**XSL format:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll < moje: XmlDataSource moje: Name = " <br/>summaryTransformXsl"my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl">< / moje: XmlDataSource ><br/> **Osadzony format XSL:** <br/> < moje: XmlDataSource moje: Name = "RequestStatusTransformXsl" > <br/> < wersja XSL: stylesheet = "1.0" xmlns:xsl = http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl = "urn: schemas-microsoft-com:xslt" ><br/>< / XSL: stylesheet >< / moje: XmlDataSource >                       |Nie | ```Xpath[;namespaces]``` <br/> Gdzie: wyrażenie Xpath jest nieprawidłowa xpath XML do najczęściej wybierz wymagane Uwaga: "/" (główną) <br/>Przestrzenie nazw jest opcjonalna lista prefiks = ciągi identyfikatora URI, rozdzielone średnikami, jeśli jest to wymagane dla wyrażenia xpath do pracy przed należących do przestrzeni nazw XML. |
| **ReferenceDeltaDataSource** | Źródło danych reprezentuje delty atrybutów wielowartościowych odwołania. Jest używany tylko w konfiguracji RCDC grupy oraz zestaw. <br/> Źródło danych nie jest ograniczona do grup lub zestawów, wymaga zmian kodu na hoście RCDC, aby przesłać takich delty. Obecnie grupa i zestaw są tylko hosty, które rozpoznają tego źródła danych.  | Tak                      | [AttributeName]. Dodaj gdzie [AttributeName] reprezentuje atrybut odwołania, a dane zwrócone dodatków delta. <br/> Przykład: [ReferenceAttribute]. Dodaj <br/>Przykład: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName]. Usuń gdzie [AttributeName] reprezentuje atrybut odwołania, a dane zwrócone usuwania delta. <br/> DeltaXml |
|**RequestDetailsDataSource**| Źródło danych reprezentuje atrybut RequestParameter obiektów żądania. Parametr ustawia maksymalną liczbę wartości atrybutów, które mają być wyświetlane na atrybutu wielowartościowego, jest używane tylko w konfiguracji RCDC dla żądania. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| Nie | DeltaXml |
|**RequestStatusDataSource**| Reprezentuje źródło danych **RequestStatusDetails** atrybutu obiektów żądania. <br/>Jest on używany tylko w konfiguracji RCDC dla żądania.  | Nie | DeltaXml |

-   Aby zdefiniować niestandardowe źródło danych XML:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   Aby użyć wbudowanych kontrolki podsumowania XSL, zdefiniuj źródło danych w następujący sposób:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Jeśli tworzysz RCDC dla typu zasobu niestandardowego służy ta metoda automatycznie renderowanie strony podsumowania dla tego zasobu niestandardowego.

Poniżej przedstawiono przykład sposobu tworzenia karty Podsumowanie w konfiguracji RCDC, przy użyciu PrimaryResourceDeltaDataSource z formantu XMLDataSource za pomocą wbudowanych XSL:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Alternatywnie użytkownik może zastąpić elementu XmlDataSource określony wcześniej o następującym formacie do definiowania układu dostosowanego strony podsumowania. Jako odwołanie domyślny plik XSL programu FIM 2010 podsumowanie znajduje się w dodatek B: domyślne podsumowanie XSL w dalszej części tego dokumentu.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Schemat dla źródeł danych
Oto schemat XSD dla dwóch typów źródeł danych:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>Zdarzenia
Zdarzenie definiuje zmieniania stanu formantu. Rozszerzalność tej funkcji jest ograniczona, ponieważ nie można zapisać dostosowane funkcji (procedura obsługi) do definiowania zachowanie jest po wyzwoleniu zdarzenia. Sam element zdarzeń może być użyty w elemencie panelu. Aby uzyskać więcej informacji zobacz sekcję panelu w dalszej części tego dokumentu. Oto schemat XSD dla elementu zdarzeń:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Zdarzenie jest elementem pustym i zawiera dwa atrybuty.

**Atrybuty:**

1.  **Nazwa**: jest to unikatowa nazwa zdarzenia. Jedynym obsługiwanym zdarzenia w **ObjectControlConfiguration** jest zdarzenie obciążenia. To zdarzenie jest wyzwalane, gdy strona jest ładowana jako pierwsza.

2.  **Program obsługi**: jest to nazwa unikatowa obsługi. Gdy zdarzenie zostanie wyzwolone, zazwyczaj program wywoływana jest metoda obsługi zmian stan formantu. nie są obsługiwane w następujących przypadkach: usuwanie istniejącego programu obsługi z istniejącego formantu, tworzenie nowy program obsługi i dołączanie do istniejącej lub nowej kontrolki.

Przykład:

Oto przykład elementu zdarzenia.
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panel**


Panel element jest element core w układzie RCDC. Oto schemat XSD dla elementu Panel:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

Ten element zawiera element cyklicznego grupowanie. Aby uzyskać więcej informacji zobacz sekcję grupowania w tym dokumencie.

Panel element zawiera cztery atrybuty:

1.  **Nazwa**: Nazwa panelu. Jest to wymagane, atrybut typu ciąg.

2.  **DisplayAsWizard**: ten atrybut jest obecnie przestarzały. Odpowiadającego mu atrybutu VerbContext w konfiguracji RCDC decyduje, jeśli układ zasobów jest w trybie kreatora lub karcie. Jeśli jest ustawiona na wartość 0 (tryb tworzenia), jest również w trybie kreatora. W przeciwnym razie jest w trybie karty. Aby uzyskać więcej informacji zobacz wprowadzenie do konfigurowania i dostosowywania portalu programu FIM w dokumentacji.

3.  **Podpis**: ten atrybut jest obecnie przestarzały. Użytkownik może określić podpisów dla strony przez dołączenie do grupy, która zawiera tylko informacje nagłówka. Aby uzyskać więcej informacji zobacz sekcję grupowania w tym dokumencie.

4.  **AutoValidate**: jest to opcjonalny logiczny atrybut. Jeśli równa PRAWDA, sprawdzanie poprawności jest wyzwalany dla każdego formantu w bieżącej karty. Domyślnie, jeśli brakuje atrybutu jest ustawiona na true. Mogą być używane w połączeniu z właściwością wyrażenia regularnego. Aby uzyskać więcej informacji zobacz "Wyrażenia regularnego" w dalszej części tego dokumentu.

## <a name="grouping"></a>Grupowanie

Grouping element definiuje ogólny układ panelu. Działa jako kontener, który umożliwia grupowanie pojedynczych formantów w różnych sekcji i karty. Oto schemat XSD dla elementu grupowania:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Istnieją trzy typy **grupowania**:

1.  **Grupowanie nagłówka**: A nagłówka grupowania jest opcjonalna. Może istnieć tylko jedno grupowanie nagłówka w **panelu**. Grupowanie nagłówka jest wyświetlana na panelu jako podpis.
    Tylko jeden UocCaptionControl jest może być używany w tej metodzie grupowania. Na przykład grupa nagłówek sekcji przykładowe.

2.  **Zawartości grup**: wymagany jest co najmniej jedno grupowanie zawartości. Może istnieć wiele grup zawartości w panelu. Grupowanie zawartości jest wyświetlany jako główny zawartości strony RCDC. Każda grupa zawartości jest wyświetlany jako karty na tym samym panelu i może zawierać od 1 do 256 formantów. Zobacz poniższą sekcję próbki przykład **Grupowanie zawartości**.

3.  **Podsumowanie grupy**: A podsumowanie grupowania jest opcjonalna. Może istnieć tylko jedno grupowanie podsumowanie w panelu. Grupowanie podsumowanie pojawia się jako ostatni kartę panelu. Tylko jeden **UocHtmlSummary** formant może służyć w grupowaniu podsumowanie Aby wyświetlić zmiany wprowadzone przed przesłaniem żądania. W sekcji przykładowe poniżej przykład grupowania podsumowanie.

Każdy typ grupowania zawiera następujące elementy:

1.  **Pomoc**: ten element zawiera tekst pomocy na karcie. Można również użyć aby dodać łącze do pliku pomocy dla karty.

2.  **Formanty**: Aby uzyskać informacje o tym elemencie, zobacz sekcję formantu w tym dokumencie. Każda grupa musi mieć formanty 1 do 256 włącznie, w zależności od typu grupowania.

3.  **Zdarzenia**: Aby uzyskać informacje o tym elemencie, zobacz sekcję zdarzeń w tym dokumencie. Opcjonalnie, każda grupa ma jedno zdarzenie. Zdarzenia, które są obsługiwane w elemencie grupowania są następujące:

    - **BeforeLeave**: to zdarzenie jest wyzwalane, gdy użytkownik jest gotowy do pozostaw na karcie zawartości grupy.
    - **AfterEnter**: to zdarzenie jest wyzwalane, gdy użytkownik jest gotowa do przejścia na karcie w grupowaniu zawartości.

Atrybuty:

1.  **Nazwa**: jest wymagana nazwa grupowanie. **Nazwa** muszą być unikatowe w obrębie **panelu**.

2.  **Podpis**: **podpis** pojawia się jako podpis nagłówka w grupowaniu nagłówka. Wygląda na to, jako podpis kartę zawartości lub podsumowanie grupowania.

3.  **Opis elementu**: atrybut opcjonalny ciąg **opis** działa tylko wtedy, gdy jest używana w grupowaniu zawartości. Użyj tego elementu, aby dać użytkownikowi końcowemu niektórych szczegółów na temat informacji zawartych w tej samej karcie.

  >[!NOTE]
  Czy ten atrybut jest używany w grupowaniu podsumowanie, XML jest uznawane za nieprawidłowe. Czy ten atrybut jest używany w grupowaniu nagłówka, XML jest uznawane za prawidłowe, ale została zignorowana.

4.  **Włączone**: opcjonalny logiczny atrybut, włączone ma ustawioną wartość true, gdy jest on niedostępny. Jeśli włączone jest ustawiona na wartość false, użytkownik końcowy widzi na karcie wyłączone. Ten atrybut będzie działać tylko w przypadku grupowania zawartości.

  >[!NOTE]
  Czy ten atrybut jest używany w grupowaniu podsumowanie, XML jest uznawane za nieprawidłowe. Czy ten atrybut jest używany w grupowaniu nagłówka, XML jest uznawane za prawidłowe, ale została zignorowana.

5.  **Widoczne**: można ukryć kartę RCDC lub jej nagłówek, przez ustawienie wartości false dla tego atrybutu. Domyślnie ten atrybut opcjonalne, wartość logiczna typu jest ustawiony na wartość true. Ten atrybut jest używana tylko na grupowanie zawartości.

  >[!NOTE]
  Jeśli istnieje tylko jedno grupowanie zawartości w panelu, ta funkcja nie działa. Istnieje więcej niż jedno grupowanie zawartości w panelu, zachowuje się jak opisano powyżej.

6.  **IsHeader**: ten atrybut jest opcjonalne, wartość logiczna atrybut, który określa, czy grupowania jest grupowanie nagłówka. Jeśli ten atrybut nie jest określony, jest ustawiona na wartość false.

7.  **IsSummary**: jest to opcjonalne, wartość logiczna atrybut, który określa, czy grupowania jest grupowanie podsumowania. Jeśli ten atrybut nie jest określony, jest ustawiona na wartość false.

![Plik XML konfiguracji RCD](media/rcd-configuration-xml-reference/image005.jpg)

Następujący przykładowy kod XML generuje poprzedniej grupowania nagłówka. Grupowanie nagłówka jest obszar tekstem przykładowy nagłówek grupowania.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![Plik XML konfiguracji RCD](media\rcd-configuration-xml-reference/image007.jpg)

Następujący przykładowy kod XML generuje poprzedniej Grupowanie zawartości. Grupowanie zawartości jest karcie po lewej stronie z tekstem **grupowania zawartości próbki**.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![Plik XML konfiguracji RCD](media/rcd-configuration-xml-reference/image010.jpg)

Następujący przykładowy kod XML generuje poprzedniej grupowania podsumowanie. Grupowanie podsumowanie jest karcie po prawej stronie tekstem **Podsumowanie**.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>Pomoc

Element pomocy Opcjonalnie można dołączyć do grupowania lub element formantu. Jeśli jest on używany w metodzie grupowania, należy pierwszy element używany. Zapewnia on tekstową pomocy dla użytkowników końcowych, aby ułatwić im dokładnych informacji. Oto schemat XSD dla elementu pomocy:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
Poniżej przedstawiono przykład elementu pomocy:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Kontrola

Grouping element zawiera jeden lub więcej elementów formantu. Formanty są główne elementy w konfiguracji RCDC. Grouping element można dostosować, definiując różnych elementów kontroli, które zawiera. Oto schemat XSD dla elementu sterowania:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Formant zawiera następujące elementy:

1.  **Pomoc**: ten element jest ignorowana. Działa tylko w metodzie grupowania.

2.  **Właściwości niestandardowe**: ten element nie jest obsługiwany.

3.  **Opcje**: ten element jest używany tylko w połączeniu z **UocDropDownList** lub **UocRadioButtonList** kontrolki. Nie jest funkcjonalny i inne formanty. W sekcji Opcje w tym dokumencie dla struktury tego elementu. Zobacz poszczególnych kontrolek, aby zobaczyć, jak jest używany w kontekście formantu.

4.  **Przyciski**: ten element jest używany tylko w połączeniu z **UocListView** formantu. Nie jest funkcjonalności dla innych formantów. Aby uzyskać więcej informacji zobacz sekcję UocListView w tym dokumencie.

5.  Właściwości: Ten element służy w wszystkie formanty do określania dodatkowe zachowania formantu. Aby uzyskać informacje o tym elemencie zobacz sekcję właściwości, w tym dokumencie.

6.  **Zdarzenia**: dla struktury tego elementu, zobacz sekcję zdarzenia we wcześniejszej części tego dokumentu. Zobacz poszczególnych definicji formantu, aby ustalić zdarzenie (event) służy w tym formancie.

Formant zawiera następujące atrybuty:

1.  **Nazwa**: jest to nazwa formantu. Nazwa formantu musi być unikatowa w ramach każdego panelu. Jest to wymagane, atrybut typu ciąg.

2.  **Właściwość TypeName**: ten atrybut określa typ formantu jest. Jest to wymagane, atrybut typu ciąg. Zobacz sekcję pojedynczych formantów w tym dokumencie dla każdej nazwy formantu.

3.  **Podpis**: można użyć tego atrybutu, aby uwzględnić podpis dla formantu.
    Podpis jest zwykle nazwę wyświetlaną danych Wyświetlanie lub wprowadzanie formantu. Można jawnie określić wartość dla podpisu lub powiązać go z informacji o schemacie atrybutu wyświetlaną nazwę. Podpis jest wyświetlany po lewej stronie formant o rozmiarze normalny. Jeśli formant obejmujące pełny ekran podpis pojawi się nad formantem. Jest to opcjonalne, atrybut typu ciąg. Aby uzyskać informacje o tym, jak można powiązać źródła danych z atrybutu lub wartość właściwości zobacz sekcję właściwości.

   W poniższym przykładzie pokazano, jak podpis można jawnie:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     Poniższy przykład Pokaż podpis możliwości korzystania ze źródłem danych. Jeśli używasz szablonu dla źródła danych opisane wcześniej w tym dokumencie, źródła danych jest schematu. Firma Microsoft zaleca, aby powiązać DisplayName atrybutu z atrybutem podpisu.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Włączone: Jest to opcjonalne, wartość logiczna typu atrybutu. Ustawiając tę wartość atrybutu na wartość false, użytkownik może wyłączyć formantu. Wartość domyślna jest ustawiana na wartość true.

5.  Widoczny: Jest to opcjonalne, wartość logiczna typu atrybutu. Ten atrybut umożliwia ukrywanie całego kontroli. Wartość domyślna jest ustawiana na wartość true.

6.  Opis: Użyj tego opcjonalne, atrybutu typu ciąg, aby uwzględnić opis, który pomoże zrozumieć, co powinny one umieszczone w formancie lub czego formantu użytkownika końcowego. Można jawnie określ wartość jako opis lub powiązać go z informacjami o opis atrybutu schematu. <br/>Opis jest wyświetlany po stronie po lewej stronie formantu o rozmiarze normalny poniżej podpis. Jeśli formant obejmujące pełny ekran opis jest wyświetlany w górnej części kontroli poniżej podpis. Aby uzyskać informacje o tym, jak można powiązać źródła danych z atrybutu lub wartość właściwości zobacz sekcję właściwości, w tym dokumencie.

7.  W poniższym przykładzie pokazano, jak opis można jawnie:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  W tym przykładzie pokazano, jak można użyć opis ze źródłem danych. Jeśli używasz szablonu dla źródła danych opisane wcześniej w tym dokumencie, źródło danych jest **schematu**. Firma Microsoft zaleca, aby powiązać ten atrybut **opis** z atrybutem opis.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: Ten atrybut wskazuje, czy formant obejmuje pełny ekran. Jest to opcjonalne, wartość logiczna typu atrybutu. Wartość domyślna ma ustawioną wartość false.

    >[!NOTE]
    Atrybuty tytuł i opis są wyłączone, gdy ten atrybut jest ustawiony na wartość true. UocLabel musi formant do zapewnienia podpis rozszerzonej kontroli.
9. **Wskazówka**: jest to opcjonalne, atrybut typu ciąg. Tekst w atrybut wskazówki pomaga określić, jaka jest prawidłowe wartości wejściowe formantu użytkownika końcowego. Wskazówka zostanie wyświetlone poniżej formantu.

10.  **AutoPostback**: jest to opcjonalne, wartość logiczna typu atrybutu. Wartość domyślna to false. Jeśli ma wartość false, Odśwież stronę, może nie Odśwież kontrolki. Informacje o AutoPostback można znaleźć właściwości formantu Microsoft ASP.NET UI o takiej samej nazwie.

11. **RightsLevel**: jest to opcjonalne, atrybut typu ciąg. Możesz powiązać ten atrybut tylko z uprawnieniami wbudowanego ze źródłem danych. Formant dynamicznie jest włączona lub wyłączona, zależny od praw użytkownika. Aby uzyskać informacje o tym, jak można powiązać źródła danych z atrybutu lub wartość właściwości zobacz sekcję właściwości w tym dokumencie.

    W tym przykładzie pokazano sposób **RightsLevel** atrybut może być używany ze źródłem danych. Jeśli używasz szablonu dla źródła danych opisane wcześniej w tym dokumencie, źródło danych jest **prawa**. Użyj nazwy atrybutu jako ścieżka.

### <a name="properties"></a>Właściwości

Właściwość umożliwia dostosować zachowanie każdego formantu. Właściwość jest elementem pustym. Oto schemat XSD dla elementu właściwości:
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



Dla każdej właściwości ma dwa wymaganych atrybutów:

1.  **Nazwa**: ten typ ciągu atrybut jest unikatową nazwę właściwości.
    Inne formanty ma inne właściwości. Istnieje kilka wspólnych właściwości, które mogą być używane przez wszystkie formanty. Aby uzyskać więcej informacji na temat nazw, które są dostępne dla danego formantu Zobacz wspólne właściwości i sekcję pojedynczych formantów.

2.  **Wartość**: jest to wartość właściwości. Typ danych zależności wartość, w którym właściwość jest przypisany do. Zobacz sekcję poniżej formatu dopuszczalną wartość dla określonej właściwości.

Niektóre właściwości mogą być powiązane z informacjami ze źródła danych. Aby to zrobić, należy użyć następującego formatu ciągu. Zobacz poszczególnych właściwości w sekcji pojedynczych formantów, aby dowiedzieć się, jak powiązać ze źródłem danych.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**PRZYKŁAD:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Wspólne właściwości
-----------------

Wszystkie formanty RCDC, które są określone w tym dokumencie może mieć następujące wspólne właściwości. Można użyć tych właściwości, oraz inne właściwości, które są specyficzne dla inne formanty.

1.  Wymagane: Ta właściwość wskazuje, że pole jest polem wymaganym lub pole opcjonalne. Wymagane pole musi pochodzić z wartością. Pusta wartość nie jest obsługiwana w przypadku ciąg na wejściu. To pole opcjonalne może być puste. Jeśli to pole jest polem wymaganym bez wartości wypełnione, komunikat o błędzie pojawia się na górze kontrolki wprowadzania. Można jawnie określić, czy pole jest wymagane lub opcjonalne. Może także powiązać pole z informacji o schemacie danego powiązania między atrybutem, a typ zasobu. Domyślnie jeśli ta właściwość nie istnieje, oznacza to, że formant jest opcjonalny kontrolki wprowadzania.

   Oto przykład, w którym używa jawną wartość dla tej właściwości:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Jest to przykład, który używa źródła danych dynamicznych dla tej właściwości. Jeśli używasz szablonu dla źródła danych, wyświetlone w poprzedniej sekcji tego dokumentu, źródła danych jest schematu. Użyj \<nazwa atrybutu\>. Wymagane, jako ścieżkę.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **Tylko do odczytu**: przez ustawienie dla tej właściwości wartość PRAWDA, użytkownik końcowy napotyka formantu w trybie tylko do odczytu. Jest to opcjonalne, wartość logiczna typu atrybutu.
    Wartość domyślna ma ustawioną wartość false. Jednak czasami zachowanie tej właściwości jest zastępowany przez typ uprawnień, które dany użytkownik ma na tworzenia powiązań danych z formantem. Na przykład jeśli użytkownik nie ma uprawnienia do aktualizacji pola, a pole jest powiązane z wbudowanym uprawnień, użytkownik widzi danych w trybie tylko do odczytu nawet ta właściwość jest ustawiona na wartość false.

3.  **Wyrażenia regularnego**: Ta właściwość określa ograniczenia, które są narzucone na wartość w formancie. Formaty wartość tej właściwości są formatów, które są obsługiwane w .NET StringRegex standard. Aby uzyskać więcej informacji, zobacz [wyrażeń regularnych programu .NET Framework](http://go.microsoft.com/fwlink/?LinkId=165361). Jeśli formant jest używany jako danych wejściowych wartość, wartość jest porównywany ograniczeń, które jest określone w tej właściwości po atempts użytkownika, aby pozostawić bieżącej strony.
    Komunikat o błędzie pojawia się u góry kontrolkę, która ma nieprawidłowe dane wejściowe. Użytkownik jawnie określić wyrażenie regularne ciągu. Użytkownik może także powiązać go z informacji o schemacie danego atrybutu. Domyślnie jeśli ta właściwość nie istnieje, oznacza to, że kontrolka nie sprawdza ograniczenia na parametry wejściowe.
    Oto przykład, w którym używa jawną wartość dla tej właściwości:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Jest to przykład, który używa źródła danych dynamicznych dla tej właściwości. Jeśli używasz szablonu dla źródła danych, przedstawione wcześniej w tym dokumencie, źródła danych jest schematu. Użyj <attribute name>. StringRegex jako ścieżka.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Widoczny: Jest to opcjonalne, wartość logiczna typu atrybutu. Ten atrybut umożliwia ukrywanie całego kontroli. Wartość domyślna jest ustawiana na wartość true.

### <a name="options"></a>Opcje

Element Opcje zawiera jeden lub więcej opcji węzły podrzędne. Element Opcje jest używany tylko z formantami UocRadioButtonList i UocDropDownList. Aby uzyskać więcej informacji o sposobie korzystania z nich zobacz sekcję poszczególnych kontrolek. Oto schemat XSD dla elementu Opcje:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


Atrybuty:

1.  Wartość: Jest wymagany atrybut typu string. Wartość atrybutu muszą być unikatowe w tej samej kontrolki. Tylko A do Z i bez uwzględniania wielkości liter znaki mogą być używane.

2.  Podpis: Wymaganego atrybutu jest wyświetlana nazwa każdej opcji.

3.  Wskazówka: Jest to atrybut opcjonalny. Aby zapewnić więcej informacji oraz wskazówki dla użytkownika końcowego, należy użyć tego atrybutu.

### <a name="environment-variables"></a>Zmienne środowiskowe

Zmienne środowiskowe w poniższej tabeli można użyć w żadnej konfiguracji RCDC.

| Zmienna | Opis elementu |
|--------|--------|
| `<LoginID>`       | Wyświetla identyfikator użytkownika, który jest aktualnie zalogowany.           |
| `<LoginDomain>`   | Wyświetla domenę użytkownika, który jest aktualnie zalogowany.       |
| `<Today>  `       | Wyświetla bieżącą datę i godzinę                                |
| `<FromToday_nnn>` | Wyświetla bieżącą datę oraz nnn i czasu. nnn jest liczbą całkowitą.  |
| `<ObjectID> `     | Identyfikator RCDC podstawowy zasób.                                     |
| `<Attribute_xxx>` | Zwraca wartość określonego atrybutu xxx RCDC zasobu podstawowego. |

### <a name="debugging-xml-configuration-files"></a>Pliki konfiguracji XML debugowania


Podczas tworzenia lub modyfikowania pliki XML konfiguracji dla konfiguracji RCDC, może pomóc zmniejszyć liczbę błędów, sprawdzając poprawność XML przed pliki XSD, użyj edytora, takiego jak Microsoft Visual Studio®. Aby uzyskać więcej informacji, zobacz [wprowadzenie do narzędzia XML w Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512).

### <a name="customizing-a-help-file"></a>Dostosowywanie pliku pomocy

Jeśli utworzysz nowe zasoby i atrybuty, można zaktualizować istniejące pliki pomocy w portalu programu FIM z zawartością dla niestandardowych zasobów. Pliki pomocy w portalu programu FIM są w formacie htm i mogą być edytowane ręcznie.

>[!IMPORTANT]
Aby uzyskać więcej informacji na temat tworzenia niestandardowych atrybutów Zobacz wprowadzenie do zarządzania atrybutu i zasobów niestandardowych w dokumentacji programu FIM 2010.

>[!IMPORTANT]
W tej sekcji nie ma informacji dotyczących podstaw formatowanie lub edycji HTML. Modyfikowanie plików pomocy już należy zapoznać się z edycji HTML


**Lokalizacja plików Pomocy**: wszystkie pliki pomocy dla portalu programu Microsoft Identity Management 2016 SP1 znajdują się w następującym folderze na serwerze usługi MIM:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Jak można znaleźć odpowiedniego pliku Pomocy**: wszystkie pliki pomocy portalu FIM są nazywane Unikatowy identyfikator globalny (GUID). Aby znaleźć poprawnego pliku niestandardowego zasobu:

1.  W portalu programu FIM Otwórz plik pomocy na stronie portalu, który chcesz dostosować.

2.  Kliknij prawym przyciskiem myszy plik pomocy, a następnie kliknij przycisk **właściwości**.

3.  Wyróżnij i skopiuj `<GUID\>.htm` pliku w polu "Adres URL".

4.  Przejdź do folderu, w którym są przechowywane pliki pomocy i wyszukaj plik.

**Dodawanie zawartości do nowego atrybutu**: Aby dodać zawartość opisowe dla nowego atrybutu w ramach istniejącego elementu grupowania (tab):

1.  Zidentyfikować i odnaleźć odpowiedniego pliku pomocy.

2.  Za pomocą edytora HTML, otwórz plik.

3.  Zlokalizuj, której chcesz dodać zawartość. Zazwyczaj jest to w dodatkowych akapitu, na przykład:

`<p xmlns="">A new paragraph with customized information.</p>`

Może to być również element wstawiony do istniejącej listy, na przykład:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Dodawanie zawartości do nowego elementu grupowania**: większość strony portalu programu FIM jest wiele grupowania elementów (lub karty) i towarzyszące pomocy zakładki pliki sekcje, które odnoszą się do każdego elementu grupowania. W sekcji określono zakładek w kodzie HTML. Na przykład jest HTML dla karty informacje o pracy z pliku pomocy na stronie Tworzenie użytkownika w portalu programu FIM:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Odwołuje się Grouping element **WorkInfo** w pliku konfiguracji XML danych **Konfiguracja tworzenia użytkownika** RCDC. Należy pamiętać, że `\<GUID\>.htm` nazwę pliku i zakładki są określone w mojej: parametr łącza:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Przykłady prostego formantu**

Na poniższej ilustracji przedstawiono przykładowe formantów różnych proste pole tekstowe w różnych trybach.

Przykład:

![](media/rcd-configuration-xml-reference/image016.gif)

Następujący segment kodu tworzy pierwszą kontrolkę pola tekstowego używa jawnego tekstu dla wszystkich atrybutów i właściwości:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


Następujący segment kodu tworzy drugi formant pola tekstowego technik wiązania dynamicznego jest używany do łączenia formantu z innego źródła danych:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


Następujący segment kodu tworzy trzeci rozwinięte etykiety i pola tekstowego kontrolki:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

Następujący segment kodu tworzy czwarty formant wyłączone pole tekstowe.
Mimo że ten formant nie zostanie wyświetlone widoczne różnica pomiędzy stan wyłączenia, a stan włączony, użytkownik nie będzie można wprowadzić danych w polu tekstowym.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>Pojedynczych formantów

### <a name="uocbutton"></a>UocButton

**Nazwa**: UocButton

**Opis elementu**: to jest formant uproszczony przycisk, który służy do wyzwalania określonych akcji. Jednak ponieważ nie można określić własne obsługi użycie tego formantu jest ograniczone.

**Właściwości**:

1.  **Wspólne właściwości wszystkich**: uzyskać informacji o tej właściwości, zobacz sekcję wspólne właściwości tego dokumentu.

2.  **Tekst**: Ta właściwość określa tekst wyświetlany na przycisku. Jest to opcjonalne, atrybut typu ciąg. Tekst przyjmie wartość ciągu jawnego.

Zdarzenie:

   • **OnButtonClicked**: zdarzenie jest emitowany po kliknięciu przycisku.

Przykład:

![](media/rcd-configuration-xml-reference/image017.png)


Następujący segment XML tworzy przycisk proste:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Nazwa**: UocCaptionControl

**Opis elementu**: ten formant jest używana do wyświetlania podpis RCDC strony. Ten formant zaprojektowano w celu można użyć tylko jako pojedynczego formantu w grupowaniu nagłówka.
Używania go w innym kontekście może spowodować problemy z renderowaniem lub błędy portalu.

**Tryb**: tylko odczyt (OneWay)

**Właściwości:**

1.  **Wspólne właściwości wszystkich:** uzyskać więcej informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  **MaxHeight:** ta właściwość określa maksymalną wysokość ikony w sekcji podpisu. Ta właściwość jest opcjonalna. Ta właściwość ma wartość w pikselach. Wartość domyślna to 32 piksele.

Przykład:

![](media/rcd-configuration-xml-reference/image020.jpg)

Generuje następujący segment kodu **podpis nagłówka**:
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


Generuje ten segment kodu **jawne podpis zawartości:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

Generuje następujący segment kodu **Nazwa wyświetlana** podpisu dynamicznego:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Nazwa**: UocCheckBox

Opis: Jest to prosty pole wyboru. Zaleca się, że użytkownik powiązanie tego formantu z danymi typu Boolean. Ten formant może służyć jako formant tylko do odczytu lub formant nadaje się do aktualizacji, na podstawie danych tworzy ona powiązanie.

>[!NOTE]
W tej wersji, gdy pole wyboru kontroli w trybie edycji, aby wyświetlić atrybut logiczny, jeśli ten atrybut nie ma wartości uprzednio przypisane do niego, kontrola zasobów dodaje wartość **false** z atrybutem podczas **OK** zostanie kliknięty w trybie edycji. Praca około ma zawsze twórz logiczny atrybut, który przyjmuje z systemem innym niż istnienia jest taka sama jak **false**, lub użyj inne formanty, takie jak przycisku radiowego dla atrybutów Boolean.

**Właściwości**:

1.  **Wspólne właściwości wszystkich**: Aby uzyskać więcej informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  **DefaultValue**: jest to opcjonalne, wartość logiczna typu właściwości. Wartość domyślna ma ustawioną wartość false. To pole określa domyślne zachowanie pola wyboru.
    Może to być określone jawnie.

3.  **Zaznaczone**: jest to opcjonalne, wartość logiczna typu właściwości. Wartość domyślna ma ustawioną wartość false. Ta wartość zastępuje właściwość DefaultValue, gdy nie jest obecny oraz DefaultValue. To pole określa zachowanie pola wyboru. Jak DefaultValue może to być określone jawnie lub powiązany z danymi z serwera.

4.  **Tekst**: jest to opcjonalne, atrybut typu ciąg. Tekst jest wyświetlany po prawej stronie pola wyboru. Ta właściwość służy do Podaj tekst, który zawiera więcej informacji dla użytkownika końcowego.

**Zdarzenia**:

   • CheckedChanged: po zmianie stanu pola wyboru jest emitowany tego zdarzenia.

W poniższym przykładzie utworzono niestandardowego powiązania między typem zasobu niestandardowego i atrybut **IsConfigurationType**. Kod XML jest używany w konfiguracji RCDC typu zasobu niestandardowego.

Przykład:

![](media/rcd-configuration-xml-reference/image022.png)

Poniższy kod tworzy segmentu **dynamiczne pole wyboru**, jak pokazano na poprzedniej ilustracji jako dynamiczny pole wyboru. Ten typ powiązania jest zazwyczaj bardziej elastyczne i użyteczne niż jawne pole wyboru. Ten atrybut musi należeć do bieżącego typu zasobu.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Opis elementu**: to jest formant wielowierszowe pole tekstowe, który obsługuje specjalne ciągu formatowania. Każda wartość między wielowartościowego wpisów jest oddzielona od siebie średnika; lub podział wiersza w polu tekstowym. Zaleca się, że ten formant związane z danymi wielowartościowego, krótki typy string i integer. Ten formant obsługuje zarówno w trybie tylko do odczytu, jak i w trybie nadaje się do aktualizacji.

**Właściwości**:

1.  **Wspólne właściwości wszystkich**: Aby uzyskać więcej informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  **Typ danych**: jest to wymagane, atrybut typu ciąg. Można określić jako **String, Integer**, lub **DateTime** jawnie typu. Może także powiązać atrybutu z atrybutem schematu **DataType** właściwości. Typ referencyjny wielowartościowego powinno zostać obsłużone przez **UOCListView** lub **UOCIdentityPicker**. Wielowartościowy Boolean nie jest obsługiwany typ danych.

3.  **Wiersze**: jest to opcjonalne, atrybut typu Liczba całkowita. Wysokość pola można zdefiniować w liczbie znaków. Wartość domyślna wynosi 1.

4.  **Kolumny**: jest to opcjonalne, atrybut typu Liczba całkowita. Można określić, ile całego pole ma wiele znaków. Wartość domyślna jest równa
    20.

5.  **Wartość**: jest to opcjonalne, atrybut typu ciąg. Możesz powiązać ten atrybut tylko ze źródłem danych.

Zdarzenia:

   • **ValueListChanged**: to zdarzenie jest wyzwalane, gdy bieżąca wartość w formancie zostanie zmieniona.

W poniższym przykładzie o nazwie atrybutu wielowartościowego ciąg **AMultiValueString** jest tworzony i powiązane z typem zasobu niestandardowego. W tym przykładzie działa tylko po utworzeniu tego powiązania.

**Przykład:**

![](media/rcd-configuration-xml-reference/image024.jpg)

Następujący segment kodu generuje powyższej ilustracji:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**Nazwa**: UocDateTimeControl

**Opis**: jest to podobne do formantu pola tekstowego, ale **opis** akceptuje tylko określonym formacie. W trybie tylko do odczytu wygląda na to, jak etykieta. Format ciągu wejściowego, która jest obsługiwana dla **DateTimeFormat** właściwości w tej sekcji.

**Właściwości**:

1.  **Wspólne właściwości wszystkich**: Aby uzyskać więcej informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  **DateTimeFormat**: jest to opcjonalne, atrybut typu ciąg. Obsługiwane formaty to daty/godziny lub DateOnly. Wartość domyślna wynosi formacie daty/godziny.
    a. Format daty/godziny: ten atrybut jest formatowana jako mm/dd/rrrr gg: mm: ss AM.

      <[!NOTE]
      Zarówno **DateTime** i **DateOnly** formatu są obsługiwane, niezależnie od użytkownika, który jest określenie różnicy.
3.  **Wartość**: jest to opcjonalne, atrybut typu ciąg. Możesz powiązać ten atrybut ze źródłem danych zasobów. Wartość tego atrybutu musi być zgodna z formatem prawidłowe daty/godziny.

Zdarzenia:

   • **DateTimeChanged**: podczas zmiany wartości daty/godziny, wystąpi zdarzenie.

Przykład:

![](media/rcd-configuration-xml-reference/image027.jpg)

Następujący segment kodu tworzy pierwszy **DateTime** formantu.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


Następujący segment kodu tworzy drugi **DateTime** formantu. Jeśli używasz w przykładowym kodzie w sekcji źródeł danych **ExpirationTime** atrybut jest powiązany z wszystkich typów zasobów. W związku z tym można jej używać z następującym kodem:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**Nazwa**: UocDropDownList

Opis: Jest to formant proste pola listy rozwijanej. Ten formant używa się zazwyczaj, gdy chcesz wybrać opcje podstawie zdefiniowanego zestawu opcji. Typy danych w ciągu, integer, datetime i Boolean nadaje się do tego formantu.

**Właściwości**:

1.  **Wspólne właściwości wszystkich**: Aby uzyskać więcej informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  **ValuePath**: właściwość można odczytać atrybutu Value ItemSource. W przypadku ItemSource jako niestandardowy ścieżki wartość ma ustawioną wartość. Jest on powiązany z pola wartości z elementu opcja, która jest zdefiniowana w dalszej części tego dokumentu.

3.  **CaptionPath**: właściwość można odczytać atrybutu Value ItemSource. ItemSource jest określony jako niestandardowy, ścieżka wartość jest równa podpis. Jest on powiązany z polem podpisu z elementu opcja, która jest zdefiniowana w dalszej części tego dokumentu.

4.  **HintPath**: właściwość można odczytać atrybutu Value ItemSource. ItemSource jest określony jako niestandardowy, ścieżka wartość jest równa wskazówki. Jest on powiązany z polem wskazówkę z elementu opcja, która jest zdefiniowana w dalszej części tego dokumentu.

5.  **ItemSource**: kolekcja ListControlItems, który definiuje opcji na liście. Użytkownik może jawnie ustaw tę wartość na niestandardowe i użyj elementu opcji, aby określić wartość ciągu.

6.  **SelectedValue**: wartość, która jest aktualnie wybrany. Jest to wymagane, właściwość typu string. Ta właściwość jest powiązana z danymi ciąg ze źródła danych.

Zdarzenia:

  • SelectedIndexChanged: zdarzenie występuje, gdy zmieni się zaznaczenie w polu listy rozwijanej.

Opcje:

Dla struktury element opcji zobacz sekcję "Opcji" w tym dokumencie.

1.  **Wartość**: wartość określonego elementu opcji można podać dowolny ciąg prawidłowe wartości wejściowe źródła danych, która kontrolka jest powiązana z.

2.  **Podpis**: podpis może być dowolną wartością ciągu.

3.  **Wskazówka**: wskazówki może być dowolną wartością ciągu.

Przykład:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Aby przykładowe działa, możesz powiązać istniejący, atrybut typu ciąg **zakres** o typie zasobu niestandardowego RCDC dotyczy.


Ten segment kodu generuje listy rozwijanej:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

Nazwa: UocFileDownload

Opis: Ten formant zawiera hiperłącza. Po kliknięciu hiperłącza, zostanie wyświetlona strona Windows Zapisz plik. Użytkownik może zapisać plik na dysku lokalnym.
Otwórz opcję jest również obsługiwany, jeśli program Internet Explorer można renderować format pliku. Typy danych zalecane, aby użyć tej kontrolki z są sformatowane ciąg (XML) i typy binarnego.

>[!NOTE]
W tej wersji programu Microsoft Identity Manager 2016 z dodatkiem SP1 użytkownik musi zamknąć okna programu Internet Explorer, w którym użytkownik otworzyć plik, a następnie odśwież stronę. Po odświeżeniu okno programu Internet Explorer, użytkownik może uruchomić pobieranie, aby zapisać lub ponownie otworzyć tego samego pliku w oknie oryginalnego.


Właściwości:

1.  **Wspólne właściwości wszystkich**: Aby uzyskać więcej informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  **Tekst**: to jest opcjonalny, atrybut typu ciąg, który definiuje tekst hiperłącza. Użytkownik może określić jawne parametry dla tej właściwości.

3.  **Wartość**: jest wymagany atrybut. Określa powiązanie atrybutu na serwerze, którego zawartość ma zostać pobrane.

4.  **PromptedFileName**: jest to opcjonalne, atrybut typu ciąg. Jest to nazwa pliku, sugerowanej użytkownikowi zapisanie pobranego pliku.

5.  **ContentType**: jest to wymagane, atrybut typu ciąg. Jest to typ pliku, którego dane są zapisywane w. Tekst lub binarny są dwie opcje obsługiwany ciąg. Jeśli tekst, wartość zwracana jest traktowane jako ciąg.
    W przeciwnym razie dla pliku binarnego, zwracana wartość jest uznawany za byte []. Zaznaczenie tekstu użytkownika można opcjonalnie Dodaj sufiks, aby określić typ formatu tekstu to w. Na przykład tekstu/xml jest nieprawidłowy.


>[!NOTE]
Jeśli wartość, którą jest powiązany ten formant jest puste, formantu brakuje hiperłącza do posłużyć do wyzwalania akcję pobierania. Jest tak, ponieważ nie ma nic do pobrania.


**Zdarzenia**:

Brak zdarzeń dla tego formantu.

**Przykład**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Przed przekazaniem tego przykładowego pliku, użytkownik musi utworzyć powiązania między typem zasobu niestandardowego i istniejący atrybut ConfigurationData.


Następujący segment kodu generuje formant pobierania plików na poprzedniej ilustracji:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Nazwa**: UocFileUpload

**Opis elementu**: ten formant zawiera pole tekstowe, który wyświetla lokalizację pliku lokalnego do przekazania, przycisk Przeglądaj plik i przycisk Prześlij. Gdy użytkownik kliknie przycisk przeglądania, zostanie wyświetlone okno Otwórz plik systemu Windows. Użytkownik końcowy może wybrać jeden plik na dysku lokalnym do przekazania. Po wybraniu pliku lokalizacji pliku jest wyświetlana w polu tekstowym. Po kliknięciu przycisku przekazywania, plik zostanie przekazany do źródła danych lokalnych po stronie klienta. Zawartość pliku nie jest jeszcze przesłane do serwera. Typy danych zalecane, aby użyć tej kontrolki z są następujące: sformatowany ciąg (XML) lub binarne typów.

>[!NOTE]
Nie są oznaki postępu przekazywania lub stanu. Gdy plik jest przekazywany do lokalnego źródła danych, pola tekstowego jest wyczyszczone.


Właściwości:

1.  Wszystkie wspólne właściwości: Informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  Wartość: Jest wymagany atrybut. Określa powiązanie ze schematem atrybutu na serwerze przekazane dane.

3.  ContentType: Jest to opcjonalne, atrybut typu ciąg. Jest to typ danych, plik jest zapisywana na serwerze. To można ustawić na tekst lub binarny. Jeśli brakuje właściwości, wartością domyślną jest plikiem binarnym.

4.  MaxFileSize: Jest to opcjonalne, atrybut typu ciąg. MaxFileSize definiuje, jak duży może być rozmiar przesłanego pliku. Domyślnie jeśli brakuje właściwości maksymalny rozmiar to 1 megabajt (MB).

5.  PromptedForNoValue: Jest to opcjonalne, atrybut typu ciąg. Definiuje tekst wyświetlany użytkownikowi, gdy nie można przekazać pliku.

Zdarzenia:

   • FileUploaded: to zdarzenie jest emitowany po pomyślnym przekazaniu pliku.

Przykład:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
Aby następujący przykładowy kod działa, możesz utworzyć nowy atrybut typu danych binarnych o nazwie ABinaryAttribute, a następnie utwórz nowe powiązanie między typem zasobu niestandardowego i tego atrybutu.


Następujący segment kodu generuje formant przekazywania na poprzedniej ilustracji:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

Nazwa: UocFilterBuilder

Opis: Jest to złożony formant, który umożliwia użytkownikowi renderowania wyrażenie MIM 2016 XPath. Niektóre wyrażenia XPath nie są obsługiwane. Aby uzyskać informacje o sposobie używania konstruktora filtrów zobacz Pomoc dla konstruktora filtrów.

Właściwości:

1.  Wspólne właściwości wszystkich: Aby uzyskać więcej informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  PermittedObjectTypes: Definiuje listę typów zasobów, które mają być wyświetlane w instrukcji select konstruktora filtru. Aby uzyskać informacje dotyczące używania konstruktora filtrów, zobacz Pomoc konstruktora filtrów. Ten ciąg jest w formacie ResourceTypeA, ResourceTypeB, w którym każdy typ zasobu jest rozdzielonych przecinkami (,).

3.  Wartość: Jest to wartość, z którą jest renderowany konstruktora filtrów.
    Jest obsługiwane tylko powiązania z danymi typu ciąg, który zawiera wyrażenie XPath. Atrybut filtru nie jest atrybutem zalecane dla powiązania tego formantu.

4.  PreviewButtonVisible: Jest to opcjonalne, wartość logiczna typu właściwości. Gdy ta właściwość jest ustawiona na wartość false, użytkownik nie ma przycisku podglądu. Wartość domyślna jest ustawiana na wartość true. Ten przycisk pozwala w połączeniu z formantu widoku listy Podgląd wyników wyrażenia XPath.

5.  ExcludeGroupMembership: Jest to właściwości typu Boolean. Jeśli ta właściwość ma wartość PRAWDA, nie można utworzyć na filtr, który używa \<atrybut odwołania\> (na przykład ResourceID) jest członkiem \<obiektu grupy\>. Innymi słowy, gdy ta właściwość ma wartość PRAWDA, nie można utworzyć na filtr, który korzysta z katalogu członkostwa grupy.

6.  PreviewButtonCaption: Jest to opcjonalny ciąg. Jeśli PreviewButtonVisible jest równa true, umożliwia tej właściwości przycisku Nadaj własny tekst. Tekst jest wyświetlany na przycisku podglądu.

Zdarzenia:

   • OnFilterChanged: to zdarzenie jest wyzwalane, gdy zmienia się zawartość konstruktora filtru.

Przykład:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
Przed skorzystaniem z tego przykładu kodu, Utwórz nowe powiązanie między istniejący atrybut filtru i niestandardowy typ zasobu.


Poniżej przedstawiono przykładowy kod, który zawiera formant UOCLabel, konstruktora filtr prosty z PermittedObjectTypes i widok listy w wersji zapoznawczej. Taki sam atrybut źródła danych, aby połączyć dwie musi wskazywać widok listy właściwości ListFilter i Konstruktor filtrów właściwości Value.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Nazwa: UocHtmlSummary

Opis: Do definiowania strony podsumowania na stronie RCDC można użyć tego formantu.
Strona podsumowania jest wyświetlany po użytkownik prześle żądanie. Tego formantu można używać tylko w grupowaniu podsumowanie, a musi być jedynym formantem. Stanowczo zaleca się, że używasz przykładowy kod, który został dostarczony.

>[!NOTE]
Ten formant nie została dokładnie przetestowana.


Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję właściwości wspólne tego dokumentu.

2.  ModificationsXml: Ta właściwość musi być sformatowany jako {powiązania źródła = różnicowych, ścieżka = DeltaXml}, gdzie delta jest zdefiniowany w nagłówku konfiguracji ObjectDataSource.

3.  TransformXsl: Ta właściwość jest zwykle w formacie {powiązania źródła = summaryTransformXsl, ścieżka = /}, gdzie summaryTransformXsl jest zdefiniowany w nagłówku elementu XmlDataSource konfiguracji.

Aby istniejący przykład tego formantu zobacz "Podsumowanie grupowania" w sekcji grupowania we wcześniejszej części tego dokumentu.

### <a name="uochyperlink"></a>UocHyperLink

Nazwa: UocHyperLink

Opis: Jest to prosty hyperlink formantu. Ten formant służy do wyświetlania informacji jako hiperłącze.

Właściwości:

1.  Wszystkie wspólne właściwości: Informacji, zobacz sekcję właściwości wspólne tego dokumentu.

2.  ObjectReference: Jest to właściwość opcjonalna, typu odwołania. Jeśli prawidłowy zasób odwołuje się identyfikator GUID, który jest zdefiniowany w tej właściwości, Hiperłącze umożliwia użytkownikowi dostępu do zasobu. Jest to wykluczają się wzajemnie z właściwością NavigateUrl (patrz poniżej).

3.  Tekst: Jest opcjonalne, właściwość typu string. Ta właściwość służy do definiowania tekst wyświetlany jako hiperłącze.

4.  NavigateUrl: Jest to właściwość opcjonalna, typ ciągu. Ta właściwość służy do określenia hiperłącza, który stanowi łącze do adresu URL pełnej ścieżki. Jest to wykluczają się wzajemnie z właściwością ObjectReference (powyżej).

Przykład:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
Potrzebny jest prawidłowy identyfikator GUID zasobu tutaj, aby połączyć. W takim przypadku drugi hiperłącze jest generowany z prawidłowym identyfikatorem GUID. Pierwsza z nich może być każdej witrynie sieci Web.


Następujący segment kodu generuje przekierowania hiperłącze:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

Następujący segment kodu generuje hiperłącze, które odwołuje się do zasobu. Jawnego odwołania mogą zostać zastąpione przez wyrażenie {powiązania źródła = obiektu, ścieżka = twórcy} to powiązanie ze źródłem danych. Może to być prawidłowy, tylko gdy Menedżer zasobów istnieje i jest odwołanie do typu wartości.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

Nazwa: UocIdentityPicker

Opis: Ten formant składa się z pole opcjonalne rozwiązanie i okno przeglądania. Pole opcjonalne Resolve składa się z pola tekstowego opcjonalnie wprowadzić tożsamości, przycisk Rozwiąż, aby rozwiązać tożsamości i przycisk Przeglądaj, aby monitować okno podręczne przeglądania. Przeglądaj okna umożliwia użytkownikowi wybranie tożsamości za pomocą formantu widoku listy. Wybrane tożsamości z okna przeglądania jest widoczny w polu rozwiązanie.

Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję właściwości wspólne tego dokumentu.

2.  UsageKeywords: Jest to właściwość opcjonalny ciąg. Można zdefiniować listę zakresów wyszukiwania do użycia w selektorze zasobów podając listę słów kluczowych użycia, które są obsługiwane przez strukturę SearchScopeConfiguration, gdzie każde słowo kluczowe jest oddzielona (').

3.  Filtr: Jest właściwością opcjonalny ciąg. Użytkownik udostępnia wyrażenia XPath do określania zakresu selektor zasobów do wyświetlania tylko elementów, które mieszczą się w ramach określonego zakresu. Ta właściwość jest wykluczają się wzajemnie z właściwością UsageKeywords (powyżej). Po zastosowaniu zakres wyszukiwania, ta właściwość nie ma znaczenia.

4.  ResultObjectType: Jest to właściwość opcjonalny ciąg. Typ zasobu jest używany do renderowania zasobów na liście wyskakujące okno dialogowe. To jest ułatwiają z filtrem Selektor tożsamości zidentyfikować, jakiego typu zasobu zwracanego przez filtr i odpowiednio przedstawienia tych danych. Ta właściwość jest wykluczają się wzajemnie z właściwością UsageKeywords (zobacz powyżej). Po zastosowaniu zakres wyszukiwania, to ustawienie nie działa. Ciąg, który jest akceptowany dla tej właściwości jest dowolną nazwę jednej, nieprawidłowy, typ zasobu, na przykład osoby.
    Gdy filtr powinien zwrócić wiele typów zasobów, zasobów jest używany.

5.  PreviewTitle: Jest to tytuł preview używane w widoku listy. Aby uzyskać informacje o tej właściwości zobacz sekcję UocListView.

6.  ListViewTitle: Jest to właściwość opcjonalny ciąg. Ta właściwość służy do definiowania tekst wyświetlany u góry widoku listy jako tytuł.

7.  Wartość: Jest to właściwość opcjonalny ciąg. Zaleca się powiązać to atrybutem schematu nawiązywania połączenia ze źródłem danych wartości.

8.  Tryb: Jest to właściwość opcjonalny ciąg. Ta właściwość umożliwia definiowanie czy przez Selektor tożsamości można wybrać jedną wartość, lub można wybrać wiele tożsamości. SingleResult i MultipleResult są dozwolone wartości. Domyślnie jest ustawiona na SingleResult.

9.  ObjectTypes: Jest to właściwość opcjonalna, typ ciągu. Można zdefiniować listę typów zasobów, które użytkownik końcowy może rozwiązać wpisy przed w polu rozwiązać selektora tożsamości. Lista składa się z listą nazw typu zasobu rozdzielanych przecinkami (,).

10. AttributesToSearch: Jest to opcjonalne, typeproperty ciągu. Można zdefiniować listę atrybutów, które mają służyć do rozpoznawania elementu w Selektor tożsamości, gdy lista jest listę atrybutów schematu rozdzielanych przecinkami (,). Na przykład, jeśli AttributesToSearch ma ustawioną wartość DisplayName, aliasu, oznacza to, ten użytkownik ma możliwość wyszukiwania elementów o nazwie wyświetlanej = \<wyszukiwania wartość\> lub Alias =\<wyszukiwania wartość\>. Zaleca się, że nazwy atrybutów, które mają być prawidłowe atrybuty dla typów zasobów docelowych źródła danych, który jest zlokalizowany w wartości. W polu ObjectTypes można znaleźć typów zasobów docelowych. Wszystkie atrybuty muszą być prawidłowe na żadnych typów określonych zasobów, które są wymienione w polu ObjectTypes.

11. ColumnsToDisplay: Jest to właściwość opcjonalna, typ ciągu. Użytkownik udostępnia listę nazw atrybutów schematu, rozdzielanych przecinkami (,). Atrybuty, które są zdefiniowane w tym miejscu tworzą kolumny w widoku listy w selektorze tożsamości.

12. Wiersze: Jest opcjonalne, właściwość typu integer. Działa tylko wtedy, gdy tryb jest ustawiony na MultipleResult. Ta właściwość umożliwia ustawienie wysokości Resolve pola tekstowego na dany rozmiar w jednostkach znakowych.

13. MainSearchScreenText: Jest to właściwość opcjonalna, typ ciągu. Jest to własny tekst, który pojawia się podczas wyszukiwania w oknie przeglądania.

Zdarzenia:

 • SelectedObjectChanged: to zdarzenie jest emitowany, gdy użytkownik zmieni wybranych zasobów.

Przykład:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Ten przykład działał należy utworzyć nowe powiązanie między atrybutem Manager i dowolnego typu zasobu niestandardowego, który dotyczy tego kodu XML.


Następujący segment kodu generuje Selektor tożsamości w trybie pojedynczego przy użyciu właściwości filtru i ResultObjectType jako część konfiguracji RCDC:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



Przykład:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Aby ten przykładowy kod działa, możesz powiązać atrybutu ExplicitMember (atrybutu wielowartościowego odwołania) na typ zasobu niestandardowego. Należy także utworzyć zakresów wyszukiwania z właściwością UsageKeyword o wartości do osoby i grupy.


Następujący segment kodu tworzy kontrolkę na poprzedniej ilustracji:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

Nazwa: UocLabel

Opis: Jest to prosty, tylko do odczytu, tekst etykiety formantu. Zaleca się, że tego formantu można użyć do wyświetlenia danych tylko do odczytu.

Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję właściwości wspólne tego dokumentu.

2.  Tekst: To jest atrybut typu ciąg. Tej właściwości należy zdefiniować, podając wartość ciągu jawnego lub przez jego powiązanie ze źródłem danych. Powiązanie próbki przypisującej walue tej właściwości to {powiązania źródła = obiektu, ścieżka =\<prawidłową nazwą atrybutu\>.

Dla przykładu formantu UocLabel Zobacz prostego formantu w sekcji przykładów prostego formantu.

### <a name="uoclistview"></a>UocListView

Nazwa: UocListView

Opis: Jest to formant zaawansowane widoku listy. Składa się z widoku prostej listy, opcjonalne proste wyszukiwanie kontrolkę opcjonalne wyszukiwania zaawansowanego, opcjonalny składnik Podgląd pola i paska przycisków akcji. Opcjonalne proste wyszukiwanie obejmuje zakres wyszukiwania i pola tekstowego proste wyszukiwanie. Formant wyszukiwania zaawansowanego jest konstruktor filtrów. Widok listy to lista prerendered zasobów. Można również wyświetlać wyniki wyszukiwania pochodzące z formantów wyszukiwania w tym formancie. Na pasku przycisku akcji definiuje, jakie można podjąć akcję na podstawie wybranego w widoku listy. W polu Wybór Podgląd pokazuje, jakie elementy są wybierane w widoku listy.

>[!IMPORTANT]
UocListView nie działa z atrybutów jednowartościowych odwołania. Może służyć tylko z atrybutów wielowartościowych odwołania. W przypadku atrybutów jednowartościowych odwołania Zobacz UocIdentityPicker w tym dokumencie.


Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję właściwości wspólne tego dokumentu.

2.  SelectedValue: Jest opcjonalne, właściwość typu string, zwykle powiązanej z atrybutu wielowartościowego odwołania akceptuje listy ciągów w formacie GUID.

3.  Wartość PageSize: Jest właściwością liczby całkowitej. opcjonalne. Użytkownik może określić, jak wiele wpisów zmieści się na jednej stronie formantu widoku listy. Wartość domyślna to 10 wpisów. Prawidłowy jest dowolną dodatnią liczbą całkowitą.

4.  UsageKeyword: Jest to właściwość opcjonalna, typ ciągu. Użytkownik może określić listę słów kluczowych, które określają, jakie zakres wyszukiwania jest używany w formancie wyszukiwania widoku listy. Dostępne są zasoby zakres wyszukiwania na serwerze programu FIM 2010. Atrybut na strukturę SearchScopeConfiguration, o nazwie UsageKeyword, służy do grupowania zestawu zakresów wyszukiwania. Widok listy zużywa listę słów kluczowych. Każde słowo kluczowe jest rozdzielanych przecinkami (,).
    To usagekeyword używany na odpowiedni zakres wyszukiwania mają być wyświetlane w tym widoku listy. To działa tylko wtedy, gdy właściwość ShowSearchControl ma wartość true.

5.  SearchControlAutoPostback: Jest to właściwość Opcjonalna wartość logiczna. Wartość tej właściwości wartość PRAWDA, aby wykonać autopostback po wyzwoleniu wyszukiwania. Domyślnie SearchControlAutoPostback ma wartość false.

6.  EmptyResultText: Jest to właściwość opcjonalna, typ ciągu. Domyślnie jest ustawiona na Brak elementów, ale można ustawić dowolną wartość ciągu. Ten tekst jest wyświetlany, gdy wynik wyszukiwania jest pusta.

7.  ButtonHeight: Jest to właściwość opcjonalna, typu Liczba całkowita. Wartość tej właściwości należy ustawić dowolną wartość dodatnią liczbą całkowitą. Ta właściwość określa wysokość przycisków na pasku akcji w pikselach. Wartość domyślna to 32 piksele.

8.  ButtonWidth: Jest to właściwość opcjonalna, typu Liczba całkowita. Wartość tej właściwości należy ustawić dowolną wartość dodatnią liczbą całkowitą. Ta właściwość określa szerokość przycisków na pasku akcji w pikselach. Wartość domyślna to 32 piksele.

9.  CaptionImageMaxHeight: Jest to właściwość opcjonalna, typu Liczba całkowita. Ustaw wartość tej właściwości dowolną dodatnią liczbą całkowitą. Ta właściwość określa wysokość maksymalna ikona opcjonalnym podpisem. Wartość domyślna to 32 piksele.

10. CaptionImageMaxWidth: Jest to właściwość opcjonalna, typu Liczba całkowita. Ustaw wartość tej właściwości dowolną dodatnią liczbą całkowitą. Ta właściwość określa szerokość ikon maksymalną opcjonalnym podpisem. Wartość domyślna to 32 piksele.

11. CaptionImageUrl: Jest to właściwość opcjonalna, typ ciągu. Ta właściwość określa adres URL prowadzący do obrazu, który jest wyświetlany jako obraz podpisu.

12. PreviewTitle: Jest to właściwość opcjonalna, typ ciągu. Ta właściwość służy do definiowania tekst wyświetlany u góry pola wyboru wersji zapoznawczej.

13. EnableSelection: Jest to opcjonalne, wartość logiczna typu właściwości. Ta właściwość umożliwia definiowanie, czy widok listy jest w trybie wyboru. Jeśli widok listy jest w trybie wyboru, kolumna pól wyboru pojawi się w lewej kolumnie widoku listy i w dolnej części widoku listy zostanie wyświetlone okno podglądu z wyboru. Wartość domyślna tej właściwości jest ustawiana na wartość true.

14. SingleSelection: Jest to opcjonalne, wartość logiczna typu właściwości. Jeśli włączono tryb zaznaczania dla widoku listy, ustawienie wartości true ogranicza użytkownika końcowego można wybrać tylko jeden element z listy. Domyślnie wartość tej właściwości jest równa false. Oznacza to, że domyślnie, użytkownik końcowy może wybrać wiele elementów z listy.

15. RedirectUrl: Jest to właściwość opcjonalna, typ ciągu. Ta właściwość służy do określania strony przekierowania po kliknięciu elementu Link na liście. Ten adres URL może zawierać symbole zastępcze, które są zastępowane rzeczywistej wartości w czasie wykonywania. Symbole zastępcze są następujące:

     {0} ◦ — typu obiektu

     {1} ◦ — identyfikator obiektu

     {2} ◦ — Nazwa wyświetlana

16.  ShowTitleBar: Jest to opcjonalne, wartość logiczna typu właściwości. Ta właściwość umożliwia określenie, czy pasek tytułu powinny być widoczne. Wartość domyślna tej właściwości to false.

17.  ShowActionBar: Jest to opcjonalne, wartość logiczna typu właściwości. Ta właściwość umożliwia określenie, czy obszar paska akcji powinny być widoczne. Wartość domyślna tej właściwości jest PRAWDA.

18.  ShowPreview: Jest to opcjonalne, wartość logiczna typu właściwości. Ta właściwość umożliwia określenie, czy obszar podglądu powinny być widoczne. Wartość domyślna tej właściwości jest PRAWDA.

19.  ShowSearchControl: Jest to opcjonalne, wartość logiczna typu właściwości. Ta właściwość umożliwia określenie, czy formant wyszukiwania powinny być widoczne. Wartość domyślna tej właściwości jest PRAWDA.

20.  ResultObjectType: Jest to właściwość opcjonalna, typ ciągu. Ta właściwość umożliwia określenie typu Oczekiwano obiektu wyników wyszukiwania. Wartość domyślna tej właściwości jest zasobem. Jeśli wynik wyszukiwania zawiera wiele typów zasobów, należy podać tę wartość jako zasób.

21.  ColumnsToDisplay: Jest to właściwość opcjonalna. Ta właściwość służy do określenia atrybutów, które mają być wyświetlane jako kolumny w widoku listy. Wartość domyślna tej właściwości to nazwa wyświetlana, typu zasobu. Każda kolumna jest reprezentowany przez nazwę atrybutu systemu. Każda kolumna jest rozdzielanych przecinkami (,). Nie trzeba określić wartość dla tej właściwości, gdy widok listy jest używana w trybie wyboru. W trybie wyboru ustawienie kolumny pochodzi z atrybutu SearchScopeColumn aktualnie wybranego zakresu wyszukiwania.

22.  ListFilter: Jest to właściwość opcjonalna, typ ciągu. Jest to wyrażenie xpath, używany do renderowania widoku listy, która działa tylko wtedy, gdy właściwość ShowSearchControl ma wartość false. Gdy ta wartość jest określona, widok listy używa tej wartości właściwości dla zapytań i widok listy nie jest w trybie wyboru. Filtr albo można powiązać z atrybutem ciągu zasobu, w następujący sposób:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     lub ciąg, który zawiera niektóre zmiennej środowiskowej wstępnie zdefiniowane, w następujący sposób: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: To przestarzała właściwości. Jego wartość powinna być nazwa systemowa atrybutu wielowartościowego do którego istnieje odwołanie. Zaleca się, że ta właściwość nie jest używana więcej. Na przykład w grupie zarządzania, zamiast następujące:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  Należy go:`<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: Jest to właściwość opcjonalna, typ ciągu. Ta właściwość umożliwia określenie, czy mają kliknij widok elementu widoku listy do wyzwolenia odświeżania strony serwera lub, aby wyświetlić szczegóły elementu. Obsługiwane są dwa wartości opcji: ModelessDialog i serwera. Wartość domyślna to ModelessDialog.

25.  SearchOnLoad: Jest to opcjonalne, wartość logiczna typu właściwości, która określa, czy kontrolka widoku listy powinien zapytania na obciążenia. Ta właściwość ma zastosowanie tylko wtedy, gdy widok listy w trybie wyboru. Wartość domyślna dla tej właściwości jest PRAWDA. Można wyłączyć ją, jeśli oczekujesz użytkownikowi zwykle wpisz tekst do wyszukiwania, aby uzyskać przydatne wyniki. W takim przypadku widok listy zawiera początkowo komunikat do Poinformuj użytkownika, jak rozpocząć wyszukiwanie. Tekst może zostać dostosowane przez następujących właściwościach:

26.  MainSearchScreenText: Tym opcjonalne właściwości typu ciąg ma zastosowanie tylko wtedy, gdy SearchOnload jest ustawiona na true. Ta właściwość umożliwia dostosowanie tekst, który jest wyświetlany na środku widoku listy, gdy widok listy nie Szukaj automatycznie. Wartość domyślna dla tej właściwości to Znajdź zasoby chcesz, używając przycisku Wyszukaj powyżej. Można określić wartość, aby tekst był bardziej odpowiednie do realizowanego scenariusza.

27.  SubSearchScreenText: Tym opcjonalne właściwości typu ciąg służy do dostosowywania tekst, który pojawia się poniżej MainSearchScreenText. Zwykle nie trzeba określać wartość dla tej właściwości, chyba że chcesz dodać niektóre dodatkowe instrukcje dotyczące sposobu korzystania z widoku listy.

Przykłady sposobu korzystania z widoku listy oraz sterowania UocFilterBuilder jako listę wersji zapoznawczej Zobacz przykłady UocFilterBuilder we wcześniejszej części tego dokumentu. Można także UocListView bez konstruktora filtrów.

### <a name="uocnumericbox"></a>UocNumericBox

Nazwa: UocNumericBox

Opis: To pole tekstowe proste, które przyjmuje tylko wartości całkowite. Ten formant obsługuje zarówno w trybie tylko do odczytu, jak i w trybie nadaje się do aktualizacji.

Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję właściwości wspólne tego dokumentu.

2.  MaxValue: Jest to właściwość opcjonalna, typu Liczba całkowita. Ta właściwość służy do definiowania weryfikacji po stronie klienta dla formantu. Wartość, który loguje się użytkownik końcowy nie może przekraczać tę wartość. Można wprowadzić całkowitą jawne lub powiązać to danych liczb całkowitych ze źródłem danych przy użyciu {powiązania źródła = schema ścieżka = IntegerMaximum}.

3.  Wartość MinValue: Jest opcjonalne, właściwość typu Liczba całkowita. Ta właściwość służy do definiowania weryfikacji po stronie klienta dla formantu. Wartość, który loguje się użytkownik końcowy nie może być starsza niż ta wartość. Można wprowadzić całkowitą jawne lub powiązać to danych liczb całkowitych ze źródłem danych przy użyciu powiązania źródła = schema, ścieżka = IntegerMinimum}.

4.  Wartość domyślna: Jest opcjonalne, właściwość typu Liczba całkowita. Ta właściwość służy do definiowania wartości domyślnej dla formantu, jeśli formant jest używany do tworzenia nowych danych. Ta wartość może tylko jawnie ustawione statyczne wartości całkowitej.

5.  Wartość: Jest opcjonalne, właściwość typu Liczba całkowita. Jeśli możesz powiązać to dane typu Liczba całkowita ze źródła danych, wartość tego atrybutu jest wyświetlany, gdy załadować strony, a następnie jest zapisany w źródle danych po przesłaniu.

Program obsługi:

   • TextChanged: to zdarzenie jest emitowany, gdy bieżąca wartość wewnątrz formantu.

Przykład:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
Następujący przykładowy kod generuje pierwsze pole numeryczne. Pole numeryczne nie jest połączony z źródła danych, ani żadnych informacji o schemacie.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

Następujący przykładowy kod generuje drugie pole numeryczne.

>[!NOTE]
Dla tego przykładu pracować należy najpierw utworzyć nową, typu Liczba całkowita atrybutu o nazwie AnIntegerAttribute i powiąż go z typem zasobu niestandardowego.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

Nazwa: UocPictureBox

Opis: Ten formant jest używany do renderowania obrazu, dane typu binary. Firma Microsoft zaleca stosowanie tego formantu z danymi typu binarnego. Obraz może być renderowana przez adres URL podany obrazu, dane typu binarnego lub źródło atrybutu, który zawiera dane typu obrazu.

Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję właściwości wspólne tego dokumentu.

2.  ImageUrl: Jest to właściwość opcjonalna, typ ciągu. Wprowadź adres URL obrazu docelowego.

3.  MaxHeight: Jest to opcjonalne, typu ciąg-właściwość. Definiuje maksymalną wysokość obrazu w pikselach.

4.  MaxWidth: Jest to właściwość opcjonalna, typ ciągu. Definiuje maksymalną szerokość obrazu do renderowania w pikselach.

5.  ImageData: Jest to właściwość typu binary. Ta właściwość służy do powiązania źródła danych z wyświetlany obraz. Źródła danych powiązania musi być w danych binarnych.
    To pole służy również jawnie Ustaw obraz, podając dane w formacie [] bajtów.

6.  ImageResource: Jest opcjonalne, binary typu właściwości.

7.  AlternativeText: Jest to właściwość opcjonalna, typ ciągu. Ta właściwość jest wyświetlany jako tekst alternatywny, gdy nie można wyświetlić obrazu.

Przykład:

>[!NOTE]
Aby użyć tego przykładu, musi mieć istniejące dane obrazu powiązanie z formantem.


Następujący segment kodu generuje formantu pole obrazu, który jest powiązany z kontrolą źródła danych:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

Następujący segment kodu generuje formantu pole obrazu, który jest powiązany adres URL obrazu za pomocą formantu:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

Nazwa: UocRadioButtonList

Opis: Jest to prosty, lista przycisk opcji. Opcje wykluczają się wzajemnie na tej liście. Ten formant jest zalecane, jeśli użytkownik ma pięć lub mniej opcji do wyboru. W przeciwnym razie UOCListView jest zalecane.

Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję właściwości wspólne tego dokumentu.

2.  ValuePath: Ścieżka wartość jest ustawiona na wartość. Jest on powiązany z pola wartości z elementu opcja, która jest zdefiniowana w tym dokumencie.

3.  CaptionPath: Ścieżka wartość jest ustawiona na podpis. Jest on powiązany z polem podpisu z elementu opcja, która jest zdefiniowana w tym dokumencie.

4.  HintPath: Ustawiono ścieżkę wartość do wskazówki. Jest on powiązany z polem wskazówkę z elementu opcja, która jest zdefiniowana w tym dokumencie.

5.  SelectedValue: Wartość, która jest aktualnie wybrany. Jest to wymagane, właściwość typu string. Ta właściwość jest powiązany z ciągu danych ze źródła danych.

Zdarzenia:

1.  SelectedIndexChanged

2.  CheckedChanged

Opcje:

Może istnieć tylko dwa elementy opcji w obszarze Opcje dla tego formantu.

1.  Wartość: Pola wartości w jednym elemencie opcja musi mieć ustawioną wartość True lub False.

2.  Podpis: Może to być dowolną wartością ciągu.

3.  Wskazówka: Może to być dowolną wartością ciągu.

Przykład:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Aby ten przykład działał należy utworzyć nowy atrybut logiczny ABooleanAttribute i powiązać go z Twojego niestandardowy typ zasobu.

Następujący segment kodu tworzy listy przycisków opcji na poprzedniej ilustracji:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


Nazwa: UocSimpleRadioButton

Opis: To jest prosty, przycisk opcji kontroli. Użyj tego formantu jest podobny do prostych pole wyboru. Istnieją dwa przyciski opcji, przedstawiający równolegle z tekstu etykiety. Zaleca się powiązanie formantu z danymi typu Boolean.

Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję wspólne właściwości tego dokumentu

2.  Tekst_prawdy: Jest to właściwość opcjonalna, typ ciągu. To jest tekst, który jest wyświetlany, gdy opcja jest zaznaczona.

3.  Tekst_fałszu: Jest to właściwość opcjonalna, typ ciągu. To jest tekst, który jest wyświetlany, gdy nie wybrano opcję.

4.  SelectedItem: Jest to opcjonalne, wartość logiczna typu właściwości. Ta wartość wskazuje, czy wybrano opcję. To można powiązać z danymi typu wartość logiczna ze źródła danych. Wartość domyślna ma ustawioną wartość false.

Zdarzenia:

   • CheckedChanged: podczas zmiany przycisk opcji stanu z wybranych do niezaznaczony lub odwrotnie, ten sygnał jest emitowany.

Przykład:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Aby przykładowe pracy, musisz utworzyć nowy atrybut logiczny ABooleanAttribute i powiązać go z Twojego niestandardowy typ zasobu. Dane konfiguracji RCDC jest stosowany do tego samego typu zasobu niestandardowego.

Następujący segment kodu generuje przycisk opcji na poprzedniej ilustracji:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

Nazwa: UocTextBox

Opis: Jest to prosty tekst pola, które obsługuje dane wejściowe typu ciąg. Zalecane jest użycie tego formantu można powiązać z danymi typu ciąg.

Właściwości:

1.  Wspólne właściwości wszystkich: Informacji o tej właściwości, zobacz sekcję właściwości wspólne tego dokumentu.

2.  Element MaxLength: Jest to opcjonalne, atrybut typu Liczba całkowita. Ta właściwość określa maksymalną długość ciągu danych wejściowych. Wartość domyślna dla tej właściwości to 128 znaków.

3.  Tekst: Jest opcjonalne, właściwość typu string. To jest tekst wyświetlany w polu tekstowym. Można zdefiniować jawnej ciąg, który znajduje się w polu tekstowym podczas ładowania początkowego formantu lub powiązać na atrybut schematu typu string.

4.  Wiersze: Jest opcjonalne, właściwość typu Liczba całkowita. Ta właściwość określa wysokość pola tekstowego w jednostkach znakowych. Wartość domyślna to 1 znak.

5.  Kolumny: Jest opcjonalne, właściwość typu Liczba całkowita. Ta właściwość określa szerokość pola tekstowego w jednostkach znakowych. Wartość domyślna to 20 znaków.

6.  Zawijaj: Jest to opcjonalne, wartość logiczna typu właściwości. Za pomocą ustawienia wartości tej właściwości na wartość true, użytkownik włącza funkcję zawijanie w polu tekstowym. Wartość domyślna tej właściwości jest ustawiana na wartość true.

7.  UniquenessValidationXPath: Jest to właściwość opcjonalna, typ ciągu. Pobiera prawidłowe wyrażenie filtru FIM XPath, a zapewnia, że wartości wejściowych przez użytkownika jest unikatowa w ramach zasobów, które znajdują się w zakresie filtru.
    Na przykład, aby upewnić się, że użytkownik zażądał, nazwa wyświetlana jest unikatowa w obrębie wszystkich grup zabezpieczeń włączoną obsługę poczty w bazie danych usługi FIM, można użyć wyrażenia XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. Działanie sprawdzania poprawności jest wykonywane, gdy użytkownik opuści strony. Ta właściwość jest obsługiwana tylko w konfiguracji RCDC w celu utworzenia zasobu.

8.  UniquenessErrorMessage: Jest to właściwość opcjonalna, typ ciągu. Ten ciąg jest używany do wyświetlania komunikat o błędzie w przypadku niepowodzenia weryfikacji UniquenessValidationXPath i może być jawnego tekstu lub zmiennej ciągu zasobu. Jeśli ta właściwość nie jest określona, domyślnego komunikatu o błędzie weryfikacji zakończonej niepowodzeniem to "% wartość już istnieje. Spróbuj użyć innego."

Zdarzenia:

   • TextChanged: to zdarzenie jest emitowany, gdy zostanie zmieniony tekst w polu tekstowym.

Zobacz sekcję przykłady prostego formantu dla kompletnego przykładu tego formantu.

## <a name="appendix-a-default-xsd-schema"></a>Dodatek A: domyślny schemat XSD

Poniżej znajduje się pełna schematu XSD dla wszystkich domyślne RCDCs dostarczanych z dodatku SP1 dla programu Microsoft Identity Manager 2016:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>Dodatek B: domyślne podsumowanie XSL

Poniżej znajduje się pełne podsumowanie XSL dostarczonego z programu Microsoft Identity Manager 2016 z dodatkiem SP1:
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
