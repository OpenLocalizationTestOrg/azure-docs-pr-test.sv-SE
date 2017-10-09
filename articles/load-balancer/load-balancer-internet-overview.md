---
title: "aaaInternet mot belastningen belastningsutjämnaren översikt | Microsoft Docs"
description: "Översikt för Internet facing belastningsutjämnare och dess funktioner. Hur fungerar en belastningsutjämnare för Azure med hjälp av virtuella datorer och molntjänster."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3514f945d69ec576ed256cdd01069491e3e43936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internet-facing-load-balancer-overview"></a><span data-ttu-id="b3db0-104">Internet Internetriktade belastningen översikt över belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="b3db0-104">Internet facing load balancer overview</span></span>

<span data-ttu-id="b3db0-105">Azure belastningsutjämnare mappar hello offentliga IP-adress och port antalet inkommande trafik toohello privata IP-adress och port antal hello virtuell dator och vice versa för hello svarstrafik från hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b3db0-105">Azure load balancer maps hello public IP address and port number of incoming traffic toohello private IP address and port number of hello virtual machine and vice versa for hello response traffic from hello virtual machine.</span></span> <span data-ttu-id="b3db0-106">Belastningsutjämningsregler tillåter toodistribute specifika typer av trafik mellan flera virtuella datorer eller tjänster.</span><span class="sxs-lookup"><span data-stu-id="b3db0-106">Load balancing rules allow you toodistribute specific types of traffic between multiple virtual machines or services.</span></span> <span data-ttu-id="b3db0-107">Sprida hello belastningen på begäran Internet-trafik över flera webbservrar eller webbroller.</span><span class="sxs-lookup"><span data-stu-id="b3db0-107">For example, you can spread hello load of web request traffic across multiple web servers or web roles.</span></span>

<span data-ttu-id="b3db0-108">Du kan definiera en offentlig slutpunkt för en molntjänst som innehåller instanser av webbroller eller arbetsroller i hello tjänstdefinitionsfilen (.csdef).</span><span class="sxs-lookup"><span data-stu-id="b3db0-108">For a cloud service that contains instances of web roles or worker roles, you can define a public endpoint in hello service definition (.csdef) file.</span></span>

<span data-ttu-id="b3db0-109">Hej *servicedefinition.csdef* filen innehåller hello slutpunktskonfiguration och när du har flera rollinstanser för distribution av en webb- eller arbetarroll rollen hello belastningsutjämnaren ska vara inställd för det..</span><span class="sxs-lookup"><span data-stu-id="b3db0-109">hello *servicedefinition.csdef* file contains hello endpoint configuration and when you have multiple role instances for a web or worker role deployment, hello load balancer will be setup for it.</span></span> <span data-ttu-id="b3db0-110">hello sätt tooadd instanser tooyour molndistribution ändrar hello instanser på hello tjänstkonfigurationsfilen (.csfg).</span><span class="sxs-lookup"><span data-stu-id="b3db0-110">hello way tooadd instances tooyour cloud deployment is changing hello instance count on hello service configuration file (.csfg).</span></span>

<span data-ttu-id="b3db0-111">hello visar följande bild en belastningsutjämnad slutpunkt för webbtrafik som delas mellan tre virtuella datorer för hello offentliga och privata TCP-port 80.</span><span class="sxs-lookup"><span data-stu-id="b3db0-111">hello following figure shows a load-balanced endpoint for web traffic that is shared among three virtual machines for hello public and private TCP port of 80.</span></span> <span data-ttu-id="b3db0-112">Dessa tre virtuella datorer finns i en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="b3db0-112">These three virtual machines are in a load-balanced set.</span></span>

![exempel på offentliga belastningsutjämnare](./media/load-balancer-internet-overview/IC727496.png)

<span data-ttu-id="b3db0-114">Bild 1 - belastningsutjämnade slutpunkt för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="b3db0-114">Figure 1 - Load-balanced endpoint for web traffic</span></span>

<span data-ttu-id="b3db0-115">När Internet-klienter skickar förfrågningar för webbsidan toohello offentliga IP-adressen för hello-Molntjänsten på TCP-port 80, distribuerar hello Azure belastningsutjämnare hello begäranden mellan hello tre virtuella datorer i hello belastningsutjämnade uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="b3db0-115">When Internet clients send web page requests toohello public IP address of hello cloud service on TCP port 80, hello Azure Load Balancer distributes hello requests between hello three virtual machines in hello load-balanced set.</span></span> <span data-ttu-id="b3db0-116">Mer information om belastningen belastningsutjämnaren algoritmer finns hello [belastningen belastningsutjämnaren översiktssidan](load-balancer-overview.md#load-balancer-features).</span><span class="sxs-lookup"><span data-stu-id="b3db0-116">For more information about load balancer algorithms, see hello [load balancer overview page](load-balancer-overview.md#load-balancer-features).</span></span>

<span data-ttu-id="b3db0-117">Som standard distribuerar Azure belastningsutjämnare nätverkstrafik jämnt mellan flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b3db0-117">By default, Azure Load Balancer distributes network traffic equally among multiple virtual machine instances.</span></span> <span data-ttu-id="b3db0-118">Du kan också konfigurera sessionen tillhörighet mer information finns i [belastningsdistributionsläget nätverksbelastningsutjämning](load-balancer-distribution-mode.md).</span><span class="sxs-lookup"><span data-stu-id="b3db0-118">You can also configure session affinity, For more information, see [load balancer distribution mode](load-balancer-distribution-mode.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3db0-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b3db0-119">Next steps</span></span>

<span data-ttu-id="b3db0-120">Lär dig mer om [intern belastningsutjämnare](load-balancer-internal-overview.md) toobetter förstå vilka belastningsutjämning är en passar bättre för din molndistribution.</span><span class="sxs-lookup"><span data-stu-id="b3db0-120">Learn about [Internal load balancer](load-balancer-internal-overview.md) toobetter understand which load balancer is a better fit for your cloud deployment.</span></span>

<span data-ttu-id="b3db0-121">Du kan också [komma igång med en Internetuppkopplad belastningsutjämnaren](load-balancer-get-started-internet-arm-ps.md) och konfigurera vilken typ av [distribution läge](load-balancer-distribution-mode.md) för en särskild belastningsutjämnare trafik nätverksproblem.</span><span class="sxs-lookup"><span data-stu-id="b3db0-121">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for an specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="b3db0-122">Om ditt program måste tookeep anslutningar alive för servrar bakom en belastningsutjämnare, du veta mer om [inaktiv TCP timeout-inställningar för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="b3db0-122">If your application needs tookeep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="b3db0-123">Det hjälper toolearn om inaktiv anslutning beteende när du använder Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="b3db0-123">It will help toolearn about idle connection behavior when you are using Azure Load Balancer.</span></span>
