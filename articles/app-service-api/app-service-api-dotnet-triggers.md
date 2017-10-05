---
title: "Apptjänst API app utlösare | Microsoft Docs"
description: "Implementera utlösare i en API-App i Azure App Service"
services: logic-apps
documentationcenter: .net
author: guangyang
manager: erikre
editor: jimbe
ms.assetid: 493c3703-786d-4434-9dca-8f77744b2f5d
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: rachelap
ms.openlocfilehash: 3ddfb142e7f1a47e2a8564387da785acf36fa61f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-api-app-triggers"></a>Utlösare för API Apps i Azure Apptjänst
> [!NOTE]
> Den här versionen av artikeln gäller för schemaversionen för API apps 2014-12-01-preview.
>
>

## <a name="overview"></a>Översikt
Den här artikeln förklarar hur du implementerar utlösare för API Apps och använda dem från en logikapp.

Alla kodfragment i det här avsnittet kopieras från den [FileWatcher API-App kodexemplet](http://go.microsoft.com/fwlink/?LinkId=534802).

Observera att du behöver hämta följande nuget-paket för koden i den här artikeln för att skapa och köra: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Vad är utlösare för API Apps?
Det är ett vanligt scenario för en API-app att utlösa en händelse så att klienter på API-appen kan vidta lämplig åtgärd som svar på händelsen. Mekanismen för REST API-baserade som har stöd för det här scenariot kallas för en utlösare för API-app.

Till exempel anta att din klientkod använder den [Twitter-anslutningen API-app](../connectors/connectors-create-api-twitter.md) och din kod behöver utföra en åtgärd baserat på nya tweets med specifika ord. I det här fallet kan du ställa in en avsökning eller push-utlösare att underlätta detta behov.

## <a name="poll-trigger-versus-push-trigger"></a>Avsökningen utlösaren jämfört med push-utlösare
För närvarande stöds två typer av utlösare:

* Avsökningen utlösaren - klienten ska avsöka API-app för meddelanden om en händelse med sagts upp
* Push-utlösare - klienten meddelas av API-app när en händelse utlöses

### <a name="poll-trigger"></a>Avsökningen utlösare
En avsökning utlösare implementeras som en vanlig REST-API och förväntar sig klienter (till exempel en logikapp) ska avsöka för att få meddelandet. Medan klienten kan upprätthålla tillstånd, är utlösarens omröstning statslösa.

Följande information om begäran och svar paket illustrera vissa viktiga aspekter av omröstningen utlösaren kontrakt:

* Förfrågan
  * HTTP-metod: hämta
  * Parametrar
    * triggerState - den här valfria parametern klienterna ange deras tillstånd så att utlösaren avsökning korrekt avgöra om du vill returnera meddelande baserat på det angivna tillståndet.
    * API-specifika parametrar
* Svar
  * Statuskoden **200** - begäran är giltig och att det finns ett meddelande från utlösaren. Innehållet i meddelandet kommer att brödtext för svar. En ”försök igen efter”-huvudet i svaret anger att ytterligare meddelandedata måste hämtas med ett efterföljande begäran-anrop.
  * Statuskoden **202** - begäran är giltig, men det finns inget nytt meddelande från utlösaren.
  * Statuskoden **4xx** -begäran är inte giltig. Klienten bör inte försöka.
  * Statuskoden **5xx** -förfrågan resulterade i ett internt serverfel och/eller ett tillfälligt problem. Klienten bör försöka.

Följande kodavsnitt är ett exempel på hur du implementerar en omröstning utlösare.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Följ dessa steg om du vill testa den här avsökningen utlösaren:

1. Distribuera API-App med en inställning för autentisering av **offentliga anonym**.
2. Anropa den **touch** åtgärden touch en fil. Följande bild visar ett exempel på begäran via Postman.
   ![Anropa Touch åtgärden via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Anropa avsökning utlösaren med den **triggerState** parametern inställd på en tidsstämpel innan steg #2. Följande bild visar exempelbegäran via Postman.
   ![Anropa utlösare avsökning via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Push-utlösare
En push-utlösare implementeras som en vanlig REST-API som skickar meddelanden till klienter som har registrerat som ska meddelas när specifika händelser eller.

Följande information om begäran och svar paket illustrera vissa viktiga aspekter av kontraktet för push-utlösare.

* Förfrågan
  * HTTP-metod: PLACERA
  * Parametrar
    * Utlösarens ID: krävs – täckande sträng (till exempel ett GUID) som representerar registreringen av push-utlösare.
    * callbackUrl: krävs - URL för återanropet ska anropa när händelsen utlöses. Anropet är en enkel HTTP POST-anrop.
    * API-specifika parametrar
* Svar
  * Statuskoden **200** -begäran om att registrera klienten lyckas.
  * Statuskoden **4xx** -begäran är inte giltig. Klienten bör inte försöka.
  * Statuskoden **5xx** -förfrågan resulterade i ett internt serverfel och/eller ett tillfälligt problem. Klienten bör försöka.
* Motringning
  * HTTP-metod: POST
  * Begäran: meddelandeinnehåll.

Följande kodavsnitt är ett exempel på hur du implementerar en push-utlösare:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Följ dessa steg om du vill testa den här avsökningen utlösaren:

1. Distribuera API-App med en inställning för autentisering av **offentliga anonym**.
2. Bläddra till [http://requestb.in/](http://requestb.in/) att skapa en RequestBin som fungerar som återanrop URL: en.
3. Anropa push-utlösare med ett GUID som **utlösarens ID** och RequestBin-URL: en som **callbackUrl**.
   ![Anropa Push-utlösare via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Anropa den **touch** åtgärden touch en fil. Följande bild visar ett exempel på begäran via Postman.
   ![Anropa Touch åtgärden via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Kontrollera RequestBin för att bekräfta att push-utlösare återanropet anropas med egenskapen utdata.
   ![Anropa utlösare avsökning via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Beskriv utlösare i API-definition
När du implementerar utlösare och distribuera din API-app till Azure, navigera till den **API-Definition** bladet i Azure preview portal och du ser att identifieras automatiskt utlösare i Gränssnittet som drivs av Swagger 2.0 API-definition av API-app.

![Bladet för API-Definition](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Om du klickar på den **hämta Swagger** knappen och öppna JSON-filen, visas resultatet liknar följande:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

Den utökade egenskapen **x-ms-schedular-utlösaren** är hur utlösare beskrivs i API-definitions- och läggs till automatiskt av gateway för API-app när du begär API-definition via gatewayen om begäran till en av de följande villkor. (Du kan också lägga till den här egenskapen manuellt.)

* Avsökningen utlösare
  * Om HTTP-metoden är **hämta**.
  * Om den **operationId** egenskap innehåller strängen **utlösaren**.
  * Om den **parametrar** -egenskapen innehåller en parameter med en **namn** egenskapen **triggerState**.
* Push-utlösare
  * Om HTTP-metoden är **PLACERA**.
  * Om den **operationId** egenskap innehåller strängen **utlösaren**.
  * Om den **parametrar** -egenskapen innehåller en parameter med en **namn** egenskapen **utlösarens ID**.

## <a name="use-api-app-triggers-in-logic-apps"></a>Använd utlösare för API Apps i Logic apps
### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Visa och konfigurera utlösare för API Apps i Logic apps designer
Om du skapar en logikapp i samma resursgrupp som API-app kommer du att kunna lägga till den till på designerytan genom att klicka på den. Följande bilder visar detta:

![Utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfigurera avsökning utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfigurera Push-utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Optimera API app utlösare för Logic apps
När du lägger till utlösare en API-app, finns det några saker du kan göra för att förbättra upplevelsen när du använder API-app i en logikapp.

Till exempel den **triggerState** parameter för avsökningsutlösare ska anges till följande uttryck i logikappen. Det här uttrycket ska utvärdera senaste anrop av utlösaren från logikappen och returnera värdet.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Obs: En förklaring av de funktioner som används i uttrycket ovan finns i dokumentationen på [språk i Arbetsflödesdefinitionen för logik App](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Logik för app-användare måste ange uttrycket ovan för den **triggerState** parameter när du använder utlösaren. Det är möjligt att har det här värdet av logik app designer via egenskapen extension **x-ms-scheduler-rekommendation**.  Den **x-ms-synlighet** utökade egenskapen kan anges till ett värde av *interna* så att parametern själva inte visas i designern.  Följande utdrag visar som.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

För push-utlösare i **utlösarens ID** parameter måste identifiera logikappen. En rekommenderad metod är att ange egenskapen till namnet på arbetsflödet med hjälp av följande uttryck:

    @workflow().name

Med hjälp av den **x-ms-scheduler-rekommendation** och **x-ms-synlighet** tilläggsegenskaper i dess API ingår API-appen kan förmedla logik app Designer att automatiskt ange det här uttrycket för den användaren.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Lägga till tilläggsegenskaper i API attributdefinitionstabellen
Information om ytterligare metadata -, till exempel egenskaper för webbtjänsttillägg **x-ms-scheduler-rekommendation** och **x-ms-synlighet** -kan läggas till i API-attributdefinitionstabellen på något av två sätt: statisk eller dynamisk.

För statisk metadata, du kan redigera direkt i */metadata/apiDefinition.swagger.json* filen i projektet och Lägg till egenskaper manuellt.

Du kan redigera filen SwaggerConfig.cs för att lägga till ett filter för åtgärden som du kan lägga till dessa tillägg för API-appar som använder dynamiska metadata.

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Följande är ett exempel på hur den här klassen kan implementeras för att underlätta scenariot för dynamisk metadata.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
