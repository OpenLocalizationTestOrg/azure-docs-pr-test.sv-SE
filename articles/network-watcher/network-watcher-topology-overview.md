---
title: "Introduktion till topologi i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över funktionerna Nätverksbevakaren topologi"
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
ms.openlocfilehash: 42443f614b76b8180ac163b9889163021adbf048
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-topology-in-azure-network-watcher"></a><span data-ttu-id="89774-103">Introduktion till topologi i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="89774-103">Introduction to topology in Azure Network Watcher</span></span>

<span data-ttu-id="89774-104">Topologi returnerar ett diagram över nätverksresurser i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="89774-104">Topology returns a graph of network resources in a virtual network.</span></span> <span data-ttu-id="89774-105">Diagrammet visar sambandet mellan resurser för att representera nätverksanslutning för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="89774-105">The graph depicts the interconnection between the resources to represent the end to end network connectivity.</span></span>

![Översikt över labbtopologi][1]

<span data-ttu-id="89774-107">I portalen returnerar topologi resursobjekt på en per virtuellt nätverk basis.</span><span class="sxs-lookup"><span data-stu-id="89774-107">In the portal, Topology returns the resource objects on a per virtual network basis.</span></span> <span data-ttu-id="89774-108">Relationerna illustreras med linjer mellan resurser resurser utanför Nätverksbevakaren region, även om resursen gruppen inte visas.</span><span class="sxs-lookup"><span data-stu-id="89774-108">The relationships are depicted by lines between the resources Resources outside of the Network Watcher region, even if in the resource group will not be displayed.</span></span> <span data-ttu-id="89774-109">Resurser som returneras i vyn portal är en delmängd av nätverkskomponenter som är visas i diagram registreringen.</span><span class="sxs-lookup"><span data-stu-id="89774-109">The resources returned in the portal view are a subset of the networking components that are graphed.</span></span> <span data-ttu-id="89774-110">Se en fullständig lista över nätverksresurser som du kan använda [PowerShell](network-watcher-topology-powershell.md) eller [REST](network-watcher-topology-rest.md)</span><span class="sxs-lookup"><span data-stu-id="89774-110">To see the full list of networking resources you can use [PowerShell](network-watcher-topology-powershell.md) or [REST](network-watcher-topology-rest.md)</span></span>

> [!NOTE]
> <span data-ttu-id="89774-111">En instans av Nätverksbevakaren krävs för varje region som du vill köra topologi på.</span><span class="sxs-lookup"><span data-stu-id="89774-111">An instance of Network Watcher is required in each region that you want to run Topology on.</span></span>

<span data-ttu-id="89774-112">Som resurser returneras anslutningen mellan dem modelleras under två relationer.</span><span class="sxs-lookup"><span data-stu-id="89774-112">As resources are returned the connection between them are modeled under two relationships.</span></span>

- <span data-ttu-id="89774-113">**Inneslutning** -exempel: virtuellt nätverk innehåller ett undernät som innehåller ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="89774-113">**Containment** - Example: Virtual Network contains a Subnet which contains a NIC</span></span>
- <span data-ttu-id="89774-114">**Associerade** -exempel: ett nätverkskort är kopplat till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="89774-114">**Associated** - Example: A NIC is associated with a VM</span></span>

### <a name="next-steps"></a><span data-ttu-id="89774-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89774-115">Next steps</span></span>

<span data-ttu-id="89774-116">Lär dig hur du använder PowerShell för att hämta topologiska vyn genom att besöka [Nätverksbevakaren topologi med PowerShell](network-watcher-topology-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="89774-116">Learn how to use PowerShell to retrieve the Topology view by visiting [Network Watcher topology with PowerShell](network-watcher-topology-powershell.md)</span></span>

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
