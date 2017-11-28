---
title: "aaaCreate ett namnområde för Azure Event Hubs och aktivera avbilda med en mall | Microsoft Docs"
description: "Skapa ett namnområde för Azure Event Hubs med en händelsehubb och aktivera avbildningsfunktionen med hjälp av Azure Resource Manager-mallar"
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 8bdda6a2-5ff1-45e3-b696-c553768f1090
ms.service: event-hubs
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: a43b4e8d690ae825047e8a9d609bfda89cf2a06f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-hubs-namespace-with-an-event-hub-and-enable-capture-using-an-azure-resource-manager-template"></a><span data-ttu-id="e521b-103">Skapa ett namnområde för Event Hubs med en händelsehubb och aktivera avbildning med hjälp av en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="e521b-103">Create an Event Hubs namespace with an event hub and enable Capture using an Azure Resource Manager template</span></span>

<span data-ttu-id="e521b-104">Den här artikeln visar hur toouse en Azure Resource Manager-mall som skapar ett namnområde som Händelsehubbar med nav-instans för en händelse och aktiverar hello [avbilda funktionen](event-hubs-capture-overview.md) i hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="e521b-104">This article shows how toouse an Azure Resource Manager template that creates an Event Hubs namespace, with one event hub instance, and also enables hello [Capture feature](event-hubs-capture-overview.md) on hello event hub.</span></span> <span data-ttu-id="e521b-105">hello artikeln beskriver hur toodefine vilka resurser har distribuerats och hur toodefine parametrar som anges när hello distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="e521b-105">hello article describes how toodefine which resources are deployed, and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="e521b-106">Du kan använda den här mallen för din egen distribution eller anpassa den toomeet dina krav.</span><span class="sxs-lookup"><span data-stu-id="e521b-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="e521b-107">Den här artikeln visar hur toospecify händelser fångas i Azure Storage-Blobbar eller ett Azure Data Lake Store, baserat på hello mål som du väljer.</span><span class="sxs-lookup"><span data-stu-id="e521b-107">This article also shows how toospecify that events are captured into either Azure Storage Blobs or an Azure Data Lake Store, based on hello destination you choose.</span></span>

<span data-ttu-id="e521b-108">Mer information om att skapa mallar finns i [Redigera Azure Resource Manager-mallar][Authoring Azure Resource Manager templates].</span><span class="sxs-lookup"><span data-stu-id="e521b-108">For more information about creating templates, see [Authoring Azure Resource Manager templates][Authoring Azure Resource Manager templates].</span></span>

<span data-ttu-id="e521b-109">Mer information om mönster och praxis för namnkonventioner för Azure-resurser finns i [Azure Resources Naming Conventions][Azure Resources naming conventions] (Namnkonventioner för Azure-resurser).</span><span class="sxs-lookup"><span data-stu-id="e521b-109">For more information about patterns and practices for Azure Resources naming conventions, see [Azure Resources naming conventions][Azure Resources naming conventions].</span></span>

<span data-ttu-id="e521b-110">Hello fullständig mallar, klickar du på hello följa GitHub länkar:</span><span class="sxs-lookup"><span data-stu-id="e521b-110">For hello complete templates, click hello following GitHub links:</span></span>

- <span data-ttu-id="e521b-111">[NAV- och aktivera avbilda tooStorage händelsemallen][Event Hub and enable Capture tooStorage template]</span><span class="sxs-lookup"><span data-stu-id="e521b-111">[Event hub and enable Capture tooStorage template][Event Hub and enable Capture tooStorage template]</span></span> 
- <span data-ttu-id="e521b-112">[NAV- och aktivera avbilda tooAzure Data Lake Store händelsemall][Event Hub and enable Capture tooAzure Data Lake Store template]</span><span class="sxs-lookup"><span data-stu-id="e521b-112">[Event hub and enable Capture tooAzure Data Lake Store template][Event Hub and enable Capture tooAzure Data Lake Store template]</span></span>

> [!NOTE]
> <span data-ttu-id="e521b-113">toocheck för hello senaste mallar, besök hello [Azure-Snabbstartsmallar] [ Azure Quickstart Templates] galleriet och Sök efter Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="e521b-113">toocheck for hello latest templates, visit hello [Azure Quickstart Templates][Azure Quickstart Templates] gallery and search for Event Hubs.</span></span>
> 
> 

