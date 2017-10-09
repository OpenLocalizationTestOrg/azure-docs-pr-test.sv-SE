---
title: "aaaIntroduction tooIP flöde verifiera i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt av hello Network Watcher IP flödet verifieringsfunktioner"
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
ms.openlocfilehash: b648a4816a7ffdc6ca54462944b574e2395e8298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooip-flow-verify-in-azure-network-watcher"></a><span data-ttu-id="2002b-103">Introduktion tooIP flöde verifiera i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="2002b-103">Introduction tooIP flow verify in Azure Network Watcher</span></span>

<span data-ttu-id="2002b-104">IP-flöde kontrollera kontrollerar om ett paket tillåts eller nekas tooor från en virtuell dator baserat på 5-tuppel information.</span><span class="sxs-lookup"><span data-stu-id="2002b-104">IP flow verify checks if a packet is allowed or denied tooor from a virtual machine based on 5-tuple information.</span></span> <span data-ttu-id="2002b-105">Den här informationen består av riktning, protokoll, lokal IP-, fjärr-IP, lokal port och Fjärrport.</span><span class="sxs-lookup"><span data-stu-id="2002b-105">This information consists of direction, protocol, local IP, remote IP, local port, and remote port.</span></span> <span data-ttu-id="2002b-106">Om hello paket nekas av en säkerhetsgrupp, returneras hello namn på hello-regel som nekats hello-paket.</span><span class="sxs-lookup"><span data-stu-id="2002b-106">If hello packet is denied by a security group, hello name of hello rule that denied hello packet is returned.</span></span> <span data-ttu-id="2002b-107">När alla käll- eller IP-adresser kan väljas funktionen hjälper administratörer att snabbt diagnostisera problem med nätverksanslutningen från eller toohello internet och från eller toohello lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="2002b-107">While any source or destination IP can be chosen, this feature helps administrators quickly diagnose connectivity issues from or toohello internet and from or toohello on-premises environment.</span></span>

<span data-ttu-id="2002b-108">IP-flöde Kontrollera riktar sig till ett nätverksgränssnitt för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2002b-108">IP flow verify targets a network interface of a virtual machine.</span></span> <span data-ttu-id="2002b-109">Trafikflöde verifieras sedan baserat på konfigurerad hello inställningar tooor från nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2002b-109">Traffic flow is then verified based on hello configured settings tooor from that network interface.</span></span> <span data-ttu-id="2002b-110">Den här funktionen är användbar för att bekräfta om en regel i en Nätverkssäkerhetsgrupp blockerar tooor för meddelanden om ingångs- eller utgående trafik från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2002b-110">This capability is useful in confirming if a rule in a Network Security Group is blocking ingress or egress traffic tooor from a virtual machine.</span></span>

<span data-ttu-id="2002b-111">Kontrollera en instans av Nätverksbevakaren behov toobe som skapats i alla regioner som du planerar toorun IP-flödet.</span><span class="sxs-lookup"><span data-stu-id="2002b-111">An instance of Network Watcher needs toobe created in all regions that you plan toorun IP flow verify.</span></span> <span data-ttu-id="2002b-112">Nätverksbevakaren är en tjänst som regional och kan endast kördes mot resurser i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="2002b-112">Network Watcher is a regional service and can only be ran against resources in hello same region.</span></span> <span data-ttu-id="2002b-113">Detta påverkar inte hello resultaten av IP-flöde Kontrollera som hello väg som är associerade med hello NIC kommer fortfarande att returneras.</span><span class="sxs-lookup"><span data-stu-id="2002b-113">This does not affect hello results of IP flow verify as hello route associated with hello NIC will still be returned.</span></span>

![1][1]

## <a name="next-steps"></a><span data-ttu-id="2002b-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2002b-115">Next steps</span></span>

<span data-ttu-id="2002b-116">Besök hello efter artikel toolearn om ett paket tillåts eller nekas för en specifik virtuell dator via hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2002b-116">Visit hello following article toolearn if a packet is allowed or denied for a specific virtual machine through hello portal.</span></span> [<span data-ttu-id="2002b-117">Kontrollera om trafik tillåts på en virtuell dator med IP-flöda Kontrollera med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="2002b-117">Check if traffic is allowed on a VM with IP Flow Verify using hello portal</span></span>](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












