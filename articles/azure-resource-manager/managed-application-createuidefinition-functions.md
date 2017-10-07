---
title: aaaAzure hanterat program skapa UI definition funktioner | Microsoft Docs
description: "Beskriver hello funktioner toouse vid UI definitioner för hanterade program i Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 7a311a25404ccaec8c19c3ed8cd7038f6887c013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="createuidefinition-functions"></a>CreateUiDefinition funktioner
Det här avsnittet innehåller hello signaturer för alla funktioner som stöds av en CreateUiDefinition.

toouse en funktion surround hello-deklaration med hakparenteser. Exempel:

```json
"[function()]"
```

Strängar och andra funktioner kan refereras som parametrar för en funktion, men strängar måste omges av enkla citattecken. Exempel:

```json
"[fn1(fn2(), 'foobar')]"
```

Om tillämpligt, kan du referera egenskaper för hello utdata för en funktion med hjälp av hello punktoperatorn. Exempel:

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a>Refererar till funktioner
Dessa funktioner kan vara används tooreference utdata från hello egenskaper eller kontexten för en CreateUiDefinition.

### <a name="basics"></a>Grunderna
Returnerar hello utdatavärden för ett element som har definierats i hello grunderna steg.

hello följande exempel returnerar hello utdata från hello element med namnet `foo` i hello grundläggande steg:

```json
"[basics('foo')]"
```

### <a name="steps"></a>steg
Returnerar hello utdatavärden för ett element som har definierats i hello specifikt steg. tooget hello utdatavärden element i hello grunderna steget Använd `basics()` i stället.

hello följande exempel returnerar hello utdata från hello element med namnet `bar` i hello steg med namnet `foo`:

```json
"[steps('foo').bar]"
```

### <a name="location"></a>location
Returnerar hello-plats som väljs i hello grunderna steg eller hello aktuell kontext.

hello följande exempel kunde returnera `"westus"`:

```json
"[location()]"
```

## <a name="string-functions"></a>Strängfunktioner
Dessa funktioner kan endast användas med JSON-strängar.

### <a name="concat"></a>concat
Sammanfogar en eller flera strängar.

Om exempelvis hello utdata värdet för `element1` om `"bar"`, och sedan det här exemplet returnerar hello sträng `"foobar!"`:

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a>delsträngen
Returnerar hello delsträngen av hello angett sträng. hello delsträngen börjar vid hello angivet index och angett hello längd.

hello följande exempel returnerar `"ftw"`:

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a>Ersätt
Returnerar en sträng i vilken alla förekomster av hello angett sträng i hello aktuella strängen ersätts med en annan sträng.

hello följande exempel returnerar `"Everything is awesome!"`:

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a>GUID
Genererar en globalt unik sträng (GUID).

hello följande exempel kunde returnera `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:

```json
"[guid()]"
```

### <a name="tolower"></a>toLower
Returnerar en sträng som konverteras toolowercase.

hello följande exempel returnerar `"foobar"`:

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a>toUpper
Returnerar en sträng som konverteras toouppercase.

hello följande exempel returnerar `"FOOBAR"`:

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a>Samlingen funktioner
Dessa funktioner kan användas med samlingar som JSON-strängar, matriser och -objekt.

### <a name="contains"></a>Innehåller
Returnerar `true` om en sträng innehåller Hej angivna delsträngen, en matris innehåller hello angivet värde eller ett objekt innehåller hello angiven nyckel.

#### <a name="example-1-string"></a>Exempel 1: sträng
hello följande exempel returnerar `true`:

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a>Exempel 2: matris
Anta att `element1` returnerar `[1, 2, 3]`. hello följande exempel returnerar `false`:

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a>Exempel 3: objektet
Anta att `element1` returnerar:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello följande exempel returnerar `true`:

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a>Längd
Returnerar hello antalet tecken i en sträng, hello antalet värden i en matris eller hello antalet nycklar i ett objekt.

#### <a name="example-1-string"></a>Exempel 1: sträng
hello följande exempel returnerar `6`:

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a>Exempel 2: matris
Anta att `element1` returnerar `[1, 2, 3]`. hello följande exempel returnerar `3`:

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Exempel 3: objektet
Anta att `element1` returnerar:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello följande exempel returnerar `2`:

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a>tom
Returnerar `true` om hello sträng, matris eller ett objekt är null eller tomt.

#### <a name="example-1-string"></a>Exempel 1: sträng
hello följande exempel returnerar `true`:

```json
"[empty('')]"
```

#### <a name="example-2-array"></a>Exempel 2: matris
Anta att `element1` returnerar `[1, 2, 3]`. hello följande exempel returnerar `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Exempel 3: objektet
Anta att `element1` returnerar:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello följande exempel returnerar `false`:

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a>Exempel 4: null och odefinierad
Anta att `element1` är `null` eller odefinierad. hello följande exempel returnerar `true`:

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a>första
Returnerar hello första tecknet i hello angett sträng; första värde för den angivna matrisen med hello; eller hello första nyckel och värde för hello angivna-objektet.

