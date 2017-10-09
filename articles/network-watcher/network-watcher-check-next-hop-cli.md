---
title: "aaaFind nästa hopp med Azure Network Watcher nexthop - Azure CLI 2.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och IP-adress med hjälp av nästa hopp med Azure CLI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 77c2bde51274bd5c64e7a2467f95139af620ca30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="e32d4-103">Ta reda på vilka hello nästa hopptyp med hello nexthop-funktionen i Azure Nätverksbevakaren använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e32d4-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="e32d4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e32d4-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="e32d4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e32d4-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="e32d4-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e32d4-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="e32d4-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e32d4-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="e32d4-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="e32d4-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="e32d4-109">Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e32d4-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="e32d4-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.</span><span class="sxs-lookup"><span data-stu-id="e32d4-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

<span data-ttu-id="e32d4-111">Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="e32d4-111">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="e32d4-112">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="e32d4-112">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="e32d4-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="e32d4-113">Before you begin</span></span>

<span data-ttu-id="e32d4-114">I det här scenariot använder du hello Azure CLI toofind hello nästa hopptyp och IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e32d4-114">In this scenario, you will use hello Azure CLI toofind hello next hop type and IP address.</span></span>

<span data-ttu-id="e32d4-115">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="e32d4-115">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="e32d4-116">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="e32d4-116">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="e32d4-117">Scenario</span><span class="sxs-lookup"><span data-stu-id="e32d4-117">Scenario</span></span>

<span data-ttu-id="e32d4-118">hello-scenario som beskrivs i den här artikeln används nästa hopp, en funktion i Nätverksbevakaren som söker efter hello nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="e32d4-118">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="e32d4-119">Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e32d4-119">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="e32d4-120">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="e32d4-120">Get Next Hop</span></span>

<span data-ttu-id="e32d4-121">tooget hello nästa hopp vi kallar hello `az network watcher show-next-hop` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e32d4-121">tooget hello next hop we call hello `az network watcher show-next-hop` cmdlet.</span></span> <span data-ttu-id="e32d4-122">Vi skickar hello cmdlet hello Nätverksbevakaren resursgrupp, hello NetworkWatcher, virtuella Id, källans IP-adress och mål-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="e32d4-122">We pass hello cmdlet hello Network Watcher resource group, hello NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="e32d4-123">I det här exemplet är IP-måladress hello tooa VM i ett annat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e32d4-123">In this example, hello destination IP address is tooa VM in another virtual network.</span></span> <span data-ttu-id="e32d4-124">Det finns en virtuell nätverksgateway mellan hello två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e32d4-124">There is a virtual network gateway between hello two virtual networks.</span></span>

<span data-ttu-id="e32d4-125">Om du inte har gjort det ännu, installerar och konfigurerar hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="e32d4-125">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="e32d4-126">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="e32d4-126">Then run hello following command:</span></span>

```azurecli
az network watcher show-next-hop --resource-group <resourcegroupName> --vm <vmNameorID> --source-ip <source-ip> --dest-ip <destination-ip>

```

> [!NOTE]
<span data-ttu-id="e32d4-127">Om hello VM har flera nätverkskort och IP-vidarebefordring är aktiverad på någon av hello nätverkskort, sedan hello NIC-parameter (-i nic-id) måste anges.</span><span class="sxs-lookup"><span data-stu-id="e32d4-127">If hello VM has multiple NICs and IP forwarding is enabled on any of hello NICs, then hello NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="e32d4-128">Annars är valfria.</span><span class="sxs-lookup"><span data-stu-id="e32d4-128">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="e32d4-129">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="e32d4-129">Review results</span></span>

<span data-ttu-id="e32d4-130">När du är färdig tillhandahålls hello resultat.</span><span class="sxs-lookup"><span data-stu-id="e32d4-130">When complete, hello results are provided.</span></span> <span data-ttu-id="e32d4-131">hello nästa hopp IP-adress returneras samt hello typ av resurs som det är.</span><span class="sxs-lookup"><span data-stu-id="e32d4-131">hello next hop IP address is returned as well as hello type of resource it is.</span></span>

```azurecli
{
    "nextHopIpAddress": null,
    "nextHopType": "Internet",
    "routeTableId": "System Route"
}
```

<span data-ttu-id="e32d4-132">hello visar följande lista hello tillgängliga NextHopType värden:</span><span class="sxs-lookup"><span data-stu-id="e32d4-132">hello following list shows hello currently available NextHopType values:</span></span>

<span data-ttu-id="e32d4-133">**Nästa Hopptyp**</span><span class="sxs-lookup"><span data-stu-id="e32d4-133">**Next Hop Type**</span></span>

* <span data-ttu-id="e32d4-134">Internet</span><span class="sxs-lookup"><span data-stu-id="e32d4-134">Internet</span></span>
* <span data-ttu-id="e32d4-135">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="e32d4-135">VirtualAppliance</span></span>
* <span data-ttu-id="e32d4-136">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="e32d4-136">VirtualNetworkGateway</span></span>
* <span data-ttu-id="e32d4-137">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="e32d4-137">VnetLocal</span></span>
* <span data-ttu-id="e32d4-138">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="e32d4-138">HyperNetGateway</span></span>
* <span data-ttu-id="e32d4-139">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="e32d4-139">VnetPeering</span></span>
* <span data-ttu-id="e32d4-140">Ingen</span><span class="sxs-lookup"><span data-stu-id="e32d4-140">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="e32d4-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e32d4-141">Next steps</span></span>

<span data-ttu-id="e32d4-142">Lär dig hur tooreview säkerhet grupp nätverksinställningarna via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="e32d4-142">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
