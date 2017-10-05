---
title: "Anpassade händelser för Azure Event Grid | Microsoft Docs"
description: "Använd Azure Event Grid för att publicera ett ämne och prenumerera på händelsen."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 0290836bebadb20085a3ce84dddc088c3af385da
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-route-custom-events-with-azure-event-grid"></a><span data-ttu-id="9a442-103">Skapa och dirigera anpassade händelser med Azure Event Grid</span><span class="sxs-lookup"><span data-stu-id="9a442-103">Create and route custom events with Azure Event Grid</span></span>

<span data-ttu-id="9a442-104">Azure Event Grid är en händelsetjänst för molnet.</span><span class="sxs-lookup"><span data-stu-id="9a442-104">Azure Event Grid is an eventing service for the cloud.</span></span> <span data-ttu-id="9a442-105">I den här artikeln använder du Azure CLI för att skapa ett anpassat ämne, prenumerera på ämnet och utlösa händelsen för att visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="9a442-105">In this article, you use the Azure CLI to create a custom topic, subscribe to the topic, and trigger the event to view the result.</span></span> <span data-ttu-id="9a442-106">Normalt kan du skicka händelser till en slutpunkt som svarar på händelsen, exempelvis en webhook eller Azure Function.</span><span class="sxs-lookup"><span data-stu-id="9a442-106">Typically, you send events to an endpoint that responds to the event, such as, a webhook or Azure Function.</span></span> <span data-ttu-id="9a442-107">Men för att enkelt beskriva den här artikeln kan skicka du händelser till en URL som endast samlar in meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9a442-107">However, to simplify this article, you send the events to a URL that merely collects the messages.</span></span> <span data-ttu-id="9a442-108">Du skapar denna URL med hjälp av en öppen källkod, ett tredjepartsverktyg som kallas [RequestBin](https://requestb.in/).</span><span class="sxs-lookup"><span data-stu-id="9a442-108">You create this URL by using an open source, third-party tool called [RequestBin](https://requestb.in/).</span></span>

>[!NOTE]
><span data-ttu-id="9a442-109">**RequestBin** är ett verktyg med öppen källkod som inte är avsett för användning med högt dataflöde.</span><span class="sxs-lookup"><span data-stu-id="9a442-109">**RequestBin** is an open source tool that is not intended for high throughput usage.</span></span> <span data-ttu-id="9a442-110">Här används verktyget endast i demonstrativt syfte.</span><span class="sxs-lookup"><span data-stu-id="9a442-110">The use of the tool here is purely demonstrative.</span></span> <span data-ttu-id="9a442-111">Om du push-överför fler än en händelse i taget kanske du inte ser alla händelser i verktyget.</span><span class="sxs-lookup"><span data-stu-id="9a442-111">If you push more than one event at a time, you might not see all of your events in the tool.</span></span>

<span data-ttu-id="9a442-112">När du är klar kan se du att händelsedata som har skickats till en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="9a442-112">When you are finished, you see that the event data has been sent to an endpoint.</span></span>

