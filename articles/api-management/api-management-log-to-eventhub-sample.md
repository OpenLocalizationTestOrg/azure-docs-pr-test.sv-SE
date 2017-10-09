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
# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a><span data-ttu-id="72b49-103">Övervaka dina API: er med Azure API Management, Händelsehubbar och Runscope</span><span class="sxs-lookup"><span data-stu-id="72b49-103">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>
<span data-ttu-id="72b49-104">Hej [API Management-tjänsten](api-management-key-concepts.md) innehåller många funktioner tooenhance hello bearbetning av HTTP-förfrågningar som skickas tooyour http-API.</span><span class="sxs-lookup"><span data-stu-id="72b49-104">hello [API Management service](api-management-key-concepts.md) provides many capabilities tooenhance hello processing of HTTP requests sent tooyour HTTP API.</span></span> <span data-ttu-id="72b49-105">Dock hello förekomsten av hello begäranden och -svar är tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="72b49-105">However, hello existence of hello requests and responses are transient.</span></span> <span data-ttu-id="72b49-106">hello-begäran har gjorts och den förs vidare via hello API Management-tjänsten tooyour backend API.</span><span class="sxs-lookup"><span data-stu-id="72b49-106">hello request is made and it flows through hello API Management service tooyour backend API.</span></span> <span data-ttu-id="72b49-107">Din API bearbetar hello begäran och ett svar går tillbaka genom toohello API konsumenten.</span><span class="sxs-lookup"><span data-stu-id="72b49-107">Your API processes hello request and a response flows back through toohello API consumer.</span></span> <span data-ttu-id="72b49-108">hello API Management-tjänsten för att hålla viktig statistik om hello API: er för visning i portalens instrumentpanel för hello Publisher, men utöver, hello information är borta.</span><span class="sxs-lookup"><span data-stu-id="72b49-108">hello API Management service keeps some important statistics about hello APIs for display in hello Publisher portal dashboard, but beyond that, hello details are gone.</span></span>

