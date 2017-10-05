---
title: "Vanliga frågor om Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller svar på vanliga frågor och svar om Azure Programgateway"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 4e6244d92f41e0aa5c8a70db0db2881036984247
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a><span data-ttu-id="d3a19-103">Vanliga frågor för Programgateway</span><span class="sxs-lookup"><span data-stu-id="d3a19-103">Frequently asked questions for Application Gateway</span></span>

## <a name="general"></a><span data-ttu-id="d3a19-104">Allmänt</span><span class="sxs-lookup"><span data-stu-id="d3a19-104">General</span></span>

<span data-ttu-id="d3a19-105">**FRÅGOR. Vad är Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-105">**Q. What is Application Gateway?**</span></span>

<span data-ttu-id="d3a19-106">Azure Application Gateway är programmet leverans domänkontrollant (ADC) som en tjänst med olika layer 7 belastningsutjämning för dina program.</span><span class="sxs-lookup"><span data-stu-id="d3a19-106">Azure Application Gateway is an Application Delivery Controller (ADC) as a service, offering various layer 7 load balancing capabilities for your applications.</span></span> <span data-ttu-id="d3a19-107">Det ger hög tillgänglighet och skalbarhet tjänsten, som är fullständigt hanterade av Azure.</span><span class="sxs-lookup"><span data-stu-id="d3a19-107">It offers highly available and scalable service, which is fully managed by Azure.</span></span>

<span data-ttu-id="d3a19-108">**FRÅGOR. Vilka funktioner stöder Programgateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-108">**Q. What features does Application Gateway support?**</span></span>

