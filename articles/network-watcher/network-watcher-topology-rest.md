---
title: "Visa Azure Nätverksbevakaren topologi - REST API | Microsoft Docs"
description: "Den här artikeln beskrivs hur du använder REST API för att fråga nätverkets topologi."
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
ms.openlocfilehash: 568f3060da372f4a08cec342e04359172522cb69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a><span data-ttu-id="a2b62-103">Visa Nätverksbevakaren topologi med REST API</span><span class="sxs-lookup"><span data-stu-id="a2b62-103">View Network Watcher topology with REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="a2b62-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a2b62-104">PowerShell</span></span>](network-watcher-topology-powershell.md)
> - [<span data-ttu-id="a2b62-105">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="a2b62-105">CLI 1.0</span></span>](network-watcher-topology-cli-nodejs.md)
> - [<span data-ttu-id="a2b62-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a2b62-106">CLI 2.0</span></span>](network-watcher-topology-cli.md)
> - [<span data-ttu-id="a2b62-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="a2b62-107">REST API</span></span>](network-watcher-topology-rest.md)

<span data-ttu-id="a2b62-108">Funktionen topologi i Nätverksbevakaren ger en bild av nätverksresurser i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a2b62-108">The Topology feature of Network Watcher provides a visual representation of the network resources in a subscription.</span></span> <span data-ttu-id="a2b62-109">I portalen visas den här visualiseringen för dig automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a2b62-109">In the portal, this visualization is presented to you automatically.</span></span> <span data-ttu-id="a2b62-110">Informationen bakom vyn topologi i portalen kan hämtas via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a2b62-110">The information behind the topology view in the portal can be retrieved through PowerShell.</span></span>
<span data-ttu-id="a2b62-111">Den här funktionen gör topologiinformationen mer flexibla data kan användas av andra verktyg för att bygga ut visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="a2b62-111">This capability makes the topology information more versatile as the data can be consumed by other tools to build out the visualization.</span></span>

<span data-ttu-id="a2b62-112">Sambandet modelleras under två relationer.</span><span class="sxs-lookup"><span data-stu-id="a2b62-112">The interconnection is modeled under two relationships.</span></span>

- <span data-ttu-id="a2b62-113">**Inneslutning** -exempel: innehåller ett undernät för virtuellt nätverk innehåller ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="a2b62-113">**Containment** - Example: VNet contains a Subnet contains a NIC</span></span>
- <span data-ttu-id="a2b62-114">**Associerade** -exempel: NIC som är kopplad till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a2b62-114">**Associated** - Example: NIC is associated with a VM</span></span>

<span data-ttu-id="a2b62-115">I följande lista finns egenskaper som returneras när du frågar topologi REST API.</span><span class="sxs-lookup"><span data-stu-id="a2b62-115">The following list is properties that are returned when querying the Topology REST API.</span></span>

* <span data-ttu-id="a2b62-116">**namnet** -namnet på resursen</span><span class="sxs-lookup"><span data-stu-id="a2b62-116">**name** - The name of the resource</span></span>
* <span data-ttu-id="a2b62-117">**ID** -uri för resursen.</span><span class="sxs-lookup"><span data-stu-id="a2b62-117">**id** - The uri of the resource.</span></span>
* <span data-ttu-id="a2b62-118">**plats** -plats där resurserna finns.</span><span class="sxs-lookup"><span data-stu-id="a2b62-118">**location** - The location where the resource exists.</span></span>
* <span data-ttu-id="a2b62-119">**kopplingarna** -en lista över kopplingar till det refererade objektet.</span><span class="sxs-lookup"><span data-stu-id="a2b62-119">**associations** - A list of associations to the referenced object.</span></span>
    * <span data-ttu-id="a2b62-120">**namnet** -namnet på den refererade resursen.</span><span class="sxs-lookup"><span data-stu-id="a2b62-120">**name** - The name of the referenced resource.</span></span>
    * <span data-ttu-id="a2b62-121">**resourceId** -resourceId är URI: n för den resurs som refereras i kopplingen.</span><span class="sxs-lookup"><span data-stu-id="a2b62-121">**resourceId** - The resourceId is the uri of the resource referenced in the association.</span></span>
    * <span data-ttu-id="a2b62-122">**associationType** -relationen mellan det underordnade objektet och överordnat refererar till det här värdet.</span><span class="sxs-lookup"><span data-stu-id="a2b62-122">**associationType** - This value references the relationship between the child object and the parent.</span></span> <span data-ttu-id="a2b62-123">Giltiga värden är **innehåller** eller **associerade**.</span><span class="sxs-lookup"><span data-stu-id="a2b62-123">Valid values are **Contains** or **Associated**.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a2b62-124">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a2b62-124">Before you begin</span></span>

<span data-ttu-id="a2b62-125">I det här scenariot kan du hämta topologiinformationen.</span><span class="sxs-lookup"><span data-stu-id="a2b62-125">In this scenario, you retrieve the topology information.</span></span> <span data-ttu-id="a2b62-126">ARMclient används för att anropa REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a2b62-126">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="a2b62-127">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="a2b62-127">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="a2b62-128">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="a2b62-128">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="a2b62-129">Scenario</span><span class="sxs-lookup"><span data-stu-id="a2b62-129">Scenario</span></span>

<span data-ttu-id="a2b62-130">Det scenario som beskrivs i den här artikeln hämtar topologi-svar för en viss resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a2b62-130">The scenario covered in this article retrieves the topology response for a given resource group.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="a2b62-131">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="a2b62-131">Log in with ARMClient</span></span>

<span data-ttu-id="a2b62-132">Logga in på armclient med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="a2b62-132">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a><span data-ttu-id="a2b62-133">Hämta topologi</span><span class="sxs-lookup"><span data-stu-id="a2b62-133">Retrieve topology</span></span>

<span data-ttu-id="a2b62-134">I följande exempel begär topologin från REST API.</span><span class="sxs-lookup"><span data-stu-id="a2b62-134">The following example requests the topology from the REST API.</span></span>  <span data-ttu-id="a2b62-135">Exemplet parametriserade för att möjliggöra flexibilitet för att skapa ett exempel.</span><span class="sxs-lookup"><span data-stu-id="a2b62-135">The example is parameterized to allow for flexibility in creating an example.</span></span>  <span data-ttu-id="a2b62-136">Ersätter alla värden med \< \> omgivande.</span><span class="sxs-lookup"><span data-stu-id="a2b62-136">Replace all values with \< \> surrounding them.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name to run topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

<span data-ttu-id="a2b62-137">Följande svar är ett exempel på ett kortare svar som returneras när hämta topologin för en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="a2b62-137">The following response is an example of a shortened response that is returned when retrieve topology for a resourcegroup:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a2b62-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a2b62-138">Next steps</span></span>

<span data-ttu-id="a2b62-139">Lär dig hur du visualisera dina NSG flödet loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="a2b62-139">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