## <a name="what-will-you-deploy"></a><span data-ttu-id="e521b-114">Vad vill du distribuera?</span><span class="sxs-lookup"><span data-stu-id="e521b-114">What will you deploy?</span></span>

<span data-ttu-id="e521b-115">Med den här mallen distribuerar du ett namnområde för Event Hubs med en händelsehubb och aktiverar även [Event Hubs Capture](event-hubs-capture-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e521b-115">With this template, you deploy an Event Hubs namespace with an event hub, and also enable [Event Hubs Capture](event-hubs-capture-overview.md).</span></span>

<span data-ttu-id="e521b-116">[Händelsehubbar](event-hubs-what-is-event-hubs.md) är en händelsebearbetning tjänsten används tooprovide händelse- och ingångsanspråk tooAzure i massiv skala med kort svarstid och hög tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="e521b-116">[Event Hubs](event-hubs-what-is-event-hubs.md) is an event processing service used tooprovide event and telemetry ingress tooAzure at massive scale, with low latency and high reliability.</span></span> <span data-ttu-id="e521b-117">Händelsen hubbar avbilda aktiverar du tooautomatically leverera hello strömmande data i Händelsehubbar tooAzure Blob storage eller Azure Data Lake Store inom en angiven tid eller storlek intervall du väljer.</span><span class="sxs-lookup"><span data-stu-id="e521b-117">Event Hubs Capture enables you tooautomatically deliver hello streaming data in Event Hubs tooAzure Blob storage or Azure Data Lake Store, within a specified time or size interval of your choosing.</span></span>

<span data-ttu-id="e521b-118">Klicka på hello efter knappen tooenable Event Hubs avbilda till Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="e521b-118">Click hello following button tooenable Event Hubs Capture into Azure Storage:</span></span>

<span data-ttu-id="e521b-119">[![Distribuera tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e521b-119">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)</span></span>

<span data-ttu-id="e521b-120">Klicka på hello efter knappen tooenable Event Hubs avbilda till Azure Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="e521b-120">Click hello following button tooenable Event Hubs Capture into Azure Data Lake Store:</span></span>

<span data-ttu-id="e521b-121">[![Distribuera tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e521b-121">[![Deploy tooAzure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture-for-adls%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="e521b-122">Parametrar</span><span class="sxs-lookup"><span data-stu-id="e521b-122">Parameters</span></span>

<span data-ttu-id="e521b-123">Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="e521b-123">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="e521b-124">hello mallen innehåller ett avsnitt som heter `Parameters` som innehåller alla hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="e521b-124">hello template includes a section called `Parameters` that contains all hello parameter values.</span></span> <span data-ttu-id="e521b-125">Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="e521b-125">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="e521b-126">Definiera inte parametrar för värden som alltid hello samma.</span><span class="sxs-lookup"><span data-stu-id="e521b-126">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="e521b-127">Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="e521b-127">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span>

<span data-ttu-id="e521b-128">hello mallen definierar hello följande parametrar.</span><span class="sxs-lookup"><span data-stu-id="e521b-128">hello template defines hello following parameters.</span></span>

### <a name="eventhubnamespacename"></a><span data-ttu-id="e521b-129">eventHubNamespaceName</span><span class="sxs-lookup"><span data-stu-id="e521b-129">eventHubNamespaceName</span></span>

<span data-ttu-id="e521b-130">hello namnet på hello [Händelsehubbar namnområde](event-hubs-create.md) toocreate.</span><span class="sxs-lookup"><span data-stu-id="e521b-130">hello name of hello [Event Hubs namespace](event-hubs-create.md) toocreate.</span></span>

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of hello EventHub namespace"
      }
}
```

### <a name="eventhubname"></a><span data-ttu-id="e521b-131">eventHubName</span><span class="sxs-lookup"><span data-stu-id="e521b-131">eventHubName</span></span>

<span data-ttu-id="e521b-132">hello namnet på hello händelsehubb skapas i hello [Händelsehubbar namnområde](event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="e521b-132">hello name of hello event hub created in hello [Event Hubs namespace](event-hubs-create.md).</span></span>

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of hello event hub"
    }
}
```

### <a name="messageretentionindays"></a><span data-ttu-id="e521b-133">messageRetentionInDays</span><span class="sxs-lookup"><span data-stu-id="e521b-133">messageRetentionInDays</span></span>

<span data-ttu-id="e521b-134">hello antal dagar tooretain hälsningsmeddelande i hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="e521b-134">hello number of days tooretain hello messages in hello event hub.</span></span> 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long tooretain hello data in event hub"
     }
 }
```

### <a name="partitioncount"></a><span data-ttu-id="e521b-135">partitionCount</span><span class="sxs-lookup"><span data-stu-id="e521b-135">partitionCount</span></span>

<span data-ttu-id="e521b-136">hello antalet partitioner toocreate i hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="e521b-136">hello number of partitions toocreate in hello event hub.</span></span>

```json
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="captureenabled"></a><span data-ttu-id="e521b-137">captureEnabled</span><span class="sxs-lookup"><span data-stu-id="e521b-137">captureEnabled</span></span>

<span data-ttu-id="e521b-138">Aktivera avbildning på hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="e521b-138">Enable Capture on hello event hub.</span></span>

```json
"captureEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable hello Capture for your event hub"
    }
 }
```
### <a name="captureencodingformat"></a><span data-ttu-id="e521b-139">captureEncodingFormat</span><span class="sxs-lookup"><span data-stu-id="e521b-139">captureEncodingFormat</span></span>

<span data-ttu-id="e521b-140">Hej kodningsformat som du anger tooserialize hello händelsedata.</span><span class="sxs-lookup"><span data-stu-id="e521b-140">hello encoding format you specify tooserialize hello event data.</span></span>

```json
"captureEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"hello encoding format in which Capture serializes hello EventData"
    }
}
```

### <a name="capturetime"></a><span data-ttu-id="e521b-141">captureTime</span><span class="sxs-lookup"><span data-stu-id="e521b-141">captureTime</span></span>

<span data-ttu-id="e521b-142">hello tidsintervall som Event Hubs avbilda inleder fånga hello data.</span><span class="sxs-lookup"><span data-stu-id="e521b-142">hello time interval in which Event Hubs Capture starts capturing hello data.</span></span>

```json
"captureTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"hello time window in seconds for hello capture"
    }
}
```

### <a name="capturesize"></a><span data-ttu-id="e521b-143">captureSize</span><span class="sxs-lookup"><span data-stu-id="e521b-143">captureSize</span></span>
<span data-ttu-id="e521b-144">hello storlek intervall som avbilda börjar fånga hello data.</span><span class="sxs-lookup"><span data-stu-id="e521b-144">hello size interval at which Capture starts capturing hello data.</span></span>

```json
"captureSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"hello size window in bytes for capture"
    }
}
```

###<a name="capturenameformat"></a><span data-ttu-id="e521b-145">captureNameFormat</span><span class="sxs-lookup"><span data-stu-id="e521b-145">captureNameFormat</span></span>

<span data-ttu-id="e521b-146">hello namnformat används av Event Hubs avbilda toowrite hello Avro-filer.</span><span class="sxs-lookup"><span data-stu-id="e521b-146">hello name format used by Event Hubs Capture toowrite hello Avro files.</span></span> <span data-ttu-id="e521b-147">Observera att namnformatet för avbildningsfunktionen måste innehålla fälten `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}` och `{Second}`.</span><span class="sxs-lookup"><span data-stu-id="e521b-147">Note that a Capture name format must contain `{Namespace}`, `{EventHub}`, `{PartitionId}`, `{Year}`, `{Month}`, `{Day}`, `{Hour}`, `{Minute}`, and `{Second}` fields.</span></span> <span data-ttu-id="e521b-148">Dessa kan ordnas i valfri ordning, med eller utan avgränsare.</span><span class="sxs-lookup"><span data-stu-id="e521b-148">These can be arranged in any order, with or without delimiters.</span></span>
 
```json
"captureNameFormat": {
      "type": "string",
      "defaultValue": "{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}",
      "metadata": {
        "description": "A Capture Name Format must contain {Namespace}, {EventHub}, {PartitionId}, {Year}, {Month}, {Day}, {Hour}, {Minute} and {Second} fields. These can be arranged in any order with or without delimeters. E.g.  Prod_{EventHub}/{Namespace}\\{PartitionId}_{Year}_{Month}/{Day}/{Hour}/{Minute}/{Second}"
      }
    }
  }
```

### <a name="apiversion"></a><span data-ttu-id="e521b-149">apiVersion</span><span class="sxs-lookup"><span data-stu-id="e521b-149">apiVersion</span></span>

<span data-ttu-id="e521b-150">hello API-version av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="e521b-150">hello API version of hello template.</span></span>

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by hello template"
    }
 }