![Händelsedata](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9a442-114">Om du väljer att installera och använda CLI lokalt måste du köra den senaste versionen av Azure CLI (2.0.14 eller senare).</span><span class="sxs-lookup"><span data-stu-id="9a442-114">If you choose to install and use the CLI locally, this article requires that you are running the latest version of Azure CLI (2.0.14 or later).</span></span> <span data-ttu-id="9a442-115">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="9a442-115">To find the version, run `az --version`.</span></span> <span data-ttu-id="9a442-116">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9a442-116">If you need to install or upgrade, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="9a442-117">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="9a442-117">Create a resource group</span></span>

<span data-ttu-id="9a442-118">Event Grid-ämnen är Azure-resurser och måste placeras i en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9a442-118">Event Grid topics are Azure resources, and must be placed in an Azure resource group.</span></span> <span data-ttu-id="9a442-119">Resursgruppen är en logisk samling där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="9a442-119">The resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="9a442-120">Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="9a442-120">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="9a442-121">I följande exempel skapas en resursgrupp med namnet *gridResourceGroup* på platsen *westus2*.</span><span class="sxs-lookup"><span data-stu-id="9a442-121">The following example creates a resource group named *gridResourceGroup* in the *westus2* location.</span></span>

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a><span data-ttu-id="9a442-122">Skapa en anpassat ämne</span><span class="sxs-lookup"><span data-stu-id="9a442-122">Create a custom topic</span></span>

<span data-ttu-id="9a442-123">Ett ämne ger en användardefinierad slutpunkt där du publicerar dina händelser.</span><span class="sxs-lookup"><span data-stu-id="9a442-123">A topic provides a user-defined endpoint that you post your events to.</span></span> <span data-ttu-id="9a442-124">I följande exempel skapas ämnet i din resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9a442-124">The following example creates the topic in your resource group.</span></span> <span data-ttu-id="9a442-125">Ersätt `<topic_name>` med ett unikt namn för ditt ämne.</span><span class="sxs-lookup"><span data-stu-id="9a442-125">Replace `<topic_name>` with a unique name for your topic.</span></span> <span data-ttu-id="9a442-126">Ämnesnamnet måste vara unikt eftersom det representeras av en DNS-post.</span><span class="sxs-lookup"><span data-stu-id="9a442-126">The topic name must be unique because it is represented by a DNS entry.</span></span> <span data-ttu-id="9a442-127">I förhandsversionen har Event Grid stöd för platserna **westus2** och **westcentralus**.</span><span class="sxs-lookup"><span data-stu-id="9a442-127">For the preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span>

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a><span data-ttu-id="9a442-128">Skapa en slutpunkt för meddelanden</span><span class="sxs-lookup"><span data-stu-id="9a442-128">Create a message endpoint</span></span>

<span data-ttu-id="9a442-129">Innan du prenumererar på ämnet ska vi ska slutpunkten för händelsemeddelandet.</span><span class="sxs-lookup"><span data-stu-id="9a442-129">Before subscribing to the topic, let's create the endpoint for the event message.</span></span> <span data-ttu-id="9a442-130">I stället för att skriva kod för att svar på händelsen ska vi skapa en slutpunkt som samlar in meddelandena så att du kan visa dem.</span><span class="sxs-lookup"><span data-stu-id="9a442-130">Rather than write code to respond to the event, let's create an endpoint that collects the messages so you can view them.</span></span> <span data-ttu-id="9a442-131">RequestBin är en öppen källkod, ett tredjepartsverktyg som låter dig skapa en slutpunkt och visa begäran som skickas till den.</span><span class="sxs-lookup"><span data-stu-id="9a442-131">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="9a442-132">Gå till [RequestBin](https://requestb.in/) och klicka på **Create a RequestBin** (skapa en lagerplats).</span><span class="sxs-lookup"><span data-stu-id="9a442-132">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span>  <span data-ttu-id="9a442-133">Kopiera lagerplatsens URL eftersom du behöver den för att prenumerera på ämnet.</span><span class="sxs-lookup"><span data-stu-id="9a442-133">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

## <a name="subscribe-to-a-topic"></a><span data-ttu-id="9a442-134">Prenumerera på ett ämne</span><span class="sxs-lookup"><span data-stu-id="9a442-134">Subscribe to a topic</span></span>

<span data-ttu-id="9a442-135">Du prenumererar på ett ämne för att ange för Event Grid vilka händelser du vill följa. I följande exempel prenumererar vi på det ämne du just skapat, och URL från RequestBin skickas som slutpunkt för händelseavisering.</span><span class="sxs-lookup"><span data-stu-id="9a442-135">You subscribe to a topic to tell Event Grid which events you want to track. The following example subscribes to the topic you created, and passes the URL from RequestBin as the endpoint for event notification.</span></span> <span data-ttu-id="9a442-136">Ersätt `<event_subscription_name>` med ett unikt namn för din prenumeration och `<URL_from_RequestBin>` med värdet från föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9a442-136">Replace `<event_subscription_name>` with a unique name for your subscription, and `<URL_from_RequestBin>` with the value from the preceding section.</span></span> <span data-ttu-id="9a442-137">Genom att ange en slutpunkt när du prenumererar kan Event Grid hantera omdirigeringen av händelser till denna slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="9a442-137">By specifying an endpoint when subscribing, Event Grid handles the routing of events to that endpoint.</span></span> <span data-ttu-id="9a442-138">För `<topic_name>` använder du det värde du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9a442-138">For `<topic_name>`, use the value you created earlier.</span></span> 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-to-your-topic"></a><span data-ttu-id="9a442-139">Skicka en händelse till ditt ämne</span><span class="sxs-lookup"><span data-stu-id="9a442-139">Send an event to your topic</span></span>

<span data-ttu-id="9a442-140">Nu ska vi utlösa en händelse och se hur Event Grid distribuerar meddelandet till slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="9a442-140">Now, let's trigger an event to see how Event Grid distributes the message to your endpoint.</span></span> <span data-ttu-id="9a442-141">Först måste vi ta fram URL och nyckel för ämnet.</span><span class="sxs-lookup"><span data-stu-id="9a442-141">First, let's get the URL and key for the topic.</span></span> <span data-ttu-id="9a442-142">Än en gång, använd din ämnesnamn för `<topic_name>`.</span><span class="sxs-lookup"><span data-stu-id="9a442-142">Again, use your topic name for `<topic_name>`.</span></span>

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

<span data-ttu-id="9a442-143">Som en enkel förklaring av den här artikeln har vi skapat exempelhändelsedata att skicka till ämnet.</span><span class="sxs-lookup"><span data-stu-id="9a442-143">To simplify this article, we have set up sample event data to send to the topic.</span></span> <span data-ttu-id="9a442-144">Ett program eller en Azure-tjänst skulle vanligtvis skicka sådana händelsedata.</span><span class="sxs-lookup"><span data-stu-id="9a442-144">Typically, an application or Azure service would send the event data.</span></span> <span data-ttu-id="9a442-145">I följande exempel hämtas händelsedata:</span><span class="sxs-lookup"><span data-stu-id="9a442-145">The following example gets the event data:</span></span>

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

<span data-ttu-id="9a442-146">Om du `echo "$body"` kan du se den fullständiga händelsen.</span><span class="sxs-lookup"><span data-stu-id="9a442-146">If you `echo "$body"` you can see the full event.</span></span> <span data-ttu-id="9a442-147">Elementet `data` av JSON är händelsens nyttolast.</span><span class="sxs-lookup"><span data-stu-id="9a442-147">The `data` element of the JSON is the payload of your event.</span></span> <span data-ttu-id="9a442-148">All välformulerad JSON kan stå i det här fältet.</span><span class="sxs-lookup"><span data-stu-id="9a442-148">Any well-formed JSON can go in this field.</span></span> <span data-ttu-id="9a442-149">Du kan också använda ämnesfältet för avancerad omdirigering och filtrering.</span><span class="sxs-lookup"><span data-stu-id="9a442-149">You can also use the subject field for advanced routing and filtering.</span></span>

<span data-ttu-id="9a442-150">CURL är ett verktyg som utför HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="9a442-150">CURL is a utility that performs HTTP requests.</span></span> <span data-ttu-id="9a442-151">I den här artikeln använder vi CURL för att skicka händelsen till vårt ämne.</span><span class="sxs-lookup"><span data-stu-id="9a442-151">In this article, we use CURL to send the event to our topic.</span></span> 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

<span data-ttu-id="9a442-152">Du har utlöst händelsen och Event Grid skickade meddelandet till den slutpunkt du konfigurerade när du startade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="9a442-152">You have triggered the event, and Event Grid sent the message to the endpoint you configured when subscribing.</span></span> <span data-ttu-id="9a442-153">Bläddra till den RequestBin-URL du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9a442-153">Browse to the RequestBin URL that you created earlier.</span></span> <span data-ttu-id="9a442-154">Eller klicka på Uppdatera i den RequestBin-webbläsaren du har öppen.</span><span class="sxs-lookup"><span data-stu-id="9a442-154">Or, click refresh in your open RequestBin browser.</span></span> <span data-ttu-id="9a442-155">Du kan se den händelse du just skickade.</span><span class="sxs-lookup"><span data-stu-id="9a442-155">You see the event you just sent.</span></span> 

```json
[{
  "id": "1807",
  "eventType": "recordInserted",
  "subject": "myapp/vehicles/motorcycles",
  "eventTime": "2017-08-10T21:03:07+00:00",
  "data": {
    "make": "Ducati",
    "model": "Monster"
  },
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/topics/{topic}"
}]
```

## <a name="clean-up-resources"></a><span data-ttu-id="9a442-156">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="9a442-156">Clean up resources</span></span>
<span data-ttu-id="9a442-157">Om du planerar att fortsätta arbeta med den här händelsen ska du inte rensa upp bland de resurser som skapades i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9a442-157">If you plan to continue working with this event, do not clean up the resources created in this article.</span></span> <span data-ttu-id="9a442-158">Om du inte planerar att fortsätta kan du använda kommandona nedan för att ta bort alla resurser som har skapats i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9a442-158">If you do not plan to continue, use the following command to delete the resources you created in this article.</span></span>

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="9a442-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a442-159">Next steps</span></span>

<span data-ttu-id="9a442-160">Nu när du vet hur du skapar ämnen och prenumerationer på händelser kan du läsa mer om vad Event Grid kan hjälpa dig med:</span><span class="sxs-lookup"><span data-stu-id="9a442-160">Now that you know how to create topics and event subscriptions, learn more about what Event Grid can help you do:</span></span>

- [<span data-ttu-id="9a442-161">Om Event Grid</span><span class="sxs-lookup"><span data-stu-id="9a442-161">About Event Grid</span></span>](overview.md)
- [<span data-ttu-id="9a442-162">Övervaka ändringar på virtuella maskiner med Azure Event Grid och Logic Apps</span><span class="sxs-lookup"><span data-stu-id="9a442-162">Monitor virtual machine changes with Azure Event Grid and Logic Apps</span></span>](monitor-virtual-machine-changes-event-grid-logic-app.md)
