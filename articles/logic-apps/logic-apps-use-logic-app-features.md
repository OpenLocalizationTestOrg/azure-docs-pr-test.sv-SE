---
title: "aaaAdd villkor och starta arbetsflöden - Azure Logic Apps | Microsoft Docs"
description: "Styra hur arbetsflöden körs i Azure Logic Apps genom att lägga till införa villkorslogik, utlösare, åtgärder och parametrar."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 76d5e44590ffa14cf70d7a93b99a241d286d555b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-logic-apps-features"></a>Använda funktioner i Logic Apps

I en [föregående avsnitt](../logic-apps/logic-apps-create-a-logic-app.md), du har skapat din första logikapp. toocontrol din logikapp arbetsflöde, du kan ange olika sökvägar för dina logic app toorun och hur för bearbetar data i matriser, samlingar och batchar. Du kan inkludera dessa element i arbetsflödet logik app:

* Villkor och [växla instruktioner](../logic-apps/logic-apps-switch-case.md) låta din logikapp som kör olika åtgärder baserat på om särskilda villkor uppfylls.

* [Slingor](../logic-apps/logic-apps-loops-and-scopes.md) låta din logikapp köra steg upprepade gånger. T.ex, du kan upprepa åtgärder via en matris när du använder en **For_each** loop. Du kan upprepa åtgärder förrän villkor när du använder en **tills** loop.

* [Scope](../logic-apps/logic-apps-loops-and-scopes.md) kan du gruppera flera instruktioner, till exempel tooimplement undantagshantering.

* [Debatching](../logic-apps/logic-apps-loops-and-scopes.md) kan logikappen startar separat arbetsflöden för objekt i en matris när du använder hello **SplitOn** kommando.

Det här avsnittet beskriver andra begrepp för att skapa din logikapp:

* Code visa tooedit en befintlig logikapp
* Alternativ för att starta ett arbetsflöde

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Villkor: Köra steg efter uppfyller ett villkor

toohave logikappen steg endast körs när data uppfyller specifika villkor, du kan lägga till ett villkor som jämför data i hello arbetsflöde mot specifika fält eller värden.

Anta att du har en logikapp som skickar för många e-postmeddelanden för inlägg på en webbplats RSS-feed. Du kan lägga till ett villkor så att din logikapp skickar e-postmeddelande endast när nya hello bokför tillhör tooa specifik kategori.

