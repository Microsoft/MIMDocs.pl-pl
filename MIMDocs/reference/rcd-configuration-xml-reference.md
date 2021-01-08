---
title: Dokumentacja kodu XML konfiguracji wyświetlania kontroli zasobów | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 0eedee975a785bd20ee37c85262a0c5f678b09b5
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010714"
---
# <a name="resource-control-display-configuration-xml-reference"></a>Dokumentacja kodu XML konfiguracji wyświetlania formantów zasobów

Zasoby konfiguracji wyświetlania kontroli zasobów (RCDC) to zasoby zdefiniowane przez użytkownika, których można użyć do kontrolowania sposobu, w jaki inne zasoby w magazynie danych Microsoft Identity Manager 2016 SP1 (MIM) są wyświetlane w interfejsie użytkownika dla użytkownika końcowego. Każdy zasób RCDC zawiera plik konfiguracyjny XML, który można zmienić, aby dodać, zmodyfikować lub usunąć tekst interfejsu użytkownika i kontrolki interfejsu użytkownika. Podczas gdy program MIM 2016 z dodatkiem SP1 zawiera kilka domyślnych zasobów RCDC, można także utworzyć niestandardowe zasoby RCDC dla zasobów niestandardowych. Aby uzyskać więcej informacji na temat korzystania z interfejsu użytkownika RCDC w portalu programu FIM, zobacz [wprowadzenie do konfigurowania i dostosowywania portalu programu FIM](/previous-versions/mim/ee534913(v=ws.10)) w dokumentacji programu FIM.


## <a name="known-issues"></a>Znane problemy

Wartość domyślna w wielu kontrolkach RCDC nie jest obsługiwana.

W tej wersji ustawienie wartości domyślnych w kontrolkach w kontrolce zasobu nie jest obsługiwane z wyjątkiem kontrolki przycisku opcji. Ten problem można obejść w polu listy rozwijanej, określając wartość domyślną, która nie jest skojarzona z żadną wartością, aby wymusić zmianę zaznaczenia przez użytkownika. Aby obejść ten problem z innymi kontrolkami, należy użyć przepływu pracy autoryzacji w celu dostarczenia wartości domyślnej podczas przekazywania żądania.

## <a name="basic-structure"></a>Struktura podstawowa

Dane XML dla zasobu RCDC składają się z pojedynczego elementu XML **ObjectControlConfiguration** .

>[!NOTE]
>Aby uzyskać pełny schemat XSD, zobacz <a href="#appendix-a">dodatek A: domyślny schemat XSD</a>.

Poniżej znajduje się schemat XSD dla elementu **ObjectControlConfiguration** :

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

Element **ObjectControlConfiguration** zawiera następujące elementy:

- **ObjectDataSource**: ten element określa właściwość TypeName klasy źródła danych używanej przez kontrolę zasobów (RC). Opis i definicja schematu znajdują się w sekcji następujące źródła danych w tym dokumencie. Element **ObjectControlConfiguration** może zawierać do 32 węzłów elementu **ObjectDataSource** .

- **XmlDataSource**: to proste źródło danych najczęściej używane do określania projektu strony podsumowania. Opis i definicja schematu znajdują się w sekcji następujące źródła danych w tym dokumencie. **ObjectControlConfiguration**: element może zawierać do 32 węzłów elementu **XmlDataSource** .

- **Panel**: Administrator może dostosować układ strony RCDC, modyfikując elementy wewnątrz elementów panelu. Aby uzyskać więcej informacji, zobacz sekcję panel w dalszej części tego dokumentu. Element **ObjectControlConfiguration** musi mieć tylko jeden element panelu.

- **Zdarzenia**: Administratorzy nie mogą zapewnić w tle dostosowanego kodu. Ta funkcja jest ograniczona. Jest to zdarzenie, które panel lub kontrolka może emitować, na podstawie zmiany stanu. Aby uzyskać więcej informacji, zobacz sekcję zdarzenia w dalszej części tego dokumentu. Element **ObjectControlConfiguration** może zawierać opcjonalnie jeden element **zdarzenia** . Ogólnie rzecz biorąc, użycie niestandardowych **zdarzeń** nie jest obsługiwane, chyba że zostały one opracowane w późniejszym czasie.

## <a name="data-sources"></a>Źródła danych

Microsoft Identity Manager używa źródeł danych jako metody wiązania danych ze składnikami interfejsu użytkownika. Ułatwia to rozdzielenie danych z warstwy prezentacji. W danych konfiguracji zasobów RCDC są dwa rodzaje źródeł danych: **ObjectDataSource** i **XmlDataSource**.

-   Element **objectdatasources** określa klasę Microsoft .NET, która dostarcza dane do wersji RC. Istnieje stały zestaw dostępnych typów elementów ObjectDataSource, których administrator może użyć podczas tworzenia RCDCs.

-   **Xmldatasources** zapewniają prosty sposób struktury danych opartych na języku XML i mogą być używane przez administratorów do udostępniania niestandardowych danych. Dane XML należy określić bezpośrednio w RCDC, chyba że używana jest wbudowana, wstępnie zdefiniowana Struktura XML. Wbudowana Struktura XML służy do generowania stron podsumowujących w wersji RC.

W RCDC można powiązać te źródła danych z atrybutami formantów interfejsu użytkownika, które są określone w RCDC w celu wygenerowania interfejsu użytkownika.

### <a name="objectdatasource-elements"></a>Elementy ObjectDataSource

Program Microsoft Identity Manager udostępnia typy wspólnych źródeł danych w poniższej tabeli, które są dostępne dla wszystkich typów zasobów (z wyjątkiem sytuacji, w których zostały zanotowane).

| TypeName | Opis | Powiązanie dwukierunkowe | Składnia powiązania |
|---|---|---|---|
| PrimaryResourceObjectDataSource | Reprezentuje to zasób programu FIM 2010, który jest tworzony, edytowany lub wyświetlany. Ścieżka w ciągu powiązania jest nazwą atrybutu. Typ zasobu jest określany przez atrybut TargetObjectType elementu RCDC, a nie w atrybucie RCDC.ConfigurationData. | Tak | `[AttributeName]` wartość atrybutu obiektu podaną przez jego nazwę. |
| PrimaryResourceDeltaDataSource  | To źródło danych kompiluje różnicowy kod XML, który porównuje pierwotny stan i bieżący stan zasobu FIM 2010. Wygenerowany różnicowy plik XML jest używany przez kontrolkę podsumowania RC w celu renderowania interfejsu użytkownika dla żądania przesyłanego przez użytkownika. | Nie | `DeltaXml` Jest on używany z formantem podsumowania do wyświetlania delty. |
| PrimaryResourceRightsDataSource | To źródło danych udostępnia wbudowane prawa dla każdego atrybutu zasobu programu FIM 2010. Dzięki temu RC może określić z wyprzedzeniem, które uprawnienia użytkownik ma dla tego atrybutu, a następnie prawidłowo renderować interfejs użytkownika dla tego atrybutu. | Nie | `[AttributeName]` |
| SchemaDataSource                | To źródło danych może służyć do uzyskiwania dostępu do informacji powiązanych ze schematem, takich jak nazwa wyświetlana, opis, czy atrybut jest wymagany, a także informacje o typie zasobu. | Nie | `[AttributeName].Required` Wartość logiczna wskazująca, czy atrybut musi mieć prawidłową wartość. <br/> `[AttributeName].DisplayNameString` Wartość wskazująca nazwę wyświetlaną powiązania. <br/> `[AttributeName].DescriptionString` Wartość wskazująca na opis powiązania. <br/>`[AttributeName].StringRegexString` wartość, która wskazuje ciąg wyrażenia regularnego dla powiązania. <br/> `[AttributeName].DisplayName` <br/> `[AttributeName].Description` <br/> `[AttributeName].IntegerValueMinimum` <br/>`[AttributeName].IntegerValueMaximum` <br/>`[AttributeName].LocalizedAllowedValues` |
| DomainDataSource                | To źródło danych umożliwia Wyliczenie domen w oparciu o zasoby konfiguracji domeny. To źródło danych może być używane tylko w RCDCs, które są przeznaczone dla zasobów grupy i zasobów użytkownika. | Tak | Domena |

Poniżej znajduje się przykładowy fragment kodu RCDC, który wiąże trzy źródła danych z kontrolką UocTextBox, aby edytować atrybut Description grupy:

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

### <a name="xmldatasource-element"></a>Element XmlDataSource

Za pomocą elementu **XmlDataSource** można określić niestandardowe dane, które RCDC mogą zużywać dla danego zasobu. W takim przypadku dane XML muszą być określone w RCDC. Alternatywnie to źródło danych może służyć do odwoływania się do wbudowanej struktury danych XML w celu renderowania interfejsu użytkownika dla stron podsumowania. Należy kontrolować typ elementu **XmlDataSource** , który ma być używany podczas definiowania go w RCDC.


| TypeName | Opis | Powiązanie dwukierunkowe | Składnia powiązania |
|---|---|---|---|
| **Elementu**            | Źródło danych reprezentuje dane XML. Dane mogą mieć postać XSL lub osadzonego formatu XSL:<ul><li>Format XSL w Microsoft.IdentityManagement.WebUI.Controls.dll:<br/>```<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement. WebUI.Controls.Resources.DefaultSummary.xsl"> </my:XmlDataSource>```</li><li>Osadzony format XSL:<br/>```<my:XmlDataSource my:Name="RequestStatusTransformXsl"><xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform xmlns:msxsl="urn:schemas-microsoft-com:xslt"></xsl:stylesheet></my:XmlDataSource>```</li></ul>  | Nie | `Xpath[;namespaces]`gdzie `Xpath` to prawidłowe wyrażenie XPath XML, aby wybrać wymaganą notatkę, najczęściej "/" (root). `namespaces` jest opcjonalną listą prefiksów ciągów identyfikatorów URI. Ciąg jest rozdzielany średnikami, co jest wymagane, aby wyrażenie XPath działało do pliku XML z przestrzenią nazw. |
| **ReferenceDeltaDataSource** | Źródło danych reprezentuje różnice w atrybutach odwołań wielowartościowych. Jest on używany tylko na RCDC dla grupy i zestawu. <br/> Chociaż źródło danych nie jest ograniczone do grup lub zestawów, wymaga zmiany kodu na hoście RCDC, aby przesłać takie delty. Obecnie grupy i zestawy są jedynymi hostami, które rozpoznają to źródło danych.  | Tak | `[AttributeName].Add` gdzie `[AttributeName]` reprezentuje atrybut Reference, a zwrócone dane są uzupełnieniem różnicowym.<ul><li>Przykład: `[ReferenceAttribute].Add`</li><li>Przykład: `<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}"/>`</li></ul>`[AttributeName].Remove` gdzie `[AttributeName]` reprezentuje atrybut Reference, a zwrócone dane są usuwaniem różnicowym. <br/> DeltaXml <!-- Is bold formatting needed for DeltaXml? --> |
|**RequestDetailsDataSource**| Źródło danych reprezentuje atrybut RequestParameter obiektów żądania. Parametr ustawia maksymalną liczbę wartości atrybutów, które mają być wyświetlane dla atrybutu wielowartościowego. Jest on używany tylko w RCDC dla żądania. `<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />`| Nie | DeltaXml |
|**RequestStatusDataSource**| Źródło danych reprezentuje atrybut **RequestStatusDetails** obiektów żądania. Jest on używany tylko w RCDC dla żądania. | Nie | DeltaXml |