#### <a name="example-1-string"></a>Exempel 1: sträng
hello följande exempel returnerar `"f"`:

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a>Exempel 2: matris
Anta att `element1` returnerar `[1, 2, 3]`. hello följande exempel returnerar `1`:

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Exempel 3: objektet
Anta att `element1` returnerar:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
hello följande exempel returnerar `{"key1": "foobar"}`:

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a>senaste
Returnerar hello sista tecknet i hello angetts sträng, hello sista värdet för den angivna matrisen med hello, eller hello senaste nyckel och värde för hello angivna-objektet.

#### <a name="example-1-string"></a>Exempel 1: sträng
hello följande exempel returnerar `"r"`:

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a>Exempel 2: matris
Anta att `element1` returnerar `[1, 2, 3]`. hello följande exempel returnerar `2`:

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a>Exempel 3: objektet
Anta att `element1` returnerar:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello följande exempel returnerar `{"key2": "raboof"}`:

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a>ta
Returnerar ett angivet antal sammanhängande tecken från hello början av hello sträng, ett angivet antal sammanhängande värden från hello start av hello matris eller ett angivet antal sammanhängande nycklar och värden från hello start av hello-objektet.

#### <a name="example-1-string"></a>Exempel 1: sträng
hello följande exempel returnerar `"foo"`:

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a>Exempel 2: matris
Anta att `element1` returnerar `[1, 2, 3]`. hello följande exempel returnerar `[1, 2]`:

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Exempel 3: objektet
Anta att `element1` returnerar:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

hello följande exempel returnerar `{"key1": "foobar"}`:

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a>Hoppa över
Kringgår ett angivet antal element i en mängd och returnerar sedan hello återstående element.

#### <a name="example-1-string"></a>Exempel 1: sträng
hello följande exempel returnerar `"bar"`:

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a>Exempel 2: matris
Anta att `element1` returnerar `[1, 2, 3]`. hello följande exempel returnerar `[3]`:

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a>Exempel 3: objektet
Anta att `element1` returnerar:

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
hello följande exempel returnerar `{"key2": "raboof"}`:

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a>Logiska funktioner
Dessa funktioner kan användas i villkorlig sats. Vissa funktioner kanske inte stöder alla JSON-datatyper.

### <a name="equals"></a>är lika med
Returnerar `true` om båda parametrarna har hello samma anger och värde. Den här funktionen stöder alla JSON-datatyper.

hello följande exempel returnerar `true`:

```json
"[equals(0, 0)]"
```

hello följande exempel returnerar `true`:

```json
"[equals('foo', 'foo')]"
```

hello följande exempel returnerar `false`:

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a>mindre
Returnerar `true` om hello första parametern är strikt mindre än andra hello-parametern. Den här funktionen har stöd för parametrarna endast för nummer och strängen.

hello följande exempel returnerar `true`:

```json
"[less(1, 2)]"
```