```

<span data-ttu-id="e521b-151">Använd hello följande parametrar om du väljer Azure Storage som mål.</span><span class="sxs-lookup"><span data-stu-id="e521b-151">Use hello following parameters if you choose Azure Storage as your destination.</span></span>

### <a name="destinationstorageaccountresourceid"></a><span data-ttu-id="e521b-152">destinationStorageAccountResourceId</span><span class="sxs-lookup"><span data-stu-id="e521b-152">destinationStorageAccountResourceId</span></span>

<span data-ttu-id="e521b-153">Avbilda kräver ett Azure Storage-konto resurs-ID tooenable fånga tooyour önskad Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e521b-153">Capture requires an Azure Storage account resource ID tooenable capturing tooyour desired Storage account.</span></span>

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing Storage account resource ID where you want hello blobs be captured"
    }
 }
```

### <a name="blobcontainername"></a><span data-ttu-id="e521b-154">blobContainerName</span><span class="sxs-lookup"><span data-stu-id="e521b-154">blobContainerName</span></span>

<span data-ttu-id="e521b-155">Hej blob-behållare i vilken toocapture din händelsedata.</span><span class="sxs-lookup"><span data-stu-id="e521b-155">hello blob container in which toocapture your event data.</span></span>

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container in which you want hello blobs captured"
    }
}
```

<span data-ttu-id="e521b-156">Använd hello följande parametrar om du väljer Azure Data Lake Store som mål.</span><span class="sxs-lookup"><span data-stu-id="e521b-156">Use hello following parameters if you choose Azure Data Lake Store as your destination.</span></span> <span data-ttu-id="e521b-157">Du måste ange behörigheter för din Data Lake Store-sökväg som du vill tooCapture hello händelse.</span><span class="sxs-lookup"><span data-stu-id="e521b-157">You must set permissions on your Data Lake Store path, in which you want tooCapture hello event.</span></span> <span data-ttu-id="e521b-158">tooset behörigheter Se [i den här artikeln](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span><span class="sxs-lookup"><span data-stu-id="e521b-158">tooset permissions, see [this article](event-hubs-capture-enable-through-portal.md#capture-data-to-an-azure-data-lake-store-account).</span></span>

###<a name="subscriptionid"></a><span data-ttu-id="e521b-159">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="e521b-159">subscriptionId</span></span>

<span data-ttu-id="e521b-160">Prenumerations-ID för hello Händelsehubbar namnområde och Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e521b-160">Subscription ID for hello Event Hubs namespace and Azure Data Lake Store.</span></span> <span data-ttu-id="e521b-161">Båda dessa resurser måste vara under hello samma prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="e521b-161">Both these resources must be under hello same subscription ID.</span></span>

```json
"subscriptionId": {
    "type": "string",
    "metadata": {
        "description": "Subscription Id of both Azure Data Lake Store and Event Hub namespace"
     }
 }
