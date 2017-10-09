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
# <a name="handle-content-types-in-logic-apps"></a><span data-ttu-id="c0632-103">Hantera innehållstyper i logikappar</span><span class="sxs-lookup"><span data-stu-id="c0632-103">Handle content types in logic apps</span></span>

<span data-ttu-id="c0632-104">Många olika typer av innehåll kan flöda via en logikapp, inklusive JSON, XML, flat-filer och binära data.</span><span class="sxs-lookup"><span data-stu-id="c0632-104">Many different types of content can flow through a logic app, including JSON, XML, flat files, and binary data.</span></span> <span data-ttu-id="c0632-105">Medan hello Logic Apps motorn stöder alla typer av innehåll, Förstå vissa internt av hello Logic Apps-motorn.</span><span class="sxs-lookup"><span data-stu-id="c0632-105">While hello Logic Apps Engine supports all content types, some are natively understood by hello Logic Apps Engine.</span></span> <span data-ttu-id="c0632-106">Andra kanske behöver omvandling eller konverteringar vid behov.</span><span class="sxs-lookup"><span data-stu-id="c0632-106">Others might require casting or conversions as necessary.</span></span> <span data-ttu-id="c0632-107">Den här artikeln beskriver hur hello motorn hanteras olika typer av innehåll och hur toocorrectly hantera dessa typer vid behov.</span><span class="sxs-lookup"><span data-stu-id="c0632-107">This article describes how hello engine handles different content types and how toocorrectly handle these types when necessary.</span></span>

## <a name="content-type-header"></a><span data-ttu-id="c0632-108">Content-Type-huvud</span><span class="sxs-lookup"><span data-stu-id="c0632-108">Content-Type Header</span></span>

