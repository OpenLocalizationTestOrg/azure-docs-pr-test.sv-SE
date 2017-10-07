---
title: "aaaCustom händelser för Azure händelse rutnät | Microsoft Docs"
description: "Använd Azure händelse rutnätet toopublish ett ämne och prenumerera toothat händelse."
services: event-grid
keywords: 
author: djrosanova
ms.author: darosa
ms.date: 08/15/2017
ms.topic: hero-article
ms.service: event-grid
ms.openlocfilehash: 5055df1c970b043cadf06978a076f7f5c83501cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-route-custom-events-with-azure-event-grid"></a><span data-ttu-id="a7afd-103">Skapa och dirigera anpassade händelser med Azure Event Grid</span><span class="sxs-lookup"><span data-stu-id="a7afd-103">Create and route custom events with Azure Event Grid</span></span>

<span data-ttu-id="a7afd-104">Azure händelse rutnätet är en eventing för hello molnet.</span><span class="sxs-lookup"><span data-stu-id="a7afd-104">Azure Event Grid is an eventing service for hello cloud.</span></span> <span data-ttu-id="a7afd-105">I den här artikeln använder hello Azure CLI toocreate anpassade avsnittet, prenumerera toohello avsnittet och utlösa hello händelse tooview hello resultat.</span><span class="sxs-lookup"><span data-stu-id="a7afd-105">In this article, you use hello Azure CLI toocreate a custom topic, subscribe toohello topic, and trigger hello event tooview hello result.</span></span> <span data-ttu-id="a7afd-106">Vanligtvis skickar händelser tooan slutpunkt som svarar toohello händelse, till exempel en webhook eller Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="a7afd-106">Typically, you send events tooan endpoint that responds toohello event, such as, a webhook or Azure Function.</span></span> <span data-ttu-id="a7afd-107">Dock toosimplify detta artikeln kan du skicka hello händelser tooa URL som endast samlar in hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="a7afd-107">However, toosimplify this article, you send hello events tooa URL that merely collects hello messages.</span></span> <span data-ttu-id="a7afd-108">Du skapar denna URL med hjälp av en öppen källkod, ett tredjepartsverktyg som kallas [RequestBin](https://requestb.in/).</span><span class="sxs-lookup"><span data-stu-id="a7afd-108">You create this URL by using an open source, third-party tool called [RequestBin](https://requestb.in/).</span></span>

>[!NOTE]
><span data-ttu-id="a7afd-109">**RequestBin** är ett verktyg med öppen källkod som inte är avsett för användning med högt dataflöde.</span><span class="sxs-lookup"><span data-stu-id="a7afd-109">**RequestBin** is an open source tool that is not intended for high throughput usage.</span></span> <span data-ttu-id="a7afd-110">hello används hello verktyget här enbart demonstrativt.</span><span class="sxs-lookup"><span data-stu-id="a7afd-110">hello use of hello tool here is purely demonstrative.</span></span> <span data-ttu-id="a7afd-111">Om du trycker på fler än en händelse i taget, kan du inte se alla dina händelser i hello-verktyget.</span><span class="sxs-lookup"><span data-stu-id="a7afd-111">If you push more than one event at a time, you might not see all of your events in hello tool.</span></span>

<span data-ttu-id="a7afd-112">När du är klar ser du att hello händelsedata har skickats tooan slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a7afd-112">When you are finished, you see that hello event data has been sent tooan endpoint.</span></span>

![Händelsedata](./media/custom-event-quickstart/request-result.png)

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a7afd-114">Om du väljer tooinstall och använda hello CLI lokalt i den här artikeln kräver att du kör hello senaste versionen av Azure CLI (2.0.14 eller senare).</span><span class="sxs-lookup"><span data-stu-id="a7afd-114">If you choose tooinstall and use hello CLI locally, this article requires that you are running hello latest version of Azure CLI (2.0.14 or later).</span></span> <span data-ttu-id="a7afd-115">toofind hello version, kör `az --version`.</span><span class="sxs-lookup"><span data-stu-id="a7afd-115">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="a7afd-116">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a7afd-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="a7afd-117">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a7afd-117">Create a resource group</span></span>

<span data-ttu-id="a7afd-118">Event Grid-ämnen är Azure-resurser och måste placeras i en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a7afd-118">Event Grid topics are Azure resources, and must be placed in an Azure resource group.</span></span> <span data-ttu-id="a7afd-119">hello resursgrupp är en logisk samling i vilka Azure resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="a7afd-119">hello resource group is a logical collection into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="a7afd-120">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a7afd-120">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="a7afd-121">hello följande exempel skapar en resursgrupp med namnet *gridResourceGroup* i hello *westus2* plats.</span><span class="sxs-lookup"><span data-stu-id="a7afd-121">hello following example creates a resource group named *gridResourceGroup* in hello *westus2* location.</span></span>

```azurecli-interactive
az group create --name gridResourceGroup --location westus2
```

## <a name="create-a-custom-topic"></a><span data-ttu-id="a7afd-122">Skapa en anpassat ämne</span><span class="sxs-lookup"><span data-stu-id="a7afd-122">Create a custom topic</span></span>

<span data-ttu-id="a7afd-123">Ett ämne ger en användardefinierad slutpunkt där du publicerar dina händelser.</span><span class="sxs-lookup"><span data-stu-id="a7afd-123">A topic provides a user-defined endpoint that you post your events to.</span></span> <span data-ttu-id="a7afd-124">hello skapar följande exempel hello-avsnittet i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a7afd-124">hello following example creates hello topic in your resource group.</span></span> <span data-ttu-id="a7afd-125">Ersätt `<topic_name>` med ett unikt namn för ditt ämne.</span><span class="sxs-lookup"><span data-stu-id="a7afd-125">Replace `<topic_name>` with a unique name for your topic.</span></span> <span data-ttu-id="a7afd-126">hello ämnesnamn måste vara unikt eftersom den representeras av en DNS-post.</span><span class="sxs-lookup"><span data-stu-id="a7afd-126">hello topic name must be unique because it is represented by a DNS entry.</span></span> <span data-ttu-id="a7afd-127">Hello förhandsversionen händelse rutnätet stöder **westus2** och **westcentralus** platser.</span><span class="sxs-lookup"><span data-stu-id="a7afd-127">For hello preview release, Event Grid supports **westus2** and **westcentralus** locations.</span></span>

```azurecli-interactive
az eventgrid topic create --name <topic_name> -l westus2 -g gridResourceGroup
```

## <a name="create-a-message-endpoint"></a><span data-ttu-id="a7afd-128">Skapa en slutpunkt för meddelanden</span><span class="sxs-lookup"><span data-stu-id="a7afd-128">Create a message endpoint</span></span>

<span data-ttu-id="a7afd-129">Nu ska vi skapa hello slutpunkt för hello händelsemeddelandet innan prenumerera toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a7afd-129">Before subscribing toohello topic, let's create hello endpoint for hello event message.</span></span> <span data-ttu-id="a7afd-130">Skapar vi en slutpunkt som samlar in hälsningsmeddelande så att du kan visa dem i stället för att skriva kod toorespond toohello händelse.</span><span class="sxs-lookup"><span data-stu-id="a7afd-130">Rather than write code toorespond toohello event, let's create an endpoint that collects hello messages so you can view them.</span></span> <span data-ttu-id="a7afd-131">RequestBin är en öppen källkod, tredjeparts-verktyget kan du toocreate en slutpunkt och visa begäranden som skickas tooit.</span><span class="sxs-lookup"><span data-stu-id="a7afd-131">RequestBin is an open source, third-party tool that enables you toocreate an endpoint, and view requests that are sent tooit.</span></span> <span data-ttu-id="a7afd-132">Gå för[RequestBin](https://requestb.in/), och klicka på **skapa en RequestBin**.</span><span class="sxs-lookup"><span data-stu-id="a7afd-132">Go too[RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span>  <span data-ttu-id="a7afd-133">Kopiera hello bin URL eftersom du behöver den när du prenumererar toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a7afd-133">Copy hello bin URL, because you need it when subscribing toohello topic.</span></span>

## <a name="subscribe-tooa-topic"></a><span data-ttu-id="a7afd-134">Prenumerera tooa avsnittet</span><span class="sxs-lookup"><span data-stu-id="a7afd-134">Subscribe tooa topic</span></span>

<span data-ttu-id="a7afd-135">Du prenumererar tooa avsnittet tootell händelse rutnätet vilka händelser som du vill tootrack.</span><span class="sxs-lookup"><span data-stu-id="a7afd-135">You subscribe tooa topic tootell Event Grid which events you want tootrack.</span></span> <span data-ttu-id="a7afd-136">hello prenumererar följande exempel toohello avsnittet du skapat och överför hello URL från RequestBin som hello slutpunkt för händelseavisering.</span><span class="sxs-lookup"><span data-stu-id="a7afd-136">hello following example subscribes toohello topic you created, and passes hello URL from RequestBin as hello endpoint for event notification.</span></span> <span data-ttu-id="a7afd-137">Ersätt `<event_subscription_name>` med ett unikt namn för din prenumeration och `<URL_from_RequestBin>` med hello-värde från hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a7afd-137">Replace `<event_subscription_name>` with a unique name for your subscription, and `<URL_from_RequestBin>` with hello value from hello preceding section.</span></span> <span data-ttu-id="a7afd-138">Genom att ange en slutpunkt när du prenumererar kan hanterar händelsen rutnätet hello routning av händelser toothat slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a7afd-138">By specifying an endpoint when subscribing, Event Grid handles hello routing of events toothat endpoint.</span></span> <span data-ttu-id="a7afd-139">För `<topic_name>`, Använd hello-värde som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a7afd-139">For `<topic_name>`, use hello value you created earlier.</span></span> 

```azurecli-interactive
az eventgrid topic event-subscription create --name <event_subscription_name> \
  --endpoint <URL_from_RequestBin> \
  -g gridResourceGroup \
  --topic-name <topic_name>
```

## <a name="send-an-event-tooyour-topic"></a><span data-ttu-id="a7afd-140">Skicka en händelse tooyour avsnittet</span><span class="sxs-lookup"><span data-stu-id="a7afd-140">Send an event tooyour topic</span></span>

<span data-ttu-id="a7afd-141">Nu ska vi utlöser en händelse toosee hur händelsen rutnätet distribuerar hello meddelandet tooyour slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a7afd-141">Now, let's trigger an event toosee how Event Grid distributes hello message tooyour endpoint.</span></span> <span data-ttu-id="a7afd-142">Först måste vi få hello URL och nyckeln för hello ämnet.</span><span class="sxs-lookup"><span data-stu-id="a7afd-142">First, let's get hello URL and key for hello topic.</span></span> <span data-ttu-id="a7afd-143">Än en gång, använd din ämnesnamn för `<topic_name>`.</span><span class="sxs-lookup"><span data-stu-id="a7afd-143">Again, use your topic name for `<topic_name>`.</span></span>

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

<span data-ttu-id="a7afd-144">toosimplify den här artikeln som vi har lagt upp exempel händelse data toosend toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a7afd-144">toosimplify this article, we have set up sample event data toosend toohello topic.</span></span> <span data-ttu-id="a7afd-145">Ett program eller en Azure-tjänsten skulle vanligtvis skickar hello händelsedata.</span><span class="sxs-lookup"><span data-stu-id="a7afd-145">Typically, an application or Azure service would send hello event data.</span></span> <span data-ttu-id="a7afd-146">hello följande exempel hämtar hello händelsedata:</span><span class="sxs-lookup"><span data-stu-id="a7afd-146">hello following example gets hello event data:</span></span>

```azurecli-interactive
body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json)'")
```

<span data-ttu-id="a7afd-147">Om du `echo "$body"` visas hello fullständig händelse.</span><span class="sxs-lookup"><span data-stu-id="a7afd-147">If you `echo "$body"` you can see hello full event.</span></span> <span data-ttu-id="a7afd-148">Hej `data` element av hello JSON är hello nyttolasten för din händelse.</span><span class="sxs-lookup"><span data-stu-id="a7afd-148">hello `data` element of hello JSON is hello payload of your event.</span></span> <span data-ttu-id="a7afd-149">All välformulerad JSON kan stå i det här fältet.</span><span class="sxs-lookup"><span data-stu-id="a7afd-149">Any well-formed JSON can go in this field.</span></span> <span data-ttu-id="a7afd-150">Du kan också använda hello ämnesfältet för avancerade Routning och filtrering.</span><span class="sxs-lookup"><span data-stu-id="a7afd-150">You can also use hello subject field for advanced routing and filtering.</span></span>

<span data-ttu-id="a7afd-151">CURL är ett verktyg som utför HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="a7afd-151">CURL is a utility that performs HTTP requests.</span></span> <span data-ttu-id="a7afd-152">Vi använder CURL toosend hello händelse tooour avsnittet i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a7afd-152">In this article, we use CURL toosend hello event tooour topic.</span></span> 

```azurecli-interactive
curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

<span data-ttu-id="a7afd-153">Du har utlösts hello händelsen och händelsen rutnätet skickas hello meddelandet toohello slutpunkt du konfigurerade när du prenumerera.</span><span class="sxs-lookup"><span data-stu-id="a7afd-153">You have triggered hello event, and Event Grid sent hello message toohello endpoint you configured when subscribing.</span></span> <span data-ttu-id="a7afd-154">Bläddra toohello RequestBin URL som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a7afd-154">Browse toohello RequestBin URL that you created earlier.</span></span> <span data-ttu-id="a7afd-155">Eller klicka på Uppdatera i den RequestBin-webbläsaren du har öppen.</span><span class="sxs-lookup"><span data-stu-id="a7afd-155">Or, click refresh in your open RequestBin browser.</span></span> <span data-ttu-id="a7afd-156">Du kan se hello händelse som du har skickat.</span><span class="sxs-lookup"><span data-stu-id="a7afd-156">You see hello event you just sent.</span></span> 

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

## <a name="clean-up-resources"></a><span data-ttu-id="a7afd-157">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="a7afd-157">Clean up resources</span></span>
<span data-ttu-id="a7afd-158">Om du planerar att arbeta med den här händelsen toocontinue inte rensa hello resurser skapas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a7afd-158">If you plan toocontinue working with this event, do not clean up hello resources created in this article.</span></span> <span data-ttu-id="a7afd-159">Om du inte planerar toocontinue kommando Använd hello följande toodelete hello resurser som du skapade i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a7afd-159">If you do not plan toocontinue, use hello following command toodelete hello resources you created in this article.</span></span>

```azurecli-interactive
az group delete --name gridResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="a7afd-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7afd-160">Next steps</span></span>

<span data-ttu-id="a7afd-161">Nu när du vet kan hur toocreate ämnen och händelseprenumerationer, mer information om vilka händelse rutnätet hjälpa dig:</span><span class="sxs-lookup"><span data-stu-id="a7afd-161">Now that you know how toocreate topics and event subscriptions, learn more about what Event Grid can help you do:</span></span>

- [<span data-ttu-id="a7afd-162">Om Event Grid</span><span class="sxs-lookup"><span data-stu-id="a7afd-162">About Event Grid</span></span>](overview.md)
- [<span data-ttu-id="a7afd-163">Övervaka ändringar på virtuella maskiner med Azure Event Grid och Logic Apps</span><span class="sxs-lookup"><span data-stu-id="a7afd-163">Monitor virtual machine changes with Azure Event Grid and Logic Apps</span></span>](monitor-virtual-machine-changes-event-grid-logic-app.md)