```

###<a name="datalakeaccountname"></a><span data-ttu-id="e521b-162">dataLakeAccountName</span><span class="sxs-lookup"><span data-stu-id="e521b-162">dataLakeAccountName</span></span>

<span data-ttu-id="e521b-163">hello Azure Data Lake Store-namn för hello avbildas händelser.</span><span class="sxs-lookup"><span data-stu-id="e521b-163">hello Azure Data Lake Store name for hello captured events.</span></span>

```json
"dataLakeAccountName": {
    "type": "string",
    "metadata": {
        "description": "Azure Data Lake Store name"
    }
}
```

###<a name="datalakefolderpath"></a><span data-ttu-id="e521b-164">dataLakeFolderPath</span><span class="sxs-lookup"><span data-stu-id="e521b-164">dataLakeFolderPath</span></span>

<span data-ttu-id="e521b-165">hello målmappens sökväg för hello avbildas händelser.</span><span class="sxs-lookup"><span data-stu-id="e521b-165">hello destination folder path for hello captured events.</span></span>

```json
"dataLakeFolderPath": {
    "type": "string",
    "metadata": {
        "description": "Destination archive folder path"
    }
}
```

## <a name="resources-toodeploy-for-azure-storage-as-destination-toocaptured-events"></a><span data-ttu-id="e521b-166">Resurser toodeploy för Azure Storage som mål toocaptured händelser</span><span class="sxs-lookup"><span data-stu-id="e521b-166">Resources toodeploy for Azure Storage as destination toocaptured events</span></span>

<span data-ttu-id="e521b-167">Skapar ett namnområde av typen **EventHubs**, med en händelsehubb och aktiverar avbilda tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="e521b-167">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture tooAzure Blob Storage.</span></span>

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "CaptureDescription":{
                        "enabled":"[parameters('captureEnabled')]",
                        "encoding":"[parameters('captureEncodingFormat')]",
                        "intervalInSeconds":"[parameters('captureTime')]",
                        "sizeLimitInBytes":"[parameters('captureSize')]",
                        "destination":{
                            "name":"EventHubCapture.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }

               }

            }
         ]
      }
   ]
```