1. I hello [Azure-portalen](https://portal.azure.com), hitta och öppna logikappen i logik App Designer.

2. Lägg till ett villkor toohello arbetsflöde plats som du vill. 

   tooadd hello villkoret mellan befintliga stegen i hello logik app arbetsflödet pekaren hello över hello pilen där du vill att tooadd hello villkor. 
   Välj hello **plustecknet** (**+**), Välj **Lägg till ett villkor**. Exempel:

   ![Lägg till villkor toologic app](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Om du vill tooadd ett villkor hello slutet av arbetsflödet aktuella gå toohello längst ned i din logikapp och välj **+ nytt steg**.

3. Nu ska du ange hello villkor. Ange hello fält i datakällan som du vill tooevaluate, hello åtgärden tooperform och hello målvärdet eller fältet. tooadd befintliga fält tooyour tillstånd, Välj hello **Lägg till dynamisk innehållslistan**.

   Exempel:

   ![Redigera villkor i grundläggande läge](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Här är hello fullständig villkor:

   ![Slutförd](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > toodefine hello villkor i koden, Välj **redigera i Avancerat läge**. Exempel:
   > 
   > ![Redigera villkor i koden](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. Under **Ja om** och **om Nej**, lägga till hello steg tooperform baserat på om hello villkor uppfylls.

   Exempel:

   ![Villkor med Ja och inga sökvägar](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Du kan dra befintliga åtgärder till hello **Ja om** och **om Nej** sökvägar.

5. När du är klar sparar du din logikapp.

Nu får du e-postmeddelanden endast när hello inlägg uppfyller villkoret.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Upprepa åtgärder via en lista med forEach

hello forEach loop anger en matris toorepeat åtgärden över. Hello flödet misslyckas om den inte är en matris. Om du har action1 som en matris med meddelanden och du vill toosend varje meddelande, inkludera du den här forEach satsen i hello egenskaper för åtgärden:`forEach : "@action('action1').outputs.messages"`

## <a name="edit-hello-code-definition-for-a-logic-app"></a>Redigera hello koddefinitionen för en logikapp

Även om du har hello logik App Designer kan redigera du hello-kod som definierar en logikapp direkt.

1. Välj på hello kommandofältet **vyn kod**.

    En fullständig redigerare öppnas och visar hello definition av du redigerade.

    ![Kodvy](media/logic-apps-use-logic-app-features/codeview.png)

    I hello textredigerare som du kan kopiera och klistra in valfritt antal åtgärder i hello samma logikapp eller mellan logikappar. 
    Du kan också enkelt lägga till eller ta bort hela avsnitt från hello definition och du kan också dela definitioner med andra.

2. toosave ändringarna, Välj **spara**.

## <a name="parameters"></a>Parametrar

Vissa funktioner i Logic Apps är endast tillgängliga i kodvy, till exempel parametrar. Parametrar som gör det enkelt tooreuse värden i hela din logikapp. Om du har en e-postadress som du vill använda i flera åtgärder, bör du till exempel definiera den e-postadressen som en parameter.

Parametrarna är bra för att dra ut värden att du är mycket troligt toochange. De är särskilt användbar när du behöver toooverride parametrar i olika miljöer. hur toooverride parametrar baserat på miljön, se toolearn [redigera logik app definitioner](../logic-apps/logic-apps-author-definitions.md) och [REST API-dokumentation](https://docs.microsoft.com/rest/api/logic).

Det här exemplet illustrerar hur tooupdate logikappen befintliga så att du kan använda parametrarna för hello frågeterm.

1. I kodvyn, hitta hello `parameters : {}` objekt och lägger till en `currentFeedUrl` objekt:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Gå toohello `When_a_feed-item_is_published` åtgärd, hitta hello `queries` avsnittet och Ersätt hello läsa värde med:`"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    toojoin två eller flera strängar, du kan också använda hello `concat` funktion. 
    Till exempel `"@concat('#',parameters('currentFeedUrl'))"` fungerar hello samma som hello ovan.

3.  När du är klar väljer **spara**. 

    Nu kan du ändra hello webbplats RSS-feed genom att ange en annan URL via hello `currentFeedURL` objekt.

Lär dig mer om [hur tooauthor logik app definitioner](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Starta logik app arbetsflöden

Har du olika alternativ för att starta hello arbetsflöde som definierats i din logikapp. Du kan alltid starta ett arbetsflöde på begäran i hello [Azure-portalen].

### <a name="recurrence-triggers"></a>Utlösare för upprepning

En utlösare för upprepning körs vid ett intervall som du anger. När hello utlösaren har införa villkorslogik, avgör hello utlösaren om hello arbetsflödet måste toorun. En utlösare Anger hello arbetsflödet ska köras genom att returnera en `200` statuskod. När hello arbetsflödet inte behöver toorun, hello utlösaren returnerar en `202` statuskod.

### <a name="callback-using-rest-apis"></a>Återanrop med hjälp av REST API: er

toostart ett arbetsflöde, tjänster kan anropa en logik app slutpunkt. den här typen av logik app på begäran, väljer du toostart **kör nu** i hello kommandofält. Se [starta arbetsflöden genom att anropa logik app slutpunkter som utlösare](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[Azure-portalen]: https://portal.azure.com

## <a name="next-steps"></a>Nästa steg

* [Växeln-instruktioner](../logic-apps/logic-apps-switch-case.md) 
* [Loopar, omfattningar och debatching](../logic-apps/logic-apps-loops-and-scopes.md)
* [Skapa Logic App-definitioner](../logic-apps/logic-apps-author-definitions.md)