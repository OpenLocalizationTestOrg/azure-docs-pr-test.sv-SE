---
title: "aaaIntroduction tootopology i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren topologi funktioner"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e753a435-38e0-482b-846b-121cb547555c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7fa1c5518e4a25a5db999d898a9ee19fd0121db7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tootopology-in-azure-network-watcher"></a><span data-ttu-id="132db-103">Introduktion tootopology i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="132db-103">Introduction tootopology in Azure Network Watcher</span></span>

<span data-ttu-id="132db-104">Topologi returnerar ett diagram över nätverksresurser i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="132db-104">Topology returns a graph of network resources in a virtual network.</span></span> <span data-ttu-id="132db-105">hello diagrammet visar hello sambandet mellan hello resurser toorepresent hello slutet tooend nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="132db-105">hello graph depicts hello interconnection between hello resources toorepresent hello end tooend network connectivity.</span></span>

![Översikt över labbtopologi][1]

<span data-ttu-id="132db-107">I hello portal returnerar topologi hello resursobjekt på en per virtuellt nätverk basis.</span><span class="sxs-lookup"><span data-stu-id="132db-107">In hello portal, Topology returns hello resource objects on a per virtual network basis.</span></span> <span data-ttu-id="132db-108">hello relationer illustreras med linjer mellan hello resurser resurser utanför hello Nätverksbevakaren region, även om hello resurs grupp inte ska visas.</span><span class="sxs-lookup"><span data-stu-id="132db-108">hello relationships are depicted by lines between hello resources Resources outside of hello Network Watcher region, even if in hello resource group will not be displayed.</span></span> <span data-ttu-id="132db-109">hello resurser returneras i hello portal-vy är en delmängd av hello nätverkskomponenter som är visas i diagram registreringen.</span><span class="sxs-lookup"><span data-stu-id="132db-109">hello resources returned in hello portal view are a subset of hello networking components that are graphed.</span></span> <span data-ttu-id="132db-110">toosee hello fullständig lista över nätverksresurser som du kan använda [PowerShell](network-watcher-topology-powershell.md) eller [REST](network-watcher-topology-rest.md)</span><span class="sxs-lookup"><span data-stu-id="132db-110">toosee hello full list of networking resources you can use [PowerShell](network-watcher-topology-powershell.md) or [REST](network-watcher-topology-rest.md)</span></span>

> [!NOTE]
> <span data-ttu-id="132db-111">En instans av Nätverksbevakaren krävs i varje region som du vill ha toorun topologi.</span><span class="sxs-lookup"><span data-stu-id="132db-111">An instance of Network Watcher is required in each region that you want toorun Topology on.</span></span>

<span data-ttu-id="132db-112">Som resurser returneras hello anslutning mellan dem modelleras under två relationer.</span><span class="sxs-lookup"><span data-stu-id="132db-112">As resources are returned hello connection between them are modeled under two relationships.</span></span>

- <span data-ttu-id="132db-113">**Inneslutning** -exempel: virtuellt nätverk innehåller ett undernät som innehåller ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="132db-113">**Containment** - Example: Virtual Network contains a Subnet which contains a NIC</span></span>
- <span data-ttu-id="132db-114">**Associerade** -exempel: ett nätverkskort är kopplat till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="132db-114">**Associated** - Example: A NIC is associated with a VM</span></span>

### <a name="next-steps"></a><span data-ttu-id="132db-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="132db-115">Next steps</span></span>

<span data-ttu-id="132db-116">Lär dig hur toouse PowerShell tooretrieve hello topologi visa genom att besöka [Nätverksbevakaren topologi med PowerShell](network-watcher-topology-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="132db-116">Learn how toouse PowerShell tooretrieve hello Topology view by visiting [Network Watcher topology with PowerShell](network-watcher-topology-powershell.md)</span></span>

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