## <a name="resources-toodeploy-for-azure-data-lake-store-as-destination"></a><span data-ttu-id="e521b-168">Resurser toodeploy för Azure Data Lake Store som mål</span><span class="sxs-lookup"><span data-stu-id="e521b-168">Resources toodeploy for Azure Data Lake Store as destination</span></span>

<span data-ttu-id="e521b-169">Skapar ett namnområde av typen **EventHubs**, med en händelsehubb och aktiverar avbilda tooAzure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="e521b-169">Creates a namespace of type **EventHubs**, with one event hub, and also enables Capture tooAzure Data Lake Store.</span></span>

```json
 "resources": [
        {
            "apiVersion": "2015-08-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "2015-08-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('eventHubName')]",
                        "ArchiveDescription": {
                            "enabled": "true",
                            "encoding": "[parameters('archiveEncodingFormat')]",
                            "intervalInSeconds": "[parameters('archiveTime')]",
                            "sizeLimitInBytes": "[parameters('archiveSize')]",
                            "destination": {
                                "name": "EventHubArchive.AzureDataLake",
                                "properties": {
                                    "DataLakeSubscriptionId": "[parameters('subscriptionId')]",
                                    "DataLakeAccountName": "[parameters('dataLakeAccountName')]",
                                    "DataLakeFolderPath": "[parameters('dataLakeFolderPath')]",
                                    "ArchiveNameFormat": "[parameters('archiveNameFormat')]"
                                }
                            }
                        }
                    }
                }
            ]
        }
    ]
```

## <a name="commands-toorun-deployment"></a><span data-ttu-id="e521b-170">Kommandon toorun distribution</span><span class="sxs-lookup"><span data-stu-id="e521b-170">Commands toorun deployment</span></span>

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a><span data-ttu-id="e521b-171">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e521b-171">PowerShell</span></span>

<span data-ttu-id="e521b-172">Distribuera din mall tooenable Event Hubs avbilda till Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="e521b-172">Deploy your template tooenable Event Hubs Capture into Azure Storage:</span></span>
 
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json
```

<span data-ttu-id="e521b-173">Distribuera din mall tooenable Event Hubs avbilda till Azure Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="e521b-173">Deploy your template tooenable Event Hubs Capture into Azure Data Lake Store:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json
```

## <a name="azure-cli"></a><span data-ttu-id="e521b-174">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e521b-174">Azure CLI</span></span>

<span data-ttu-id="e521b-175">Välja Azure Blob Storage som mål:</span><span class="sxs-lookup"><span data-stu-id="e521b-175">Choosing Azure Blob Storage as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json][]
```

<span data-ttu-id="e521b-176">Välja Azure Data Lake Store som mål:</span><span class="sxs-lookup"><span data-stu-id="e521b-176">Choosing Azure Data Lake Store as destination:</span></span>

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture-for-adls/azuredeploy.json][]
```

## <a name="next-steps"></a><span data-ttu-id="e521b-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e521b-177">Next steps</span></span>

<span data-ttu-id="e521b-178">Du kan också konfigurera Event Hubs avbilda via hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e521b-178">You can also configure Event Hubs Capture via hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="e521b-179">Mer information finns i [aktivera Event Hubs avbilda med hello Azure-portalen](event-hubs-capture-enable-through-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e521b-179">For more information, see [Enable Event Hubs Capture using hello Azure portal](event-hubs-capture-enable-through-portal.md).</span></span>

<span data-ttu-id="e521b-180">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="e521b-180">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="e521b-181">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="e521b-181">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="e521b-182">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="e521b-182">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="e521b-183">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="e521b-183">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Azure Resources naming conventions]: https://azure.microsoft.com/documentation/articles/guidance-naming-conventions/
[Event hub and enable Capture tooStorage template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture
[Event hub and enable Capture tooAzure Data Lake Store template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture-for-adls
