---
title: aaaAzure PowerShell-exempel | Microsoft Docs
description: Azure PowerShell-exempel
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: georgewallace
ms.openlocfilehash: 130a6e755691b46a9549cad5acaa5bde4fe38e95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-networking"></a><span data-ttu-id="f29be-103">Azure PowerShell-exempel för nätverk</span><span class="sxs-lookup"><span data-stu-id="f29be-103">Azure PowerShell Samples for networking</span></span>

<span data-ttu-id="f29be-104">hello innehåller följande tabell länkar tooscripts som skapats med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f29be-104">hello following table includes links tooscripts built using Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="f29be-105">**Anslutning mellan Azure-resurser**</span><span class="sxs-lookup"><span data-stu-id="f29be-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="f29be-106">Skapa ett virtuellt nätverk för flera nivåer program</span><span class="sxs-lookup"><span data-stu-id="f29be-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="f29be-107">Skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="f29be-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="f29be-108">Trafik toohello frontend undernät är begränsad tooHTTP, medan trafik toohello backend-undernät är begränsad tooSQL, port 1433.</span><span class="sxs-lookup"><span data-stu-id="f29be-108">Traffic toohello front-end subnet is limited tooHTTP, while traffic toohello back-end subnet is limited tooSQL, port 1433.</span></span> |
| [<span data-ttu-id="f29be-109">Peer-två virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="f29be-109">Peer two virtual networks</span></span>](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="f29be-110">Skapar och ansluter två virtuella nätverk i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="f29be-110">Creates and connects two virtual networks in hello same region.</span></span> |
| [<span data-ttu-id="f29be-111">Vidarebefordra trafik via en virtuell nätverksenhet</span><span class="sxs-lookup"><span data-stu-id="f29be-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="f29be-112">Skapar ett virtuellt nätverk med frontend och backend-undernät och en virtuell dator som kan tooroute trafik mellan hello två undernät.</span><span class="sxs-lookup"><span data-stu-id="f29be-112">Creates a virtual network with front-end and back-end subnets and a VM that is able tooroute traffic between hello two subnets.</span></span> |
| [<span data-ttu-id="f29be-113">Filtrera inkommande och utgående nätverkstrafik för VM</span><span class="sxs-lookup"><span data-stu-id="f29be-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="f29be-114">Skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="f29be-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="f29be-115">Inkommande nätverkstrafik toohello frontend undernät är begränsad tooHTTP och HTTPS...</span><span class="sxs-lookup"><span data-stu-id="f29be-115">Inbound network traffic toohello front-end subnet is limited tooHTTP and HTTPS..</span></span> <span data-ttu-id="f29be-116">Utgående trafik toohello Internet från hello backend-undernät är inte tillåtet.</span><span class="sxs-lookup"><span data-stu-id="f29be-116">Outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="f29be-117">**Läsa in belastningsutjämning och trafik riktning**</span><span class="sxs-lookup"><span data-stu-id="f29be-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="f29be-118">Läsa in belastningsutjämna trafik tooVMs för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="f29be-118">Load balance traffic tooVMs for high availability</span></span>](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="f29be-119">Skapar flera virtuella datorer i en hög tillgänglighet och konfiguration för Utjämning av nätverksbelastning.</span><span class="sxs-lookup"><span data-stu-id="f29be-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="f29be-120">Belastningsutjämning för flera webbplatser på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f29be-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="f29be-121">Skapar två virtuella datorer med flera IP-konfigurationer, domänanslutna tooan Azure Tillgänglighetsuppsättning, tillgängligt via en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f29be-121">Creates two VMs with multiple IP configurations, joined tooan Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="f29be-122">Direkt-trafik över flera regioner för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="f29be-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="f29be-123">Skapar två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="f29be-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
