---
title: 'aaaCreate web API: er och REST-API: er som kopplingar - Azure Logic Apps | Microsoft Docs'
description: "Skapa webb-API: er och REST API: er toocall din API: er, tjänster eller system i arbetsflöden för system-integration med Azure Logikappar"
keywords: "webb-API: er, REST API: er, kopplingar, arbetsflöden, system-integrering"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: bd229179-7199-4aab-bae0-1baf072c7659
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/26/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 2792204d1d298a6f810e75211c1789ee204c2fdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-custom-apis-as-connectors-for-logic-apps"></a>Skapa anpassade API: er som kopplingar för logic apps

Även om Azure Logikappar erbjuder [100 + inbyggda kopplingar](../connectors/apis-list.md) att du kan använda i logik app arbetsflöden, kanske du vill toocall API: er, system, och tjänster som inte är tillgängliga som kopplingar. Du kan skapa egna anpassade API: er som innehåller åtgärder och utlösare toouse i logikappar. Här följer andra orsaker till varför du kanske vill toocreate egna API: er för används som kopplingar i logikappar:

* Utöka det aktuella systemet integrering och data integration arbetsflöden.
* Hjälp kunder att använda din toomanage professional eller personliga uppgifter för tjänsten.
* Expandera hello reach, synlighet och användning för din tjänst.