<span data-ttu-id="c0632-109">toostart i princip ska vi titta på hello två `Content-Types` som inte kräver konvertering eller omvandling som du kan använda i en logikapp: `application/json` och `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="c0632-109">toostart basically, let's look at hello two `Content-Types` that don't require conversion or casting that you can use in a logic app: `application/json` and `text/plain`.</span></span>

## <a name="applicationjson"></a><span data-ttu-id="c0632-110">Application/JSON</span><span class="sxs-lookup"><span data-stu-id="c0632-110">Application/JSON</span></span>

<span data-ttu-id="c0632-111">Hej arbetsflödesmotorn förlitar sig på hello `Content-Type` huvudet från HTTP anropar toodetermine hello lämpliga hantering.</span><span class="sxs-lookup"><span data-stu-id="c0632-111">hello workflow engine relies on hello `Content-Type` header from HTTP calls toodetermine hello appropriate handling.</span></span> <span data-ttu-id="c0632-112">Alla förfrågningar med hello innehållstypen `application/json` lagras och hanteras som ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="c0632-112">Any request with hello content type `application/json` is stored and handled as a JSON Object.</span></span> <span data-ttu-id="c0632-113">Innehållet i JSON kan dessutom tolkas som standard utan någon omvandling.</span><span class="sxs-lookup"><span data-stu-id="c0632-113">Also, JSON content can be parsed by default without needing any casting.</span></span> 

<span data-ttu-id="c0632-114">Till exempel en begäran med hello content-type-huvud gick att parsa `application/json ` i ett arbetsflöde med hjälp av ett uttryck som `@body('myAction')['foo'][0]` tooget hello värdet `bar` i det här fallet:</span><span class="sxs-lookup"><span data-stu-id="c0632-114">For example, you could parse a request that has hello content type header `application/json ` in a workflow by using an expression like `@body('myAction')['foo'][0]` tooget hello value `bar` in this case:</span></span>

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

<span data-ttu-id="c0632-115">Inga ytterligare omvandling krävs.</span><span class="sxs-lookup"><span data-stu-id="c0632-115">No additional casting is needed.</span></span> <span data-ttu-id="c0632-116">Om du arbetar med data som är JSON men inte har ett huvud angivet, du kan manuellt omvandla den tooJSON med hello `@json()` fungerar, till exempel: `@json(triggerBody())['foo']`.</span><span class="sxs-lookup"><span data-stu-id="c0632-116">If you are working with data that is JSON but didn't have a header specified, you can manually cast it tooJSON using hello `@json()` function, for example: `@json(triggerBody())['foo']`.</span></span>

### <a name="schema-and-schema-generator"></a><span data-ttu-id="c0632-117">Schema- och schema-generator</span><span class="sxs-lookup"><span data-stu-id="c0632-117">Schema and schema generator</span></span>

<span data-ttu-id="c0632-118">hello begäran utlösare kan du tooenter en JSON-schema för hello nyttolasten du förväntar dig tooreceive.</span><span class="sxs-lookup"><span data-stu-id="c0632-118">hello Request trigger lets you tooenter a JSON schema for hello payload you expect tooreceive.</span></span> <span data-ttu-id="c0632-119">Det här schemat kan hello designer generera token så att du kan använda hello innehåll för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="c0632-119">This schema lets hello designer generate tokens so you can consume hello content of hello request.</span></span> <span data-ttu-id="c0632-120">Om du inte har ett schema som är klar, Välj **Använd exemplet nyttolast toogenerate schemat**, så du kan generera ett JSON-schema från en exempel-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="c0632-120">If you don't have a schema ready, select **Use sample payload toogenerate schema**, so you can generate a JSON schema from a sample payload.</span></span>

![Schemat](./media/logic-apps-http-endpoint/manualtrigger.png)

### <a name="parse-json-action"></a><span data-ttu-id="c0632-122">'Parsa JSON-åtgärd</span><span class="sxs-lookup"><span data-stu-id="c0632-122">'Parse JSON' action</span></span>

<span data-ttu-id="c0632-123">Hej `Parse JSON` kan parsa JSON-innehåll till eget token för förbrukning av logik app.</span><span class="sxs-lookup"><span data-stu-id="c0632-123">hello `Parse JSON` action lets you parse JSON content into friendly tokens for logic app consumption.</span></span> <span data-ttu-id="c0632-124">Liknande toohello begäran utlösare med hjälp av den här åtgärden kan du ange eller skapa en JSON-schema för hello innehåll du vill tooparse.</span><span class="sxs-lookup"><span data-stu-id="c0632-124">Similar toohello Request trigger, this action lets you enter or generate a JSON schema for hello content you want tooparse.</span></span> <span data-ttu-id="c0632-125">Det här verktyget gör datahantering från Service Bus, Azure Cosmos DB och så vidare, enklare.</span><span class="sxs-lookup"><span data-stu-id="c0632-125">This tool makes consuming data from Service Bus, Azure Cosmos DB, and so on, much easier.</span></span>

![Parsa JSON](./media/logic-apps-content-type/ParseJSON.png)

## <a name="textplain"></a><span data-ttu-id="c0632-127">Text/plain</span><span class="sxs-lookup"><span data-stu-id="c0632-127">Text/plain</span></span>

<span data-ttu-id="c0632-128">Liknande för`application/json`, HTTP-meddelanden som tagits emot med hello `Content-Type` rubriken för `text/plain` lagras i obearbetat format.</span><span class="sxs-lookup"><span data-stu-id="c0632-128">Similar too`application/json`, HTTP messages received with hello `Content-Type` header of `text/plain` are stored in raw form.</span></span> <span data-ttu-id="c0632-129">Även om dessa meddelanden ingår i efterföljande åtgärder utan omvandling dessa begäranden går med `Content-Type`: `text/plain` rubrik.</span><span class="sxs-lookup"><span data-stu-id="c0632-129">Also, if those messages are included in subsequent actions without casting, those requests go out with  `Content-Type`: `text/plain` header.</span></span> <span data-ttu-id="c0632-130">Till exempel när du arbetar med en flat-fil kan du få den här HTTP innehåll som `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="c0632-130">For example, when working with a flat file, you might get this HTTP content as `text/plain`:</span></span>

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

