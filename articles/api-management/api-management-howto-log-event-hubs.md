---
title: "Logga händelser i Händelsehubbar i Azure API Management | Microsoft Docs"
description: "Lär dig mer om att logga händelser till Händelsehubbar i Azure API Management."
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
ms.openlocfilehash: a310236179677046ec49930b07cfdffdadc37974
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a><span data-ttu-id="55f71-103">Logga händelser i Händelsehubbar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="55f71-103">How to log events to Azure Event Hubs in Azure API Management</span></span>
<span data-ttu-id="55f71-104">Händelsehubbar i Azure är en mycket skalbar tjänst för dataingång som kan mata in miljontals händelser per sekund så att du kan bearbeta och analysera de enorma mängder data som dina anslutna enheter och program producerar.</span><span class="sxs-lookup"><span data-stu-id="55f71-104">Azure Event Hubs is a highly scalable data ingress service that can ingest millions of events per second so that you can process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="55f71-105">Händelsehubbar fungerar som ”ytterdörren” för en händelsepipeline, och när data har samlats in i en händelsehubb, det kan omvandlas och lagras med hjälp av en leverantör av realtidsanalys eller adaptrar för batchbearbetning/lagring.</span><span class="sxs-lookup"><span data-stu-id="55f71-105">Event Hubs acts as the "front door" for an event pipeline, and once data is collected into an event hub, it can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="55f71-106">Händelsehubbar frikopplar produktionen av en händelseström från användningen av dessa händelser så att händelsekonsumenterna kan komma åt dem på sitt eget schema.</span><span class="sxs-lookup"><span data-stu-id="55f71-106">Event Hubs decouples the production of a stream of events from the consumption of those events, so that event consumers can access the events on their own schedule.</span></span>

