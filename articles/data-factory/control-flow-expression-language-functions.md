---
title: Uttryck och funktioner i Azure Data Factory | Microsoft Docs
description: "Den här artikeln innehåller information om uttryck och funktioner som du kan använda för att skapa data factory entiteter."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 78f21576bb7d839e5b5c4d8c2b721e381d663406
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/23/2018
---
# <a name="expressions-and-functions-in-azure-data-factory"></a>Uttryck och funktioner i Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1 – allmänt tillgänglig](v1/data-factory-functions-variables.md)
> * [Version 2 – förhandsversion](control-flow-expression-language-functions.md)

Den här artikeln innehåller information om uttryck och funktioner som stöds av Azure Data Factory (version 2). 

## <a name="introduction"></a>Introduktion
JSON-värden i definitionen kan vara literal eller uttryck som utvärderas vid körning. Exempel:  
  
```json
"name": "value"
```

 (eller)  
  
```json
"name": "@pipeline().parameters.password"
```


> [!NOTE]
> Den här artikeln gäller för version 2 av Data Factory, som för närvarande är en förhandsversion. Om du använder version 1 av Data Factory-tjänsten, som är allmänt tillgänglig (GA), se [funktioner och variabler i Data Factory V1](v1/data-factory-functions-variables.md).


## <a name="expressions"></a>Uttryck  
Uttryck kan finnas var som helst i ett strängvärde i JSON och alltid resultera i en annan JSON-värde. Om ett JSON-värde är ett uttryck, brödtexten i uttrycket ska extraheras genom att ta bort @-tecknet (@). Om det krävs en teckensträng som börjar med @, det måste avgränsas med@. I följande exempel visas hur uttryck utvärderas.  
  
|JSON-värde|Resultat|  
|----------------|------------|  
|”parametrar”|Tecken-parametrar, returneras.|  
|”Parametrar [1]”|Tecken-parametrar [1] ”returneras.|  
|"@@"|En 1 tecken-sträng som innehåller ”@” returneras.|  
|" @"|En 2 teckensträng som innehåller ”@” returneras.|  
  
 Uttryck kan även visas i strängar, med hjälp av en funktion som kallas *string interpolerade* där uttryck omslutas i `@{ ... }`. Exempel: `"name" : "First Name: @{pipeline().parameters.firstName} Last Name: @{pipeline().parameters.lastName}"`  
  
 Med strängen interpolerade är resultatet alltid en sträng. Säg att jag har definierat `myNumber` som `42` och `myString` som `foo`:  
  
|JSON-värde|Resultat|  
|----------------|------------|  
|”@pipeline(). parameters.myString”| Returnerar `foo` som en sträng.|  
|”@{pipeline ()-.parameters.myString}”| Returnerar `foo` som en sträng.|  
|”@pipeline(). parameters.myNumber”| Returnerar `42` som en *nummer*.|  
|”@{pipeline ()-.parameters.myNumber}”| Returnerar `42` som en *sträng*.|  
|”Svaret är: @{pipeline ()-.parameters.myNumber}”| Returnerar strängen `Answer is: 42`.|  
|”@concat(' Svaret är: ', string(pipeline().parameters.myNumber))”| Returnerar strängen`Answer is: 42`|  
|”Svaret är: @@ {pipeline ()-.parameters.myNumber}”| Returnerar strängen `Answer is: @{pipeline().parameters.myNumber}`.|  
  
### <a name="examples"></a>Exempel

#### <a name="a-dataset-with-a-parameter"></a>En datamängd med en parameter
I följande exempel visas BlobDataset tar en parameter med namnet **sökvägen**. Dess värde används för att ange ett värde för den **folderPath** egenskapen med hjälp av följande uttryck: `@{dataset().path}`. 

```json
{
    "name": "BlobDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "@dataset().path"
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            }
        }
    }
}
```

#### <a name="a-pipeline-with-a-parameter"></a>En pipeline med en parameter
I följande exempel tar pipeline **inputPath** och **outputPath** parametrar. Den **sökväg** parametriserade blob dataset anges med hjälp av värdena för dessa parametrar. Syntax används här: `pipeline().parameters.parametername`. 

