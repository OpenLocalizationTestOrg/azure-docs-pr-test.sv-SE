---
title: "Visa Azure Nätverksbevakaren topologi - Azure CLI 1.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur du använder Azure CLI 1.0 för att fråga nätverkets topologi."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 9178c485a92e04564c95dae8073f045b5c639bb7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-azure-cli-10"></a><span data-ttu-id="9a57a-103">Visa Nätverksbevakaren topologi med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9a57a-103">View Network Watcher topology with Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="9a57a-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9a57a-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="9a57a-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9a57a-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="9a57a-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9a57a-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="9a57a-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="9a57a-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="9a57a-108">Funktionen topologi i Nätverksbevakaren ger en bild av nätverksresurser i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9a57a-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="9a57a-109">I portalen visas den här visualiseringen för dig automatiskt.</span><span class="sxs-lookup"><span data-stu-id="9a57a-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="9a57a-110">Informationen bakom vyn topologi i portalen kan hämtas via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9a57a-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="9a57a-111">Den här funktionen gör topologiinformationen mer flexibla data kan användas av andra verktyg för att bygga ut visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="9a57a-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="9a57a-112">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="9a57a-112">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> 

<span data-ttu-id="9a57a-113">Sambandet modelleras under två relationer.</span><span class="sxs-lookup"><span data-stu-id="9a57a-113">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="9a57a-114">**Inneslutning** -exempel: innehåller ett undernät för virtuellt nätverk innehåller ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="9a57a-114">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="9a57a-115">**Associerade** -exempel: NIC som är kopplad till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="9a57a-115">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="9a57a-116">I följande lista finns egenskaper som returneras när du frågar topologi REST API.</span><span class="sxs-lookup"><span data-stu-id="9a57a-116">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="9a57a-117">**namnet** -namnet på resursen</span><span class="sxs-lookup"><span data-stu-id="9a57a-117">**name** - The name of the resource</span></span>
* <span data-ttu-id="9a57a-118">**ID** -uri för resursen.</span><span class="sxs-lookup"><span data-stu-id="9a57a-118">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="9a57a-119">**plats** -plats där resurserna finns.</span><span class="sxs-lookup"><span data-stu-id="9a57a-119">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="9a57a-120">**kopplingarna** -en lista över kopplingar till det refererade objektet.</span><span class="sxs-lookup"><span data-stu-id="9a57a-120">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="9a57a-121">**namnet** -namnet på den refererade resursen.</span><span class="sxs-lookup"><span data-stu-id="9a57a-121">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="9a57a-122">**resourceId** -resourceId är URI: n för den resurs som refereras i kopplingen.</span><span class="sxs-lookup"><span data-stu-id="9a57a-122">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="9a57a-123">**associationType** -relationen mellan det underordnade objektet och överordnat refererar till det här värdet.</span><span class="sxs-lookup"><span data-stu-id="9a57a-123">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="9a57a-124">Giltiga värden är **innehåller** eller **associerade**.</span><span class="sxs-lookup"><span data-stu-id="9a57a-124">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9a57a-125">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="9a57a-125">Before you begin</span></span>

<span data-ttu-id="9a57a-126">I det här scenariot kan du använda den `network watcher topology` för att hämta topologiinformationen.</span><span class="sxs-lookup"><span data-stu-id="9a57a-126">In this scenario, you use the `network watcher topology` cmdlet to retrieve the topology information.</span></span> <span data-ttu-id="9a57a-127">Det finns också en artikel om hur du [hämta nätverkets topologi med REST API](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="9a57a-127">There is also an article on how to [retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="9a57a-128">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="9a57a-128">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="9a57a-129">Scenario</span><span class="sxs-lookup"><span data-stu-id="9a57a-129">Scenario</span></span>

<span data-ttu-id="9a57a-130">Det scenario som beskrivs i den här artikeln hämtar topologi-svar för en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9a57a-130">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="retrieve-topology"></a><span data-ttu-id="9a57a-131">Hämta topologi</span><span class="sxs-lookup"><span data-stu-id="9a57a-131">Retrieve topology</span></span>

<span data-ttu-id="9a57a-132">Den `network watcher topology` cmdlet hämtar topologin för en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9a57a-132">The `network watcher topology` cmdlet retrieves the topology for a given resource group.</span></span> <span data-ttu-id="9a57a-133">Lägg till argumentet ”--json” att visa oput i json-format</span><span class="sxs-lookup"><span data-stu-id="9a57a-133">Add the argument "--json" to view the oput in json format</span></span>

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a><span data-ttu-id="9a57a-134">Resultat</span><span class="sxs-lookup"><span data-stu-id="9a57a-134">Results</span></span>

<span data-ttu-id="9a57a-135">Resultaten har egenskapen name ”resurser”, som innehåller text för json-svar för den `network watcher topology` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9a57a-135">The results returned have a property name "Resources", which contains the json response body for the `network watcher topology` cmdlet.</span></span>  <span data-ttu-id="9a57a-136">Svaret innehåller resurser för Nätverkssäkerhetsgruppen och deras associationer (det vill säga innehåller, associerade).</span><span class="sxs-lookup"><span data-stu-id="9a57a-136">The response contains the resources in the Network Security Group and their associations (that is, Contains, Associated).</span></span>

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="9a57a-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a57a-137">Next steps</span></span>

<span data-ttu-id="9a57a-138">Mer information om säkerhetsregler som tillämpas på nätverksresurserna genom att besöka [Säkerhetsöversikt grupp vy](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="9a57a-138">Learn more about the security rules that are applied to your network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