<span data-ttu-id="55f71-107">Den här artikeln är en medlem i den [integrera Azure API Management med Händelsehubbar](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video och beskriver hur du loggar händelser för API Management med hjälp av Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="55f71-107">This article is a companion to the [Integrate Azure API Management with Event Hubs](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video and describes how to log API Management events using Azure Event Hubs.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="55f71-108">Skapa ett Azure Event Hub</span><span class="sxs-lookup"><span data-stu-id="55f71-108">Create an Azure Event Hub</span></span>
<span data-ttu-id="55f71-109">Om du vill skapa en ny Händelsehubb, logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com) och på **ny**->**Apptjänster**->**Service Bus**  -> **Händelsehubb**->**Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="55f71-109">To create a new Event Hub, sign-in to the [Azure classic portal](https://manage.windowsazure.com) and click **New**->**App Services**->**Service Bus**->**Event Hub**->**Quick Create**.</span></span> <span data-ttu-id="55f71-110">Ange namnet på en Händelsehubb region, väljer en prenumeration och ett namnområde.</span><span class="sxs-lookup"><span data-stu-id="55f71-110">Enter an Event Hub name, region, select a subscription, and select a namespace.</span></span> <span data-ttu-id="55f71-111">Om du inte har skapat ett namnområde kan du skapa en genom att skriva ett namn i den **Namespace** textruta.</span><span class="sxs-lookup"><span data-stu-id="55f71-111">If you haven't previously created a namespace you can create one by typing a name in the **Namespace** textbox.</span></span> <span data-ttu-id="55f71-112">När alla egenskaper har konfigurerats, klickar du på **skapa en ny Händelsehubb** att skapa Händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="55f71-112">Once all properties are configured, click **Create a new Event Hub** to create the Event Hub.</span></span>

![Skapa händelsehubb][create-event-hub]

<span data-ttu-id="55f71-114">Gå sedan till den **konfigurera** för din nya Event Hub och skapa två **delade åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="55f71-114">Next, navigate to the **Configure** tab for your new Event Hub and create two **shared access policies**.</span></span> <span data-ttu-id="55f71-115">Namn på det första **sändande** och ge den **skicka** behörigheter.</span><span class="sxs-lookup"><span data-stu-id="55f71-115">Name the first one **Sending** and give it **Send** permissions.</span></span>

![Skicka princip][sending-policy]

<span data-ttu-id="55f71-117">Namn på det andra **tar emot**, ger det **lyssna** behörigheter och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="55f71-117">Name the second one **Receiving**, give it **Listen** permissions, and click **Save**.</span></span>

![Emot princip][receiving-policy]

<span data-ttu-id="55f71-119">Varje princip för delad åtkomst gör att program kan skicka och ta emot händelser till och från Event Hub.</span><span class="sxs-lookup"><span data-stu-id="55f71-119">Each shared access policy allows applications to send and receive events to and from the Event Hub.</span></span> <span data-ttu-id="55f71-120">Om du vill komma åt anslutningssträngar för dessa principer, navigera till den **instrumentpanelen** Event Hub och klicka på fliken **anslutningsinformationen**.</span><span class="sxs-lookup"><span data-stu-id="55f71-120">To access the connection strings for these policies, navigate to the **Dashboard** tab of the Event Hub and click **Connection information**.</span></span>

![Anslutningssträng][event-hub-dashboard]

<span data-ttu-id="55f71-122">Den **sändande** anslutningssträngen som används när du loggar händelser, och **tar emot** anslutningssträngen som används när du hämtar händelser från Event Hub.</span><span class="sxs-lookup"><span data-stu-id="55f71-122">The **Sending** connection string is used when logging events, and the **Receiving** connection string is used when downloading events from the Event Hub.</span></span>

![Anslutningssträng][event-hub-connection-string]

## <a name="create-an-api-management-logger"></a><span data-ttu-id="55f71-124">Skapa en API Management-logg</span><span class="sxs-lookup"><span data-stu-id="55f71-124">Create an API Management logger</span></span>
<span data-ttu-id="55f71-125">Nu när du har en Händelsehubb nästa steg är att konfigurera en [loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) i API Management-tjänsten så att den kan logga händelser till Händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="55f71-125">Now that you have an Event Hub, the next step is to configure a [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) in your API Management service so that it can log events to the Event Hub.</span></span>

<span data-ttu-id="55f71-126">API Management loggare konfigureras med hjälp av den [API Management REST API](http://aka.ms/smapi).</span><span class="sxs-lookup"><span data-stu-id="55f71-126">API Management loggers are configured using the [API Management REST API](http://aka.ms/smapi).</span></span> <span data-ttu-id="55f71-127">Innan du använder REST API för första gången, granska den [krav](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) och se till att du har [aktiverat åtkomst till REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span><span class="sxs-lookup"><span data-stu-id="55f71-127">Before using the REST API for the first time, review the [prerequisites](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) and ensure that you have [enabled access to the REST API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).</span></span>

<span data-ttu-id="55f71-128">Gör en HTTP PUT-begäran med hjälp av följande URL: en mall för att skapa en logg.</span><span class="sxs-lookup"><span data-stu-id="55f71-128">To create a logger, make an HTTP PUT request using the following URL template.</span></span>

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2014-02-14-preview`

* <span data-ttu-id="55f71-129">Ersätt `{your service}` med namnet på din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="55f71-129">Replace `{your service}` with the name of your API Management service instance.</span></span>
* <span data-ttu-id="55f71-130">Ersätt `{new logger name}` med önskat namn för din nya meddelandeloggfiler.</span><span class="sxs-lookup"><span data-stu-id="55f71-130">Replace `{new logger name}` with the desired name for your new logger.</span></span> <span data-ttu-id="55f71-131">Du sedan hänvisar till det här namnet när du konfigurerar den [loggen till eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) princip</span><span class="sxs-lookup"><span data-stu-id="55f71-131">You will reference this name when you configure the [log-to-eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) policy</span></span>

<span data-ttu-id="55f71-132">Lägg till följande huvuden i begäran.</span><span class="sxs-lookup"><span data-stu-id="55f71-132">Add the following headers to the request.</span></span>

* <span data-ttu-id="55f71-133">Content-Type: application/json</span><span class="sxs-lookup"><span data-stu-id="55f71-133">Content-Type : application/json</span></span>
* <span data-ttu-id="55f71-134">Auktorisering: SharedAccessSignature 58...</span><span class="sxs-lookup"><span data-stu-id="55f71-134">Authorization : SharedAccessSignature 58...</span></span>
  * <span data-ttu-id="55f71-135">Anvisningar för att generera den `SharedAccessSignature` finns [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span><span class="sxs-lookup"><span data-stu-id="55f71-135">For instructions on generating the `SharedAccessSignature` see [Azure API Management REST API Authentication](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).</span></span>

<span data-ttu-id="55f71-136">Ange text på begäran med hjälp av följande mall.</span><span class="sxs-lookup"><span data-stu-id="55f71-136">Specify the request body using the following template.</span></span>

```json
{
  "type" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* <span data-ttu-id="55f71-137">`type`måste anges till `AzureEventHub`.</span><span class="sxs-lookup"><span data-stu-id="55f71-137">`type` must be set to `AzureEventHub`.</span></span>
* <span data-ttu-id="55f71-138">`description`ger en valfri beskrivning av loggaren och kan vara en tom sträng om så önskas.</span><span class="sxs-lookup"><span data-stu-id="55f71-138">`description` provides an optional description of the logger and can be a zero length string if desired.</span></span>
* <span data-ttu-id="55f71-139">`credentials`innehåller den `name` och `connectionString` för din Azure-Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="55f71-139">`credentials` contains the `name` and `connectionString` of your Azure Event Hub.</span></span>

<span data-ttu-id="55f71-140">När du gör begäran om loggaren skapas en statuskod för `201 Created` returneras.</span><span class="sxs-lookup"><span data-stu-id="55f71-140">When you make the request, if the logger is created a status code of `201 Created` is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="55f71-141">Andra möjliga returkoder och skälen finns [skapa en loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span><span class="sxs-lookup"><span data-stu-id="55f71-141">For other possible return codes and their reasons, see [Create a Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT).</span></span> <span data-ttu-id="55f71-142">Att se hur utföra andra åtgärder, till exempel listan, uppdatera och ta bort, finns det [loggaren](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) dokumentationen för enheten.</span><span class="sxs-lookup"><span data-stu-id="55f71-142">To see how perform other operations such as list, update, and delete, see the [Logger](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entity documentation.</span></span>
>
>

## <a name="configure-log-to-eventhubs-policies"></a><span data-ttu-id="55f71-143">Konfigurera principer för logg-eventhubs</span><span class="sxs-lookup"><span data-stu-id="55f71-143">Configure log-to-eventhubs policies</span></span>
<span data-ttu-id="55f71-144">Du kan konfigurera loggen till eventhubs principer till önskade logghändelser när du har konfigurerat din loggaren i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="55f71-144">Once your logger is configured in API Management, you can configure your log-to-eventhubs policies to log the desired events.</span></span> <span data-ttu-id="55f71-145">Log-eventhubs principen kan användas i avsnittet princip för inkommande eller utgående princip-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="55f71-145">The log-to-eventhubs policy can be used in either the inbound policy section or the outbound policy section.</span></span>

<span data-ttu-id="55f71-146">För att konfigurera principer, logga in på den [Azure-portalen](https://portal.azure.com), navigera till din API Management-tjänst och klicka på **Publisher portal** åt publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="55f71-146">To configure policies, sign-in to the [Azure portal](https://portal.azure.com), navigate to your API Management service, and click **Publisher portal** to access the publisher portal.</span></span>

![Utgivarportalen][publisher-portal]

<span data-ttu-id="55f71-148">Klicka på **principer** Markera önskad produkt och API i API Management-menyn till vänster och klicka på **Lägg till princip**.</span><span class="sxs-lookup"><span data-stu-id="55f71-148">Click **Policies** in the API Management menu on the left, select the desired product and API, and click **Add policy**.</span></span> <span data-ttu-id="55f71-149">I det här exemplet lägger vi till en princip för att den **Echo API** i den **obegränsad** produkten.</span><span class="sxs-lookup"><span data-stu-id="55f71-149">In this example we're adding a policy to the **Echo API** in the **Unlimited** product.</span></span>

![Lägg till princip][add-policy]

<span data-ttu-id="55f71-151">Placera markören i den `inbound` princip för och klicka på den **loggen till EventHub** princip för att infoga den `log-to-eventhub` Principmall för instruktionen.</span><span class="sxs-lookup"><span data-stu-id="55f71-151">Position your cursor in the `inbound` policy section and click the **Log to EventHub** policy to insert the `log-to-eventhub` policy statement template.</span></span>

![Principredigerare][event-hub-policy]

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```

<span data-ttu-id="55f71-153">Ersätt `logger-id` med namnet på API Management-logg som du konfigurerade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="55f71-153">Replace `logger-id` with the name of the API Management logger you configured in the previous step.</span></span>

<span data-ttu-id="55f71-154">Du kan använda ett uttryck som returnerar en sträng som värde för den `log-to-eventhub` element.</span><span class="sxs-lookup"><span data-stu-id="55f71-154">You can use any expression that returns a string as the value for the `log-to-eventhub` element.</span></span> <span data-ttu-id="55f71-155">En sträng som innehåller datum och tid, tjänstnamn, förfrågnings-id, ip-adressen för begäran och åtgärdsnamn loggas i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="55f71-155">In this example a string containing the date and time, service name, request id, request ip address, and operation name is logged.</span></span>

<span data-ttu-id="55f71-156">Klicka på **spara** att spara den uppdaterade principkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="55f71-156">Click **Save** to save the updated policy configuration.</span></span> <span data-ttu-id="55f71-157">Så snart den är i sparat principen är aktiv och händelser som loggas till avsedda Händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="55f71-157">As soon as it is saved the policy is active and events are logged to the designated Event Hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="55f71-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55f71-158">Next steps</span></span>
* <span data-ttu-id="55f71-159">Lär dig mer om Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="55f71-159">Learn more about Azure Event Hubs</span></span>
  * [<span data-ttu-id="55f71-160">Kom igång med Händelsehubbar i Azure</span><span class="sxs-lookup"><span data-stu-id="55f71-160">Get started with Azure Event Hubs</span></span>](../event-hubs/event-hubs-c-getstarted-send.md)
  * [<span data-ttu-id="55f71-161">Ta emot meddelanden med EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="55f71-161">Receive messages with EventProcessorHost</span></span>](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [<span data-ttu-id="55f71-162">Programmeringsguide för Event Hubs</span><span class="sxs-lookup"><span data-stu-id="55f71-162">Event Hubs programming guide</span></span>](../event-hubs/event-hubs-programming-guide.md)
* <span data-ttu-id="55f71-163">Mer information om API-hantering och Händelsehubbar-integrering</span><span class="sxs-lookup"><span data-stu-id="55f71-163">Learn more about API Management and Event Hubs integration</span></span>
  * [<span data-ttu-id="55f71-164">Loggaren enhetsreferens</span><span class="sxs-lookup"><span data-stu-id="55f71-164">Logger entity reference</span></span>](https://docs.microsoft.com/rest/api/apimanagement/loggers)
  * [<span data-ttu-id="55f71-165">principreferens för loggen till eventhub</span><span class="sxs-lookup"><span data-stu-id="55f71-165">log-to-eventhub policy reference</span></span>](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [<span data-ttu-id="55f71-166">Övervaka dina API: er med Azure API Management, Händelsehubbar och Runscope</span><span class="sxs-lookup"><span data-stu-id="55f71-166">Monitor your APIs with Azure API Management, Event Hubs and Runscope</span></span>](api-management-log-to-eventhub-sample.md)    

## <a name="watch-a-video-walkthrough"></a><span data-ttu-id="55f71-167">Titta på en videogenomgång</span><span class="sxs-lookup"><span data-stu-id="55f71-167">Watch a video walkthrough</span></span>
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
