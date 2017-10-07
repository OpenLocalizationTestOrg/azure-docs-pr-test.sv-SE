---
title: aaaOverview i Azure DNS | Microsoft Docs
description: "Översikt över DNS-värdtjänsten på Microsoft Azure. Värd för din domän i Microsoft Azure."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: gwallace
ms.openlocfilehash: a10f87c488356469e9c04aabde31129049563891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-overview"></a><span data-ttu-id="3e591-104">Översikt över Azure DNS</span><span class="sxs-lookup"><span data-stu-id="3e591-104">Azure DNS overview</span></span>

<span data-ttu-id="3e591-105">hello Domain Name System eller DNS är översätter (eller lösa) en webbplats eller tjänst name tooits IP-adress.</span><span class="sxs-lookup"><span data-stu-id="3e591-105">hello Domain Name System, or DNS, is responsible for translating (or resolving) a website or service name tooits IP address.</span></span> <span data-ttu-id="3e591-106">Azure DNS är värdtjänsten för DNS-domäner som tillhandahåller namnmatchning med hjälp av Microsoft Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="3e591-106">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span> <span data-ttu-id="3e591-107">Värd för dina domäner i Azure, kan du hantera din DNS hello poster med samma autentiseringsuppgifter, API: er, verktyg och fakturering som andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="3e591-107">By hosting your domains in Azure, you can manage your DNS records using hello same credentials, APIs, tools, and billing as your other Azure services.</span></span>

![Översikt över DNS](./media/dns-overview/scenario.png)

## <a name="features"></a><span data-ttu-id="3e591-109">Funktioner</span><span class="sxs-lookup"><span data-stu-id="3e591-109">Features</span></span>

* <span data-ttu-id="3e591-110">**Tillförlitlighet och prestanda** -DNS-domäner i Azure DNS finns på Azures globalt nätverk av DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="3e591-110">**Reliability and performance** - DNS domains in Azure DNS are hosted on Azure's global network of DNS name servers.</span></span> <span data-ttu-id="3e591-111">Vi använder Anycast nätverk så att varje DNS-frågan har besvarats av hello närmaste tillgängliga DNS-server.</span><span class="sxs-lookup"><span data-stu-id="3e591-111">We use Anycast networking so that each DNS query is answered by hello closest available DNS server.</span></span> <span data-ttu-id="3e591-112">Detta ger både snabb prestanda och hög tillgänglighet för din domän.</span><span class="sxs-lookup"><span data-stu-id="3e591-112">This provides both fast performance and high availability for your domain.</span></span>

* <span data-ttu-id="3e591-113">**Sömlös integration** -hello Azure DNS-tjänsten kan använda toomanage DNS-posterna för din Azure-tjänster och kan vara används tooprovide DNS för din externa resurser.</span><span class="sxs-lookup"><span data-stu-id="3e591-113">**Seamless integration** - hello Azure DNS service can be used toomanage DNS records for your Azure services and can be used tooprovide DNS for your external resources as well.</span></span> <span data-ttu-id="3e591-114">Azure DNS är integrerad i hello Azure-portalen och använder hello samma autentiseringsuppgifter, fakturering och support-kontrakt som andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="3e591-114">Azure DNS is integrated in hello Azure portal and uses hello same credentials, billing and support contract as your other Azure services.</span></span>

* <span data-ttu-id="3e591-115">**Säkerhet** -hello Azure DNS-tjänsten är baserad på Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="3e591-115">**Security** - hello Azure DNS service is based on Azure Resource Manager.</span></span> <span data-ttu-id="3e591-116">Därför fördelar från Resource Manager-funktioner, till exempel rollbaserad åtkomstkontroll, granskningsloggar och låsa resurs.</span><span class="sxs-lookup"><span data-stu-id="3e591-116">As such, it benefits from Resource Manager features such as role-based access control, audit logs, and resource locking.</span></span> <span data-ttu-id="3e591-117">Dina domäner och poster kan hanteras via hello Azure-portalen, Azure PowerShell-cmdlets och hello plattformsoberoende Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="3e591-117">Your domains and records can be managed via hello Azure portal, Azure PowerShell cmdlets, and hello cross-platform Azure CLI.</span></span> <span data-ttu-id="3e591-118">Program som kräver automatisk DNS-hantering kan integreras med hello-tjänsten via hello REST-API och SDK: er.</span><span class="sxs-lookup"><span data-stu-id="3e591-118">Applications requiring automatic DNS management can integrate with hello service via hello REST API and SDKs.</span></span>

<span data-ttu-id="3e591-119">Azure DNS stöder för närvarande inte köp av domännamn.</span><span class="sxs-lookup"><span data-stu-id="3e591-119">Azure DNS does not currently support purchasing of domain names.</span></span> <span data-ttu-id="3e591-120">Om du vill toopurchase domäner, måste toouse från tredje part domännamnsregistratorn.</span><span class="sxs-lookup"><span data-stu-id="3e591-120">If you want toopurchase domains, you need toouse a third-party domain name registrar.</span></span> <span data-ttu-id="3e591-121">hello registrator debiterar vanligtvis en liten årlig avgift.</span><span class="sxs-lookup"><span data-stu-id="3e591-121">hello registrar typically charges a small annual fee.</span></span> <span data-ttu-id="3e591-122">hello domäner kan finnas i Azure DNS för för hantering av DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="3e591-122">hello domains can then be hosted in Azure DNS for management of DNS records.</span></span> <span data-ttu-id="3e591-123">Se [delegera en domän tooAzure DNS](dns-domain-delegation.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="3e591-123">See [Delegate a Domain tooAzure DNS](dns-domain-delegation.md) for details.</span></span>

## <a name="pricing"></a><span data-ttu-id="3e591-124">Prissättning</span><span class="sxs-lookup"><span data-stu-id="3e591-124">Pricing</span></span>

<span data-ttu-id="3e591-125">DNS-fakturering baseras på hello antal DNS-zoner i Azure och av hello antal DNS-frågor.</span><span class="sxs-lookup"><span data-stu-id="3e591-125">DNS billing is based on hello number of DNS zones hosted in Azure and by hello number of DNS queries.</span></span> <span data-ttu-id="3e591-126">Mer om prissättningen besök toolearn [priser för Azure DNS](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="3e591-126">toolearn more about pricing visit [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>

## <a name="faq"></a><span data-ttu-id="3e591-127">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="3e591-127">FAQ</span></span>

<span data-ttu-id="3e591-128">Vanliga frågor om Azure DNS finns hello [Azure DNS FAQ](dns-faq.md).</span><span class="sxs-lookup"><span data-stu-id="3e591-128">For frequently asked questions about Azure DNS, see hello [Azure DNS FAQ](dns-faq.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e591-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e591-129">Next steps</span></span>

<span data-ttu-id="3e591-130">Lär dig mer om DNS-zoner och poster genom att besöka: [DNS-zoner och innehåller en översikt över](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="3e591-130">Learn about DNS zones and records by visiting: [DNS zones and records overview](dns-zones-records.md).</span></span>

<span data-ttu-id="3e591-131">Lär dig hur för[skapa en DNS-zon](./dns-getstarted-create-dnszone-portal.md) i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="3e591-131">Learn how too[create a DNS zone](./dns-getstarted-create-dnszone-portal.md) in Azure DNS.</span></span>

<span data-ttu-id="3e591-132">Lär dig mer om hello andra nyckeln [nätverk](../networking/networking-overview.md) Azure.</span><span class="sxs-lookup"><span data-stu-id="3e591-132">Learn about some of hello other key [networking capabilities](../networking/networking-overview.md) of Azure.</span></span>

