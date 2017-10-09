---
title: "aaaMonitor API: er med Azure API Management och Händelsehubbar Runscope | Microsoft Docs"
description: "Exempelprogram som visar hello loggen till eventhub principen genom att ansluta Azure API Management, Azure Event Hubs och Runscope för HTTP-loggning och övervakning"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: c528cf6f-5f16-4a06-beea-fa1207541a47
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 7456a2436f3a2d7b815b70b65fca9481d39c5fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Övervaka dina API: er med Azure API Management, Händelsehubbar och Runscope
Hej [API Management-tjänsten](api-management-key-concepts.md) innehåller många funktioner tooenhance hello bearbetning av HTTP-förfrågningar som skickas tooyour http-API. Dock hello förekomsten av hello begäranden och -svar är tillfälligt. hello-begäran har gjorts och den förs vidare via hello API Management-tjänsten tooyour backend API. Din API bearbetar hello begäran och ett svar går tillbaka genom toohello API konsumenten. hello API Management-tjänsten för att hålla viktig statistik om hello API: er för visning i portalens instrumentpanel för hello Publisher, men utöver, hello information är borta.

Med hjälp av hello [loggen till eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [princip](api-management-howto-policies.md) i hello API Management-tjänsten kan du skicka information från hello förfrågan och svar tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md). Det finns en mängd olika anledningar för varför toogenerate händelser från HTTP-meddelanden skickas tooyour API: er. Några exempel verifieringskedja för uppdateringar, användningsanalys, undantag varningar och 3 part integreringar.   

Den här artikeln visar hur toocapture hela HTTP-begäran och svar hälsningsmeddelande, skickar det tooan Event Hub och vidarebefordrar meddelandet tooa från tredje part tjänsten som innehåller HTTP-loggning och övervakning av tjänster.

## <a name="why-send-from-api-management-service"></a>Varför skicka från API Management-tjänsten?
Det är möjligt toowrite HTTP mellanprogram som kan ansluta till http-API ramverk toocapture HTTP-begäranden och -svar och feed dem till loggning och övervakning system. hello Nackdelen med toothis metod är hello HTTP mellanprogram måste toobe integreras hello backend API och måste matcha hello plattform hello API. Om det finns flera API: er måste var och en distribuera hello mellanprogram. Det finns ofta skäl varför backend API: er inte kan uppdateras.

Med loggning infrastruktur hello Azure API Management-tjänsten toointegrate erbjuder en centraliserad och plattformsoberoende lösning. Det är också skalbara delvis på grund av toohello [georeplikering](api-management-howto-deploy-multi-region.md) funktionerna i Azure API Management.

## <a name="why-send-tooan-azure-event-hub"></a>Varför skicka tooan Azure Event Hub?
Det är en rimlig tooask, varför ska man skapa en princip som är specifika tooAzure Händelsehubbar? Det finns många olika ställen där jag kanske toolog Mina förfrågningar. Varför inte bara skicka hello begäranden direkt toohello slutdestinationen?  Det är ett alternativ. Men vid av loggningsbegäranden från en API management-tjänsten, är det nödvändigt tooconsider hur meddelandeloggning påverkar hello prestanda för hello API. Stegvis ökning belastning kan hanteras genom att öka tillgängliga instanser av systemkomponenter eller genom att utnyttja geo-replikering. Kort toppar i trafik kan dock göra begäranden toobe avsevärt fördröjd om begäranden toologging infrastruktur startar tooslow under belastning.

hello Azure Event Hubs är utformad tooingress stora mängder data, med kapacitet för att hantera ett mycket högre antal händelser än hello antal HTTP-begäranden processen för de flesta API: er. hello Event Hub fungerar som en typ av avancerade buffert mellan din API-tjänsten och hello hanteringsinfrastruktur som ska lagra och bearbeta hälsningsmeddelande. Detta säkerställer att din API-prestanda blir inte lidande på grund av toohello loggning infrastruktur.  

När tooan Händelsehubb har skickats till hello data sparas och väntar på att Event Hub konsumenter tooprocess den. hello Event Hub bryr sig inte hur den bearbetas, det är bara mån om huruvida hello-meddelande levereras har.     

