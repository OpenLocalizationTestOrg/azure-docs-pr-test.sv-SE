---
title: "aaaOverview IPv6 för Azure-belastningsutjämnaren | Microsoft Docs"
description: "Så här fungerar IPv6-stöd för Azure belastningsutjämnare och belastningsutjämnade virtuella datorer."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "IPv6, azure belastningsutjämnare, dual stack, offentlig IP-adress, inbyggd ipv6, mobil, iot"
ms.assetid: 6a1d583f-a305-40fd-a94b-fa42e1943bbb
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: 5b203f77d86cc1ad455f4ebb297097aef46b658d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a><span data-ttu-id="aa8d4-104">Översikt över IPv6 för Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="aa8d4-104">Overview of IPv6 for Azure Load Balancer</span></span>

<span data-ttu-id="aa8d4-105">Internetriktade belastningsutjämnare kan distribueras med en IPv6-adress.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-105">Internet-facing load balancers can be deployed with an IPv6 address.</span></span> <span data-ttu-id="aa8d4-106">Dessutom tooIPv4 anslutningen detta gör att hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="aa8d4-106">In addition tooIPv4 connectivity, this enables hello following capabilities:</span></span>

* <span data-ttu-id="aa8d4-107">Intern slutpunkt till slutpunkt IPv6-anslutning mellan offentliga Internet-klienter och Azure virtuella datorer (VM) via hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-107">Native end-to-end IPv6 connectivity between public Internet clients and Azure Virtual Machines (VMs) through hello load balancer.</span></span>
* <span data-ttu-id="aa8d4-108">Intern slutpunkt till slutpunkt IPv6 utgående anslutningar mellan virtuella datorer och offentliga Internet IPv6-aktiverade klienter.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-108">Native end-to-end IPv6 outbound connectivity between VMs and public Internet IPv6-enabled clients.</span></span>

<span data-ttu-id="aa8d4-109">hello följande bild illustrerar hello IPv6-funktioner för Azure-belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-109">hello following picture illustrates hello IPv6 functionality for Azure Load Balancer.</span></span>

