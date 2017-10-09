---
title: "aaaHandle innehållstyper - Azure Logic Apps | Microsoft Docs"
description: "Hur Azure Logikappar behandlar innehållstyper vid design och körning"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: cd1f08fd-8cde-4afc-86ff-2e5738cc8288
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: a823249c5388b15ae0aae450b40499b420ea005e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="handle-content-types-in-logic-apps"></a>Hantera innehållstyper i logikappar

Många olika typer av innehåll kan flöda via en logikapp, inklusive JSON, XML, flat-filer och binära data. Medan hello Logic Apps motorn stöder alla typer av innehåll, Förstå vissa internt av hello Logic Apps-motorn. Andra kanske behöver omvandling eller konverteringar vid behov. Den här artikeln beskriver hur hello motorn hanteras olika typer av innehåll och hur toocorrectly hantera dessa typer vid behov.

## <a name="content-type-header"></a>Content-Type-huvud

toostart i princip ska vi titta på hello två `Content-Types` som inte kräver konvertering eller omvandling som du kan använda i en logikapp: `application/json` och `text/plain`.

## <a name="applicationjson"></a>Application/JSON

Hej arbetsflödesmotorn förlitar sig på hello `Content-Type` huvudet från HTTP anropar toodetermine hello lämpliga hantering. Alla förfrågningar med hello innehållstypen `application/json` lagras och hanteras som ett JSON-objekt. Innehållet i JSON kan dessutom tolkas som standard utan någon omvandling. 

Till exempel en begäran med hello content-type-huvud gick att parsa `application/json ` i ett arbetsflöde med hjälp av ett uttryck som `@body('myAction')['foo'][0]` tooget hello värdet `bar` i det här fallet:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

Inga ytterligare omvandling krävs. Om du arbetar med data som är JSON men inte har ett huvud angivet, du kan manuellt omvandla den tooJSON med hello `@json()` fungerar, till exempel: `@json(triggerBody())['foo']`.

### <a name="schema-and-schema-generator"></a>Schema- och schema-generator

hello begäran utlösare kan du tooenter en JSON-schema för hello nyttolasten du förväntar dig tooreceive. Det här schemat kan hello designer generera token så att du kan använda hello innehåll för hello-begäran. Om du inte har ett schema som är klar, Välj **Använd exemplet nyttolast toogenerate schemat**, så du kan generera ett JSON-schema från en exempel-nyttolast.

![Schemat](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a>'Parsa JSON-åtgärd

Hej `Parse JSON` kan parsa JSON-innehåll till eget token för förbrukning av logik app. Liknande toohello begäran utlösare med hjälp av den här åtgärden kan du ange eller skapa en JSON-schema för hello innehåll du vill tooparse. Det här verktyget gör datahantering från Service Bus, Azure Cosmos DB och så vidare, enklare.

![Parsa JSON](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a>Text/plain

Liknande för`application/json`, HTTP-meddelanden som tagits emot med hello `Content-Type` rubriken för `text/plain` lagras i obearbetat format. Även om dessa meddelanden ingår i efterföljande åtgärder utan omvandling dessa begäranden går med `Content-Type`: `text/plain` rubrik. Till exempel när du arbetar med en flat-fil kan du få den här HTTP innehåll som `text/plain`:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

Om i hello nästa åtgärd, skickar hello begäran som hello innehållet i en annan begäran (`@body('flatfile')`), hello förfrågan skulle ha en `text/plain` Content-Type-huvud. Om du arbetar med data som har oformaterad text men inte har ett huvud angivet, kan du manuellt omvandla hello data tootext med hello `@string()` fungerar, till exempel: `@string(triggerBody())`.

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Application/xml och program/oktett-ström och konverterare funktioner

hello Logic Apps motorn alltid bevarar hello `Content-Type` som togs emot på hello HTTP-begäran eller ett svar. Om hello motorn tar emot innehåll med hello `Content-Type` av `application/octet-stream`, och inkluderar att innehåll i en efterföljande åtgärd utan omvandling, hello utgående begäran har `Content-Type`: `application/octet-stream`. Det här sättet kan hello-motorn garantera att data inte förloras när via hello arbetsflöde. Hello åtgärdstillstånd (indata och utdata) lagras dock i ett JSON-objekt som hello tillstånd flyttar via hello arbetsflöde. Så toopreserve vissa datatyper, hello motorn konverterar hello innehåll tooa binary base64-kodad sträng med tillämpliga metadata som bevarar både `$content` och `$content-type`, som automatiskt ska konverteras. 

* `@json()`-kastar data för`application/json`
* `@xml()`-kastar data för`application/xml`
* `@binary()`-kastar data för`application/octet-stream`
* `@string()`-kastar data för`text/plain`
* `@base64()`-Konverterar innehållet tooa base64-sträng
* `@base64toString()`– konverterar en base64-kodad sträng för`text/plain`
* `@base64toBinary()`– konverterar en base64-kodad sträng för`application/octet-stream`
* `@encodeDataUri()`-kodar en sträng som en bitmatris dataUri
* `@decodeDataUri()`-avkodar ett dataUri till en bytematris

Till exempel om du har fått en HTTP-begäran med `Content-Type`: `application/xml`:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Du kan konvertera och använda senare med något som liknar `@xml(triggerBody())`, eller i en funktion som `@xpath(xml(triggerBody()), '/CustomerName')`.

## <a name="other-content-types"></a>Andra typer av innehåll

Andra typer av innehåll som stöds och arbeta med logic apps, men kan kräva manuellt hämtning hello meddelandetexten genom att avkoda hello `$content`. Anta att du utlöser en `application/x-www-url-formencoded` begäran var `$content` är hello nyttolast kodad som en base64-sträng toopreserve alla data:

```
CustomerName=Frank&Address=123+Avenue
```

Eftersom hello begäran inte oformaterad text eller JSON, hello begäran lagras i hello-åtgärd på följande sätt:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

För närvarande finns det inte finns en inbyggd funktion för formulärdata, så att du kan fortfarande använda informationen i ett arbetsflöde genom att öppna manuellt hello data med en funktion som `@string(body('formdataAction'))`. Om du vill använda hello utgående begärans tooalso har hello `application/x-www-url-formencoded` content-type-huvud, kan du lägga till hello toohello åtgärd begärandetexten utan någon omvandling som `@body('formdataAction')`. Men den här metoden fungerar bara om hello brödtext är hello endast parameter i hello `body` indata. Om du försöker toouse `@body('formdataAction')` i en `application/json` begär kan du få ett körningsfel eftersom hello kodade brödtext skickas.

