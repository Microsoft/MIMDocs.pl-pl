---
title: Dokumentacja dla deweloperów pakietu BHOLD dla Microsoft Identity Manager 2016 | Microsoft Docs
description: Dokumentacja dla deweloperów pakietu BHOLD
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: edf9f88a4d96d212cef4aa41cbad7d0b4446534f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92760792"
---
# <a name="bhold-developer-reference-for-microsoft-identity-manager-2016"></a>Dokumentacja dla deweloperów pakietu BHOLD dla Microsoft Identity Manager 2016

Moduł pakietu BHOLD-Core może przetwarzać polecenia skryptu. Można to zrobić bezpośrednio przy użyciu bscript.dll w projekcie .NET. Korzystanie również z interfejsu b1scriptservice. asmx usługi sieci Web. 

Przed wykonaniem skryptu należy zebrać wszystkie informacje wewnątrz skryptu, aby utworzyć ten skrypt. Te informacje można zbierać z następujących źródeł:

   * Dane wejściowe użytkownika
   * PAKIETU BHOLD dane
   * Aplikacje
   * Inne

Dane pakietu BHOLD można pobrać przy użyciu funkcji GetInfo obiektu skryptu. Istnieje kompletna lista poleceń, które mogą przedstawić wszystkie dane przechowywane w bazie danych pakietu BHOLD. Przedstawione dane są jednak uzależnione od uprawnień widoku zalogowanego użytkownika. Wynik jest w formie dokumentu XML, który można analizować.

Innym źródłem informacji może być jedna z aplikacji, które są kontrolowane przez pakietu BHOLD. Przystawka aplikacji ma specjalną funkcję, FunctionDispatch, która może służyć do prezentowania informacji specyficznych dla aplikacji. Jest to również prezentowane jako dokument XML.

Na koniec, jeśli nie istnieje żaden inny sposób, skrypt może zawierać polecenia bezpośrednio do innych aplikacji lub systemów. NoThenstallation dodatkowego oprogramowania na serwerze pakietu BHOLD może podważać bezpieczeństwo całego systemu.

Wszystkie te informacje są umieszczane w jednym dokumencie XML i przypisywane do obiektu skryptu pakietu BHOLD. Obiekt łączy ten dokument ze wstępnie zdefiniowaną funkcją. Wstępnie zdefiniowana funkcja jest dokumentem XSL, który tłumaczy dokument wejściowy skryptu na dokument polecenia pakietu BHOLD.

![Przetwarzanie skryptu pakietu BHOLD](media/mim2016-bhold-developer-reference/image001.png)

Polecenia są wykonywane w takiej samej kolejności jak w dokumencie. Jeśli jedna funkcja zakończy się niepowodzeniem, wszystkie polecenia wykonane są wycofywane.

## <a name="script-object"></a>Obiekt skryptu
W tej sekcji opisano sposób korzystania z obiektu skryptu.

### <a name="retrieve-bhold-information"></a>Pobieranie informacji o pakietu BHOLD
Funkcja **GetInfo** służy do pobierania informacji z dostępnych danych w systemie autoryzacji pakietu BHOLD. Funkcja wymaga nazwy funkcji i ostatecznie jednego lub większej liczby parametrów. Jeśli ta funkcja się powiedzie, obiekt pakietu BHOLD lub kolekcja zostanie zwrócona w postaci dokumentu XML.

Jeśli funkcja nie powiedzie się, funkcja GetInfo zwraca pusty ciąg lub błąd. Opis błędu i numer mogą służyć do uzyskania dodatkowych informacji o błędzie.

Funkcja GetInfo "FunctionDispatch" może służyć do pobierania informacji z aplikacji kontrolowanej przez system pakietu BHOLD. Ta funkcja wymaga trzech parametrów: identyfikatora aplikacji, funkcji wysyłania w postaci, w jakiej jest zdefiniowany w ASI, oraz dokumentu XML z dodatkowymi informacjami dla ASI. Jeśli funkcja się powiedzie, wynik będzie dostępny w formacie XML w obiekcie wynik.

Poniższy fragment kodu ilustruje prostą przykładową wartość GetInfo:

```C#
ScriptProcessor myScriptProcessor = new ScriptProcessor();
myScriptProcessor.Initializae("CORP\\b1user");
myScriptProcessor.GetInfo("OrgUnit", "1");
```

Podobnie do obiektu **BScript** można także uzyskać dostęp za pośrednictwem usługi sieci Web `b1scriptservice` . W tym celu należy dodać odwołanie sieci Web do projektu za pomocą http:// <server> : 5151/pakietu BHOLD/Core/b1scriptservice. asmx, gdzie <server> to serwer z zainstalowanymi plikami binarnymi pakietu BHOLD. Aby uzyskać więcej informacji, zobacz [Dodawanie odwołania usługi sieci Web do projektu programu Visual Studio](https://msdn.microsoft.com/library/d9w023sx(v=vs.71).aspx).

Poniższy przykład pokazuje, jak używać funkcji **GetInfo** z usługi sieci Web. Ten kod pobiera jednostkę organizacyjną, która ma OrgID 1, a następnie wyświetla nazwę tej jednostki organizacyjnej na ekranie.

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Xml;