![Azure belastningsutjämnare med IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

<span data-ttu-id="aa8d4-111">När har distribuerats, kan en IPv4- eller IPv6-aktiverad Internet-klient kommunicera med hello offentliga IPv4- eller IPv6-adresser (eller värdnamn) av hello Azure Internetriktade belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-111">Once deployed, an IPv4 or IPv6-enabled Internet client can communicate with hello public IPv4 or IPv6 addresses (or hostnames) of hello Azure Internet-facing Load Balancer.</span></span> <span data-ttu-id="aa8d4-112">hello belastningen belastningsutjämnaren vägar hello IPv6-paket toohello privata IPv6-adresser för hello virtuella datorer med hjälp av network address translation (NAT).</span><span class="sxs-lookup"><span data-stu-id="aa8d4-112">hello load balancer routes hello IPv6 packets toohello private IPv6 addresses of hello VMs using network address translation (NAT).</span></span> <span data-ttu-id="aa8d4-113">hello IPv6-Internet-klienten inte kan kommunicera direkt med hello IPv6-adressen för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-113">hello IPv6 Internet client cannot communicate directly with hello IPv6 address of hello VMs.</span></span>

## <a name="features"></a><span data-ttu-id="aa8d4-114">Funktioner</span><span class="sxs-lookup"><span data-stu-id="aa8d4-114">Features</span></span>

<span data-ttu-id="aa8d4-115">Inbyggt IPv6-stöd för virtuella datorer som distribueras via Azure Resource Manager innehåller:</span><span class="sxs-lookup"><span data-stu-id="aa8d4-115">Native IPv6 support for VMs deployed via Azure Resource Manager provides:</span></span>

1. <span data-ttu-id="aa8d4-116">Utjämning av nätverksbelastning IPv6-tjänster för IPv6-klienter i hello Internet</span><span class="sxs-lookup"><span data-stu-id="aa8d4-116">Load-balanced IPv6 services for IPv6 clients on hello Internet</span></span>
2. <span data-ttu-id="aa8d4-117">Inbyggd IPv6 och IPv4-slutpunkterna på virtuella datorer (”dubbla Staplad”)</span><span class="sxs-lookup"><span data-stu-id="aa8d4-117">Native IPv6 and IPv4 endpoints on VMs ("dual stacked")</span></span>
3. <span data-ttu-id="aa8d4-118">Inkommande och utgående initieras inbyggda IPv6-anslutningar</span><span class="sxs-lookup"><span data-stu-id="aa8d4-118">Inbound and outbound-initiated native IPv6 connections</span></span>
4. <span data-ttu-id="aa8d4-119">Protokoll som stöds till exempel TCP, UDP- och HTTP (S) Aktivera en mängd olika arkitekturer för tjänsten</span><span class="sxs-lookup"><span data-stu-id="aa8d4-119">Supported protocols such as TCP, UDP, and HTTP(S) enable a full range of service architectures</span></span>

## <a name="benefits"></a><span data-ttu-id="aa8d4-120">Fördelar</span><span class="sxs-lookup"><span data-stu-id="aa8d4-120">Benefits</span></span>

<span data-ttu-id="aa8d4-121">Den här funktionen möjliggör hello följande viktiga fördelar:</span><span class="sxs-lookup"><span data-stu-id="aa8d4-121">This functionality enables hello following key benefits:</span></span>

* <span data-ttu-id="aa8d4-122">Uppfyller offentliga regleringar som kräver att nya program är tillgängligt endast tooIPv6-klienter</span><span class="sxs-lookup"><span data-stu-id="aa8d4-122">Meet government regulations requiring that new applications be accessible tooIPv6-only clients</span></span>
* <span data-ttu-id="aa8d4-123">Aktivera mobil- och Internet av saker (IOT) utvecklare toouse dubbla Staplad (IPv4 + IPv6) Azure Virtual Machines tooaddress hello växande mobile & IOT marknader</span><span class="sxs-lookup"><span data-stu-id="aa8d4-123">Enable mobile and Internet of things (IOT) developers toouse dual-stacked (IPv4+IPv6) Azure Virtual Machines tooaddress hello growing mobile & IOT markets</span></span>

## <a name="details-and-limitations"></a><span data-ttu-id="aa8d4-124">Information och begränsningar</span><span class="sxs-lookup"><span data-stu-id="aa8d4-124">Details and limitations</span></span>

<span data-ttu-id="aa8d4-125">Information</span><span class="sxs-lookup"><span data-stu-id="aa8d4-125">Details</span></span>

* <span data-ttu-id="aa8d4-126">hello Azure DNS-tjänsten innehåller poster för både IPv4-A- och IPv6-AAAA namn och svarar med båda posterna för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-126">hello Azure DNS service contains both IPv4 A and IPv6 AAAA name records and responds with both records for hello load balancer.</span></span> <span data-ttu-id="aa8d4-127">hello klienten väljer vilka adress (IPv4 eller IPv6) toocommunicate med.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-127">hello client chooses which address (IPv4 or IPv6) toocommunicate with.</span></span>
* <span data-ttu-id="aa8d4-128">När en virtuell dator initierar anslutning tooa offentliga Internet IPv6-anslutna enheter, hello VM källa IPv6-adress som är nätverksadress översättas (NAT) toohello offentliga IPv6-adressen för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-128">When a VM initiates a connection tooa public Internet IPv6-connected device, hello VM's source IPv6 address is network address translated (NAT) toohello public IPv6 address of hello load balancer.</span></span>
* <span data-ttu-id="aa8d4-129">Virtuella datorer som kör hello Linux-operativsystem måste vara konfigurerade tooreceive en IPv6-IP-adress via DHCP.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-129">VMs running hello Linux operating system must be configured tooreceive an IPv6 IP address via DHCP.</span></span> <span data-ttu-id="aa8d4-130">Många av hello Linux bilder i hello Azure-galleriet redan är konfigurerad toosupport IPv6 utan modifiering.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-130">Many of hello Linux images in hello Azure Gallery are already configured toosupport IPv6 without modification.</span></span> <span data-ttu-id="aa8d4-131">Mer information finns i [konfigurera DHCPv6 för virtuella Linux-datorer](load-balancer-ipv6-for-linux.md)</span><span class="sxs-lookup"><span data-stu-id="aa8d4-131">For more information, see [Configuring DHCPv6 for Linux VMs](load-balancer-ipv6-for-linux.md)</span></span>
* <span data-ttu-id="aa8d4-132">Om du väljer att skapa en IPv4-avsökning toouse en avsökning med din belastningsutjämnare och använda den med både hello IPv4 och IPv6-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-132">If you choose toouse a health probe with your load balancer, create an IPv4 probe and use it with both hello IPv4 and IPv6 endpoints.</span></span> <span data-ttu-id="aa8d4-133">Om hello-tjänsten på den virtuella datorn stängs av, tas både hello IPv4 och IPv6-slutpunkter bort från rotationen.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-133">If hello service on your VM goes down, both hello IPv4 and IPv6 endpoints are taken out of rotation.</span></span>

<span data-ttu-id="aa8d4-134">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="aa8d4-134">Limitations</span></span>

* <span data-ttu-id="aa8d4-135">Du kan inte lägga till IPv6-belastningsutjämningsregler i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-135">You cannot add IPv6 load balancing rules in hello Azure portal.</span></span> <span data-ttu-id="aa8d4-136">hello-regler kan endast skapas med hello mall CLI, PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-136">hello rules can only be created through hello template, CLI, PowerShell.</span></span>
* <span data-ttu-id="aa8d4-137">Du kan inte uppgradera befintlig VMs toouse IPv6-adresser.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-137">You may not upgrade existing VMs toouse IPv6 addresses.</span></span> <span data-ttu-id="aa8d4-138">Du måste distribuera nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-138">You must deploy new VMs.</span></span>
* <span data-ttu-id="aa8d4-139">En IPv6-adress kan tilldelas tooa nätverksgränssnitt på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-139">A single IPv6 address can be assigned tooa single network interface in each VM.</span></span>
* <span data-ttu-id="aa8d4-140">hello offentliga IPv6-adresser kan inte tilldelas tooa VM.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-140">hello public IPv6 addresses cannot be assigned tooa VM.</span></span> <span data-ttu-id="aa8d4-141">De kan endast tilldelas tooa belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-141">They can only be assigned tooa load balancer.</span></span>
* <span data-ttu-id="aa8d4-142">Du kan konfigurera hello omvänd DNS-sökning för din offentliga IPv6-adresser.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-142">You cannot configure hello reverse DNS lookup for your public IPv6 addresses.</span></span>
* <span data-ttu-id="aa8d4-143">hello virtuella datorer med hello IPv6-adresser kan inte vara medlemmar i en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-143">hello VMs with hello IPv6 addresses cannot be members of an Azure Cloud Service.</span></span> <span data-ttu-id="aa8d4-144">De kan anslutna tooan Azure Virtual Network (VNet) och kommunicera med varandra via sina IPv4-adresser.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-144">They can be connected tooan Azure Virtual Network (VNet) and communicate with each other over their IPv4 addresses.</span></span>
* <span data-ttu-id="aa8d4-145">Privat IPv6-adresser kan distribueras på enskilda virtuella datorer i en resursgrupp, men kan inte distribueras i en resursgrupp via Skaluppsättningar.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-145">Private IPv6 addresses can be deployed on individual VMs in a resource group but cannot be deployed into a resource group via Scale Sets.</span></span>
* <span data-ttu-id="aa8d4-146">Virtuella Azure-datorer kan inte ansluta över IPv6 tooother virtuella datorer, andra Azure-tjänster eller lokala enheter.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-146">Azure VMs cannot connect over IPv6 tooother VMs, other Azure services, or on-premises devices.</span></span> <span data-ttu-id="aa8d4-147">De kan endast kommunicera med hello Azure belastningsutjämnare via IPv6.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-147">They can only communicate with hello Azure load balancer over IPv6.</span></span> <span data-ttu-id="aa8d4-148">De kan dock kommunicera med andra resurserna med IPv4.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-148">However, they can communicate with these other resources using IPv4.</span></span>
* <span data-ttu-id="aa8d4-149">Nätverkssäkerhetsgrupp (NSG)-skydd för IPv4 stöds i dual stack (IPv4 + IPv6) distributioner.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-149">Network Security Group (NSG) protection for IPv4 is supported in dual-stack (IPv4+IPv6) deployments.</span></span> <span data-ttu-id="aa8d4-150">NSG: er gäller inte toohello IPv6-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-150">NSGs do not apply toohello IPv6 endpoints.</span></span>
* <span data-ttu-id="aa8d4-151">hello IPv6-slutpunkten på hello virtuella datorn inte är direkt exponerade toohello internet.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-151">hello IPv6 endpoint on hello VM is not exposed directly toohello internet.</span></span> <span data-ttu-id="aa8d4-152">Det är bakom en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-152">It is behind a load balancer.</span></span> <span data-ttu-id="aa8d4-153">Endast hello portar som angetts i hello regler för inläsning av belastningsutjämnaren är tillgänglig via IPv6.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-153">Only hello ports specified in hello load balancer rules are accessible over IPv6.</span></span>
* <span data-ttu-id="aa8d4-154">Ändra hello IdleTimeout parametern för IPv6 är **stöds för närvarande inte**.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-154">Changing hello IdleTimeout parameter for IPv6 is **currently not supported**.</span></span> <span data-ttu-id="aa8d4-155">hello standardvärdet är fyra minuter.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-155">hello default is four minutes.</span></span>
* <span data-ttu-id="aa8d4-156">Ändra hello loadDistributionMethod parameter för IPv6 är **stöds för närvarande inte**.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-156">Changing hello loadDistributionMethod parameter for IPv6 is **currently not supported**.</span></span>
* <span data-ttu-id="aa8d4-157">Reserverad IP-IPv6-adresser (där ipallocationmethod inställt = statisk) är **stöds för närvarande inte**.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-157">Reserved IPv6 IPs (where IPAllocationMethod = static) are **currently not supported**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa8d4-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa8d4-158">Next steps</span></span>

<span data-ttu-id="aa8d4-159">Lär dig hur toodeploy en belastningsutjämnare med IPv6.</span><span class="sxs-lookup"><span data-stu-id="aa8d4-159">Learn how toodeploy a load balancer with IPv6.</span></span>

* [<span data-ttu-id="aa8d4-160">Tillgängligheten för IPv6 efter region</span><span class="sxs-lookup"><span data-stu-id="aa8d4-160">Availability of IPv6 by region</span></span>](https://go.microsoft.com/fwlink/?linkid=828357)
* [<span data-ttu-id="aa8d4-161">Distribuera en belastningsutjämnare med IPv6 med en mall</span><span class="sxs-lookup"><span data-stu-id="aa8d4-161">Deploy a load balancer with IPv6 using a template</span></span>](load-balancer-ipv6-internet-template.md)
* [<span data-ttu-id="aa8d4-162">Distribuera en belastningsutjämnare med IPv6 med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa8d4-162">Deploy a load balancer with IPv6 using Azure PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
* [<span data-ttu-id="aa8d4-163">Distribuera en belastningsutjämnare med IPv6 med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="aa8d4-163">Deploy a load balancer with IPv6 using Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