```json
{
    "name": "Adfv2QuickStartPipeline",
    "properties": {
        "activities": [
            {
                "name": "CopyFromBlobToBlob",
                "type": "Copy",
                "inputs": [
                    {
                        "referenceName": "BlobDataset",
                        "parameters": {
                            "path": "@pipeline().parameters.inputPath"
                        },
                        "type": "DatasetReference"
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "BlobDataset",
                        "parameters": {
                            "path": "@pipeline().parameters.outputPath"
                        },
                        "type": "DatasetReference"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                }
            }
        ],
        "parameters": {
            "inputPath": {
                "type": "String"
            },
            "outputPath": {
                "type": "String"
            }
        }
    }
}
```
  
## <a name="functions"></a>Funktioner  
 Du kan anropa funktioner i uttryck. Följande avsnitt innehåller information om de funktioner som kan användas i ett uttryck.  

## <a name="string-functions"></a>Strängfunktioner  
 Följande funktioner gäller endast för strängar. Du kan också använda ett antal samling funktioner i strängar.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|concat|Kombinerar valfritt antal strängar tillsammans. Om exempelvis parameter1 är `foo,` returnerar följande uttryck `somevalue-foo-somevalue`:`concat('somevalue-',pipeline().parameters.parameter1,'-somevalue')`<br /><br /> **Parameternummer**: 1...*n*<br /><br /> **Namnet**: sträng*n*<br /><br /> **Beskrivning**: krävs. Strängar att kombinera till en sträng.|  
|delsträngen|Returnerar en delmängd av tecken från en sträng. Till exempel följande uttryck:<br /><br /> `substring('somevalue-foo-somevalue',10,3)`<br /><br /> Returnerar:<br /><br /> `foo`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Strängen där delsträngen tas.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: startIndex<br /><br /> **Beskrivning**: krävs. Index för där delsträngen börjar i parameter 1.<br /><br /> **Parameternummer**: 3<br /><br /> **Namnet**: längd<br /><br /> **Beskrivning**: krävs. Längden på delsträngen.|  
|Ersätt|Ersätter en sträng med en given sträng. Till exempel uttrycket:<br /><br /> `replace('the old string', 'old', 'new')`<br /><br /> Returnerar:<br /><br /> `the new string`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs.  Om parametern 2 finns i parameter 1, den sträng som är sökte efter parametern 2 och uppdateras med parametern 3.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: gamla sträng<br /><br /> **Beskrivning**: krävs. Strängen som ska ersätta med parametern 3 när en matchning hittas i parameter 1<br /><br /> **Parameternummer**: 3<br /><br /> **Namnet**: ny sträng<br /><br /> **Beskrivning**: krävs. Den sträng som används för att ersätta strängen i parametern 2 när en matchning hittas i parameter 1.|  
|GUID| Genererar en globalt unik sträng (aka. GUID). Till exempel följande utdata genereras `c2ecc88d-88c8-4096-912c-d6f2e2b138ce`:<br /><br /> `guid()`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. En enda Formatspecificeraren som anger [formatera värdet för detta Guid](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). Formatparametern kan vara ”N”, ”D”, ”B”, ”P” eller ”X”. Om formatet inte är anges, används ”D”.|  
|toLower|Konverterar en sträng till gemener. Till exempel följande returnerar `two by two is four`:`toLower('Two by Two is Four')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Strängen som ska konverteras till lägre skiftläge. Om ett tecken i strängen inte har en motsvarighet på gemener, är det inkluderade oförändrad i den returnerade strängen.|  
|toUpper|Konverterar en sträng till versaler. Följande uttryck returnerar till exempel `TWO BY TWO IS FOUR`:`toUpper('Two by Two is Four')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Strängen som ska konverteras till övre skiftläge. Om ett tecken i strängen inte har motsvarande versaler, är det inkluderade oförändrad i den returnerade strängen.|  
|indexOf|Hitta index för ett värde inom strängen fall insensitively. Följande uttryck returnerar till exempel `7`:`indexof('hello, world.', 'world')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Den sträng som kan innehålla värdet.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Värdet att söka index.|  
|lastIndexOf|Hitta senaste index för ett värde inom strängen fall insensitively. Följande uttryck returnerar till exempel `3`:`lastindexof('foofoo', 'foo')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Den sträng som kan innehålla värdet.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Värdet att söka index.|  
|StartsWith|Kontrollerar om strängen börjar med ett värde ärende insensitively. Följande uttryck returnerar till exempel `true`:`lastindexof('hello, world', 'hello')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Den sträng som kan innehålla värdet.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Värdet för strängen kan börja med.|  
|EndsWith|Kontrollerar om strängen slutar med ett värde ärende insensitively. Följande uttryck returnerar till exempel `true`:`lastindexof('hello, world', 'world')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Den sträng som kan innehålla värdet.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Värdet för strängen kan avslutas med.|  
|split|Delar upp strängen med avgränsare. Följande uttryck returnerar till exempel `["a", "b", "c"]`:`split('a;b;c',';')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Den sträng som delas.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Avgränsare.|  
  
  
## <a name="collection-functions"></a>Samlingen funktioner  
 Dessa funktioner fungerar på samlingar som matriser, strängar, och ibland ordlistor.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|innehåller|Returnerar true om uppslagslistan innehåller en nyckel, lista innehåller värdet eller strängen innehåller delsträngen. Exempelvis returnerar följande uttryck`true:``contains('abacaba','aca')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: inom samlingen<br /><br /> **Beskrivning**: krävs. Att söka i samlingen.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: Sök objekt<br /><br /> **Beskrivning**: krävs. Objektet för att hitta i den **inom samlingen**.|  