hello följande exempel returnerar `false`:

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a>lessOrEquals
Returnerar `true` om hello första parametern är mindre än eller lika med toohello andra parametern. Den här funktionen har stöd för parametrarna endast för nummer och strängen.

hello följande exempel returnerar `true`:

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a>större
Returnerar `true` om hello första parametern är strikt större än andra hello-parametern. Den här funktionen har stöd för parametrarna endast för nummer och strängen.

hello följande exempel returnerar `false`:

```json
"[greater(1, 2)]"
```

hello följande exempel returnerar `true`:

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a>greaterOrEquals
Returnerar `true` om hello första parametern är större än eller lika toohello andra parametern. Den här funktionen har stöd för parametrarna endast för nummer och strängen.

hello följande exempel returnerar `true`:

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a>och
Returnerar `true` om alla hello parametrarna för`true`. Den här funktionen har stöd för två eller flera parametrar av typen Boolean.

hello följande exempel returnerar `true`:

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

hello följande exempel returnerar `false`:

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a>eller
Returnerar `true` om minst en av parametrarna hello utvärderar för`true`. Den här funktionen har stöd för två eller flera parametrar av typen Boolean.

hello följande exempel returnerar `true`:

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

hello följande exempel returnerar `true`:

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a>inte
Returnerar `true` om hello parametern utvärderar för`false`. Den här funktionen har stöd för parametrar av typen Boolean.

hello följande exempel returnerar `true`:

```json
"[not(false)]"
```

hello följande exempel returnerar `false`:

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a>Slå samman
Returnerar hello hello första icke-null-parameterns värde. Den här funktionen stöder alla JSON-datatyper.

Anta att `element1` och `element2` är odefinierad. hello följande exempel returnerar `"foobar"`:

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a>Konverteringsfunktioner
Dessa funktioner kan vara används tooconvert värden mellan JSON-datatyper och kodningar.

### <a name="int"></a>int
Konverterar hello parametern tooan heltal. Den här funktionen har stöd för parametrar av typen nummer och strängen.

hello följande exempel returnerar `1`:

```json
"[int('1')]"
```

hello följande exempel returnerar `2`:

```json
"[int(2.9)]"
```

### <a name="float"></a>flyttal
Konverterar hello parametern tooa flyttal. Den här funktionen har stöd för parametrar av typen nummer och strängen.

hello följande exempel returnerar `1.0`:

```json
"[float('1.0')]"
```

hello följande exempel returnerar `2.9`:

```json
"[float(2.9)]"
```

### <a name="string"></a>Sträng
Konverterar hello parametern tooa sträng. Den här funktionen stöder alla datatyper i JSON-parametrar.

hello följande exempel returnerar `"1"`:

```json
"[string(1)]"
```

hello följande exempel returnerar `"2.9"`:

```json
"[string(2.9)]"
```

hello följande exempel returnerar `"[1,2,3]"`:

```json
"[string([1,2,3])]"
```

hello följande exempel returnerar `"{"foo":"bar"}"`:

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a>bool
Konverterar hello parametern tooa booleskt värde. Den här funktionen har stöd för parametrar av typen antal, sträng och booleskt värde. Liknande tooBooleans i JavaScript, vilket värde som helst förutom `0` eller `'false'` returnerar `true`.

hello följande exempel returnerar `true`:

```json
"[bool(1)]"
```

hello följande exempel returnerar `false`:

```json
"[bool(0)]"
```

hello följande exempel returnerar `true`:

```json
"[bool(true)]"
```

hello följande exempel returnerar `true`:

```json
"[bool('true')]"
```

### <a name="parse"></a>parsa
Konverterar hello tooa egna parametertypen. I den här funktionen är med andra ord hello inversen av `string()`. Den här funktionen stöder endast av typen string-parametrar.

hello följande exempel returnerar `1`:

```json
"[parse('1')]"
```

hello följande exempel returnerar `true`:

```json
"[parse('true')]"
```

hello följande exempel returnerar `[1,2,3]`:

```json
"[parse('[1,2,3]')]"
```

