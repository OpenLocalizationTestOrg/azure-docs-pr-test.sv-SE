---
title: aaaSchema uppdaterar juni 1 2016 - Azure Logic Apps | Microsoft Docs
description: "Skapa JSON-definitioner för Logic Apps i Azure med schemaversionen 2016-06-01"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: b0347fbbd692a93b63a2f8b741402a225450b35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Schema-uppdateringar för Logikappar i Azure - 1 juni 2016

Den här nya schemat och API-version för Logikappar i Azure innehåller viktiga förbättringar som gör logikappar mer tillförlitlig och enklare toouse:

* [Scope](#scopes) låter dig kapsla åtgärder som en samling åtgärder.
* [Villkor och slingor](#conditions-loops) är nu förstklassigt åtgärder.
* Mer exakta ordning för att köra åtgärder med hello `runAfter` egenskapen ersätta`dependsOn`

tooupgrade dina logikappar från hello 1 augusti 2015 Förhandsgranska schemat toohello den 1 juni 2016 schemat [kolla hello uppgradera avsnittet](##upgrade-your-schema).

<a name="scopes"></a>
## <a name="scopes"></a>Scope

Det här schemat innehåller scope, så att du kan gruppera tillsammans eller kapslade åtgärder i varandra. Ett villkor kan exempelvis innehålla ytterligare villkor. Lär dig mer om [omfång syntax](../logic-apps/logic-apps-loops-and-scopes.md), eller läsa det här exemplet grundläggande omfång:

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

<a name="conditions-loops"></a>
## <a name="conditions-and-loops-changes"></a>Villkor och slingor ändringar

I tidigare schemat har versioner, villkor och slingor parametrar som är associerade med en enda åtgärd. Det här schemat lyfter denna begränsning så villkor och slingor visas nu som Åtgärdstyper. Lär dig mer om [slingor och scope](../logic-apps/logic-apps-loops-and-scopes.md), eller läsa det här enkla exemplet för en åtgärd som villkoret:

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>
## <a name="runafter-property"></a>Egenskapen 'runAfter'

Hej `runAfter` egenskapen ersätter `dependsOn`, ger högre precision när du anger hello kör ordning för åtgärder baserat på hello status för tidigare åtgärder.

Hej `dependsOn` egenskapen var synonym med ”hello åtgärd kördes och lyckades”, oavsett hur många gånger du vill ha tooexecute en åtgärd, baserat på om hello föregående åtgärd har slutförts, misslyckades eller hoppas över. Hej `runAfter` egenskapen ger den flexibiliteten som ett objekt som anger alla hello åtgärdsnamn efter vilken hello objektet körs. Den här egenskapen definierar också en matris med statuslägen som är godkända som utlösare. Till exempel om du vill toorun efter steget A slutförs och även efter steg B lyckas eller misslyckas kan du skapa det här `runAfter` egenskapen:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>Uppgradera schemat

Uppgradera toohello tar nya schemat bara några steg. hello uppgraderingsprocessen innehåller kör hello uppgraderingsskript, Spara som en ny logikapp och du kan eventuellt att skriva över hello tidigare logikapp.

1. Öppna logikappen i hello Azure-portalen.

2. Gå för**översikt**. Välj hello logik app verktygsfältet **uppdatera Schema**.
   
    ![Välj Uppdatera Schema][1]
   
    hello returneras uppgraderade definition, som du kan kopiera och klistra in i en resursdefinition om det behövs. 
    Men vi **rekommenderar** du väljer **Spara som** toomake till att alla referenser för anslutningen är giltiga i hello uppgraderas logikapp.

3. I hello uppgradera bladet i verktygsfältet väljer **Spara som**.

4. Ange hello logik namn och status. toodeploy logikappen uppgraderade väljer **skapa**.

5. Bekräfta att logikappen uppgraderade fungerar som förväntat.
   
   > [!NOTE]
   > Om du använder en manuell eller begäran utlösare ändras hello återanrop URL i den nya logikappen. Test hello nya URL: en toomake att hello slutpunkt till slutpunkt-upplevelse fungerar. toopreserve tidigare URL: er, kan du klona via din befintliga logikapp.

6. *Valfria* toooverwrite logikappen tidigare med hello ny schemaversion hello i verktygsfältet väljer **klona**, nästa för**uppdatera Schema**. Det här steget är nödvändigt endast om du vill tookeep hello samma resurs-ID eller begäran utlösaren URL för din logikapp.

### <a name="upgrade-tool-notes"></a>Uppgradering av verktyget

#### <a name="mapping-conditions"></a>Mappningsvillkoren

I hello uppgraderas definition är hello-verktyget bästa prestanda på Gruppera true och false gren åtgärder som omfattning. Mer specifikt hello designer mönstret för `@equals(actions('a').status, 'Skipped')` ska visas som en `else` åtgärd. Om hello upptäcks inte kan tolkas mönster, kan hello verktyget Skapa olika villkor för både hello true och hello falsk förgrening. Du kan mappa om åtgärder efter uppgraderingen, om det behövs.

#### <a name="foreach-loop-with-condition"></a>'foreach-loop med villkoret

I nya hello-schemat kan du använda hello åtgärd tooreplicate hello filtermönstret av en `foreach` med ett villkor per artikel, men den här ändringen ska ske automatiskt när du uppgraderar. hello villkor blir en filteråtgärd innan hello foreach-slinga för att returnera en matris av objekt som matchar villkoret hello och att matris skickades till hello foreach-åtgärd. Ett exempel finns [slingor och scope](../logic-apps/logic-apps-loops-and-scopes.md).

#### <a name="resource-tags"></a>Resurstaggar

När du har uppgraderat tas resurstaggar bort, så måste du återställa dem för hello uppgraderas arbetsflödet.

## <a name="other-changes"></a>Andra ändringar

### <a name="renamed-manual-trigger-toorequest-trigger"></a>Byta namn på ”manuell” utlösaren too'request-utlösare

Hej `manual` utlösartypen var föråldrad och har fått nytt namn för`request` med typen `http`. Den här ändringen skapar mer konsekvens för hello typ av mönster som hello utlösare är används toobuild.

### <a name="new-filter-action"></a>Ny ”filter”-åtgärd

toofilter en stor matris ned tooa mindre uppsättning objekt, hello nya `filter` accepterar en matris och ett villkor, utvärderar hello villkor för varje objekt och returnerar en matris med objekt som uppfyller villkoret hello.

### <a name="restrictions-for-foreach-and-until-actions"></a>Begränsningar för 'foreach ”och” till ”-åtgärder

Hej `foreach` och `until` loop är begränsad tooa enda åtgärd.

### <a name="new-trackedproperties-for-actions"></a>Ny 'trackedProperties' för åtgärder

Åtgärder kan nu använda en annan egenskap som kallas `trackedProperties`, vilket är samma nivå toohello `runAfter` och `type` egenskaper. Det här objektet anger vissa åtgärd indata eller utdata som du vill tooinclude i hello Azure diagnostisk telemetri orsakat som en del av ett arbetsflöde. Exempel:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Nästa steg
* [Skapa arbetsflödesdefinitioner för logic apps](../logic-apps/logic-apps-author-definitions.md)
* [Skapa mallar för distribution för logic apps](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