Aby zdefiniować niestandardowe źródło danych XML, użyj następującego kodu XML:

 ```XML
<my:XmlDataSource my:Name="MyCustomData" >
     %Insert custom, properly formatted XML data here%
</my:XmlDataSource>
```

Aby użyć wbudowanego kodu XSL kontrolki podsumowania, zdefiniuj źródło danych w następujący sposób:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

Jeśli tworzysz RCDC dla niestandardowego typu zasobu, możesz użyć tej metody, aby automatycznie renderować stronę podsumowania dla tego zasobu niestandardowego.

Poniżej przedstawiono przykład sposobu tworzenia karty podsumowania w RCDC, przy użyciu elementu **PrimaryResourceDeltaDataSource** z elementem **XmlDataSource** przy użyciu wbudowanego kodu xsl:

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

Alternatywnie użytkownik może zastąpić element XmlDataSource określony wcześniej następującym formatem, aby zdefiniować dostosowany układ strony podsumowania. Jako odwołanie, domyślny plik XSL z podsumowaniem programu FIM 2010 jest zawarty w dodatku B: domyślny plik XSL podsumowania w dalszej części tego dokumentu.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Schemat dla źródeł danych
Poniższy schemat XSD generuje dwa typy źródeł danych:

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

## <a name="event-element"></a>Element zdarzenia
Element **zdarzenia** definiuje zmieniający się stan kontrolki. Rozszerzalność tej funkcji jest ograniczona, ponieważ nie można zapisać dostosowanej funkcji (procedury obsługi) w celu określenia, jakie zachowanie jest wyzwalane po wyzwoleniu zdarzenia. Ten sam element zdarzenia może być używany w elemencie panelu. Aby uzyskać więcej informacji, zobacz sekcję panel w dalszej części tego dokumentu.

Poniżej znajduje się schemat XSD dla elementu zdarzenia:

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

**Zdarzenie** jest pustym elementem i ma następujące atrybuty:

- **Nazwa**: jest to unikatowa nazwa zdarzenia. Jedynym obsługiwanym zdarzeniem w **ObjectControlConfiguration** jest zdarzenie ładowania. To zdarzenie jest wyzwalane podczas pierwszego ładowania strony.

- **Procedura obsługi**: jest to unikatowa nazwa programu obsługi. Gdy zdarzenie jest wyzwalane, zazwyczaj wywoływana jest metoda programu, aby obsłużyć zmianę stanu formantu. Następujące przypadki nie są obsługiwane:

   - Usuwanie istniejącego programu obsługi z istniejącej kontrolki.
   - Tworzenie nowego programu obsługi.
   - Dołączanie programu obsługi do istniejącej lub nowej kontrolki.

Poniżej przedstawiono przykład elementu **Events** :

```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

## <a name="panel-element"></a>Element Panelu
Element **panel** jest elementem podstawowym w układzie RCDC. Poniżej znajduje się schemat XSD dla elementu panelu:

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

Element **panelu** zawiera element cykliczny, **grupowanie**. Aby uzyskać więcej informacji, zobacz sekcję Grupowanie w tym dokumencie.

Element panelu ma następujące atrybuty:

- **Nazwa**: nazwa panelu. Jest to wymagany atrybut typu String.

- **DisplayAsWizard**: ten atrybut jest obecnie przestarzały. Odpowiedni atrybut VerbContext na RCDC określa, czy układ zasobów jest w trybie kreatora, czy na karcie. Jeśli jest ustawiona na 0 (tryb tworzenia), jest również w trybie kreatora. W przeciwnym razie jest w trybie karty. Aby uzyskać więcej informacji, zobacz Wprowadzenie do konfigurowania i dostosowywania portalu programu FIM w dokumentacji.

- **Podpis**: ten atrybut jest obecnie przestarzały. Użytkownik może określić podpisy dla strony, dołączając grupę, która zawiera tylko informacje nagłówka. Aby uzyskać więcej informacji, zobacz sekcję Grupowanie w tym dokumencie.

- **Autowalidacja**: jest to opcjonalny atrybut Boolean. Gdy jest ustawiona na true, walidacja jest wyzwalana dla każdej kontrolki na bieżącej karcie. Domyślnie, jeśli brakuje atrybutu, zostanie ustawiona wartość true. Może być używana w połączeniu z właściwością RegularExpression. Aby uzyskać więcej informacji, zobacz sekcję "RegularExpression" w dalszej części tego dokumentu.

## <a name="grouping-element"></a>Element grupujący
Element **grupujący** definiuje ogólny układ panelu. Działa jako kontener, który grupuje poszczególne kontrolki w różne sekcje i karty. Poniżej znajduje się schemat XSD dla elementu grupującego:

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

Istnieją trzy typy elementów **grupujących** :

- **Grupowanie nagłówków**: grupowanie nagłówków jest opcjonalne. W **panelu** może istnieć tylko jedno grupowanie nagłówka. Grupowanie nagłówków pojawia się na górze panelu jako napis. W tym grupowaniu można używać tylko jednego UocCaptionControl. Przykład grupowania nagłówków można znaleźć w sekcji przykładowej.

- **Grupowanie zawartości**: wymagane jest co najmniej jedno Grupowanie zawartości. W panelu może istnieć wiele grup zawartości. Grupowanie zawartości jest wyświetlane jako główna Zawartość strony RCDC. Każde Grupowanie zawartości jest wyświetlane jako karta w tym samym panelu i może zawierać od 1 do 256 kontrolek. Przykład **grupowania zawartości** można znaleźć w sekcji przykładów.

- **Grupowanie podsumowań**: grupowanie podsumowań jest opcjonalne. W panelu może istnieć tylko jedno grupowanie podsumowujące. Grupowanie podsumowań pojawia się jako ostatnia karta panelu. Tylko jeden formant **UocHtmlSummary** może być używany w grupowaniu podsumowującym do wyświetlania zmian wprowadzonych przez użytkownika przed przesłaniem żądania. Przykład grupowania podsumowującego znajduje się w sekcji przykładów.

Każdy typ grupowania zawiera następujące elementy:

- **Pomoc**: ten element zawiera tekst pomocy na karcie. Można go również użyć do dodania linku do pliku pomocy dla karty.

- **Kontrolki**: Aby uzyskać informacje na temat tego elementu, zobacz sekcję kontrolki w tym dokumencie. Każde grupowanie musi zawierać od 1 do 256 kontrolek włącznie, w zależności od typu grupowania.

- **Zdarzenia**: Aby uzyskać informacje na temat tego elementu, zobacz sekcję Events w tym dokumencie. Każde grupowanie może, jako opcję, ma jedno zdarzenie. Zdarzenia obsługiwane w elemencie grupującym są następujące:

    - **BeforeLeave**: to zdarzenie jest wyzwalane, gdy użytkownik jest gotowy do opuszczenia karty w grupowaniu zawartości.
    - **AfterEnter**: to zdarzenie jest wyzwalane, gdy użytkownik jest gotowy do wprowadzenia karty w grupie zasobów.

Grupowanie może zawierać następujące siedem atrybutów:

- **Nazwa**: jest to wymagana nazwa grupowania. **Nazwa** musi być unikatowa w obrębie **panelu**.

- **Podpis**: **podpis** pojawia się jako podpis nagłówka w grupowaniu nagłówka. Pojawia się jako podpis karty zawartości lub grupowania podsumowania.

- **Opis**: opcjonalny atrybut ciągu, **Description** działa tylko wtedy, gdy jest używany w grupowaniu zawartości. Użyj tego elementu, aby dać użytkownikowi końcowemu szczegółowe informacje o informacjach znajdujących się na tej samej karcie.

    >[!NOTE]
    >Jeśli ten atrybut jest używany w grupowaniu podsumowującym, kod XML jest uznawany za nieprawidłowy. Jeśli ten atrybut jest używany w grupowaniu nagłówka, kod XML jest uznawany za prawidłowy, ale ignorowany.
    >

- **Włączone**: opcjonalny atrybut Boolean, enabled ma ustawioną wartość true, jeśli nie istnieje. Jeśli ustawienie jest włączone ma wartość FAŁSZ, użytkownik końcowy zobaczy kartę wyłączone. Ten atrybut jest funkcjonalny tylko w grupowaniu zawartości.

    >[!NOTE]
    >Jeśli ten atrybut jest używany w grupowaniu podsumowującym, kod XML jest uznawany za nieprawidłowy. Jeśli ten atrybut jest używany w grupowaniu nagłówka, kod XML jest uznawany za prawidłowy, ale ignorowany.
    >

- **Widoczne**: można ukryć kartę strony RCDC lub jej nagłówek, ustawiając dla tego atrybutu wartość false. Domyślnie ten opcjonalny atrybut typu Boolean ma ustawioną wartość true. Ten atrybut jest funkcjonalny tylko w grupowaniu zawartości.

    >[!NOTE]
    >Jeśli w panelu istnieje tylko jedno Grupowanie zawartości, ta funkcja nie działa. W przypadku więcej niż jednego grupowania zawartości w panelu działa ono jak wcześniej opisane.

- **Isnagłówker**: ten atrybut jest opcjonalnym atrybutem Boolean, który definiuje, czy grupowanie jest grupowaniem nagłówków. Jeśli ten atrybut nie jest określony, zostanie ustawiona wartość false.

- **Issummary**: jest to opcjonalny atrybut logiczny, który określa, czy grupowanie jest grupą podsumowującą. Jeśli ten atrybut nie jest określony, zostanie ustawiona wartość false.

### <a name="examples-for-types-of-grouping-elements"></a>Przykłady typów elementów grupowania
Ta sekcja zawiera przykłady dla elementu grupowania.

#### <a name="example-header-grouping"></a>Przykład: grupowanie nagłówków
Na poniższej ilustracji przedstawiono przykładowe grupowanie nagłówków:

![Grupowanie nagłówków](media/rcd-configuration-xml-reference/image005.jpg)

Poniższy kod XML generuje przykładową grupę nagłówków. W kodzie XML grupowanie nagłówka jest obszar z napisem "Przykładowe grupowanie nagłówka".

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

#### <a name="example-content-grouping"></a>Przykład: Grupowanie zawartości
Na poniższej ilustracji przedstawiono przykładowe Grupowanie zawartości:

![Grupowanie zawartości](media/rcd-configuration-xml-reference/image007.jpg)

Poniższy kod XML generuje przykładowe Grupowanie zawartości. W kodzie XML Grupowanie zawartości to obszar z napisem "Przykładowe Grupowanie zawartości".

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

#### <a name="example-summary-grouping"></a>Przykład: grupowanie podsumowań

Na poniższej ilustracji przedstawiono przykładowe grupowanie podsumowań:

![Grupowanie podsumowań](media/rcd-configuration-xml-reference/image010.jpg)

Poniższy kod XML generuje przykładowe grupowanie podsumowań. W kodzie XML grupowanie podsumowań to obszar z tekstem podpisów "Przykładowe grupowanie podsumowań".

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

### <a name="help-element"></a>Element pomocy
Element **pomocy** może być zawarty w grupie lub elemencie kontrolnym jako element opcjonalny. Jeśli jest używana w grupowaniu, musi być pierwszym używanym elementem. Umożliwia ona użytkownikom końcowym pomoc w dostarczaniu dokładnych informacji. Następujący schemat XSD jest przeznaczony dla elementu pomocy:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```

