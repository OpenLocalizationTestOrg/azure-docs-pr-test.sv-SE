---
title: "Sök efter nästa hopp med Azure-nätverk Watcher nexthop - REST | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta nästa hopptyp är och ip-adressen med nästa hopp med hjälp av Azure REST API"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 644713d365191bf5e51517d0cc565efbc2abc144
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="d3824-103">Ta reda på vilka nästa hopptyp är med nästa hopp-funktionen i Azure Nätverksbevakaren med hjälp av Azure REST API</span><span class="sxs-lookup"><span data-stu-id="d3824-103">Find out what the next hop type is using the Next Hop capability in Aure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="d3824-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3824-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="d3824-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d3824-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="d3824-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d3824-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="d3824-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d3824-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="d3824-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="d3824-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="d3824-109">Nästa hopp är en funktion i Nätverksbevakaren som tillhandahåller möjligheten get nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d3824-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="d3824-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk för att komma till sin destination.</span><span class="sxs-lookup"><span data-stu-id="d3824-110">This capability is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d3824-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="d3824-111">Before you begin</span></span>

<span data-ttu-id="d3824-112">ARMclient används för att anropa REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d3824-112">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="d3824-113">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="d3824-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="d3824-114">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="d3824-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="d3824-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="d3824-115">Scenario</span></span>

<span data-ttu-id="d3824-116">Det scenario som beskrivs i den här artikeln använder nästa hopp, en funktion i Nätverksbevakaren som söker efter nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="d3824-116">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="d3824-117">Läs mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3824-117">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="d3824-118">I det här scenariot kommer du att:</span><span class="sxs-lookup"><span data-stu-id="d3824-118">In this scenario, you will:</span></span>

* <span data-ttu-id="d3824-119">Hämta nästa hopp för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d3824-119">Retrieve the next hop for a virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="d3824-120">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="d3824-120">Log in with ARMClient</span></span>

<span data-ttu-id="d3824-121">Logga in på armclient med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="d3824-121">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="d3824-122">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d3824-122">Retrieve a virtual machine</span></span>

<span data-ttu-id="d3824-123">Kör följande skript för att returnera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d3824-123">Run the following script to return a virtual machine.</span></span> <span data-ttu-id="d3824-124">Den här informationen behövs för att köra nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="d3824-124">This information is needed for running next hop.</span></span>

<span data-ttu-id="d3824-125">Följande kod måste ha värden för följande variabler:</span><span class="sxs-lookup"><span data-stu-id="d3824-125">The following code needs values for the following variables:</span></span>

- <span data-ttu-id="d3824-126">**subscriptionId** -prenumerations-Id ska användas.</span><span class="sxs-lookup"><span data-stu-id="d3824-126">**subscriptionId** - The subscription Id to use.</span></span>
- <span data-ttu-id="d3824-127">**resourceGroupName** -namnet på en resursgrupp som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d3824-127">**resourceGroupName** - The name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="d3824-128">Id för den virtuella datorn används i följande exempel i följande utdata:</span><span class="sxs-lookup"><span data-stu-id="d3824-128">From the following output, the id of the virtual machine is used in the following example:</span></span>

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a><span data-ttu-id="d3824-129">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="d3824-129">Get Next Hop</span></span>

<span data-ttu-id="d3824-130">Nästa hopp från en virtuell dator kan hämtas när authorization-huvud har skapats.</span><span class="sxs-lookup"><span data-stu-id="d3824-130">Once the authorization header is created, the next hop from a virtual machine can be retrieved.</span></span> <span data-ttu-id="d3824-131">Följande värden måste ersättas för exemplet ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d3824-131">The following values must be replaced for the code example to work.</span></span>

> [!Important]
> <span data-ttu-id="d3824-132">För nätverket Watcher REST API-anrop som resursgruppens namn i URI-begäran är resursgruppen som innehåller Nätverksbevakaren, inte resurserna du utför de diagnostiska åtgärderna på.</span><span class="sxs-lookup"><span data-stu-id="d3824-132">For Network Watcher REST API calls the resource group name in the request URI is the resource group that contains the Network Watcher, not the resources you are performing the diagnostic actions on.</span></span>

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> <span data-ttu-id="d3824-133">Nästa hopp kräver att den Virtuella datorresursen har allokerats för att köras.</span><span class="sxs-lookup"><span data-stu-id="d3824-133">Next hop requires that the VM resource is allocated to run.</span></span>

## <a name="results"></a><span data-ttu-id="d3824-134">Resultat</span><span class="sxs-lookup"><span data-stu-id="d3824-134">Results</span></span>

<span data-ttu-id="d3824-135">Följande fragment är ett exempel på utdata togs emot.</span><span class="sxs-lookup"><span data-stu-id="d3824-135">The following snippet is an example of the output received.</span></span> <span data-ttu-id="d3824-136">Det resulterar i följande värden:</span><span class="sxs-lookup"><span data-stu-id="d3824-136">The results contain the following values:</span></span>

* <span data-ttu-id="d3824-137">**nextHopType** -värdet är ett av följande värden: Internet VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway eller None.</span><span class="sxs-lookup"><span data-stu-id="d3824-137">**nextHopType** - This value is one of the following values: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway, or None.</span></span>
* <span data-ttu-id="d3824-138">**nextHopIpAddress** -IP-adressen för nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="d3824-138">**nextHopIpAddress** - The IP address of the next hop.</span></span>
* <span data-ttu-id="d3824-139">**routeTableId** - värdet är antingen en uri för vägtabellen som är associerad med vägen eller om ingen användardefinierad väg är definierad värdet för *Systemväg* returneras.</span><span class="sxs-lookup"><span data-stu-id="d3824-139">**routeTableId** - The value is either a uri for the route table associated with the route, or if no user-defined route is defined the value of *System Route* is returned.</span></span>

<span data-ttu-id="d3824-140">Följande är resultatet i json-format.</span><span class="sxs-lookup"><span data-stu-id="d3824-140">The following are the results in json format.</span></span>

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a><span data-ttu-id="d3824-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3824-141">Next steps</span></span>

<span data-ttu-id="d3824-142">När du har kunnat ta reda på nästa hopp för en virtuell dator, kan du visa säkerheten för dina nätverksresurser genom att besöka [Säkerhetsvyn översikt](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d3824-142">Once you have been able to find out the next hop for a virtual machine, you can view the security of your network resources by visiting [Security View overview](network-watcher-security-group-view-overview.md)</span></span>














