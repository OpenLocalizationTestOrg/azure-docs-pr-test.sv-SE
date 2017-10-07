---
title: aaaAuthor anpassad R-moduler i Azure Machine Learning | Microsoft Docs
description: "Snabbstart för att skapa anpassade R-moduler i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6cbc628a-7e60-42ce-9f90-20aaea7ba630
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 03/24/2017
ms.author: bradsev;ankarlof
ms.openlocfilehash: 8007c2abe20a4ab990f38b6d09bc4e6834ad2082
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a>Skapa anpassade R-moduler i Azure Machine Learning
Det här avsnittet beskrivs hur tooauthor och distribuera en anpassad R-modul i Azure Machine Learning. Det förklarar vilka anpassade R-moduler är och vilka filer som används toodefine dem. Den illustrerar hur tooconstruct hello filer som definierar en modul och hur tooregister hello-modul för distribution i en Machine Learning-arbetsytan. hello beskrivs element och attribut som används i hello definition av hello anpassad modul sedan i detalj. Hur diskuteras också toouse extra funktioner och filer och flera utdata. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a>Vad är en anpassad R-modul?
En **anpassad modul** är en användardefinierad modul som kan vara upp tooyour arbetsytan och köras som en del av en Azure Machine Learning-experiment. En **anpassad R-modul** är en anpassad modul som kör en användardefinierad R-funktion. **R** är ett programmeringsspråk för statistisk databehandling och bilder som ofta används av forskare statistiker och data för att implementera algoritmer. R är för närvarande hello enda språk som stöds i anpassade moduler, men stöd för ytterligare språk är schemalagd för framtida versioner.

Anpassade moduler har **förstklassigt status** i Azure Machine Learning i hello meningen att de kan användas som en annan modul. De kan köras med andra moduler som ingår i publicerade experiment eller visualiseringar. Du har kontroll över hello-algoritmen som implementeras av hello modul, hello inkommande och utgående portarna toobe används, hello modellering parametrar och andra olika runtime-funktioner. Ett experiment som innehåller anpassade moduler kan också publiceras till hello Cortana Intelligence Gallery lättare att dela.

## <a name="files-in-a-custom-r-module"></a>Filer i en anpassad R-modul
En anpassad R-modul har definierats som en .zip-fil som innehåller minst två filer:

* En **källfilen** som implementerar hello R-funktionen som exponeras av hello-modul
* En **XML-definitionsfilen** som beskriver hello anpassad modul gränssnitt

Ytterligare extra filer kan också tas med i hello ZIP-fil som innehåller funktioner som kan nås från hello anpassad modul. Det här alternativet diskuteras i hello **argument** tillhör hello referensavsnitt **element i XML-definitionsfilen för hello** följande hello quickstart exempel.

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a>Snabbstart exempel: definiera, paketera och registrera en anpassad R-modul
Det här exemplet illustrerar hur tooconstruct hello filer som krävs av en anpassad R-modul, paketera dem till en zip-filen och sedan registrera hello-modul i Machine Learning-arbetsytan. hello exempel zip-paketet och exempel filer kan hämtas från [hämta CustomAddRows.zip filen](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).

## <a name="hello-source-file"></a>hello källfilen
Överväg hello exempel på en **anpassad Lägg till rader** modul som ändrar hello standardimplementering av hello **lägga till rader** modulen används tooconcatenate rader (observationer) från två datamängder (dataramar). hello standard **lägga till rader** modulen lägger till hello rader hello andra inkommande dataset toohello End hello första inkommande datauppsättningen med hello `rbind` algoritmen. hello anpassade `CustomAddRows` funktionen accepterar två datauppsättningar på samma sätt, men även accepterar en parameter med booleska växlingsutrymme som en ytterligare indata. Om hello växlingen parameter har angetts för**FALSKT**, den returnerar hello samma datamängd som hello standardimplementeringen. Men om hello växlingen parametern **SANT**, hello funktionen lägger till rader i första inkommande dataset toohello slutet av hello andra dataset i stället. Hej CustomAddRows.R-fil som innehåller hello implementering av hello R `CustomAddRows` funktion som exponeras av hello **anpassad Lägg till rader** modulen har hello följande R-koden.

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="hello-xml-definition-file"></a>hello XML-definitionsfilen
tooexpose detta `CustomAddRows` fungera som en XML-definitionsfilen en Azure Machine Learning-modul måste skapas toospecify hur hello **anpassad Lägg till rader** modulen ska ser ut och fungerar. 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother. Dataset 2 is concatenated tooDataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify hello base language, script file and R function toouse for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: hello values of hello id attributes in hello Input and Arg elements must match hello parameter names in hello R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>hello combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


