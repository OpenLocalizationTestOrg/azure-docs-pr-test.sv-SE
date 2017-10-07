---
title: "aaaAzure Resource Manager-stöd för belastningsutjämnaren | Microsoft Docs"
description: "Med hjälp av powershell för belastningsutjämnaren med Azure Resource Manager. Med hjälp av mallar för belastningsutjämnare"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: d0394f11-ee5a-4407-9d86-79c936297265
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3c02d9382de00fefe6af8f4f8a2b7b62b344f285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a><span data-ttu-id="6ca9b-104">Med Azure belastningsutjämnare Azure Resource Manager-stöd</span><span class="sxs-lookup"><span data-stu-id="6ca9b-104">Using Azure Resource Manager Support with Azure Load Balancer</span></span>

<span data-ttu-id="6ca9b-105">Azure Resource Manager är hello önskade ramverk för tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-105">Azure Resource Manager is hello preferred management framework for services in Azure.</span></span> <span data-ttu-id="6ca9b-106">Azure belastningsutjämnare kan hanteras med Azure Resource Manager-baserade API: er och verktyg.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-106">Azure Load Balancer can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="concepts"></a><span data-ttu-id="6ca9b-107">Koncept</span><span class="sxs-lookup"><span data-stu-id="6ca9b-107">Concepts</span></span>

<span data-ttu-id="6ca9b-108">Med Resource Manager innehåller Azure belastningsutjämnare hello följande underordnade resurser:</span><span class="sxs-lookup"><span data-stu-id="6ca9b-108">With Resource Manager, Azure Load Balancer contains hello following child resources:</span></span>

* <span data-ttu-id="6ca9b-109">Frontend IP-konfiguration – en belastningsutjämnare kan innehålla en eller flera klientdelen IP-adresser, kallas även för en virtuell IP-adresser (VIP).</span><span class="sxs-lookup"><span data-stu-id="6ca9b-109">Front-end IP configuration – a Load balancer can include one or more front end IP addresses, otherwise known as a virtual IPs (VIPs).</span></span> <span data-ttu-id="6ca9b-110">IP-adresserna fungera som ingång för hello-trafik.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-110">These IP addresses serve as ingress for hello traffic.</span></span>
* <span data-ttu-id="6ca9b-111">Backend-adresspool – det här är IP-adresser som är associerade med hello virtuella nätverk Interface Card (NIC) toowhich belastningen ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-111">Back-end address pool – these are IP addresses associated with hello virtual machine Network Interface Card (NIC) toowhich load will be distributed.</span></span>
* <span data-ttu-id="6ca9b-112">Belastningsutjämning regler – en regel för egenskapen mappar en given klientdelen IP-adress och port kombination tooa uppsättning serverdel IP-adresser och port-kombination.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-112">Load balancing rules – a rule property maps a given front end IP and port combination tooa set of back end IP addresses and port combination.</span></span> <span data-ttu-id="6ca9b-113">En enskild belastningsutjämnare kan ha flera regler för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-113">A single load balancer can have multiple load balancing rules.</span></span> <span data-ttu-id="6ca9b-114">Varje regel är en kombination av en frontend-IP och port och backend-IP och port som associeras med virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-114">Each rule is a combination of a front-end IP and port and back-end IP and port associated with VMs.</span></span>
* <span data-ttu-id="6ca9b-115">Avsökningar – avsökningar aktiverar du tookeep spåra hello hälsotillståndet hos VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-115">Probes – probes enable you tookeep track of hello health of VM instances.</span></span> <span data-ttu-id="6ca9b-116">Om en hälsoavsökningen misslyckas ska hello VM-instans tas bort från rotation automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-116">If a health probe fails, hello VM instance will be taken out of rotation automatically.</span></span>
* <span data-ttu-id="6ca9b-117">Inkommande NAT-regler – NAT-regler som definierar hello inkommande trafik som passerar genom hello klientdelen IP och distribuerade toohello tillbaka sluta IP.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-117">Inbound NAT rules – NAT rules defining hello inbound traffic flowing through hello front end IP and distributed toohello back end IP.</span></span>

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a><span data-ttu-id="6ca9b-118">Snabbstartsmallar</span><span class="sxs-lookup"><span data-stu-id="6ca9b-118">Quickstart templates</span></span>

<span data-ttu-id="6ca9b-119">Azure Resource Manager kan du tooprovision dina program med en deklarativ mall.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-119">Azure Resource Manager allows you tooprovision your applications using a declarative template.</span></span> <span data-ttu-id="6ca9b-120">I samma mall kan du distribuera flera tjänster tillsammans med deras beroenden.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-120">In a single template, you can deploy multiple services along with their dependencies.</span></span> <span data-ttu-id="6ca9b-121">Du använder hello samma mall toorepeatedly distribuera programmet under varje steg i livscykeln för hello-programmet.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-121">You use hello same template toorepeatedly deploy your application during every stage of hello application lifecycle.</span></span>