I princip kopplingar är web API: er som använder REST för anslutningsbara gränssnitt [Swagger metadataformat](http://swagger.io/specification/) för dokumentation och JSON som exchange format. Eftersom kopplingar REST API: er som kommunicerar via HTTP-slutpunkter kan använda du alla språk som .NET, Java eller Node.js, för att skapa kopplingar. Du kan också vara värd för dina API: er på [Azure App Service](../app-service/app-service-value-prop-what-is.md), en plattform-som-en tjänst (PaaS) erbjudande som ger hello bästa enklaste och mest skalbara sätt för API-värd. 

För anpassade API: er toowork med logic apps, din API kan tillhandahålla [ *åtgärder* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) som utför olika uppgifter i logik app arbetsflöden. Din API kan också fungera som en [ *utlösaren* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) som startar en logik app arbetsflödet när nya data eller en händelse uppfyller ett angivet villkor. Det här avsnittet beskrivs vanliga mönster som du kan följa för att skapa åtgärder och utlösare i ditt API baserat på hello beteende som du vill att din API-tooprovide.

> [!TIP] 
> Även om du kan distribuera dina API: er som [webbappar](../app-service-web/app-service-web-overview.md), Överväg att distribuera dina API: er som [API apps](../app-service-api/app-service-api-apps-why-best-platform.md), som kan underlätta ditt jobb när du skapar, värd och använda API: er i hello molnet och lokalt. Du saknar toochange någon kod i dina API: er, distribuera bara din kod tooan API-app. Lär dig hur för [skapa API apps som skapats med ASP.NET](../app-service-api/app-service-api-dotnet-get-started.md), [Java](../app-service-api/app-service-api-java-api-app.md), eller [Node.js](../app-service-api/app-service-api-nodejs-api-app.md). 
>
> API-App prov inbyggda för logic apps finns hello [Azure Logic Apps GitHub-lagringsplatsen](http://github.com/logicappsio) eller [blogg](http://aka.ms/logicappsblog).

## <a name="helpful-tools"></a>Användbara verktyg

En anpassad API fungerar bäst med logic apps när hello API har också en [Swagger-dokument](http://swagger.io/specification/) som beskriver hello API: er åtgärder och parametrar.
Många bibliotek som [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle), kan generera hello Swagger-filen automatiskt åt dig. tooannotate hello Swagger-filen för visningsnamn, egenskapstyperna och så vidare, du kan också använda [TRex](https://github.com/nihaue/TRex) så att din Swagger-fil som fungerar bra med logic apps.

<a name="actions"></a>

## <a name="action-patterns"></a>Åtgärden mönster

Din anpassade API bör ge för logic apps tooperform uppgifter [ *åtgärder*](./logic-apps-what-are-logic-apps.md#logic-app-concepts). Varje åtgärd i ditt API mappar tooan åtgärd. En grundläggande åtgärd är en domänkontrollant som accepterar HTTP-begäranden och returnerar HTTP-svar. Exempelvis skickar en logikapp en HTTP-begäran tooyour webbapp eller API-app. Returnerar ett HTTP-svar om din app, tillsammans med innehåll som hello logik app kan bearbeta.

För en åtgärd som standard kan skriva en metod för HTTP-begäran i ditt API och beskriver den metoden i en Swagger-fil. Du kan sedan anropa din API direkt med en [HTTP-åtgärd](../connectors/connectors-native-http.md) eller en [HTTP + Swagger](../connectors/connectors-native-http-swagger.md) åtgärd. Som standard måste svar returneras inom hello [tidsgränsen för begäran](./logic-apps-limits-and-config.md). 

![Standard åtgärd mönster](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a>toomake en logikapp Vänta medan din API är klar aktiviteter som längre körs, din API kan följa hello [asynkron avsökning mönster](#async-pattern) eller hello [asynkron webhook mönster](#webhook-actions) beskrivs i det här avsnittet. Tänk dig hello processen för att begära en anpassad enkelt från en bageri för en liknelsen som hjälper dig att visualisera dessa mönster olika beteenden. hello avsökning mönster speglar hello beteende där du anropa hello bageri var 20 minuter toocheck om hello enkelt är klar. Hej webhook mönster speglar hello beteende där hello bagerier och frågar efter ditt telefonnummer så att de kan ringa dig när hello enkelt är klar.

Exempel, finns hello [Logic Apps GitHub-lagringsplatsen](https://github.com/logicappsio). Dessutom lär dig mer om [användningsmätning för åtgärder](logic-apps-pricing.md).

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-hello-polling-action-pattern"></a>Utför tidskrävande uppgifter med hello avsökning åtgärd mönster

toohave din API utföra uppgifter som kan köras längre än hello [tidsgränsen för begäran](./logic-apps-limits-and-config.md), du kan använda hello asynkron avsökning mönster. Det här mönstret har din API gör fungerar i en separat tråd men behålla en aktiv anslutning toohello Logic Apps-motor. På så sätt kan hello logikapp har ingen timeout eller Fortsätt med hello nästa steg i hello arbetsflöde innan din API slutar fungera.

Här är hello allmänna mönster:

1. Kontrollera att hello-motorn vet att din API godkänd hello begäran och igång.
2. När hello motorn gör efterföljande begäranden om jobbstatus, kan du hello motorn veta när din API är klar hello-aktivitet.
3. Returnera relevanta data toohello motorn så att hello logik app arbetsflödet kan fortsätta.

<a name="bakery-polling-action"></a>Nu använda hello tidigare bageri liknelsen toohello avsökning mönster och anta att du anropar en bageri och ordning anpassade enkelt för leverans. hello-processen för att göra hello enkelt tar tid och du vill inte toowait på hello telefon medan hello bageri fungerar på hello enkelt. hello bageri bekräftar din beställning och har du anropa var tjugonde minut hello enkelt status. Efter 20 minuter skickar måste du anropa hello bageri, men de talar om att din enkelt inte gjort och att du ska anropa i en annan 20 minuter. Fram och tillbaka flera gånger innan du anropar och hello bageri talar om att din order är klar och levererar ditt enkelt. 

Vi mappa tillbaka den här avsökningen mönster. hello bageri representerar din anpassade API medan hello enkelt kunder representerar hello Logic Apps-motorn. När hello motorn anropar din API med en begäran, din API bekräftar hello begäran och svarar med hello tidsintervall när hello-motorn kan kontrollera jobbstatus. hello motorn fortsätter att kontrollera jobbstatus tills din API svarar hello jobbet är klart och returnerar data tooyour logikapp, som sedan fortsätter arbetsflödet. 

![Avsökningen åtgärd mönster](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

Här följer hello särskilda åtgärder för din API-toofollow beskrivs ur hello API: er:

1. När kommer din API en HTTP-begäran toostart arbete, omedelbart returnera ett HTTP `202 ACCEPTED` svar med hello `location` huvud som beskrivs senare i det här steget. Svaret kan hello Logic Apps motorn vet att din API fick hello begäran godkända hello nyttolasten i begäran (indata) och bearbetar nu. 
   
   Hej `202 ACCEPTED` svar ska inkludera dessa huvuden:
   
   * *Krävs*: A `location` huvud som anger hello absolut sökväg tooa URL där hello Logic Apps motorn kan kontrollera status för ditt API

   * *Valfria*: A `retry-after` huvud som anger hello antal sekunder som hello motorn ska vänta innan hello `location` URL: en för jobbstatus. 

     Som standard kontrollerar hello motorn var 20 sekunder. toospecify ett annat intervall inkluderar hello `retry-after` sidhuvud och hello antalet sekunder innan nästa hello-avsökning.

2. När hello anges tiden går hello Logic Apps motorn avsökningar hello `location` URL toocheck jobbstatus. Din API ska utföra dessa kontroller och returnera dessa svar:
   
   * Om hello jobbet är klart returnerar ett HTTP `200 OK` svar, tillsammans med hello svar nyttolasten (indata till nästa steg i hello).

   * Om jobbet hello fortfarande bearbetar returnera ett annat HTTP `202 ACCEPTED` svar, men med hello samma sidhuvuden som hello ursprungliga svar.

När din API följer detta mönster, har du inte toodo något i hello logik app arbetsflödet definition toocontinue Kontrollera jobbstatus. När hello motorn hämtar HTTP `202 ACCEPTED` svar och en giltig `location` huvud, hello motorn värnar hello asynkront mönster och kontrollerar hello `location` sidhuvud tills din API returnerar ett icke-202 svar.

> [!TIP]
> Ett exempel asynkront mönster granska [asynkron controller svar exemplet i GitHub](https://github.com/logicappsio/LogicAppsAsyncResponseSample).

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-hello-webhook-action-pattern"></a>Utför tidskrävande uppgifter med hello webhook åtgärd mönster

Alternativt kan använda du hello webhook mönster för asynkron bearbetning och tidskrävande uppgifter. Det här mönstret har hello logikapp stanna och vänta på ett ”återanrop” från din API-toofinish bearbetning innan du fortsätter arbetsflödet. Den här återanropet är en HTTP POST som skickar ett meddelande tooa URL när en händelse inträffar. 

<a name="bakery-webhook-action"></a>Nu använda hello tidigare bageri liknelsen toohello webhook mönster och anta att du anropar en bageri och ordning anpassade enkelt för leverans. hello-processen för att göra hello enkelt tar tid och du vill inte toowait på hello telefon medan hello bageri fungerar på hello enkelt. hello bageri bekräftar din order, men den här tiden kan du ge dem ditt telefonnummer så att de kan ringa dig när hello enkelt är klar. Den här gången anger hello bageri när din order är klar och levererar ditt enkelt.

När vi mappa det här mönstret för webhook tillbaka representerar hello bageri din anpassade API när du hello enkelt kund, representerar hello Logic Apps-motorn. hello motorn din API: n med en begäran och inkluderar en ”callback” Webbadress.
När hello jobbet är klart din API använder hello URL toonotify hello motor och returnera data tooyour logikapp, som sedan fortsätter arbetsflödet. 

Konfigurera två slutpunkter på styrenheten för det här mönstret: `subscribe` och`unsubscribe`

*  `subscribe`slutpunkten: när körningen når din API-åtgärd i hello arbetsflöde, hello Logic Apps motorn anrop hello `subscribe` slutpunkt. Det här steget gör hello logik app toocreate en motringning URL som din API som lagrar och vänta sedan hello motringning från din API när arbetet är klar. Din API och sedan anropar tillbaka med en HTTP POST toohello URL och vidarebefordrar alla returnerade innehåll och rubriker som inkommande toohello logikapp.

* `unsubscribe`slutpunkten: om hello logikapp kör avbryts hello Logic Apps motorn anrop hello `unsubscribe` slutpunkt. Din API kan sedan avregistrera hello återanrop URL och stoppa alla processer som krävs.

![Webhook åtgärd mönster](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> Hello logik App Designer stöder för närvarande inte identifierande webhook slutpunkterna via Swagger. Så det här mönstret du har tooadd en [ **Webhook** åtgärd](../connectors/connectors-native-webhook.md) och ange hello URL, rubriker och brödtext för din begäran. Se även [arbetsflödesåtgärder och utlösare](logic-apps-workflow-actions-triggers.md#api-connection-webhook-action). toopass i hello återanrop URL som du kan använda hello `@listCallbackUrl()` arbetsflödesfunktion i något av hello tidigare fälten efter behov.

> [!TIP]
> Ett exempel webhook-mönster, granska [webhook utlösaren exemplet i GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Utlösaren mönster

Anpassat API som kan fungera som en [ *utlösaren* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) som startar en logikapp när nya data eller en händelse uppfyller ett angivet villkor. Den här utlösaren kan antingen Kontrollera regelbundet, eller vänta och lyssna efter nya data eller händelser på tjänsteslutpunkt. Om nya data eller en händelse uppfyller hello angivet villkor, hello utlösare utlöses och startar hello logikappen som lyssnar toothat utlösare. toostart logic apps sätt din API kan följa hello [ *avsökning utlösaren* ](#polling-triggers) eller hello [ *webhook utlösaren* ](#webhook-triggers) mönster. Dessa mönster är liknande tootheir motsvarigheter för [avsökning åtgärder](#async-pattern) och [webhook-åtgärder](#webhook-actions). Dessutom lär dig mer om [användningsmätning för utlösare](logic-apps-pricing.md).

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-hello-polling-trigger-pattern"></a>Sök regelbundet efter nya data eller händelser med hello avsökning utlösaren mönster

En *avsökning utlösaren* fungerar ungefär som hello [avsökning åtgärd](#async-pattern) beskrivs tidigare i det här avsnittet. hello Logic Apps motorn anropar regelbundet och kontrollerar hello utlösaren slutpunkt för nya data eller händelser. Om motorn hello söker efter nya data eller en händelse som uppfyller dina villkor, utlöses hello utlösare. Sedan skapas hello engine logik app-instansen, som bearbetar hello data som indata. 

![Avsökning utlösaren mönster](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Varje avsökning begäran räknas som en körning av åtgärden, även när inga logik app-instansen har skapats. tooprevent bearbetning hello samma information flera gånger, utlösaren ska rensa data som redan har lästs in och skickats toohello logikapp.

Här följer specifika steg för en avsökning utlösare som beskrivs ur hello API: er:

| Hitta nya data eller händelsen?  | API-svar | 
| ------------------------- | ------------ |
| Hitta | Returnera ett HTTP `200 OK` status med hello svar nyttolast (indata för nästa steg). <br/>Det här svaret skapar en logik app-instansen och startar hello arbetsflöde. |
| Det gick inte att hitta | Returnera ett HTTP `202 ACCEPTED` status med en `location` rubrik och en `retry-after` huvud. <br/>Utlösare, hello `location` huvud måste även innehålla en `triggerState` frågeparameter som vanligtvis är ”tidsstämpel”. Din API kan använda den här identifieraren tootrack hello senast hello logik app utlöstes. |

Till exempel tooperiodically Kontrollera din tjänst för nya filer, kan du skapa en avsökning utlösare som har dessa beteenden:

| Begäran innehåller `triggerState`? | API-svar |
| -------------------------------- | -------------|
| Nej | Returnera ett HTTP `202 ACCEPTED` status plus en `location` huvud med `triggerState` ange toohello aktuell tid och hello `retry-after` too15 intervall i sekunder. |
| Ja | Kontrollera din tjänst för filer som lagts till efter hello `DateTime` för `triggerState`. |

| Antal filer hittades | API-svar |
| --------------------- | -------------|
| Enskild fil | Returnera ett HTTP `200 OK` status och hello innehåll nyttolast, uppdatering `triggerState` toohello `DateTime` för hello returnerade att filen och ange `retry-after` too15 intervall i sekunder. |
| Flera filer | Returnera en fil i taget och en HTTP- `200 OK` status, uppdatera `triggerState`, och ange hello `retry-after` too0 intervall i sekunder. </br>De här stegen kan hello motorn vet att flera data är tillgängliga, och som hello-motorn omedelbart begära hello data från hello URL i hello `location` huvud. |
| Inga filer | Returnera ett HTTP `202 ACCEPTED` status, ändras inte `triggerState`, och ange hello `retry-after` too15 intervall i sekunder. |

> [!TIP]
> Ett exempel avsökning utlösaren mönster, granska detta [avsökning utlösaren controller exemplet i GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs).

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-hello-webhook-trigger-pattern"></a>Vänta och lyssna efter nya data eller händelser med hello webhook utlösaren mönster

En webhook-utlösare är en *push-utlösare* som väntar och lyssnar efter nya data eller händelser på tjänsteslutpunkt. Om nya data eller en händelse uppfyller angivna villkor, hello utlösare utlöses hello och skapar en logik app-instansen, som bearbetar hello data som indata.
Webhook-utlösare fungerar ungefär som hello [webhook-åtgärder](#webhook-actions) tidigare i det här avsnittet och ställs in med `subscribe` och `unsubscribe` slutpunkter. 

* `subscribe`slutpunkten: när du lägger till och spara en webhook-utlösare i din logikapp hello Logic Apps motorn anrop hello `subscribe` slutpunkt. Det här steget gör hello logik app toocreate en motringning URL som lagras av din API. När det finns nya data eller en händelse som uppfyller hello villkoret din API-anrop tillbaka med en HTTP POST toohello URL. hello innehåll nyttolasten och rubriker överföra som inkommande toohello logikapp.

* `unsubscribe`slutpunkten: om hello webhook utlösare eller hela logikapp raderas hello Logic Apps motorn anrop hello `unsubscribe` slutpunkt. Din API kan sedan avregistrera hello återanrop URL och stoppa alla processer som krävs.

![Webhook-utlösare mönster](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> Hello logik App Designer stöder för närvarande inte identifierande webhook slutpunkterna via Swagger. Så det här mönstret du har tooadd en [ **Webhook** utlösaren](../connectors/connectors-native-webhook.md) och ange hello URL, rubriker och brödtext för din begäran. Se även [HTTPWebhook utlösaren](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). toopass i hello återanrop URL som du kan använda hello `@listCallbackUrl()` arbetsflödesfunktion i något av hello tidigare fälten efter behov.
>
> tooprevent bearbetning hello samma information flera gånger, utlösaren ska rensa data som redan har lästs in och skickats toohello logikapp.

> [!TIP]
> Ett exempel webhook-mönster, granska [webhook utlösaren controller exemplet i GitHub](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs).

## <a name="deploy-call-and-secure-custom-apis"></a>Distribuera, anropa och skydda anpassade API: er

När du har skapat dina anpassade API: er, konfigurera dina API: er för distribution så att du kan anropa dem på ett säkert sätt. Lär dig hur för[distribuera anropa och skydda anpassade API: er för logic apps](./logic-apps-custom-hosted-api.md).

## <a name="publish-custom-apis-tooazure"></a>Publicera anpassade API: er tooAzure

toomake dina anpassade API: er offentliga användas i Azure, skicka din nomineringarna toohello [Microsoft Azure-certifierad programmet](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Få hjälp

För detaljerad hjälp med anpassade API: er Kontakta [ customapishelp@microsoft.com ](mailto:customapishelp@microsoft.com).

tooask frågor, besvara frågor och se vilka andra användare i Azure Logic Apps gör finns hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp förbättra Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Logic Apps användaren feedbackwebbplats](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Nästa steg

* [Användningsmätning för åtgärder och utlösare](logic-apps-pricing.md)
* [Hantera innehållstyper](./logic-apps-content-type.md)
* [Hantera fel och undantag](./logic-apps-exception-handling.md)
* [Säker åtkomst tooyour logikappar](./logic-apps-securing-a-logic-app.md)
* [Anropa utlösare eller kapsla logikappar med HTTP-slutpunkter](./logic-apps-http-endpoint.md)