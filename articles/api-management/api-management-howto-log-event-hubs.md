---
title: "aaaHow toolog händelser tooAzure Händelsehubbar i Azure API Management | Microsoft Docs"
description: "Lär dig hur toolog händelser tooAzure Händelsehubbar i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 09ca65fc48a874467c6662858f7594e9b19fcdb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toolog-events-tooazure-event-hubs-in-azure-api-management"></a><span data-ttu-id="a8e92-103">Hur toolog händelser tooAzure Händelsehubbar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a8e92-103">How toolog events tooAzure Event Hubs in Azure API Management</span></span>
<span data-ttu-id="a8e92-104">Händelsehubbar i Azure är en mycket skalbar tjänst för dataingång som kan mata in miljontals händelser per sekund så att du kan bearbeta och analysera hello stora mängder data som produceras av dina anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="a8e92-104">Azure Event Hubs is a highly scalable data ingress service that can ingest millions of events per second so that you can process and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="a8e92-105">Händelsehubbar fungerar som hello ”ytterdörren” för en händelsepipeline, och när data har samlats in i en händelsehubb, det kan omvandlas och lagras med hjälp av en leverantör av realtidsanalys eller adaptrar för batchbearbetning/lagring.</span><span class="sxs-lookup"><span data-stu-id="a8e92-105">Event Hubs acts as hello "front door" for an event pipeline, and once data is collected into an event hub, it can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="a8e92-106">Händelsehubbar frikopplar hello framställning av en dataström med händelser från hello användningen av dessa händelser så att händelsekonsumenterna kan komma åt hello händelser på sitt eget schema.</span><span class="sxs-lookup"><span data-stu-id="a8e92-106">Event Hubs decouples hello production of a stream of events from hello consumption of those events, so that event consumers can access hello events on their own schedule.</span></span>

