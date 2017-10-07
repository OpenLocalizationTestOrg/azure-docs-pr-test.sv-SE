---
title: "aaaFind nästa hopp med Azure Network Watcher nästa hopp - REST | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och ip-adressen med nästa hopp med hello Azure REST API"
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
ms.openlocfilehash: a2b61b355aae8ae513ebd44837184fbc6cfd668c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="e45ab-103">Ta reda på vilka hello nästa hopptyp med hello nexthop-funktionen i Azure Nätverksbevakaren med hjälp av Azure REST API</span><span class="sxs-lookup"><span data-stu-id="e45ab-103">Find out what hello next hop type is using hello Next Hop capability in Aure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="e45ab-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e45ab-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="e45ab-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e45ab-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="e45ab-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e45ab-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="e45ab-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e45ab-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="e45ab-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="e45ab-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="e45ab-109">Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e45ab-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="e45ab-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.</span><span class="sxs-lookup"><span data-stu-id="e45ab-110">This capability is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="e45ab-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="e45ab-111">Before you begin</span></span>

<span data-ttu-id="e45ab-112">ARMclient är används toocall hello REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e45ab-112">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="e45ab-113">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="e45ab-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="e45ab-114">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="e45ab-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="e45ab-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="e45ab-115">Scenario</span></span>

<span data-ttu-id="e45ab-116">hello-scenario som beskrivs i den här artikeln används nästa hopp, en funktion i Nätverksbevakaren som söker efter hello nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="e45ab-116">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="e45ab-117">Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e45ab-117">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="e45ab-118">I det här scenariot kommer du att:</span><span class="sxs-lookup"><span data-stu-id="e45ab-118">In this scenario, you will:</span></span>

* <span data-ttu-id="e45ab-119">Hämta hello nästa hopp för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e45ab-119">Retrieve hello next hop for a virtual machine.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="e45ab-120">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="e45ab-120">Log in with ARMClient</span></span>

<span data-ttu-id="e45ab-121">Logga in tooarmclient med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="e45ab-121">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="e45ab-122">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e45ab-122">Retrieve a virtual machine</span></span>

<span data-ttu-id="e45ab-123">Kör följande skript tooreturn hello en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e45ab-123">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="e45ab-124">Den här informationen behövs för att köra nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="e45ab-124">This information is needed for running next hop.</span></span>

<span data-ttu-id="e45ab-125">följande kod hello måste värden för hello följande variabler:</span><span class="sxs-lookup"><span data-stu-id="e45ab-125">hello following code needs values for hello following variables:</span></span>

- <span data-ttu-id="e45ab-126">**subscriptionId** -hello prenumerations-Id toouse.</span><span class="sxs-lookup"><span data-stu-id="e45ab-126">**subscriptionId** - hello subscription Id toouse.</span></span>
- <span data-ttu-id="e45ab-127">**resourceGroupName** - hello namn på en resursgrupp som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e45ab-127">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="e45ab-128">Från hello följande utdata, används hello id för hello virtuell dator i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e45ab-128">From hello following output, hello id of hello virtual machine is used in hello following example:</span></span>

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

## <a name="get-next-hop"></a><span data-ttu-id="e45ab-129">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="e45ab-129">Get Next Hop</span></span>

<span data-ttu-id="e45ab-130">När du har skapat hello authorization-huvud kan hello nästa hopp från en virtuell dator hämtas.</span><span class="sxs-lookup"><span data-stu-id="e45ab-130">Once hello authorization header is created, hello next hop from a virtual machine can be retrieved.</span></span> <span data-ttu-id="e45ab-131">hello måste följande värden ersättas för hello kod exempel toowork.</span><span class="sxs-lookup"><span data-stu-id="e45ab-131">hello following values must be replaced for hello code example toowork.</span></span>

> [!Important]
> <span data-ttu-id="e45ab-132">För nätverket Watcher REST API-anrop hello resursgruppens namn i hello Begärd URI är hello resursgruppen som innehåller hello Nätverksbevakaren, inte hello resurser som du utför hello diagnostiska åtgärder på.</span><span class="sxs-lookup"><span data-stu-id="e45ab-132">For Network Watcher REST API calls hello resource group name in hello request URI is hello resource group that contains hello Network Watcher, not hello resources you are performing hello diagnostic actions on.</span></span>

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
> <span data-ttu-id="e45ab-133">Nästa hopp kräver att hello Virtuella datorresursen fördelas toorun.</span><span class="sxs-lookup"><span data-stu-id="e45ab-133">Next hop requires that hello VM resource is allocated toorun.</span></span>

## <a name="results"></a><span data-ttu-id="e45ab-134">Resultat</span><span class="sxs-lookup"><span data-stu-id="e45ab-134">Results</span></span>

<span data-ttu-id="e45ab-135">hello är följande fragment ett exempel på hello utdata togs emot.</span><span class="sxs-lookup"><span data-stu-id="e45ab-135">hello following snippet is an example of hello output received.</span></span> <span data-ttu-id="e45ab-136">hello resulterar hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="e45ab-136">hello results contain hello following values:</span></span>

* <span data-ttu-id="e45ab-137">**nextHopType** -värdet är ett av följande värden hello: Internet VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway eller None.</span><span class="sxs-lookup"><span data-stu-id="e45ab-137">**nextHopType** - This value is one of hello following values: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway, or None.</span></span>
* <span data-ttu-id="e45ab-138">**nextHopIpAddress** -hello IP-adressen för hello nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="e45ab-138">**nextHopIpAddress** - hello IP address of hello next hop.</span></span>
* <span data-ttu-id="e45ab-139">**routeTableId** - hello-värdet är antingen en uri för hello vägtabell associerad med hello väg, eller om ingen användardefinierad väg definierade hello värdet för *Systemväg* returneras.</span><span class="sxs-lookup"><span data-stu-id="e45ab-139">**routeTableId** - hello value is either a uri for hello route table associated with hello route, or if no user-defined route is defined hello value of *System Route* is returned.</span></span>

<span data-ttu-id="e45ab-140">hello följande finns hello resultat i json-format.</span><span class="sxs-lookup"><span data-stu-id="e45ab-140">hello following are hello results in json format.</span></span>

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a><span data-ttu-id="e45ab-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e45ab-141">Next steps</span></span>

<span data-ttu-id="e45ab-142">När du har kan toofind ut hello nästa hopp för en virtuell dator kan du visa hello säkerheten för dina nätverksresurser genom att besöka [Säkerhetsvyn översikt](network-watcher-security-group-view-overview.md)</span><span class="sxs-lookup"><span data-stu-id="e45ab-142">Once you have been able toofind out hello next hop for a virtual machine, you can view hello security of your network resources by visiting [Security View overview](network-watcher-security-group-view-overview.md)</span></span>














