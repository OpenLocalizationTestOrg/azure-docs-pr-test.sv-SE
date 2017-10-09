---
title: "aaaView Azure Nätverksbevakaren topologi - Azure CLI 1.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse Azure CLI 1.0 tooquery nätverkets topologi."
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
ms.openlocfilehash: 30679d6dc74e85bacfc86c63bd1afa873893c772
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-azure-cli-10"></a><span data-ttu-id="fcd54-103">Visa Nätverksbevakaren topologi med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fcd54-103">View Network Watcher topology with Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="fcd54-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fcd54-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="fcd54-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fcd54-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="fcd54-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fcd54-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="fcd54-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="fcd54-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="fcd54-108">hello topologi funktion i Nätverksbevakaren ger en bild av hello nätverksresurser i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fcd54-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="fcd54-109">Hello-portalen visas den här visualiseringen tooyou automatiskt.</span><span class="sxs-lookup"><span data-stu-id="fcd54-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="fcd54-110">hello informationen bakom hello topologiska vyn i hello portal kan hämtas via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fcd54-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="fcd54-111">Den här funktionen gör hello topologiinformation mer flexibla hello data kan användas av andra verktyg toobuild ut hello visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="fcd54-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="fcd54-112">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="fcd54-112">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> 

<span data-ttu-id="fcd54-113">hello sammankoppling modelleras under två relationer.</span><span class="sxs-lookup"><span data-stu-id="fcd54-113">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="fcd54-114">**Inneslutning** -exempel: innehåller ett undernät för virtuellt nätverk innehåller ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="fcd54-114">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="fcd54-115">**Associerade** -exempel: NIC som är kopplad till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="fcd54-115">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="fcd54-116">hello finns följande lista egenskaper som returneras när du frågar hello topologi REST API.</span><span class="sxs-lookup"><span data-stu-id="fcd54-116">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="fcd54-117">**namnet** - hello namn på hello resurs</span><span class="sxs-lookup"><span data-stu-id="fcd54-117">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="fcd54-118">**ID** -hello hello resurs-uri.</span><span class="sxs-lookup"><span data-stu-id="fcd54-118">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="fcd54-119">**plats** -hello plats där hello resurserna finns.</span><span class="sxs-lookup"><span data-stu-id="fcd54-119">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="fcd54-120">**kopplingarna** -en lista över kopplingar toohello refererar till objektet.</span><span class="sxs-lookup"><span data-stu-id="fcd54-120">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="fcd54-121">**namnet** -hello namnet på hello refererar till resursen.</span><span class="sxs-lookup"><span data-stu-id="fcd54-121">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="fcd54-122">**resourceId** -hello resourceId är hello uri hello-resurs som refereras i hello association.</span><span class="sxs-lookup"><span data-stu-id="fcd54-122">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="fcd54-123">**associationType** -hello förhållandet mellan hello underordnat objekt och hello överordnade refererar till det här värdet.</span><span class="sxs-lookup"><span data-stu-id="fcd54-123">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="fcd54-124">Giltiga värden är **innehåller** eller **associerade**.</span><span class="sxs-lookup"><span data-stu-id="fcd54-124">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fcd54-125">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="fcd54-125">Before you begin</span></span>

<span data-ttu-id="fcd54-126">I det här scenariot kan du använda hello `network watcher topology` cmdlet tooretrieve hello topologiinformation.</span><span class="sxs-lookup"><span data-stu-id="fcd54-126">In this scenario, you use hello `network watcher topology` cmdlet tooretrieve hello topology information.</span></span> <span data-ttu-id="fcd54-127">Det finns också en artikel om hur för[hämta nätverkets topologi med REST API](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="fcd54-127">There is also an article on how too[retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="fcd54-128">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="fcd54-128">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="fcd54-129">Scenario</span><span class="sxs-lookup"><span data-stu-id="fcd54-129">Scenario</span></span>

<span data-ttu-id="fcd54-130">hello-scenario som beskrivs i den här artikeln hämtar hello topologi svar för en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fcd54-130">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="retrieve-topology"></a><span data-ttu-id="fcd54-131">Hämta topologi</span><span class="sxs-lookup"><span data-stu-id="fcd54-131">Retrieve topology</span></span>

<span data-ttu-id="fcd54-132">Hej `network watcher topology` cmdlet hämtar hello topologin för en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fcd54-132">hello `network watcher topology` cmdlet retrieves hello topology for a given resource group.</span></span> <span data-ttu-id="fcd54-133">Lägga till hello argumentet ”--json” tooview hello oput i json-format</span><span class="sxs-lookup"><span data-stu-id="fcd54-133">Add hello argument "--json" tooview hello oput in json format</span></span>

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a><span data-ttu-id="fcd54-134">Resultat</span><span class="sxs-lookup"><span data-stu-id="fcd54-134">Results</span></span>

<span data-ttu-id="fcd54-135">hello resultaten har egenskapen name ”resurser”, som innehåller hello json svarstexten för hello `network watcher topology` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fcd54-135">hello results returned have a property name "Resources", which contains hello json response body for hello `network watcher topology` cmdlet.</span></span>  <span data-ttu-id="fcd54-136">hello svaret innehåller hello resurser i hello säkerhetsgrupp för nätverk och deras associationer (det vill säga innehåller, associerade).</span><span class="sxs-lookup"><span data-stu-id="fcd54-136">hello response contains hello resources in hello Network Security Group and their associations (that is, Contains, Associated).</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="fcd54-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fcd54-137">Next steps</span></span>

<span data-ttu-id="fcd54-138">Mer information om hello säkerhetsregler som tillämpade tooyour nätverksresurser genom att besöka [Säkerhetsöversikt grupp vy](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="fcd54-138">Learn more about hello security rules that are applied tooyour network resources by visiting [Security group view overview](network-watcher-security-group-view-overview.md)</span></span>
