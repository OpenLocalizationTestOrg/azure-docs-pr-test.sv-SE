---
title: aaaWorkflow Definition Language schema - Azure Logic Apps | Microsoft Docs
description: "Definiera arbetsflöden baserat på hello arbetsflödet definition schemat för Azure Logic Apps"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 26c94308-aa0d-4730-97b6-de848bffff91
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: f12440e9ca269a9236132deeb888dbde7fb734d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="workflow-definition-language-schema-for-azure-logic-apps"></a>Arbetsflödet Definition Language schemat för Azure Logic Apps

En arbetsflödesdefinition innehåller hello faktiska logik som körs som en del av din logikapp. Den här definitionen innehåller en eller flera utlösare som startar hello logikapp och en eller flera åtgärder för hello logik app tootake.  
  
## <a name="basic-workflow-definition-structure"></a>Grundläggande arbetsflöde definition struktur

Här är hello grundläggande struktur för en arbetsflödesdefinition:  
  
```json
{
    "$schema": "<schema-of the-definition>",
    "contentVersion": "<version-number-of-definition>",
    "parameters": { <parameter-definitions-of-definition> },
    "triggers": [ { <definition-of-flow-triggers> } ],
    "actions": [ { <definition-of-flow-actions> } ],
    "outputs": { <output-of-definition> }
}
```
  