hello följande exempel returnerar `{"foo":"bar"}`:

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a>encodeBase64
Kodar hello parametern tooa Base64-kodad sträng. Den här funktionen stöder endast av typen string-parametrar.

hello följande exempel returnerar `"Zm9vYmFy"`:

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a>decodeBase64
Avkodar hello-parameter från en Base64-kodad sträng. Den här funktionen stöder endast av typen string-parametrar.

hello följande exempel returnerar `"foobar"`:

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a>encodeUriComponent
Kodar hello parametern tooa URL-kodad sträng. Den här funktionen stöder endast av typen string-parametrar.

hello följande exempel returnerar `"https%3A%2F%2Fportal.azure.com%2F"`:

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a>decodeUriComponent
Avkodar hello-parameter från en URL-kodad sträng. Den här funktionen stöder endast av typen string-parametrar.

hello följande exempel returnerar `"https://portal.azure.com/"`:

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a>Matematiska funktioner
### <a name="add"></a>Lägg till
Adderar två tal och returnerar hello resultat.

hello följande exempel returnerar `3`:

```json
"[add(1, 2)]"
```

### <a name="sub"></a>Sub
Subtraherar hello andra tal från första hello och returnerar hello resultat.

hello följande exempel returnerar `1`:

```json
"[sub(3, 2)]"
```

### <a name="mul"></a>mul
Multiplicerar två tal och returnerar hello resultat.

hello följande exempel returnerar `6`:

```json
"[mul(2, 3)]"
```

### <a name="div"></a>div
Dividerar hello hello andra talet första tal och returnerar hello resultat. hello resultatet är alltid ett heltal.

hello följande exempel returnerar `2`:

```json
"[div(6, 3)]"
```

### <a name="mod"></a>MOD
Dividerar hello hello andra talet första tal och returnerar hello resten.

hello följande exempel returnerar `0`:

```json
"[mod(6, 3)]"
```

hello följande exempel returnerar `2`:

```json
"[mod(6, 4)]"
```

### <a name="min"></a>min.
Returnerar hello liten av hello två tal.

hello följande exempel returnerar `1`:

```json
"[min(1, 2)]"
```

### <a name="max"></a>Max
Returnerar hello större av hello två tal.

hello följande exempel returnerar `2`:

```json
"[max(1, 2)]"
```

### <a name="range"></a>intervallet
Genererar en sekvens av integral siffror i hello intervall som anges.

hello följande exempel returnerar `[1,2,3]`:

```json
"[range(1, 3)]"
```

### <a name="rand"></a>SLUMP
Returnerar ett slumpmässigt heltal inom hello intervall som anges. Den här funktionen genererar inte kryptografiskt säker slumptal.

hello följande exempel kunde returnera `42`:

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a>våning
Returnerar hello största heltal som är mindre än eller lika toohello angetts nummer.

hello följande exempel returnerar `3`:

```json
"[floor(3.14)]"
```

### <a name="ceil"></a>ceil
Returnerar hello största heltal som är större än eller lika med toohello angetts nummer.

hello följande exempel returnerar `4`:

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a>Datumfunktioner
### <a name="utcnow"></a>utcNow
Returnerar en sträng i ISO 8601-format för hello på hello lokala datorns aktuella datum och tid.

hello följande exempel kunde returnera `"1990-12-31T23:59:59.000Z"`:

```json
"[utcNow()]"
```

### <a name="addseconds"></a>Lägg_till_sekunder
Lägger till ett heltal med sekunder toohello angivna tidsstämpel.

hello följande exempel returnerar `"1991-01-01T00:00:00.000Z"`:

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a>addMinutes
Lägger till ett heltal med minuter toohello angivna tidsstämpel.

hello följande exempel returnerar `"1991-01-01T00:00:59.000Z"`:

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a>addHours
Lägger till ett heltal med timmar toohello angivna tidsstämpel.

hello följande exempel returnerar `"1991-01-01T00:59:59.000Z"`:

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a>Nästa steg
* En introduktion tooAzure Resource Manager finns [översikt över Azure Resource Manager](resource-group-overview.md).

