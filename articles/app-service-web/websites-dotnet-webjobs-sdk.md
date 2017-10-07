---
title: "aaaWhat är hello Azure WebJobs SDK"
description: "En introduktion toohello Azure WebJobs SDK. Beskriver vilka hello SDK är typiska scenarier som det är användbart för, och kodexempel."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 8281267b-572b-4b14-a328-6704493ea682
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: efac7a75c3b68a6a6601fb298f2ccac9bd71709d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-hello-azure-webjobs-sdk"></a>Vad är hello Azure WebJobs SDK
## <a id="overview"></a>Översikt över
Den här artikeln förklaras vad hello WebJobs-SDK är, går igenom några vanliga scenarier som den är användbar för och ger en översikt över hur du använder i din kod.

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

[WebJobs](websites-webjobs-resources.md) är en funktion i Azure App Service som gör att du toorun ett program eller skript i hello samma kontext som webbprogrammet, API-app eller mobila appar. Hej syftet hello [WebJobs SDK](websites-webjobs-resources.md) är toosimplify hello koden du skriver för vanliga uppgifter som ett Webbjobb kan utföra, till exempel bild bearbetning, kö bearbetning, RSS aggregering, filunderhåll, och skicka e-post. Hej WebJobs SDK har inbyggda funktioner för att arbeta med Azure Storage- och Service Bus för schemalagda aktiviteter och hantera fel och för många andra vanliga scenarier. Dessutom har designats toobe extensible. Hej [WebJobs-SDK är öppen källkod](https://github.com/Azure/azure-webjobs-sdk/), och det finns en [öppen källkod lagringsplatsen för tillägg](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

Hej WebJobs SDK innehåller hello följande komponenter:

* **NuGet-paket**. NuGet-paket som du lägger till tooa konsolprogram för Visual Studio-projekt med ett ramverk som din kod använder genom att pynta din metoder med WebJobs SDK-attribut.
* **Instrumentpanelen**. En del av hello WebJobs SDK ingår i Azure App Service och ger omfattande övervakning och diagnostik för program som använder hello NuGet-paket. Du har inte toowrite kod toouse funktionerna övervakning och diagnostik.

## <a id="scenarios"></a>Scenarier
Här följer några vanliga scenarier som du kan hantera enklare med hello Azure WebJobs SDK:

* Bild bearbetning eller andra processorintensiva arbetet. En vanlig funktion för webbplatser är hello möjlighet tooupload bilder eller videofiler. Ofta vill du toomanipulate hello innehåll när det överförs, men du inte vill toomake hello användaren vänta medan du gör.
* Bearbetning av en kö. Ett vanligt sätt för ett webbprogram klientdel toocommunicate med en serverdelstjänst är toouse köer. När hello webbplats måste tooget arbete, skickas ett meddelande till en kö. En backend-tjänst tar emot meddelanden från kön hello och hello arbete. Du kan använda köer för bearbetning av avbildningen: till exempel när hello användaren överför ett antal filer, placera hello filnamn i en kö meddelandet toobe som tas upp av hello serverdelen för bearbetning. Eller så kan du använda köer tooimprove plats svarstider. I stället för att skriva direkt tooa SQL-databas, till exempel skriva tooa kön, meddela hello användaren du är klar och låta hello backend service referensen tidsfördröjning relationsdatabas fungerar. Ett exempel på behandling med avbildningen kön finns hello [WebJobs SDK komma igång-kursen](websites-dotnet-webjobs-sdk-get-started.md).
* RSS-aggregering. Om du har en webbplats som innehåller en lista med RSS-feeds kan du dra i alla hello artiklar från hello feeds i bakgrunden.
* Filunderhåll, till exempel aggregering av information eller rensa loggfiler.  Du måste kanske loggfiler som skapas av flera platser eller för separata tidsintervall som du vill använda toocombine i order toorun analys jobb på dem. Eller så kanske vill tooschedule en aktivitet toorun veckovisa tooclean in gamla loggfiler.
* Ingång i Azure-tabeller. Du kan ha filer som lagras och blobbar och vill tooparse dem och lagra hello data i tabeller. hello ingång funktion kan skriva till flera rader (miljoner i vissa fall) och hello WebJobs SDK gör det möjligt tooimplement funktionen enkelt. hello SDK innehåller också realtidsövervakning av förloppsindikatorer såsom hello antal rader som skrivits i hello tabell.
* Andra långvariga aktiviteter som du vill toorun i en bakgrundstråd som [skicka e-post](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 
* Alla aktiviteter som du vill toorun enligt ett schema, till exempel en säkerhetskopiering utförs varje natt.

I många av dessa scenarier kan du tooscale en web app toorun på flera virtuella datorer som kör flera WebJobs samtidigt. I vissa scenarier som detta kan resultera i hello samma komma bearbetade data är flera gånger, men det inte ett problem när du använder hello inbyggda kön, blob och Service Bus-utlösare för hello WebJobs-SDK. hello SDK garanterar att dina funktioner kommer att bearbetas bara en gång för varje meddelande eller blob.

Hej WebJobs SDK gör det också enkelt toohandle vanliga scenarier för felhantering. Du kan ställa in aviseringar toosend meddelanden när en funktion misslyckas och du kan ange timeout så att en funktion annulleras automatiskt om det inte slutföras inom angiven tid.

## <a id="code"></a>Kodexempel
hello-kod för att hantera vanliga uppgifter som fungerar med Azure Storage är enkelt. I din konsolprogram `Main` metod som du skapar en `JobHost` objekt som samordnar hello anropar toomethods som du skriver. Hej WebJobs SDK framework vet toocall din metoder och vilka parametern värden toouse baserat på hello WebJobs SDK attribut när du använder i dem. hello SDK innehåller *utlösare* som anger vilka villkor orsaka hello funktionen toobe anropas och *bindning* som anger hur tooget information till och från metodparametrar.

Till exempel hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) -attributet skapar en funktionen toobe anropas när ett meddelande i en kö och om hello meddelandeformatet är JSON för en bytematris eller en anpassad typ, hello-meddelande automatiskt avserialiserat. Hej [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) attributet utlöser en process när en ny blob skapas i ett Azure Storage-konto.

Här är ett enkelt program som genomsöker en kö och skapar en blob för varje kö meddelande som tagits emot:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

Hej `JobHost` objektet är en behållare för en uppsättning funktioner för bakgrunden. Hej `JobHost` objektet Övervakare hello fungerar, söker efter händelser som utlöser dem och körs hello funktioner när utlösaren händelser inträffar. Du anropar en `JobHost` metoden tooindicate om du vill hello behållaren processen toorun på hello aktuella tråden eller en bakgrundstråd. I exemplet hello hello `RunAndBlock` metoden körs kontinuerligt hello processen på hello aktuella tråden.

Eftersom hello `ProcessQueueMessage` metod i det här exemplet har en `QueueTrigger` attributet hello utlösare för funktionen är hello mottagning av ett nytt meddelande i kön. Hej `JobHost` objektet söker efter nya Kömeddelanden på hello kö som anges (”webjobsqueue” i det här exemplet) och när ett hittas anropas `ProcessQueueMessage`. 

Hej `QueueTrigger` attributet Binder hello `inputText` parametervärdet toohello hälsningsmeddelande för kön. Och hello `Blob` attributet bindningar en `TextWriter` objektet tooa blob med namnet ”blobname” i en behållare med namnet ”containername”.  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

hello funktionen använder sedan dessa parametrar toowrite hello värdet hello kön meddelandet toohello blob:

        writer.WriteLine(inputText);

hello utlösare och binder funktioner i hello WebJobs SDK förenklar hello-kod som du har toowrite. Hej lågnivå kod krävs tooprocess köer, blobbar, eller filer eller tooinitiate schemalagda aktiviteter, görs av hello WebJobs SDK framework. Till exempel hello framework skapar köer som inte finns ännu, öppnas hello kön, läser kö meddelanden, tar bort kö meddelanden när bearbetningen är klar, skapar blobbbehållare som inte finns ännu, skriver tooblobs och så vidare.

hello följande kodexempel visas en mängd utlösare i en Webbjobb: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, och `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            too= "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors toohello Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Schemaläggning
Hej `TimerTrigger` attributet ger du hello möjlighet tootrigger funktioner toorun enligt ett schema. Du kan schemalägga ett Webbjobb som en hela via Azure eller schema enskilda funktioner i ett Webbjobb med hello WebJobs SDK `TimerTrigger`. Här följer ett exempel på koden.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Fler exempel finns i [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) i hello azure-webjobs-sdk-tillägg-databas på GitHub.com.

## <a name="extensibility"></a>Utökningsbarhet
Du är inte begränsade toobuilt i funktionen--hello WebJobs SDK kan du toowrite anpassade utlösare och Binder-dokument.  Du kan till exempel skriva utlösare för återkommande scheman och cache-händelser. En [öppen källkod databasen](https://github.com/Azure/azure-webjobs-sdk-extensions) innehåller en [detaljerad vägledning om WebJobs SDK utökningsbarhet](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) och exempel kod toohelp komma igång skriva egna utlösare och Binder-dokument.

## <a id="workerrole"></a>Med hjälp av hello WebJobs SDK utanför WebJobs
Ett program som använder hello hello WebJobs-SDK är en standard konsolprogram och kan köras var som helst--den har inte toorun som ett Webbjobb. Du kan testa programmet hello lokalt på utvecklingsdatorn och i produktion kan du köra det i en arbetsroll molntjänst eller en Windows-tjänst om du föredrar en av dessa miljöer. 

Hello instrumentpanelen är endast tillgänglig som ett tillägg för en webbapp i Azure App Service. Om du vill toorun utanför ett Webbjobb och fortfarande använda hello instrumentpanelen kan du konfigurera en webbplats app toouse hello samma lagringskonto WebJobs SDK-instrumentpanelen anslutningssträngen refererar till, och att webbprogrammet WebJobs-instrumentpanelen visas sedan data om funktionen körning från ditt program som kör någon annanstans. Du kan hämta toohello instrumentpanel med hello URL: en https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions. Mer information finns i [får en instrumentpanel för lokal utveckling med hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), men Observera att hello blogginlägget visar ett gamla namn för anslutningssträngen. 

## <a id="nostorage"></a>Instrumentpanelen funktioner
Hej WebJobs SDK innehåller flera fördelar, även om du inte använder WebJobs-SDK utlösare eller bindning:

* Du kan anropa funktioner från hello instrumentpanelen.
* Du kan spela upp funktioner från hello instrumentpanelen.
* Du kan visa loggar i hello instrumentpanel, länkade toohello viss Webbjobb (programloggarna, skrivs med hjälp av Console.Out, Console.Error, Trace, etc.) eller länkade toohello viss funktionsanrop som genererade dem (loggar som skrivs med hjälp av en `TextWriter` objekt som hello SDK skickar toohello funktion som en parameter). 

Mer information finns i [hur toomanually anropa en funktion](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) och [hur toowrite loggar](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Nästa steg
Mer information om hello WebJobs SDK finns [Azure WebJobs rekommenderas resurser](http://go.microsoft.com/fwlink/?linkid=390226).

Information om hello senaste förbättringar toohello WebJobs SDK finns hello [viktig information](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).

