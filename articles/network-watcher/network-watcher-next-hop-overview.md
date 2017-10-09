---
title: "aaaIntroduction toonext hopp i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren nästa hopp-funktion"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 916af736d0d52abadeafed746f0f8a0173b11033
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonext-hop-in-azure-network-watcher"></a><span data-ttu-id="d6ff4-103">Introduktion toonext hopp i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="d6ff4-103">Introduction toonext hop in Azure Network Watcher</span></span>

<span data-ttu-id="d6ff4-104">Trafik från en virtuell dator skickas tooa mål baserat på hello effektiva vägar som är associerade med ett nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="d6ff4-104">Traffic from a VM is sent tooa destination based on hello effective routes associated with a NIC.</span></span> <span data-ttu-id="d6ff4-105">Nästa hopp hämtar hello nästa hopptyp och IP-adressen för ett paket från en specifik virtuell dator och nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="d6ff4-105">Next hop gets hello next hop type and IP address of a packet from a specific virtual machine and NIC.</span></span> <span data-ttu-id="d6ff4-106">Detta hjälper toodetermine om hello paketet tas dirigerad toohello mål eller är hello trafik som svart holed.</span><span class="sxs-lookup"><span data-stu-id="d6ff4-106">This helps toodetermine if hello packet is being directed toohello destination or is hello traffic being black holed.</span></span> <span data-ttu-id="d6ff4-107">En felaktig konfiguration av vägar av hello användare, där en trafik är riktat tooan lokal plats eller en virtuell installation, kan leda tooconnectivity problem.</span><span class="sxs-lookup"><span data-stu-id="d6ff4-107">An improper configuration of routes by hello user, where a traffic is directed tooan on-premises location or a virtual appliance, can lead tooconnectivity issues.</span></span> <span data-ttu-id="d6ff4-108">Nästa hopp returnerar också hello vägtabell associerad med hello nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="d6ff4-108">Next hop also returns hello route table associated with hello next hop.</span></span> <span data-ttu-id="d6ff4-109">När du frågar ett nexthop om hello flödet definieras som en användardefinierad väg, returneras den rutten.</span><span class="sxs-lookup"><span data-stu-id="d6ff4-109">When querying a next hop if hello route is defined as a user-defined route, that route will be returned.</span></span> <span data-ttu-id="d6ff4-110">Annars returnerar nästa hopp ”Systemväg”.</span><span class="sxs-lookup"><span data-stu-id="d6ff4-110">Otherwise Next hop returns "System Route".</span></span>

![Nästa hopp-översikt][1]

<span data-ttu-id="d6ff4-112">hello följer en lista över hello nästa hopptyper som kan returneras när du frågar nästa hopp.</span><span class="sxs-lookup"><span data-stu-id="d6ff4-112">hello following is a list of hello next hop types that can be returned when querying Next hop.</span></span>

* <span data-ttu-id="d6ff4-113">Internet</span><span class="sxs-lookup"><span data-stu-id="d6ff4-113">Internet</span></span>
* <span data-ttu-id="d6ff4-114">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="d6ff4-114">VirtualAppliance</span></span>
* <span data-ttu-id="d6ff4-115">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="d6ff4-115">VirtualNetworkGateway</span></span>
* <span data-ttu-id="d6ff4-116">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="d6ff4-116">VnetLocal</span></span>
* <span data-ttu-id="d6ff4-117">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="d6ff4-117">HyperNetGateway</span></span>
* <span data-ttu-id="d6ff4-118">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="d6ff4-118">VnetPeering</span></span>
* <span data-ttu-id="d6ff4-119">Ingen</span><span class="sxs-lookup"><span data-stu-id="d6ff4-119">None</span></span>

### <a name="next-steps"></a><span data-ttu-id="d6ff4-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6ff4-120">Next steps</span></span>

<span data-ttu-id="d6ff4-121">Lär dig hur toouse nästa hopp toofind problem med nätverksanslutningen genom att besöka [Kontrollera hello nästa hopp på en virtuell dator](network-watcher-check-next-hop-portal.md)</span><span class="sxs-lookup"><span data-stu-id="d6ff4-121">Learn how toouse next hop toofind issues with network connectivity by visiting [Check hello next hop on a VM](network-watcher-check-next-hop-portal.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