> [!NOTE]
> Hej [arbetsflöde Management REST API](https://docs.microsoft.com/rest/api/logic/workflows) dokumentationen innehåller information om hur toocreate och hantera logik app arbetsflöden.
  
|Elementnamn|Krävs|Beskrivning|  
|------------------|--------------|-----------------|  
|$schema|Nej|Anger hello plats för hello JSON-schema som beskriver hello version av hello definition language. Den här platsen måste anges när du refererar till en definition av externt. För det här dokumentet är hello plats: <p>`https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#`|  
|contentVersion|Nej|Anger hello definitionsversion. Du kan använda det här värdet toomake till att rätt definition av hello används när du distribuerar ett arbetsflöde med hello definition.|  
|parameters|Nej|Anger parametrarna tooinput data till hello definition. Du kan definiera högst 50 parametrar.|  
|Utlösare|Nej|Anger information för hello utlösare som initierar hello arbetsflöde. Högst 10 utlösare kan definieras.|  
|åtgärder|Nej|Anger åtgärder som vidtas som hello flödet kör. Du kan definiera högst 250 åtgärder.|  
|utdata|Nej|Anger information om hello distribueras resurs. Du kan definiera högst 10 utdata.|  
  
## <a name="parameters"></a>Parametrar

Det här avsnittet anger alla hello-parametrar som används i hello arbetsflödesdefinitionen vid tidpunkten för distribution. Alla parametrar måste deklareras i det här avsnittet innan de kan användas i andra avsnitt i hello definition.  
  
hello visar följande exempel hello struktur för en parameterdefinition:  

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": <default-value-of-parameter>,
        "allowedValues": [ <array-of-allowed-values> ],
        "metadata" : { "key": { "name": "value"} }
    }
}
```

|Elementnamn|Krävs|Beskrivning|  
|------------------|--------------|-----------------|  
|typ|Ja|**Typen**: sträng <p> **Deklarationen**:`"parameters": {"parameter1": {"type": "string"}` <p> **Specifikationen**:`"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Typen**: securestring <p> **Deklarationen**:`"parameters": {"parameter1": {"type": "securestring"}}` <p> **Specifikationen**:`"parameters": {"parameter1": {"value": "myparamvalue1"}}` <p> **Typen**: int <p> **Deklarationen**:`"parameters": {"parameter1": {"type": "int"}}` <p> **Specifikationen**:`"parameters": {"parameter1": {"value" : 5}}` <p> **Typen**: bool <p> **Deklarationen**:`"parameters": {"parameter1": {"type": "bool"}}` <p> **Specifikationen**:`"parameters": {"parameter1": { "value": true }}` <p> **Typen**: matris <p> **Deklarationen**:`"parameters": {"parameter1": {"type": "array"}}` <p> **Specifikationen**:`"parameters": {"parameter1": { "value": [ array-of-values ]}}` <p> **Typen**: objektet <p> **Deklarationen**:`"parameters": {"parameter1": {"type": "object"}}` <p> **Specifikationen**:`"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Typen**: secureobject <p> **Deklarationen**:`"parameters": {"parameter1": {"type": "object"}}` <p> **Specifikationen**:`"parameters": {"parameter1": { "value": { JSON-object } }}` <p> **Obs:** hello `securestring` och `secureobject` typer returneras inte i `GET` åtgärder. Alla lösenord, nycklar och hemligheter ska använda den här typen.|  
|Standardvärde|Nej|Anger hello standardvärdet för parametern hello när inget värde anges när hello hello resursen skapas.|  
|allowedValues|Nej|Anger en matris med tillåtna värden för hello-parametern.|  
|Metadata|Nej|Anger ytterligare information om hello-parametern som en läsbar beskrivning eller designläge data som används av Visual Studio eller andra verktyg.|  
  
Det här exemplet visar hur du kan använda en parameter under hello brödtexten i en åtgärd:  
  
```json
"body" :
{
  "property1": "@parameters('parameter1')"
}
```

 Parametrarna kan också användas i utdata.  
  
## <a name="triggers-and-actions"></a>Utlösare och åtgärder  

Ange hello-anrop som kan delta i arbetsflödeskörning utlösare och åtgärder. Mer information om det här avsnittet finns [arbetsflödesåtgärder och utlösare](logic-apps-workflow-actions-triggers.md).
  
## <a name="outputs"></a>utdata  

Utdata ange information som kan returneras från ett arbetsflöde som körs. Till exempel om du har en viss status eller ett värde som du vill tootrack för varje kör kan du inkludera att data i hello kör utdata. hello data visas i hello Management REST API för att köra och i hello hanteringsgränssnittet för att köras i hello Azure-portalen. Du kan också flöda dessa utdata tooother externa system som PowerBI för att skapa instrumentpaneler. Utdata är *inte* används toorespond tooincoming begäranden på hello Service REST API. toorespond tooan inkommande begäran med hjälp av hello `response` åtgärdstyp, här är ett exempel:
  
```json
"outputs": {  
  "key1": {  
    "value": "value1",  
    "type" : "<type-of-value>"  
  }  
} 
```

|Elementnamn|Krävs|Beskrivning|  
|------------------|--------------|-----------------|  
|key1|Ja|Anger hello nyckelidentifierare för hello utdata. Ersätt **key1** med ett namn som du vill toouse tooidentify hello utdata.|  
|värde|Ja|Anger hello värdet i hello utdata.|  
|typ|Ja|Anger hello hello-värde som har angetts. Möjliga är värden: <ul><li>`string`</li><li>`securestring`</li><li>`int`</li><li>`bool`</li><li>`array`</li><li>`object`</li></ul>|
  
## <a name="expressions"></a>Uttryck  

JSON-värdena i hello definition kan vara literal eller de kan vara uttryck som utvärderas när hello definition används. Exempel:  
  
```json
"name": "value"
```

 eller  
  
```json
"name": "@parameters('password') "
```

> [!NOTE]
> Vissa uttryck hämta sina värden från runtime-åtgärder som inte kanske finns hello början av hello körning. Du kan använda **funktioner** toohelp hämta några av dessa värden.  
  
Uttryck kan finnas var som helst i ett strängvärde i JSON och alltid resultera i en annan JSON-värde. När ett JSON-värde har fastställda toobe ett uttryck, hello brödtext hello uttryck hämtas genom att ta bort hello @-tecknet (@). Om det krävs en teckensträng som börjar med @, att strängen måste hoppas genom att använda@. hello följande exempel visar hur uttryck utvärderas.  
  
|JSON-värde|Resultat|  
|----------------|------------|  
|”parametrar”|hello tecken-parametrar, returneras.|  
|”Parametrar [1]”|hello tecken-parametrar [1] ”returneras.|  
|"@@"|En 1 tecken-sträng som innehåller ”@” returneras.|  
|" @"|En 2 teckensträng som innehåller ”@” returneras.|  
  
Med *string interpolerade*, uttryck kan även visas i strängar där uttryck omslutas i `@{ ... }`. Exempel: <p>`"name" : "First Name: @{parameters('firstName')} Last Name: @{parameters('lastName')}"`

hello resultatet är alltid en sträng, vilket gör den här funktionen liknande toohello `concat` funktion. Anta att du har definierat `myNumber` som `42` och `myString` som `sampleString`:  
  
|JSON-värde|Resultat|  
|----------------|------------|  
|”@parameters(minSträng)”|Returnerar `sampleString` som en sträng.|  
|”@{parameters('myString')}”|Returnerar `sampleString` som en sträng.|  
|”@parameters(myNumber)”|Returnerar `42` som en *nummer*.|  
|”@{parameters('myNumber')}”|Returnerar `42` som en *sträng*.|  
|”Svaret är: @{parameters('myNumber')}”|Returnerar hello sträng `Answer is: 42`.|  
|”@concat(' Svaret är: ', string(parameters('myNumber')))”|Returnerar hello sträng`Answer is: 42`|  
|”Svaret är: @@ {parameters('myNumber')}”|Returnerar hello sträng `Answer is: @{parameters('myNumber')}`.|  
  
## <a name="operators"></a>Operatorer  

Operatorer är hello tecken som du kan använda i uttryck eller funktioner. 
  
|Operatorn|Beskrivning|  
|--------------|-----------------|  
|.|hello punktoperatorn kan du tooreference egenskaper för ett objekt|  
|?|hello frågetecken operatör kan du referera till null egenskaper för ett objekt utan ett körningsfel. Du kan till exempel använda det här uttrycket toohandle null utlösa utdata: <p>`@coalesce(trigger().outputs?.body?.property1, 'my default value')`|  
|'|hello enkelt citattecken är hello endast sätt toowrap stränglitteraler. Du kan inte använda dubbla citattecken i uttryck eftersom detta skiljetecken står i konflikt med hello JSON offert som omsluter hello hela uttrycket.|  
|[]|hello hakparenteser kan vara tooget används ett värde från en matris med ett visst index. Om du har en åtgärd som skickar till exempel `range(0,10)`i toohello `forEach` funktion, som du kan använda den här funktionen tooget artiklarna matriser:  <p>`myArray[item()]`|  
  
## <a name="functions"></a>Funktioner  

Du kan också kontakta funktioner i uttryck. hello visar följande tabell hello-funktioner som kan användas i ett uttryck.  
  
|uttryck|Utvärdering|  
|----------------|----------------|  
|”@function(” Hello ”)”|Anrop hello funktionen tillhör hello definition med hello teckensträng Hello som den första parametern för hello.|  
|”@function(” Är det kall!') ”|Anropar hello funktionen medlem hello definition med hello teckensträng, den är lågfrekvent!' som den första parametern för hello|  
|”@function.prop1 ()”|Returnerar hello värdet på egenskapen för hello egenskap1 för hello `myfunction` tillhör hello definition.|  
|”@function(” Hello ”) .prop1”|Anrop hello funktionen tillhör hello definition med hello exakta strängen ”Hello” som hello första parametern och returnerar hello egenskap1 egenskap i hello-objektet.|  
|”@function(parameters('Hello'))”|Utvärderar hello Hello parametern och skickar hello värdet toofunction|  
  
### <a name="referencing-functions"></a>Refererar till funktioner  

Du kan använda dessa funktioner tooreference utdata från andra åtgärder i hello logikapp eller värden som skickades när hello logikapp skapades. Du kan till exempel referera hello data från ett steg toouse den i en annan.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|parameters|Returnerar ett parametervärde som definieras i hello definition. <p>`parameters('password')` <p> **Parameternummer**: 1 <p> **Namnet**: parametern <p> **Beskrivning**: krävs. hello namnet på hello-parameter vars värden som du vill.|  
|Åtgärden|Gör ett uttryck tooderive sitt värde från andra JSON-namn och värdepar eller hello utdata från hello aktuella runtime-åtgärden. hello-egenskapen som representeras av propertyPath i följande exempel hello är valfri. Om propertyPath inte anges är hello referensen toohello hela action-objekt. Den här funktionen kan bara användas i-tills villkoren i en åtgärd. <p>`action().outputs.body.propertyPath`|  
|åtgärder|Gör ett uttryck tooderive sitt värde från andra JSON-namn och värdepar eller hello utdata från hello runtime-åtgärd. Dessa uttryck deklarera uttryckligen att en åtgärd är beroende av en annan åtgärd. hello-egenskapen som representeras av propertyPath i följande exempel hello är valfri. Om propertyPath inte anges är hello referensen toohello hela action-objekt. Du kan använda antingen det här elementet eller hello villkor elementet toospecify beroenden, men du behöver inte toouse både för hello samma beroende resurs. <p>`actions('myAction').outputs.body.propertyPath` <p> **Parameternummer**: 1 <p> **Namnet**: åtgärdsnamn <p> **Beskrivning**: krävs. hello namnet på hello åtgärd vars värden som du vill. <p> Tillgängliga egenskaper på hello åtgärdsobjektet är: <ul><li>`name`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>Se hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=850646) mer information om dessa egenskaper.|
|Utlösare|Gör ett uttryck tooderive sitt värde från andra JSON-namn och värdepar eller hello utdata hello runtime utlösare. hello-egenskapen som representeras av propertyPath i följande exempel hello är valfri. Om propertyPath inte anges är hello referensen toohello hela utlösande objektet. <p>`trigger().outputs.body.propertyPath` <p>När den används i en utlösare indata returneras hello hello utdata för hello tidigare körning. Men när den används i en utlösarvillkor hello `trigger` funktionen returnerar hello utdata för hello aktuella körning. <p> Tillgängliga egenskaper för hello utlösarobjekt är: <ul><li>`name`</li><li>`scheduledTime`</li><li>`startTime`</li><li>`endTime`</li><li>`inputs`</li><li>`outputs`</li><li>`status`</li><li>`code`</li><li>`trackingId`</li><li>`clientTrackingId`</li></ul> <p>Se hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=850644) mer information om dessa egenskaper.|
|actionOutputs|Den här funktionen är snabbformen för`actions('actionName').outputs` <p> **Parameternummer**: 1 <p> **Namnet**: åtgärdsnamn <p> **Beskrivning**: krävs. hello namnet på hello åtgärd vars värden som du vill.|  
|actionBody och brödtext|Dessa funktioner är snabbformen för`actions('actionName').outputs.body` <p> **Parameternummer**: 1 <p> **Namnet**: åtgärdsnamn <p> **Beskrivning**: krävs. hello namnet på hello åtgärd vars värden som du vill.|  
|triggerOutputs|Den här funktionen är snabbformen för`trigger().outputs`|  
|triggerBody|Den här funktionen är snabbformen för`trigger().outputs.body`|  
|Objektet|När den används i en upprepande åtgärd returnerar funktionen hello-objekt som finns i hello matris för den här iteration av hello-åtgärd. Om du har en åtgärd som körs för varje objekt en matris av meddelanden som kan du använda följande syntax: <p>`"input1" : "@item().subject"`| 
  
