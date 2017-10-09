---
title: "aaaFind nästa hopp med Azure Network Watcher nexthop - Azure-portalen | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och ip-adressen med nästa hopp med hello Azure-portalen"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b64a2a5275c15aa8bdb10601de4ae1504a9ab551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-hello-portal"></a><span data-ttu-id="570e7-103">Ta reda på vilka hello nästa hopptyp med hello nästa hopp-funktionen i Azure Nätverksbevakaren med hello-portalen</span><span class="sxs-lookup"><span data-stu-id="570e7-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using hello portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="570e7-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="570e7-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="570e7-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="570e7-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="570e7-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="570e7-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="570e7-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="570e7-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="570e7-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="570e7-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="570e7-109">Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="570e7-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="570e7-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.</span><span class="sxs-lookup"><span data-stu-id="570e7-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="570e7-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="570e7-111">Before you begin</span></span>

<span data-ttu-id="570e7-112">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="570e7-112">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="570e7-113">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="570e7-113">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="570e7-114">Scenario</span><span class="sxs-lookup"><span data-stu-id="570e7-114">Scenario</span></span>

<span data-ttu-id="570e7-115">hello-scenario som beskrivs i den här artikeln används nästa hopp toofind hello nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="570e7-115">hello scenario covered in this article uses Next hop toofind out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="570e7-116">Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="570e7-116">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="570e7-117">I det här scenariot kommer du att:</span><span class="sxs-lookup"><span data-stu-id="570e7-117">In this scenario, you will:</span></span>

* <span data-ttu-id="570e7-118">Hämta hello nästa hopp från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="570e7-118">Retrieve hello next hop from a virtual machine.</span></span>

## <a name="get-next-hop"></a><span data-ttu-id="570e7-119">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="570e7-119">Get Next Hop</span></span>

### <a name="step-1"></a><span data-ttu-id="570e7-120">Steg 1</span><span class="sxs-lookup"><span data-stu-id="570e7-120">Step 1</span></span>

<span data-ttu-id="570e7-121">Navigera tooyour Nätverksbevakaren resurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="570e7-121">Navigate tooyour Network Watcher resource in hello Azure portal.</span></span>

### <a name="step-2"></a><span data-ttu-id="570e7-122">Steg 2</span><span class="sxs-lookup"><span data-stu-id="570e7-122">Step 2</span></span>

<span data-ttu-id="570e7-123">Klicka på **nästa hopp** i hello navigeringsfönstret, Välj hello virtuell dator och nätverksgränssnitt, fyller du i hello källa och mål-IP och klicka på hello **nästa hopp** knappen.</span><span class="sxs-lookup"><span data-stu-id="570e7-123">Click **Next hop** in hello navigation pane, select hello virtual machine and network interface, fill out hello source and destination IP, and click hello **Next hop** button.</span></span>

> [!NOTE]
> <span data-ttu-id="570e7-124">Nästa hopp kräver att hello Virtuella datorresursen fördelas toorun.</span><span class="sxs-lookup"><span data-stu-id="570e7-124">Next hop requires that hello VM resource is allocated toorun.</span></span>

![Hämta nästa hopp-översikt][1]

### <a name="step-3"></a><span data-ttu-id="570e7-126">Steg 3</span><span class="sxs-lookup"><span data-stu-id="570e7-126">Step 3</span></span>

<span data-ttu-id="570e7-127">När hello åtgärden är klar, tillhandahålls hello resultat.</span><span class="sxs-lookup"><span data-stu-id="570e7-127">Once hello task is complete, hello results are provided.</span></span> <span data-ttu-id="570e7-128">Hej IP-adress och typ av enhet hello nästa hopp är visas.</span><span class="sxs-lookup"><span data-stu-id="570e7-128">hello IP address and type of device hello next hop is, is displayed.</span></span> <span data-ttu-id="570e7-129">hello följande tabell visar hello tillgängliga returnerade värden i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="570e7-129">hello following table shows hello available returned values in hello portal.</span></span>

<span data-ttu-id="570e7-130">**Nästa Hopptyp**</span><span class="sxs-lookup"><span data-stu-id="570e7-130">**Next Hop Type**</span></span>

* <span data-ttu-id="570e7-131">Internet</span><span class="sxs-lookup"><span data-stu-id="570e7-131">Internet</span></span>
* <span data-ttu-id="570e7-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="570e7-132">VirtualAppliance</span></span>
* <span data-ttu-id="570e7-133">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="570e7-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="570e7-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="570e7-134">VnetLocal</span></span>
* <span data-ttu-id="570e7-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="570e7-135">HyperNetGateway</span></span>
* <span data-ttu-id="570e7-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="570e7-136">VnetPeering</span></span>
* <span data-ttu-id="570e7-137">Ingen</span><span class="sxs-lookup"><span data-stu-id="570e7-137">None</span></span>

<span data-ttu-id="570e7-138">Om en anpassad väg har använt tooroute visas den här trafiken hello användardefinierad väg (UDR) samt med hello resultat.</span><span class="sxs-lookup"><span data-stu-id="570e7-138">If a custom route was used tooroute this traffic, hello User-defined route (UDR) is shown as well with hello results.</span></span>

![Nästa hopp-resultat][2]

## <a name="next-steps"></a><span data-ttu-id="570e7-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="570e7-140">Next steps</span></span>

<span data-ttu-id="570e7-141">Lär dig hur tooreview säkerhet grupp nätverksinställningarna via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="570e7-141">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