Następujący przykładowy kod XML generuje element pomocy:

```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control-element"></a>Element Control
Element grupujący zawiera jeden lub więcej elementów **sterujących** . Formanty są głównymi elementami w RCDC. Można dostosować element grupowania, definiując różne elementy kontrolne, które zawiera. Następujący schemat XSD jest przeznaczony dla elementu Control:

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

Element kontrolny zawiera następujące elementy:

- **Pomoc**: ten element jest ignorowany. Działa tylko w grupowaniu.

- **CustomProperties**: ten element nie jest obsługiwany.

- **Opcje**: ten element jest używany tylko w połączeniu z kontrolkami **UocDropDownList** lub **UocRadioButtonList** . Nie działa ona z żadną inną kontrolką. Zapoznaj się z sekcją opcje w tym dokumencie, aby poznać strukturę tego elementu. Zobacz sekcję poszczególne kontrolki w tym dokumencie, aby zobaczyć, jak opcje są używane przez formant.

- **Przyciski**: ten element jest używany tylko w połączeniu z kontrolką **UocListView** . Nie działa dla żadnych innych kontrolek. Aby uzyskać więcej informacji, zobacz sekcję UocListView w tym dokumencie.

- **Właściwości**: ten element jest używany we wszystkich kontrolkach w celu określenia dodatkowych zachowań kontrolki. Aby uzyskać informacje na temat tego elementu, zobacz sekcję właściwości w tym dokumencie.

- **Zdarzenia**: dla struktury tego elementu zapoznaj się z sekcją zdarzenia wcześniej w tym dokumencie. Zobacz sekcję poszczególne kontrolki w tym dokumencie, aby zobaczyć, które zdarzenia są używane w formancie.

Element kontrolny może zawierać następujące 10 atrybutów:

- **Nazwa**: to jest nazwa formantu. Nazwa formantu musi być unikatowa w każdym panelu. Jest to wymagany atrybut typu String.

- **TypeName**: ten atrybut określa typ kontrolki, która jest. Jest to wymagany atrybut typu String. Zapoznaj się z sekcją poszczególne kontrolki w tym dokumencie dla każdej nazwy formantu.

- **Caption**: ten atrybut służy do dołączania podpisu dla kontrolki. Podpis jest zazwyczaj nazwą wyświetlaną danych wyświetlanych lub wprowadzanych przez formant. Można jawnie określić wartość dla podpisu lub powiązać ją z informacjami o nazwie wyświetlanej atrybutu schematu. Podpis pojawia się po lewej stronie kontrolki o normalnej wielkości. Jeśli kontrolka zawiera pełny ekran, podpis pojawia się nad formantem. Jest to opcjonalny atrybut typu String. Informacje o sposobie powiązania źródła danych z atrybutem lub wartością właściwości można znaleźć w sekcji Właściwości.

    Poniższy przykład pokazuje, jak można jawnie użyć podpisu:

    ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
    ```

    Poniższy przykład pokazuje, jak można użyć podpisu ze źródłem danych. Jeśli użyto szablonu dla źródła danych pokazanego wcześniej w tym dokumencie, źródło danych jest schematem. Zalecamy powiązanie właściwości DisplayName z atrybutem Caption.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```

- **Włączone**: jest to opcjonalny atrybut typu Boolean. Ustawienie tej wartości atrybutu na false powoduje, że użytkownik może wyłączyć formant. Wartość domyślna to true.

- **Visible**: jest to opcjonalny atrybut typu Boolean. Tego atrybutu można użyć, aby ukryć cały formant. Wartość domyślna to true.

- **Opis**: Użyj tego opcjonalnego atrybutu typu String, aby dołączyć opis, który pomoże użytkownikowi końcowemu zrozumieć, co powinno być umieszczone w kontrolce lub jakie ma formant. Można jawnie określić wartość dla opisu lub powiązać ją z informacjami o opisie atrybutu schematu.

    Opis pojawia się po lewej stronie kontrolki o normalnej wielkości pod podpisem. Jeśli kontrolka zawiera pełny ekran, opis pojawia się u góry kontrolki pod podpisem. Informacje o sposobie powiązania źródła danych z atrybutem lub wartością właściwości można znaleźć w sekcji właściwości w tym dokumencie.

    Poniższy przykład pokazuje, jak można jawnie użyć opisu:

    ```XML
    <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
    ```

    Ten przykład pokazuje, jak opis może być używany ze źródłem danych. Jeśli użyto szablonu dla źródła danych pokazanego wcześniej w tym dokumencie, źródło danych jest **schematem**. Zalecamy, aby powiązać **Opis** atrybutu z atrybutem Description.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
    ```

- **ExpandArea**: ten atrybut wskazuje, czy kontrolka obejmuje pełny ekran. Jest to opcjonalny atrybut typu Boolean. Wartość domyślna to false.

    >[!NOTE]
    >Atrybuty podpisu i opisu są wyłączone, gdy ten atrybut ma wartość true. Użyj kontrolki UocLabel, aby podać podpis rozszerzonej kontrolki.
    >

- **Wskazówka**: jest to opcjonalny atrybut typu String. Tekst w atrybucie wskazówki ułatwia użytkownikowi końcowemu decydowanie o tym, co jest prawidłowymi danymi wejściowymi dla kontrolki. Wskazówka zostanie wyświetlona pod kontrolką.

- **Autoogłaszanie zwrotne**: jest to opcjonalny atrybut typu Boolean. Wartość domyślna to false. Jeśli zostanie ustawiona na wartość false, odświeżenie strony może nie spowodować odświeżenia formantu. Aby uzyskać informacje na temat autoogłaszania zwrotnego, poszukaj Microsoft ASP.NET właściwości kontrolki interfejsu użytkownika o tej samej nazwie.

- **RightsLevel**: jest to opcjonalny atrybut typu String. Ten atrybut można powiązać tylko z prawami wbudowanymi ze źródłem danych. Kontrolka jest dynamicznie włączana lub wyłączana na podstawie praw użytkownika. Informacje o sposobie powiązania źródeł danych z atrybutem lub wartością właściwości można znaleźć w sekcji właściwości w tym dokumencie.

    Ten przykład pokazuje, jak atrybut **RightsLevel** może być używany ze źródłem danych. Jeśli użyto szablonu dla źródła danych pokazanego wcześniej w tym dokumencie, Twoje źródło danych to **prawa**. Użyj nazwy atrybutu jako ścieżki.
    <!--- no example provided -->

### <a name="property-element"></a>Property, element
Można użyć elementu **Właściwości** , aby bardziej dostosować zachowanie poszczególnych kontrolek. Właściwość jest pustym elementem. Następujący schemat XSD jest przeznaczony dla elementu Property:

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

Każda właściwość ma następujące dwa wymagane atrybuty:

- **Nazwa**: ten atrybut typu String jest unikatową nazwą właściwości. Różne kontrolki mają różne właściwości. Istnieją pewne typowe właściwości, które mogą być używane przez wszystkie kontrolki. Aby uzyskać więcej informacji na temat nazw dostępnych dla danej kontrolki, zobacz sekcję wspólne właściwości i poszczególne kontrolki tego dokumentu.

- **Wartość**: jest to wartość właściwości. Typ danych wartości zależy od właściwości, do której jest przypisana. W poniższej sekcji znajduje się dozwolony format wartości dla określonych właściwości.


#### <a name="bind-property-with-data-source-content"></a>Powiązywanie właściwości z zawartością źródła danych
Niektóre właściwości można powiązać z informacjami ze źródła danych. Użyj następującego formatu ciągu, aby utworzyć to powiązanie. Zobacz opis poszczególnych właściwości w sekcji poszczególne kontrolki tego dokumentu, aby dowiedzieć się, jak powiązać właściwości ze źródłem danych.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

   Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}

   SourceExpression:= “Source=” + [ObjectDataSourceName]

   PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

   ModeExpression:= “Mode=” + [ModeChoice]

   ModeChoice:= “OneWay”|”TwoWay”

   ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

   AttributeName:= valid schema attribute name from the data source.

   AttributePropertyName:= valid property name of a schema attribute from the data source.
```

Poniższy kod XML przedstawia sposób powiązania źródła danych z elementem **Właściwości** :

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
```


<h2 id="common-properties">Wspólne właściwości</h2>

Wszystkie kontrolki RCDC określone w tym dokumencie mogą mieć wspólne właściwości opisane w tej sekcji. Można użyć tych właściwości wraz z innymi właściwościami, które są specyficzne dla różnych kontrolek.

- **Wymagane**: Ta właściwość wskazuje, że pole jest polem wymaganym lub polem opcjonalnym. Wymagane pole musi być wypełnione wartością. Pusta wartość nie jest obsługiwana w przypadku danych wejściowych ciągu. Pole opcjonalne może pozostać puste. Jeśli to pole jest polem wymaganym bez wypełnionej wartości, w górnej części kontrolki wprowadzania zostanie wyświetlony komunikat o błędzie. Można jawnie określić, czy pole jest wymagane, czy opcjonalne. Można również powiązać pole z informacjami o schemacie danego powiązania między atrybutem a typem zasobu. Domyślnie, jeśli brakuje tej właściwości, oznacza to, że formant jest opcjonalną kontrolką wejściową.

    W poniższym przykładzie użyta jest jawna wartość dla tej właściwości:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```

    Jest to przykład, który używa dynamicznego źródła danych dla tej właściwości. Jeśli użyto szablonu dla źródła danych pokazanego w poprzedniej sekcji tego dokumentu, źródło danych jest schematem. Użyj `<attribute name>.Required` jako ścieżki.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```

- **Tylko do odczytu**: przez ustawienie tej właściwości na wartość true użytkownik końcowy będzie mógł przeprowadzić kontrolę w trybie tylko do odczytu. Jest to opcjonalny atrybut typu Boolean. Wartość domyślna to false. Jednak czasami zachowanie tej właściwości jest zastępowane przez typ praw, które osoba ma w powiązaniu danych z formantem. Na przykład jeśli użytkownik nie ma uprawnień do aktualizowania pola, a pole jest powiązane z prawami wbudowanymi, użytkownik widzi dane w trybie tylko do odczytu, nawet ta właściwość ma wartość false.

