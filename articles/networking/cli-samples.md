---
title: Azure CLI-exempel | Microsoft Docs
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
ms.openlocfilehash: 7977460f61bfdabd399e45e86d9bbf2e5083992b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-samples-for-networking"></a><span data-ttu-id="35acc-103">Azure CLI-exempel för nätverk</span><span class="sxs-lookup"><span data-stu-id="35acc-103">Azure CLI Samples for networking</span></span>

<span data-ttu-id="35acc-104">Följande tabell innehåller länkar till bash-skript som skapats med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="35acc-104">The following table includes links to bash scripts built using the Azure CLI.</span></span>

| | |
|-|-|
|<span data-ttu-id="35acc-105">**Anslutning mellan Azure-resurser**</span><span class="sxs-lookup"><span data-stu-id="35acc-105">**Connectivity between Azure resources**</span></span>||
| [<span data-ttu-id="35acc-106">Skapa ett virtuellt nätverk för flera nivåer program</span><span class="sxs-lookup"><span data-stu-id="35acc-106">Create a virtual network for multi-tier applications</span></span>](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="35acc-107">Skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="35acc-107">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="35acc-108">Trafik till undernätet för frontend är begränsad till http- och SSH, medan trafik till backend-undernät är begränsad till MySQL port 3306.</span><span class="sxs-lookup"><span data-stu-id="35acc-108">Traffic to the front-end subnet is limited to HTTP and SSH, while traffic to the back-end subnet is limited to MySQL, port 3306.</span></span> |
| [<span data-ttu-id="35acc-109">Peer-två virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="35acc-109">Peer two virtual networks</span></span>](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="35acc-110">Skapar och ansluter två virtuella nätverk i samma region.</span><span class="sxs-lookup"><span data-stu-id="35acc-110">Creates and connects two virtual networks in the same region.</span></span> |
| [<span data-ttu-id="35acc-111">Vidarebefordra trafik via en virtuell nätverksenhet</span><span class="sxs-lookup"><span data-stu-id="35acc-111">Route traffic through a network virtual appliance</span></span>](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="35acc-112">Skapar ett virtuellt nätverk med frontend och backend-undernät och en virtuell dator som kan vidarebefordra trafik mellan två undernät.</span><span class="sxs-lookup"><span data-stu-id="35acc-112">Creates a virtual network with front-end and back-end subnets and a VM that is able to route traffic between the two subnets.</span></span> |
| [<span data-ttu-id="35acc-113">Filtrera inkommande och utgående nätverkstrafik för VM</span><span class="sxs-lookup"><span data-stu-id="35acc-113">Filter inbound and outbound VM network traffic</span></span>](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="35acc-114">Skapar ett virtuellt nätverk med frontend och backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="35acc-114">Creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="35acc-115">Inkommande nätverkstrafik till undernätet frontend är begränsad till HTTP, HTTPS och SSH.</span><span class="sxs-lookup"><span data-stu-id="35acc-115">Inbound network traffic to the front-end subnet is limited to HTTP, HTTPS and SSH.</span></span> <span data-ttu-id="35acc-116">Utgående trafik till Internet från backend-undernät är inte tillåtet.</span><span class="sxs-lookup"><span data-stu-id="35acc-116">Outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> |
|<span data-ttu-id="35acc-117">**Läsa in belastningsutjämning och trafik riktning**</span><span class="sxs-lookup"><span data-stu-id="35acc-117">**Load balancing and traffic direction**</span></span>||
| [<span data-ttu-id="35acc-118">Läs in Utjämna trafiken till virtuella datorer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="35acc-118">Load balance traffic to VMs for high availability</span></span>](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="35acc-119">Skapar flera virtuella datorer i en hög tillgänglighet och konfiguration för Utjämning av nätverksbelastning.</span><span class="sxs-lookup"><span data-stu-id="35acc-119">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="35acc-120">Belastningsutjämning för flera webbplatser på virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="35acc-120">Load balance multiple websites on VMs</span></span>](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | <span data-ttu-id="35acc-121">Skapar två virtuella datorer med flera IP-konfigurationer, ansluten till en Azure Tillgänglighetsuppsättning, tillgängligt via en Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="35acc-121">Creates two VMs with multiple IP configurations, joined to an Azure Availability Set, accessible through an Azure Load Balancer.</span></span> |
| [<span data-ttu-id="35acc-122">Direkt-trafik över flera regioner för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="35acc-122">Direct traffic across multiple regions for high application availability</span></span>](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  <span data-ttu-id="35acc-123">Skapar två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="35acc-123">Creates two app service plans, two web apps, a traffic manager profile, and two traffic manager endpoints.</span></span> |
| | |
