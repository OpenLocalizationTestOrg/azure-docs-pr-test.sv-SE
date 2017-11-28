---
title: "Översikt över belastningen belastningsutjämnare mot Internet | Microsoft Docs"
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
ms.openlocfilehash: c420b38fbe8054bc4b701f89ebc417677ca47a27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="internet-facing-load-balancer-overview"></a><span data-ttu-id="7e89e-104">Internet Internetriktade belastningen översikt över belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7e89e-104">Internet facing load balancer overview</span></span>

<span data-ttu-id="7e89e-105">Azure belastningsutjämnare mappar offentliga IP-adress och port antalet inkommande trafik till privata IP-adress och port numret för den virtuella datorn och vice versa för svarstrafik från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7e89e-105">Azure load balancer maps the public IP address and port number of incoming traffic to the private IP address and port number of the virtual machine and vice versa for the response traffic from the virtual machine.</span></span> <span data-ttu-id="7e89e-106">Belastningsutjämningsregler kan du distribuera specifika typer av trafik mellan flera virtuella datorer eller tjänster.</span><span class="sxs-lookup"><span data-stu-id="7e89e-106">Load balancing rules allow you to distribute specific types of traffic between multiple virtual machines or services.</span></span> <span data-ttu-id="7e89e-107">Sprida belastningen på begäran Internet-trafik över flera webbservrar eller webbroller.</span><span class="sxs-lookup"><span data-stu-id="7e89e-107">For example, you can spread the load of web request traffic across multiple web servers or web roles.</span></span>

<span data-ttu-id="7e89e-108">Du kan definiera en offentlig slutpunkt för en molntjänst som innehåller instanser av webbroller eller arbetsroller i tjänstdefinitionsfilen (.csdef).</span><span class="sxs-lookup"><span data-stu-id="7e89e-108">For a cloud service that contains instances of web roles or worker roles, you can define a public endpoint in the service definition (.csdef) file.</span></span>

<span data-ttu-id="7e89e-109">Den *servicedefinition.csdef* filen innehåller slutpunktskonfigurationen och när du har flera rollinstanser för distribution av en webb- eller arbetarroll rollen belastningsutjämnaren konfigureras för den.</span><span class="sxs-lookup"><span data-stu-id="7e89e-109">The *servicedefinition.csdef* file contains the endpoint configuration and when you have multiple role instances for a web or worker role deployment, the load balancer will be setup for it.</span></span> <span data-ttu-id="7e89e-110">Sätt att lägga till instanser för din molndistribution ändrar instansantalet på tjänstkonfigurationsfilen (.csfg).</span><span class="sxs-lookup"><span data-stu-id="7e89e-110">The way to add instances to your cloud deployment is changing the instance count on the service configuration file (.csfg).</span></span>

<span data-ttu-id="7e89e-111">Följande bild visar en belastningsutjämnad slutpunkt för webbtrafik som delas mellan tre virtuella datorer för den offentliga och privata TCP-porten 80.</span><span class="sxs-lookup"><span data-stu-id="7e89e-111">The following figure shows a load-balanced endpoint for web traffic that is shared among three virtual machines for the public and private TCP port of 80.</span></span> <span data-ttu-id="7e89e-112">Dessa tre virtuella datorer finns i en belastningsutjämnad uppsättning.</span><span class="sxs-lookup"><span data-stu-id="7e89e-112">These three virtual machines are in a load-balanced set.</span></span>

![exempel på offentliga belastningsutjämnare](./media/load-balancer-internet-overview/IC727496.png)

<span data-ttu-id="7e89e-114">Bild 1 - belastningsutjämnade slutpunkt för webbtrafik</span><span class="sxs-lookup"><span data-stu-id="7e89e-114">Figure 1 - Load-balanced endpoint for web traffic</span></span>

<span data-ttu-id="7e89e-115">När Internet-klienter skickar webbsida begäranden till den offentliga IP-adressen för Molntjänsten på TCP-port 80, distribuerar Azure belastningsutjämnare begäranden mellan tre virtuella datorer i den belastningsutjämnade uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="7e89e-115">When Internet clients send web page requests to the public IP address of the cloud service on TCP port 80, the Azure Load Balancer distributes the requests between the three virtual machines in the load-balanced set.</span></span> <span data-ttu-id="7e89e-116">Mer information om belastningen belastningsutjämnaren algoritmer finns i [belastningen belastningsutjämnaren översiktssidan](load-balancer-overview.md#load-balancer-features).</span><span class="sxs-lookup"><span data-stu-id="7e89e-116">For more information about load balancer algorithms, see the [load balancer overview page](load-balancer-overview.md#load-balancer-features).</span></span>

<span data-ttu-id="7e89e-117">Som standard distribuerar Azure belastningsutjämnare nätverkstrafik jämnt mellan flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7e89e-117">By default, Azure Load Balancer distributes network traffic equally among multiple virtual machine instances.</span></span> <span data-ttu-id="7e89e-118">Du kan också konfigurera sessionen tillhörighet mer information finns i [belastningsdistributionsläget nätverksbelastningsutjämning](load-balancer-distribution-mode.md).</span><span class="sxs-lookup"><span data-stu-id="7e89e-118">You can also configure session affinity, For more information, see [load balancer distribution mode](load-balancer-distribution-mode.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e89e-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e89e-119">Next steps</span></span>

<span data-ttu-id="7e89e-120">Lär dig mer om [intern belastningsutjämnare](load-balancer-internal-overview.md) att bättre förstå vilka belastningsutjämning är en passar bättre för din molndistribution.</span><span class="sxs-lookup"><span data-stu-id="7e89e-120">Learn about [Internal load balancer](load-balancer-internal-overview.md) to better understand which load balancer is a better fit for your cloud deployment.</span></span>

<span data-ttu-id="7e89e-121">Du kan också [komma igång med en Internetuppkopplad belastningsutjämnaren](load-balancer-get-started-internet-arm-ps.md) och konfigurera vilken typ av [distribution läge](load-balancer-distribution-mode.md) för en särskild belastningsutjämnare trafik nätverksproblem.</span><span class="sxs-lookup"><span data-stu-id="7e89e-121">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for an specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="7e89e-122">Om ditt program kräver aktiva anslutningar för servrar bakom en belastningsutjämnare rekommenderar vi att du läser mer om [timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="7e89e-122">If your application needs to keep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="7e89e-123">Den här artikeln innehåller information om beteendet vid inaktiva anslutningar när du använder Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="7e89e-123">It will help to learn about idle connection behavior when you are using Azure Load Balancer.</span></span>