- **RegularExpression**: Ta właściwość określa ograniczenia, które są nakładane na wartość w formancie. Formaty tej wartości właściwości są formatami obsługiwanymi w standardzie .NET StringRegex. Aby uzyskać więcej informacji, zobacz [.NET Framework wyrażeń regularnych](https://go.microsoft.com/fwlink/?LinkId=165361). Jeśli formant jest używany do wprowadzania wartości, wartość jest sprawdzana względem ograniczenia, które jest określone w tej właściwości, gdy użytkownik próbuje opuścić bieżącą stronę. Komunikat o błędzie pojawia się na górze formantu, który ma nieprawidłowe dane wejściowe. Użytkownik może jawnie określić wyrażenie regularne ciągu. Użytkownik może również powiązać z informacjami o schemacie danego atrybutu. Domyślnie, jeśli brakuje tej właściwości, oznacza to, że formant nie sprawdza żadnych ograniczeń dotyczących ciągów wejściowych.

    W poniższym przykładzie użyta jest jawna wartość dla tej właściwości:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```

    Jest to przykład, który używa dynamicznego źródła danych dla tej właściwości. Jeśli użyto szablonu do źródła danych pokazanego wcześniej w tym dokumencie, źródło danych jest schematem. Użyj `<attribute name>.StringRegex` jako ścieżki.

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```

- **Visible**: jest to opcjonalny atrybut typu Boolean. Tego atrybutu można użyć, aby ukryć cały formant. Wartość domyślna to true.


<h3 id="options-element">Element Options</h3>

Element Options zawiera jeden lub więcej podwęzłów **opcji** . Element **Options** jest używany tylko z kontrolkami **UocRadioButtonList** i **UocDropDownList** . Aby uzyskać szczegółowe informacje na temat sposobu korzystania z tych kontrolek, zobacz sekcję poszczególne kontrolki tego dokumentu.

Następujący schemat XSD jest przeznaczony dla elementu Options:

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

Element **Options** ma następujące atrybuty:

- **Wartość**: jest to wymagany atrybut typu String. Atrybut value musi być unikatowy w obrębie tej samej kontrolki. Można używać tylko znaków od i do Z, bez uwzględniania wielkości liter.

- **Caption**: ten wymagany atrybut jest nazwą wyświetlaną każdej opcji.

- **Wskazówka**: jest to opcjonalny atrybut. Użyj tego atrybutu, aby uzyskać więcej informacji i wskazówek dla użytkownika końcowego.


## <a name="environment-variables"></a>Zmienne środowiskowe

Następujące zmienne środowiskowe mogą być używane w dowolnej konfiguracji RCDC:

| Zmienna | Opis |
|---|---|
| `<LoginID>`       | Wyświetla identyfikator zalogowanego użytkownika.           |
| `<LoginDomain>`   | Wyświetla domenę zalogowanego użytkownika.       |
| `<Today>  `       | Wyświetla bieżącą datę i godzinę                                |
| `<FromToday_nnn>` | Wyświetla bieżącą datę, plus `nnn` i godzinę, gdzie `nnn` jest liczbą całkowitą.  |
| `<ObjectID> `     | Podstawowy identyfikator zasobu RCDC.                                     |
| `<Attribute_xxx>` | Zwraca określony atrybut, XXX, zasobu podstawowego RCDC. |


## <a name="debug-xml-configuration-files"></a>Debuguj pliki konfiguracji XML

Podczas tworzenia lub modyfikowania plików konfiguracji XML dla RCDC, można zmniejszyć liczbę błędów przez zweryfikowanie kodu XML względem plików XSD przy użyciu edytora, takiego jak Microsoft Visual Studio. Aby uzyskać więcej informacji, zobacz [wprowadzenie do narzędzi XML w programie Visual Studio 2005](https://go.microsoft.com/fwlink/?LinkID=74512).


## <a name="customize-help-files"></a>Dostosuj pliki pomocy

W przypadku tworzenia nowych zasobów i atrybutów możesz chcieć zaktualizować istniejące pliki pomocy w portalu programu FIM o zawartość niestandardowych zasobów. Pliki pomocy w portalu programu FIM są w formacie. htm i mogą być edytowane ręcznie. Aby uzyskać więcej informacji na temat tworzenia atrybutów niestandardowych, zobacz Wprowadzenie do niestandardowego zasobu i zarządzania atrybutami w dokumentacji programu FIM 2010.

>[!IMPORTANT]
>Informacje dotyczące podstaw formatowania lub edytowania kodu HTML nie są zawarte w tym artykule. Użytkownicy powinni wiedzieć, jak edytować pliki HTML.

### <a name="location-of-help-files"></a>Lokalizacja plików pomocy
Wszystkie pliki pomocy dla portalu Microsoft Identity Manager 2016 z dodatkiem SP1 znajdują się w folderze `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html` na serwerze usługi programu MIM.

### <a name="locate-a-specific-help-file"></a>Lokalizowanie określonego pliku pomocy
Wszystkie pliki pomocy dla portalu programu FIM mają nazwę z unikatowym identyfikatorem globalnym (GUID). Aby zlokalizować właściwy plik dla zasobu niestandardowego:

1. W portalu programu FIM Otwórz plik pomocy na stronie portalu, który chcesz dostosować.

2. Kliknij prawym przyciskiem myszy plik pomocy i wybierz polecenie **Właściwości**.

3. Zaznacz i skopiuj `<GUID\>.htm` plik w polu **adres URL** .

4. Przejdź do folderu, w którym są przechowywane pliki pomocy, a następnie wyszukaj plik.


## <a name="add-content-for-attribute-in-existing-grouping-element"></a>Dodaj zawartość dla atrybutu w istniejącym elemencie grupowania
Aby dodać opisową zawartość dla nowego atrybutu w istniejącym elemencie grupującym (Tab):

1. Zidentyfikuj i Znajdź odpowiedni plik pomocy.

2. Za pomocą edytora HTML Otwórz plik.

3. Znajdź lokalizację, w której chcesz dodać zawartość. Zwykle jest to w dodatkowym paragrafie, na przykład:

    `<p xmlns="">A new paragraph with customized information.</p>`

    Może to być również element wstawiony do istniejącej listy, na przykład:

    ```
    <li class="unordered"><b>First Name</b> – The first name of the User.<br>
    <li class="unordered"><b>Last Name</b> - The last name of the User.<br>
    <li class="unordered"><b>Added a new line</b><br>
    ```

## <a name="add-content-for-existing-grouping-element"></a>Dodaj zawartość dla istniejącego elementu grupowania
Większość stron portalu FIM ma wiele elementów grupujących (lub kart), a towarzyszące pliki pomocy mają zakładki z zakładkami, które odnoszą się do każdego elementu grupowania. Zakładki w kodzie HTML są określone w sekcjach. Na przykład jest to kod HTML karty informacje służbowe z pliku pomocy dla strony Tworzenie użytkownika w portalu programu FIM:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Odwołuje się do niego element grupujący **WorkInfo** w pliku XML danych konfiguracyjnych dla **konfiguracji do tworzenia przez użytkownika** RCDC. `\<GUID\>.htm`Nazwa pliku i zakładka są określone w `my:Link` parametrze:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

### <a name="simple-control-samples"></a>Proste przykłady formantów
Ta sekcja zawiera przykłady tworzenia różnych prostych kontrolek pól tekstowych.

Na poniższej ilustracji przedstawiono proste kontrolki pola tekstowego w różnych trybach:

![Proste kontrolki pola tekstowego](media/rcd-configuration-xml-reference/image016.gif)

Poniższy segment kodu tworzy pierwszy formant pola tekstowego, który używa jawnego tekstu dla wszystkich atrybutów i właściwości:

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

Poniższy segment kodu tworzy drugą kontrolkę pole tekstowe, która używa techniki powiązania dynamicznego do łączenia kontrolki z innym źródłem danych:

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

Poniższy segment kodu tworzy trzecią rozwiniętą etykietę i kontrolkę pola tekstowego:

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

Poniższy segment kodu tworzy czwartą wyłączoną kontrolkę pole tekstowe. Chociaż ta kontrolka nie wyświetla widocznej różnicy między wyłączonym stanem a włączonym stanem, użytkownik nie może wprowadzać danych w polu tekstowym.

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


## <a name="individual-controls"></a>Poszczególne kontrolki

Ta sekcja zawiera dokumenty dotyczące poszczególnych kontrolek, które są dostępne w Microsoft Identity Manager 2016 SP1.

### <a name="uocbutton"></a>UocButton

**Nazwa**: UocButton

**Opis**: jest to prosty formant Button, który służy do wyzwalania określonych akcji. Jednak ponieważ nie można określić własnego programu obsługi, korzystanie z tego formantu jest ograniczone.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **Tekst**: Ta właściwość określa tekst, który pojawia się na przycisku. Jest to opcjonalny atrybut typu String. Tekst przyjmuje jawną wartość ciągu.

**Zdarzenia**:

- **OnButtonClicked**: zdarzenie jest emitowane po kliknięciu przycisku.

**Przykład:**

![UocButton — formant](media/rcd-configuration-xml-reference/image017.png)

Poniższy segment XML generuje prosty przycisk kontrolki UocButton:

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

**Opis**: ten formant służy do wyświetlania podpisu strony RCDC. Ten formant jest przeznaczony do użycia tylko jako jeden formant w grupowaniu nagłówka. Korzystanie z niego w dowolnym innym kontekście może spowodować problemy z renderowaniem lub błędy portalu.

**Tryb**: tylko do odczytu (OneWay)

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **MaxHeight:** Ta właściwość określa maksymalną wysokość ikony w sekcji Caption. Ta właściwość jest opcjonalna. Ta właściwość przyjmuje wartość całkowitą w pikselach. Wartość domyślna to 32 pikseli.

**Zdarzenia**:

- Brak zdarzeń dla tego formantu.

**Przykład:**

![UocCaptionControl — formant](media/rcd-configuration-xml-reference/image020.jpg)

Poniższy segment kodu generuje **podpis nagłówka**:

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

Poniższy segment kodu generuje **jawny podpis zawartości**:

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

Poniższy segment kodu generuje napis dynamiczny **wyświetlanej nazwy** :

```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```


### <a name="uoccheckbox"></a>UocCheckBox

**Nazwa**: UocCheckBox

**Opis**: to jest prosta kontrolka pola wyboru. Zalecamy, aby użytkownik powiąże tę kontrolkę z danymi typu Boolean. Ta kontrolka może być używana jako kontrolka tylko do odczytu lub aktualizowalna kontrolka na podstawie danych, z którymi wiąże się.

>[!NOTE]
>W tej wersji przy użyciu kontrolki pole wyboru w trybie edycji do wyświetlania atrybutu logicznego, jeśli atrybut nie ma wcześniej przypisanej wartości, formant zasobu dodaje wartość **false** do atrybutu, gdy kliknięto **przycisk OK** w trybie edycji. Obejście polega na tym, że zawsze utworzysz atrybut Boolean, który zakłada, że nieobecność jest taka sama jak **false**, lub użyć innych kontrolek, takich jak przycisk radiowy dla atrybutów logicznych.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **DefaultValue**: jest to opcjonalna właściwość typu Boolean. Wartość domyślna to false. To pole określa domyślne zachowanie pola wyboru. Można to określić jawnie.

- **Sprawdzono**: jest to opcjonalna właściwość typu Boolean. Wartość domyślna to false. Ta wartość zastępuje właściwość DefaultValue, gdy jest ona obecna wraz z wartością DefaultValue. To pole określa zachowanie pola wyboru. Podobnie jak wartość DefaultValue, można ją określić jawnie lub powiązać z danymi z serwera.

- **Tekst**: jest to opcjonalny atrybut typu String. Tekst jest wyświetlany po prawej stronie pola wyboru. Ta właściwość służy do określania tekstu, który zawiera więcej informacji dla użytkownika końcowego.

**Zdarzenia**:

- **CheckedChanged**: gdy pole wyboru zmieni swój stan, to zdarzenie jest emitowane.

**Przykład:**

W poniższym przykładzie tworzone jest niestandardowe powiązanie między typem zasobu niestandardowego a atrybutem **Isconfigurationtype**. KOD XML jest używany w RCDC typu zasobu niestandardowego.

![UocCheckBox — formant](media/rcd-configuration-xml-reference/image022.png)

Poniższy segment kodu generuje **dynamiczne pole wyboru**, jak pokazano na powyższym rysunku pole wyboru dynamicznego. Ten typ powiązania jest bardziej uniwersalny i przydatny niż jawne pole wyboru. Ten atrybut musi należeć do bieżącego typu zasobu.

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

**Nazwa**: UocCommonMultiValueControl

**Opis**: jest to wielowierszowa kontrolka pola tekstowego, która obsługuje specjalne formatowanie ciągu. Każda wartość wśród wpisów wielowartościowych jest oddzielona od siebie średnikami (;) lub podział wiersza w polu tekstowym. Zalecamy powiązanie tej kontrolki z danymi typów wielowartościowych, krótkich ciągów i liczb całkowitych. Ten formant obsługuje tryb tylko do odczytu i tryb aktualizowalny.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **Typ danych**: jest to wymagany atrybut typu String. Można ją określić jako **ciąg, liczbę całkowitą** lub typ **DateTime** jawnie. Można również powiązać atrybut z właściwością **typu** Schema. Typ referencyjny wielowartościowego powinien być obsługiwany przez **UOCListView** lub **UOCIdentityPicker**. Wartość logiczna wielowartościowego nie jest obsługiwanym typem danych.

- **Wiersze**: jest to opcjonalny atrybut typu Integer. Można zdefiniować wysokość pola w postaci liczby znaków. Wartość domyślna to 1.

- **Kolumny**: jest to opcjonalny atrybut typu Integer. Można zdefiniować, ile pola ma zawierać liczbę znaków. Wartość domyślna to 20.

- **Wartość**: jest to opcjonalny atrybut typu String. Ten atrybut można powiązać tylko ze źródłem danych.

**Zdarzenia**:

- **ValueListChanged**: to zdarzenie jest wyzwalane, gdy bieżąca wartość w formancie zostanie zmieniona.

**Przykład:**

W poniższym przykładzie atrybut ciągu wielowartościowego o nazwie **AMultiValueString** jest tworzony i powiązany z niestandardowym typem zasobu. Ten przykład działa tylko po utworzeniu tego powiązania.

![UocCommonMultiValueControl — formant](media/rcd-configuration-xml-reference/image024.jpg)

Poniższy segment kodu generuje formant **UocCommonMultiValueControl** :

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

**Opis**: przypomina formant pola tekstowego, ale **Opis** akceptuje tylko określony format. W trybie tylko do odczytu, wygląda jak etykieta. Aby uzyskać format obsługiwanego ciągu wejściowego, zobacz Właściwość **DateTimeFormat** w tej sekcji.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **DateTimeFormat**: jest to opcjonalny atrybut typu String. Obsługiwane formaty to **DateTime** i **DateOnly**. Wartość domyślna jest ustawiana na format **DateTime** .

  - **DateTime**: atrybut jest sformatowany jako mm/dd/rrrr hh: mm: SS.
  - **DateOnly**: atrybut jest sformatowany jako mm/dd/rrrr.

    >[!NOTE]
    >Obsługiwane są zarówno formaty **DateTime** , jak i **DateOnly** , niezależnie od użytkownika, który określa różnicę.
    >

- **Wartość**: jest to opcjonalny atrybut typu String. Ten atrybut jest powiązany ze źródłem danych zasobów. Wartość tego atrybutu musi być zgodna z prawidłowym formatem DateTime.

**Zdarzenia**:

- **DateTimeChanged**: gdy wartość daty i godziny ulegnie zmianie, wystąpi zdarzenie.

**Przykład:**

![UocDateTimeControl — formant](media/rcd-configuration-xml-reference/image027.jpg)

Poniższy segment kodu generuje pierwszy formant **DateTime** .

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

Poniższy segment kodu generuje drugą kontrolkę **DateTime** . Jeśli użyto przykładowego kodu w sekcji źródła danych, atrybut **ExpirationTime** jest powiązany z wszystkimi typami zasobów. Z tego względu można użyć następującego kodu:

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

**Opis**: to jest prosta kontrolka pola rozwijanego. Ten formant służy do wybierania opcji ze zdefiniowanego zestawu opcji. Typy danych String, Integer, DateTime i Boolean są dobrymi kandydatami dla tej kontrolki.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **ValuePath**: właściwość do pobrania atrybutu Value z ustawieniem właściwości ItemSource. Gdy ustawieniem właściwości ItemSource jest określony jako niestandardowy, ścieżka wartości jest ustawiona na wartość. Jest on powiązany z polem Value z elementu Option, zgodnie z opisem w tej sekcji.

- **CaptionPath**: właściwość do pobrania atrybutu Value z ustawieniem właściwości ItemSource. Gdy ustawieniem właściwości ItemSource jest określony jako niestandardowy, ścieżka wartości jest ustawiona na Caption. Wiąże się z polem Caption z elementu Option, zgodnie z opisem w tej sekcji.

- **HintPath**: właściwość do pobrania atrybutu Value z ustawieniem właściwości ItemSource. Gdy ustawieniem właściwości ItemSource jest określony jako niestandardowy, ścieżka wartości jest ustawiana na wskazówkę. Wiąże się z polem wskazówki z elementu Option, zgodnie z opisem w tej sekcji.

- **Ustawieniem właściwości ItemSource**: Kolekcja ListControlItems, która definiuje opcje na liście. Użytkownik może jawnie ustawić ten element na niestandardowy i użyć elementu Option, zgodnie z opisem w tej sekcji, aby określić wartość ciągu.

- **SelectedValue**: aktualnie wybrana wartość. Jest to wymagana właściwość typu String. Ta właściwość jest powiązana z danymi ciągu ze źródła danych.

**Zdarzenia**:

- **SelectedIndexChanged.**: zdarzenie występuje, gdy zmieni się zaznaczenie w polu listy rozwijanej.

**Opcje**:

Aby uzyskać strukturę elementu **Options** , zobacz <a href="#options-element">Opcje elementu</a>.

- **Wartość**: wartość pojedynczego elementu Options może być ustawiona na dowolny ciąg, który jest prawidłowym danymi wejściowymi źródła danych, z którym jest powiązany formant.

- **Caption**: podpis może być dowolną wartością ciągu.

- **Wskazówka**: Wskazówka może być dowolną wartością ciągu.

**Przykład:**

![UocDropDownList — formant](media/rcd-configuration-xml-reference/image030.jpg)


![Opcje w kontrolce UocDropDownList](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
>Aby wykonać przykładowe działanie, należy powiązać istniejący **zakres** atrybutów typu String z niestandardowym typem zasobu, do którego odnosi się RCDC.


Poniższy segment kodu generuje listę rozwijaną:

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

**Nazwa**: UocFileDownload

**Opis**: Ta kontrolka zawiera hiperlink. Po kliknięciu hiperlinku zostanie wyświetlona strona Zapisz plik systemu Windows. Użytkownik może zapisać plik na dysku lokalnym. Opcja Otwórz jest również obsługiwana, jeśli program Internet Explorer może renderować format pliku. Zalecane typy danych do użycia z tym formantem to sformatowane ciągi (XML) i typy binarne.

>[!NOTE]
>W tej wersji programu Microsoft Identity Manager 2016 z dodatkiem SP1 użytkownik musi zamknąć okno programu Internet Explorer, w którym otworzyła plik, a następnie odświeżyć stronę. Po odświeżeniu okna programu Internet Explorer użytkownik może uruchomić pobieranie, aby zapisać lub otworzyć ten sam plik ponownie w pierwotnym oknie.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **Tekst**: jest to opcjonalny atrybut typu String, który definiuje tekst hiperlinku. Użytkownik może określić jawny ciąg dla tej właściwości.

- **Wartość**: jest to atrybut wymagany. Określa powiązanie atrybutu na serwerze, którego zawartość ma zostać pobrana.

- **PromptedFileName**: jest to opcjonalny atrybut typu String. Jest to nazwa pliku, który jest sugerowany dla użytkownika podczas zapisywania pobranego pliku.

- **ContentType**: jest to wymagany atrybut typu String. Jest to typ pliku, w którym są zapisywane dane. Tekst lub binarne są dwoma obsługiwanymi opcjami ciągu. Jeśli jest to tekst, wartość zwracana jest uznawana za długi ciąg. W przeciwnym razie w przypadku wartości binarnej wartość zwracana jest traktowana jako Byte []. W przypadku wybrania tekstu użytkownik może, jako opcję, dodać sufiks, aby określić typ formatowania, w którym znajduje się tekst. Na przykład tekst/XML jest prawidłowy.

>[!NOTE]
>Gdy wartość, która jest powiązana z tą kontrolką, jest pusta, formant nie ma hiperłącza, które ma być używane do wyzwalania akcji pobierania. Dzieje się tak, ponieważ nie ma niczego do pobrania.

**Zdarzenia**:

- Brak zdarzeń dla tego formantu.

**Przykład:**

![UocFileDownload — formant](media/rcd-configuration-xml-reference/image035.png)

>[!NOTE]
>Przed przekazaniem tego pliku przykładowego użytkownik musi utworzyć powiązanie między typem zasobu niestandardowego a istniejącym atrybutem ConfigurationData.

Poniższy segment kodu generuje formant pobierania pliku:

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

**Opis**: Ta kontrolka zawiera pole tekstowe, w którym jest wyświetlana lokalizacja pliku lokalnego, który ma zostać przekazany, przycisk Przeglądaj plik i przycisk Przekaż. Gdy użytkownik końcowy kliknie przycisk Przeglądaj, zostanie wyświetlone okno Otwórz plik systemu Windows. Użytkownik końcowy może wybrać jeden plik na dysku lokalnym do przekazania. Po wybraniu pliku w polu tekstowym zostanie wyświetlona lokalizacja pliku. Po kliknięciu przycisku Przekaż plik zostanie przekazany do lokalnego źródła danych po stronie klienta. Zawartość pliku nie została jeszcze przesłana do serwera. Zalecane typy danych do użycia w tym formancie są następujące: sformatowane ciągi (XML) lub typy binarne.

>[!NOTE]
>Nie ma oznak postępu przekazywania lub stanu. Gdy plik zostanie przekazany do lokalnego źródła danych, pole tekstowe jest wyczyszczone.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **Wartość**: jest to atrybut wymagany. Określa powiązanie atrybutu schematu na serwerze, do którego dane są przekazywane.

- **ContentType**: jest to opcjonalny atrybut typu String. Jest to typ danych, na który jest zapisywany plik na serwerze. Można ustawić na wartość Text lub Binary. Gdy brakuje właściwości, wartość domyślna to Binary.

- **MaxFileSize**: jest to opcjonalny atrybut typu String. MaxFileSize definiuje, jak duży rozmiar przekazanego pliku. Domyślnie, jeśli brakuje właściwości, maksymalny rozmiar wynosi 1 megabajt (MB).

- **PromptedForNoValue**: jest to opcjonalny atrybut typu String. Definiuje tekst, który pojawia się dla użytkownika, gdy plik nie jest przekazywany.

**Zdarzenia**:

- **FileUploaded**: to zdarzenie jest emitowane po pomyślnym przekazaniu pliku.

**Przykład:**

![UocFileUpload — formant](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
>Aby wykonać następujący przykładowy kod, należy utworzyć nowy atrybut typu binarnego o nazwie ABinaryAttribute, a następnie utworzyć nowe powiązanie między typem zasobu niestandardowego a tym atrybutem.

Poniższy segment kodu generuje formant przekazywania:
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

**Nazwa**: UocFilterBuilder

**Opis**: jest to złożona kontrolka, która umożliwia użytkownikowi renderowanie wyrażenia XPath programu MIM 2016. Niektóre wyrażenia XPath nie są obsługiwane. Aby uzyskać informacje o sposobach korzystania z konstruktora filtrów, zobacz Pomoc dla konstruktora filtrów.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **PermittedObjectTypes**: definiuje listę typów zasobów, które mają być wyświetlane w instrukcji SELECT konstruktora filtru. Informacje o sposobach korzystania z konstruktora filtrów można znaleźć w pomocy programu Filter Builder. Ciąg jest w formacie ResourceTypeA, ResourceTypeB, gdzie każdy typ zasobu jest rozdzielony przecinkami ",".

- **Wartość**: jest to wartość, z którą jest renderowany Konstruktor filtrów. Obsługiwane jest tylko powiązanie z danymi typu String, które zawierają wyrażenie XPath. Atrybut Filter jest zalecanym atrybutem dla powiązania tej kontrolki.

- **PreviewButtonVisible**: jest to opcjonalna właściwość typu Boolean. Gdy ta właściwość ma wartość FAŁSZ, użytkownik nie zobaczy przycisku podglądu. Wartość domyślna to true. Ten przycisk może być używany w połączeniu z kontrolką widoku listy, aby wyświetlić podgląd wyników wyrażenia XPath.

- **ExcludeGroupMembership**: jest to właściwość logiczna. Gdy ta właściwość ma wartość true, nie można utworzyć filtru, który używa \<Reference Attribute\> (na przykład ResourceID) jest członkiem \<Group object\> . Innymi słowy, gdy ta właściwość ma wartość true, nie można utworzyć filtru, który używa katalogu członkostwa w grupie.

- **PreviewButtonCaption**: jest to opcjonalny ciąg. Gdy PreviewButtonVisible ma wartość true, można użyć tej właściwości, aby nadać przyciskowi dostosowany tekst. Tekst jest wyświetlany na przycisku podglądu.

**Zdarzenia**:

- **OnFilterChanged**: to zdarzenie jest wyzwalane po zmianie zawartości konstruktora filtrów.

**Przykład:**

![UocFilterBuilder — formant](media/rcd-configuration-xml-reference/image044.png)

Następujący przykładowy kod zawiera kontrolkę UOCLabel, prosty Konstruktor filtrów z PermittedObjectTypes i widok listy podglądów. Wskaż Właściwość ListFilter widoku listy i Właściwość Filter Builder wartość na ten sam atrybut źródła danych, aby połączyć dwa.

>[!NOTE]
>Przed użyciem tego przykładowego kodu Utwórz nowe powiązanie między istniejącym atrybutem filtru a niestandardowym typem zasobu.

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


### <a name="uochtmlsummary"></a>UocHtmlSummary

**Nazwa**: UocHtmlSummary

**Opis**: można użyć tego formantu, aby zdefiniować stronę podsumowania na stronie RCDC. Ta strona podsumowania zostanie wyświetlona po przesłaniu żądania przez użytkownika końcowego. Ta kontrolka może być używana tylko w grupowaniu podsumowującym i musi być jedyną kontrolką. Zdecydowanie zalecamy korzystanie z dostarczonego przykładowego kodu.

>[!NOTE]
>Ta kontrolka nie została szczegółowo przetestowana.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **ModificationsXml**: Ta właściwość musi być sformatowana jako {Binding Source = Delta, Path = DeltaXml}, gdzie Delta jest zdefiniowana w elemencie ObjectDataSource nagłówka Configuration.

- **TransformXsl**: Ta właściwość jest sformatowana jako {Binding Source = SummaryTransformXsl, Path =/}, gdzie summaryTransformXsl jest zdefiniowana w nagłówku XmlDataSource nagłówka konfiguracji.

**Zdarzenia**:

- Brak zdarzeń dla tego formantu.

**Przykład:**

Przykład tej kontrolki zawiera przykład grupowania podsumowującego w sekcji grupowanie elementu tego dokumentu.


### <a name="uochyperlink"></a>UocHyperLink

**Nazwa**: UocHyperLink

**Opis**: jest to prosta kontrolka Hyperlink. Możesz użyć tego formantu, aby wyświetlić informacje jako hiperlink.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **ObjectReference**: jest to opcjonalna właściwość typu odwołania. Jeśli do danego zasobu jest przywoływany identyfikator GUID, który jest zdefiniowany w tej właściwości, hiperłącze umożliwia użytkownikowi końcowemu uzyskanie dostępu do zasobu. Jest to wzajemnie wykluczające się z właściwością NavigateUrl.

- **Tekst**: jest to opcjonalna właściwość typu String. Ta właściwość służy do definiowania tekstu wyświetlanego jako hiperlink.

- **NavigateUrl**: jest to opcjonalna właściwość typu String. Ta właściwość służy do definiowania adresu URL pełnej ścieżki, z którym łączy się hiperłącze. Jest to wzajemnie wykluczające się z właściwością ObjectReference.

**Zdarzenia**:

- Brak zdarzeń dla tego formantu.

**Przykład:**

![UocHyperLink — formant](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
>Wymagany jest prawidłowy identyfikator GUID zasobu, do którego ma zostać utworzone połączenie. W takim przypadku drugie hiperłącze jest generowane z prawidłowym identyfikatorem GUID. Pierwszym z nich może być dowolna witryna sieci Web.

Poniższy segment kodu generuje hiperłącze przekierowania:

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

Poniższy segment kodu generuje hiperłącze odwołujące się do zasobu. Jawne odwołanie można zastąpić wyrażeniem {Binding Source = Object, Path = Creator}, aby powiązać je ze źródłem danych. Ten element może być prawidłowy tylko wtedy, gdy istnieje Menedżer zasobów i jest wartością typu odwołania.

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

**Nazwa**: UocIdentityPicker

**Opis**: Ta kontrolka składa się z opcjonalnego pola rozpoznawania i okna przeglądania. Pole opcjonalne rozwiązanie składa się z opcjonalnego pola tekstowego, aby wprowadzić tożsamość, przycisk Rozwiąż, aby rozwiązać tożsamość, i przycisk przeglądania w celu wyświetlenia monitu o wyskakujące okienko przeglądania. Okno Przeglądanie umożliwia użytkownikowi Wybieranie tożsamości za pomocą kontrolki widoku listy. Wybrana tożsamość z okna przeglądania jest odzwierciedlona w polu rozwiązywanie.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **UsageKeywords**: jest to opcjonalna Właściwość ciągu. Można zdefiniować listę zakresów wyszukiwania, które mają być używane w selektorze zasobów, dostarczając listę słów kluczowych użycia, które są obsługiwane przez strukturę SearchScopeConfiguration, gdzie każde słowo kluczowe jest oddzielone apostrofem (').

- **Filtr**: jest to opcjonalna Właściwość ciągu. Użytkownik dostarcza wyrażenie XPath, aby określić zakres selektora zasobów, aby wyświetlał tylko elementy mieszczące się w określonym zakresie. Ta właściwość wzajemnie się wykluczają z właściwością UsageKeywords. Gdy zakres wyszukiwania jest stosowany, ta właściwość nie ma żadnego skutku.

- **ResultObjectType**: jest to opcjonalna Właściwość ciągu. Typ zasobu służy do renderowania zasobów w podręcznym oknie dialogowym. Jest on używany z filtrem, aby pomóc selektorowi tożsamości identyfikować, jaki typ zasobów jest zwracany przez filtr, i odpowiednio renderować dane. Ta właściwość wzajemnie się wykluczają z właściwością UsageKeywords. Gdy zakres wyszukiwania jest stosowany, nie ma to żadnego skutku. Ciąg, który jest akceptowany dla tej właściwości, jest dowolną pojedynczą, prawidłową nazwą typu zasobu, na przykład osoba. Gdy filtr będzie zwracać wiele typów zasobów, zostanie użyty zasób.

- **PreviewTitle**: to jest tytuł podglądu używany w widoku listy. Aby uzyskać informacje na temat tej właściwości, zobacz sekcję UocListView.

- **ListViewTitle**: jest to opcjonalna Właściwość ciągu. Ta właściwość służy do definiowania tekstu wyświetlanego na górze widoku listy jako tytuł.

- **Wartość**: jest to opcjonalna Właściwość ciągu. Zaleca się, aby powiązać ten element z atrybutem schematu w celu połączenia wartości ze źródłem danych.

- **Tryb**: jest to opcjonalna Właściwość ciągu. Ta właściwość służy do definiowania, czy można wybrać jedną wartość z selektora tożsamości lub wybrać wiele tożsamości. SingleResult i MultipleResult są dozwolonymi wartościami. Domyślnie jest to SingleResult.

- **ObjectTypes**: jest to opcjonalna właściwość typu String. Można zdefiniować listę typów zasobów, które użytkownik końcowy może rozpoznać w polu Rozwiązywanie tożsamości. Lista składa się z listy nazw typów zasobów rozdzielonych przecinkami ",".

- **AttributesToSearch**: jest to opcjonalna właściwość typu String. Można zdefiniować listę atrybutów, która będzie używana do rozpoznawania elementu w selektorze tożsamości, gdzie list jest listą atrybutów schematu oddzielonych przecinkami ",". Na przykład jeśli AttributesToSearch jest ustawiona na `DisplayName, Alias` , użytkownik może przeszukiwać elementy za pomocą `DisplayName = \<search value\>` lub `Alias=\<search value\>` . Nazwy atrybutów wprowadzone w tym miejscu powinny być prawidłowymi atrybutami dla docelowych typów zasobów źródła danych określonego we właściwości Value. Docelowe typy zasobów można znaleźć w polu ObjectTypes. Wszystkie atrybuty muszą być prawidłowe dla wszystkich typów zasobów, które są cytowane w polu ObjectTypes.

- **ColumnsToDisplay**: jest to opcjonalna właściwość typu String. Użytkownik udostępnia listę nazw atrybutów schematu oddzielonych przecinkami ",". Atrybuty, które są zdefiniowane w tym miejscu, tworzą kolumnę widoku listy w selektorze tożsamości.

- **Wiersze**: jest to opcjonalna Właściwość Integer. Działa tylko wtedy, gdy tryb jest ustawiony na MultipleResult. Ta właściwość służy do ustawiania wysokości pola tekstowego Rozpoznaj do danego rozmiaru w jednostkach znakowych.

- **MainSearchScreenText**: jest to opcjonalna właściwość typu String. Jest to dostosowany tekst, który pojawia się, gdy wyszukiwanie zostanie uruchomione w oknie przeglądania.

**Zdarzenia**:

- **SelectedObjectChanged**: to zdarzenie jest emitowane, gdy użytkownik zmieni wybrane zasoby.

**Przykład:**

![Kontrolka UocIdentityPicker w trybie SingleResult](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
>Aby ta przykład działała, należy utworzyć nowe powiązanie między atrybutem Menedżera i dowolnym niestandardowym typem zasobu, którego dotyczy ten kod XML.

Poniższy segment kodu generuje selektor tożsamości w trybie SingleResult przy użyciu właściwości Filter i ResultObjectType w ramach RCDC:

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

Na poniższej ilustracji przedstawiono selektor tożsamości w trybie MultipleResult:

![Kontrolka UocIdentityPicker w trybie MultipleResult](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
>Aby ten przykładowy kod działał, należy powiązać atrybut ExplicitMember (atrybut odwołania wielowartościowego) z niestandardowym typem zasobu. Utwórz zakresy wyszukiwania przy użyciu właściwości UsageKeyword ustawionej na osoby i grupę.

Poniższy segment kodu tworzy selektor tożsamości w trybie MultipleResult:

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

**Nazwa**: UocLabel

**Opis**: jest to prosta, tylko do odczytu kontrolka etykieta tekstu. Zaleca się, aby ten formant był używany do wyświetlania danych tylko do odczytu.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **Tekst**: jest to atrybut typu String. Tę właściwość można zdefiniować przez podanie jawnej wartości ciągu lub powiązanie jej ze źródłem danych. Przykładowe powiązanie, które przypisuje wartość tej właściwości, to {Binding Source = Object, Path = \<valid attribute name\> .

Przykład kontrolki UocLabel można znaleźć w sekcji proste kontrolki w prostych przykładach kontrolek.


### <a name="uoclistview"></a>UocListView

**Nazwa**: UocListView

**Opis**: jest to zaawansowana kontrolka widoku listy. Składa się z prostego widoku listy, opcjonalnego prostego wyszukiwania, opcjonalnej zaawansowanej kontrolki wyszukiwania, opcjonalnego pola podglądu wyboru i paska przycisku akcji. Opcjonalne proste wyszukiwanie składa się z zakresu wyszukiwania i prostego pola tekstowego wyszukiwania. Zaawansowany formant wyszukiwania jest konstruktorem filtru. Widok listy zawiera wstępnie renderowaną listę zasobów. Może również wyświetlać wyniki wyszukiwania pochodzące z kontrolek wyszukiwania w tym formancie. Pasek przycisku akcji definiuje, jakie akcje można wykonać w zależności od wyboru w widoku listy. Pole podglądu zaznaczenia pokazuje, które elementy są wybrane z widoku listy.

>[!IMPORTANT]
>UocListView nie działa z atrybutami odwołania jednowartościowego. Może być używany tylko z atrybutami odwołań wielowartościowych. Aby uzyskać atrybuty odwołania jednowartościowego, zobacz UocIdentityPicker w tym dokumencie.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **SelectedValue**: jest to opcjonalna właściwość typu String, która jest powiązana z atrybutem odwołania wielowartościowego akceptującym listę ciągów sformatowanych za pomocą identyfikatora GUID.

- **PageSize**: jest to opcjonalna Właściwość Integer. Użytkownik może określić, ile wpisów mieści się na jednej stronie w kontrolce widoku listy. Wartość domyślna to 10 wpisów. Wszystkie dodatnie liczby całkowite są prawidłowe.

- **UsageKeyword**: jest to opcjonalna właściwość typu String. Użytkownik może określić listę słów kluczowych, które definiują zakres wyszukiwania używany w kontrolce wyszukiwania Wyświetl listę. Istnieją zasoby zakresu wyszukiwania na serwerze programu FIM 2010. Atrybut w strukturze SearchScopeConfiguration o nazwie UsageKeyword jest używany do grupowania zestawu zakresów wyszukiwania. Widok listy wykorzystuje tę listę słów kluczowych. Każde słowo kluczowe jest oddzielone przecinkiem (,). Jest to słowo kluczowe użycie używane w odpowiednim zakresie wyszukiwania, który ma być wyświetlany w tym widoku listy. Ta wartość działa tylko wtedy, gdy właściwość ShowSearchControl ma wartość true.

- **SearchControlAutoPostback**: jest to opcjonalna właściwość logiczna. Ustaw wartość tej właściwości na true, aby przeprowadzić autozwrotnie po wyzwoleniu wyszukiwania. Domyślnie SearchControlAutoPostback jest ustawiona na false.

- **EmptyResultText**: jest to opcjonalna właściwość typu String. Domyślnie jest ono ustawione na brak elementów, ale można je ustawić na dowolną wartość ciągu. Ten tekst jest wyświetlany, gdy wynik wyszukiwania jest pusty.

- **ButtonHeight**: jest to opcjonalna właściwość typu Integer. Ustaw wartość tej właściwości na dowolną dodatnią wartość całkowitą. Ta właściwość definiuje wysokość przycisków na pasku akcji w pikselach. Wartość domyślna to 32 pikseli.

- **ButtonWidth**: jest to opcjonalna właściwość typu Integer. Ustaw wartość tej właściwości na dowolną dodatnią wartość całkowitą. Ta właściwość określa szerokość przycisków na pasku akcji (w pikselach). Wartość domyślna to 32 pikseli.

- **CaptionImageMaxHeight**: jest to opcjonalna właściwość typu Integer. Ustaw wartość tej właściwości na dowolną dodatnią liczbę całkowitą. Ta właściwość definiuje maksymalną wysokość ikony etykiety opcjonalnej. Wartość domyślna to 32 pikseli.

- **CaptionImageMaxWidth**: jest to opcjonalna właściwość typu Integer. Ustaw wartość tej właściwości na dowolną dodatnią liczbę całkowitą. Ta właściwość definiuje maksymalną szerokość ikony etykiety opcjonalnej. Wartość domyślna to 32 pikseli.

- **CaptionImageUrl**: jest to opcjonalna właściwość typu String. Ta właściwość określa adres URL, który łączy się z obrazem wyświetlanym jako obraz podpisu.

- **PreviewTitle**: jest to opcjonalna właściwość typu String. Ta właściwość służy do definiowania tekstu, który pojawia się w górnej części okna podglądu zaznaczenia.

- **EnableSelection**: jest to opcjonalna właściwość typu Boolean. Ta właściwość służy do określenia, czy widok listy jest w trybie zaznaczania. Jeśli widok listy jest w trybie wyboru, kolumna pól wyboru pojawia się w kolumnie z lewej strony widoku listy, a w dolnej części widoku listy pojawia się okno podglądu zaznaczenia. Wartość domyślna tej właściwości jest ustawiona na true.

- **SingleSelection**: jest to opcjonalna właściwość typu Boolean. Jeśli tryb wyboru jest włączony dla widoku listy, ustawienie tej wartości na true spowoduje, że użytkownik końcowy zaznaczy tylko jeden element z listy. Domyślnie wartość tej właściwości jest równa false. Oznacza to, że domyślnie użytkownik końcowy może wybrać wiele elementów z listy.

- **RedirectURL**: jest to opcjonalna właściwość typu String. Użyj tej właściwości, aby określić stronę do przekierowania po kliknięciu elementu z linkiem na liście. Ten adres URL może zawierać symbole zastępcze, które są zastępowane rzeczywistą wartością podczas środowiska uruchomieniowego. Symbole zastępcze są następujące:

    - {0} objectType
    - {1} Obiektu
    - {2} Nazwa

- **ShowTitleBar**: jest to opcjonalna właściwość typu Boolean. Użyj tej właściwości, aby określić, czy pasek tytułu ma być widoczny. Wartość domyślna tej właściwości to false.

- **ShowActionBar**: jest to opcjonalna właściwość typu Boolean. Użyj tej właściwości, aby określić, czy obszar paska akcji powinien być widoczny. Wartość domyślna tej właściwości to true.

- **ShowPreview**: jest to opcjonalna właściwość typu Boolean. Użyj tej właściwości, aby określić, czy obszar podglądu powinien być widoczny. Wartość domyślna tej właściwości to true.

- **ShowSearchControl**: jest to opcjonalna właściwość typu Boolean. Użyj tej właściwości, aby określić, czy formant wyszukiwania ma być widoczny. Wartość domyślna tej właściwości to true.

- **ResultObjectType**: jest to opcjonalna właściwość typu String. Ta właściwość umożliwia określenie oczekiwanego typu obiektu wyników wyszukiwania. Wartość domyślna tej właściwości to zasób. Jeśli wynik wyszukiwania zawiera wiele typów zasobów, ta wartość powinna być określona jako zasób.

- **ColumnsToDisplay**: jest to właściwość opcjonalna. Użyj tej właściwości, aby określić atrybuty, które mają być wyświetlane w widoku listy jako kolumny. Wartość domyślna tej właściwości to DisplayName, ResourceType. Każda kolumna jest reprezentowana przez nazwę systemową atrybutu. Każda kolumna jest oddzielona przecinkami ",". Nie trzeba określać wartości tej właściwości, gdy widok listy jest używany w trybie zaznaczania. W trybie wyboru ustawienie kolumny pochodzi z atrybutu SearchScopeColumn zakresu wyszukiwania, który jest aktualnie wybrany.

- **ListFilter**: jest to opcjonalna właściwość typu String. Jest to wyrażenie XPath używane do renderowania widoku listy i działa tylko wtedy, gdy właściwość ShowSearchControl ma wartość false. Gdy ta wartość jest określona, widok listy używa tej wartości właściwości dla zapytań i widok listy nie jest w trybie wyboru. Filtr może być powiązany z atrybutem ciągu zasobu:

    `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>`

    lub być ciągiem zawierającym wstępnie zdefiniowaną zmienną środowiskową:

    `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

- **Targetattribute**: jest to przestarzała właściwość. Jej wartość powinna być nazwą systemową z wielowartościowym atrybutem, którego dotyczy odwołanie. Zalecamy, aby ta właściwość nie była używana więcej. Na przykład w przypadku zarządzania grupami zamiast korzystania z:

    `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>`

    Używanych

    `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>`

- **ItemClickBehavior**: jest to opcjonalna właściwość typu String. Użyj tej właściwości, aby określić, czy element widoku listy ma być wyzwalany na serwerze, czy ma być wyświetlany widok szczegółów elementu. Obsługiwane są dwie wartości opcji: ModelessDialog i Server. Wartość domyślna to ModelessDialog.

- **SearchOnLoad**: jest to opcjonalna właściwość typu Boolean określająca, czy kontrolka widoku listy powinna wykonywać zapytania dotyczące ładowania. Ta właściwość ma zastosowanie tylko wtedy, gdy widok listy jest w trybie zaznaczania. Wartość domyślna tej właściwości to true. Można ją wyłączyć, jeśli oczekujesz, że użytkownik zwykle wpisze tekst do wyszukiwania, aby uzyskać zrozumiały wynik. W takim przypadku widok listy początkowo pokazuje komunikat informujący użytkownika, jak wykonać wyszukiwanie. Tekst może być dostosowany przez następujące właściwości:

- **MainSearchScreenText**: Ta opcjonalna właściwość typu String ma zastosowanie tylko wtedy, gdy SearchOnload ma wartość true. Ta właściwość może służyć do dostosowywania tekstu wyświetlanego w środku widoku listy, gdy widok listy nie jest automatycznie przeszukiwany. Wartość domyślna tej właściwości polega na znalezieniu zasobów przy użyciu wyszukiwania, jak opisano wcześniej. Możesz określić wartość, aby tekst był bardziej istotny dla Twojego scenariusza.

- **SubSearchScreenText**: Ta opcjonalna właściwość typu String służy do dostosowywania tekstu, który pojawia się po właściwości **MainSearchScreenText** . Zwykle nie trzeba określać wartości tej właściwości, chyba że chcesz dodać dodatkowe instrukcje dotyczące korzystania z widoku listy.

**Zdarzenia**:

- Brak zdarzeń dla tego formantu.

**Przykład:**

Aby zapoznać się z przykładami używania widoku listy wraz z kontrolką UocFilterBuilder jako listą zapoznawczą, zobacz przykłady UocFilterBuilder we wcześniejszej części tego dokumentu. UocListView można również użyć bez konstruktora filtrów.


### <a name="uocnumericbox"></a>UocNumericBox

**Nazwa**: UocNumericBox

**Opis**: to proste pole tekstowe, które przyjmuje tylko wartości całkowite. Ten formant obsługuje tryb tylko do odczytu i tryb aktualizowalny.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **MaxValue**: jest to opcjonalna właściwość typu Integer. Ta właściwość służy do definiowania weryfikacji po stronie klienta dla kontrolki. Wartość wprowadzana przez użytkownika końcowego nie może przekroczyć tej wartości. Możesz wprowadzić jawną liczbę całkowitą lub powiązać ją z danymi całkowitymi ze źródła danych, używając {Binding Source = Schema, Path = IntegerMaximum}.

- **MinValue**: jest to opcjonalna właściwość typu Integer. Ta właściwość służy do definiowania weryfikacji po stronie klienta dla kontrolki. Wartość wprowadzana przez użytkownika końcowego nie może być mniejsza niż ta wartość. Możesz wprowadzić jawną liczbę całkowitą lub powiązać ją z danymi całkowitymi ze źródła danych, używając {Binding Source = Schema, Path = IntegerMinimum}.

- **DefaultValue**: jest to opcjonalna właściwość typu Integer. Ta właściwość służy do definiowania wartości domyślnej kontrolki, jeśli formant jest używany do tworzenia nowych danych. Tę wartość można jawnie ustawić tylko jako statyczną liczbę całkowitą.

- **Wartość**: jest to opcjonalna właściwość typu Integer. Gdy powiążesz ten element z danymi typu Integer ze źródła danych, wartość tego atrybutu jest wyświetlana podczas ładowania strony, a następnie jest zapisywana w źródle danych po utworzeniu.

**Zdarzenia**:

- **TextChanged**: to zdarzenie jest emitowane, gdy bieżąca wartość wewnątrz formantu zostanie zmieniona.

**Przykład:**

![UocNumericBox — formant](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
>Poniższy przykładowy kod generuje pierwsze pole liczbowe. Pole liczbowe nie jest połączone ze źródłem danych ani żadnymi informacjami o schemacie.

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

Poniższy przykładowy kod generuje drugie pole liczbowe.

>[!NOTE]
>Aby ta przykład działała, należy najpierw utworzyć nowy atrybut typu Integer o nazwie AnIntegerAttribute i powiązać go z niestandardowym typem zasobu.

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

**Nazwa**: UocPictureBox

**Opis**: ten formant służy do renderowania danych obrazu, typu binarnego. Zalecamy, aby ta kontrolka była używana z danymi typu binarnego. Obraz może być renderowany przy użyciu podanego adresu URL obrazu, danych typu binarnego lub źródła atrybutu zawierającego dane typu obraz.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **ImageUrl**: jest to opcjonalna właściwość typu String. Wprowadź adres URL obrazu docelowego.

- **MaxHeight**: jest to opcjonalna właściwość typu String. Definiuje maksymalną wysokość obrazu, który ma być renderowany w pikselach.

- **MaxWidth**: jest to opcjonalna właściwość typu String. Definiuje maksymalną szerokość obrazu, który ma być renderowany w pikselach.

- **ImageData**: jest to właściwość typu binarnego. Ta właściwość służy do powiązania źródła danych z wyświetlanym obrazem. Powiązane źródło danych musi mieć wartość binarną. Możesz również użyć tego pola, aby jawnie ustawić obraz, dostarczając dane w formacie Byte [].

- **ImageResource**: jest to opcjonalna właściwość typu binary.

- **AlternativeText**: jest to opcjonalna właściwość typu String. Ta właściwość jest wyświetlana jako alternatywny tekst, gdy nie można wyświetlić obrazu.

**Zdarzenia**:

- Brak zdarzeń dla tego formantu.

**Przykład:**

>[!NOTE]
>Aby użyć tego przykładu, musisz mieć istniejące powiązane dane obrazu z kontrolką.

Poniższy segment kodu generuje formant pola obrazu, który wiąże źródło danych z kontrolką:

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

Poniższy segment kodu generuje formant pola obrazu, który wiąże obraz adresu URL z kontrolką:

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

**Nazwa**: UocRadioButtonList

**Opis**: jest to prosta lista przycisków opcji. Opcje te wykluczają się wzajemnie na tej liście. Ta kontrolka jest zalecana, gdy użytkownicy mają pięć lub mniej opcji do wyboru. W przeciwnym razie zalecane jest UOCListView.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **ValuePath**: ścieżka wartości jest ustawiona na wartość. Jest on powiązany z polem Value z elementu Option, zgodnie z opisem w tej sekcji.

- **CaptionPath**: ścieżka wartości jest ustawiona na Caption. Jest on powiązany z polem Caption z elementu Option, zgodnie z opisem w tej sekcji.

- **HintPath**: ścieżka wartości ma ustawioną wskazówkę. Wiąże się z polem wskazówki z elementu Option, zgodnie z opisem w tej sekcji.

- **SelectedValue**: aktualnie wybrana wartość. Jest to wymagana właściwość typu String. Ta właściwość wiąże się z danymi ciągu ze źródła danych.

**Zdarzenia**:

- **SelectedIndexChanged.**: zdarzenie występuje, gdy zostanie zmieniony wybrany przycisk radiowy.

- **CheckedChanged**: Kiedy przycisk radiowy zmienia swój stan, to zdarzenie jest emitowane.

**Opcje**:

Dostępne są tylko dwa elementy **opcji** jako opcje dla tej kontrolki. Aby uzyskać strukturę elementu **Options** , zobacz <a href="#options-element">Opcje elementu</a>.

- **Wartość**: pole Value w pojedynczym elemencie Option musi mieć wartość true lub false.

- **Podpis**: może to być dowolna wartość ciągu.

- **Wskazówka**: może to być dowolna wartość ciągu.

**Przykład:**

![UocRadioButtonList — formant](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
>Aby ta przykład działała, należy utworzyć nowy atrybut Boolean, ABooleanAttribute i powiązać go z niestandardowym typem zasobu.

Poniższy segment kodu tworzy listę przycisków opcji:

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

**Nazwa**: UocSimpleRadioButton

**Opis**: jest to prosta Kontrolka przycisku opcji. Użycie tego formantu jest podobne do prostego pola wyboru. Istnieją dwa przyciski opcji, które pokazują obok siebie tekst etykiet. Zalecane jest powiązanie kontrolki z danymi typu Boolean.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **TrueText**: jest to opcjonalna właściwość typu String. Jest to tekst wyświetlany po wybraniu przycisku opcji.

- **FalseText**: jest to opcjonalna właściwość typu String. Jest to tekst wyświetlany, gdy przycisk opcji nie jest zaznaczony.

- **SelectedItem**: jest to opcjonalna właściwość typu Boolean. Ta wartość wskazuje, że przycisk opcji jest zaznaczony. Może to wiązać się z danymi typu Boolean ze źródła danych. Wartość domyślna to false.

**Zdarzenia**:

- **CheckedChanged**: gdy przycisk opcji zmienia stan z zaznaczonego na niezaznaczony lub odwrotnie, ten sygnał jest emitowany.

**Przykład:**

![UocSimpleRadioButton — formant](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
>Aby wykonać przykładową działanie, należy utworzyć nowy atrybut Boolean ABooleanAttribute i powiązać go z niestandardowym typem zasobu. Dane RCDC są stosowane do tego samego niestandardowego typu zasobu.

Poniższy segment kodu generuje przycisk opcji:

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

**Nazwa**: UocTextBox

**Opis**: to proste pole tekstowe, które obsługuje wprowadzanie typu String. Zalecamy używanie tej kontrolki do wiązania z danymi typu String.

**Właściwości**:

- Wszystkie typowe właściwości: Aby uzyskać informacje o tych właściwościach, zobacz <a href="#common-properties">Common Properties</a>.

- **MaxLength**: jest to opcjonalny atrybut typu Integer. Ta właściwość określa maksymalną długość ciągu wejściowego. Wartość domyślna tej właściwości to 128 znaków.

- **Tekst**: jest to opcjonalna właściwość typu String. Jest to tekst, który pojawia się w polu tekstowym. Można zdefiniować jawny ciąg, który pojawia się w polu tekstowym podczas początkowego ładowania kontrolki lub powiązać go z atrybutem schematu typu String.

- **Wiersze**: jest to opcjonalna właściwość typu Integer. Ta właściwość definiuje wysokość pola tekstowego w jednostkach znakowych. Wartość domyślna to jeden znak.

- **Kolumny**: jest to opcjonalna właściwość typu Integer. Ta właściwość określa szerokość pola tekstowego w jednostkach znakowych. Wartość domyślna to 20 znaków.

- **Zawijanie**: jest to opcjonalna właściwość typu Boolean. Ustawiając wartość tej właściwości na true, użytkownik włącza funkcję zawijania wyrazów w polu tekstowym. Wartość domyślna tej właściwości jest ustawiona na true.

- **UniquenessValidationXPath**: jest to opcjonalna właściwość typu String. Przyjmuje prawidłowe wyrażenie filtru XPath programu FIM i zapewnia, że dane wejściowe wartości przez użytkownika są unikatowe w obrębie zasobów należących do zakresu filtru. Aby na przykład upewnić się, że użytkownik zażądał nazwy wyświetlanej w ramach wszystkich grup zabezpieczeń z włączoną obsługą poczty w bazie danych usługi FIM, użyj wyrażenia XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’` . Akcja walidacji jest wykonywana, gdy użytkownik opuści stronę. Ta właściwość jest obsługiwana tylko w RCDC do tworzenia zasobów.

- **UniquenessErrorMessage**: jest to opcjonalna właściwość typu String. Ten ciąg służy do wyświetlania komunikatu o błędzie, Jeśli weryfikacja UniquenessValidationXPath nie powiedzie się i może być jawnym tekstem lub zmienną zasobów ciągu. Jeśli ta właściwość nie zostanie określona, domyślny komunikat o błędzie dotyczący nieudanej weryfikacji to "% VALUE% już istnieje. Spróbuj użyć innego elementu ".

**Zdarzenia**:

- **TextChanged**: to zdarzenie jest emitowane po zmianie tekstu w polu tekstowym.

**Przykład:**

Zapoznaj się z sekcją proste przykłady formantów, aby uzyskać pełny przykład tego formantu.


<br/>
<h2 id="appendix-a">Dodatek A: domyślny schemat XSD</h2>

W tej sekcji przedstawiono kompletny schemat XSD dla wszystkich domyślnych RCDCs, które są dostarczane z Microsoft Identity Manager 2016 z dodatkiem SP1.

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

<br/>
<h2 id="appendix-b">Dodatek B: domyślny plik XSL podsumowania</h2>

Ta sekcja zawiera kompletny zbiorczy kod XSL, który jest dostarczany z Microsoft Identity Manager 2016 z dodatkiem SP1.

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
