---
title: "Utlösare för aaaApp API Apps | Microsoft Docs"
description: "Hur tooimplement utlöser i en API-App i Azure App Service"
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
ms.openlocfilehash: 2d6b6a942a23c0a93987e9c48b69ecc739bfd814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-api-app-triggers"></a>Utlösare för API Apps i Azure Apptjänst
> [!NOTE]
> Den här versionen av hello artikeln gäller tooAPI apps 2014-12-01-preview schemaversion.
>
>

## <a name="overview"></a>Översikt
Den här artikeln förklarar hur tooimplement API-app utlöser och använda dem från en logikapp.

Alla hello kodfragment i det här avsnittet kopieras från hello [FileWatcher API-App kodexemplet](http://go.microsoft.com/fwlink/?LinkId=534802).

Observera att du behöver toodownload hello följande nuget-paket för hello koden i den här artikeln toobuild och kör: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Vad är utlösare för API Apps?
Det är ett vanligt scenario för en händelse för toofire en API-app så att klienter på hello API-app kan vidta lämpliga åtgärder för hello i svaret toohello händelse. hello REST API-baserad funktion som stöder det här scenariot kallas för en utlösare för API-app.

Till exempel anta att din klientkod använder hello [Twitter-anslutningen API-app](../connectors/connectors-create-api-twitter.md) och din kod måste tooperform en åtgärd baserat på nya tweets med specifika ord. I så fall måste kan du ställa in en avsökning eller push-utlösare toofacilitate detta behov.

## <a name="poll-trigger-versus-push-trigger"></a>Avsökningen utlösaren jämfört med push-utlösare
För närvarande stöds två typer av utlösare:

* Avsökningen utlösaren - klienten ska avsöka hello API-app för meddelanden om en händelse med sagts upp
* Push-utlösare - klienten meddelas av hello API-app när en händelse utlöses

### <a name="poll-trigger"></a>Avsökningen utlösare
En avsökning utlösare implementeras som en vanlig REST-API och förväntar att dess klienter (till exempel en logikapp) toopoll i ordning tooget meddelande. När klienten hello kan upprätthålla tillstånd, är hello avsökning utlösaren själva tillståndslösa.

hello följande information om begäran och svar hälsningspaket illustrera vissa viktiga aspekter av hello avsökning utlösaren kontrakt:

* Förfrågan
  * HTTP-metod: hämta
  * Parametrar
    * triggerState - den här valfria parametern klienterna toospecify deras tillstånd så som hello avsökning utlösaren korrekt kan bestämma om tooreturn meddelande eller inte baserat på hello angetts tillstånd.
    * API-specifika parametrar
* Svar
  * Statuskoden **200** - begäran är giltig och att det finns ett meddelande från hello utlösare. hello innehåll av hello-meddelande kommer att hello svarstexten. Ett ”försök igen efter”-huvud i hello svaret anger att ytterligare meddelandedata måste hämtas med ett efterföljande begäran-anrop.
  * Statuskoden **202** - begäran är giltig, men det finns inget nytt meddelande från hello utlösare.
  * Statuskoden **4xx** -begäran är inte giltig. hello klienten bör inte försöka hello-begäran.
  * Statuskoden **5xx** -förfrågan resulterade i ett internt serverfel och/eller ett tillfälligt problem. hello klienten bör försöka hello-begäran.

hello följande kodavsnitt är ett exempel på hur tooimplement röstning utlösa.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check toosee whether there is any file touched after hello timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after hello timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after hello timestamp, tell hello caller toopoll again after 1 mintue.
        else
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

tootest utlösa den här avsökningen, gör du följande:

1. Distribuera hello API App med en inställning för autentisering av **offentliga anonym**.
2. Anropa hello **touch** åtgärden tootouch en fil. hello följande bild visar ett exempel på begäran via Postman.
   ![Anropa Touch åtgärden via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Anropa hello avsökning utlösare med hello **triggerState** parameterinställning tooa tidsstämpel tidigare tooStep #2. hello visar följande bild hello exempelbegäran via Postman.
   ![Anropa utlösare avsökning via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Push-utlösare
En push-utlösare implementeras som en vanlig REST-API som skickar meddelanden tooclients som har registrerat toobe meddelas när specifika händelser eller.

följande information om begäran och svar hälsningspaket hello illustrera vissa viktiga aspekter av hello push-utlösare kontraktet.

* Förfrågan
  * HTTP-metod: PLACERA
  * Parametrar
    * Utlösarens ID: krävs – ogenomskinlig sträng (till exempel ett GUID) som representerar hello registreringen av push-utlösare.
    * callbackUrl: krävs - URL för hello återanrop tooinvoke när hello händelsen utlöses. hello anrop är en enkel HTTP POST-anrop.
    * API-specifika parametrar
* Svar
  * Statuskoden **200** -begäran tooregister klienten lyckas.
  * Statuskoden **4xx** -begäran är inte giltig. hello klienten bör inte försöka hello-begäran.
  * Statuskoden **5xx** -förfrågan resulterade i ett internt serverfel och/eller ett tillfälligt problem. hello klienten bör försöka hello-begäran.
* Motringning
  * HTTP-metod: POST
  * Begäran: meddelandeinnehåll.

hello följande kodavsnitt är ett exempel på hur tooimplement en push utlösa:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by hello AppService service SDK.
        // Here it defines hello input of hello push trigger is a string and hello output toohello callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register hello trigger toosome trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by hello AppService service SDK indicating hello registration is completed.
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
            // Use FileSystemWatcher toolisten toofile change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire hello push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate hello FileSystemWatcher object with hello triggerId.
            _store[triggerId] = watcher;

        }

        // Fire hello assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed tooinvoke hello callback.
            Runtime runtime,
            // hello callback tooinvoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK tooinvoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

tootest utlösa den här avsökningen, gör du följande:

1. Distribuera hello API App med en inställning för autentisering av **offentliga anonym**.
2. Bläddra för[http://requestb.in/](http://requestb.in/) toocreate en RequestBin som fungerar som återanrop URL: en.
3. Anropa hello push-utlösare med ett GUID som **utlösarens ID** och hello RequestBin URL: en som **callbackUrl**.
   ![Anropa Push-utlösare via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Anropa hello **touch** åtgärden tootouch en fil. hello följande bild visar ett exempel på begäran via Postman.
   ![Anropa Touch åtgärden via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Kontrollera hello RequestBin tooconfirm som hello push-utlösare återanropet anropas med egenskapen utdata.
   ![Anropa utlösare avsökning via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Beskriv utlösare i API-definition
När du implementerar hello utlösare och distribuera din API app tooAzure, navigera toohello **API-Definition** bladet i hello Azure preview portal och du ser att identifieras automatiskt utlösare i hello-Gränssnittet som styrs av Hej Swagger 2.0 API-definition av hello API-app.

![Bladet för API-Definition](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Om du klickar på hello **hämta Swagger** knappen och öppna hello JSON-fil visas resultaten liknande toohello följande:

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

Hej tilläggsegenskapen **x-ms-schedular-utlösaren** är hur utlösare beskrivs i API-definitions- och läggs till automatiskt av gateway för hello API-app när du begär hello API-definition via hello gateway om hello begär tooone av Hej följande villkor. (Du kan också lägga till den här egenskapen manuellt.)

* Avsökningen utlösare
  * Om hello HTTP-metoden är **hämta**.
  * Om hello **operationId** egenskapen innehåller hello sträng **utlösaren**.
  * Om hello **parametrar** -egenskapen innehåller en parameter med en **namn** egenskapsuppsättning för**triggerState**.
* Push-utlösare
  * Om hello HTTP-metoden är **PLACERA**.
  * Om hello **operationId** egenskapen innehåller hello sträng **utlösaren**.
  * Om hello **parametrar** -egenskapen innehåller en parameter med en **namn** egenskapsuppsättning för**utlösarens ID**.

## <a name="use-api-app-triggers-in-logic-apps"></a>Använd utlösare för API Apps i Logic apps
### <a name="list-and-configure-api-app-triggers-in-hello-logic-apps-designer"></a>Visa och konfigurera utlösare för API Apps i hello Logic apps designer
Om du skapar en logikapp i hello samma resursgrupp som Hej API-appen, kommer du att kunna tooadd den toohello designerytan genom att klicka på den. hello följande bilder visar detta:

![Utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfigurera avsökning utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfigurera Push-utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Optimera API app utlösare för Logic apps
När du lägger till utlösare tooan API-app, finns det några saker du kan göra tooimprove hello upplevelse när du använder hello API-app i en logikapp.

Till exempel hello **triggerState** parameter för avsökningsutlösare ska anges toohello följande uttryck i hello logikapp. Det här uttrycket ska utvärdera hello senaste anrop av hello utlösaren från hello logikapp och returnera värdet.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

: En förklaring av hello-funktioner som används i hello uttrycket ovan finns toohello dokumentation på [språk i Arbetsflödesdefinitionen för logik App](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Logik appanvändare skulle behöva tooprovide hello-uttryck ovanför hello **triggerState** parameter när du använder hello utlösare. Det är möjligt toohave värdet förinställningen av hello logik app designer via hello tilläggsegenskapen **x-ms-scheduler-rekommendation**.  Hej **x-ms-synlighet** utökade egenskapen kan anges tooa värdet för *interna* så att hello parametern själva inte visas hello designer.  hello följande fragment visas som.

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

Push-utlösare hello **utlösarens ID** parameter måste identifiera hello logikapp. En rekommenderad metod är tooset toohello egenskapsnamnet för hello arbetsflöde med hjälp av hello följande uttryck:

    @workflow().name

Med hjälp av hello **x-ms-scheduler-rekommendation** och **x-ms-synlighet** tilläggsegenskaper i dess API ingår, hello API-app kan förmedla toohello logik app designer tooautomatically Ställ in uttryck för hello användare.

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
Information om ytterligare metadata -, till exempel hello tilläggsegenskaper **x-ms-scheduler-rekommendation** och **x-ms-synlighet** -kan läggas till i hello API attributdefinitionstabellen på något av två sätt: statisk eller dynamisk.

För statisk metadata, kan du direkt redigera hello */metadata/apiDefinition.swagger.json* i projektet och Lägg till hello egenskaper manuellt.

Du kan redigera hello SwaggerConfig.cs filen tooadd ett åtgärden filter som kan lägga till dessa tillägg för API-appar som använder dynamiska metadata.

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


hello följande är ett exempel på hur den här klassen kan vara implementerad toofacilitate hello dynamiska metadata scenario.

    // Add extension properties on hello triggerState parameter
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
                    // x-ms-visibility: set too'internal' toosignify this is an internal field
                    // x-ms-scheduler-recommendation: set tooa value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