|Längd|Returnerar antalet element i en matris eller sträng. Följande uttryck returnerar till exempel `3`:`length('abc')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen<br /><br /> **Beskrivning**: krävs. Samlingen för att hämta längden för.|  
|tom|Returnerar true om objektet, matris eller sträng är tom. Följande uttryck returnerar till exempel `true`:<br /><br /> `empty('')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen<br /><br /> **Beskrivning**: krävs. Samlingen för att kontrollera om den är tom.|  
|skärningspunkten|Returnerar en enda matris eller ett objekt med vanliga element mellan matriser eller objekt som överförs till den. Den här funktionen returnerar till exempel `[1, 2]`:<br /><br /> `intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])`<br /><br /> Parametrar för funktionen kan antingen vara en uppsättning objekt eller en uppsättning matriser (inte en blandning av dessa). Om det finns två objekt med samma namn, visas det sista objektet med det namnet i det sista objektet.<br /><br /> **Parameternummer**: 1...*n*<br /><br /> **Namnet**: samlingen*n*<br /><br /> **Beskrivning**: krävs. Samlingar att utvärdera. Ett objekt måste vara i alla samlingar som skickades till visas i resultatet.|  
|Union|Returnerar en enda matris eller ett objekt med alla element i matrisen eller objekt överförs. Den här funktionen returnerar till exempel`[1, 2, 3, 10, 101]:`<br /><br /> :`union([1, 2, 3], [101, 2, 1, 10])`<br /><br /> Parametrar för funktionen kan antingen vara en uppsättning objekt eller en uppsättning matriser (inte en blandning av dessa). Om det finns två objekt med samma namn i det slutgiltiga resultatet, visas det sista objektet med det namnet i det sista objektet.<br /><br /> **Parameternummer**: 1...*n*<br /><br /> **Namnet**: samlingen*n*<br /><br /> **Beskrivning**: krävs. Samlingar att utvärdera. Ett objekt som visas i någon av samlingarna som visas i resultatet.|  
|första|Returnerar det första elementet i matrisen eller sträng skickades. Den här funktionen returnerar till exempel `0`:<br /><br /> `first([0,2,3])`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen<br /><br /> **Beskrivning**: krävs. Göra det första objektet från samlingen.|  
|senaste|Returnerar det sista elementet i matrisen eller sträng skickades. Den här funktionen returnerar till exempel `3`:<br /><br /> `last('0123')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen<br /><br /> **Beskrivning**: krävs. Göra det sista objektet från samlingen.|  
|ta|Returnerar först **antal** element från en matris eller sträng skickades, till exempel den här funktionen returnerar `[1, 2]`:`take([1, 2, 3, 4], 2)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen<br /><br /> **Beskrivning**: krävs. Samlingen för att ta först **antal** objekt från.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: antal<br /><br /> **Beskrivning**: krävs. Antal objekt som ska ta den **samling**. Måste vara ett positivt heltal.|  
|Hoppa över|Returnerar element i matrisen med början vid index **antal**, till exempel den här funktionen returnerar `[3, 4]`:<br /><br /> `skip([1, 2 ,3 ,4], 2)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen<br /><br /> **Beskrivning**: krävs. Samlingen för att hoppa över först **antal** objekt från.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: antal<br /><br /> **Beskrivning**: krävs. Antalet objekt som ska tas bort från framför **samling**. Måste vara ett positivt heltal.|  
  
## <a name="logical-functions"></a>Logiska funktioner  
 Dessa funktioner är användbara i villkor, de kan användas för att utvärdera någon typ av logik.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|lika med|Returnerar true om två värden är lika. Till exempel om parameter1 foo följande uttryck returnerar `true`:`equals(pipeline().parameters.parameter1), 'foo')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: objekt-1<br /><br /> **Beskrivning**: krävs. Det objekt som jämför med **objekt 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: objekt 2<br /><br /> **Beskrivning**: krävs. Det objekt som jämför med **objekt 1**.|  