<span data-ttu-id="d3a19-109">Programgateway stöder SSL-avlastning och SSL för slutpunkt till slutpunkt, Brandvägg för webbaserade program, cookie-baserad session tillhörighet, url sökväg-baserade routning, värd för flera plats och andra.</span><span class="sxs-lookup"><span data-stu-id="d3a19-109">Application Gateway supports SSL offloading and end to end SSL, Web Application Firewall, cookie-based session affinity, url path-based routing, multi site hosting, and others.</span></span> <span data-ttu-id="d3a19-110">En fullständig lista över funktioner som stöds finns [introduktion till Programgateway](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="d3a19-110">For a full list of supported features, visit [Introduction to Application Gateway](application-gateway-introduction.md)</span></span>

<span data-ttu-id="d3a19-111">**FRÅGOR. Vad är skillnaden mellan Programgateway och Azure belastningsutjämnare?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-111">**Q. What is the difference between Application Gateway and Azure Load Balancer?**</span></span>

<span data-ttu-id="d3a19-112">Programgateway är en lager 7 belastningsutjämnare, vilket innebär att den fungerar med webbtrafik endast (WebSocket-HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="d3a19-112">Application Gateway is a layer 7 load balancer, which means it works with web traffic only (HTTP/HTTPS/WebSocket).</span></span> <span data-ttu-id="d3a19-113">Det stöder funktioner som SSL-avslutning, cookie-baserad session tillhörighet och resursallokering för trafik för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="d3a19-113">It supports capabilities such as SSL termination, cookie-based session affinity, and round robin for load balancing traffic.</span></span> <span data-ttu-id="d3a19-114">Belastningsutjämnare, Läs in saldon trafik på layer 4 (TCP/UDP).</span><span class="sxs-lookup"><span data-stu-id="d3a19-114">Load Balancer, load balances traffic at layer 4 (TCP/UDP).</span></span>

<span data-ttu-id="d3a19-115">**FRÅGOR. Vilka protokoll stöder Programgateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-115">**Q. What protocols does Application Gateway support?**</span></span>

<span data-ttu-id="d3a19-116">Application Gateway har stöd för HTTP, HTTPS och WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3a19-116">Application Gateway supports HTTP, HTTPS, and WebSocket.</span></span>

<span data-ttu-id="d3a19-117">**FRÅGOR. Vilka resurser som stöds i dag som en del av serverdelspool?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-117">**Q. What resources are supported today as part of backend pool?**</span></span>

<span data-ttu-id="d3a19-118">Serverdelspooler kan bestå av nätverkskort, skalningsuppsättningar i virtuella datorer, offentliga IP-adresser, trots att interna IP-adresser, fullständigt kvalificerade domännamnet (FQDN) och flera innehavare-servrar som Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d3a19-118">Backend pools can be composed of NICs, virtual machine scale sets, public IPs, internal IPs, fully qualified domain names (FQDN), and multi-tenant back-ends like Azure Web Apps.</span></span> <span data-ttu-id="d3a19-119">Programmet Gateway backend-poolmedlemmar är inte kopplad till en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="d3a19-119">Application Gateway backend pool members are not tied to an availability set.</span></span> <span data-ttu-id="d3a19-120">Medlemmar i serverdelspooler kan vara över kluster, Datacenter eller utanför Azure så länge som de har IP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d3a19-120">Members of backend pools can be across clusters, data centers, or outside of Azure as long as they have IP connectivity.</span></span>

<span data-ttu-id="d3a19-121">**FRÅGOR. Vilka regioner är tjänsten?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-121">**Q. What regions is the service available in?**</span></span>

<span data-ttu-id="d3a19-122">Programgateway är tillgänglig i alla regioner för globala Azure.</span><span class="sxs-lookup"><span data-stu-id="d3a19-122">Application Gateway is available in all regions of global Azure.</span></span> <span data-ttu-id="d3a19-123">Det är också tillgängliga i [Azure Kina](https://www.azure.cn/) och [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span><span class="sxs-lookup"><span data-stu-id="d3a19-123">It is also available in [Azure China](https://www.azure.cn/) and [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span></span>

<span data-ttu-id="d3a19-124">**FRÅGOR. Är detta en dedikerad distribution för min prenumeration eller delas mellan kunder?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-124">**Q. Is this a dedicated deployment for my subscription or is it shared across customers?**</span></span>

<span data-ttu-id="d3a19-125">Programgateway är en särskild distribution i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d3a19-125">Application Gateway is a dedicated deployment in your virtual network.</span></span>

<span data-ttu-id="d3a19-126">**FRÅGOR. Är HTTP -> HTTPS omdirigering stöds?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-126">**Q. Is HTTP->HTTPS redirection supported?**</span></span>

<span data-ttu-id="d3a19-127">Omdirigering stöds.</span><span class="sxs-lookup"><span data-stu-id="d3a19-127">Redirection is supported.</span></span> <span data-ttu-id="d3a19-128">Besök [Programgateway omdirigering översikt](application-gateway-redirect-overview.md) vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="d3a19-128">Visit [Application Gateway redirect overview](application-gateway-redirect-overview.md) to learn more.</span></span>

<span data-ttu-id="d3a19-129">**FRÅGOR. I vilken ordning bearbetas lyssnare?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-129">**Q. In what order are listeners processed?**</span></span>

<span data-ttu-id="d3a19-130">Lyssnare bearbetas i den ordning som de visas.</span><span class="sxs-lookup"><span data-stu-id="d3a19-130">Listeners are processed in the order they are shown.</span></span> <span data-ttu-id="d3a19-131">Därför om en grundläggande lyssnare matchar en inkommande begäran bearbetas först.</span><span class="sxs-lookup"><span data-stu-id="d3a19-131">For that reason if a basic listener matches an incoming request it processes it first.</span></span>  <span data-ttu-id="d3a19-132">Lyssnare för flera platser bör konfigureras innan en grundläggande lyssnare så trafik dirigeras till rätt serverdel.</span><span class="sxs-lookup"><span data-stu-id="d3a19-132">Multi-site listeners should be configured before a basic listener to ensure traffic is routed to the correct back-end.</span></span>

<span data-ttu-id="d3a19-133">**FRÅGOR. Var hittar jag Application Gateway IP- och DNS?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-133">**Q. Where do I find Application Gateway’s IP and DNS?**</span></span>

<span data-ttu-id="d3a19-134">När du använder en offentlig IP-adress som en slutpunkt måste kan den här informationen hittas på den offentliga IP-adressresursen eller på sidan Översikt för Programgateway i portalen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-134">When using a public IP address as an endpoint, this information can be found on the public IP address resource or on the Overview page for the Application Gateway in the portal.</span></span> <span data-ttu-id="d3a19-135">För interna IP-adresser finns detta på sidan Översikt.</span><span class="sxs-lookup"><span data-stu-id="d3a19-135">For internal IP addresses, this can be found on the Overview page.</span></span>

<span data-ttu-id="d3a19-136">**FRÅGOR. IP- eller DNS-ändras under livslängden för Programgatewayen?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-136">**Q. Does the IP or DNS change over the lifetime of the Application Gateway?**</span></span>

<span data-ttu-id="d3a19-137">Ändra VIP om gatewayen stoppas och startas av kunden.</span><span class="sxs-lookup"><span data-stu-id="d3a19-137">The VIP can change if the gateway is stopped and started by the customer.</span></span> <span data-ttu-id="d3a19-138">DNS-servern som är associerade med Programgateway ändras inte under hela livscykeln för gateway.</span><span class="sxs-lookup"><span data-stu-id="d3a19-138">The DNS associated with Application Gateway does not change over the lifecycle of the gateway.</span></span> <span data-ttu-id="d3a19-139">Därför rekommenderas att använda en CNAME-alias och peka på DNS-serveradress för Programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-139">For this reason, it is recommended to use a CNAME alias and point it to the DNS address of the Application Gateway.</span></span>

<span data-ttu-id="d3a19-140">**FRÅGOR. Application Gateway har stöd för statisk IP-adress?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-140">**Q. Does Application Gateway support static IP?**</span></span>

<span data-ttu-id="d3a19-141">Nej, Application Gateway har inte stöd för statiska offentliga IP-adresser, men det stöder statiska interna IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="d3a19-141">No, Application Gateway does not support static public IP addresses, but it does support static internal IPs.</span></span>

<span data-ttu-id="d3a19-142">**FRÅGOR. Stöder Programgateway flera offentliga IP-adresser på gateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-142">**Q. Does Application Gateway support multiple public IPs on the gateway?**</span></span>

<span data-ttu-id="d3a19-143">Endast en offentlig IP-adress stöds på en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d3a19-143">Only one public IP address is supported on an Application Gateway.</span></span>

<span data-ttu-id="d3a19-144">**FRÅGOR. Stöder Programgateway x vidarebefordras för huvuden?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-144">**Q. Does Application Gateway support x-forwarded-for headers?**</span></span>

<span data-ttu-id="d3a19-145">Ja, infogar Programgateway x vidarebefordras för x vidarebefordras proto och x vidarebefordras port huvuden i begäran vidarebefordras till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-145">Yes, Application Gateway inserts x-forwarded-for, x-forwarded-proto, and x-forwarded-port headers into the request forwarded to the backend.</span></span> <span data-ttu-id="d3a19-146">Formatet för x vidarebefordras för huvudet är en kommaavgränsad lista över IP:Port.</span><span class="sxs-lookup"><span data-stu-id="d3a19-146">The format for x-forwarded-for header is a comma-separated list of IP:Port.</span></span> <span data-ttu-id="d3a19-147">Giltiga värden för x vidarebefordras proto är http eller https.</span><span class="sxs-lookup"><span data-stu-id="d3a19-147">The valid values for x-forwarded-proto are http or https.</span></span> <span data-ttu-id="d3a19-148">X vidarebefordras port Anger vilken port som begäran uppnått vid Programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-148">X-forwarded-port specifies the port at which the request reached at the Application Gateway.</span></span>

<span data-ttu-id="d3a19-149">**FRÅGOR. Hur lång tid tar det innan du distribuerar en Programgateway? Min Programgateway fortfarande fungerar när uppdateras?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-149">**Q. How long does it take to deploy an Application Gateway? Does my Application Gateway still work when being updated?**</span></span>

<span data-ttu-id="d3a19-150">Nya Programgateway distributioner kan ta upp till 20 minuter att etablera.</span><span class="sxs-lookup"><span data-stu-id="d3a19-150">New Application Gateway deployments can take up to 20 minutes to provision.</span></span> <span data-ttu-id="d3a19-151">Ändringar i storlek/instansantal är inte störande och gatewayen förblir aktiv under denna tid.</span><span class="sxs-lookup"><span data-stu-id="d3a19-151">Changes to instance size/count are not disruptive, and the gateway remains active during this time.</span></span>

## <a name="configuration"></a><span data-ttu-id="d3a19-152">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="d3a19-152">Configuration</span></span>

<span data-ttu-id="d3a19-153">**FRÅGOR. Programgateway alltid distribueras i ett virtuellt nätverk?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-153">**Q. Is Application Gateway always deployed in a virtual network?**</span></span>

<span data-ttu-id="d3a19-154">Ja, distribueras alltid Application Gateway i ett undernät för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="d3a19-154">Yes, Application Gateway is always deployed in a virtual network subnet.</span></span> <span data-ttu-id="d3a19-155">Det här undernätet kan endast innehålla Programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="d3a19-155">This subnet can only contain Application Gateways.</span></span>

<span data-ttu-id="d3a19-156">**FRÅGOR. Kontakta Programgateway till instanser utanför dess virtuellt nätverk?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-156">**Q. Can Application Gateway talk to instances outside its virtual network?**</span></span>

<span data-ttu-id="d3a19-157">Programgateway kan kommunicera instanser utanför det virtuella nätverket som det är så länge det finns en IP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d3a19-157">Application Gateway can talk to instances outside of the virtual network that it is in as long as there is IP connectivity.</span></span> <span data-ttu-id="d3a19-158">Om du planerar att använda interna IP-adresser som medlemmar i serverdelen och det kräver [VNET-Peering](../virtual-network/virtual-network-peering-overview.md) eller [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="d3a19-158">If you plan to use internal IPs as backend pool members, then it requires [VNET Peering](../virtual-network/virtual-network-peering-overview.md) or [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>

<span data-ttu-id="d3a19-159">**FRÅGOR. Kan jag distribuera något annat i Programgateway undernät?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-159">**Q. Can I deploy anything else in the Application Gateway subnet?**</span></span>

<span data-ttu-id="d3a19-160">Nej, men du kan distribuera andra programgatewayer i undernätet.</span><span class="sxs-lookup"><span data-stu-id="d3a19-160">No, but you can deploy other application gateways in the subnet.</span></span>

<span data-ttu-id="d3a19-161">**FRÅGOR. Stöds Nätverkssäkerhetsgrupper i undernät Programgateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-161">**Q. Are Network Security Groups supported on the Application Gateway subnet?**</span></span>

<span data-ttu-id="d3a19-162">Nätverkssäkerhetsgrupper stöds i Application Gateway-undernät med följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="d3a19-162">Network Security Groups are supported on the Application Gateway subnet with the following restrictions:</span></span>

* <span data-ttu-id="d3a19-163">Undantag ställas på inkommande trafik på portarna 65503 65534 för backend-hälsotillstånd ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="d3a19-163">Exceptions must be put in for incoming traffic on ports 65503-65534 for backend health to work correctly.</span></span>

* <span data-ttu-id="d3a19-164">Utgående internet-anslutningen blockeras inte.</span><span class="sxs-lookup"><span data-stu-id="d3a19-164">Outbound internet connectivity can not be blocked.</span></span>

* <span data-ttu-id="d3a19-165">Trafik från taggen AzureLoadBalancer måste tillåtas.</span><span class="sxs-lookup"><span data-stu-id="d3a19-165">Traffic from the AzureLoadBalancer tag must be allowed.</span></span>

<span data-ttu-id="d3a19-166">**FRÅGOR. Vad är begränsningar i Programgateway? Kan jag öka dessa gränser?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-166">**Q. What are the limits on Application Gateway? Can I increase these limits?**</span></span>

<span data-ttu-id="d3a19-167">Besök [programmet Gateway gränser](../azure-subscription-service-limits.md#application-gateway-limits) att visa gränserna.</span><span class="sxs-lookup"><span data-stu-id="d3a19-167">Visit [Application Gateway Limits](../azure-subscription-service-limits.md#application-gateway-limits) to view the limits.</span></span>

<span data-ttu-id="d3a19-168">**FRÅGOR. Kan jag använda Programgateway för både externa och interna trafik samtidigt?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-168">**Q. Can I use Application Gateway for both external and internal traffic simultaneously?**</span></span>

<span data-ttu-id="d3a19-169">Ja, stöder Application Gateway har en intern IP-adress och en extern IP-adress per Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="d3a19-169">Yes, Application Gateway supports having one internal IP and one external IP per Application Gateway.</span></span>

<span data-ttu-id="d3a19-170">**FRÅGOR. VNet-peering stöds?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-170">**Q. Is VNet peering supported?**</span></span>

<span data-ttu-id="d3a19-171">Ja, VNet-peering stöds och är bra för trafik i andra virtuella nätverk för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="d3a19-171">Yes, VNet peering is supported and is beneficial for load balancing traffic in other virtual networks.</span></span>

<span data-ttu-id="d3a19-172">**FRÅGOR. Kan jag tala med lokala servrar när de är anslutna via ExpressRoute eller VPN-tunnlar?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-172">**Q. Can I talk to on-premises servers when they are connected by ExpressRoute or VPN tunnels?**</span></span>

<span data-ttu-id="d3a19-173">Ja, förutsatt att trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="d3a19-173">Yes, as long as traffic is allowed.</span></span>

<span data-ttu-id="d3a19-174">**FRÅGOR. Kan jag har en serverdelspool som betjänar många program på olika portar?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-174">**Q. Can I have one backend pool serving many applications on different ports?**</span></span>

<span data-ttu-id="d3a19-175">Arkitektur för Micro stöds.</span><span class="sxs-lookup"><span data-stu-id="d3a19-175">Micro service architecture is supported.</span></span> <span data-ttu-id="d3a19-176">Du behöver flera http-inställningar som konfigurerats för avsökning på olika portar.</span><span class="sxs-lookup"><span data-stu-id="d3a19-176">You would need multiple http settings configured to probe on different ports.</span></span>

<span data-ttu-id="d3a19-177">**FRÅGOR. Stöder anpassade avsökningar jokertecken/regex på svarsdata?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-177">**Q. Do custom probes support wildcards/regex on response data?**</span></span>

<span data-ttu-id="d3a19-178">Anpassade avsökningar stöder inte jokertecken eller regex på svarsdata.</span><span class="sxs-lookup"><span data-stu-id="d3a19-178">Custom probes do not support wildcard or regex on response data.</span></span> 

<span data-ttu-id="d3a19-179">**FRÅGOR. Hur bearbetas regler?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-179">**Q. How are rules processed?**</span></span>

<span data-ttu-id="d3a19-180">Regler bearbetas i den ordning som de är konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="d3a19-180">Rules are processed in the order they are configured.</span></span> <span data-ttu-id="d3a19-181">Du rekommenderas att regler för flera platser konfigureras innan grundläggande regler för att minska risken att trafik dirigeras till olämpliga serverdelen som grundläggande regeln matchar baserat på port innan regeln flera platser utvärderas.</span><span class="sxs-lookup"><span data-stu-id="d3a19-181">It is recommended that multi-site rules are configured before basic rules to reduce the chance that traffic is routed to the inappropriate backend as the basic rule would match traffic based on port prior to the multi-site rule being evaluated.</span></span>

<span data-ttu-id="d3a19-182">**FRÅGOR. Hur bearbetas regler?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-182">**Q. How are rules processed?**</span></span>

<span data-ttu-id="d3a19-183">Regler bearbetas i den ordning de skapades.</span><span class="sxs-lookup"><span data-stu-id="d3a19-183">Rules are processed in the order they are created.</span></span> <span data-ttu-id="d3a19-184">Du rekommenderas att regler för flera platser konfigureras före grundläggande regler.</span><span class="sxs-lookup"><span data-stu-id="d3a19-184">It is recommended that multi-site rules are configured before basic rules.</span></span> <span data-ttu-id="d3a19-185">Genom att konfigurera flera platser lyssnare först den här konfigurationen minskar risken att trafik dirigeras till olämpliga serverdelen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-185">By configuring multi-site listeners first, this configuration reduces the chance that traffic is routed to the inappropriate backend.</span></span> <span data-ttu-id="d3a19-186">Den här routning kan inträffa eftersom grundläggande regeln matchar baserat på port innan regeln flera platser utvärderas.</span><span class="sxs-lookup"><span data-stu-id="d3a19-186">This routing issue can occur as the basic rule would match traffic based on port prior to the multi-site rule being evaluated.</span></span>

<span data-ttu-id="d3a19-187">**FRÅGOR. Vad fältet värden för anpassade avsökningar obestämd?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-187">**Q. What does the Host field for custom probes signify?**</span></span>

<span data-ttu-id="d3a19-188">Värdfältet namnet att skicka avsökningen till.</span><span class="sxs-lookup"><span data-stu-id="d3a19-188">Host field specifies the name to send the probe to.</span></span> <span data-ttu-id="d3a19-189">Gäller endast när flera platser har konfigurerats på Application Gateway, Använd annars ”127.0.0.1”.</span><span class="sxs-lookup"><span data-stu-id="d3a19-189">Applicable only when multi-site is configured on Application Gateway, otherwise use '127.0.0.1'.</span></span> <span data-ttu-id="d3a19-190">Det här värdet skiljer sig från den virtuella datorns värdnamn och har formatet \<protokollet\>://\<värden\>:\<port\>\<sökvägen\>.</span><span class="sxs-lookup"><span data-stu-id="d3a19-190">This value is different from VM host name and is in format \<protocol\>://\<host\>:\<port\>\<path\>.</span></span>

<span data-ttu-id="d3a19-191">**FRÅGOR. Kan jag godkända Programgateway åtkomst till några käll-IP-adresser?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-191">**Q. Can I whitelist Application Gateway access to a few source IPs?**</span></span>

<span data-ttu-id="d3a19-192">Det här scenariot kan göras med hjälp av NSG: er i Application Gateway-undernät.</span><span class="sxs-lookup"><span data-stu-id="d3a19-192">This scenario can be done using NSGs on Application Gateway subnet.</span></span> <span data-ttu-id="d3a19-193">Följande begränsningar ska placeras i undernät i listan prioritetsordning:</span><span class="sxs-lookup"><span data-stu-id="d3a19-193">The following restrictions should be put on the subnet in the listed order of priority:</span></span>

* <span data-ttu-id="d3a19-194">Tillåt inkommande trafik från käll-IP-/ IP-intervall.</span><span class="sxs-lookup"><span data-stu-id="d3a19-194">Allow incoming traffic from source IP/IP range.</span></span>

* <span data-ttu-id="d3a19-195">Tillåt inkommande begäranden från alla källor till portar 65503 65534 för [backend hälsa kommunikation](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="d3a19-195">Allow incoming requests from all sources to ports 65503-65534 for [backend health communication](application-gateway-diagnostics.md).</span></span>

* <span data-ttu-id="d3a19-196">Tillåt inkommande Azure belastningsutjämnare avsökningar (taggen AzureLoadBalancer) och inkommande trafik i virtuella nätverk (taggen VirtualNetwork) på den [NSG](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="d3a19-196">Allow incoming Azure Load Balancer probes (AzureLoadBalancer tag) and inbound virtual network traffic (VirtualNetwork tag) on the [NSG](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="d3a19-197">Blockera alla andra inkommande trafik med en neka alla regeln.</span><span class="sxs-lookup"><span data-stu-id="d3a19-197">Block all other incoming traffic with a Deny all rule.</span></span>

* <span data-ttu-id="d3a19-198">Tillåt utgående trafik till internet för alla mål.</span><span class="sxs-lookup"><span data-stu-id="d3a19-198">Allow outbound traffic to the internet for all destinations.</span></span>

## <a name="performance"></a><span data-ttu-id="d3a19-199">Prestanda</span><span class="sxs-lookup"><span data-stu-id="d3a19-199">Performance</span></span>

<span data-ttu-id="d3a19-200">**FRÅGOR. Hur stöder Programgateway hög tillgänglighet och skalbarhet?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-200">**Q. How does Application Gateway support high availability and scalability?**</span></span>

<span data-ttu-id="d3a19-201">Application Gateway har stöd för scenarier med hög tillgänglighet när du har två eller fler instanser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="d3a19-201">Application Gateway supports high availability scenarios when you have two or more instances deployed.</span></span> <span data-ttu-id="d3a19-202">Azure distribuerar dessa instanser uppdatering och feltolerans domäner så att alla instanser inte växlar på samma gång.</span><span class="sxs-lookup"><span data-stu-id="d3a19-202">Azure distributes these instances across update and fault domains to ensure that all instances do not fail at the same time.</span></span> <span data-ttu-id="d3a19-203">Programgateway stöder skalbarhet genom att lägga till flera instanser av samma gateway att dela belastningen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-203">Application Gateway supports scalability by adding multiple instances of the same gateway to share the load.</span></span>

<span data-ttu-id="d3a19-204">**FRÅGOR. Hur jag uppnå katastrofåterställning över datacenter med Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-204">**Q. How do I achieve DR scenario across data centers with Application Gateway?**</span></span>

<span data-ttu-id="d3a19-205">Kunder kan använda Traffic Manager distribuerar trafik över flera Programgatewayer i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="d3a19-205">Customers can use Traffic Manager to distribute traffic across multiple Application Gateways in different datacenters.</span></span>

<span data-ttu-id="d3a19-206">**FRÅGOR. Automatisk skalning stöds?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-206">**Q. Is auto scaling supported?**</span></span>

<span data-ttu-id="d3a19-207">Nej, men Application Gateway har ett mått på genomflödet som kan användas för att meddela dig när ett tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="d3a19-207">No, but Application Gateway has a throughput metric that can be used to alert you when a threshold is reached.</span></span> <span data-ttu-id="d3a19-208">Lägga till instanser manuellt eller ändrar storlek startar inte om gatewayen och påverkar inte befintliga trafiken.</span><span class="sxs-lookup"><span data-stu-id="d3a19-208">Manually adding instances or changing size does not restart the gateway and does not impact existing traffic.</span></span>

<span data-ttu-id="d3a19-209">**FRÅGOR. Stöder manuell skala upp eller ned orsak driftavbrott?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-209">**Q. Does manual scale up/down cause downtime?**</span></span>

<span data-ttu-id="d3a19-210">Det finns inget driftstopp, instanser är fördelade på uppgraderingsdomäner och feldomäner.</span><span class="sxs-lookup"><span data-stu-id="d3a19-210">There is no downtime, instances are distributed across upgrade domains and fault domains.</span></span>

<span data-ttu-id="d3a19-211">**FRÅGOR. Kan jag ändra instansstorleken från medelstora till stora utan avbrott?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-211">**Q. Can I change instance size from medium to large without disruption?**</span></span>

<span data-ttu-id="d3a19-212">Ja, distribuerar Azure instanser uppdatering och feltolerans domäner så att alla instanser inte växlar på samma gång.</span><span class="sxs-lookup"><span data-stu-id="d3a19-212">Yes, Azure distributes instances across update and fault domains to ensure that all instances do not fail at the same time.</span></span> <span data-ttu-id="d3a19-213">Application Gateway har stöd för skalning genom att lägga till flera instanser av samma gateway att dela belastningen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-213">Application Gateway supports scaling by adding multiple instances of the same gateway to share the load.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="d3a19-214">SSL-konfiguration</span><span class="sxs-lookup"><span data-stu-id="d3a19-214">SSL Configuration</span></span>

<span data-ttu-id="d3a19-215">**FRÅGOR. Vilka certifikat som stöds på Programgateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-215">**Q. What certificates are supported on Application Gateway?**</span></span>

<span data-ttu-id="d3a19-216">Automatiskt signerat certifikat, CA-certifikat och certifikat jokertecken stöds.</span><span class="sxs-lookup"><span data-stu-id="d3a19-216">Self signed certs, CA certs, and wild-card certs are supported.</span></span> <span data-ttu-id="d3a19-217">EV certifikat stöds inte.</span><span class="sxs-lookup"><span data-stu-id="d3a19-217">EV certs are not supported.</span></span>

<span data-ttu-id="d3a19-218">**FRÅGOR. Vilka är de aktuella krypteringssviter som stöds av Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-218">**Q. What are the current cipher suites supported by Application Gateway?**</span></span>

<span data-ttu-id="d3a19-219">Följande är de aktuella krypteringssviter som stöds av Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d3a19-219">The following are the current cipher suites supported by application gateway.</span></span> <span data-ttu-id="d3a19-220">Besök: [Konfigurera SSL princip versioner och krypteringssviter på Programgateway](application-gateway-configure-ssl-policy-powershell.md) att lära dig hur du anpassar SSL-alternativ.</span><span class="sxs-lookup"><span data-stu-id="d3a19-220">Visit: [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) to learn how to customize SSL options.</span></span>

- <span data-ttu-id="d3a19-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="d3a19-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="d3a19-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="d3a19-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="d3a19-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="d3a19-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="d3a19-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="d3a19-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="d3a19-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="d3a19-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="d3a19-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="d3a19-233">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-233">TLS_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-234">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-234">TLS_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="d3a19-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="d3a19-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="d3a19-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="d3a19-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="d3a19-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="d3a19-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="d3a19-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="d3a19-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span></span>
- <span data-ttu-id="d3a19-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="d3a19-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span></span>

<span data-ttu-id="d3a19-247">**FRÅGOR. Stöder Programgateway också omkryptering av trafik till serverdelen?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-247">**Q. Does Application Gateway also support re-encryption of traffic to the backend?**</span></span>

<span data-ttu-id="d3a19-248">Ja, stöder Programgateway avlastning SSL och SSL för slutpunkt till slutpunkt, som krypterar trafik till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-248">Yes, Application Gateway supports SSL offload, and end to end SSL, which re-encrypts the traffic to the backend.</span></span>

<span data-ttu-id="d3a19-249">**FRÅGOR. Kan jag konfigurera SSL-policy för att styra SSL-protokoll versioner?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-249">**Q. Can I configure SSL policy to control SSL Protocol versions?**</span></span>

<span data-ttu-id="d3a19-250">Ja, kan du konfigurera Programgateway att neka TLS1.0 och TLS1.1 TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="d3a19-250">Yes, you can configure Application Gateway to deny TLS1.0, TLS1.1, and TLS1.2.</span></span> <span data-ttu-id="d3a19-251">SSL 2.0 och 3.0 är redan inaktiverad som standard och kan inte konfigureras.</span><span class="sxs-lookup"><span data-stu-id="d3a19-251">SSL 2.0 and 3.0 are already disabled by default and are not configurable.</span></span>

<span data-ttu-id="d3a19-252">**FRÅGOR. Kan jag konfigurera chiffersviter och principen ordning?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-252">**Q. Can I configure cipher suites and policy order?**</span></span>

<span data-ttu-id="d3a19-253">Ja, [konfiguration av krypteringssviter](application-gateway-ssl-policy-overview.md) stöds.</span><span class="sxs-lookup"><span data-stu-id="d3a19-253">Yes, [configuration of cipher suites](application-gateway-ssl-policy-overview.md) is supported.</span></span> <span data-ttu-id="d3a19-254">När du definierar en anpassad princip måste minst en av de följande krypteringssviter aktiveras.</span><span class="sxs-lookup"><span data-stu-id="d3a19-254">When defining a custom policy, at least one of the following cipher suites must be enabled.</span></span> <span data-ttu-id="d3a19-255">Programgateway använder SHA256 till för hantering av backend.</span><span class="sxs-lookup"><span data-stu-id="d3a19-255">Application gateway uses SHA256 to for backend management.</span></span>

* <span data-ttu-id="d3a19-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span> 
* <span data-ttu-id="d3a19-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
* <span data-ttu-id="d3a19-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="d3a19-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="d3a19-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
* <span data-ttu-id="d3a19-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="d3a19-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>

<span data-ttu-id="d3a19-262">**FRÅGOR. Hur många SSL-certifikat som stöds?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-262">**Q. How many SSL certificates are supported?**</span></span>

<span data-ttu-id="d3a19-263">Upp till 20 SSL som certifikat stöds.</span><span class="sxs-lookup"><span data-stu-id="d3a19-263">Up to 20 SSL certificates are supported.</span></span>

<span data-ttu-id="d3a19-264">**FRÅGOR. Hur många certifikat för klientautentisering för backend-omkryptering stöds?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-264">**Q. How many authentication certificates for backend re-encryption are supported?**</span></span>

<span data-ttu-id="d3a19-265">Upp till 10 autentisering stöds certifikat med ett standardvärde på 5.</span><span class="sxs-lookup"><span data-stu-id="d3a19-265">Up to 10 authentication certificates are supported with a default of 5.</span></span>

<span data-ttu-id="d3a19-266">**FRÅGOR. Kan Programgateway integreras med Azure Key Vault internt?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-266">**Q. Does Application Gateway integrate with Azure Key Vault natively?**</span></span>

<span data-ttu-id="d3a19-267">Nej, det inte är integrerat med Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d3a19-267">No, it is not integrated with Azure Key Vault.</span></span>

## <a name="web-application-firewall-waf-configuration"></a><span data-ttu-id="d3a19-268">Konfiguration av webbprogram Firewall (Brandvägg)</span><span class="sxs-lookup"><span data-stu-id="d3a19-268">Web Application Firewall (WAF) Configuration</span></span>

<span data-ttu-id="d3a19-269">**FRÅGOR. Erbjuder Brandvägg SKU alla funktioner som är tillgängliga med Standard-SKU?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-269">**Q. Does the WAF SKU offer all the features available with the Standard SKU?**</span></span>

<span data-ttu-id="d3a19-270">Ja, Brandvägg stöder alla funktioner i Standard-SKU.</span><span class="sxs-lookup"><span data-stu-id="d3a19-270">Yes, WAF supports all the features in the Standard SKU.</span></span>

<span data-ttu-id="d3a19-271">**FRÅGOR. Vad är versionen som CR Application Gateway har stöd för?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-271">**Q. What is the CRS version Application Gateway supports?**</span></span>

<span data-ttu-id="d3a19-272">Programgateway stöder CR [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) och CR [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span><span class="sxs-lookup"><span data-stu-id="d3a19-272">Application Gateway supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span></span>

<span data-ttu-id="d3a19-273">**FRÅGOR. Hur övervakar Brandvägg?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-273">**Q. How do I monitor WAF?**</span></span>

<span data-ttu-id="d3a19-274">Brandvägg övervakas via diagnostikloggning, mer information om diagnostikloggning kan hittas på [diagnostik loggning och mått för Programgateway](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="d3a19-274">WAF is monitored through diagnostic logging, more information on diagnostic logging can be found at [Diagnostics Logging and Metrics for Application Gateway](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="d3a19-275">**FRÅGOR. Blockerar identifieringsläget trafik?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-275">**Q. Does detection mode block traffic?**</span></span>

<span data-ttu-id="d3a19-276">Nej, loggar identifieringsläget endast trafik som utlöses av en Brandvägg regel.</span><span class="sxs-lookup"><span data-stu-id="d3a19-276">No, detection mode only logs traffic, which triggered a WAF rule.</span></span>

<span data-ttu-id="d3a19-277">**FRÅGOR. Hur anpassar Brandvägg regler?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-277">**Q. How do I customize WAF rules?**</span></span>

<span data-ttu-id="d3a19-278">Ja, Brandvägg regler kan anpassas för mer information om hur du anpassar dem besök [anpassa Brandvägg regelgrupper och regler](application-gateway-customize-waf-rules-portal.md)</span><span class="sxs-lookup"><span data-stu-id="d3a19-278">Yes, WAF rules are customizable, for more information on how to customize them visit [Customize WAF rule groups and rules](application-gateway-customize-waf-rules-portal.md)</span></span>

<span data-ttu-id="d3a19-279">**FRÅGOR. Vilka regler som är tillgängliga?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-279">**Q. What rules are currently available?**</span></span>

<span data-ttu-id="d3a19-280">Brandvägg stöder för närvarande CR [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) och [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), som ger grundläggande säkerhet mot de flesta av de översta 10 säkerhetsrisker som identifieras av den öppna Web Application säkerhet projekt (OWASP) finns här [ OWASP topp 10 säkerhetsrisker](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span><span class="sxs-lookup"><span data-stu-id="d3a19-280">WAF currently supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), which provide baseline security against most of the top 10 vulnerabilities identified by the Open Web Application Security Project (OWASP) found here [OWASP top 10 Vulnerabilities](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span></span>

* <span data-ttu-id="d3a19-281">Skydd mot SQL-inmatning</span><span class="sxs-lookup"><span data-stu-id="d3a19-281">SQL injection protection</span></span>

* <span data-ttu-id="d3a19-282">Skydd mot skriptkörning över flera webbplatser</span><span class="sxs-lookup"><span data-stu-id="d3a19-282">Cross site scripting protection</span></span>

* <span data-ttu-id="d3a19-283">Skydd mot vanliga webbattacker, som kommandoinmatning, dold HTTP-begäran, delning av HTTP-svar och attack genom införande av fjärrfil</span><span class="sxs-lookup"><span data-stu-id="d3a19-283">Common Web Attacks Protection such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attack</span></span>

* <span data-ttu-id="d3a19-284">Skydd mot åtgärder som inte följer HTTP-protokollet</span><span class="sxs-lookup"><span data-stu-id="d3a19-284">Protection against HTTP protocol violations</span></span>

* <span data-ttu-id="d3a19-285">Skydd mot avvikelser i HTTP-protokollet som att användaragent för värden och accept-huvud saknas</span><span class="sxs-lookup"><span data-stu-id="d3a19-285">Protection against HTTP protocol anomalies such as missing host user-agent and accept headers</span></span>

* <span data-ttu-id="d3a19-286">Skydd mot robotar, crawlers och skannrar</span><span class="sxs-lookup"><span data-stu-id="d3a19-286">Prevention against bots, crawlers, and scanners</span></span>

* <span data-ttu-id="d3a19-287">Identifiering av program vanliga felinställningar (det vill säga Apache, IIS osv.)</span><span class="sxs-lookup"><span data-stu-id="d3a19-287">Detection of common application misconfigurations (that is, Apache, IIS, etc.)</span></span>

<span data-ttu-id="d3a19-288">**FRÅGOR. Stöder Brandvägg också DDoS förebyggande?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-288">**Q. Does WAF also support DDoS prevention?**</span></span>

<span data-ttu-id="d3a19-289">Nej, Brandvägg ger inte några DDoS förebyggande.</span><span class="sxs-lookup"><span data-stu-id="d3a19-289">No, WAF does not provide DDoS prevention.</span></span>

## <a name="diagnostics-and-logging"></a><span data-ttu-id="d3a19-290">Diagnostik- och loggning</span><span class="sxs-lookup"><span data-stu-id="d3a19-290">Diagnostics and Logging</span></span>

<span data-ttu-id="d3a19-291">**FRÅGOR. Vilka typer av loggar är tillgängliga med Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-291">**Q. What types of logs are available with Application Gateway?**</span></span>

<span data-ttu-id="d3a19-292">Det finns tre loggarna för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d3a19-292">There are three logs available for Application Gateway.</span></span> <span data-ttu-id="d3a19-293">Mer information om dessa loggar och andra diagnostiska funktioner finns [Backend hälsa och diagnostik loggar mått för Programgateway](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="d3a19-293">For more information on these logs and other diagnostic capabilities, visit [Backend health, diagnostics logs, and metrics for Application Gateway](application-gateway-diagnostics.md).</span></span>

- <span data-ttu-id="d3a19-294">**ApplicationGatewayAccessLog** -åtkomst-loggen innehåller varje förfrågan har skickats till Programgateway klientdel.</span><span class="sxs-lookup"><span data-stu-id="d3a19-294">**ApplicationGatewayAccessLog** - The access log contains each request submitted to the Application Gateway frontend.</span></span> <span data-ttu-id="d3a19-295">Innehåller information som anroparens IP begärd, URL svar svarstid, returkod, byte in och ut. Åtkomstlogg samlas in varje 300 sekunder.</span><span class="sxs-lookup"><span data-stu-id="d3a19-295">The data includes the caller's IP, URL requested, response latency, return code, bytes in and out. Access log is collected every 300 seconds.</span></span> <span data-ttu-id="d3a19-296">Den här loggfilen innehåller en post per instans av Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="d3a19-296">This log contains one record per instance of Application Gateway.</span></span>
- <span data-ttu-id="d3a19-297">**ApplicationGatewayPerformanceLog** -prestanda-loggen innehåller prestandainformation om för per instanser inklusive totala begäran skickades, genomflöde i byte, totalt antal begäranden som betjänats, antalet misslyckade begäranden, felfria och feltillstånd backend- instansantal.</span><span class="sxs-lookup"><span data-stu-id="d3a19-297">**ApplicationGatewayPerformanceLog** - The performance log captures performance information on per instance basis including total request served, throughput in bytes, total requests served, failed request count, healthy and unhealthy back-end instance count.</span></span>
- <span data-ttu-id="d3a19-298">**ApplicationGatewayFirewallLog** -brandväggsloggen innehåller begäranden som loggas via identifiering eller förhindra läget för en Programgateway som är konfigurerad med Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="d3a19-298">**ApplicationGatewayFirewallLog** - The firewall log contains requests that are logged through either detection or prevention mode of an application gateway that is configured with web application firewall.</span></span>

<span data-ttu-id="d3a19-299">**FRÅGOR. Hur vet jag om min backend poolmedlemmar felfri?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-299">**Q. How do I know if my backend pool members are healthy?**</span></span>

<span data-ttu-id="d3a19-300">Du kan använda PowerShell-cmdleten `Get-AzureRmApplicationGatewayBackendHealth` eller verifiera hälsotillstånd via portalen genom att besöka [Gateway Programdiagnostik](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="d3a19-300">You can use the PowerShell cmdlet `Get-AzureRmApplicationGatewayBackendHealth` or verify health through the portal by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="d3a19-301">**FRÅGOR. Vad är bevarandeprincip på diagnostik loggarna?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-301">**Q. What is the retention policy on the diagnostics logs?**</span></span>

<span data-ttu-id="d3a19-302">Diagnostikloggar flöde till kunder storage-konto och kunder kan ange den bevarandeprincip baserat på deras inställningar.</span><span class="sxs-lookup"><span data-stu-id="d3a19-302">Diagnostic logs flow to the customers storage account and customers can set the retention policy based on their preference.</span></span> <span data-ttu-id="d3a19-303">Diagnostikloggar kan också skickas till en Händelsehubb eller logganalys.</span><span class="sxs-lookup"><span data-stu-id="d3a19-303">Diagnostic logs can also be sent to an Event Hub or Log Analytics.</span></span> <span data-ttu-id="d3a19-304">Besök [Gateway Programdiagnostik](application-gateway-diagnostics.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d3a19-304">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) for more details.</span></span>

<span data-ttu-id="d3a19-305">**FRÅGOR. Hur skaffar jag granskningsloggarna för Programgateway**</span><span class="sxs-lookup"><span data-stu-id="d3a19-305">**Q. How do I get audit logs for Application Gateway?**</span></span>

<span data-ttu-id="d3a19-306">Granskningsloggar är tillgängliga för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="d3a19-306">Audit logs are available for Application Gateway.</span></span> <span data-ttu-id="d3a19-307">I portalen klickar du på **aktivitetsloggen** i bladet menyn för en Programgateway åtkomst till händelseloggen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-307">In the portal, click **Activity Log** in the menu blade of an Application Gateway to access the audit log.</span></span> 

<span data-ttu-id="d3a19-308">**FRÅGOR. Kan jag ställa in aviseringar med Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-308">**Q. Can I set alerts with Application Gateway?**</span></span>

<span data-ttu-id="d3a19-309">Ja, Application Gateway har stöd för varningar, aviseringar konfigureras av mått.</span><span class="sxs-lookup"><span data-stu-id="d3a19-309">Yes, Application Gateway does support alerts, alerts are configured off metrics.</span></span>  <span data-ttu-id="d3a19-310">Application Gateway har för närvarande ”dataflöde”, vilket kan konfigureras för mått till aviseringen.</span><span class="sxs-lookup"><span data-stu-id="d3a19-310">Application Gateway currently has a metric of "throughput", which can be configured to alert.</span></span> <span data-ttu-id="d3a19-311">Läs mer om aviseringar om [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="d3a19-311">To learn more about alerts, visit [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="d3a19-312">**FRÅGOR. Backend-hälsa returnerar okänd status, vilka kan vara orsaken till denna status?**</span><span class="sxs-lookup"><span data-stu-id="d3a19-312">**Q. Backend health returns unknown status, what could be causing this status?**</span></span>

<span data-ttu-id="d3a19-313">Den vanligaste orsaken är åtkomst till serverdelen blockeras av en NSG eller anpassade DNS.</span><span class="sxs-lookup"><span data-stu-id="d3a19-313">The most common reason is access to the backend is being blocked by an NSG or custom DNS.</span></span> <span data-ttu-id="d3a19-314">Besök [Backend hälsa, diagnostikloggning och mått för Programgateway](application-gateway-diagnostics.md) vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="d3a19-314">Visit [Backend health, diagnostics logging, and metrics for Application Gateway](application-gateway-diagnostics.md) to learn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3a19-315">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3a19-315">Next Steps</span></span>

<span data-ttu-id="d3a19-316">Mer information om Programgateway besök [introduktion till Programgateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d3a19-316">To learn more about Application Gateway visit [Introduction to Application Gateway](application-gateway-introduction.md).</span></span>