namespace bhold_console
{
    class Program
    {
        static void Main(string[] args)
        {
             var bholdService = new BHOLDCORE.B1ScriptService();
             bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";
             string orgname= "";

             if (args.Length == 3)
             {
                 //Use explicit credentials from command line
                 bholdService.UseDefaultCredentials = false;
                 bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                 bholdService.PreAuthenticate = true;
             }
             else
             {
                 bholdService.UseDefaultCredentials = true;
                 bholdService.PreAuthenticate = true;
             }

             //Load BHOLD information into an xml document and loop through document to find the bholdDescription value
             var myOrgUnit = new System.Xml.XmlDocument();
             myOrgUnit.LoadXml(bholdService.GetInfo("OrgUnit","1","","");

            XmlNodeList myList = myOrgUnit.SelectNodes(("//item");

            foreach (XmlNode myNode in myList)
            {
                for (int i = 0; i < myNode.ChildNodes.Count; i++)
                {
                    if (myNode.ChildNodes[i].InnerText.ToString() == "bholdDescription")
                    {
                        orgname = myNode.ChildNodes[i + 1].InnerText.ToString();
                    }
                }
            }

            System.Console.WriteLine("The Organizational Unit Name is: " + orgname);

        }
    }
}
```

W poniższym przykładzie w języku VBScript użyto usługi sieci Web za pośrednictwem protokołu SOAP i przy użyciu metody GetInfo. Aby uzyskać dodatkowe przykłady protokołów SOAP 1,1, SOAP 1,2 i HTTP POST, zobacz sekcję dokumentacja zarządzana pakietu BHOLD lub możesz przejść do usługi sieci Web bezpośrednio z przeglądarki i wyświetlić je w tym miejscu.

```VB
Dim SOAPRequest
Dim SOAPParameters
Dim SOAPResponse
Dim xmlhttp

Set xmlhttp = CreateObject("Microsoft.XMLHTTP")

xmlhttp.open "POST", "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx", False, "CORP\Administrator", "abc123*2k"

xmlhttp.setRequestHeader "Content-type", "text/xml; charset=utf-8"
xmlhttp.setRequestHeader "SOAPAction", "http://B1/B1ScriptService/GetInfo"

SOAPRequest = "<?xml version='1.0' encoding='utf-8'?> <soap:Envelope" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsi=""http://" & vbCRLF
SOAPRequest = SOAPRequest & " www.w3.org/2001/XMLSchema-instance""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsd=""http://www.w3.org/2001/XMLSchema""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:soap=""http://schemas.xmlsoap.org/soap/envelope/"">" & vbCRLF
SOAPRequest = SOAPRequest & " <soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " <GetInfo xmlns=""http://B1/B1ScriptService"">" & vbCRLF
SOAPRequest = SOAPRequest & " <functionName>OrgUnit</functionName>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter1>1</parameter1>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter2></parameter2>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter3></parameter3>" & vbCRLF
SOAPRequest = SOAPRequest & " </GetInfo>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Envelope>"
MsgBox SOAPRequest

xmlhttp.send SOAPRequest 

SOAPResponse = xmlhttp.responseText

MsgBox SOAPResponse
```

## <a name="execute-scripts"></a>Wykonywanie skryptów

Funkcja **metodę executescript** obiektu **BScript** może służyć do wykonywania skryptów. Ta funkcja wymaga dwóch parametrów. Pierwszy parametr jest dokumentem XML zawierającym informacje niestandardowe, które mają być używane przez skrypt. Drugi parametr jest nazwą wstępnie zdefiniowanego skryptu do użycia. InIn katalog wstępnie zdefiniowanych skryptów pakietu BHOLD powinien być dokumentem XSL o takiej samej nazwie jak funkcja, ale z rozszerzeniem. xsl.

Jeśli funkcja nie powiedzie się, funkcja metodę executescript zwraca wartość false. Opis błędu i numer mogą być używane do dowiedzieć się, co poszło źle. Poniżej przedstawiono przykład użycia metody sieci Web ExecuteXML. Ta metoda wywołuje metodę executescript.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample
{
    class Program
    {
        static void Main(string[] args)
        {
            var bholdService = new BHOLDCORE.B1ScriptService();
            bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";

            if (args.Length == 3)
            {
                //Use explicit credentials from command line
                bholdService.UseDefaultCredentials = false;
                bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                bholdService.PreAuthenticate = true;
            }
            else
            {
                bholdService.UseDefaultCredentials = true;
                bholdService.PreAuthenticate = true;
            }
            System.Console.WriteLine( "Add user #3 to role #44, result: {0}", bholdService.ExecuteXml(roleAddUser("44", "3")) );
            System.Console.WriteLine("Add user D1 to role 'MR-OU2 No Users', result: {0}", bholdService.ExecuteXml(roleAddUser("MR-OU2 No Users", "D1")));

        }

        private static System.Xml.XmlNode roleAddUser(string roleId, string userId)
        {
            var script = new System.Xml.XmlDocument();
            script.LoadXml(string.Format("<functions>"
                                        +"  <function name='roleadduser' roleid='{0}' userid='{1}' />"
                                        +"</functions>",
                                        roleId,
                                        userId)
                           );
            return script.DocumentElement;
```

## <a name="bholdscriptresult"></a>BholdScriptResult

Ta funkcja GetInfo jest dostępna po wykonaniu funkcji **metodę executescript** . Funkcja zwraca ciąg w formacie XML zawierający pełny raport wykonania. Węzeł skryptu zawiera strukturę XML wykonanego skryptu.

Dla każdej funkcji, która kończy się niepowodzeniem podczas wykonywania skryptu, funkcja Node jest dodawana z nazwą węzłów. ExecuteXML i błąd jest dodawany do końca dokumentu dodawane są wszystkie wygenerowane identyfikatory.

Należy zauważyć, że dodawane są tylko te funkcje, które zawierają błąd. Numer błędu równy "0" oznacza, że funkcja nie jest wykonywana. 

```
<Bhold>
  <Script>
    <functions>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>     
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>      
    </functions>
  </Script>
  <Function>
    <Name>OrgUnitadd</Name>
    <ExecutedXML>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>
    </ExecutedXML>
    <Error Number="5" Description="Violation of UNIQUE KEY constraint 'IX_OrgUnits'. Cannot insert duplicate key in object 'dbo.OrgUnits'.
The statement has been terminated."/>
  </Function>
  <Function>
    <Name>roleaddOrgUnit</Name>
    <ExecutedXML>
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>
    </ExecutedXML>
    <Error Number="0" Description=""/>
  </Function>
  <IDS>
    <ID name="@ID@">35</ID>
  </IDS>
</Bhold>
```

## <a name="id-parameters"></a>Parametry identyfikatora
Parametry identyfikatora mają specjalne traktowanie. Wartości inne niż liczbowe są używane jako wartość wyszukiwania do lokalizowania odpowiednich jednostek w magazynie danych pakietu BHOLD. Gdy wartość wyszukiwania nie jest unikatowa, zwracana jest pierwsza jednostka, która jest zgodna z wartością wyszukiwania.

Aby rozróżnić wartości wyszukiwania liczbowego od identyfikatorów, można użyć prefiksu. Gdy pierwsze sześć znaków wartości wyszukiwania równa się "no_id:", wówczas te znaki są usuwane przed użyciem wartości do wyszukania. Można użyć symboli wieloznacznych SQL "%".

Następujące pola są używane z wartością wyszukiwania:

| Typ identyfikatora   | Pole wyszukiwania |
|---|---|
| OrgUnitID | Opis  |
| roleID    | Opis  |
| taskID    | Opis  |
| userID    | DefaultAlias |

## <a name="script-access-and-permissions"></a>Dostęp do skryptu i uprawnienia
Kod po stronie serwera na stronach Active Server służy do wykonywania skryptów. W związku z tym dostęp do skryptu oznacza dostęp do tych stron. System pakietu BHOLD przechowuje informacje o punktach wejścia na stronach niestandardowych. Te informacje obejmują stronę początkową i opis funkcji (Obsługa wielu języków powinna być obsługiwana).

Użytkownik jest uprawniony do wprowadzania stron niestandardowych i wykonywania skryptu. Każdy punkt wejścia jest prezentowany jako zadanie. Każdy użytkownik, który zdobył to zadanie za pośrednictwem roli lub jednostki, może wykonać odpowiednią funkcję.

Nowa funkcja w menu przedstawia wszystkie funkcje niestandardowe, które mogą być wykonywane przez użytkownika. Ponieważ skrypt może wykonywać akcje w systemie pakietu BHOLD w ramach tożsamości innej niż zalogowany użytkownik. Istnieje możliwość udzielenia uprawnienia do wykonywania jednej konkretnej akcji bez nadzoru nad jakimkolwiek obiektem. Na przykład może to być przydatne w przypadku pracownika, który może wprowadzać nowych klientów do firmy. Te skrypty mogą również służyć do tworzenia stron samorejestracji.

## <a name="command-script"></a>Skrypt polecenia
Skrypt polecenia zawiera listę funkcji, które są wykonywane przez system pakietu BHOLD. Lista jest zapisywana w dokumencie XML, który jest zgodny z następującymi definicjami:


|   Skrypt polecenia   |              `<functions>functions</functions>`               |
|--------------------|---------------------------------------------------------------|
|      — funkcje      |                      Funkcja {Function}                      |
|       — funkcja      | <funkcja Name = "FunctionName" functionParameters [Return] (/> \| > listaparametrów </function>) |
|    functionName    | Prawidłowa nazwa funkcji, zgodnie z opisem w poniższych sekcjach. |
| functionParameters |                     { functionParameter }                     |
| functionParameter  |               ParameterName = "parameterValue"                |
|   parameterName    |                    Prawidłowa nazwa parametru.                    |
|   parameterValue   |                      @variable@ \| wartość                      |
|       wartość        |                   Prawidłowa wartość parametru.                    |
|   Listaparametrów    |          <parameters> Body  </parameters>          |
|   Body    | <parameter name="parameterName"> parameterValue </parameter>  |
|       return       |                      Return = " @variable @"                      |
|      zmienna      |                    Nazwa zmiennej niestandardowej.                    |

KOD XML zawiera następujące tłumaczenia znaków specjalnych:

| Plik XML | Znak |
|---|---|
| ```&amp;```  | & |
| ```&lt;```   | < |
| ```&gt;```   | > |
| ```&quot;``` | " |
| ```&apos;``` | ' |

Te znaki XML mogą być używane w identyfikatorach, ale nie są zalecane.

Poniższy kod przedstawia przykład prawidłowego dokumentu polecenia z trzema funkcjami:

```
<functions>

   <functionname="OrgUnitAdd" parentID="34" description="Acme Inc." orgtypeID="5" return="@UnitID@" />

   <function name="UserAdd" description="John Doe" alias="jdoe" languageID="1" OrgUnitID="@UnitID@" />

   <function name="TaskAddFile" taskID="93" path="/customers/purchase">
      <parameters>
         <parameter name="history"> True</parameter>
      </parameters>
   </function>

</functions>
```

Funkcja **OrgUnitAdd** przechowuje identyfikator utworzonej jednostki w zmiennej o nazwie UnitID. Ta zmienna jest używana jako dane wejściowe dla funkcji UserAdd. Wartość zwracana przez tę funkcję nie jest używana. W poniższych sekcjach opisano wszystkie dostępne funkcje, wymagane parametry i ich wartości zwracane.

## <a name="execute-functions"></a>Funkcje wykonywania
W tej sekcji opisano sposób korzystania z funkcji Execute.

### <a name="abaattributeruleadd"></a>ABAAttributeRuleAdd
Utwórz nową regułę atrybutu dla określonego typu atrybutu. Reguły atrybutów mogą być łączone tylko z jednym typem atrybutu.

Określona reguła atrybutu może być połączona z wszystkimi możliwymi typami atrybutów. 

Nie można zmienićtype reguły z poleceniem "ABAattributeruletypeupdate". Wymaga, aby opis atrybutu był unikatowy.


|    Argumenty    |                                                                                                                                                                                                                  Typ                                                                                                                                                                                                                  |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Opis   |                                                                                                                                                                                                                  Tekst                                                                                                                                                                                                                  |
|    Typ ruletype     | Określ rodzaj reguły atrybutu. W zależności od rodzaju typu reguły atrybutu należy uwzględnić inne argumenty. Następujące wartości typu reguły są prawidłowe:<ul><li>0: wyrażenie regularne (Dodaj argument "value").</li><li>1: wartość (Dodaj argumenty "operator" i "wartość").</li><li>2: Lista wartości.</li><li>3: zakres (Dodaj argumenty "RangeMin" i "RangeMax").</li><li>4: wiek (Dodaj argumenty "operator" i "wartość").</li></ul> |
|  InvertResult   | ```["0"|"1"|"N"|"Y"]``` |
| AttributeTypeID |                                                                                                                                                                                                                  Tekst                                                                                                                                                                                                                  |

| Argumenty opcjonalne | Typ |
|---|---|
| Operator | Tekst <br>**Uwaga** : ten argument jest wymagany, jeśli parametr **ruletype** ma wartooć 1 lub 4. Możliwe wartości to "=", "<" lub ">". Tagi XML muszą używać " &gt; " dla ">" i " &lt; " dla "<". |
| RangeMin | Liczba <br>**Uwaga** : ten argument jest obowiązkowy, jeśli **ruletype** ma 3. |
| RangeMax | Liczba <br>**Uwaga** : ten argument jest obowiązkowy, jeśli **ruletype** ma 3. |
| Wartość    | Tekst <br>**Uwaga** : ten argument jest obowiązkowy, jeśli parametr **ruletype** ma wartość 0, 1 lub 4. Argument musi być wartością numeryczną lub alfanumeryczną. |
| Typ zwracany AttributeRuleID | Tekst   |


### <a name="applicationadd"></a>applicationadd
Tworzy nową aplikację, zwraca identyfikator nowej aplikacji.

| Argumenty | Typ |
|---|---|
| description (opis) |      |
| maszynie     |      |
| moduł      |      |
| parametr   |      |
| protokol    |      |
| nazwa użytkownika    |      |
| hasło    |      |
| svroleID (opcjonalnie)                | Jeśli ten argument nie jest obecny, używana jest rola nadzorcy bieżącego użytkownika. |
| Applicationaliasformula (opcjonalnie) | Formuła aliasu służy do tworzenia aliasu dla użytkownika, gdy zostanie przypisany do uprawnienia aplikacji. Alias jest tworzony, jeśli użytkownik nie ma jeszcze aliasu dla tej aplikacji. Jeśli wartość nie jest określona, domyślny alias użytkownika jest używany jako alias aplikacji. Formuła jest sformatowana jako ` [<<objecttype>>.<<nameofobjecttypeattribute>>(startindexoffset,length offset)]` . Przesunięcie jest opcjonalne. Można używać tylko atrybutów użytkownika i aplikacji. Można użyć bezpłatnego tekstu. Znaki zastrzeżone to lewy nawias kwadratowy ([) i prawy nawias kwadratowy (]). Na przykład: `[Application.bholdDescription]\[User.bholdDefAlias(1,5)]`. |
| Typ zwracany | Identyfikator nowej aplikacji. |

### <a name="attributesetvalue"></a>AttributeSetValue
Ustawia wartość typu atrybutu połączonego z typem obiektu. Wymaga, aby opisy typu obiektu i typu atrybutu były unikatowe.

| Argumenty | Typ |
|---|---|
| ObjectTypeID     | Tekst |
| ObjectID         | Tekst |
| AttributeTypeID  | Tekst |
| Wartość            | Tekst |
| Typ zwracany      | Typ |

### <a name="attributetypeadd"></a>AttributeTypeAdd
Wstawia nowy typ atrybutu/Typ właściwości.


|        Argumenty        |                                               Typ                                                |
|-------------------------|---------------------------------------------------------------------------------------------------|
|       Datatypeid        |                                               Tekst                                                |
| Opis (= tożsamość) | Tekst <br>**Uwaga** : nie można używać słów zarezerwowanych, takich jak "a", "frm", "ID", "usr" i "pakietu BHOLD". |
|        MaxLength        |                                       Liczba w [1,.., 255]                                        |
| ListOfValues (wartość logiczna)  |                                          ```["0"|"1"|"N"|"Y"]```                               |
|      DefaultValue       |                                               Tekst                                                |
|       Typ zwracany       |                                               Typ                                                |
|     AttributeTypeID     |                                               Tekst                                                |

### <a name="attributetypesetadd"></a>AttributeTypeSetAdd
Wstawia nowy zestaw typów atrybutów. Wymaga, aby opis zestawu typu atrybutu był unikatowy.

| Argumenty | Typ |
|---|---|
| Opis (= tożsamość) |Tekst |
| Typ zwracany | Typ |
| AttributeTypeSetID | Tekst |

### <a name="attributetypesetaddattributetype"></a>AttributeTypeSetAddAttributeType
Wstawia nowy typ atrybutu w istniejącym typie atrybutu. Wymaga, aby opisy zestawu i typu atrybutu były unikatowe.


|     Argumenty      |                              Typ                              |
|--------------------|----------------------------------------------------------------|
| AttributeTypeSetID |                              Tekst                              |
|  AttributeTypeID   |                              Tekst                              |
|       Zamówienie        |                             Liczba                             |
|     LocationID     | Tekst <br>**Uwaga** : Lokalizacja ma wartość "Group" lub "Single". |
|     Obowiązkowy      |                    ```["0"|"1"|"N"|"Y"]```                  |
|    Typ zwracany     |                              Typ                              |

### <a name="objecttypeaddattributetypeset"></a>ObjectTypeAddAttributeTypeSet
Dodaje typ atrybutu ustawiony do typu obiektu. Wymaga, aby opis typu obiektu i typ atrybutu były unikatowe. Typy obiektów to: system, OrgUnit, użytkownik, zadanie.

| Argumenty | Typ |
|---|---|
| ObjectTypeID | Tekst | 
| AttributeTypeSetID | Tekst | 
| Zamówienie | Liczba | 
| Widoczne | <ul><li>0: ustawiony typ atrybutu jest widoczny.</li><li>2: ustawiony typ atrybutu jest widoczny, gdy zostanie wybrany przycisk **więcej informacji** .</li><li>1: zestaw typów atrybutów jest niewidoczny.</li></ul> | 
| Typ zwracany | Typ | 

### <a name="orgunitadd"></a>OrgUnitadd
Tworzy nową jednostkę organizacyjną, zwraca identyfikator nowej jednostki organizacyjnej.

| Argumenty | Typ |
|---|---|
| description (opis) | | 
| orgtypeID | | 
| parentID | | 
| OrgUnitinheritedroles (opcjonalnie) | | 
| Typ zwracany | Typ | 
| Identyfikator nowej jednostki | OrgUnitinheritedroles parametru <br>ma wartość yes lub No. | 

### <a name="orgunitaddsupervisor"></a>OrgUnitaddsupervisor
Ustaw użytkownika jako nadzorcę jednostki organizacyjnej.

| Argumenty | Typ |
|---|---|
| svroleID | Można również użyć identyfikatora użytkownika argumentu. W takim przypadku wybrana jest domyślna rola nadzoru. Domyślna rola opiekuna ma nazwę, taką jak **__svrole** , a po niej numer. Identyfikator użytkownika argumentu może być używany w celu zapewnienia zgodności z poprzednimi wersjami. | 
| OrgUnitID | | 

### <a name="orgunitadduser"></a>OrgUnitadduser
Ustaw użytkownika jako członka jednostki organizacyjnej.

| Argumenty | Typ |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="orgunitdelete"></a>OrgUnitdelete
Usuwa jednostkę organizacyjną.

| Argumenty | Typ |
|---|---|
| OrgUnitID | | 

### <a name="orgunitdeleteuser"></a>OrgUnitdeleteuser
Usuwa użytkownika jako członka jednostki organizacyjnej.

| Argumenty | Typ |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="roleadd"></a>roleadd
Tworzy nową rolę.

| Argumenty | Typ |
|---|---|
| Opis | | 
| svrole | | 
| svroleID (opcjonalnie) | Jeśli ten argument nie jest obecny, zostanie użyta rola opiekuna bieżącego użytkownika. | 
| ContextAdaptable (opcjonalnie) | ```["0","1","N","Y"]``` | 
| MaxPermissions (opcjonalnie) | Liczba całkowita | 
| MaxRoles (opcjonalnie) | Liczba całkowita | 
| MaxUsers (opcjonalnie) | Liczba całkowita | 
| Typ zwracany | Typ | 
| Identyfikator nowej roli | | 

### <a name="roleaddorgunit"></a>roleaddOrgUnit
Przypisuje rolę do jednostki organizacyjnej.


|    Argumenty    |                                      Typ                                      |
|-----------------|--------------------------------------------------------------------------------|
|    OrgUnitID    |                                     roleID                                     |
| inheritThisRole | wartość "true" lub "false" wskazuje, czy rola jest proponowana dla jednostek bazowych. |

### <a name="roleaddrole"></a>roleaddrole
Przypisuje rolę jako podrolę innej roli.

| Argumenty | Typ |
|---|---|
| roleID | | 
| subRoleID | | 

### <a name="roleaddsupervisor"></a>roleaddsupervisor
Ustaw użytkownika jako nadzorcę roli.

| Argumenty | Typ |
|---|---|
| svroleID | Można również użyć identyfikatora użytkownika argumentu. W takim przypadku wybrana jest domyślna rola nadzoru. Domyślna rola opiekuna ma nazwę, taką jak **__svrole** , a po niej numer. Identyfikator użytkownika argumentu może być używany w celu zapewnienia zgodności z poprzednimi wersjami. | 
| roleID | | 

### <a name="roleadduser"></a>roleadduser
Przypisuje rolę użytkownikowi. Rola nie może być rolą dodające kontekstu, jeśli nie podano identyfikator ContextId.

| Argumenty | Typ |
|---|---|
| userID | | 
| roleID | | 
| durationtype (opcjonalnie) | Może zawierać wartości "Free", "hours" i "Days". | 
| durationLength (opcjonalnie) | Wymagane, gdy wartość durationtype to "hours" lub "Days". powinna zawierać liczbę całkowitą liczby godzin lub dni, do których rola jest przypisana do użytkownika. | 
| Uruchom (opcjonalnie) | Data i godzina przypisania roli. Gdy ten atrybut zostanie pominięty, rola zostaje przypisana natychmiast. Format daty to "RRRR-MM-DDTgg: NN: SS", gdzie wymagane są tylko rok, miesiąc i dzień. na przykład "2004-12-11" i "2004-11-28T08:00" są prawidłowymi wartościami. | 
| End (opcjonalnie) | Data i godzina odwołania roli. Gdy są podawane wartości durationtype i durationLength, ta wartość jest ignorowana. Format daty to "RRRR-MM-DDTgg: NN: SS", gdzie wymagane są tylko rok, miesiąc i dzień. na przykład "2004-12-11" i "2004-11-28T08:00" są prawidłowymi wartościami. | 
| linkreason | Wymagane, gdy jest określony początek, koniec lub czas trwania, w przeciwnym razie jest ignorowany. | 
| Identyfikator ContextId (opcjonalnie) | Identyfikator jednostki organizacyjnej, wymagany tylko w przypadku ról z możliwością dostosowania kontekstu. | 

### <a name="roledelete"></a>roledelete
Usuwa rolę.

| Argumenty | Typ |
|---|---|
| roleID | | 

### <a name="roledeleteuser"></a>roledeleteuser
Usuwa przypisanie roli do użytkownika. Dziedziczone role użytkownika są odwoływane przez to polecenie.

| Argumenty | Typ |
|---|---|
| userID | | 
| roleID | | 
| Identyfikator ContextId (opcjonalnie) |  | 

### <a name="roleproposeorgunit"></a>roleproposeOrgUnit
Proponuje rolę do przypisywania jej do członków i OrgUnits OrgUnit.

| Argumenty | Typ |
|---|---|
| OrgUnitID | | 
| roleID | | 
| durationtype (opcjonalnie) | Może zawierać wartości "Free", "hours" i "Days". | 
| durationLength | Wymagane, gdy wartość durationtype ma wartość "hours" lub "Days", powinna zawierać liczbę całkowitą liczby godzin lub dni, przez które rola jest przypisana do użytkownika. | 
| durationFixed | wartość "true" lub "false" wskazuje, czy przypisanie tej roli do użytkownika powinno być równe durationLength. | 
| inheritThisRole | wartość "true" lub "false" wskazuje, czy rola jest proponowana dla jednostek bazowych. | 

### <a name="taskadd"></a>taskadd
Tworzy nowe zadanie, zwraca identyfikator nowego zadania.

| Argumenty | Typ |
|---|---|
| Identyfikator | | 
| description (opis) | Tekst z maksymalnie 254 znaków. | 
| TASKNAME | Tekst z maksymalnie 254 znaków. | 
| tokenGroupID | | 
| svroleID (opcjonalnie) | Jeśli ten argument nie jest obecny, zostanie użyta rola opiekuna bieżącego użytkownika. | 
| contextAdaptable (opcjonalnie) | ```["0","1","N","Y"]``` | 
| niekonstrukcja (opcjonalnie) | ```["0","1","N","Y"]``` | 
| auditaction (opcjonalnie) | <ul><li>0: nieznane (ustawienie domyślne)</li><li>1: ReportOnly</li><li>2: AlertAppAll</li><li>3: AlertAppObsolete</li><li>4: AlertAppMissing</li><li>5: EnforceAppAll</li><li>6: EnforceAppObsolete</li><li>7: EnforceAppMissing</li><li>8: AlertEnforceAppAll</li><li>9: AlertEnforceAppObsolete</li><li>10: AlertEnforceAppMissing</li><li>11: nieportal</li></ul> | 
| auditalertmail (opcjonalnie) | Adres e-mail dla alertów dotyczących tego uprawnienia jest wysyłany przez audytora. Jeśli ten argument nie jest obecny, używany jest adres e-mail alertu audytora. | 
| MaxRoles (opcjonalnie) | Liczba całkowita | 
| MaxUsers (opcjonalnie) | Liczba całkowita | 
| Typ zwracany | Identyfikator nowego zadania. | 

### <a name="taskadditask"></a>taskadditask
Wskaż, że dwa zadania są niezgodne.

| Argumenty | Typ |
|---|---|
| taskID | | 
| taskID2 | | 

### <a name="taskaddrole"></a>taskaddrole
Przypisuje zadanie do roli.

| Argumenty | Typ |
|---|---|
| roleID | | 
| taskID | | 

### <a name="taskaddsupervisor"></a>taskaddsupervisor
Nadanie użytkownikowi opiekuna zadania.

| Argumenty | Typ |
|---|---|
| svroleID | Można również użyć identyfikatora użytkownika argumentu. W takim przypadku wybrana jest domyślna rola nadzoru. Domyślna rola opiekuna ma nazwę, taką jak **__svrole** , a po niej numer. Identyfikator użytkownika argumentu może być używany w celu zapewnienia zgodności z poprzednimi wersjami. | 
| taskID | | 

### <a name="useradd"></a>useradd
Tworzy nowego użytkownika, zwraca identyfikator nowego użytkownika.

| Argumenty | Typ |
|---|---|
| description (opis) | | 
| alias | | 
| languageID | <ul><li>1: angielski</li><li>2: holenderski</li></ul> | 
| OrgUnitID | | 
EndDate (opcjonalnie) |Format daty to "RRRR-MM-DDTgg: NN: SS", gdzie wymagane są tylko rok, miesiąc i dzień. na przykład "2004-12-11" i "2004-11-28T08:00" są prawidłowymi wartościami. | 
| wyłączone (opcjonalnie) | <ul><li>0: włączone</li><li>1: wyłączone</li></ul> | 
| MaxPermissions (opcjonalnie) | Liczba całkowita | 
| MaxRoles (opcjonalnie) | Liczba całkowita | 
| Typ zwracany | Identyfikator nowego użytkownika. | 

### <a name="useraddrole"></a>UserAddRole
Dodaje rolę użytkownika.
<!--- missing content -->

| Argumenty | Typ |
|---|---|
|  | | 

### <a name="userdeleterole"></a>UserDeleteRole 
Usuwa rolę użytkownika.
<!--- missing content -->

| Argumenty | Typ |
|---|---|
|  | | 

### <a name="userupdate"></a>Userupdate
Aktualizuje użytkownika.

| Argumenty | Typ |
|---|---|
| UserID | | 
| Opis (opcjonalnie)|  | 
| language | <ul><li>1: angielski</li><li>2: holenderski</li></ul> | 
| userDisabled (opcjonalnie) |<ul><li>0: włączone</li><li>1: wyłączone</li></ul> | 
| UserEndDate (opcjonalnie) | Format daty to "RRRR-MM-DDTgg: NN: SS", gdzie wymagane są tylko rok, miesiąc i dzień. na przykład "2004-12-11" i "2004-11-28T08:00" są prawidłowymi wartościami. | 
| firstName (opcjonalnie) | | 
| middleName (opcjonalnie) | | 
| lastName (opcjonalnie) | | 
| maxPermissions (opcjonalnie) | Liczba całkowita |
| maxRoles (opcjonalnie) | Liczba całkowita | 


## <a name="getinfo-functions"></a>GetInfo — funkcje
Zestaw funkcji opisanych w tej sekcji może służyć do pobierania informacji przechowywanych w systemie pakietu BHOLD. Każdą funkcję można wywołać za pomocą funkcji GetInfo z obiektu BScript. Niektóre obiekty wymagają parametrów. Zwrócone dane podlegają uprawnieniom widok i nadzorowanym obiektom zalogowanego użytkownika.

### <a name="getinfo-arguments"></a>GetInfo — argumenty

| Nazwa | Opis |
|---|---|
| aplikacje | Zwraca listę aplikacji. | 
| AttributeType | Zwraca listę typów atrybutów. | 
| orgtypes | Zwraca listę typów jednostek organizacyjnych. | 
| OrgUnits | Zwraca listę jednostek organizacyjnych bez atrybutów jednostek organizacyjnych. | 
| OrgUnitproposedroles | Zwraca listę proponowanych ról połączonych z jednostką organizacyjną. | 
| OrgUnitroles | Zwraca listę bezpośrednio połączonych ról danej jednostki organizacyjnej | 
| Objecttypeattributetypes | | 
| uprawnienia | | 
| permissionusers | | 
| role | Zwraca listę ról. | 
| roletasks | Zwraca listę zadań danej roli. | 
| zadania | Zwraca wszystkie zadania znane przez pakietu BHOLD. | 
| users | Zwraca listę użytkowników. | 
| usersroles | Zwraca listę połączonych ról opiekuna danego użytkownika. | 
| userpermissions | Zwraca listę uprawnień danego użytkownika. | 

### <a name="orgunit-info"></a>Informacje OrgUnit

| Nazwa | Parametry | Typ zwracany |
|---|---|---|
| OrgUnit | OrgUnitID | OrgUnit | 
| OrgUnitasiattributes | OrgUnitID | Kolekcja | 
| OrgUnits| Filtrowanie (opcjonalne), proptypeid (opcjonalnie)<br> Wyszukuje jednostki, które zawierają ciąg opisany w **filtrze** w elemencie proptype opisanym w **proptypeid** . W przypadku pominięcia tego identyfikatora filtr dotyczy opisu jednostki. Jeśli nie podano żadnego filtru, wszystkie widoczne jednostki są zwracane. | Kolekcja | 
| OrgUnitOrgUnits | OrgUnitID | Kolekcja | 
| OrgUnitparents | OrgUnitID | Kolekcja | 
| OrgUnitpropertyvalues | OrgUnitID | Kolekcja | 
| OrgUnitproptypes |  | Kolekcja | 
| OrgUnitusers | OrgUnitID | Kolekcja | 
| OrgUnitproposedroles | OrgUnitID | Kolekcja | 
| OrgUnitroles | OrgUnitID | Kolekcja | 
| OrgUnitinheritedroles | OrgUnitID | Kolekcja | 
| OrgUnitsupervisors | OrgUnitID | Kolekcja | 
| OrgUnitinheritedsupervisors| OrgUnitID | Kolekcja | 
| OrgUnitsupervisorroles | OrgUnitID | Kolekcja | 

### <a name="role-information"></a>Informacje o roli

| Nazwa | Parametry | Typ zwracany |
|---|---|---|
| role (rola) | roleID | Obiekt | 
| role | Filtr (opcjonalnie) | Kolekcja | 
| roleasiattributes | roleID | Kolekcja | 
| roleOrgUnits | roleID | Kolekcja | 
| roleparentroles | roleID | Kolekcja | 
| rolesubroles | roleID | Kolekcja | 
| rolesupervisors | roleID| Kolekcja | 
| rolesupervisorroles | roleID | Kolekcja | 
| roletasks | roleID | Kolekcja | 
| roleusers | roleID | Kolekcja | 
| rolesupervisorroles | roleID | Kolekcja | 
| proposedroleOrgUnits | roleID | Kolekcja | 
| proposedroleusers | roleID | Kolekcja | 

### <a name="permission---task-information"></a>Uprawnienie — informacje o zadaniu

| Nazwa | Parametry | Typ zwracany |
|---|---|---|
| zezwolenie | TaskID | Uprawnienie | 
| uprawnienia | Filtr (opcjonalnie) | Kolekcja | 
| permissionasiattributes | TaskID | Kolekcja | 
| permissionattachments | TaskID | Kolekcja | 
| permissionattributetypes | - | Kolekcja | 
| permissionparams | TaskID | Kolekcja | 
| permissionroles | TaskID | Kolekcja | 
| permissionsupervisors | TaskID | Kolekcja | 
| permissionsupervisorroles | TaskID | Kolekcja | 
| permissionusers | TaskID | Kolekcja | 
| task | TaskID | Zadanie | 
| zadania| Filtr (opcjonalnie) | Kolekcja | 
| taskattachments | TaskID | Kolekcja | 
| taskparams | TaskID | Kolekcja | 
| taskroles | TaskID | Kolekcja | 
| tasksupervisors | TaskID | Kolekcja | 
| tasksupervisorroles | TaskID | Kolekcja | 
| taskusers | TaskID | Kolekcja | 

### <a name="user-information"></a>Informacje o użytkowniku

| Nazwa | Parametry | Typ zwracany |
|---|---|---|
| użytkownik | UserID | Użytkownik | 
| users | Filtrowanie (opcjonalne), attributetypeid (opcjonalnie) <br>Wyszukuje użytkowników, którzy znajdują się w elemencie AttributeType określonym przez attributetypeid ciągu określonego przez filtr. W przypadku pominięcia tego identyfikatora filtr dotyczy domyślnego aliasu użytkownika. Jeśli nie podano żadnego filtru, zostaną zwrócone wszystkie widoczne użytkownicy. Na przykład:<ul><li>`GetInfo("users")` zwraca wszystkich użytkowników.</li><li>`GetInfo("users", "%dmin%")` zwraca wszystkich użytkowników z ciągiem "DMin" w domyślnym aliasie.<br></li><li>Załóżmy, że użytkownicy mają dodatkowy atrybut o nazwie `"City".GetInfo("users", "%msterda%", "City")` . To wywołanie zwraca wszystkich użytkowników z ciągiem "msterda" w atrybucie miasto.</li></ul> | UserCollection | 
| usersapplications | UserID | Kolekcja | 
| Userpermissions | UserID | Kolekcja | 
| roli użytkownika | UserID | Kolekcja | 
| usersroles | UserID | Kolekcja | 
| userstasks | UserID | Kolekcja | 
| usersunits | UserID | Kolekcja | 
| usertasks | UserID | Kolekcja | 
| userunits | UserID | Kolekcja | 

### <a name="return-types"></a>Typy zwracane
W tej sekcji opisano typy zwracane funkcji GetInfo.

| Nazwa | Typ zwracany |
|---|---|
| Kolekcja |```=<ITEMS>{<ITEM description="..." id="..." />}</ITEMS>``` | 
| Obiekt |```=<ITEM type="…" description="..." />``` | 
| OrgUnit |```= <ITEM id="…" description="..." orgtype="..." parent="..."> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Uprawnienie |```= <ITEM id="…" description="…" name="…" tokengroup="…" application="…" > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Role |```= <ITEMS> {<ITEM id="…" description="…" />} </ITEMS>``` | 
| Rola |```= <ITEM id="…" description="… " > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Zadanie | Zobacz uprawnienie | 
| Użytkownicy | ```= <ITEMS> {<ITEM description="…" id="…" alias="…" />} </ITEMS>``` | 
| Użytkownik |```= <ITEM id="…" description="…" alias="…" firstname="…" lastname="…" uuid="…" language="…"> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 

## <a name="script-sample"></a>Przykładowy skrypt

Firma ma serwer pakietu BHOLD i chce, aby zautomatyzowany skrypt, który tworzy nowych klientów. Informacje o firmie i jej Menedżerze zakupów wchodzą w dostosowanej stronie sieci Web. Każdy klient jest prezentowany w modelu jako jednostka w ramach klientów jednostkowych. Kierownik ds. zakupów jest również członkiem jako nadzorcą tej jednostki. Utworzona zostanie rola, która nadaje właścicielom prawo do zakupu nazwy nowego klienta.

Jednak ten klient nie istnieje w aplikacji. Istnieje specjalna funkcja zaimplementowana w ASI FunctionDispatch, która tworzy nowe konto klienta w aplikacji do zakupu. Każdy klient ma typ klienta.

Możliwe typy można także przedstawić za pomocą funkcji FunctionDispatch. AA wybiera poprawny typ dla nowego klienta.

Utwórz rolę i zadanie, aby przedstawić uprawnienia do zakupu. Uprawnienie do zakupu rzeczywistego jest prezentowane przez ASI jako plik `/customers/customer id/purchase` . Ten plik powinien być połączony z nowym zadaniem.

Strona Active Server, która gromadzi informacje, wygląda następująco:

```VB
<%@ Language=VBScript %>
<% Option Explicit %>
<html>
<body>
<form action="MySubmit.asp" method=post>
<input type="hidden" name="OrgUnitID" 
     value="<% = Request("ID") %>">
Company <input type="text" name="Description"> <br>
Type <select name="OrgType">
<%Dim oOrgType
For Each oOrgType on bscript.getinfo("Orgtypes") %>
<option value="<% = oOrgType.OrgTypeID %>">
<% = oOrgType.Description %>
</option> <%
Next %>
</select>  <br>
Manager <input type="text" name=" manager"> <br>
Alias <input type=" text" name=" alias"> <br>
e-mail <input type=" text" name=" email"> <br>
<input type="submit">
</form>
</body>
</html>
```

Wszystkie dostosowane strony trzeba będzie zażądać odpowiednich informacji i utworzyć dokument XML z żądanymi informacjami. W tym przykładzie Strona wysyłania przesyła dane w dokumencie XML, przypisując je do **b1script. Obiekt Parameters** i wreszcie wywołuje `b1script.ExecuteScript("MyScript")` funkcję.

Poniższy skrypt wejściowy przedstawia ten przykład:

```
<customer>
<description>ACME inc.</description>
<orgtype>5<orgtype>
<name>John Doe</name>
<alias>jdoe</alias>
<email>jdoe@acme.com</email>
</customer>
```

Ten skrypt wejściowy nie zawiera żadnych poleceń dla pakietu BHOLD. Dzieje się tak, ponieważ ten skrypt nie jest wykonywany bezpośrednio przez pakietu BHOLD; Zamiast tego jest to dane wejściowe dla wstępnie zdefiniowanej funkcji. Ta wstępnie zdefiniowana funkcja tłumaczy ten obiekt na dokument XML za pomocą poleceń pakietu BHOLD. Ten mechanizm wstrzymuje wysyłanie skryptów do systemu pakietu BHOLD, które zawierają funkcje, które użytkownik nie może wykonać, na przykład pomocą polecenia SETUSER i funkcji do ASI.

```
<?xml version="1.0" encoding="utf-8" ?> 
- <functions xmlns="http://tempuri.org/BscriptFunctions.xsd">

  <function name="roleadduser" roleid="" userid="" /> 
  <function name="roledeleteuser" roleid="" userid="" /> 
  </functions>
```

## <a name="next-steps"></a>Następne kroki
- [Przewodnik po pojęciach pakietu BHOLD](../bhold/bhold-concepts-guide.md)
- [Historia wersji pakietu BHOLD](version-bhold-history.md)