<span data-ttu-id="6ca9b-122">Mallar kan innehålla definitioner för virtuella datorer, virtuella nätverk, Tillgänglighetsuppsättningar, nätverksgränssnitt (NIC), Storage-konton, belastningsutjämnare, Nätverkssäkerhetsgrupper och offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-122">Templates can include definitions for Virtual Machines, Virtual Networks, Availability Sets, Network Interfaces (NICs), Storage Accounts, Load Balancers, Network Security Groups, and Public IPs.</span></span> <span data-ttu-id="6ca9b-123">Du kan skapa allt du behöver för komplexa program med mallar.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-123">With templates you can create everything you need for a complex application.</span></span> <span data-ttu-id="6ca9b-124">hello mallfilen kan kontrolleras till innehållshantering system för versionskontroll och samarbete.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-124">hello template file can be checked into content management system for version control and collaboration.</span></span>

[<span data-ttu-id="6ca9b-125">Mer information om mallar</span><span class="sxs-lookup"><span data-stu-id="6ca9b-125">Learn more about templates</span></span>](../azure-resource-manager/resource-manager-template-walkthrough.md)

[<span data-ttu-id="6ca9b-126">Mer information om nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="6ca9b-126">Learn more about Network Resources</span></span>](../virtual-network/resource-groups-networking.md)

<span data-ttu-id="6ca9b-127">Snabbstartsmallar med hjälp av Azure belastningsutjämnare finns i en [GitHub-lagringsplatsen](https://github.com/Azure/azure-quickstart-templates) värd för en uppsättning community genereras mallar.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-127">Quickstart templates using Azure Load Balancer can be found in a [GitHub repository](https://github.com/Azure/azure-quickstart-templates) hosting a set of community generated templates.</span></span>

<span data-ttu-id="6ca9b-128">Exempel på mallar:</span><span class="sxs-lookup"><span data-stu-id="6ca9b-128">Examples of templates:</span></span>

* [<span data-ttu-id="6ca9b-129">2 virtuella datorer i en belastningsutjämnare och belastningsutjämningsregler</span><span class="sxs-lookup"><span data-stu-id="6ca9b-129">2 VMs in a Load Balancer and load balancing rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544799)
* [<span data-ttu-id="6ca9b-130">2 virtuella datorer i ett VNET med en intern belastningsutjämnare och belastningsutjämnare regler</span><span class="sxs-lookup"><span data-stu-id="6ca9b-130">2 VMs in a VNET with an Internal Load Balancer and Load Balancer rules</span></span>](http://go.microsoft.com/fwlink/?LinkId=544800)
* [<span data-ttu-id="6ca9b-131">2 virtuella datorer i en belastningsutjämnare och konfigurera NAT-regler på hello LB</span><span class="sxs-lookup"><span data-stu-id="6ca9b-131">2 VMs in a Load Balancer and configure NAT rules on hello LB</span></span>](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a><span data-ttu-id="6ca9b-132">Konfigurera Azure belastningsutjämnare med PowerShell eller CLI</span><span class="sxs-lookup"><span data-stu-id="6ca9b-132">Setting up Azure Load Balancer with a PowerShell or CLI</span></span>

<span data-ttu-id="6ca9b-133">Kom igång med Azure Resource Manager cmdlets, kommandoradsverktyg och REST API: er</span><span class="sxs-lookup"><span data-stu-id="6ca9b-133">Get started with Azure Resource Manager cmdlets, command line tools, and REST APIs</span></span>

* <span data-ttu-id="6ca9b-134">[Azure-nätverk Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) kan vara används toocreate belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-134">[Azure Networking Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) can be used toocreate a Load Balancer.</span></span>
* [<span data-ttu-id="6ca9b-135">Hur toocreate en belastningsutjämnare med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6ca9b-135">How toocreate a load balancer using Azure Resource Manager</span></span>](load-balancer-get-started-ilb-arm-ps.md)
* [<span data-ttu-id="6ca9b-136">Med hjälp av hello Azure CLI med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6ca9b-136">Using hello Azure CLI with Azure Resource Management</span></span>](../xplat-cli-azure-resource-manager.md)
* [<span data-ttu-id="6ca9b-137">Belastningsutjämnaren REST API: er</span><span class="sxs-lookup"><span data-stu-id="6ca9b-137">Load Balancer REST APIs</span></span>](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a><span data-ttu-id="6ca9b-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ca9b-138">Next steps</span></span>

<span data-ttu-id="6ca9b-139">Du kan också [komma igång med en Internetuppkopplad belastningsutjämnaren](load-balancer-get-started-internet-arm-ps.md) och konfigurera vilken typ av [distribution läge](load-balancer-distribution-mode.md) för en särskild belastningsutjämnare trafik nätverksproblem.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-139">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for a specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="6ca9b-140">Lär dig hur toomanage [inaktiv TCP timeout-inställningar för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="6ca9b-140">Learn how toomanage [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="6ca9b-141">Detta är viktigt när ditt program behöver tookeep anslutningar alive för servrar bakom en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="6ca9b-141">This is important when your application needs tookeep connections alive for servers behind a load balancer.</span></span>
