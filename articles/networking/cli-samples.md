---
title: aaaAzure CLI prover | Microsoft Docs
description: Azure CLI-exempel
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 8001b7e72480cfd0122325f7fb81c32aaad072d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-networking"></a><span data-ttu-id="c8dbc-103">Azure CLI-exempel för nätverk</span><span class="sxs-lookup"><span data-stu-id="c8dbc-103">Azure CLI Samples for networking</span></span>

<span data-ttu-id="c8dbc-104">hello innehåller följande tabell länkar toobash skript som skapats med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-104">hello following table includes links toobash scripts built using hello Azure CLI.</span></span>

| | |
|-|-|
|<span data-ttu-id="c8dbc-105">**Anslutning mellan Azure-resurser**</span><span class="sxs-lookup"><span data-stu-id="c8dbc-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="c8dbc-106">Skapa ett virtuellt nätverk för flera nivåer program</span><span class="sxs-lookup"><span data-stu-id="c8dbc-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c8dbc-107">Skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="c8dbc-108">Trafik toohello frontend undernät är begränsad tooHTTP och SSH, medan trafik toohello backend-undernät är begränsad tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-108">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> |
| [<span data-ttu-id="c8dbc-109">Peer-två virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="c8dbc-109">Peer two virtual networks</span></span>](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c8dbc-110">Skapar och ansluter två virtuella nätverk i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-110">Creates and connects two virtual networks in hello same region.</span></span> |
| [<span data-ttu-id="c8dbc-111">Vidarebefordra trafik via en virtuell nätverksenhet</span><span class="sxs-lookup"><span data-stu-id="c8dbc-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c8dbc-112">Skapar ett virtuellt nätverk med frontend och backend-undernät och en virtuell dator som kan tooroute trafik mellan hello två undernät.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-112">Creates a virtual network with front-end and back-end subnets and a VM that is able tooroute traffic between hello two subnets.</span></span> |
| [<span data-ttu-id="c8dbc-113">Filtrera inkommande och utgående nätverkstrafik för VM</span><span class="sxs-lookup"><span data-stu-id="c8dbc-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c8dbc-114">Skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="c8dbc-115">Inkommande nätverkstrafik toohello frontend undernät är begränsad tooHTTP, HTTPS och SSH.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-115">Inbound network traffic toohello front-end subnet is limited tooHTTP, HTTPS and SSH.</span></span> <span data-ttu-id="c8dbc-116">Utgående trafik toohello Internet från hello backend-undernät är inte tillåtet.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-116">Outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="c8dbc-117">**Läsa in belastningsutjämning och trafik riktning**</span><span class="sxs-lookup"><span data-stu-id="c8dbc-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="c8dbc-118">Läsa in belastningsutjämna trafik tooVMs för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="c8dbc-118">Load balance traffic tooVMs for high availability</span></span>](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c8dbc-119">Skapar flera virtuella datorer i en hög tillgänglighet och konfiguration för Utjämning av nätverksbelastning.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="c8dbc-120">Belastningsutjämning för flera webbplatser på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c8dbc-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="c8dbc-121">Skapar två virtuella datorer med flera IP-konfigurationer, domänanslutna tooan Azure Tillgänglighetsuppsättning, tillgängligt via en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-121">Creates two VMs with multiple IP configurations, joined tooan Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="c8dbc-122">Direkt-trafik över flera regioner för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="c8dbc-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="c8dbc-123">Skapar två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="c8dbc-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
