---
title: "Introduktion till IP-flöde verifiera i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt av nätverket Watcher IP flödet verifieringsfunktioner"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 9c0dfc449b3d93d8aa4551ce16476c8313d731fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a><span data-ttu-id="378fe-103">Introduktion till IP-flöde verifiera i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="378fe-103">Introduction to IP flow verify in Azure Network Watcher</span></span>

<span data-ttu-id="378fe-104">IP-flöde kontrollera kontrollerar om ett paket tillåts eller nekas till eller från en virtuell dator baserat på 5-tuppel information.</span><span class="sxs-lookup"><span data-stu-id="378fe-104">IP flow verify checks if a packet is allowed or denied to or from a virtual machine based on 5-tuple information.</span></span> <span data-ttu-id="378fe-105">Den här informationen består av riktning, protokoll, lokal IP-, fjärr-IP, lokal port och Fjärrport.</span><span class="sxs-lookup"><span data-stu-id="378fe-105">This information consists of direction, protocol, local IP, remote IP, local port, and remote port.</span></span> <span data-ttu-id="378fe-106">Om paketet nekas av en säkerhetsgrupp, returneras namnet på regeln som nekats paketet.</span><span class="sxs-lookup"><span data-stu-id="378fe-106">If the packet is denied by a security group, the name of the rule that denied the packet is returned.</span></span> <span data-ttu-id="378fe-107">När alla käll- eller IP-adresser kan väljas hjälper administratörer att snabbt diagnostisera problem med nätverksanslutningen från eller till internet och från eller till den lokala miljön i den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="378fe-107">While any source or destination IP can be chosen, this feature helps administrators quickly diagnose connectivity issues from or to the internet and from or to the on-premises environment.</span></span>

<span data-ttu-id="378fe-108">IP-flöde Kontrollera riktar sig till ett nätverksgränssnitt för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="378fe-108">IP flow verify targets a network interface of a virtual machine.</span></span> <span data-ttu-id="378fe-109">Trafikflöde verifieras sedan baserat på de konfigurerade inställningarna till eller från nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="378fe-109">Traffic flow is then verified based on the configured settings to or from that network interface.</span></span> <span data-ttu-id="378fe-110">Den här funktionen är användbar för att bekräfta om en regel i en Nätverkssäkerhetsgrupp blockerar inkommande trafik eller utgående trafik till eller från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="378fe-110">This capability is useful in confirming if a rule in a Network Security Group is blocking ingress or egress traffic to or from a virtual machine.</span></span>

<span data-ttu-id="378fe-111">Kontrollera en instans av Nätverksbevakaren måste skapas i alla regioner som du tänker köra IP-flödet.</span><span class="sxs-lookup"><span data-stu-id="378fe-111">An instance of Network Watcher needs to be created in all regions that you plan to run IP flow verify.</span></span> <span data-ttu-id="378fe-112">Nätverksbevakaren är en tjänst som regional och kan endast kördes mot resurser i samma region.</span><span class="sxs-lookup"><span data-stu-id="378fe-112">Network Watcher is a regional service and can only be ran against resources in the same region.</span></span> <span data-ttu-id="378fe-113">Den här har inte påverka resultatet av IP-flöde Kontrollera som returneras fortfarande vägen som är kopplade till nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="378fe-113">This does not affect the results of IP flow verify as the route associated with the NIC will still be returned.</span></span>

![1][1]

## <a name="next-steps"></a><span data-ttu-id="378fe-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="378fe-115">Next steps</span></span>

<span data-ttu-id="378fe-116">Besök följande artikel om du vill veta om ett paket tillåts eller nekas för en specifik virtuell dator via portalen.</span><span class="sxs-lookup"><span data-stu-id="378fe-116">Visit the following article to learn if a packet is allowed or denied for a specific virtual machine through the portal.</span></span> [<span data-ttu-id="378fe-117">Kontrollera om trafik tillåts på en virtuell dator med IP-flöda Kontrollera med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="378fe-117">Check if traffic is allowed on a VM with IP Flow Verify using the portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












