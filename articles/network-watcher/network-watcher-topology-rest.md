---
title: "aaaView Azure Nätverksbevakaren topologi - REST API | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse REST-API tooquery nätverkets topologi."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 39292025bcd561f007c9e31271b1389be48ea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a><span data-ttu-id="99a17-103">Visa Nätverksbevakaren topologi med REST API</span><span class="sxs-lookup"><span data-stu-id="99a17-103">View Network Watcher topology with REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="99a17-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="99a17-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="99a17-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="99a17-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="99a17-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="99a17-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="99a17-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="99a17-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="99a17-108">hello topologi funktion i Nätverksbevakaren ger en bild av hello nätverksresurser i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="99a17-108">hello Topology feature of Network Watcher provides a visual representation of hello network resources in a subscription.</span></span> <span data-ttu-id="99a17-109">Hello-portalen visas den här visualiseringen tooyou automatiskt.</span><span class="sxs-lookup"><span data-stu-id="99a17-109">In hello portal, this visualization is presented tooyou automatically.</span></span> <span data-ttu-id="99a17-110">hello informationen bakom hello topologiska vyn i hello portal kan hämtas via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="99a17-110">hello information behind hello topology view in hello portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="99a17-111">Den här funktionen gör hello topologiinformation mer flexibla hello data kan användas av andra verktyg toobuild ut hello visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="99a17-111">This capability makes hello topology information more versatile as hello data can be consumed by other tools toobuild out hello visualization.</span></span>

<span data-ttu-id="99a17-112">hello sammankoppling modelleras under två relationer.</span><span class="sxs-lookup"><span data-stu-id="99a17-112">hello interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="99a17-113">**Inneslutning** -exempel: innehåller ett undernät för virtuellt nätverk innehåller ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="99a17-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="99a17-114">**Associerade** -exempel: NIC som är kopplad till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="99a17-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="99a17-115">hello finns följande lista egenskaper som returneras när du frågar hello topologi REST API.</span><span class="sxs-lookup"><span data-stu-id="99a17-115">hello following list is properties that are returned when querying hello Topology REST API.</span></span>

* <span data-ttu-id="99a17-116">**namnet** - hello namn på hello resurs</span><span class="sxs-lookup"><span data-stu-id="99a17-116">**name** - hello name of hello resource</span></span>
* <span data-ttu-id="99a17-117">**ID** -hello hello resurs-uri.</span><span class="sxs-lookup"><span data-stu-id="99a17-117">**id** - hello uri of hello resource.</span></span>
* <span data-ttu-id="99a17-118">**plats** -hello plats där hello resurserna finns.</span><span class="sxs-lookup"><span data-stu-id="99a17-118">**location** - hello location where hello resource exists.</span></span>
* <span data-ttu-id="99a17-119">**kopplingarna** -en lista över kopplingar toohello refererar till objektet.</span><span class="sxs-lookup"><span data-stu-id="99a17-119">**associations** - A list of associations toohello referenced object.</span></span>
    * <span data-ttu-id="99a17-120">**namnet** -hello namnet på hello refererar till resursen.</span><span class="sxs-lookup"><span data-stu-id="99a17-120">**name** - hello name of hello referenced resource.</span></span>
    * <span data-ttu-id="99a17-121">**resourceId** -hello resourceId är hello uri hello-resurs som refereras i hello association.</span><span class="sxs-lookup"><span data-stu-id="99a17-121">**resourceId** - hello resourceId is hello uri of hello resource referenced in hello association.</span></span>
    * <span data-ttu-id="99a17-122">**associationType** -hello förhållandet mellan hello underordnat objekt och hello överordnade refererar till det här värdet.</span><span class="sxs-lookup"><span data-stu-id="99a17-122">**associationType** - This value references hello relationship between hello child object and hello parent.</span></span> <span data-ttu-id="99a17-123">Giltiga värden är **innehåller** eller **associerade**.</span><span class="sxs-lookup"><span data-stu-id="99a17-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="99a17-124">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="99a17-124">Before you begin</span></span>

<span data-ttu-id="99a17-125">I det här scenariot kan du hämta hello topologiinformation.</span><span class="sxs-lookup"><span data-stu-id="99a17-125">In this scenario, you retrieve hello topology information.</span></span> <span data-ttu-id="99a17-126">ARMclient är används toocall hello REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="99a17-126">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="99a17-127">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="99a17-127">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="99a17-128">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="99a17-128">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="99a17-129">Scenario</span><span class="sxs-lookup"><span data-stu-id="99a17-129">Scenario</span></span>

<span data-ttu-id="99a17-130">hello-scenario som beskrivs i den här artikeln hämtar hello topologi svar för en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="99a17-130">hello scenario covered in this article retrieves hello topology response for a given resource group.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="99a17-131">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="99a17-131">Log in with ARMClient</span></span>

<span data-ttu-id="99a17-132">Logga in tooarmclient med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="99a17-132">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a><span data-ttu-id="99a17-133">Hämta topologi</span><span class="sxs-lookup"><span data-stu-id="99a17-133">Retrieve topology</span></span>

<span data-ttu-id="99a17-134">följande exempel hello begär hello topologi från hello REST API.</span><span class="sxs-lookup"><span data-stu-id="99a17-134">hello following example requests hello topology from hello REST API.</span></span>  <span data-ttu-id="99a17-135">hello exempel är parametriserade tooallow flexibilitet att skapa ett exempel.</span><span class="sxs-lookup"><span data-stu-id="99a17-135">hello example is parameterized tooallow for flexibility in creating an example.</span></span>  <span data-ttu-id="99a17-136">Ersätter alla värden med \< \> omgivande.</span><span class="sxs-lookup"><span data-stu-id="99a17-136">Replace all values with \< \> surrounding them.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name toorun topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

<span data-ttu-id="99a17-137">hello efter svar är ett exempel på ett kortare svar som returneras när hämta topologin för en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="99a17-137">hello following response is an example of a shortened response that is returned when retrieve topology for a resourcegroup:</span></span>

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a><span data-ttu-id="99a17-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99a17-138">Next steps</span></span>

<span data-ttu-id="99a17-139">Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="99a17-139">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

