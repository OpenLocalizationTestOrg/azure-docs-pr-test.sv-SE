---
title: "aaaCall, utlösa eller kapsla arbetsflöden med HTTP-slutpunkter – Azure Logic Apps | Microsoft Docs"
description: "Konfigurera HTTP-slutpunkter toocall, utlösare eller kapslade arbetsflöden för Azure Logic Apps"
services: logic-apps
keywords: "arbetsflöden, HTTP-slutpunkter"
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 072a314c3bff75ab7696f86bb063bb7c03c4ae89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-logic-apps"></a>Anropa utlösare eller kapsla arbetsflöden med HTTP-slutpunkter i logikappar

Du kan internt exponera synkron HTTP-slutpunkter som utlösare för logic apps så att du kan utlösa eller anropa dina logic apps via en annan URL. Du kan också kapsla arbetsflöden i dina logic apps med hjälp av ett mönster för callable slutpunkter.

toocreate http-slutpunkter kan du lägga till dessa utlösare så att dina logic apps kan ta emot inkommande begäranden:

* [Förfrågan](../connectors/connectors-native-reqres.md)

* [API-anslutning Webhooken](logic-apps-workflow-actions-triggers.md#api-connection-trigger)

* [HTTP-webhook](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > Även om våra exempel använder hello **begära** utlösare, kan du använda någon av hello visas http-utlösare och alla principer identiskt gäller toohello andra typer av utlösare.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>Ställ in en HTTP-slutpunkt för din logikapp

toocreate http-slutpunkt, lägga till en utlösare som kan ta emot inkommande begäranden.

1. Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen"). Gå tooyour logikapp och öppna logik App Designer.

2. Lägg till en utlösare som gör att din logikapp ta emot inkommande begäranden. Lägg exempelvis till hello **begära** utlösaren tooyour logikapp.

3.  Under **begära brödtext JSON-Schema**, du kan även ange en JSON-schema för hello-nyttolasten (data) som du förväntar dig hello utlösaren tooreceive.

    hello-designer använder det här schemat för att generera token att din logikapp kan använda tooconsume parse och skicka data från hello utlösare genom arbetsflödet. 
    Mer om [token genereras från JSON-scheman](#generated-tokens).

    Ange hello schemat visas i hello designer i det här exemplet:

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![Lägg till hello begäran åtgärd][1]

    > [!TIP]
    > 
    > Du kan skapa ett schema för en JSON-nyttolast med exempel från ett verktyg som [jsonschema.net](http://jsonschema.net/), eller i hello **begära** utlösaren genom att välja **Använd exemplet nyttolast toogenerate schemat**. 
    > Ange ditt exempel nyttolasten och välj **klar**.

    Till exempel det här exemplet nyttolast:

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    genererar det här schemat:

    ```json
    }
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  Spara din logikapp. Under **HTTP POST toothis URL**, nu bör du hitta en URL som genererade motringning som det här exemplet:

    ![Genererade återanrop URL för slutpunkten](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Denna URL innehåller en delad signatur åtkomst (SAS)-nyckel i hello frågeparametrar som används för autentisering. 
    Du kan också få hello HTTP slutpunkts-URL från din app översikt för logiken i hello Azure-portalen. Under **utlösaren historik**, Välj din utlösare:

    ![Hämta HTTP slutpunkts-URL från Azure-portalen][2]

    Eller du kan hämta hello-URL genom att göra anropet:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-hello-http-method-for-your-trigger"></a>Ändra hello HTTP-metoden för utlösaren

Som standard hello **begäran** utlösaren förväntar sig en HTTP POST-begäran, men du kan använda en annan HTTP-metod. 

> [!NOTE]
> Du kan ange endast en metod.

1. På din **begära** utlösa, Välj **visa avancerade alternativ**.

2. Öppna hello **metoden** lista. Det här exemplet väljer du **hämta** så att du kan testa din HTTP slutpunkts-URL senare.

    > [!NOTE]
    > Du kan välja andra HTTP-metoden eller ange en anpassad metod för egna logikapp.

    ![Ändra HTTP-metoden](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>Acceptera parametrar via HTTP-slutpunkt-URL

När du vill att din HTTP-slutpunkt URL tooaccept-parametrar kan du anpassa din utlösaren relativ sökväg.

1. På din **begära** utlösa, Välj **visa avancerade alternativ**. 

2. Under **metoden**, ange hello HTTP-metod som du vill att din begäran toouse. Välj det här exemplet hello **hämta** metod om du inte redan har gjort så att du kan testa din HTTP slutpunkts-URL.

      > [!NOTE]
      > Du måste också explicit ange HTTP-metoden för utlösaren när du anger en relativ sökväg för utlösaren.

3. Under **relativa sökvägen**, ange hello relativ sökväg för hello-parameter som URL: en ska acceptera, till exempel `customers/{customerID}`.

    ![Ange hello HTTP-metoden och relativ sökväg för parametern](./media/logic-apps-http-endpoint/relativeurl.png)

4. toouse hello parametern, lägga till en **svar** åtgärd tooyour logikapp. (Välj under din utlösaren **nytt steg** > **lägga till en åtgärd** > **svar**) 

5. I ditt svar **brödtext**, inkludera hello token för hello-parameter som du angav i ditt utlösaren relativ sökväg.

    Till exempel tooreturn `Hello {customerID}`, uppdatera ditt svar **brödtext** med `Hello {customerID token}`. 
    hello dynamisk innehåll lista bör visas och visa hello `customerID` token för du tooselect.

    ![Lägg till parametern tooresponse brödtext](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    Din **brödtext** bör se ut som i det här exemplet:

    ![Svarstexten med parametern](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Spara din logikapp. 

    HTTP-slutpunkt-URL innehåller nu hello relativ sökväg, till exempel: 

    https & # 58;//prod-00.southcentralus.logic.azure.com/workflows/f90cb66c52ea4e9cabe0abf4e197deff/triggers/manual/paths/invoke/customers/{customerID}...

7. tootest din HTTP-slutpunkt, kopiera och klistra in hello uppdaterade URL: en till ett nytt webbläsarfönster, men Ersätt `{customerID}` med `123456`, och tryck på RETUR.

    Webbläsaren bör visa den här texten: 

    `Hello 123456`

<a name="generated-tokens"></a>
### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>Token genereras från JSON-scheman för din logikapp

När du anger ett JSON-schema i din **begära** Utlös, hello logik App Designer genererar token för egenskaper i schemat. Du kan sedan använda dessa token för att skicka data via logik app arbetsflödet.

Det här exemplet, om du lägger till hello `title` och `name` egenskaper tooyour JSON-schema, deras token är nu tillgängliga toouse i senare arbetsflödessteg. 

Här är hello fullständigt JSON-schema:

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a>Skapa inkapslade arbetsflöden för logic apps

Du kan kapsla arbetsflöden i din logikapp genom att lägga till andra logikappar som kan ta emot begäranden. tooinclude dessa logikappar lägga till hello **Logikappar i Azure - Välj ett arbetsflöde för Logic Apps** åtgärd tooyour utlösare. Sedan kan du välja från berättigade logic apps.

![Lägg till en annan logikapp](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>Anropa eller utlösare logikappar via HTTP-slutpunkter

När du har skapat HTTP-slutpunkten kan du utlösa logikappen via en `POST` metoden toohello fullständiga URL: en. Logikappar har inbyggt stöd för direkt åtkomst slutpunkter.

## <a name="reference-content-from-an-incoming-request"></a>Referensinnehåll från en inkommande begäran

Om hello innehåll typ är `application/json`, kan du referera egenskaper från hello inkommande begäran. Annars behandlas innehåll som en binär enhet som du kan skicka tooother API: er. tooreference det här innehållet i hello arbetsflödet, måste du konvertera innehållet. Om du skickar till exempel `application/xml` innehåll, kan du använda `@xpath()` för ett XPath-extrahering eller `@json()` för konvertering av XML-tooJSON. Lär dig mer om [arbeta med innehållstyper](../logic-apps/logic-apps-content-type.md).

tooget hello utdata från en inkommande begäran, kan du använda hello `@triggerOutputs()` funktion. hello utdata kan se ut det här exemplet:

```json
{
    "headers": {
        "content-type" : "application/json"
    },
    "body": {
        "myProperty" : "property value"
    }
}
```

tooaccess hello `body` egenskapen mer specifikt kan du använda hello `@triggerBody()` genväg. 

## <a name="respond-toorequests"></a>Svara toorequests

Du kanske vill toorespond toocertain begäranden som startar en logikapp genom att returnera innehållet toohello anroparen. tooconstruct hello statuskoden rubrik och brödtext för ditt svar, du kan använda hello **svar** åtgärd. Den här åtgärden kan finnas var som helst i din logikapp, inte bara i hello slutet av arbetsflödet.

> [!NOTE] 
> Om din logikapp inte innehåller en **svar**, hello HTTP-ändpunkten svarar *omedelbart* med en **202 godkända** status. Dessutom för hello ursprungliga tooget hello svar på begäranden, även alla steg som krävs för hello svar måste slutföras inom hello [tidsgränsen för begäran](./logic-apps-limits-and-config.md) såvida inte du anropa hello arbetsflödet som en kapslad logikapp. Om inget svar inträffar inom denna inkommande begäran om hello timeout och tar emot hello HTTP-svar **408 klientens**. För kapslade logic apps fortsätter hello överordnade logikapp toowait svar tills slutförts, oavsett hur lång tid som krävs.

### <a name="construct-hello-response"></a>Skapa hello svar

Du kan inkludera mer än ett sidhuvud och varje typ av innehåll i hello svarstexten. I vårt exempelsvar hello huvudet anger att hello svar har innehållstyp `application/json`. och hello brödtext innehåller `title` och `name`, baserat på hello JSON-schemat uppdateras tidigare för hello **begära** utlösare.

![Åtgärd för HTTP-svar][3]

Svar har följande egenskaper:

| Egenskap | Beskrivning |
| --- | --- |
| statusCode |Anger hello HTTP-statuskoden för svarande toohello inkommande begäran. Den här koden kan vara en giltig statuskod som börjar med 2xx, 4xx eller 5xx. 3xx statuskoder är inte tillåtna. |
| Rubriker |Definierar valfritt antal huvuden tooinclude hello svar. |
| Brödtext |Anger ett body-objekt som kan vara en sträng, en JSON-objekt eller även binära innehåll som refereras från föregående steg. |

Här är vad hello JSON-schema som ser ut som nu för hello **svar** åtgärd:

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> tooview hello fullständig JSON-definitionen för din logikapp på hello logik App Designer väljer **vyn kod**.

## <a name="q--a"></a>Frågor och svar

#### <a name="q-what-about-url-security"></a>F: Vad händer om URL-säkerhet?

S: azure genererar på ett säkert sätt logik app återanrop URL: er med hjälp av en delad signatur åtkomst (SAS). Den här signaturen passerar som en frågeparameter och måste verifieras innan din logikapp kan utlösas. Azure genererar hello signatur med hjälp av en unik kombination av en hemlig nyckel per logikapp, hello Utlösarnamn och hello-åtgärden som utförs. Så att de inte kan skapa en giltig signatur såvida inte någon har appen åtkomstnyckel toohello hemliga logik.

   > [!IMPORTANT]
   > Produktions- och säkra system rekommenderar vi mot anropa logikappen direkt från hello webbläsare eftersom:
   > 
   > * hello delade åtkomstnyckeln visas i hello-URL.
   > * Du kan inte hantera säker innehåll principer på grund av tooshared domäner över Logikapp kunder.

#### <a name="q-can-i-configure-http-endpoints-further"></a>F: kan jag konfigurera HTTP-slutpunkter ytterligare?

S: Ja, HTTP-slutpunkter stöd för mer avancerad konfiguration via [ **API Management**](../api-management/api-management-key-concepts.md). Den här tjänsten erbjuder också hello kapaciteten för du tooconsistently hantera alla dina API: er, inklusive logikappar, konfigurera anpassade domännamn, använda flera autentiseringsmetoder och mycket mer, till exempel:

* [Ändra hello begäran metod](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [Ändra hello URL-segment i hello-begäran](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* Ställ in din API Management-domäner i hello [Azure-portalen](https://portal.azure.com/ "Azure-portalen")
* Ställa in principen toocheck för grundläggande autentisering

#### <a name="q-what-changed-when-hello-schema-migrated-from-hello-december-1-2014-preview"></a>F: vad ändrats när hello schemat migreras från hello 1 December 2014 preview?

S: här är en sammanfattning om dessa ändringar:

| 1 december 2014 preview | Den 1 juni 2016 |
| --- | --- |
| Klicka på **HTTP-lyssnaren** API-App |Klicka på **manuell utlösaren** (ingen API-App krävs) |
| HTTP-lyssnaren inställningen ”*skickar svar automatiskt*” |Innehåller en **svar** åtgärd eller inte i Arbetsflödesdefinitionen för hello |
| Konfigurera grundläggande eller OAuth-autentisering |via API-hantering |
| Konfigurera HTTP-metoden |Under **visa avancerade alternativ**, Välj en HTTP-metod |
| Konfigurera relativ sökväg |Under **visa avancerade alternativ**, lägga till en relativ sökväg |
| Referens hello inkommande brödtext via`@triggerOutputs().body.Content` |Referens till`@triggerOutputs().body` |
| **Skicka HTTP-svar** åtgärd på hello HTTP-lyssnare |Klicka på **svara tooHTTP begäran** (ingen API-App krävs) |

## <a name="get-help"></a>Få hjälp

tooask frågor besvara frågor och lär dig vilka andra Azure Logikappar användarna gör besök hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nästa steg

* [Skapa Logic App-definitioner](./logic-apps-author-definitions.md)
* [Hantera fel och undantag](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png