<span data-ttu-id="a8e92-107">Den här artikeln är en tillhörande toohello [integrera Azure API Management med Händelsehubbar](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video och beskriver hur toolog API Management händelser med hjälp av Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="a8e92-107">This article is a companion toohello [Integrate Azure API Management with Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video and describes how toolog API Management events using Azure Event Hubs.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="a8e92-108">Skapa ett Azure Event Hub</span><span class="sxs-lookup"><span data-stu-id="a8e92-108">Create an Azure Event Hub</span></span>
<span data-ttu-id="a8e92-109">toocreate en ny Händelsehubb inloggning toohello [klassiska Azure-portalen](https://manage.windowsazure.com) och på **ny**->**Apptjänster**->**Service Bus**  -> **Händelsehubb**->**Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="a8e92-109">toocreate a new Event Hub, sign-in toohello [Azure classic portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Service Bus**->**Event Hub**->**Quick Create**.</span></span> <span data-ttu-id="a8e92-110">Ange namnet på en Händelsehubb region, väljer en prenumeration och ett namnområde.</span><span class="sxs-lookup"><span data-stu-id="a8e92-110">Enter an Event Hub name, region, select a subscription, and select a namespace.</span></span> <span data-ttu-id="a8e92-111">Om du inte tidigare har skapat ett namnområde kan du skapa en genom att skriva ett namn i hello **Namespace** textruta.</span><span class="sxs-lookup"><span data-stu-id="a8e92-111">If you haven't previously created a namespace you can create one by typing a name in hello **Namespace** textbox.</span></span> <span data-ttu-id="a8e92-112">När alla egenskaper har konfigurerats, klickar du på **skapa en ny Händelsehubb** toocreate hello Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a8e92-112">Once all properties are configured, click **Create a new Event Hub** toocreate hello Event Hub.</span></span>

![Skapa händelsehubb][create-event-hub]

<span data-ttu-id="a8e92-114">Nu ska du navigera toohello **konfigurera** för din nya Event Hub och skapa två **delade åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="a8e92-114">Next, navigate toohello **Configure** tab for your new Event Hub and create two **shared access policies**.</span></span> <span data-ttu-id="a8e92-115">Namnge hello första **sändande** och ge den **skicka** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="a8e92-115">Name hello first one **Sending** and give it **Send** permissions.</span></span>

![Skicka princip][sending-policy]

<span data-ttu-id="a8e92-117">Namnge hello andra **tar emot**, ger det **lyssna** behörigheter och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="a8e92-117">Name hello second one **Receiving**, give it **Listen** permissions, and click **Save**.</span></span>

![Emot princip][receiving-policy]

<span data-ttu-id="a8e92-119">Varje princip för delad åtkomst kan program toosend och ta emot händelser tooand från hello Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a8e92-119">Each shared access policy allows applications toosend and receive events tooand from hello Event Hub.</span></span> <span data-ttu-id="a8e92-120">tooaccess hello-anslutningssträngar för dessa principer navigera toohello **instrumentpanelen** hello Event Hub och klicka på fliken **anslutningsinformationen**.</span><span class="sxs-lookup"><span data-stu-id="a8e92-120">tooaccess hello connection strings for these policies, navigate toohello **Dashboard** tab of hello Event Hub and click **Connection information**.</span></span>

![Anslutningssträng][event-hub-dashboard]

<span data-ttu-id="a8e92-122">Hej **sändande** anslutningssträngen som används när du loggar händelser, och hello **tar emot** anslutningssträngen som används när du hämtar händelser från hello Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a8e92-122">hello **Sending** connection string is used when logging events, and hello **Receiving** connection string is used when downloading events from hello Event Hub.</span></span>

![Anslutningssträng][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a><span data-ttu-id="a8e92-124">Skapa en API Management-logg</span><span class="sxs-lookup"><span data-stu-id="a8e92-124">Create an API Management logger</span></span>
<span data-ttu-id="a8e92-125">Nu när du har en Händelsehubb hello nästa steg är tooconfigure en [loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) i API Management-tjänsten så att den kan logga händelser toohello Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a8e92-125">Now that you have an Event Hub, hello next step is tooconfigure a [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) in your API Management service so that it can log events toohello Event Hub.</span></span>

<span data-ttu-id="a8e92-126">API Management loggare konfigureras med hjälp av hello [API Management REST API](http://aka.ms/smapi).</span><span class="sxs-lookup"><span data-stu-id="a8e92-126">API Management loggers are configured using hello [API Management REST API](http://aka.ms/smapi).</span></span> <span data-ttu-id="a8e92-127">Innan du använder hello REST API för hello första gången, granska hello [krav](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) och se till att du har [aktiverat åtkomst toohello REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span><span class="sxs-lookup"><span data-stu-id="a8e92-127">Before using hello REST API for hello first time, review hello [prerequisites](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) and ensure that you have [enabled access toohello REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span></span>

<span data-ttu-id="a8e92-128">toocreate loggaren, gör ett HTTP PUT-begäran som använder hello följande URL: en mall.</span><span class="sxs-lookup"><span data-stu-id="a8e92-128">toocreate a logger, make an HTTP PUT request using hello following URL template.</span></span>

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* <span data-ttu-id="a8e92-129">Ersätt `{your service}` med hello namnet på din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="a8e92-129">Replace `{your service}` with hello name of your API Management service instance.</span></span>
* <span data-ttu-id="a8e92-130">Ersätt `{new logger name}` med hello önskat namn för din nya meddelandeloggfiler.</span><span class="sxs-lookup"><span data-stu-id="a8e92-130">Replace `{new logger name}` with hello desired name for your new logger.</span></span> <span data-ttu-id="a8e92-131">Du sedan hänvisar till det här namnet när du konfigurerar hello [loggen till eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) princip</span><span class="sxs-lookup"><span data-stu-id="a8e92-131">You will reference this name when you configure hello [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) policy</span></span>

<span data-ttu-id="a8e92-132">Lägg till hello efter huvuden toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="a8e92-132">Add hello following headers toohello request.</span></span>

* <span data-ttu-id="a8e92-133">Content-Type: application/json</span><span class="sxs-lookup"><span data-stu-id="a8e92-133">Content-Type : application/json</span></span>
* <span data-ttu-id="a8e92-134">Auktorisering: SharedAccessSignature 58...</span><span class="sxs-lookup"><span data-stu-id="a8e92-134">Authorization : SharedAccessSignature 58...</span></span>
  * <span data-ttu-id="a8e92-135">Anvisningar för att generera hello `SharedAccessSignature` finns [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span><span class="sxs-lookup"><span data-stu-id="a8e92-135">For instructions on generating hello `SharedAccessSignature` see [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span></span>

<span data-ttu-id="a8e92-136">Ange hello begärandetexten med hello följande mall.</span><span class="sxs-lookup"><span data-stu-id="a8e92-136">Specify hello request body using hello following template.</span></span>

```json
{
  "type" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of hello Event Hub from hello Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* <span data-ttu-id="a8e92-137">`type`måste anges för`AzureEventHub`.</span><span class="sxs-lookup"><span data-stu-id="a8e92-137">`type` must be set too`AzureEventHub`.</span></span>
* <span data-ttu-id="a8e92-138">`description`ger en valfri beskrivning av hello loggaren och kan vara en tom sträng om så önskas.</span><span class="sxs-lookup"><span data-stu-id="a8e92-138">`description` provides an optional description of hello logger and can be a zero length string if desired.</span></span>
* <span data-ttu-id="a8e92-139">`credentials`innehåller hello `name` och `connectionString` för din Azure-Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="a8e92-139">`credentials` contains hello `name` and `connectionString` of your Azure Event Hub.</span></span>

<span data-ttu-id="a8e92-140">När du gör hello begäran om hello loggaren skapas en statuskod för `201 Created` returneras.</span><span class="sxs-lookup"><span data-stu-id="a8e92-140">When you make hello request, if hello logger is created a status code of `201 Created` is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e92-141">Andra möjliga returkoder och skälen finns [skapa en loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span><span class="sxs-lookup"><span data-stu-id="a8e92-141">For other possible return codes and their reasons, see [Create a Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span></span> <span data-ttu-id="a8e92-142">toosee hur utföra andra åtgärder som finns i listan, uppdatera och ta bort, hello [loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) dokumentationen för enheten.</span><span class="sxs-lookup"><span data-stu-id="a8e92-142">toosee how perform other operations such as list, update, and delete, see hello [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entity documentation.</span></span>
>
>

## <a name="configure-log-to-eventhubs-policies"></a><span data-ttu-id="a8e92-143">Konfigurera principer för logg-eventhubs</span><span class="sxs-lookup"><span data-stu-id="a8e92-143">Configure log-to-eventhubs policies</span></span>
<span data-ttu-id="a8e92-144">När din loggaren har konfigurerats i API-hantering kan konfigurera du din loggen till eventhubs principer toolog hello önskad händelser.</span><span class="sxs-lookup"><span data-stu-id="a8e92-144">Once your logger is configured in API Management, you can configure your log-to-eventhubs policies toolog hello desired events.</span></span> <span data-ttu-id="a8e92-145">hello loggen till eventhubs principen kan användas antingen hello inkommande principavsnitt eller hello utgående principavsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8e92-145">hello log-to-eventhubs policy can be used in either hello inbound policy section or hello outbound policy section.</span></span>

<span data-ttu-id="a8e92-146">tooconfigure principer, logga in toohello [Azure-portalen](https://portal.azure.com), navigera tooyour API Management-tjänsten och klicka på **Publisher portal** tooaccess hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="a8e92-146">tooconfigure policies, sign-in toohello [Azure portal](https://portal.azure.com), navigate tooyour API Management service, and click **Publisher portal** tooaccess hello publisher portal.</span></span>

![Utgivarportalen][publisher-portal]

<span data-ttu-id="a8e92-148">Klicka på **principer** hello API Management menyn hello vänster, Välj önskade hello-produkten och API och klicka **Lägg till princip**.</span><span class="sxs-lookup"><span data-stu-id="a8e92-148">Click **Policies** in hello API Management menu on hello left, select hello desired product and API, and click **Add policy**.</span></span> <span data-ttu-id="a8e92-149">I det här exemplet lägger vi till en princip toohello **Echo API** i hello **obegränsad** produkten.</span><span class="sxs-lookup"><span data-stu-id="a8e92-149">In this example we're adding a policy toohello **Echo API** in hello **Unlimited** product.</span></span>

![Lägg till princip][add-policy]

<span data-ttu-id="a8e92-151">Placera markören i hello `inbound` princip avsnittet och klicka på hello **loggen tooEventHub** princip tooinsert hello `log-to-eventhub` Principmall för instruktionen.</span><span class="sxs-lookup"><span data-stu-id="a8e92-151">Position your cursor in hello `inbound` policy section and click hello **Log tooEventHub** policy tooinsert hello `log-to-eventhub` policy statement template.</span></span>

![Principredigerare][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

<span data-ttu-id="a8e92-153">Ersätt `logger-id` med hello namnet hello API Management-logg som du konfigurerade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="a8e92-153">Replace `logger-id` with hello name of hello API Management logger you configured in hello previous step.</span></span>

<span data-ttu-id="a8e92-154">Du kan använda ett uttryck som returnerar en sträng som värde hello hello `log-to-eventhub` element.</span><span class="sxs-lookup"><span data-stu-id="a8e92-154">You can use any expression that returns a string as hello value for hello `log-to-eventhub` element.</span></span> <span data-ttu-id="a8e92-155">En sträng som innehåller hello datum och klockslag, tjänstnamn, förfrågnings-id, ip-adressen för begäran och åtgärdsnamn loggas i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a8e92-155">In this example a string containing hello date and time, service name, request id, request ip address, and operation name is logged.</span></span>

<span data-ttu-id="a8e92-156">Klicka på **spara** toosave hello uppdateras principkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a8e92-156">Click **Save** toosave hello updated policy configuration.</span></span> <span data-ttu-id="a8e92-157">Så snart den är sparad hello principen är aktiv och händelser är loggade toohello avses Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a8e92-157">As soon as it is saved hello policy is active and events are logged toohello designated Event Hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8e92-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8e92-158">Next steps</span></span>
* <span data-ttu-id="a8e92-159">Lär dig mer om Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a8e92-159">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="a8e92-160">Kom igång med Händelsehubbar i Azure</span><span class="sxs-lookup"><span data-stu-id="a8e92-160">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="a8e92-161">Ta emot meddelanden med EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="a8e92-161">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="a8e92-162">Programmeringsguide för Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a8e92-162">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="a8e92-163">Mer information om API-hantering och Händelsehubbar-integrering</span><span class="sxs-lookup"><span data-stu-id="a8e92-163">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="a8e92-164">Loggaren enhetsreferens</span><span class="sxs-lookup"><span data-stu-id="a8e92-164">Logger entity reference</span></span>](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [<span data-ttu-id="a8e92-165">principreferens för loggen till eventhub</span><span class="sxs-lookup"><span data-stu-id="a8e92-165">log-to-eventhub policy reference</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [<span data-ttu-id="a8e92-166">Övervaka dina API: er med Azure API Management, Händelsehubbar och Runscope</span><span class="sxs-lookup"><span data-stu-id="a8e92-166">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a><span data-ttu-id="a8e92-167">Titta på en videogenomgång</span><span class="sxs-lookup"><span data-stu-id="a8e92-167">Watch a video walkthrough</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Integrate-Azure-API-Management-with-Event-Hubs/player]
>
>

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