Det är kritiska toonote som hello värdet för hello **id** attribut för hello **indata** och **%d{arg/** element i XML-filen för hello måste matcha hello funktionen parameternamn för hello R kod i hello CustomAddRows.R filen exakt: (*dataset1*, *dataset2*, och *växlingen* i hello exemplet). På liknande sätt hello värdet för hello **entryPoint** attribut för hello **språk** elementet måste matcha hello namnet på hello funktion i hello R-skriptet exakt: (*CustomAddRows* i hello exemplet). 

Däremot hello **id** attribut för hello **utdata** element överensstämmer inte tooany variabler i hello R-skriptet. När flera utdata krävs bara returnera en lista från hello R-funktionen med resultat som placeras *i hello samma ordning* som **utdata** element deklareras i hello XML-fil.

### <a name="package-and-register-hello-module"></a>Paket- och registrera hello-modul
Spara dessa två filer som *CustomAddRows.R* och *CustomAddRows.xml* och sedan zip hello två filer tillsammans i en *CustomAddRows.zip* fil.

tooregister dem i Machine Learning-arbetsytan, gå tooyour arbetsytan i hello Machine Learning Studio klickar du på hello **+ ny** knappen hello längst ned och välj **modul -> från ZIP-PAKETET** tooupload hello nya **anpassad Lägg till rader** modul.

![Ladda upp Zip](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

Hej **anpassad Lägg till rader** modulen är nu redo toobe nås av Machine Learning-experiment.

## <a name="elements-in-hello-xml-definition-file"></a>Element i XML-definitionsfilen för hello
### <a name="module-elements"></a>Modulen element
Hej **modulen** elementet har använt toodefine en anpassad modul i hello XML-fil. Flera moduler kan definieras i en XML-fil med flera **modulen** element. Varje modul på arbetsytan måste ha ett unikt namn. Registrera en anpassad modul med samma namn som en befintlig anpassad modul hello och den ersätter hello befintlig modul med hello ny. Anpassade moduler kan dock vara registrerade med samma namn som en befintlig Azure Machine Learning-modul hello. Om så är fallet bör de visas i hello **anpassad** hello modulpaletten kategori.

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother...</Description>/> 


Inom hello **modulen** element, du kan ange två ytterligare valfritt element:

* en **ägare** element som är inbäddad i hello-modul  
* en **beskrivning** element som innehåller text som visas i snabbt hjälp för hello modulen och när du hovrar över hello-modul i hello Machine Learning-Användargränssnittet.

Regler för tecken-gränser i hello modulen element:

* Hej värdet för hello **namn** attribut i hello **modulen** element får inte överstiga 64 tecken. 
* Hej innehållet i hello **beskrivning** element får inte överstiga 128 tecken.
* Hej innehållet i hello **ägare** element får inte överstiga 32 tecken.

En modul resultaten kan vara deterministisk eller nondeterministic.* * som standard, alla moduler anses toobe deterministisk. Det vill säga ska med en oföränderlig uppsättning indataparametrar och data hello modulen returnera hello samma resultat eacRAND eller en functionh körningen. Det här beteendet, Azure Machine Learning Studio Kör endast moduler som har markerats som deterministisk om en parameter eller hello indata har ändrats. Returnerar hello cachelagrade resultaten ger också mycket snabbare körning av experiment.

Det finns funktioner som är icke-deterministisk, till exempel SLUMP eller en funktion som returnerar hello aktuellt datum och tid. Om din modulen använder en icke-deterministisk funktion, kan du ange att hello-modulen är icke-deterministiska genom att ange hello valfria **isDeterministic** attribut för**FALSKT**. Detta garanterar att hello-modulen körs när hello experiment körs, även om hello modulindata och parametrar inte har ändrats. 

### <a name="language-definition"></a>Definition av språk
Hej **språk** -elementet i XML-definitionsfilen är används toospecify hello anpassad modul språk. R är för närvarande hello stöds bara språk. Hej värdet för hello **sourceFile** attributet måste vara hello namn på hello R-fil som innehåller hello funktionen toocall när hello-modulen körs. Den här filen måste vara en del av hello zip-paketet. Hej värdet för hello **entryPoint** attributet är hello namnet på hello funktion som anropas och måste matcha en giltig funktion som definierats med i hello källfil.

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a>Portar
hello inkommande och utgående portar för en anpassad modul har angetts i underordnade element för hello **portar** avsnitt i hello XML-definitionsfilen. hello ordningen på elementen anger hello layout erfarna (UX) av användare. hello första underordnade **inkommande** eller **utdata** som anges i hello **portar** element i XML-filen för hello blir hello vänstra indataporten i hello Machine Learning UX.
Varje indata och utdataport kan ha en valfri **beskrivning** underordnat element som anger hello text som visas när du hovrar hello markören över hello port i hello Machine Learning-Användargränssnittet.

**Portarna regler**:

* Maximalt antal **inkommande och utgående portarna** är 8 för varje.

### <a name="input-elements"></a>Inkommande element
Portar kan du arbetsytan och toopass data tooyour R-funktionen. Hej **datatyper** som stöds för inkommande portar är följande: 

**DataTable:** tooyour R funktionen skickas i den här typen som en data.frame. I själva verket några typer (till exempel CSV-filer eller ARFF-filer) som stöds av Machine Learning och som är kompatibla med **DataTable** är konverterade tooa data.frame automatiskt. 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

Hej **id** attribut som är associerade med varje **DataTable** indataport måste ha ett unikt värde och värdet måste matcha motsvarande namngiven parameter i R-funktionen.
Valfria **DataTable** portar som inte angavs som indata i ett experiment har hello värdet **NULL** skickade toohello R-funktionen och valfria zip portar ignoreras om hello-indata inte är anslutet. Hej **isOptional** attributet är valfri för båda hello **DataTable** och **Zip** datatyper och är *FALSKT* som standard.

**ZIP:** anpassade moduler kan acceptera en zip-fil som indata. Den här indata packat till hello R arbetskatalog för din funktion

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files toobe extracted toohello R working directory.</Description>
           </Input>

För anpassade R-moduler saknar hello-id för en Zip-port toomatch eventuella parametrar för hello R-funktionen. Det beror på att hello zip-filen är automatiskt extraherade toohello R arbetskatalogen.

**Regler för inkommande:**

* Hej värdet för hello **id** attribut för hello **indata** elementet måste vara ett giltigt R variabelnamn.
* Hej värdet för hello **id** attribut för hello **indata** element får inte vara längre än 64 tecken.
* Hej värdet för hello **namn** attribut för hello **indata** element får inte vara längre än 64 tecken.
* Hej innehållet i hello **beskrivning** element får inte vara längre än 128 tecken
* Hej värdet för hello **typen** attribut för hello **indata** elementet måste vara *Zip* eller *DataTable*.
* hello värdet för hello **isOptional** attribut för hello **indata** elementet krävs inte (och är *FALSKT* som standard om inget värde anges), men om detta anges måste det vara *SANT* eller *FALSKT*.

### <a name="output-elements"></a>Utdata element
**Standard utgående portar:** portar är mappade toohello returvärden från din R-funktionen, som sedan kan användas av efterföljande moduler. *DataTable* är hello endast standardutdata porttyp som stöds för närvarande. (Stöd för *inlärning* och *transformerar* är kommande.) En *DataTable* utdata har definierats som:

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

Utdata i anpassade R-moduler hello värdet för hello **id** inte har toocorrespond med något i hello R-skriptet, men det måste vara unikt. För en enskild modulen utdata hello returvärde från hello R-funktionen måste vara en *data.frame*. I ordning toooutput fler än ett objekt av-datatypen stöds, hello lämpliga portar måste toobe som angetts i XML-definitionsfilen för hello och hello-objekten måste toobe returneras som en lista. hello utdata objekt tilldelas toooutput portar från vänster tooright, reflektion hello ordning där hello objekten placeras i hello returnerade lista.

Om du vill toomodify hello exempelvis **anpassad Lägg till rader** modulen toooutput hello ursprungliga två datamängder *dataset1* och *dataset2*, dessutom toohello nya ansluten DataSet, *dataset*, (i en ordning, från vänster tooright som: *dataset*, *dataset1*, *dataset2*), definiera hello utgående portar i hello CustomAddRows.xml fil på följande sätt:

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


Och returnerar hello lista över objekt i en lista i hello i 'CustomAddRows.R' i rätt ordning:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

**Visualiseringen utdata:** du kan också ange en utdataporten av typen *visualiseringen*, som visar hello utdata från hello R grafik enheten och konsolen utdata. Den här porten är inte en del av hello R funktionen utdata och inte störa hello ordning hello andra utgående porttyper. tooadd en visualiseringen port toohello anpassade moduler, lägga till en **utdata** element med värdet *visualiseringen* för dess **typen** attribut:

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View hello R console graphics device output.</Description>
    </Output>

**Utgående regler:**

* Hej värdet för hello **id** attribut för hello **utdata** elementet måste vara ett giltigt R variabelnamn.
* Hej värdet för hello **id** attribut för hello **utdata** element får inte vara längre än 32 tecken.
* Hej värdet för hello **namn** attribut för hello **utdata** element får inte vara längre än 64 tecken.
* Hej värdet för hello **typen** attribut för hello **utdata** elementet måste vara *visualiseringen*.

### <a name="arguments"></a>Argument
Ytterligare information kan skickas toohello R-funktionen via modulparametrar som definieras i hello **argument** element. Dessa parametrar visas i hello längst till höger egenskapsrutan för hello Machine Learning Användargränssnittet när hello modul väljs. Argument kan vara någon av hello stöds eller så kan du skapa en anpassad uppräkning vid behov. Liknande toohello **portar** element, **argument** element kan ha en valfri **beskrivning** element som anger hello text som visas när du hovrar hello mus över hello parameternamnet.
Valfria egenskaper för en modul som standardvärde, minValue och maxValue kan läggas till tooany argumentet som attribut tooa **egenskaper** element. Giltiga egenskaperna för hello **egenskaper** element beroende hello Argumenttypen och beskrivs med hello stöds Argumenttyperna i hello nästa avsnitt. Argument med hello **isOptional** egenskapsuppsättning för**”true”** kräver inte hello användaren tooenter ett värde. Om ett värde inte tillhandahålls toohello argumentet, skickas toohello funktionens startadress inte i hello argumentet. Hello funktionens startadress-argument som är valfritt måste toobe explicit hanteras av hello funktion, t.ex. tilldelade standardvärdet NULL i hello post punkt funktionsdefinitionen. Ett valfritt argument kommer bara att verkställa hello andra argumentet villkor, d.v.s. min eller max, om ett värde som tillhandahålls av hello användare.
Det är viktigt att var och en av hello parametrar har unika ID-värden som är kopplade till dem som in- och utdataenheter. I vår Snabbstartsguide exempel hello associerade id-parametern har *växlingen*.

### <a name="arg-element"></a>%D{arg/ element
En modul-parameter har definierats med hello **%d{arg/** underordnat element för hello **argument** avsnitt i hello XML-definitionsfilen. Precis som med hello underordnade element i hello **portar** avsnittet, hello sorteringen av parametrar i hello **argument** avsnittet definierar hello layout påträffades i hello UX. hello parametrar visas, uppifrån nedåt i hello Användargränssnittet i samma ordning i vilken de definieras hello i hello XML-fil. hello-typer som stöds av Machine Learning för parametrar i den här listan. 

**int** – ett heltal (32-bitars) typparameter.

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* *Valfria egenskaper*: **min**, **max**, **standard** och **isOptional**

**dubbla** – en parameter av typen double.

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* *Valfria egenskaper*: **min**, **max**, **standard** och **isOptional**

**bool** – en boolesk parameter som representeras av en kryssruta i UX.

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* *Valfria egenskaper*: **standard** -falskt om inte angetts

**strängen**: en vanlig sträng

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* *Valfria egenskaper*: **standard** och **isOptional**

**ColumnPicker**: en parameter för val av kolumn. Den här typen visas i hello UX med väljaren för en kolumn. Hej **egenskapen** element är används här toospecify hello-id för hello port från vilken kolumner väljs, där hello-porttyp måste vara *DataTable*. hello resultatet av hello kolumnmarkeringen skickas toohello R-funktionen som en lista med strängar som innehåller hello valt kolumnnamn. 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* *Obligatoriska egenskaper*: **portId** -matchar hello-id för ett indata-element med typen *DataTable*.
* *Valfria egenskaper*:
  
  * **allowedTypes** -filter hello kolumnen typer från vilken du kan välja. Giltiga värden är: 
    
    * numeriskt
    * Booleskt värde
    * Kategoriska
    * Sträng
    * Etikett
    * Funktion
    * Poäng
    * Alla
  * **standard** -giltig standardvalen för hello kolumnen Väljaren inkluderar: 
    
    * Ingen
    * NumericFeature
    * NumericLabel
    * NumericScore
    * NumericAll
    * BooleanFeature
    * BooleanLabel
    * BooleanScore
    * BooleanAll
    * CategoricalFeature
    * CategoricalLabel
    * CategoricalScore
    * CategoricalAll
    * StringFeature
    * StringLabel
    * StringScore
    * StringAll
    * AllLabel
    * AllFeature
    * AllScore
    * Alla

**Listrutan**: ett användardefinierat uppräknade () listrutan. hello nedrullningsbara objekt har angetts i hello **egenskaper** element med ett **objektet** element. Hej **id** för varje **objektet** måste vara unika och en giltig R-variabel. Hej värdet för hello **namn** av en **objektet** fungerar både som hello text som visas och hello-värde som har överförts toohello R-funktionen.

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* *Valfria egenskaper*:
  * **standard** - hello värde hello standardegenskapen måste överensstämma med ett id-värde från en hello **objektet** element.

### <a name="auxiliary-files"></a>Extra filer
Alla filer som placeras i en anpassad modul ZIP-filen är pågående toobe tillgängliga för användning under körning. Alla finns katalogstrukturer bevaras. Det innebär att filen ursprung fungerar hello samma lokalt och i Azure Machine Learning-körning. 

> [!NOTE]
> Observera att alla filer är extraherade too'src' directory så att alla sökvägar som ska ha ' src /-prefix.
> 
> 

Anta exempelvis att du vill tooremove några rader med NAs från hello datamängd och även ta bort alla dubbla rader innan mata ut den till CustomAddRows och du redan har skrivit ett R-funktion som gör detta i en fil RemoveDupNARows.R:

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
Du kan källfil hello extra RemoveDupNARows.R i hello CustomAddRows funktionen:

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

Därefter ladda upp en zip-fil som innehåller 'CustomAddRows.R', 'CustomAddRows.xml' och 'RemoveDupNARows.R' som en anpassad R-modul.

## <a name="execution-environment"></a>Körningsmiljö
hello körningsmiljö för hello R-skriptet hello använder samma version av R som hello **köra R-skriptet** modul och kan använda hello samma paket som standard. Du kan också lägga till ytterligare R-paket tooyour anpassad modul genom att inkludera dem i hello anpassad modul zip-paketet. Bara läsa in dem i din R-skriptet som i din egen miljö för R. 

**Begränsningar av hello körningsmiljö** omfattar:

* Icke-beständiga filsystem: filer som skrivits när hello anpassad modul körs är inte beständiga över flera körningar av hello samma modul.
* Ingen nätverksåtkomst