|mindre|Returnerar true om det första argumentet är mindre än andra. Observera att värden kan bara vara av typen heltal, flyttal eller string. Följande uttryck returnerar till exempel `true`:`less(10,100)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: objekt-1<br /><br /> **Beskrivning**: krävs. Objektet för att kontrollera om det är mindre än **objekt 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: objekt 2<br /><br /> **Beskrivning**: krävs. Objektet för att kontrollera om det är större än **objekt 1**.|  
|lessOrEquals|Returnerar true om det första argumentet är mindre än eller lika med andra. Observera att värden kan bara vara av typen heltal, flyttal eller string. Följande uttryck returnerar till exempel `true`:`lessOrEquals(10,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: objekt-1<br /><br /> **Beskrivning**: krävs. Objektet som ska kontrollera om det är mindre eller lika med **objekt 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: objekt 2<br /><br /> **Beskrivning**: krävs. Objektet för att kontrollera om den är större än eller lika med **objekt 1**.|  
|större|Returnerar true om det första argumentet är större än andra. Observera att värden kan bara vara av typen heltal, flyttal eller string. Följande uttryck returnerar till exempel `false`:`greater(10,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: objekt-1<br /><br /> **Beskrivning**: krävs. Objektet för att kontrollera om det är större än **objekt 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: objekt 2<br /><br /> **Beskrivning**: krävs. Objektet för att kontrollera om det är mindre än **objekt 1**.|  
|greaterOrEquals|Returnerar true om det första argumentet är större än eller lika med andra. Observera att värden kan bara vara av typen heltal, flyttal eller string. Följande uttryck returnerar till exempel `false`:`greaterOrEquals(10,100)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: objekt-1<br /><br /> **Beskrivning**: krävs. Objektet för att kontrollera om den är större än eller lika med **objekt 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: objekt 2<br /><br /> **Beskrivning**: krävs. Objektet för att kontrollera om det är mindre än eller lika med **objekt 1**.|  
|och|Returnerar true om båda parametrarna är uppfyllda. Båda argument måste vara booleska värden. Följande returnerar `false`:`and(greater(1,10),equals(0,0))`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: booleskt 1<br /><br /> **Beskrivning**: krävs. Det första argumentet måste vara `true`.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: booleskt 2<br /><br /> **Beskrivning**: krävs. Det andra argumentet måste vara `true`.|  
|eller|Returnerar SANT om någon av parametrarna är sant. Båda argument måste vara booleska värden. Följande returnerar `true`:`or(greater(1,10),equals(0,0))`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: booleskt 1<br /><br /> **Beskrivning**: krävs. Det första argumentet som kan vara `true`.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: booleskt 2<br /><br /> **Beskrivning**: krävs. Det andra argumentet får vara `true`.|  
|inte|Returnerar true om parametern är `false`. Båda argument måste vara booleska värden. Följande returnerar `true`:`not(contains('200 Success','Fail'))`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: booleskt<br /><br /> **Beskrivning**: returnerar true om parametern är `false`. Båda argument måste vara booleska värden. Följande returnerar `true`:`not(contains('200 Success','Fail'))`|  
|Om|Returnerar ett angivet värde baserat på om uttryck resultatet i `true` eller `false`.  Till exempel följande returnerar `"yes"`:`if(equals(1, 1), 'yes', 'no')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: uttryck<br /><br /> **Beskrivning**: krävs. Ett booleskt värde som anger vilket värde som returneras av uttrycket.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: True<br /><br /> **Beskrivning**: krävs. Värdet som returneras om uttrycket är `true`.<br /><br /> **Parameternummer**: 3<br /><br /> **Namnet**: False<br /><br /> **Beskrivning**: krävs. Värdet som returneras om uttrycket är `false`.|  
  
## <a name="conversion-functions"></a>Konverteringsfunktioner  
 Dessa funktioner används för att konvertera mellan var och en av de inbyggda typerna för språk:  
  
-   sträng  
  
-   heltal  
  
-   flyt  
  
-   boolesk  
  
-   matriser  
  
-   ordlistor  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|int|Konvertera parametern till ett heltal. Exempelvis returnerar följande uttryck 100 som ett tal i stället för en sträng:`int('100')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: värde<br /><br /> **Beskrivning**: krävs. Det värde som konverteras till ett heltal.|  
|sträng|Konvertera parametern till en sträng. Följande uttryck returnerar till exempel `'10'`: `string(10)` du kan också konvertera ett objekt till en sträng, till exempel om den **foo** parametern är ett objekt med en egenskap `bar : baz`, och sedan följande skulle returnera `{"bar" : "baz"}``string(pipeline().parameters.foo)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: värde<br /><br /> **Beskrivning**: krävs. Det värde som konverteras till en sträng.|  
|JSON|Konvertera parametern till ett JSON-värde för typen. Det är motsatsen till string(). Följande uttryck returnerar till exempel `[1,2,3]` som en matris i stället för en sträng:<br /><br /> `parse('[1,2,3]')`<br /><br /> På samma sätt kan du konvertera en sträng till ett objekt. Till exempel `json('{"bar" : "baz"}')` returnerar:<br /><br /> `{ "bar" : "baz" }`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Den sträng som konverteras till en inbyggd typ-värde.<br /><br /> Json-funktionen stöder xml-indata. Till exempel parametervärdet för:<br /><br /> `<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>`<br /><br /> konverteras till följande json:<br /><br /> `{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|flyt|Konvertera argumentet parametern till ett flyttal. Följande uttryck returnerar till exempel `10.333`:`float('10.333')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: värde<br /><br /> **Beskrivning**: krävs. Det värde som har konverterats till ett flyttal.|  
|bool|Konvertera parametern till ett booleskt värde. Följande uttryck returnerar till exempel `false`:`bool(0)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: värde<br /><br /> **Beskrivning**: krävs. Det värde som har konverterats till ett booleskt värde.|  
|Slå samman|Returnerar det första icke-null-objektet i argument som skickas. Obs: en tom sträng inte är null. Till exempel om parametrar 1 och 2 inte har definierats, returneras `fallback`:`coalesce(pipeline().parameters.parameter1', pipeline().parameters.parameter2 ,'fallback')`<br /><br /> **Parameternummer**: 1...*n*<br /><br /> **Namnet**: objektet*n*<br /><br /> **Beskrivning**: krävs. Objekt som ska eftersökas `null`.|  
|Base64|Returnerar base64-representation av Indatasträngen. Följande uttryck returnerar till exempel `c29tZSBzdHJpbmc=`:`base64('some string')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: String 1<br /><br /> **Beskrivning**: krävs. Strängen som koda till base64-representation.|  
|base64ToBinary|Returnerar en a binär representation av en base64-kodad sträng. Följande uttryck returnerar till exempel binär representation av en sträng: `base64ToBinary('c29tZSBzdHJpbmc=')`.<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Base64-kodad sträng.|  
|base64ToString|Returnerar en strängrepresentation av en based64 kodad sträng. Följande uttryck returnerar till exempel en sträng: `base64ToString('c29tZSBzdHJpbmc=')`.<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Base64-kodad sträng.|  
|Binär|Returnerar en a binär representation av ett värde.  Följande uttryck returnerar till exempel en a binär representation av en sträng:`binary(‘some string’).`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: värde<br /><br /> **Beskrivning**: krävs. Det värde som konverteras till binär.|  
|dataUriToBinary|Returnerar en a binär representation av en data-URI. Exempelvis returnerar följande uttryck binär representation av en sträng:`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Data-URI att konvertera till binär representation.|  
|dataUriToString|Returnerar en strängrepresentation av en data-URI. Följande uttryck returnerar till exempel en sträng:`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br />**Beskrivning**: krävs. URI för att konvertera till sträng som innehåller data.|  
|dataUri|Returnerar en data-URI för ett värde. Exempelvis returnerar följande uttryck data:`text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=: dataUri('some string')`<br /><br /> **Parameternummer**: 1<br /><br />**Namnet**: värde<br /><br />**Beskrivning**: krävs. Värdet som ska konverteras till data-URI.|  
|decodeBase64|Returnerar en strängrepresentation av en inkommande based64-sträng. Följande uttryck returnerar till exempel `some string`:`decodeBase64('c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: returnerar en strängrepresentation av en inkommande based64-sträng.|  
|encodeUriComponent|Visar URL: en sträng som skickas. Följande uttryck returnerar till exempel `You+Are%3ACool%2FAwesome`:`encodeUriComponent('You Are:Cool/Awesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Strängen som escape-URL-osäkra tecken från.|  
|decodeUriComponent|UN-URL-visar den sträng som skickas. Följande uttryck returnerar till exempel `You Are:Cool/Awesome`:`encodeUriComponent('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. Strängen som avkoda URL-osäkra tecken från.|  
|decodeDataUri|Returnerar en a binär representation av ett indata-URI-strängen. Följande uttryck returnerar till exempel binär representation av `some string`:`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br /> **Beskrivning**: krävs. DataURI att avkoda i en a binär representation.|  
|uriComponent|Returnerar en URI kodad representation av ett värde. Exempelvis returnerar följande uttryck`You+Are%3ACool%2FAwesome: uriComponent('You Are:Cool/Awesome ')`<br /><br /> Parameterinformation om: Number: 1, Name: sträng, beskrivning: krävs. Strängen som ska vara URI-kodad.|  
|uriComponentToBinary|Returnerar en a binär representation av en URI-kodad sträng. Följande uttryck returnerar till exempel en a binär representation av `You Are:Cool/Awesome`:`uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: sträng<br /><br />**Beskrivning**: krävs. URI-kodad sträng.|  
|uriComponentToString|Returnerar en strängrepresentation av en URI-kodad sträng. Följande uttryck returnerar till exempel `You Are:Cool/Awesome`:`uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Parameternummer**: 1<br /><br />**Namnet**: sträng<br /><br />**Beskrivning**: krävs. URI-kodad sträng.|  
|xml|Returnera ett xml-representation av värdet. Följande uttryck returnerar till exempel ett XML-innehåll representeras av `'\<name>Alan\</name>'`: `xml('\<name>Alan\</name>')`. Xml-funktionen stöder JSON-objekt samt indata. Till exempel parametern `{ "abc": "xyz" }` konverteras till en xml-innehåll`\<abc>xyz\</abc>`<br /><br /> **Parameternummer**: 1<br /><br />**Namnet**: värde<br /><br />**Beskrivning**: krävs. Värdet som ska konverteras till XML.|  
|XPath|Returnera en matris med xml-noder som matchar ett värde som xpath-uttryck som utvärderas till det xpath-uttrycken.<br /><br />  **Exempel 1**<br /><br /> Anta att värdet för parametern 'p1' är en strängrepresentation av följande XML-filen:<br /><br /> `<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>`<br /><br /> 1. Den här koden:`xpath(xml(pipeline().parameters.p1), '/lab/robot/name')`<br /><br /> Returnerar<br /><br /> `[ <name>R1</name>, <name>R2</name> ]`<br /><br /> medan<br /><br /> 2. Den här koden:`xpath(xml(pipeline().parameters.p1, ' sum(/lab/robot/parts)')`<br /><br /> Returnerar<br /><br /> `13`<br /><br /> <br /><br /> **Exempel 2**<br /><br /> Anges i följande XML-innehåll:<br /><br /> `<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>`<br /><br /> 1.  Den här koden:`@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')`<br /><br /> eller<br /><br /> 2. Den här koden:`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')`<br /><br /> Returnerar<br /><br /> `<Location xmlns="http://foo.com">bar</Location>`<br /><br /> och<br /><br /> 3. Den här koden:`@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')`<br /><br /> Returnerar<br /><br /> ``bar``<br /><br /> **Parameternummer**: 1<br /><br />**Namnet**: Xml<br /><br />**Beskrivning**: krävs. XML-filen som du vill utvärdera XPath-uttryck.<br /><br /> **Parameternummer**: 2<br /><br />**Namnet**: XPath<br /><br />**Beskrivning**: krävs. XPath-uttrycket ska utvärderas.|  
|matris|Konvertera parametern till en matris.  Följande uttryck returnerar till exempel `["abc"]`:`array('abc')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: värde<br /><br /> **Beskrivning**: krävs. Det värde som konverteras till en matris.|
|createArray|Skapar en matris av parametrarna.  Följande uttryck returnerar till exempel `["a", "c"]`:`createArray('a', 'c')`<br /><br /> **Parameternummer**: 1... n<br /><br /> **Namnet**: alla n<br /><br /> **Beskrivning**: krävs. Värdena som används för att kombinera till en matris.|

## <a name="math-functions"></a>Matematikfunktioner  
 Dessa funktioner kan användas för båda typer av siffror: **heltal** och **flyter**.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|lägg till|Returnerar resultatet av att lägga till de två talen. Den här funktionen returnerar till exempel `20.333`:`add(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: genom att hålla 1<br /><br /> **Beskrivning**: krävs. Det tal som ska lägga till **genom att hålla 2**.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: genom att hålla 2<br /><br /> **Beskrivning**: krävs. Det tal som ska lägga till **genom att hålla 1**.|  
|Sub|Returnerar resultatet av subtraktion av två tal. Den här funktionen returnerar till exempel: `-0.333`:<br /><br /> `sub(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namn på**: Minuend<br /><br /> **Beskrivning**: krävs. Numret som **Subtrahend** tas bort från.<br /><br /> **Parameternummer**: 2<br /><br /> **Namn på**: Subtrahend<br /><br /> **Beskrivning**: krävs. Det tal som ska ta bort från den **Minuend**.|  
|mul|Returnerar resultatet av multiplikationen av de två talen. Till exempel följande returnerar `103.33`:<br /><br /> `mul(10,10.333)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namn på**: Multiplicand 1<br /><br /> **Beskrivning**: krävs. Talet som multipliceras **Multiplicand 2** med.<br /><br /> **Parameternummer**: 2<br /><br /> **Namn på**: Multiplicand 2<br /><br /> **Beskrivning**: krävs. Talet som multipliceras **Multiplicand 1** med.|  
|div|Returnerar resultatet från divisionen av de två talen. Till exempel följande returnerar `1.0333`:<br /><br /> `div(10.333,10)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: täljaren<br /><br /> **Beskrivning**: krävs. Det tal som du delar med den **Divisor**.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: Divisor<br /><br /> **Beskrivning**: krävs. Det tal som ska divideras med det **täljaren** av.|  
|MOD|Returnerar resultatet av resten efter att divisionen av de två talen (modulo). Följande uttryck returnerar till exempel `2`:<br /><br /> `mod(10,4)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: täljaren<br /><br /> **Beskrivning**: krävs. Det tal som du delar med den **Divisor**.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: Divisor<br /><br /> **Beskrivning**: krävs. Det tal som ska divideras med det **täljaren** av. Efter att divisionen tas resten.|  
|min.|Det finns två olika mönster för att anropa den här funktionen: `min([0,1,2])` här min en matris. Det här uttrycket returnerar `0`. Den här funktionen kan också ta en kommaavgränsad lista med värden: `min(0,1,2)` returneras också 0. Observera att alla värden måste vara tal, så om parametern är en matris det måste innehålla endast siffror i den.<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen eller -värde<br /><br /> **Beskrivning**: krävs. Det kan antingen vara en matris med värden för att hitta det minsta värdet eller det första värdet i en mängd.<br /><br /> **Parameternummer**: 2...*n*<br /><br /> **Namnet**: värde*n*<br /><br /> **Beskrivning**: valfria. Om den första parametern är ett värde, du kan sedan skicka ytterligare värden och minimum för alla överförda värden returneras.|  
|max|Det finns två olika mönster för att anropa den här funktionen:`max([0,1,2])`<br /><br /> Här matris max en. Det här uttrycket returnerar `2`. Den här funktionen kan också ta en kommaavgränsad lista med värden: `max(0,1,2)` den här funktionen returnerar 2. Observera att alla värden måste vara tal, så om parametern är en matris det måste innehålla endast siffror i den.<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen eller -värde<br /><br /> **Beskrivning**: krävs. Det kan antingen vara en matris med värden för att hitta det största värdet eller det första värdet i en mängd.<br /><br /> **Parameternummer**: 2...*n*<br /><br /> **Namnet**: värde*n*<br /><br /> **Beskrivning**: valfria. Om den första parametern är ett värde, du kan sedan skicka ytterligare värden och maxvärdet för alla överförda värden returneras.|  
|intervallet| Genererar en heltalsmatris som startar från ett visst antal och du definierar returnerade Matrislängden. Den här funktionen returnerar till exempel `[3,4,5,6]`:<br /><br /> `range(3,4)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: startIndex<br /><br /> **Beskrivning**: krävs. Det är det första heltalet i matrisen.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: antal<br /><br /> **Beskrivning**: krävs. Antal heltal som är i matrisen.|  
|SLUMP| Genererar ett slumpmässigt heltal inom det angivna intervallet (sammanlagt i båda ändar. Detta kan till exempel returnera `42`:<br /><br /> `rand(-1000,1000)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: minsta<br /><br /> **Beskrivning**: krävs. Ett heltal som kan returneras.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: högsta<br /><br /> **Beskrivning**: krävs. Det största heltal som kan returneras.|  
  
## <a name="date-functions"></a>Datumfunktioner  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|utcnow|Returnerar aktuell tidsstämpel som en sträng. Till exempel `2015-03-15T13:27:36Z`:<br /><br /> `utcnow()`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger att formatera värdet för den här tidsstämpel. Om formatet inte är anges, används ISO 8601-format (”o”).|  
|lägg_till_sekunder|Lägger till ett heltal sekunder en tidsstämpel för sträng skickades. Antalet sekunder som kan vara positivt eller negativt. Resultatet är en sträng i ISO 8601-format (”o”) som standard om inte en Formatspecificeraren har angetts. Till exempel `2015-03-15T13:27:00Z`:<br /><br /> `addseconds('2015-03-15T13:27:36Z', -36)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. En sträng som innehåller tidpunkten.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: sekunder<br /><br /> **Beskrivning**: krävs. Antal sekunder att lägga till. Kan vara negativ subtrahera sekunder.<br /><br /> **Parameternummer**: 3<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger att formatera värdet för den här tidsstämpel. Om formatet inte är anges, används ISO 8601-format (”o”).|  
|addminutes|Lägger till ett heltal antal minuter en tidsstämpel för sträng skickades. Antalet minuter som kan vara positivt eller negativt. Resultatet är en sträng i ISO 8601-format (”o”) som standard om inte en Formatspecificeraren har angetts. Till exempel `2015-03-15T14:00:36Z`:<br /><br /> `addminutes('2015-03-15T13:27:36Z', 33)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. En sträng som innehåller tidpunkten.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: minuter<br /><br /> **Beskrivning**: krävs. Antal minuter att lägga till. Kan vara negativ subtrahera minuter.<br /><br /> **Parameternummer**: 3<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger att formatera värdet för den här tidsstämpel. Om formatet inte är anges, används ISO 8601-format (”o”).|  
|addhours|Lägger till ett heltal timmar en tidsstämpel för sträng skickades. Antalet timmar kan vara positivt eller negativt. Resultatet är en sträng i ISO 8601-format (”o”) som standard om inte en Formatspecificeraren har angetts. Till exempel `2015-03-16T01:27:36Z`:<br /><br /> `addhours('2015-03-15T13:27:36Z', 12)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. En sträng som innehåller tidpunkten.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: timmar<br /><br /> **Beskrivning**: krävs. Antal timmar att lägga till. Kan vara negativ subtrahera timmar.<br /><br /> **Parameternummer**: 3<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger att formatera värdet för den här tidsstämpel. Om formatet inte är anges, används ISO 8601-format (”o”).|  
|adddays|Lägger till ett heltal antal dagar till en sträng tidsstämpel skickades. Antalet dagar kan vara positivt eller negativt. Resultatet är en sträng i ISO 8601-format (”o”) som standard om inte en Formatspecificeraren har angetts. Till exempel `2015-02-23T13:27:36Z`:<br /><br /> `addseconds('2015-03-15T13:27:36Z', -20)`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. En sträng som innehåller tidpunkten.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: dagar<br /><br /> **Beskrivning**: krävs. Antal dagar att lägga till. Kan vara negativ subtrahera dagar.<br /><br /> **Parameternummer**: 3<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger att formatera värdet för den här tidsstämpel. Om formatet inte är anges, används ISO 8601-format (”o”).|  
|FormatDateTime|Returnerar en sträng i datumformat. Resultatet är en sträng i ISO 8601-format (”o”) som standard om inte en Formatspecificeraren har angetts. Till exempel `2015-02-23T13:27:36Z`:<br /><br /> `formatDateTime('2015-03-15T13:27:36Z', 'o')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: datum<br /><br /> **Beskrivning**: krävs. En sträng som innehåller datum.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger att formatera värdet för den här tidsstämpel. Om formatet inte är anges, används ISO 8601-format (”o”).|  

## <a name="next-steps"></a>Nästa steg
En lista över systemvariabler som du kan använda i uttryck, se [systemvariabler](control-flow-system-variables.md).