<span data-ttu-id="72b49-109">Med hjälp av hello [loggen till eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [princip](api-management-howto-policies.md) i hello API Management-tjänsten kan du skicka information från hello förfrågan och svar tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="72b49-109">By using hello [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [policy](api-management-howto-policies.md) in hello API Management service you can send any details from hello request and response tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span> <span data-ttu-id="72b49-110">Det finns en mängd olika anledningar för varför toogenerate händelser från HTTP-meddelanden skickas tooyour API: er.</span><span class="sxs-lookup"><span data-stu-id="72b49-110">There are a variety of reasons why you may want toogenerate events from HTTP messages being sent tooyour APIs.</span></span> <span data-ttu-id="72b49-111">Några exempel verifieringskedja för uppdateringar, användningsanalys, undantag varningar och 3 part integreringar.</span><span class="sxs-lookup"><span data-stu-id="72b49-111">Some examples include audit trail of updates, usage analytics, exception alerting and 3rd party integrations.</span></span>   

<span data-ttu-id="72b49-112">Den här artikeln visar hur toocapture hela HTTP-begäran och svar hälsningsmeddelande, skickar det tooan Event Hub och vidarebefordrar meddelandet tooa från tredje part tjänsten som innehåller HTTP-loggning och övervakning av tjänster.</span><span class="sxs-lookup"><span data-stu-id="72b49-112">This article demonstrates how toocapture hello entire HTTP request and response message, send it tooan Event Hub and then relay that message tooa third party service that provides HTTP logging and monitoring services.</span></span>

## <a name="why-send-from-api-management-service"></a><span data-ttu-id="72b49-113">Varför skicka från API Management-tjänsten?</span><span class="sxs-lookup"><span data-stu-id="72b49-113">Why Send From API Management Service?</span></span>
<span data-ttu-id="72b49-114">Det är möjligt toowrite HTTP mellanprogram som kan ansluta till http-API ramverk toocapture HTTP-begäranden och -svar och feed dem till loggning och övervakning system.</span><span class="sxs-lookup"><span data-stu-id="72b49-114">It is possible toowrite HTTP middleware that can plug into HTTP API frameworks toocapture HTTP requests and responses and feed them into logging and monitoring systems.</span></span> <span data-ttu-id="72b49-115">hello Nackdelen med toothis metod är hello HTTP mellanprogram måste toobe integreras hello backend API och måste matcha hello plattform hello API.</span><span class="sxs-lookup"><span data-stu-id="72b49-115">hello downside toothis approach is hello HTTP middleware needs toobe integrated into hello backend API and must match hello platform of hello API.</span></span> <span data-ttu-id="72b49-116">Om det finns flera API: er måste var och en distribuera hello mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="72b49-116">If there are multiple APIs then each one must deploy hello middleware.</span></span> <span data-ttu-id="72b49-117">Det finns ofta skäl varför backend API: er inte kan uppdateras.</span><span class="sxs-lookup"><span data-stu-id="72b49-117">Often there are reasons why backend APIs cannot be updated.</span></span>

<span data-ttu-id="72b49-118">Med loggning infrastruktur hello Azure API Management-tjänsten toointegrate erbjuder en centraliserad och plattformsoberoende lösning.</span><span class="sxs-lookup"><span data-stu-id="72b49-118">Using hello Azure API Management service toointegrate with logging infrastructure provides a centralized and platform-independent solution.</span></span> <span data-ttu-id="72b49-119">Det är också skalbara delvis på grund av toohello [georeplikering](api-management-howto-deploy-multi-region.md) funktionerna i Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="72b49-119">It is also scalable, in part due toohello [geo-replication](api-management-howto-deploy-multi-region.md) capabilities of Azure API Management.</span></span>

## <a name="why-send-tooan-azure-event-hub"></a><span data-ttu-id="72b49-120">Varför skicka tooan Azure Event Hub?</span><span class="sxs-lookup"><span data-stu-id="72b49-120">Why send tooan Azure Event Hub?</span></span>
<span data-ttu-id="72b49-121">Det är en rimlig tooask, varför ska man skapa en princip som är specifika tooAzure Händelsehubbar?</span><span class="sxs-lookup"><span data-stu-id="72b49-121">It is a reasonable tooask, why create a policy that is specific tooAzure Event Hubs?</span></span> <span data-ttu-id="72b49-122">Det finns många olika ställen där jag kanske toolog Mina förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="72b49-122">There are many different places where I might want toolog my requests.</span></span> <span data-ttu-id="72b49-123">Varför inte bara skicka hello begäranden direkt toohello slutdestinationen?</span><span class="sxs-lookup"><span data-stu-id="72b49-123">Why not just send hello requests directly toohello final destination?</span></span>  <span data-ttu-id="72b49-124">Det är ett alternativ.</span><span class="sxs-lookup"><span data-stu-id="72b49-124">That is an option.</span></span> <span data-ttu-id="72b49-125">Men vid av loggningsbegäranden från en API management-tjänsten, är det nödvändigt tooconsider hur meddelandeloggning påverkar hello prestanda för hello API.</span><span class="sxs-lookup"><span data-stu-id="72b49-125">However, when making logging requests from an API management service, it is necessary tooconsider how logging messages will impact hello performance of hello API.</span></span> <span data-ttu-id="72b49-126">Stegvis ökning belastning kan hanteras genom att öka tillgängliga instanser av systemkomponenter eller genom att utnyttja geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="72b49-126">Gradual increases in load can be handled by increasing available instances of system components or by taking advantage of geo-replication.</span></span> <span data-ttu-id="72b49-127">Kort toppar i trafik kan dock göra begäranden toobe avsevärt fördröjd om begäranden toologging infrastruktur startar tooslow under belastning.</span><span class="sxs-lookup"><span data-stu-id="72b49-127">However, short spikes in traffic can cause requests toobe significantly delayed if requests toologging infrastructure start tooslow under load.</span></span>

<span data-ttu-id="72b49-128">hello Azure Event Hubs är utformad tooingress stora mängder data, med kapacitet för att hantera ett mycket högre antal händelser än hello antal HTTP-begäranden processen för de flesta API: er.</span><span class="sxs-lookup"><span data-stu-id="72b49-128">hello Azure Event Hubs is designed tooingress huge volumes of data, with capacity for dealing with a far higher number of events than hello number of HTTP requests most APIs process.</span></span> <span data-ttu-id="72b49-129">hello Event Hub fungerar som en typ av avancerade buffert mellan din API-tjänsten och hello hanteringsinfrastruktur som ska lagra och bearbeta hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="72b49-129">hello Event Hub acts as a kind of sophisticated buffer between your API management service and hello infrastructure that will store and process hello messages.</span></span> <span data-ttu-id="72b49-130">Detta säkerställer att din API-prestanda blir inte lidande på grund av toohello loggning infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="72b49-130">This ensures that your API performance will not suffer due toohello logging infrastructure.</span></span>  

<span data-ttu-id="72b49-131">När tooan Händelsehubb har skickats till hello data sparas och väntar på att Event Hub konsumenter tooprocess den.</span><span class="sxs-lookup"><span data-stu-id="72b49-131">Once hello data has been passed tooan Event Hub it is persisted and will wait for Event Hub consumers tooprocess it.</span></span> <span data-ttu-id="72b49-132">hello Event Hub bryr sig inte hur den bearbetas, det är bara mån om huruvida hello-meddelande levereras har.</span><span class="sxs-lookup"><span data-stu-id="72b49-132">hello Event Hub does not care how it will be processed, it just cares about making sure hello message will be successfully delivered.</span></span>     

<span data-ttu-id="72b49-133">Händelsehubbar har hello möjlighet toostream händelser toomultiple konsumentgrupper.</span><span class="sxs-lookup"><span data-stu-id="72b49-133">Event Hubs have hello ability toostream events toomultiple consumer groups.</span></span> <span data-ttu-id="72b49-134">Detta gör att händelser toobe bearbetas av helt olika system.</span><span class="sxs-lookup"><span data-stu-id="72b49-134">This allows events toobe processed by completely different systems.</span></span> <span data-ttu-id="72b49-135">Detta gör att stödja många integrationsscenarier utan att lägga tillägg fördröjningar i hello bearbetning av hello API begäran inom hello API Management-tjänsten eftersom endast en händelse måste toobe genereras.</span><span class="sxs-lookup"><span data-stu-id="72b49-135">This enables supporting many integration scenarios without putting addition delays on hello processing of hello API request within hello API Management service as only one event needs toobe generated.</span></span>

## <a name="a-policy-toosend-applicationhttp-messages"></a><span data-ttu-id="72b49-136">En princip toosend program/HTTP-meddelanden</span><span class="sxs-lookup"><span data-stu-id="72b49-136">A policy toosend application/http messages</span></span>
<span data-ttu-id="72b49-137">En Händelsehubb accepterar händelsedata som en sträng.</span><span class="sxs-lookup"><span data-stu-id="72b49-137">An Event Hub accepts event data as a simple string.</span></span> <span data-ttu-id="72b49-138">hello innehållet i strängen är helt tooyou.</span><span class="sxs-lookup"><span data-stu-id="72b49-138">hello contents of that string are completely up tooyou.</span></span> <span data-ttu-id="72b49-139">toobe kan toopackage upp en HTTP-begäran och skicka ut tooEvent hubbar vi behöver tooformat hello sträng med information om hello begäran eller svar.</span><span class="sxs-lookup"><span data-stu-id="72b49-139">toobe able toopackage up an HTTP request and send it off tooEvent Hubs we need tooformat hello string with hello request or response information.</span></span> <span data-ttu-id="72b49-140">I situationer, om det finns ett befintligt format vi kan återanvända kan sedan vi inte toowrite våra egna parsning kod.</span><span class="sxs-lookup"><span data-stu-id="72b49-140">In situations like this, if there is an existing format we can reuse, then we may not have toowrite our own parsing code.</span></span> <span data-ttu-id="72b49-141">Först I anses vara med hello [HAR](http://www.softwareishard.com/blog/har-12-spec/) för att skicka HTTP-begäranden och -svar.</span><span class="sxs-lookup"><span data-stu-id="72b49-141">Initially I considered using hello [HAR](http://www.softwareishard.com/blog/har-12-spec/) for sending HTTP requests and responses.</span></span> <span data-ttu-id="72b49-142">Men är det här formatet optimerad för att lagra en sekvens av HTTP-begäranden i ett baserade JSON-format.</span><span class="sxs-lookup"><span data-stu-id="72b49-142">However, this format is optimized for storing a sequence of HTTP requests in a JSON based format.</span></span> <span data-ttu-id="72b49-143">Det finns ett antal obligatoriska element som lagts till onödiga komplexitet hello scenariot att skicka hello HTTP-meddelande via hello-överföring.</span><span class="sxs-lookup"><span data-stu-id="72b49-143">It contained a number of mandatory elements that added unnecessary complexity for hello scenario of passing hello HTTP message over hello wire.</span></span>  

<span data-ttu-id="72b49-144">Ett alternativ har toouse hello `application/http` medietyp enligt beskrivningen i hello HTTP-specifikationen [RFC 7230](http://tools.ietf.org/html/rfc7230).</span><span class="sxs-lookup"><span data-stu-id="72b49-144">An alternative option was toouse hello `application/http` media type as described in hello HTTP specification [RFC 7230](http://tools.ietf.org/html/rfc7230).</span></span> <span data-ttu-id="72b49-145">Den här medietypen använder hello exakt samma som format används tooactually skicka HTTP-meddelanden via hello överföring, men hela hello-meddelande kan placeras i ett annat HTTP-begäran hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="72b49-145">This media type uses hello exact same format that is used tooactually send HTTP messages over hello wire, but hello entire message can be put in hello body of another HTTP request.</span></span> <span data-ttu-id="72b49-146">I vårt fall vi bara toouse hello brödtext som våra meddelandet toosend tooEvent Hubs.</span><span class="sxs-lookup"><span data-stu-id="72b49-146">In our case we are just going toouse hello body as our message toosend tooEvent Hubs.</span></span> <span data-ttu-id="72b49-147">Det är enkelt, en tolk som finns i [Microsoft ASP.NET Web API 2.2 Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) bibliotek som kan tolka detta format och konvertera till hello intern `HttpRequestMessage` och `HttpResponseMessage` objekt.</span><span class="sxs-lookup"><span data-stu-id="72b49-147">Conveniently, there is a parser that exists in [Microsoft ASP.NET Web API 2.2 Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) libraries that can parse this format and convert it into hello native `HttpRequestMessage` and `HttpResponseMessage` objects.</span></span>

<span data-ttu-id="72b49-148">toobe kan toocreate det här meddelandet måste tootake nytta av C#-baserade [principuttrycken](https://msdn.microsoft.com/library/azure/dn910913.aspx) i Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="72b49-148">toobe able toocreate this message we need tootake advantage of C# based [Policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure API Management.</span></span> <span data-ttu-id="72b49-149">Här är hello-principen som skickar en HTTP-begäran meddelandet tooAzure Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="72b49-149">Here is hello policy which sends a HTTP request message tooAzure Event Hubs.</span></span>

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

### <a name="policy-declaration"></a><span data-ttu-id="72b49-150">Deklarationen för principen</span><span class="sxs-lookup"><span data-stu-id="72b49-150">Policy declaration</span></span>
<span data-ttu-id="72b49-151">Det några särskilda saker värt att nämna om den här principuttryck.</span><span class="sxs-lookup"><span data-stu-id="72b49-151">There a few particular things worth mentioning about this policy expression.</span></span> <span data-ttu-id="72b49-152">hello loggen till eventhub princip har ett attribut som kallas loggaren-id som refererar toohello namnet på loggaren som har skapats i hello API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="72b49-152">hello log-to-eventhub policy has an attribute called logger-id which refers toohello name of logger that has been created within hello API Management service.</span></span> <span data-ttu-id="72b49-153">information om hur toosetup Event Hub-logg i hello API Management-tjänsten finns i dokumentet hello hello [hur toolog händelser tooAzure Händelsehubbar i Azure API Management](api-management-howto-log-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="72b49-153">hello details of how toosetup an Event Hub logger in hello API Management service can be found in hello document [How toolog events tooAzure Event Hubs in Azure API Management](api-management-howto-log-event-hubs.md).</span></span> <span data-ttu-id="72b49-154">andra hello-attributet är en valfri parameter som instruerar Händelsehubbar vilka partition toostore hello-meddelande i.</span><span class="sxs-lookup"><span data-stu-id="72b49-154">hello second attribute is an optional parameter that instructs Event Hubs which partition toostore hello message in.</span></span> <span data-ttu-id="72b49-155">Händelsehubbar använder partitioner tooenable skalbarhet och kräver minst två.</span><span class="sxs-lookup"><span data-stu-id="72b49-155">Event Hubs use partitions tooenable scalabilty and require a minimum of two.</span></span> <span data-ttu-id="72b49-156">hello sorterade leverans av meddelanden garanterat enbart inom en partition.</span><span class="sxs-lookup"><span data-stu-id="72b49-156">hello ordered delivery of messages is only guaranteed within a partition.</span></span> <span data-ttu-id="72b49-157">Om vi inte att instruera Event Hub i vilken partition tooplace hello-meddelande, använder en algoritm för resursallokering toodistribute hello belastning.</span><span class="sxs-lookup"><span data-stu-id="72b49-157">If we do not instruct Event Hub in which partition tooplace hello message, it will use a round-robin algorithm toodistribute hello load.</span></span> <span data-ttu-id="72b49-158">Dock kan orsaka några av våra toobe för meddelanden som bearbetas i fel ordning.</span><span class="sxs-lookup"><span data-stu-id="72b49-158">However, that may cause some of our messages toobe processed out of order.</span></span>  

### <a name="partitions"></a><span data-ttu-id="72b49-159">Partitioner</span><span class="sxs-lookup"><span data-stu-id="72b49-159">Partitions</span></span>
<span data-ttu-id="72b49-160">tooensure våra meddelanden tooconsumers levereras i ordning och dra nytta av hello belastningen distribution möjligheterna för partitioner, jag har valt toosend HTTP-begäran meddelanden tooone partition och HTTP-svar meddelanden tooa andra partition.</span><span class="sxs-lookup"><span data-stu-id="72b49-160">tooensure our messages are delivered tooconsumers in order and take advantage of hello load distribution capability of partitions, I chose toosend HTTP request messages tooone partition and HTTP response messages tooa second partition.</span></span> <span data-ttu-id="72b49-161">Detta säkerställer att en även belastningsdistribution och vi kan garantera att alla begäranden som kommer att användas i ordning och alla svar används i ordning.</span><span class="sxs-lookup"><span data-stu-id="72b49-161">This will ensure an even load distribution and we can guarantee that all requests will be consumed in order and all responses will be consumed in order.</span></span> <span data-ttu-id="72b49-162">Det är möjligt att ett svar toobe upptog före hello motsvarande begäran, men som inte är ett problem som vi har en annan mekanism för korrelerar begär tooresponses och vi vet att förfrågningar alltid kommer före svar.</span><span class="sxs-lookup"><span data-stu-id="72b49-162">It is possible for a response toobe consumed before hello corresponding request, but as that is not a problem as we have a different mechanism for correlating requests tooresponses and we know that requests always come before responses.</span></span>

### <a name="http-payloads"></a><span data-ttu-id="72b49-163">HTTP-nyttolaster</span><span class="sxs-lookup"><span data-stu-id="72b49-163">HTTP payloads</span></span>
<span data-ttu-id="72b49-164">När du har skapat hello `requestLine` vi kontrollerar toosee om hello begärandetexten trunkeras.</span><span class="sxs-lookup"><span data-stu-id="72b49-164">After building hello `requestLine` we check toosee if hello request body should be truncated.</span></span> <span data-ttu-id="72b49-165">hello frågans brödtext är trunkerat tooonly 1024.</span><span class="sxs-lookup"><span data-stu-id="72b49-165">hello request body is truncated tooonly 1024.</span></span> <span data-ttu-id="72b49-166">Detta kan ökas, men enskilda Event Hub-meddelanden är begränsad too256KB så är det troligt att vissa HTTP-meddelande organ kommer inte att få plats i ett enda meddelande.</span><span class="sxs-lookup"><span data-stu-id="72b49-166">This could be increased, however individual Event Hub messages are limited too256KB, so it is likely that some HTTP message bodies will not fit in a single message.</span></span> <span data-ttu-id="72b49-167">När du gör loggning och analyser mycket information kan härledas från bara hello HTTP-begäran rad och rubriker.</span><span class="sxs-lookup"><span data-stu-id="72b49-167">When doing logging and analytics a significant amount of information can be derived from just hello HTTP request line and headers.</span></span> <span data-ttu-id="72b49-168">Dessutom många API-begäranden endast returnera små organ och så hello förlust av information värde med trunkera stora organ är ganska minimal i jämförelse toohello minskning i överföringen, bearbetning och lagring kostnader tookeep innehållet i meddelandetexten.</span><span class="sxs-lookup"><span data-stu-id="72b49-168">Also, many API requests only return small bodies and so hello loss of information value by truncating large bodies is fairly minimal in comparison toohello reduction in transfer, processing and storage costs tookeep all body contents.</span></span> <span data-ttu-id="72b49-169">En slutlig anteckning om bearbetning hello brödtext är vi behöver toopass `true` toohello som<string>()-metod eftersom vi läser hello brödtext innehåll, men det var även vill hello backend API toobe kan tooread hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="72b49-169">One final note about processing hello body is that we need toopass `true` toohello As<string>() method because we are reading hello body contents, but was also want hello backend API toobe able tooread hello body.</span></span> <span data-ttu-id="72b49-170">Genom att skicka true toothis metoden orsaka vi hello brödtext toobe buffras så att den kan läsa en andra gång.</span><span class="sxs-lookup"><span data-stu-id="72b49-170">By passing true toothis method we cause hello body toobe buffered so that it can be read a second time.</span></span> <span data-ttu-id="72b49-171">Det här är viktigt toobe känna till om du har en API som stöder överföring av stora filer eller använder långt avsökning.</span><span class="sxs-lookup"><span data-stu-id="72b49-171">This is important toobe aware of if you have an API that does uploading of very large files or uses long polling.</span></span> <span data-ttu-id="72b49-172">I dessa fall skulle det vara bästa tooavoid läsning av hello brödtext.</span><span class="sxs-lookup"><span data-stu-id="72b49-172">In these cases it would be best tooavoid reading hello body at all.</span></span>   

### <a name="http-headers"></a><span data-ttu-id="72b49-173">HTTP-huvuden</span><span class="sxs-lookup"><span data-stu-id="72b49-173">HTTP headers</span></span>
<span data-ttu-id="72b49-174">HTTP-huvuden kan överföras bara via till hello meddelandeformat i formatet enkla nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="72b49-174">HTTP Headers can be simply transferred over into hello message format in a simple key/value pair format.</span></span> <span data-ttu-id="72b49-175">Vi har valt toostrip ut vissa tidskritiska fält, tooavoid i onödan läcka autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="72b49-175">We have chosen toostrip out certain security sensitive fields, tooavoid unnecessarily leaking credential information.</span></span> <span data-ttu-id="72b49-176">Det är inte troligt att API-nycklar och andra autentiseringsuppgifter används för analytics ändamål.</span><span class="sxs-lookup"><span data-stu-id="72b49-176">It is unlikely that API keys and other credentials would be used for analytics purposes.</span></span> <span data-ttu-id="72b49-177">Om vi vill toodo analys på hello användar- och hello viss produkt de använder och vi kan hämta som från hello `context` objekt och lägger till toohello meddelandet.</span><span class="sxs-lookup"><span data-stu-id="72b49-177">If we wish toodo analysis on hello user and hello particular product they are using then we could get that from hello `context` object and add that toohello message.</span></span>     

### <a name="message-metadata"></a><span data-ttu-id="72b49-178">Meddelandets Metadata</span><span class="sxs-lookup"><span data-stu-id="72b49-178">Message Metadata</span></span>
<span data-ttu-id="72b49-179">När du skapar hello slutfört toosend toohello händelsehubb hello första raden är inte en del av hello `application/http` meddelande.</span><span class="sxs-lookup"><span data-stu-id="72b49-179">When building hello complete message toosend toohello event hub, hello first line is not actually part of hello `application/http` message.</span></span> <span data-ttu-id="72b49-180">hello första raden är ytterligare metadata som består av om hello-meddelande är ett meddelande i begäran eller svar och ett meddelande-id som används toocorrelate begär tooresponses.</span><span class="sxs-lookup"><span data-stu-id="72b49-180">hello first line is additional metadata consisting of whether hello message is a request or response message and a message id which is used toocorrelate requests tooresponses.</span></span> <span data-ttu-id="72b49-181">hello-meddelande-id skapas med hjälp av en annan princip som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="72b49-181">hello message id is created by using another policy that looks like this:</span></span>

```xml
<set-variable name="message-id" value="@(Guid.NewGuid())" />
```

<span data-ttu-id="72b49-182">Vi kan ha skapats hello begärandemeddelandet lagras att i en variabel tills hello svar var returneras och sedan bara skickas hello förfrågan och svar som ett enda meddelande.</span><span class="sxs-lookup"><span data-stu-id="72b49-182">We could have created hello request message, stored that in a variable until hello response was returned and then simply sent hello request and response as a single message.</span></span> <span data-ttu-id="72b49-183">Hej dock två genom att skicka hello förfrågan och svar oberoende av varandra och med hjälp av ett meddelande-id toocorrelate, vi få lite mer flexibilitet i hello meddelandestorlek, hello möjlighet tootake nytta av flera partitioner med bibehållna meddelandet ordning och hello begäran att visas i vår loggning instrumentpanelen snabbare.</span><span class="sxs-lookup"><span data-stu-id="72b49-183">However, by sending hello request and response independently and using a message id toocorrelate hello two, we get a bit more flexibility in hello message size, hello ability tootake advantage of multiple partitions whilst maintaining message order and hello request will appear in our logging dashboard sooner.</span></span> <span data-ttu-id="72b49-184">Det kan också finnas vissa scenarier där ett giltigt svar skickas aldrig toohello händelsehubb, möjligen på grund av tooa allvarligt begäran fel i hello API Management-tjänsten, men vi kommer fortfarande ha en post för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="72b49-184">There also may be some scenarios where a valid response is never sent toohello event hub, possibly due tooa fatal request error in hello API Management service, but we still will have a record of hello request.</span></span>

<span data-ttu-id="72b49-185">hello princip toosend hello HTTP-svarsmeddelandet ser ut ungefär toohello begäran och genomsökningsåtgärden hello principkonfiguration ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="72b49-185">hello policy toosend hello response HTTP message looks very similar toohello request and so hello complete policy configuration looks like this:</span></span>

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

<span data-ttu-id="72b49-186">Hej `set-variable` princip skapar ett värde som kan nås av både hello `log-to-eventhub` princip i hello `<inbound>` avsnittet och hello `<outbound>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72b49-186">hello `set-variable` policy creates a value that is accessible by both hello `log-to-eventhub` policy in hello `<inbound>` section and hello `<outbound>` section.</span></span>  

## <a name="receiving-events-from-event-hubs"></a><span data-ttu-id="72b49-187">Ta emot händelser från Event Hubs</span><span class="sxs-lookup"><span data-stu-id="72b49-187">Receiving events from Event Hubs</span></span>
<span data-ttu-id="72b49-188">Händelser från Azure Event Hub tas emot med hello [AMQP protokollet](http://www.amqp.org/).</span><span class="sxs-lookup"><span data-stu-id="72b49-188">Events from Azure Event Hub are received using hello [AMQP protocol](http://www.amqp.org/).</span></span> <span data-ttu-id="72b49-189">hello Microsoft Service Bus-teamet har gjort klienten bibliotek tillgängliga toomake hello förbrukar händelser enklare.</span><span class="sxs-lookup"><span data-stu-id="72b49-189">hello Microsoft Service Bus team have made client libraries available toomake hello consuming events easier.</span></span> <span data-ttu-id="72b49-190">Det finns två olika metoder som stöds, ett används en *direkt konsumenten* och hello andra använder hello `EventProcessorHost` klass.</span><span class="sxs-lookup"><span data-stu-id="72b49-190">There are two different approaches supported, one is being a *Direct Consumer* and hello other is using hello `EventProcessorHost` class.</span></span> <span data-ttu-id="72b49-191">Exempel på dessa två metoder kan hittas i hello [Programmeringsguide för Event Hubs](../event-hubs/event-hubs-programming-guide.md).</span><span class="sxs-lookup"><span data-stu-id="72b49-191">Examples of these two approaches can be found in hello [Event Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).</span></span> <span data-ttu-id="72b49-192">hello korta versionen av hello skillnaderna är `Direct Consumer` ger dig fullständig kontroll och hello `EventProcessorHost` har vissa delar av hello mekanik arbete för du men görs vissa antaganden om hur du bearbetar dessa händelser.</span><span class="sxs-lookup"><span data-stu-id="72b49-192">hello short version of hello differences is, `Direct Consumer` gives you complete control and hello `EventProcessorHost` does some of hello plumbing work for you but makes certain assumptions about how you will process those events.</span></span>  

### <a name="eventprocessorhost"></a><span data-ttu-id="72b49-193">EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="72b49-193">EventProcessorHost</span></span>
<span data-ttu-id="72b49-194">I det här exemplet ska vi använda hello `EventProcessorHost` för enkelhetens skull, men den kan inte hello bäst för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="72b49-194">In this sample, we will use hello `EventProcessorHost` for simplicity, however it may not hello best choice for this particular scenario.</span></span> <span data-ttu-id="72b49-195">`EventProcessorHost`hello arbete ser till att du inte har tooworry om trådmodell problem inom en viss händelse processor-klass.</span><span class="sxs-lookup"><span data-stu-id="72b49-195">`EventProcessorHost` does hello hard work of making sure you don't have tooworry about threading issues within a particular event processor class.</span></span> <span data-ttu-id="72b49-196">I vårt scenario, är vi dock bara konvertera hello meddelandeformat tooanother och passerar den längs tooanother tjänst med en async-metod.</span><span class="sxs-lookup"><span data-stu-id="72b49-196">However, in our scenario, we are simply converting hello message tooanother format and passing it along tooanother service using an async method.</span></span> <span data-ttu-id="72b49-197">Det behövs ingen för att uppdatera delade tillstånd och därför ingen risk för trådmodell problem.</span><span class="sxs-lookup"><span data-stu-id="72b49-197">There is no need for updating shared state and therefore no risk of threading issues.</span></span> <span data-ttu-id="72b49-198">I de flesta scenarier `EventProcessorHost` är förmodligen hello bäst och det är visserligen hello enklare alternativ.</span><span class="sxs-lookup"><span data-stu-id="72b49-198">For most scenarios, `EventProcessorHost` is probably hello best choice and it is certainly hello easier option.</span></span>     

### <a name="ieventprocessor"></a><span data-ttu-id="72b49-199">IEventProcessor</span><span class="sxs-lookup"><span data-stu-id="72b49-199">IEventProcessor</span></span>
<span data-ttu-id="72b49-200">hello centrala begrepp när du använder `EventProcessorHost` är toocreate en en implementering av hello `IEventProcessor` gränssnitt som innehåller hello metoden `ProcessEventAsync`.</span><span class="sxs-lookup"><span data-stu-id="72b49-200">hello central concept when using `EventProcessorHost` is toocreate a an implementation of hello `IEventProcessor` interface which contains hello method `ProcessEventAsync`.</span></span> <span data-ttu-id="72b49-201">hello grunden för metoden visas här:</span><span class="sxs-lookup"><span data-stu-id="72b49-201">hello essence of that method is shown here:</span></span>

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

<span data-ttu-id="72b49-202">En lista över EventData objekt har skickats till hello-metoden och vi iterera över listan.</span><span class="sxs-lookup"><span data-stu-id="72b49-202">A list of EventData objects are passed into hello method and we iterate over that list.</span></span> <span data-ttu-id="72b49-203">hello byte för varje metod parsas till ett HttpMessage objekt och det objektet har skickats tooan instans av IHttpMessageProcessor.</span><span class="sxs-lookup"><span data-stu-id="72b49-203">hello bytes of each method are parsed into a HttpMessage object and that object is passed tooan instance of IHttpMessageProcessor.</span></span>

### <a name="httpmessage"></a><span data-ttu-id="72b49-204">HttpMessage</span><span class="sxs-lookup"><span data-stu-id="72b49-204">HttpMessage</span></span>
<span data-ttu-id="72b49-205">Hej `HttpMessage` instans innehåller tre delar av data:</span><span class="sxs-lookup"><span data-stu-id="72b49-205">hello `HttpMessage` instance contains three pieces of data:</span></span>

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

<span data-ttu-id="72b49-206">Hej `HttpMessage` instans innehåller en `MessageId` GUID som gör att vi tooconnect hello HTTP-begäran toohello motsvarande HTTP-svar och ett booleskt värde som anger om hello-objekt innehåller en instans av en HttpRequestMessage och HttpResponseMessage.</span><span class="sxs-lookup"><span data-stu-id="72b49-206">hello `HttpMessage` instance contains a `MessageId` GUID that allows us tooconnect hello HTTP request toohello corresponding HTTP response and a boolean value that identifies if hello object contains an instance of a HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="72b49-207">Med hjälp av hello inbyggda HTTP-klasser från `System.Net.Http`, jag har tootake kan utnyttja hello `application/http` parsning av kod som ingår i `System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="72b49-207">By using hello built in HTTP classes from `System.Net.Http`, I was able tootake advantage of hello `application/http` parsing code that is included in `System.Net.Http.Formatting`.</span></span>  

### <a name="ihttpmessageprocessor"></a><span data-ttu-id="72b49-208">IHttpMessageProcessor</span><span class="sxs-lookup"><span data-stu-id="72b49-208">IHttpMessageProcessor</span></span>
<span data-ttu-id="72b49-209">Hej `HttpMessage` instans vidarebefordras sedan tooimplementation av `IHttpMessageProcessor` som är ett gränssnitt som jag har skapat toodecouple hello mottagning och tolkning av hello händelse från Azure Event Hub och hello faktiska bearbetningen av den.</span><span class="sxs-lookup"><span data-stu-id="72b49-209">hello `HttpMessage` instance is then forwarded tooimplementation of `IHttpMessageProcessor` which is an interface I created toodecouple hello receiving and interpretation of hello event from Azure Event Hub and hello actual processing of it.</span></span>

## <a name="forwarding-hello-http-message"></a><span data-ttu-id="72b49-210">Vidarebefordran hello HTTP-meddelande</span><span class="sxs-lookup"><span data-stu-id="72b49-210">Forwarding hello HTTP message</span></span>
<span data-ttu-id="72b49-211">För det här exemplet jag valt det vore intressant toopush hello HTTP-begäran via för[Runscope](http://www.runscope.com).</span><span class="sxs-lookup"><span data-stu-id="72b49-211">For this sample, I decided it would be interesting toopush hello HTTP Request over too[Runscope](http://www.runscope.com).</span></span> <span data-ttu-id="72b49-212">Runscope är en molnbaserad tjänst som är specialiserat på HTTP-felsökning, loggning och övervakning.</span><span class="sxs-lookup"><span data-stu-id="72b49-212">Runscope is a cloud based service that specializes in HTTP debugging, logging and monitoring.</span></span> <span data-ttu-id="72b49-213">De har en kostnadsfri nivå så att det är enkelt tootry och ger oss toosee hello HTTP-förfrågningar i realtid passerar genom våra API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="72b49-213">They have a free tier, so it is easy tootry and it allows us toosee hello HTTP requests in real-time flowing through our API Management service.</span></span>

<span data-ttu-id="72b49-214">Hej `IHttpMessageProcessor` implementering ser ut så här,</span><span class="sxs-lookup"><span data-stu-id="72b49-214">hello `IHttpMessageProcessor` implementation looks like this,</span></span>

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

<span data-ttu-id="72b49-215">Det var kan tootake nytta av en [befintliga klientbibliotek för Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) som gör det enkelt toopush `HttpRequestMessage` och `HttpResponseMessage` instanser in till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="72b49-215">I was able tootake advantage of an [existing client library for Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) that makes it easy toopush `HttpRequestMessage` and `HttpResponseMessage` instances up into their service.</span></span> <span data-ttu-id="72b49-216">I ordning hello tooaccess Runscope API du behöver ett konto och en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="72b49-216">In order tooaccess hello Runscope API you will need an account and an API Key.</span></span> <span data-ttu-id="72b49-217">Anvisningar för att hämta en API-nyckel finns i hello [skapa program tooAccess Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) skärmutsändning.</span><span class="sxs-lookup"><span data-stu-id="72b49-217">Instructions for getting an API key can be found in hello [Creating Applications tooAccess Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) screencast.</span></span>

## <a name="complete-sample"></a><span data-ttu-id="72b49-218">Fullständigt exempel</span><span class="sxs-lookup"><span data-stu-id="72b49-218">Complete sample</span></span>
<span data-ttu-id="72b49-219">Hej [källkod](https://github.com/darrelmiller/ApimEventProcessor) och tester för hello exemplet finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="72b49-219">hello [source code](https://github.com/darrelmiller/ApimEventProcessor) and tests for hello sample are on GitHub.</span></span> <span data-ttu-id="72b49-220">Du behöver ett [API Management-tjänsten](api-management-get-started.md), [anslutna Händelsehubben](api-management-howto-log-event-hubs.md), och en [Lagringskonto](../storage/common/storage-create-storage-account.md) toorun hello exempel själv.</span><span class="sxs-lookup"><span data-stu-id="72b49-220">You will need an [API Management Service](api-management-get-started.md), [a connected Event Hub](api-management-howto-log-event-hubs.md), and a [Storage Account](../storage/common/storage-create-storage-account.md) toorun hello sample for yourself.</span></span>   

<span data-ttu-id="72b49-221">hello prov är endast ett enkelt konsolprogram som lyssnar efter händelser som kommer från Event Hub konverterar dem till en `HttpRequestMessage` och `HttpResponseMessage` objekt och vidarebefordrar dem på toohello Runscope API.</span><span class="sxs-lookup"><span data-stu-id="72b49-221">hello sample is just a simple Console application that listens for events coming from Event Hub, converts them into a `HttpRequestMessage` and `HttpResponseMessage` objects and then forwards them on toohello Runscope API.</span></span>

<span data-ttu-id="72b49-222">I följande animerad bild hello, ser du begäran tooan API i hello Developer-portalen, hello konsolen program som visar hello-meddelande tas emot, bearbetas och vidarebefordras och hello förfrågan och svar som visas i hello Runscope trafik Inspector.</span><span class="sxs-lookup"><span data-stu-id="72b49-222">In hello following animated image, you can see a request being made tooan API in hello Developer Portal, hello Console application showing hello message being received, processed and forwarded and then hello request and response showing up in hello Runscope Traffic inspector.</span></span>

![Demonstration av begäran vidarebefordras tooRunscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a><span data-ttu-id="72b49-224">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="72b49-224">Summary</span></span>
<span data-ttu-id="72b49-225">Azure API Management-tjänsten tillhandahåller en användbar toocapture hello HTTP-trafik färdas tooand från dina API: er.</span><span class="sxs-lookup"><span data-stu-id="72b49-225">Azure API Management service provides an ideal place toocapture hello HTTP traffic travelling tooand from your APIs.</span></span> <span data-ttu-id="72b49-226">Händelsehubbar i Azure är en mycket skalbar, billig lösning för att samla in trafiken och matar in sekundär bearbetningssystem för loggning, övervakning och andra avancerade analyser.</span><span class="sxs-lookup"><span data-stu-id="72b49-226">Azure Event Hubs is a highly scalable, low cost solution for capturing that traffic and feeding it into secondary processing systems for logging, monitoring and other sophisticated analytics.</span></span> <span data-ttu-id="72b49-227">Ansluter too3rd part trafik övervakningssystem som Runscope är en enkel som några dussin rader med kod.</span><span class="sxs-lookup"><span data-stu-id="72b49-227">Connecting too3rd party traffic monitoring systems like Runscope is a simple as a few dozen lines of code.</span></span>

## <a name="next-steps"></a><span data-ttu-id="72b49-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72b49-228">Next steps</span></span>
* <span data-ttu-id="72b49-229">Lär dig mer om Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="72b49-229">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="72b49-230">Kom igång med Händelsehubbar i Azure</span><span class="sxs-lookup"><span data-stu-id="72b49-230">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="72b49-231">Ta emot meddelanden med EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="72b49-231">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="72b49-232">Programmeringsguide för Event Hubs</span><span class="sxs-lookup"><span data-stu-id="72b49-232">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="72b49-233">Mer information om API-hantering och Händelsehubbar-integrering</span><span class="sxs-lookup"><span data-stu-id="72b49-233">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="72b49-234">Hur toolog händelser tooAzure Händelsehubbar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="72b49-234">How toolog events tooAzure Event Hubs in Azure API Management</span></span>](api-management-howto-log-event-hubs.md)
  * [<span data-ttu-id="72b49-235">Loggaren enhetsreferens</span><span class="sxs-lookup"><span data-stu-id="72b49-235">Logger entity reference</span></span>](https://msdn.microsoft.com/library/azure/mt592020.aspx)
  * [<span data-ttu-id="72b49-236">principreferens för loggen till eventhub</span><span class="sxs-lookup"><span data-stu-id="72b49-236">log-to-eventhub policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
