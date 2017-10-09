---
title: "aaaView Azure Nätverksbevakaren topologi - PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse PowerShell tooquery nätverkets topologi."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: bd0e882d-8011-45e8-a7ce-de231a69fb85
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2bc0ecf5baa81a68be53f55c74f362a7bc97116f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-powershell"></a><span data-ttu-id="75190-103">Visa Nätverksbevakaren topologi med PowerShell</span><span class="sxs-lookup"><span data-stu-id="75190-103">View Network Watcher topology with PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="75190-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="75190-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="75190-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="75190-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="75190-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="75190-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="75190-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="75190-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="75190-108">hello topologi funktion i Nätverksbevakaren ger en bild av hello nätverksresurser i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="75190-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="75190-109">Hello-portalen visas den här visualiseringen tooyou automatiskt.</span><span class="sxs-lookup"><span data-stu-id="75190-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="75190-110">hello informationen bakom hello topologiska vyn i hello portal kan hämtas via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="75190-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="75190-111">Den här funktionen gör hello topologiinformation mer flexibla hello data kan användas av andra verktyg toobuild ut hello visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="75190-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="75190-112">hello sammankoppling modelleras under två relationer.</span><span class="sxs-lookup"><span data-stu-id="75190-112">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="75190-113">**Inneslutning** -exempel: innehåller ett undernät för virtuellt nätverk innehåller ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="75190-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="75190-114">**Associerade** -exempel: NIC som är kopplad till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="75190-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="75190-115">hello finns följande lista egenskaper som returneras när du frågar hello topologi REST API.</span><span class="sxs-lookup"><span data-stu-id="75190-115">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="75190-116">**namnet** - hello namn på hello resurs</span><span class="sxs-lookup"><span data-stu-id="75190-116">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="75190-117">**ID** -hello hello resurs-uri.</span><span class="sxs-lookup"><span data-stu-id="75190-117">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="75190-118">**plats** -hello plats där hello resurserna finns.</span><span class="sxs-lookup"><span data-stu-id="75190-118">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="75190-119">**kopplingarna** -en lista över kopplingar toohello refererar till objektet.</span><span class="sxs-lookup"><span data-stu-id="75190-119">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="75190-120">**namnet** -hello namnet på hello refererar till resursen.</span><span class="sxs-lookup"><span data-stu-id="75190-120">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="75190-121">**resourceId** -hello resourceId är hello uri hello-resurs som refereras i hello association.</span><span class="sxs-lookup"><span data-stu-id="75190-121">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="75190-122">**associationType** -hello förhållandet mellan hello underordnat objekt och hello överordnade refererar till det här värdet.</span><span class="sxs-lookup"><span data-stu-id="75190-122">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="75190-123">Giltiga värden är **innehåller** eller **associerade**.</span><span class="sxs-lookup"><span data-stu-id="75190-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="75190-124">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="75190-124">Before you begin</span></span>

<span data-ttu-id="75190-125">I det här scenariot kan du använda hello `Get-AzureRmNetworkWatcherTopology` cmdlet tooretrieve hello topologiinformation.</span><span class="sxs-lookup"><span data-stu-id="75190-125">In this scenario, you use hello `Get-AzureRmNetworkWatcherTopology` cmdlet tooretrieve hello topology information.</span></span> <span data-ttu-id="75190-126">Det finns också en artikel om hur för[hämta nätverkets topologi med REST API](network-watcher-topology-rest.md).</span><span class="sxs-lookup"><span data-stu-id="75190-126">There is also an article on how too[retrieve network topology with REST API](network-watcher-topology-rest.md).</span></span>

<span data-ttu-id="75190-127">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="75190-127">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="75190-128">Scenario</span><span class="sxs-lookup"><span data-stu-id="75190-128">Scenario</span></span>

<span data-ttu-id="75190-129">hello-scenario som beskrivs i den här artikeln hämtar hello topologi svar för en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="75190-129">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="75190-130">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="75190-130">Retrieve Network Watcher</span></span>

<span data-ttu-id="75190-131">hello första steget är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="75190-131">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="75190-132">Hej `$networkWatcher` variabel har skickats toohello `Get-AzureRmNetworkWatcherTopology` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75190-132">hello `$networkWatcher` variable is passed toohello `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a><span data-ttu-id="75190-133">Hämta topologi</span><span class="sxs-lookup"><span data-stu-id="75190-133">Retrieve topology</span></span>

<span data-ttu-id="75190-134">Hej `Get-AzureRmNetworkWatcherTopology` cmdlet hämtar hello topologin för en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="75190-134">hello `Get-AzureRmNetworkWatcherTopology` cmdlet retrieves hello topology for a given resource group.</span></span>

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a><span data-ttu-id="75190-135">Resultat</span><span class="sxs-lookup"><span data-stu-id="75190-135">Results</span></span>

<span data-ttu-id="75190-136">hello resultaten har egenskapen name ”resurser”, som innehåller hello json svarstexten för hello `Get-AzureRmNetworkWatcherTopology` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="75190-136">hello results returned have a property name "Resources", which contains hello json response body for hello `Get-AzureRmNetworkWatcherTopology` cmdlet.</span></span>  <span data-ttu-id="75190-137">hello svaret innehåller hello resurser i hello säkerhetsgrupp för nätverk och deras associationer (det vill säga innehåller, associerade).</span><span class="sxs-lookup"><span data-stu-id="75190-137">hello response contains hello resources in hello Network Security Group and their associations (that is, Contains, Associated).</span></span>

```json
Id              : 00000000-0000-0000-0000-000000000000
CreatedDateTime : 2/1/2017 7:52:21 PM
LastModified    : 2/1/2017 7:46:18 PM
Resources       : [
                    {
                      "Name": "testrg-vnet",
                      "Id":
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet/subnets/default"
                        }
                      ]
                    },
                    {
                      "Name": "default",
                      "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testr
                  g-vnet/subnets/default",
                      "Location": "westcentralus",
                      "Associations": []
                    },
                    {
                      "Name": "testrg-vnet2",
                      "Id": 
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet2",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/default"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "GatewaySubnet",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/GatewaySubnet"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "gateway2",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworkGateways/gateway2"
                        }
                      ]
                    },
                    ...
                  ]
```

## <a name="next-steps"></a><span data-ttu-id="75190-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75190-138">Next steps</span></span>

<span data-ttu-id="75190-139">Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="75190-139">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