Händelsehubbar har hello möjlighet toostream händelser toomultiple konsumentgrupper. Detta gör att händelser toobe bearbetas av helt olika system. Detta gör att stödja många integrationsscenarier utan att lägga tillägg fördröjningar i hello bearbetning av hello API begäran inom hello API Management-tjänsten eftersom endast en händelse måste toobe genereras.

## <a name="a-policy-toosend-applicationhttp-messages"></a>En princip toosend program/HTTP-meddelanden
En Händelsehubb accepterar händelsedata som en sträng. hello innehållet i strängen är helt tooyou. toobe kan toopackage upp en HTTP-begäran och skicka ut tooEvent hubbar vi behöver tooformat hello sträng med information om hello begäran eller svar. I situationer, om det finns ett befintligt format vi kan återanvända kan sedan vi inte toowrite våra egna parsning kod. Först I anses vara med hello [HAR](http://www.softwareishard.com/blog/har-12-spec/) för att skicka HTTP-begäranden och -svar. Men är det här formatet optimerad för att lagra en sekvens av HTTP-begäranden i ett baserade JSON-format. Det finns ett antal obligatoriska element som lagts till onödiga komplexitet hello scenariot att skicka hello HTTP-meddelande via hello-överföring.  

Ett alternativ har toouse hello `application/http` medietyp enligt beskrivningen i hello HTTP-specifikationen [RFC 7230](http://tools.ietf.org/html/rfc7230). Den här medietypen använder hello exakt samma som format används tooactually skicka HTTP-meddelanden via hello överföring, men hela hello-meddelande kan placeras i ett annat HTTP-begäran hello brödtext. I vårt fall vi bara toouse hello brödtext som våra meddelandet toosend tooEvent Hubs. Det är enkelt, en tolk som finns i [Microsoft ASP.NET Web API 2.2 Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) bibliotek som kan tolka detta format och konvertera till hello intern `HttpRequestMessage` och `HttpResponseMessage` objekt.

toobe kan toocreate det här meddelandet måste tootake nytta av C#-baserade [principuttrycken](https://msdn.microsoft.com/library/azure/dn910913.aspx) i Azure API Management. Här är hello-principen som skickar en HTTP-begäran meddelandet tooAzure Händelsehubbar.

```xml
<log-to-eventhub logger-id="conferencelogger" partition-id="0">
@{
   var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                               context.Request.Method,
                                               context.Request.Url.Path + context.Request.Url.QueryString);

   var body = context.Request.Body?.As<string>(true);
   if (body != null && body.Length > 1024)
   {
       body = body.Substring(0, 1024);
   }

   var headers = context.Request.Headers
                          .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                          .ToArray<string>();

   var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

   return "request:"   + context.Variables["message-id"] + "\n"
                       + requestLine + headerString + "\r\n" + body;
}
</log-to-eventhub>
```

### <a name="policy-declaration"></a>Deklarationen för principen
Det några särskilda saker värt att nämna om den här principuttryck. hello loggen till eventhub princip har ett attribut som kallas loggaren-id som refererar toohello namnet på loggaren som har skapats i hello API Management-tjänsten. information om hur toosetup Event Hub-logg i hello API Management-tjänsten finns i dokumentet hello hello [hur toolog händelser tooAzure Händelsehubbar i Azure API Management](api-management-howto-log-event-hubs.md). andra hello-attributet är en valfri parameter som instruerar Händelsehubbar vilka partition toostore hello-meddelande i. Händelsehubbar använder partitioner tooenable skalbarhet och kräver minst två. hello sorterade leverans av meddelanden garanterat enbart inom en partition. Om vi inte att instruera Event Hub i vilken partition tooplace hello-meddelande, använder en algoritm för resursallokering toodistribute hello belastning. Dock kan orsaka några av våra toobe för meddelanden som bearbetas i fel ordning.  

### <a name="partitions"></a>Partitioner
tooensure våra meddelanden tooconsumers levereras i ordning och dra nytta av hello belastningen distribution möjligheterna för partitioner, jag har valt toosend HTTP-begäran meddelanden tooone partition och HTTP-svar meddelanden tooa andra partition. Detta säkerställer att en även belastningsdistribution och vi kan garantera att alla begäranden som kommer att användas i ordning och alla svar används i ordning. Det är möjligt att ett svar toobe upptog före hello motsvarande begäran, men som inte är ett problem som vi har en annan mekanism för korrelerar begär tooresponses och vi vet att förfrågningar alltid kommer före svar.

### <a name="http-payloads"></a>HTTP-nyttolaster
När du har skapat hello `requestLine` vi kontrollerar toosee om hello begärandetexten trunkeras. hello frågans brödtext är trunkerat tooonly 1024. Detta kan ökas, men enskilda Event Hub-meddelanden är begränsad too256KB så är det troligt att vissa HTTP-meddelande organ kommer inte att få plats i ett enda meddelande. När du gör loggning och analyser mycket information kan härledas från bara hello HTTP-begäran rad och rubriker. Dessutom många API-begäranden endast returnera små organ och så hello förlust av information värde med trunkera stora organ är ganska minimal i jämförelse toohello minskning i överföringen, bearbetning och lagring kostnader tookeep innehållet i meddelandetexten. En slutlig anteckning om bearbetning hello brödtext är vi behöver toopass `true` toohello som<string>()-metod eftersom vi läser hello brödtext innehåll, men det var även vill hello backend API toobe kan tooread hello brödtext. Genom att skicka true toothis metoden orsaka vi hello brödtext toobe buffras så att den kan läsa en andra gång. Det här är viktigt toobe känna till om du har en API som stöder överföring av stora filer eller använder långt avsökning. I dessa fall skulle det vara bästa tooavoid läsning av hello brödtext.   

### <a name="http-headers"></a>HTTP-huvuden
HTTP-huvuden kan överföras bara via till hello meddelandeformat i formatet enkla nyckel/värde-par. Vi har valt toostrip ut vissa tidskritiska fält, tooavoid i onödan läcka autentiseringsuppgifter. Det är inte troligt att API-nycklar och andra autentiseringsuppgifter används för analytics ändamål. Om vi vill toodo analys på hello användar- och hello viss produkt de använder och vi kan hämta som från hello `context` objekt och lägger till toohello meddelandet.     

### <a name="message-metadata"></a>Meddelandets Metadata
När du skapar hello slutfört toosend toohello händelsehubb hello första raden är inte en del av hello `application/http` meddelande. hello första raden är ytterligare metadata som består av om hello-meddelande är ett meddelande i begäran eller svar och ett meddelande-id som används toocorrelate begär tooresponses. hello-meddelande-id skapas med hjälp av en annan princip som ser ut så här:

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

Vi kan ha skapats hello begärandemeddelandet lagras att i en variabel tills hello svar var returneras och sedan bara skickas hello förfrågan och svar som ett enda meddelande. Hej dock två genom att skicka hello förfrågan och svar oberoende av varandra och med hjälp av ett meddelande-id toocorrelate, vi få lite mer flexibilitet i hello meddelandestorlek, hello möjlighet tootake nytta av flera partitioner med bibehållna meddelandet ordning och hello begäran att visas i vår loggning instrumentpanelen snabbare. Det kan också finnas vissa scenarier där ett giltigt svar skickas aldrig toohello händelsehubb, möjligen på grund av tooa allvarligt begäran fel i hello API Management-tjänsten, men vi kommer fortfarande ha en post för hello-begäran.

hello princip toosend hello HTTP-svarsmeddelandet ser ut ungefär toohello begäran och genomsökningsåtgärden hello principkonfiguration ser ut så här:

```xml
<policies>
  <inbound>
      <set-variable name="message-id" value="@(Guid.NewGuid())" />
      <log-to-eventhub logger-id="conferencelogger" partition-id="0">
      @{
          var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                      context.Request.Method,
                                                      context.Request.Url.Path + context.Request.Url.QueryString);

          var body = context.Request.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Request.Headers
                               .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                               .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                               .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "request:"   + context.Variables["message-id"] + "\n"
                              + requestLine + headerString + "\r\n" + body;
      }
  </log-to-eventhub>
  </inbound>
  <backend>
      <forward-request follow-redirects="true" />
  </backend>
  <outbound>
      <log-to-eventhub logger-id="conferencelogger" partition-id="1">
      @{
          var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                              context.Response.StatusCode,
                                              context.Response.StatusReason);

          var body = context.Response.Body?.As<string>(true);
          if (body != null && body.Length > 1024)
          {
              body = body.Substring(0, 1024);
          }

          var headers = context.Response.Headers
                                          .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                          .ToArray<string>();

          var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

          return "response:"  + context.Variables["message-id"] + "\n"
                              + statusLine + headerString + "\r\n" + body;
     }
  </log-to-eventhub>
  </outbound>
</policies>
```

Hej `set-variable` princip skapar ett värde som kan nås av både hello `log-to-eventhub` princip i hello `<inbound>` avsnittet och hello `<outbound>` avsnitt.  

## <a name="receiving-events-from-event-hubs"></a>Ta emot händelser från Event Hubs
Händelser från Azure Event Hub tas emot med hello [AMQP protokollet](http://www.amqp.org/). hello Microsoft Service Bus-teamet har gjort klienten bibliotek tillgängliga toomake hello förbrukar händelser enklare. Det finns två olika metoder som stöds, ett används en *direkt konsumenten* och hello andra använder hello `EventProcessorHost` klass. Exempel på dessa två metoder kan hittas i hello [Programmeringsguide för Event Hubs](../event-hubs/event-hubs-programming-guide.md). hello korta versionen av hello skillnaderna är `Direct Consumer` ger dig fullständig kontroll och hello `EventProcessorHost` har vissa delar av hello mekanik arbete för du men görs vissa antaganden om hur du bearbetar dessa händelser.  

### <a name="eventprocessorhost"></a>EventProcessorHost
I det här exemplet ska vi använda hello `EventProcessorHost` för enkelhetens skull, men den kan inte hello bäst för det här scenariot. `EventProcessorHost`hello arbete ser till att du inte har tooworry om trådmodell problem inom en viss händelse processor-klass. I vårt scenario, är vi dock bara konvertera hello meddelandeformat tooanother och passerar den längs tooanother tjänst med en async-metod. Det behövs ingen för att uppdatera delade tillstånd och därför ingen risk för trådmodell problem. I de flesta scenarier `EventProcessorHost` är förmodligen hello bäst och det är visserligen hello enklare alternativ.     

### <a name="ieventprocessor"></a>IEventProcessor
hello centrala begrepp när du använder `EventProcessorHost` är toocreate en en implementering av hello `IEventProcessor` gränssnitt som innehåller hello metoden `ProcessEventAsync`. hello grunden för metoden visas här:

```c#
async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
{

   foreach (EventData eventData in messages)
   {
       _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

       try
       {
           var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
           await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
       }
       catch (Exception ex)
       {
           _Logger.LogError(ex.Message);
       }
   }
    ... checkpointing code snipped ...
}
```

En lista över EventData objekt har skickats till hello-metoden och vi iterera över listan. hello byte för varje metod parsas till ett HttpMessage objekt och det objektet har skickats tooan instans av IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
Hej `HttpMessage` instans innehåller tre delar av data:

```c#
public class HttpMessage
{
   public Guid MessageId { get; set; }
   public bool IsRequest { get; set; }
   public HttpRequestMessage HttpRequestMessage { get; set; }
   public HttpResponseMessage HttpResponseMessage { get; set; }

... parsing code snipped ...

}
```

Hej `HttpMessage` instans innehåller en `MessageId` GUID som gör att vi tooconnect hello HTTP-begäran toohello motsvarande HTTP-svar och ett booleskt värde som anger om hello-objekt innehåller en instans av en HttpRequestMessage och HttpResponseMessage. Med hjälp av hello inbyggda HTTP-klasser från `System.Net.Http`, jag har tootake kan utnyttja hello `application/http` parsning av kod som ingår i `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
Hej `HttpMessage` instans vidarebefordras sedan tooimplementation av `IHttpMessageProcessor` som är ett gränssnitt som jag har skapat toodecouple hello mottagning och tolkning av hello händelse från Azure Event Hub och hello faktiska bearbetningen av den.

## <a name="forwarding-hello-http-message"></a>Vidarebefordran hello HTTP-meddelande
För det här exemplet jag valt det vore intressant toopush hello HTTP-begäran via för[Runscope](http://www.runscope.com). Runscope är en molnbaserad tjänst som är specialiserat på HTTP-felsökning, loggning och övervakning. De har en kostnadsfri nivå så att det är enkelt tootry och ger oss toosee hello HTTP-förfrågningar i realtid passerar genom våra API Management-tjänsten.

Hej `IHttpMessageProcessor` implementering ser ut så här,

```c#
public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
{
   private HttpClient _HttpClient;
   private ILogger _Logger;
   private string _BucketKey;
   public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
   {
       _HttpClient = httpClient;
       var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
       _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
       _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
       _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
       _Logger = logger;
   }

   public async Task ProcessHttpMessage(HttpMessage message)
   {
       var runscopeMessage = new RunscopeMessage()
       {
           UniqueIdentifier = message.MessageId
       };

       if (message.IsRequest)
       {
           _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
           runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
       }
       else
       {
           _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
           runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
       }

       var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
       messagesLink.BucketKey = _BucketKey;
       messagesLink.RunscopeMessage = runscopeMessage;
       var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
       _Logger.LogDebug("Request sent tooRunscope");
   }
}
```

Det var kan tootake nytta av en [befintliga klientbibliotek för Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) som gör det enkelt toopush `HttpRequestMessage` och `HttpResponseMessage` instanser in till tjänsten. I ordning hello tooaccess Runscope API du behöver ett konto och en API-nyckel. Anvisningar för att hämta en API-nyckel finns i hello [skapa program tooAccess Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) skärmutsändning.

## <a name="complete-sample"></a>Fullständigt exempel
Hej [källkod](https://github.com/darrelmiller/ApimEventProcessor) och tester för hello exemplet finns på GitHub. Du behöver ett [API Management-tjänsten](api-management-get-started.md), [anslutna Händelsehubben](api-management-howto-log-event-hubs.md), och en [Lagringskonto](../storage/common/storage-create-storage-account.md) toorun hello exempel själv.   

hello prov är endast ett enkelt konsolprogram som lyssnar efter händelser som kommer från Event Hub konverterar dem till en `HttpRequestMessage` och `HttpResponseMessage` objekt och vidarebefordrar dem på toohello Runscope API.

I följande animerad bild hello, ser du begäran tooan API i hello Developer-portalen, hello konsolen program som visar hello-meddelande tas emot, bearbetas och vidarebefordras och hello förfrågan och svar som visas i hello Runscope trafik Inspector.

![Demonstration av begäran vidarebefordras tooRunscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Sammanfattning
Azure API Management-tjänsten tillhandahåller en användbar toocapture hello HTTP-trafik färdas tooand från dina API: er. Händelsehubbar i Azure är en mycket skalbar, billig lösning för att samla in trafiken och matar in sekundär bearbetningssystem för loggning, övervakning och andra avancerade analyser. Ansluter too3rd part trafik övervakningssystem som Runscope är en enkel som några dussin rader med kod.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om Azure Event Hubs
  * [Kom igång med Händelsehubbar i Azure](../event-hubs/event-hubs-c-getstarted-send.md)
  * [Ta emot meddelanden med EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Programmeringsguide för Event Hubs](../event-hubs/event-hubs-programming-guide.md)
* Mer information om API-hantering och Händelsehubbar-integrering
  * [Hur toolog händelser tooAzure Händelsehubbar i Azure API Management](api-management-howto-log-event-hubs.md)
  * [Loggaren enhetsreferens](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [principreferens för loggen till eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