<span data-ttu-id="c0632-131">Om i hello nästa åtgärd, skickar hello begäran som hello innehållet i en annan begäran (`@body('flatfile')`), hello förfrågan skulle ha en `text/plain` Content-Type-huvud.</span><span class="sxs-lookup"><span data-stu-id="c0632-131">If in hello next action, you send hello request as hello body of another request (`@body('flatfile')`), hello request would have a `text/plain` Content-Type header.</span></span> <span data-ttu-id="c0632-132">Om du arbetar med data som har oformaterad text men inte har ett huvud angivet, kan du manuellt omvandla hello data tootext med hello `@string()` fungerar, till exempel: `@string(triggerBody())`.</span><span class="sxs-lookup"><span data-stu-id="c0632-132">If you are working with data that is plain text but didn't have a header specified, you can manually cast hello data tootext using hello `@string()` function, for example: `@string(triggerBody())`.</span></span>

## <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a><span data-ttu-id="c0632-133">Application/xml och program/oktett-ström och konverterare funktioner</span><span class="sxs-lookup"><span data-stu-id="c0632-133">Application/xml and Application/octet-stream and converter functions</span></span>

<span data-ttu-id="c0632-134">hello Logic Apps motorn alltid bevarar hello `Content-Type` som togs emot på hello HTTP-begäran eller ett svar.</span><span class="sxs-lookup"><span data-stu-id="c0632-134">hello Logic Apps Engine always preserves hello `Content-Type` that was received on hello HTTP request or response.</span></span> <span data-ttu-id="c0632-135">Om hello motorn tar emot innehåll med hello `Content-Type` av `application/octet-stream`, och inkluderar att innehåll i en efterföljande åtgärd utan omvandling, hello utgående begäran har `Content-Type`: `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="c0632-135">So if hello engine receives content with hello `Content-Type` of `application/octet-stream`, and you include that content in a subsequent action without casting, hello outgoing request has `Content-Type`: `application/octet-stream`.</span></span> <span data-ttu-id="c0632-136">Det här sättet kan hello-motorn garantera att data inte förloras när via hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="c0632-136">This way, hello engine can guarantee data isn't lost while moving through hello workflow.</span></span> <span data-ttu-id="c0632-137">Hello åtgärdstillstånd (indata och utdata) lagras dock i ett JSON-objekt som hello tillstånd flyttar via hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="c0632-137">However, hello action state (inputs and outputs) is stored in a JSON object as hello state moves through hello workflow.</span></span> <span data-ttu-id="c0632-138">Så toopreserve vissa datatyper, hello motorn konverterar hello innehåll tooa binary base64-kodad sträng med tillämpliga metadata som bevarar både `$content` och `$content-type`, som automatiskt ska konverteras.</span><span class="sxs-lookup"><span data-stu-id="c0632-138">So toopreserve some data types, hello engine converts hello content tooa binary base64 encoded string with appropriate metadata that preserves both `$content` and `$content-type`, which are automatically be converted.</span></span> 

* <span data-ttu-id="c0632-139">`@json()`-kastar data för`application/json`</span><span class="sxs-lookup"><span data-stu-id="c0632-139">`@json()` - casts data too`application/json`</span></span>
* <span data-ttu-id="c0632-140">`@xml()`-kastar data för`application/xml`</span><span class="sxs-lookup"><span data-stu-id="c0632-140">`@xml()` - casts data too`application/xml`</span></span>
* <span data-ttu-id="c0632-141">`@binary()`-kastar data för`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="c0632-141">`@binary()` - casts data too`application/octet-stream`</span></span>
* <span data-ttu-id="c0632-142">`@string()`-kastar data för`text/plain`</span><span class="sxs-lookup"><span data-stu-id="c0632-142">`@string()` - casts data too`text/plain`</span></span>
* <span data-ttu-id="c0632-143">`@base64()`-Konverterar innehållet tooa base64-sträng</span><span class="sxs-lookup"><span data-stu-id="c0632-143">`@base64()` - converts content tooa base64 string</span></span>
* <span data-ttu-id="c0632-144">`@base64toString()`– konverterar en base64-kodad sträng för`text/plain`</span><span class="sxs-lookup"><span data-stu-id="c0632-144">`@base64toString()` - converts a base64 encoded string too`text/plain`</span></span>
* <span data-ttu-id="c0632-145">`@base64toBinary()`– konverterar en base64-kodad sträng för`application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="c0632-145">`@base64toBinary()` - converts a base64 encoded string too`application/octet-stream`</span></span>
* <span data-ttu-id="c0632-146">`@encodeDataUri()`-kodar en sträng som en bitmatris dataUri</span><span class="sxs-lookup"><span data-stu-id="c0632-146">`@encodeDataUri()` - encodes a string as a dataUri byte array</span></span>
* <span data-ttu-id="c0632-147">`@decodeDataUri()`-avkodar ett dataUri till en bytematris</span><span class="sxs-lookup"><span data-stu-id="c0632-147">`@decodeDataUri()` - decodes a dataUri into a byte array</span></span>

<span data-ttu-id="c0632-148">Till exempel om du har fått en HTTP-begäran med `Content-Type`: `application/xml`:</span><span class="sxs-lookup"><span data-stu-id="c0632-148">For example, if you received an HTTP request with `Content-Type`: `application/xml`:</span></span>

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

<span data-ttu-id="c0632-149">Du kan konvertera och använda senare med något som liknar `@xml(triggerBody())`, eller i en funktion som `@xpath(xml(triggerBody()), '/CustomerName')`.</span><span class="sxs-lookup"><span data-stu-id="c0632-149">You could cast and use later with something like `@xml(triggerBody())`, or in a function like `@xpath(xml(triggerBody()), '/CustomerName')`.</span></span>

## <a name="other-content-types"></a><span data-ttu-id="c0632-150">Andra typer av innehåll</span><span class="sxs-lookup"><span data-stu-id="c0632-150">Other content types</span></span>

<span data-ttu-id="c0632-151">Andra typer av innehåll som stöds och arbeta med logic apps, men kan kräva manuellt hämtning hello meddelandetexten genom att avkoda hello `$content`.</span><span class="sxs-lookup"><span data-stu-id="c0632-151">Other content types are supported and work with logic apps, but might require manually retrieving hello message body by decoding hello `$content`.</span></span> <span data-ttu-id="c0632-152">Anta att du utlöser en `application/x-www-url-formencoded` begäran var `$content` är hello nyttolast kodad som en base64-sträng toopreserve alla data:</span><span class="sxs-lookup"><span data-stu-id="c0632-152">For example, suppose you trigger an `application/x-www-url-formencoded` request where `$content` is hello payload encoded as a base64 string toopreserve all data:</span></span>

```
CustomerName=Frank&Address=123+Avenue
```

<span data-ttu-id="c0632-153">Eftersom hello begäran inte oformaterad text eller JSON, hello begäran lagras i hello-åtgärd på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c0632-153">Because hello request isn't plain text or JSON, hello request is stored in hello action as follows:</span></span>

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

För närvarande finns det inte finns en inbyggd funktion för formulärdata, så att du kan fortfarande använda informationen i ett arbetsflöde genom att öppna manuellt hello data med en funktion som `@string(body('formdataAction'))`. Om du vill använda hello utgående begärans tooalso har hello `application/x-www-url-formencoded` content-type-huvud, kan du lägga till hello toohello åtgärd begärandetexten utan någon omvandling som `@body('formdataAction')`. Men den här metoden fungerar bara om hello brödtext är hello endast parameter i hello `body` indata. <span data-ttu-id="c0632-157">Om du försöker toouse `@body('formdataAction')` i en `application/json` begär kan du få ett körningsfel eftersom hello kodade brödtext skickas.</span><span class="sxs-lookup"><span data-stu-id="c0632-157">If you try toouse `@body('formdataAction')` in an `application/json` request, you get a runtime error because hello encoded body is sent.</span></span>