### <a name="collection-functions"></a>Samlingen funktioner  

Dessa funktioner fungerar över samlingar och tillämpa vanligtvis tooArrays strängar och ibland ordlistor.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|Innehåller|Returnerar true om uppslagslistan innehåller en nyckel, lista innehåller värdet eller strängen innehåller delsträngen. Den här funktionen returnerar till exempel `true`: <p>`contains('abacaba','aca')` <p> **Parameternummer**: 1 <p> **Namnet**: inom samlingen <p> **Beskrivning**: krävs. hello samling toosearch inom. <p> **Parameternummer**: 2 <p> **Namnet**: Sök objekt <p> **Beskrivning**: krävs. Hej objektet toofind inuti hello **inom samlingen**.|  
|Längd|Returnerar hello antalet element i en matris eller sträng. Den här funktionen returnerar till exempel `3`:  <p>`length('abc')` <p> **Parameternummer**: 1 <p> **Namnet**: samlingen <p> **Beskrivning**: krävs. hello samling för vilken tooget hello längd.|  
|tom|Returnerar true om objektet, matris eller sträng är tom. Den här funktionen returnerar till exempel `true`: <p>`empty('')` <p> **Parameternummer**: 1 <p> **Namnet**: samlingen <p> **Beskrivning**: krävs. Hej samling toocheck om den är tom.|  
|skärningspunkten|Returnerar en enda matris eller ett objekt som har gemensamma element mellan matriser eller objekt som överförts. Den här funktionen returnerar till exempel `[1, 2]`: <p>`intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])` <p>hello parametrar för funktionen hello kan antingen vara en uppsättning objekt eller en uppsättning matriser (inte en blandning av båda). Om det finns två objekt med hello namn samma, senaste hello-objektet med det namnet visas i hello sista objektet. <p> **Parameternummer**: 1...*n* <p> **Namnet**: samlingen*n* <p> **Beskrivning**: krävs. hello samlingar tooevaluate. Ett objekt måste vara i alla samlingar som skickades i tooappear i hello resultat.|  
|Union|Returnerar en enda matris eller ett objekt med alla hello-element i matrisen eller objekt överförs toothis funktionen. Den här funktionen returnerar till exempel `[1, 2, 3, 10, 101]`: <p>`union([1, 2, 3], [101, 2, 1, 10])` <p>hello parametrar för funktionen hello kan antingen vara en uppsättning objekt eller en uppsättning matriser (inte en blandning av dessa). Om det finns två objekt med samma namn hello i hello slutversionen, visas sista hello-objekt med det namnet i hello sista objektet. <p> **Parameternummer**: 1...*n* <p> **Namnet**: samlingen*n* <p> **Beskrivning**: krävs. hello samlingar tooevaluate. Ett objekt som visas i någon av hello samlingar visas också i hello resultat.|  
|första|Returnerar hello första elementet i hello matris eller sträng skickades. Den här funktionen returnerar till exempel `0`: <p>`first([0,2,3])` <p> **Parameternummer**: 1 <p> **Namnet**: samlingen <p> **Beskrivning**: krävs. Hej tootake hello första samlingsobjektet från.|  
|senaste|Returnerar hello sista elementet i hello matris eller sträng skickades. Den här funktionen returnerar till exempel `3`: <p>`last('0123')` <p> **Parameternummer**: 1 <p> **Namnet**: samlingen <p> **Beskrivning**: krävs. Hej tootake hello senaste samlingsobjektet från.|  
|ta|Returnerar hello först **antal** element från hello matris eller sträng skickades. Den här funktionen returnerar till exempel `[1, 2]`:  <p>`take([1, 2, 3, 4], 2)` <p> **Parameternummer**: 1 <p> **Namnet**: samlingen <p> **Beskrivning**: krävs. hello samling från där tootake hello först **antal** objekt. <p> **Parameternummer**: 2 <p> **Namnet**: antal <p> **Beskrivning**: krävs. Hej antalet objekt tootake från hello **samling**. Måste vara ett positivt heltal.|  
|Hoppa över|Returnerar hello element i matrisen för hello startar vid index **antal**. Den här funktionen returnerar till exempel `[3, 4]`: <p>`skip([1, 2 ,3 ,4], 2)` <p> **Parameternummer**: 1 <p> **Namnet**: samlingen <p> **Beskrivning**: krävs. Hej samling tooskip hello först **antal** objekt från. <p> **Parameternummer**: 2 <p> **Namnet**: antal <p> **Beskrivning**: krävs. Hej antalet objekt tooremove från hello framför **samling**. Måste vara ett positivt heltal.|  
|join|Returnerar en sträng med varje objekt i en matris som kopplas med avgränsare, till exempel returneras `"1,2,3,4"`:<br /><br /> `join([1, 2, 3, 4], ',')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: samlingen<br /><br /> **Beskrivning**: krävs. Hej toojoin samlingsobjekt från.<br /><br /> **Parameternummer**: 2<br /><br /> **Namnet**: avgränsare<br /><br /> **Beskrivning**: krävs. hello sträng toodelimit objekt.|  
  
### <a name="string-functions"></a>Strängfunktioner

hello följande funktioner endast gäller toostrings. Du kan också använda vissa funktioner i samlingen i strängar.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|concat|Kombinerar valfritt antal strängar tillsammans. Om parametern 1 är till exempel `p1`, den här funktionen returnerar `somevalue-p1-somevalue`: <p>`concat('somevalue-',parameters('parameter1'),'-somevalue')` <p> **Parameternummer**: 1...*n* <p> **Namnet**: sträng*n* <p> **Beskrivning**: krävs. hello strängar toocombine till en sträng.|  
|delsträngen|Returnerar en delmängd av tecken från en sträng. Den här funktionen returnerar till exempel `abc`: <p>`substring('somevalue-abc-somevalue',10,3)` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello sträng från vilken hello delsträngen hämtas. <p> **Parameternummer**: 2 <p> **Namnet**: startIndex <p> **Beskrivning**: krävs. hello index där hello delsträngen börjar i parameter 1. <p> **Parameternummer**: 3 <p> **Namnet**: längd <p> **Beskrivning**: krävs. hello längd hello delsträngen.|  
|Ersätt|Ersätter en sträng med en given sträng. Den här funktionen returnerar till exempel `hello new string`: <p>`replace('hello old string', 'old', 'new')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello-sträng som är sökte efter parametern 2 och uppdateras med parametern 3, när parametern 2 hittas i parameter 1. <p> **Parameternummer**: 2 <p> **Namnet**: gamla sträng <p> **Beskrivning**: krävs. hello sträng tooreplace med parametern 3, när en matchning hittas i parameter 1 <p> **Parameternummer**: 3 <p> **Namnet**: ny sträng <p> **Beskrivning**: krävs. hello sträng som är används tooreplace hello sträng i parametern 2 när en matchning hittas i parameter 1.|  
|GUID|Den här funktionen genererar ett globalt unik sträng (GUID). Den här funktionen kan till exempel generera detta GUID:`c2ecc88d-88c8-4096-912c-d6f2e2b138ce` <p>`guid()` <p> **Parameternummer**: 1 <p> **Namnet**: Format <p> **Beskrivning**: valfria. En enda Formatspecificeraren som anger [hur tooformat hello värdet för detta Guid](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx). Hej formatparametern kan vara ”N”, ”D”, ”B”, ”P” eller ”X”. Om formatet inte är anges, används ”D”.|  
|toLower|Konverterar en sträng toolowercase. Den här funktionen returnerar till exempel `two by two is four`: <p>`toLower('Two by Two is Four')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello sträng tooconvert toolower skiftläge. Om ett tecken i strängen hello inte har en motsvarighet på gemener ingår hello tecken oförändrad i hello returnerade sträng.|  
|toUpper|Konverterar en sträng toouppercase. Den här funktionen returnerar till exempel `TWO BY TWO IS FOUR`: <p>`toUpper('Two by Two is Four')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello sträng tooconvert tooupper skiftläge. Om ett tecken i strängen hello saknar motsvarande versaler ingår hello tecken oförändrad i hello returnerade sträng.|  
|indexOf|Hitta hello index för ett värde inom strängen fall insensitively. Den här funktionen returnerar till exempel `7`: <p>`indexof('hello, world.', 'world')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello-sträng som kan innehålla hello värde. <p> **Parameternummer**: 2 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello värdet toosearch hello index.|  
|lastIndexOf|Hitta hello senaste index för ett värde inom strängen fall insensitively. Den här funktionen returnerar till exempel `3`: <p>`lastindexof('foofoo', 'foo')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello-sträng som kan innehålla hello värde. <p> **Parameternummer**: 2 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello värdet toosearch hello index.|  
|StartsWith|Kontrollerar om hello strängen börjar med ett värde ärende insensitively. Den här funktionen returnerar till exempel `true`: <p>`startswith('hello, world', 'hello')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello-sträng som kan innehålla hello värde. <p> **Parameternummer**: 2 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello värdet hello sträng kan börja med.|  
|EndsWith|Kontrollerar om hello sträng slutar med ett värde ärende insensitively. Den här funktionen returnerar till exempel `true`: <p>`endswith('hello, world', 'world')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello-sträng som kan innehålla hello värde. <p> **Parameternummer**: 2 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello värdet hello sträng kan avslutas med.|  
|split|Delar hello sträng med avgränsare. Den här funktionen returnerar till exempel `["a", "b", "c"]`: <p>`split('a;b;c',';')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello-sträng som delas. <p> **Parameternummer**: 2 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello avgränsare.|  
  
### <a name="logical-functions"></a>Logiska funktioner  

Dessa funktioner är användbara i villkor och kan vara används tooevaluate någon typ av logik.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|är lika med|Returnerar true om två värden är lika. Till exempel om parameter1 är someValue, returnerar funktionen `true`: <p>`equals(parameters('parameter1'), 'someValue')` <p> **Parameternummer**: 1 <p> **Namnet**: objekt-1 <p> **Beskrivning**: krävs. Hej objektet toocompare för**objekt 2**. <p> **Parameternummer**: 2 <p> **Namnet**: objekt 2 <p> **Beskrivning**: krävs. Hej objektet toocompare för**objekt 1**.|  
|mindre|Returnerar true om hello första argumentet är mindre än hello andra. Observera att värden kan bara vara av typen heltal, flyttal eller string. Den här funktionen returnerar till exempel `true`: <p>`less(10,100)` <p> **Parameternummer**: 1 <p> **Namnet**: objekt-1 <p> **Beskrivning**: krävs. Hej objektet toocheck om det är mindre än **objekt 2**. <p> **Parameternummer**: 2 <p> **Namnet**: objekt 2 <p> **Beskrivning**: krävs. Hej objektet toocheck om det är större än **objekt 1**.|  
|lessOrEquals|Returnerar true om hello första argumentet är mindre än eller lika med toohello andra. Observera att värden kan bara vara av typen heltal, flyttal eller string. Den här funktionen returnerar till exempel `true`: <p>`lessOrEquals(10,10)` <p> **Parameternummer**: 1 <p> **Namnet**: objekt-1 <p> **Beskrivning**: krävs. Hej objektet toocheck om det är mindre än eller lika med för**objekt 2**. <p> **Parameternummer**: 2 <p> **Namnet**: objekt 2 <p> **Beskrivning**: krävs. Hej objektet toocheck om det är större än eller lika med för**objekt 1**.|  
|större|Returnerar true om hello första argumentet är större än hello andra. Observera att värden kan bara vara av typen heltal, flyttal eller string. Den här funktionen returnerar till exempel `false`:  <p>`greater(10,10)` <p> **Parameternummer**: 1 <p> **Namnet**: objekt-1 <p> **Beskrivning**: krävs. Hej objektet toocheck om det är större än **objekt 2**. <p> **Parameternummer**: 2 <p> **Namnet**: objekt 2 <p> **Beskrivning**: krävs. Hej objektet toocheck om det är mindre än **objekt 1**.|  
|greaterOrEquals|Returnerar true om hello första argumentet är större än eller lika toohello andra. Observera att värden kan bara vara av typen heltal, flyttal eller string. Den här funktionen returnerar till exempel `false`: <p>`greaterOrEquals(10,100)` <p> **Parameternummer**: 1 <p> **Namnet**: objekt-1 <p> **Beskrivning**: krävs. Hej objektet toocheck om det är större än eller lika med för**objekt 2**. <p> **Parameternummer**: 2 <p> **Namnet**: objekt 2 <p> **Beskrivning**: krävs. Hej objektet toocheck om det är mindre än eller lika för**objekt 1**.|  
|och|Returnerar true om båda parametrarna är true. Båda argument måste toobe booleska värden. Den här funktionen returnerar till exempel `false`: <p>`and(greater(1,10),equals(0,0))` <p> **Parameternummer**: 1 <p> **Namnet**: booleskt 1 <p> **Beskrivning**: krävs. Hej första argument måste vara `true`. <p> **Parameternummer**: 2 <p> **Namnet**: booleskt 2 <p> **Beskrivning**: krävs. andra hello-argumentet måste vara `true`.|  
|eller|Returnerar true om endera parametern är true. Båda argument måste toobe booleska värden. Den här funktionen returnerar till exempel `true`: <p>`or(greater(1,10),equals(0,0))` <p> **Parameternummer**: 1 <p> **Namnet**: booleskt 1 <p> **Beskrivning**: krävs. Hej första argument som kan vara `true`. <p> **Parameternummer**: 2 <p> **Namnet**: booleskt 2 <p> **Beskrivning**: krävs. hello andra argumentet får vara `true`.|  
|inte|Returnerar true om hello parametrar är `false`. Båda argument måste toobe booleska värden. Den här funktionen returnerar till exempel `true`: <p>`not(contains('200 Success','Fail'))` <p> **Parameternummer**: 1 <p> **Namnet**: booleskt <p> **Beskrivning**: returnerar true om hello parametrar är `false`. Båda argument måste toobe booleska värden. Den här funktionen returnerar `true`:`not(contains('200 Success','Fail'))`|  
|Om|Returnerar ett angivet värde baserat på om hello uttryck resulterade i `true` eller `false`.  Den här funktionen returnerar till exempel `"yes"`: <p>`if(equals(1, 1), 'yes', 'no')` <p> **Parameternummer**: 1 <p> **Namnet**: uttryck <p> **Beskrivning**: krävs. Ett booleskt värde som anger vilka hello värdeuttryck ska returnera. <p> **Parameternummer**: 2 <p> **Namnet**: True <p> **Beskrivning**: krävs. Hej värdet tooreturn om hello-uttrycket är `true`. <p> **Parameternummer**: 3 <p> **Namnet**: False <p> **Beskrivning**: krävs. Hej värdet tooreturn om hello-uttrycket är `false`.|  
  
### <a name="conversion-functions"></a>Konverteringsfunktioner  

Dessa funktioner är används tooconvert mellan varje hello inbyggda typer i hello språk:  
  
- Sträng  
  
- heltal  
  
- flyttal  
  
- Booleskt värde  
  
- matriser  
  
- ordlistor  

-   Formulär  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|int|Konvertera hello parametern tooan heltal. Den här funktionen returnerar till exempel 100 som ett tal i stället för en sträng: <p>`int('100')` <p> **Parameternummer**: 1 <p> **Namnet**: värde <p> **Beskrivning**: krävs. hello-värde som är konverterade tooan heltal.|  
|Sträng|Konvertera hello parametern tooa sträng. Den här funktionen returnerar till exempel `'10'`: <p>`string(10)` <p>Du kan också konvertera ett objekt tooa sträng. Till exempel, om hello `myPar` parametern är ett objekt med en egenskap `abc : xyz`, och sedan på den här funktionen returnerar `{"abc" : "xyz"}`: <p>`string(parameters('myPar'))` <p> **Parameternummer**: 1 <p> **Namnet**: värde <p> **Beskrivning**: krävs. hello värde som är konverterade tooa sträng.|  
|JSON|Konvertera hello tooa JSON typen parametervärdet och är hello motsatt av `string()`. Den här funktionen returnerar till exempel `[1,2,3]` som en matris i stället för en sträng: <p>`json('[1,2,3]')` <p>På samma sätt kan du konvertera ett tooan string-objekt. Den här funktionen returnerar till exempel `{ "abc" : "xyz" }`: <p>`json('{"abc" : "xyz"}')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello strängvärde som är konverterade tooa inbyggd typ. <p>Hej `json()` har stöd för XML som indata för funktionen. Till exempel hello parametervärde är: <p>`<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>` <p>är konverterade toothis JSON: <p>`{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|flyttal|Konvertera hello parametern argumentet tooa flyttal. Den här funktionen returnerar till exempel `10.333`: <p>`float('10.333')` <p> **Parameternummer**: 1 <p> **Namnet**: värde <p> **Beskrivning**: krävs. hello värde som är konverterade tooa flyttal.|  
|bool|Konvertera hello parametern tooa booleskt värde. Den här funktionen returnerar till exempel `false`: <p>`bool(0)` <p> **Parameternummer**: 1 <p> **Namnet**: värde <p> **Beskrivning**: krävs. hello-värde som är konverterade tooa boolean.|  
|Slå samman|Returnerar hello första icke-null-objekt i hello-argument som skickas. **Obs**: en tom sträng inte är null. Till exempel om parametrar 1 och 2 inte har definierats, returnerar funktionen `fallback`:  <p>`coalesce(parameters('parameter1'), parameters('parameter2') ,'fallback')` <p> **Parameternummer**: 1...*n* <p> **Namnet**: objektet*n* <p> **Beskrivning**: krävs. hello objekt toocheck för null.|  
|Base64|Returnerar hello base64-representation av hello Indatasträngen. Den här funktionen returnerar till exempel `c29tZSBzdHJpbmc=`: <p>`base64('some string')` <p> **Parameternummer**: 1 <p> **Namnet**: String 1 <p> **Beskrivning**: krävs. hello sträng tooencode i base64-representation.|  
|base64ToBinary|Returnerar en a binär representation av en base64-kodad sträng. Den här funktionen returnerar till exempel hello binär representation av `some string`: <p>`base64ToBinary('c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello base64-kodad sträng.|  
|base64ToString|Returnerar en strängrepresentation av en based64 kodad sträng. Den här funktionen returnerar till exempel `some string`: <p>`base64ToString('c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello base64-kodad sträng.|  
|Binär|Returnerar en a binär representation av ett värde.  Den här funktionen returnerar till exempel en a binär representation av `some string`: <p>`binary('some string')` <p> **Parameternummer**: 1 <p> **Namnet**: värde <p> **Beskrivning**: krävs. hello-värde som är konverterade toobinary.|  
|dataUriToBinary|Returnerar en a binär representation av en data-URI. Den här funktionen returnerar till exempel hello binär representation av `some string`: <p>`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello data URI tooconvert toobinary representation.|  
|dataUriToString|Returnerar en strängrepresentation av en data-URI. Den här funktionen returnerar till exempel `some string`: <p>`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng<p> **Beskrivning**: krävs. hello data URI tooconvert tooString representation.|  
|dataUri|Returnerar en data-URI för ett värde. Den här funktionen returnerar till exempel data URI `text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=`: <p>`dataUri('some string')` <p> **Parameternummer**: 1<p> **Namnet**: värde<p> **Beskrivning**: krävs. hello värdet tooconvert toodata URI.|  
|decodeBase64|Returnerar en strängrepresentation av en inkommande based64-sträng. Den här funktionen returnerar till exempel `some string`: <p>`decodeBase64('c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: returnerar en strängrepresentation av en inkommande based64-sträng.|  
|encodeUriComponent|URL-visar hello sträng som skickas. Den här funktionen returnerar till exempel `You+Are%3ACool%2FAwesome`: <p>`encodeUriComponent('You Are:Cool/Awesome')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello sträng tooescape URL-osäkra tecken från.|  
|decodeUriComponent|UN-URL-visar hello sträng som skickas. Den här funktionen returnerar till exempel `You Are:Cool/Awesome`: <p>`encodeUriComponent('You+Are%3ACool%2FAwesome')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello sträng toodecode hello URL-osäkra tecken från.|  
|decodeDataUri|Returnerar en a binär representation av ett indata-URI-strängen. Den här funktionen returnerar till exempel hello binär representation av `some string`: <p>`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng <p> **Beskrivning**: krävs. Hej dataURI toodecode i en a binär representation.|  
|uriComponent|Returnerar en URI kodad representation av ett värde. Den här funktionen returnerar till exempel `You+Are%3ACool%2FAwesome`: <p>`uriComponent('You Are:Cool/Awesome')` <p> **Parameternummer**: 1<p> **Namnet**: sträng <p> **Beskrivning**: krävs. hello sträng toobe URI-kodad.|  
|uriComponentToBinary|Returnerar en a binär representation av en URI-kodad sträng. Den här funktionen returnerar till exempel en a binär representation av `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Parameternummer**: 1 <p> **Namnet**: sträng<p> **Beskrivning**: krävs. hello URI-kodad sträng.|  
|uriComponentToString|Returnerar en strängrepresentation av en URI-kodad sträng. Den här funktionen returnerar till exempel `You Are:Cool/Awesome`: <p>`uriComponentToBinary('You+Are%3ACool%2FAwesome')` <p> **Parameternummer**: 1<p> **Namnet**: sträng<p> **Beskrivning**: krävs. hello URI-kodad sträng.|  
|xml|Returnera en XML-representation av hello värde. Den här funktionen returnerar exempelvis XML innehåll representeras av `'\<name>Alan\</name>'`: <p>`xml('\<name>Alan\</name>')` <p>Hej `xml()` har stöd för JSON-objekt som indata för funktionen. Till exempel hello parametern `{ "abc": "xyz" }` är konverterade tooXML innehåll:`\<abc>xyz\</abc>` <p> **Parameternummer**: 1<p> **Namnet**: värde<p> **Beskrivning**: krävs. hello värdet tooconvert tooXML.|  
|XPath|Returnera en matris med matchande hello xpath-uttryck för ett värde som hello xpath-uttryck som utvärderas till XML-noder. <p> **Exempel 1** <p>Anta hello värdet för parametern `p1` är en strängrepresentation av den här XML: <p>`<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>` <p>Den här koden:`xpath(xml(parameters('p1'), '/lab/robot/name')` <p>Returnerar <p>`[ <name>R1</name>, <name>R2</name> ]` <p>När den här koden: <p>`xpath(xml(parameters('p1'), ' sum(/lab/robot/parts)')` <p>Returnerar <p>`13` <p> <p> **Exempel 2** <p>Angivna hello följande XML-innehåll: <p>`<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>` <p>Den här koden:`@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')` <p>eller den här koden: <p>`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')` <p>Returnerar <p>`<Location xmlns="http://abc.com">xyz</Location>` <p>Och koden:`@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')` <p>Returnerar <p>``xyz`` <p> **Parameternummer**: 1 <p> **Namnet**: Xml <p> **Beskrivning**: krävs. hello XML på vilka tooevaluate hello XPath uttryck. <p> **Parameternummer**: 2 <p> **Namnet**: XPath <p> **Beskrivning**: krävs. hello XPath-uttrycket tooevaluate.|  
|matris|Konvertera hello tooan parametermatris. Den här funktionen returnerar till exempel `["abc"]`: <p>`array('abc')` <p> **Parameternummer**: 1 <p> **Namnet**: värde <p> **Beskrivning**: krävs. hello-värde som är konverterade tooan matris.|
|createArray|Skapar en matris av hello parametrar. Den här funktionen returnerar till exempel `["a", "c"]`: <p>`createArray('a', 'c')` <p> **Parameternummer**: 1...*n* <p> **Namnet**: alla*n* <p> **Beskrivning**: krävs. hello värden toocombine i en matris.|
|triggerFormDataValue|Returnerar ett värde matchar hello nyckelnamn från formulärdata eller utdata för formuläret-kodade utlösare.  Om det finns flera matchar det kommer fel.  Exempelvis returnerar följande hello `bar`:`triggerFormDataValue('foo')`<br /><br />**Parameternummer**: 1<br /><br />**Namnet**: nyckelnamn<br /><br />**Beskrivning**: krävs. hello nyckelnamn av hello formuläret data värdet tooreturn.|
|triggerFormDataMultiValues|Returnerar en matris med värden som matchar hello nyckelnamn från formulärdata eller utdata för formuläret-kodade utlösare.  Exempelvis returnerar följande hello `["bar"]`:`triggerFormDataValue('foo')`<br /><br />**Parameternummer**: 1<br /><br />**Namnet**: nyckelnamn<br /><br />**Beskrivning**: krävs. hello nyckelnamn hello formulärdata värden tooreturn.|
|triggerMultipartBody|Returnerar hello brödtext för en del i en multipart utdata hello utlösare.<br /><br />**Parameternummer**: 1<br /><br />**Namnet**: Index<br /><br />**Beskrivning**: krävs. hello index för hello del tooretrieve.|
|formDataValue|Returnerar ett värde matchar hello nyckelnamn från formulärdata eller formulär-kodade utdata.  Om det finns flera matchar det kommer fel.  Exempelvis returnerar följande hello `bar`:`formDataValue('someAction', 'foo')`<br /><br />**Parameternummer**: 1<br /><br />**Namnet**: åtgärdsnamn<br /><br />**Beskrivning**: krävs. hello namnet på hello-åtgärd med ett formulär-data eller en form-kodad.<br /><br />**Parameternummer**: 2<br /><br />**Namnet**: nyckelnamn<br /><br />**Beskrivning**: krävs. hello nyckelnamn av hello formuläret data värdet tooreturn.|
|formDataMultiValues|Returnerar en matris med värden som matchar hello nyckelnamn från formulärdata eller formulär-kodade utdata.  Exempelvis returnerar följande hello `["bar"]`:`formDataMultiValues('someAction', 'foo')`<br /><br />**Parameternummer**: 1<br /><br />**Namnet**: åtgärdsnamn<br /><br />**Beskrivning**: krävs. hello namnet på hello-åtgärd med ett formulär-data eller en form-kodad.<br /><br />**Parameternummer**: 2<br /><br />**Namnet**: nyckelnamn<br /><br />**Beskrivning**: krävs. hello nyckelnamn hello formulärdata värden tooreturn.|
|multipartBody|Returnerar hello brödtext för en del i en multipart resultatet av en åtgärd.<br /><br />**Parameternummer**: 1<br /><br />**Namnet**: åtgärdsnamn<br /><br />**Beskrivning**: krävs. hello namnet på hello-åtgärd med ett multipart svar.<br /><br />**Parameternummer**: 2<br /><br />**Namnet**: Index<br /><br />**Beskrivning**: krävs. hello index för hello del tooretrieve.|

### <a name="math-functions"></a>Matematiska funktioner  

Dessa funktioner kan användas för båda typer av siffror: **heltal** och **flyter**.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|Lägg till|Returnerar hello resultat från att lägga till hello två tal. Den här funktionen returnerar till exempel `20.333`: <p>`add(10,10.333)` <p> **Parameternummer**: 1 <p> **Namnet**: genom att hålla 1 <p> **Beskrivning**: krävs. Hej antalet tooadd för**genom att hålla 2**. <p> **Parameternummer**: 2 <p> **Namnet**: genom att hålla 2 <p> **Beskrivning**: krävs. Hej antalet tooadd för**genom att hålla 1**.|  
|Sub|Returnerar hello resultat från att subtrahera två tal. Den här funktionen returnerar till exempel `-0.333`: <p>`sub(10,10.333)` <p> **Parameternummer**: 1 <p> **Namn på**: Minuend <p> **Beskrivning**: krävs. hello number som **Subtrahend** tas bort från. <p> **Parameternummer**: 2 <p> **Namn på**: Subtrahend <p> **Beskrivning**: krävs. Hej nummer tooremove från hello **Minuend**.|  
|mul|Returnerar hello resultat från att multiplicera hello två tal. Den här funktionen returnerar till exempel `103.33`: <p>`mul(10,10.333)` <p> **Parameternummer**: 1 <p> **Namn på**: Multiplicand 1 <p> **Beskrivning**: krävs. Hej nummer toomultiply **Multiplicand 2** med. <p> **Parameternummer**: 2 <p> **Namn på**: Multiplicand 2 <p> **Beskrivning**: krävs. Hej nummer toomultiply **Multiplicand 1** med.|  
|div|Returnerar hello resultat från att dela hello två tal. Den här funktionen returnerar till exempel `1.0333`: <p>`div(10.333,10)` <p> **Parameternummer**: 1 <p> **Namnet**: täljaren <p> **Beskrivning**: krävs. Hej nummer toodivide av hello **Divisor**. <p> **Parameternummer**: 2 <p> **Namnet**: Divisor <p> **Beskrivning**: krävs. hello nummer toodivide hello **täljaren** av.|  
|MOD|Returnerar hello resten efter att dela hello två tal (modulo). Den här funktionen returnerar till exempel `2`: <p>`mod(10,4)` <p> **Parameternummer**: 1 <p> **Namnet**: täljaren <p> **Beskrivning**: krävs. Hej nummer toodivide av hello **Divisor**. <p> **Parameternummer**: 2 <p> **Namnet**: Divisor <p> **Beskrivning**: krävs. hello nummer toodivide hello **täljaren** av. Efter hello delning tas hello resten.|  
|min.|Det finns två olika mönster för att anropa den här funktionen. <p>Här `min` en matris och hello funktionen returnerar `0`: <p>`min([0,1,2])` <p>Den här funktionen kan också ta en kommaavgränsad lista med värden och returnerar `0`: <p>`min(0,1,2)` <p> **Obs**: alla värden måste vara siffror, så om hello-parametern är en matris, hello matris har tooonly har siffror. <p> **Parameternummer**: 1 <p> **Namnet**: samlingen eller -värde <p> **Beskrivning**: krävs. Antingen en matris med värden toofind hello minimivärdet eller hello första värdet i en mängd. <p> **Parameternummer**: 2...*n* <p> **Namnet**: värde*n* <p> **Beskrivning**: valfria. Om hello första parametern är ett värde, du kan sedan skicka ytterligare värden och hello minimum för alla överförda värden returneras.|  
|Max|Det finns två olika mönster för att anropa den här funktionen. <p>Här `max` en matris och hello funktionen returnerar `2`: <p>`max([0,1,2])` <p>Den här funktionen kan också ta en kommaavgränsad lista med värden och returnerar `2`: <p>`max(0,1,2)` <p> **Obs**: alla värden måste vara siffror, så om hello-parametern är en matris, hello matris har tooonly har siffror. <p> **Parameternummer**: 1 <p> **Namnet**: samlingen eller -värde <p> **Beskrivning**: krävs. Antingen en matris med värden toofind hello maxvärdet eller hello första värdet i en mängd. <p> **Parameternummer**: 2...*n* <p> **Namnet**: värde*n* <p> **Beskrivning**: valfria. Om hello första parametern är ett värde, du kan sedan skicka ytterligare värden och hello maxvärdet för alla överförda värden returneras.|  
|intervallet|Genererar en heltalsmatris som startar från ett visst antal. Du kan definiera hello tidslängd hello returnerat matris. <p>Den här funktionen returnerar till exempel `[3,4,5,6]`: <p>`range(3,4)` <p> **Parameternummer**: 1 <p> **Namnet**: startIndex <p> **Beskrivning**: krävs. hello första heltal i hello matris. <p> **Parameternummer**: 2 <p> **Namnet**: antal <p> **Beskrivning**: krävs. Det här värdet är hello antalet heltal som är i hello matris.|  
|SLUMP|Genererar ett slumpmässigt heltal inom hello angivna intervall (sammanlagt endast på första sidan). Den här funktionen kan till exempel returnera antingen `0` eller '1': <p>`rand(0,2)` <p> **Parameternummer**: 1 <p> **Namnet**: minsta <p> **Beskrivning**: krävs. hello lägsta heltal som kan returneras. <p> **Parameternummer**: 2 <p> **Namnet**: högsta <p> **Beskrivning**: krävs. Det här värdet är hello nästa heltal efter hello största heltal som kan returneras.|  
  
### <a name="date-functions"></a>Datumfunktioner  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|utcnow|Returnerar hello aktuell tidsstämpel som en sträng, till exempel: `2017-03-15T13:27:36Z`: <p>`utcnow()` <p> **Parameternummer**: 1 <p> **Namnet**: Format <p> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).|  
|lägg_till_sekunder|Lägger till ett heltal antal sekunder tooa sträng tidsstämpel skickades. hello antalet sekunder som kan vara positivt eller negativt. Som standard hello resultatet är en sträng i ISO 8601-format (”o”), om inte en Formatspecificeraren har angetts. Till exempel: `2015-03-15T13:27:00Z`: <p>`addseconds('2015-03-15T13:27:36Z', -36)` <p> **Parameternummer**: 1 <p> **Namnet**: tidsstämpel <p> **Beskrivning**: krävs. En sträng som innehåller hello tidpunkt. <p> **Parameternummer**: 2 <p> **Namnet**: sekunder <p> **Beskrivning**: krävs. hello antal sekunder tooadd. Kan vara negativa toosubtract sekunder. <p> **Parameternummer**: 3 <p> **Namnet**: Format <p> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).|  
|addminutes|Lägger till ett heltal antal minuter tooa sträng tidsstämpel skickades. hello antal minuter kan vara positivt eller negativt. Som standard hello resultatet är en sträng i ISO 8601-format (”o”), om inte en Formatspecificeraren har angetts. Till exempel: `2015-03-15T14:00:36Z`: <p>`addminutes('2015-03-15T13:27:36Z', 33)` <p> **Parameternummer**: 1 <p> **Namnet**: tidsstämpel <p> **Beskrivning**: krävs. En sträng som innehåller hello tidpunkt. <p> **Parameternummer**: 2 <p> **Namnet**: minuter <p> **Beskrivning**: krävs. hello antal minuter tooadd. Kan vara negativa toosubtract minuter. <p> **Parameternummer**: 3 <p> **Namnet**: Format <p> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).|  
|addhours|Lägger till ett heltal antal timmar tooa sträng tidsstämpel skickades. hello antal timmar kan vara positivt eller negativt. Som standard hello resultatet är en sträng i ISO 8601-format (”o”), om inte en Formatspecificeraren har angetts. Till exempel: `2015-03-16T01:27:36Z`: <p>`addhours('2015-03-15T13:27:36Z', 12)` <p> **Parameternummer**: 1 <p> **Namnet**: tidsstämpel <p> **Beskrivning**: krävs. En sträng som innehåller hello tidpunkt. <p> **Parameternummer**: 2 <p> **Namnet**: timmar <p> **Beskrivning**: krävs. hello antal timmar tooadd. Kan vara negativa toosubtract timmar. <p> **Parameternummer**: 3 <p> **Namnet**: Format <p> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).|  
|adddays|Lägger till ett heltal antal dagar tooa sträng tidsstämpel skickades. hello antalet dagar kan vara positivt eller negativt. Som standard hello resultatet är en sträng i ISO 8601-format (”o”), om inte en Formatspecificeraren har angetts. Till exempel: `2015-02-23T13:27:36Z`: <p>`addseconds('2015-03-15T13:27:36Z', -20)` <p> **Parameternummer**: 1 <p> **Namnet**: tidsstämpel <p> **Beskrivning**: krävs. En sträng som innehåller hello tidpunkt. <p> **Parameternummer**: 2 <p> **Namnet**: dagar <p> **Beskrivning**: krävs. hello antal dagar tooadd. Kan vara negativa toosubtract dagar. <p> **Parameternummer**: 3 <p> **Namnet**: Format <p> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).|  
|FormatDateTime|Returnerar en sträng i datumformat. Som standard hello resultatet är en sträng i ISO 8601-format (”o”), om inte en Formatspecificeraren har angetts. Till exempel: `2015-02-23T13:27:36Z`: <p>`formatDateTime('2015-03-15T13:27:36Z', 'o')` <p> **Parameternummer**: 1 <p> **Namnet**: datum <p> **Beskrivning**: krävs. En sträng som innehåller hello datum. <p> **Parameternummer**: 2 <p> **Namnet**: Format <p> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).|  
|startOfHour|Returnerar hello början av hello timme tooa sträng tidsstämpel skickades. Till exempel `2017-03-15T13:00:00Z`:<br /><br /> `startOfHour('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. Detta är en sträng som innehåller hello tid.<br /><br />**Parameternummer**: 2<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).|  
|startOfDay|Returnerar hello början av hello dag tooa sträng tidsstämpel skickades. Till exempel `2017-03-15T00:00:00Z`:<br /><br /> `startOfDay('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. Detta är en sträng som innehåller hello tid.<br /><br />**Parameternummer**: 2<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).| 
|startOfMonth|Returnerar hello början av hello månad tooa sträng tidsstämpel skickades. Till exempel `2017-03-01T00:00:00Z`:<br /><br /> `startOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. Detta är en sträng som innehåller hello tid.<br /><br />**Parameternummer**: 2<br /><br /> **Namnet**: Format<br /><br /> **Beskrivning**: valfria. Antingen en [enstaka format specificerare tecken](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) eller en [anpassat formatmönster](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) som anger hur tooformat hello värdet för den här tidsstämpel. Om formatet inte är anges, används hello ISO 8601-format (”o”).| 
|DayOfWeek|Returnerar hello dagen i veckan komponent i en sträng tidsstämpel.  Söndag är 0, måndag är 1 och så vidare. Till exempel `3`:<br /><br /> `dayOfWeek('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. Detta är en sträng som innehåller hello tid.| 
|Dag i månaden|Returnerar hello dagen i månaden komponent i en sträng tidsstämpel. Till exempel `15`:<br /><br /> `dayOfMonth('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. Detta är en sträng som innehåller hello tid.| 
|DayOfYear|Returnerar hello dagen på året komponent i en sträng tidsstämpel. Till exempel `74`:<br /><br /> `dayOfYear('2017-03-15T13:27:36Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. Detta är en sträng som innehåller hello tid.| 
|skalstreck|Returnerar hello markeringar-egenskapen för ett sträng-tidsstämpel. Till exempel `1489603019`:<br /><br /> `ticks('2017-03-15T18:36:59Z')`<br /><br /> **Parameternummer**: 1<br /><br /> **Namnet**: tidsstämpel<br /><br /> **Beskrivning**: krävs. Detta är en sträng som innehåller hello tid.| 
  
### <a name="workflow-functions"></a>Funktioner för arbetsflöde  

Dessa funktioner för att få information om själva hello arbetsflödet vid körning.  
  
|Funktionsnamn|Beskrivning|  
|-------------------|-----------------|  
|listCallbackUrl|Returnerar en sträng toocall tooinvoke hello utlösare eller åtgärd. <p> **Obs**: den här funktionen kan endast användas i en **httpWebhook** och **apiConnectionWebhook**, inte i en **manuell**, **återkommande**, **http**, eller **apiConnection**. <p>Till exempel hello `listCallbackUrl()` fungerar returnerar: <p>`https://prod-01.westus.logic.azure.com:443/workflows/1235...ABCD/triggers/manual/run?api-version=2015-08-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=xxx...xxx` |  
|arbetsflöde|Den här funktionen innehåller alla hello-information för hello själva arbetsflödet vid hello körning. <p> Tillgängliga egenskaper på hello arbetsflödesobjekt är: <ul><li>`name`</li><li>`type`</li><li>`id`</li><li>`location`</li><li>`run`</li></ul> <p> Hej värdet för hello `run` egenskapen är ett objekt med följande egenskaper: <ul><li>`name`</li><li>`type`</li><li>`id`</li></ul> <p>Se hello [Rest API](http://go.microsoft.com/fwlink/p/?LinkID=525617) mer information om dessa egenskaper.<p> Till exempel tooget hello namnet på hello aktuella kör, Använd hello `"@workflow().run.name"` uttryck. |

## <a name="next-steps"></a>Nästa steg

[Åtgärder och utlösare för arbetsflöde](logic-apps-workflow-actions-triggers.md)
