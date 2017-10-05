---
title: "Översikt över Azure DNS | Microsoft Docs"
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
ms.openlocfilehash: 3705457e4c90f8869496f7f5177531bd128d1057
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-dns-overview"></a><span data-ttu-id="32f4c-104">Översikt över Azure DNS</span><span class="sxs-lookup"><span data-stu-id="32f4c-104">Azure DNS overview</span></span>

<span data-ttu-id="32f4c-105">Domain Name System- eller DNS, ansvarar för att översätta (eller lösa) en webbplats eller tjänst namn till dess IP-adress.</span><span class="sxs-lookup"><span data-stu-id="32f4c-105">The Domain Name System, or DNS, is responsible for translating (or resolving) a website or service name to its IP address.</span></span> <span data-ttu-id="32f4c-106">Azure DNS är värdtjänsten för DNS-domäner som tillhandahåller namnmatchning med hjälp av Microsoft Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="32f4c-106">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span> <span data-ttu-id="32f4c-107">Genom att använda Azure som värd för dina domäner kan du hantera dina DNS-poster med samma autentiseringsuppgifter, API:er, verktyg och fakturering som för dina andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="32f4c-107">By hosting your domains in Azure, you can manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services.</span></span>

![Översikt över DNS](./media/dns-overview/scenario.png)

## <a name="features"></a><span data-ttu-id="32f4c-109">Funktioner</span><span class="sxs-lookup"><span data-stu-id="32f4c-109">Features</span></span>

* <span data-ttu-id="32f4c-110">**Tillförlitlighet och prestanda** -DNS-domäner i Azure DNS finns på Azures globalt nätverk av DNS-namnservrar.</span><span class="sxs-lookup"><span data-stu-id="32f4c-110">**Reliability and performance** - DNS domains in Azure DNS are hosted on Azure's global network of DNS name servers.</span></span> <span data-ttu-id="32f4c-111">Vi använder Anycast nätverk så att varje DNS-frågan besvaras närmaste tillgängliga DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="32f4c-111">We use Anycast networking so that each DNS query is answered by the closest available DNS server.</span></span> <span data-ttu-id="32f4c-112">Detta ger både snabb prestanda och hög tillgänglighet för din domän.</span><span class="sxs-lookup"><span data-stu-id="32f4c-112">This provides both fast performance and high availability for your domain.</span></span>

* <span data-ttu-id="32f4c-113">**Sömlös integration** -Azure DNS-tjänsten kan användas för att hantera DNS-posterna för din Azure-tjänster och kan användas för att tillhandahålla DNS för din externa resurser.</span><span class="sxs-lookup"><span data-stu-id="32f4c-113">**Seamless integration** - The Azure DNS service can be used to manage DNS records for your Azure services and can be used to provide DNS for your external resources as well.</span></span> <span data-ttu-id="32f4c-114">Azure DNS är integrerat i Azure-portalen och använder samma autentiseringsuppgifter, fakturering och support-kontrakt som andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="32f4c-114">Azure DNS is integrated in the Azure portal and uses the same credentials, billing and support contract as your other Azure services.</span></span>

* <span data-ttu-id="32f4c-115">**Säkerhet** -Azure DNS-tjänsten är baserad på Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="32f4c-115">**Security** - The Azure DNS service is based on Azure Resource Manager.</span></span> <span data-ttu-id="32f4c-116">Därför fördelar från Resource Manager-funktioner, till exempel rollbaserad åtkomstkontroll, granskningsloggar och låsa resurs.</span><span class="sxs-lookup"><span data-stu-id="32f4c-116">As such, it benefits from Resource Manager features such as role-based access control, audit logs, and resource locking.</span></span> <span data-ttu-id="32f4c-117">Dina domäner och poster kan hanteras via Azure-portalen, Azure PowerShell-cmdlets och plattformsoberoende Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="32f4c-117">Your domains and records can be managed via the Azure portal, Azure PowerShell cmdlets, and the cross-platform Azure CLI.</span></span> <span data-ttu-id="32f4c-118">Program som kräver automatisk DNS-hantering kan integreras med via REST-API och SDK-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="32f4c-118">Applications requiring automatic DNS management can integrate with the service via the REST API and SDKs.</span></span>

<span data-ttu-id="32f4c-119">Azure DNS stöder för närvarande inte köp av domännamn.</span><span class="sxs-lookup"><span data-stu-id="32f4c-119">Azure DNS does not currently support purchasing of domain names.</span></span> <span data-ttu-id="32f4c-120">Om du vill köpa domäner måste du använda en tredje parts domännamnsregistratorn.</span><span class="sxs-lookup"><span data-stu-id="32f4c-120">If you want to purchase domains, you need to use a third-party domain name registrar.</span></span> <span data-ttu-id="32f4c-121">Registratorn debiterar vanligtvis en liten årlig avgift.</span><span class="sxs-lookup"><span data-stu-id="32f4c-121">The registrar typically charges a small annual fee.</span></span> <span data-ttu-id="32f4c-122">Domänerna kan finnas i Azure DNS för för hantering av DNS-poster.</span><span class="sxs-lookup"><span data-stu-id="32f4c-122">The domains can then be hosted in Azure DNS for management of DNS records.</span></span> <span data-ttu-id="32f4c-123">Se [delegera en domän till Azure DNS](dns-domain-delegation.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="32f4c-123">See [Delegate a Domain to Azure DNS](dns-domain-delegation.md) for details.</span></span>

## <a name="pricing"></a><span data-ttu-id="32f4c-124">Prissättning</span><span class="sxs-lookup"><span data-stu-id="32f4c-124">Pricing</span></span>

<span data-ttu-id="32f4c-125">DNS-fakturering baseras på antalet DNS-zoner i Azure och av antal DNS-frågor.</span><span class="sxs-lookup"><span data-stu-id="32f4c-125">DNS billing is based on the number of DNS zones hosted in Azure and by the number of DNS queries.</span></span> <span data-ttu-id="32f4c-126">Mer information om priser finns [priser för Azure DNS](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="32f4c-126">To learn more about pricing visit [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>

## <a name="faq"></a><span data-ttu-id="32f4c-127">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="32f4c-127">FAQ</span></span>

<span data-ttu-id="32f4c-128">Vanliga frågor och svar om Azure DNS, finns det [Azure DNS vanliga frågor om](dns-faq.md).</span><span class="sxs-lookup"><span data-stu-id="32f4c-128">For frequently asked questions about Azure DNS, see the [Azure DNS FAQ](dns-faq.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="32f4c-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32f4c-129">Next steps</span></span>

<span data-ttu-id="32f4c-130">Lär dig mer om DNS-zoner och poster genom att besöka: [DNS-zoner och innehåller en översikt över](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="32f4c-130">Learn about DNS zones and records by visiting: [DNS zones and records overview](dns-zones-records.md).</span></span>

<span data-ttu-id="32f4c-131">Lär dig hur du [skapa en DNS-zon](./dns-getstarted-create-dnszone-portal.md) i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="32f4c-131">Learn how to [create a DNS zone](./dns-getstarted-create-dnszone-portal.md) in Azure DNS.</span></span>

<span data-ttu-id="32f4c-132">Lär dig mer om de andra viktiga [nätverksfunktionerna](../networking/networking-overview.md) i Azure.</span><span class="sxs-lookup"><span data-stu-id="32f4c-132">Learn about some of the other key [networking capabilities](../networking/networking-overview.md) of Azure.</span></span>

