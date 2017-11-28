---
title: "frågor för Azure Programgateway och aaaFrequently | Microsoft Docs"
description: "Den här sidan innehåller svar toofrequently frågor och svar om Azure Programgateway"
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
ms.openlocfilehash: b2df3a82a71a3264d3d34d317d08e4b4f72c6e3e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a><span data-ttu-id="19108-103">Vanliga frågor för Programgateway</span><span class="sxs-lookup"><span data-stu-id="19108-103">Frequently asked questions for Application Gateway</span></span>

## <a name="general"></a><span data-ttu-id="19108-104">Allmänt</span><span class="sxs-lookup"><span data-stu-id="19108-104">General</span></span>

<span data-ttu-id="19108-105">**FRÅGOR. Vad är Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-105">**Q. What is Application Gateway?**</span></span>

<span data-ttu-id="19108-106">Azure Application Gateway är programmet leverans domänkontrollant (ADC) som en tjänst med olika layer 7 belastningsutjämning för dina program.</span><span class="sxs-lookup"><span data-stu-id="19108-106">Azure Application Gateway is an Application Delivery Controller (ADC) as a service, offering various layer 7 load balancing capabilities for your applications.</span></span> <span data-ttu-id="19108-107">Det ger hög tillgänglighet och skalbarhet tjänsten, som är fullständigt hanterade av Azure.</span><span class="sxs-lookup"><span data-stu-id="19108-107">It offers highly available and scalable service, which is fully managed by Azure.</span></span>

<span data-ttu-id="19108-108">**FRÅGOR. Vilka funktioner stöder Programgateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-108">**Q. What features does Application Gateway support?**</span></span>

<span data-ttu-id="19108-109">Programgateway stöder SSL genom att avlasta- och slutdatum tooend SSL, Brandvägg för webbaserade program, cookie-baserad session tillhörighet, url sökväg-baserade routning, värd för flera plats och andra.</span><span class="sxs-lookup"><span data-stu-id="19108-109">Application Gateway supports SSL offloading and end tooend SSL, Web Application Firewall, cookie-based session affinity, url path-based routing, multi site hosting, and others.</span></span> <span data-ttu-id="19108-110">En fullständig lista över funktioner som stöds finns [introduktion tooApplication Gateway](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="19108-110">For a full list of supported features, visit [Introduction tooApplication Gateway](application-gateway-introduction.md)</span></span>

<span data-ttu-id="19108-111">**FRÅGOR. Vad är hello skillnaden mellan Programgateway och Azure belastningsutjämnare?**</span><span class="sxs-lookup"><span data-stu-id="19108-111">**Q. What is hello difference between Application Gateway and Azure Load Balancer?**</span></span>

<span data-ttu-id="19108-112">Programgateway är en lager 7 belastningsutjämnare, vilket innebär att den fungerar med webbtrafik endast (WebSocket-HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="19108-112">Application Gateway is a layer 7 load balancer, which means it works with web traffic only (HTTP/HTTPS/WebSocket).</span></span> <span data-ttu-id="19108-113">Det stöder funktioner som SSL-avslutning, cookie-baserad session tillhörighet och resursallokering för trafik för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="19108-113">It supports capabilities such as SSL termination, cookie-based session affinity, and round robin for load balancing traffic.</span></span> <span data-ttu-id="19108-114">Belastningsutjämnare, Läs in saldon trafik på layer 4 (TCP/UDP).</span><span class="sxs-lookup"><span data-stu-id="19108-114">Load Balancer, load balances traffic at layer 4 (TCP/UDP).</span></span>

<span data-ttu-id="19108-115">**FRÅGOR. Vilka protokoll stöder Programgateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-115">**Q. What protocols does Application Gateway support?**</span></span>

<span data-ttu-id="19108-116">Application Gateway har stöd för HTTP, HTTPS och WebSocket.</span><span class="sxs-lookup"><span data-stu-id="19108-116">Application Gateway supports HTTP, HTTPS, and WebSocket.</span></span>

<span data-ttu-id="19108-117">**FRÅGOR. Vilka resurser som stöds i dag som en del av serverdelspool?**</span><span class="sxs-lookup"><span data-stu-id="19108-117">**Q. What resources are supported today as part of backend pool?**</span></span>

<span data-ttu-id="19108-118">Serverdelspooler kan bestå av nätverkskort, skalningsuppsättningar i virtuella datorer, offentliga IP-adresser, trots att interna IP-adresser, fullständigt kvalificerade domännamnet (FQDN) och flera innehavare-servrar som Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="19108-118">Backend pools can be composed of NICs, virtual machine scale sets, public IPs, internal IPs, fully qualified domain names (FQDN), and multi-tenant back-ends like Azure Web Apps.</span></span> <span data-ttu-id="19108-119">Programgateway backend inte poolmedlemmar knutna tooan tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="19108-119">Application Gateway backend pool members are not tied tooan availability set.</span></span> <span data-ttu-id="19108-120">Medlemmar i serverdelspooler kan vara över kluster, Datacenter eller utanför Azure så länge som de har IP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="19108-120">Members of backend pools can be across clusters, data centers, or outside of Azure as long as they have IP connectivity.</span></span>

<span data-ttu-id="19108-121">**FRÅGOR. Vilka regioner är hello tjänsten?**</span><span class="sxs-lookup"><span data-stu-id="19108-121">**Q. What regions is hello service available in?**</span></span>

<span data-ttu-id="19108-122">Programgateway är tillgänglig i alla regioner för globala Azure.</span><span class="sxs-lookup"><span data-stu-id="19108-122">Application Gateway is available in all regions of global Azure.</span></span> <span data-ttu-id="19108-123">Det är också tillgängliga i [Azure Kina](https://www.azure.cn/) och [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span><span class="sxs-lookup"><span data-stu-id="19108-123">It is also available in [Azure China](https://www.azure.cn/) and [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span></span>

<span data-ttu-id="19108-124">**FRÅGOR. Är detta en dedikerad distribution för min prenumeration eller delas mellan kunder?**</span><span class="sxs-lookup"><span data-stu-id="19108-124">**Q. Is this a dedicated deployment for my subscription or is it shared across customers?**</span></span>

<span data-ttu-id="19108-125">Programgateway är en särskild distribution i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="19108-125">Application Gateway is a dedicated deployment in your virtual network.</span></span>

<span data-ttu-id="19108-126">**FRÅGOR. Är HTTP -> HTTPS omdirigering stöds?**</span><span class="sxs-lookup"><span data-stu-id="19108-126">**Q. Is HTTP->HTTPS redirection supported?**</span></span>

<span data-ttu-id="19108-127">Omdirigering stöds.</span><span class="sxs-lookup"><span data-stu-id="19108-127">Redirection is supported.</span></span> <span data-ttu-id="19108-128">Besök [Programgateway omdirigering översikt](application-gateway-redirect-overview.md) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="19108-128">Visit [Application Gateway redirect overview](application-gateway-redirect-overview.md) toolearn more.</span></span>

<span data-ttu-id="19108-129">**FRÅGOR. I vilken ordning bearbetas lyssnare?**</span><span class="sxs-lookup"><span data-stu-id="19108-129">**Q. In what order are listeners processed?**</span></span>

<span data-ttu-id="19108-130">Lyssnare bearbetas i hello ordning de visas.</span><span class="sxs-lookup"><span data-stu-id="19108-130">Listeners are processed in hello order they are shown.</span></span> <span data-ttu-id="19108-131">Därför om en grundläggande lyssnare matchar en inkommande begäran bearbetas först.</span><span class="sxs-lookup"><span data-stu-id="19108-131">For that reason if a basic listener matches an incoming request it processes it first.</span></span>  <span data-ttu-id="19108-132">Lyssnare för flera platser bör konfigureras innan en grundläggande lyssnare tooensure trafik dirigeras toohello rätt backend.</span><span class="sxs-lookup"><span data-stu-id="19108-132">Multi-site listeners should be configured before a basic listener tooensure traffic is routed toohello correct back-end.</span></span>

<span data-ttu-id="19108-133">**FRÅGOR. Var hittar jag Application Gateway IP- och DNS?**</span><span class="sxs-lookup"><span data-stu-id="19108-133">**Q. Where do I find Application Gateway’s IP and DNS?**</span></span>

<span data-ttu-id="19108-134">När du använder en offentlig IP-adress som en slutpunkt, kan den här informationen hittas på hello offentliga IP-adressresurs eller hello översiktssidan för hello Programgateway hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="19108-134">When using a public IP address as an endpoint, this information can be found on hello public IP address resource or on hello Overview page for hello Application Gateway in hello portal.</span></span> <span data-ttu-id="19108-135">För interna IP-adresser, kan detta hittas på översiktssidan för hello.</span><span class="sxs-lookup"><span data-stu-id="19108-135">For internal IP addresses, this can be found on hello Overview page.</span></span>

<span data-ttu-id="19108-136">**FRÅGOR. Hello IP- eller DNS-ändras under hello livslängd för hello Programgateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-136">**Q. Does hello IP or DNS change over hello lifetime of hello Application Gateway?**</span></span>

<span data-ttu-id="19108-137">Ändra hello VIP om hello gateway stoppas och startas av hello kunden.</span><span class="sxs-lookup"><span data-stu-id="19108-137">hello VIP can change if hello gateway is stopped and started by hello customer.</span></span> <span data-ttu-id="19108-138">hello DNS som är associerade med Programgateway ändras inte över hello livscykeln för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="19108-138">hello DNS associated with Application Gateway does not change over hello lifecycle of hello gateway.</span></span> <span data-ttu-id="19108-139">Därför är det rekommenderade toouse en CNAME-alias och peka på den toohello DNS-serveradress för hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="19108-139">For this reason, it is recommended toouse a CNAME alias and point it toohello DNS address of hello Application Gateway.</span></span>

<span data-ttu-id="19108-140">**FRÅGOR. Application Gateway har stöd för statisk IP-adress?**</span><span class="sxs-lookup"><span data-stu-id="19108-140">**Q. Does Application Gateway support static IP?**</span></span>

<span data-ttu-id="19108-141">Nej, Application Gateway har inte stöd för statiska offentliga IP-adresser, men det stöder statiska interna IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="19108-141">No, Application Gateway does not support static public IP addresses, but it does support static internal IPs.</span></span>

<span data-ttu-id="19108-142">**FRÅGOR. Stöder Programgateway flera offentliga IP-adresser på hello gateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-142">**Q. Does Application Gateway support multiple public IPs on hello gateway?**</span></span>

<span data-ttu-id="19108-143">Endast en offentlig IP-adress stöds på en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="19108-143">Only one public IP address is supported on an Application Gateway.</span></span>

<span data-ttu-id="19108-144">**FRÅGOR. Stöder Programgateway x vidarebefordras för huvuden?**</span><span class="sxs-lookup"><span data-stu-id="19108-144">**Q. Does Application Gateway support x-forwarded-for headers?**</span></span>

<span data-ttu-id="19108-145">Ja, Programgateway infogar x vidarebefordras för, x vidarebefordras proto och x vidarebefordras port huvuden i hello begäran vidarebefordras toohello backend.</span><span class="sxs-lookup"><span data-stu-id="19108-145">Yes, Application Gateway inserts x-forwarded-for, x-forwarded-proto, and x-forwarded-port headers into hello request forwarded toohello backend.</span></span> <span data-ttu-id="19108-146">hello-formatet för x vidarebefordras för huvudet är en kommaavgränsad lista över IP:Port.</span><span class="sxs-lookup"><span data-stu-id="19108-146">hello format for x-forwarded-for header is a comma-separated list of IP:Port.</span></span> <span data-ttu-id="19108-147">hello giltiga värden för x vidarebefordras proto är http eller https.</span><span class="sxs-lookup"><span data-stu-id="19108-147">hello valid values for x-forwarded-proto are http or https.</span></span> <span data-ttu-id="19108-148">X vidarebefordras port Anger hello port på vilken hello-begäran på hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="19108-148">X-forwarded-port specifies hello port at which hello request reached at hello Application Gateway.</span></span>

<span data-ttu-id="19108-149">**FRÅGOR. Hur lång tid tar det toodeploy en Programgateway? Min Programgateway fortfarande fungerar när uppdateras?**</span><span class="sxs-lookup"><span data-stu-id="19108-149">**Q. How long does it take toodeploy an Application Gateway? Does my Application Gateway still work when being updated?**</span></span>

<span data-ttu-id="19108-150">Nya Programgateway distributioner kan ta upp too20 minuter tooprovision.</span><span class="sxs-lookup"><span data-stu-id="19108-150">New Application Gateway deployments can take up too20 minutes tooprovision.</span></span> <span data-ttu-id="19108-151">Ändringar tooinstance storlek/antal är inte störande och hello gateway förblir aktiv under denna tid.</span><span class="sxs-lookup"><span data-stu-id="19108-151">Changes tooinstance size/count are not disruptive, and hello gateway remains active during this time.</span></span>

## <a name="configuration"></a><span data-ttu-id="19108-152">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="19108-152">Configuration</span></span>

<span data-ttu-id="19108-153">**FRÅGOR. Programgateway alltid distribueras i ett virtuellt nätverk?**</span><span class="sxs-lookup"><span data-stu-id="19108-153">**Q. Is Application Gateway always deployed in a virtual network?**</span></span>

<span data-ttu-id="19108-154">Ja, distribueras alltid Application Gateway i ett undernät för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="19108-154">Yes, Application Gateway is always deployed in a virtual network subnet.</span></span> <span data-ttu-id="19108-155">Det här undernätet kan endast innehålla Programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="19108-155">This subnet can only contain Application Gateways.</span></span>

<span data-ttu-id="19108-156">**FRÅGOR. Kontakta Programgateway tooinstances utanför dess virtuella nätverket?**</span><span class="sxs-lookup"><span data-stu-id="19108-156">**Q. Can Application Gateway talk tooinstances outside its virtual network?**</span></span>

<span data-ttu-id="19108-157">Programgateway kontakta tooinstances utanför hello virtuella nätverk som det är så länge det finns en IP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="19108-157">Application Gateway can talk tooinstances outside of hello virtual network that it is in as long as there is IP connectivity.</span></span> <span data-ttu-id="19108-158">Om du planerar toouse interna IP-adresser som backend poolmedlemmar, då kräver [VNET-Peering](../virtual-network/virtual-network-peering-overview.md) eller [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="19108-158">If you plan toouse internal IPs as backend pool members, then it requires [VNET Peering](../virtual-network/virtual-network-peering-overview.md) or [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>

<span data-ttu-id="19108-159">**FRÅGOR. Kan jag distribuera något annat i hello Programgateway undernät?**</span><span class="sxs-lookup"><span data-stu-id="19108-159">**Q. Can I deploy anything else in hello Application Gateway subnet?**</span></span>

<span data-ttu-id="19108-160">Nej, men du kan distribuera andra programgatewayer i hello undernät.</span><span class="sxs-lookup"><span data-stu-id="19108-160">No, but you can deploy other application gateways in hello subnet.</span></span>

<span data-ttu-id="19108-161">**FRÅGOR. Stöds Nätverkssäkerhetsgrupper på hello Programgateway undernät?**</span><span class="sxs-lookup"><span data-stu-id="19108-161">**Q. Are Network Security Groups supported on hello Application Gateway subnet?**</span></span>

<span data-ttu-id="19108-162">Nätverkssäkerhetsgrupper stöds i hello Programgateway undernät med hello följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="19108-162">Network Security Groups are supported on hello Application Gateway subnet with hello following restrictions:</span></span>

* <span data-ttu-id="19108-163">Undantag ställas på inkommande trafik på portarna 65503 65534 för serverdelen hälsa toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="19108-163">Exceptions must be put in for incoming traffic on ports 65503-65534 for backend health toowork correctly.</span></span>

* <span data-ttu-id="19108-164">Utgående internet-anslutningen blockeras inte.</span><span class="sxs-lookup"><span data-stu-id="19108-164">Outbound internet connectivity can not be blocked.</span></span>

* <span data-ttu-id="19108-165">Trafik från hello taggen AzureLoadBalancer måste tillåtas.</span><span class="sxs-lookup"><span data-stu-id="19108-165">Traffic from hello AzureLoadBalancer tag must be allowed.</span></span>

<span data-ttu-id="19108-166">**FRÅGOR. Vad är hello gränserna för Programgateway? Kan jag öka dessa gränser?**</span><span class="sxs-lookup"><span data-stu-id="19108-166">**Q. What are hello limits on Application Gateway? Can I increase these limits?**</span></span>

<span data-ttu-id="19108-167">Besök [programmet Gateway gränser](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello gränser.</span><span class="sxs-lookup"><span data-stu-id="19108-167">Visit [Application Gateway Limits](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello limits.</span></span>

<span data-ttu-id="19108-168">**FRÅGOR. Kan jag använda Programgateway för både externa och interna trafik samtidigt?**</span><span class="sxs-lookup"><span data-stu-id="19108-168">**Q. Can I use Application Gateway for both external and internal traffic simultaneously?**</span></span>

<span data-ttu-id="19108-169">Ja, stöder Application Gateway har en intern IP-adress och en extern IP-adress per Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="19108-169">Yes, Application Gateway supports having one internal IP and one external IP per Application Gateway.</span></span>

<span data-ttu-id="19108-170">**FRÅGOR. VNet-peering stöds?**</span><span class="sxs-lookup"><span data-stu-id="19108-170">**Q. Is VNet peering supported?**</span></span>

<span data-ttu-id="19108-171">Ja, VNet-peering stöds och är bra för trafik i andra virtuella nätverk för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="19108-171">Yes, VNet peering is supported and is beneficial for load balancing traffic in other virtual networks.</span></span>

<span data-ttu-id="19108-172">**FRÅGOR. Kan jag kontakta tooon lokala servrar när de är anslutna via ExpressRoute eller VPN-tunnlar?**</span><span class="sxs-lookup"><span data-stu-id="19108-172">**Q. Can I talk tooon-premises servers when they are connected by ExpressRoute or VPN tunnels?**</span></span>

<span data-ttu-id="19108-173">Ja, förutsatt att trafik tillåts.</span><span class="sxs-lookup"><span data-stu-id="19108-173">Yes, as long as traffic is allowed.</span></span>

<span data-ttu-id="19108-174">**FRÅGOR. Kan jag har en serverdelspool som betjänar många program på olika portar?**</span><span class="sxs-lookup"><span data-stu-id="19108-174">**Q. Can I have one backend pool serving many applications on different ports?**</span></span>

<span data-ttu-id="19108-175">Arkitektur för Micro stöds.</span><span class="sxs-lookup"><span data-stu-id="19108-175">Micro service architecture is supported.</span></span> <span data-ttu-id="19108-176">Du behöver flera HTTP-inställningarna tooprobe på olika portar.</span><span class="sxs-lookup"><span data-stu-id="19108-176">You would need multiple http settings configured tooprobe on different ports.</span></span>

<span data-ttu-id="19108-177">**FRÅGOR. Stöder anpassade avsökningar jokertecken/regex på svarsdata?**</span><span class="sxs-lookup"><span data-stu-id="19108-177">**Q. Do custom probes support wildcards/regex on response data?**</span></span>

<span data-ttu-id="19108-178">Anpassade avsökningar stöder inte jokertecken eller regex på svarsdata.</span><span class="sxs-lookup"><span data-stu-id="19108-178">Custom probes do not support wildcard or regex on response data.</span></span> 

<span data-ttu-id="19108-179">**FRÅGOR. Hur bearbetas regler?**</span><span class="sxs-lookup"><span data-stu-id="19108-179">**Q. How are rules processed?**</span></span>

<span data-ttu-id="19108-180">Regler bearbetas i hello ordning som de är konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="19108-180">Rules are processed in hello order they are configured.</span></span> <span data-ttu-id="19108-181">Du rekommenderas att regler för flera platser konfigureras innan grundläggande regler tooreduce hello chans att trafiken dirigeras toohello olämpliga backend som grundläggande hello-regeln matchar baserat på tidigare toohello flera platser portregel som utvärderas.</span><span class="sxs-lookup"><span data-stu-id="19108-181">It is recommended that multi-site rules are configured before basic rules tooreduce hello chance that traffic is routed toohello inappropriate backend as hello basic rule would match traffic based on port prior toohello multi-site rule being evaluated.</span></span>

<span data-ttu-id="19108-182">**FRÅGOR. Hur bearbetas regler?**</span><span class="sxs-lookup"><span data-stu-id="19108-182">**Q. How are rules processed?**</span></span>

<span data-ttu-id="19108-183">Regler bearbetas i hello ordning de skapades.</span><span class="sxs-lookup"><span data-stu-id="19108-183">Rules are processed in hello order they are created.</span></span> <span data-ttu-id="19108-184">Du rekommenderas att regler för flera platser konfigureras före grundläggande regler.</span><span class="sxs-lookup"><span data-stu-id="19108-184">It is recommended that multi-site rules are configured before basic rules.</span></span> <span data-ttu-id="19108-185">Den här konfigurationen minskar hello chansen att trafik är routade toohello olämpliga backend genom att konfigurera flera platser lyssnare först.</span><span class="sxs-lookup"><span data-stu-id="19108-185">By configuring multi-site listeners first, this configuration reduces hello chance that traffic is routed toohello inappropriate backend.</span></span> <span data-ttu-id="19108-186">Routning problemet kan inträffa eftersom grundläggande hello-regeln matchar baserat på tidigare toohello flera platser portregel som utvärderas.</span><span class="sxs-lookup"><span data-stu-id="19108-186">This routing issue can occur as hello basic rule would match traffic based on port prior toohello multi-site rule being evaluated.</span></span>

<span data-ttu-id="19108-187">**FRÅGOR. Vad hello värdfältet för anpassade avsökningar obestämd?**</span><span class="sxs-lookup"><span data-stu-id="19108-187">**Q. What does hello Host field for custom probes signify?**</span></span>

<span data-ttu-id="19108-188">Värd anger hello namn toosend hello avsökningen till.</span><span class="sxs-lookup"><span data-stu-id="19108-188">Host field specifies hello name toosend hello probe to.</span></span> <span data-ttu-id="19108-189">Gäller endast när flera platser har konfigurerats på Application Gateway, Använd annars ”127.0.0.1”.</span><span class="sxs-lookup"><span data-stu-id="19108-189">Applicable only when multi-site is configured on Application Gateway, otherwise use '127.0.0.1'.</span></span> <span data-ttu-id="19108-190">Det här värdet skiljer sig från den virtuella datorns värdnamn och har formatet \<protokollet\>://\<värden\>:\<port\>\<sökvägen\>.</span><span class="sxs-lookup"><span data-stu-id="19108-190">This value is different from VM host name and is in format \<protocol\>://\<host\>:\<port\>\<path\>.</span></span>

<span data-ttu-id="19108-191">**FRÅGOR. Kan jag godkända Programgateway åtkomst tooa få datakällan IP-adresser?**</span><span class="sxs-lookup"><span data-stu-id="19108-191">**Q. Can I whitelist Application Gateway access tooa few source IPs?**</span></span>

<span data-ttu-id="19108-192">Det här scenariot kan göras med hjälp av NSG: er i Application Gateway-undernät.</span><span class="sxs-lookup"><span data-stu-id="19108-192">This scenario can be done using NSGs on Application Gateway subnet.</span></span> <span data-ttu-id="19108-193">hello följande begränsningar ska placeras i hello undernät i hello visas prioritetsordning:</span><span class="sxs-lookup"><span data-stu-id="19108-193">hello following restrictions should be put on hello subnet in hello listed order of priority:</span></span>

* <span data-ttu-id="19108-194">Tillåt inkommande trafik från käll-IP-/ IP-intervall.</span><span class="sxs-lookup"><span data-stu-id="19108-194">Allow incoming traffic from source IP/IP range.</span></span>

* <span data-ttu-id="19108-195">Tillåt inkommande begäranden från alla källor tooports 65503 65534 för [backend hälsa kommunikation](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="19108-195">Allow incoming requests from all sources tooports 65503-65534 for [backend health communication](application-gateway-diagnostics.md).</span></span>

* <span data-ttu-id="19108-196">Tillåt inkommande Azure belastningsutjämnare avsökningar (taggen AzureLoadBalancer) och inkommande trafik i virtuella nätverk (taggen VirtualNetwork) på hello [NSG](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="19108-196">Allow incoming Azure Load Balancer probes (AzureLoadBalancer tag) and inbound virtual network traffic (VirtualNetwork tag) on hello [NSG](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="19108-197">Blockera alla andra inkommande trafik med en neka alla regeln.</span><span class="sxs-lookup"><span data-stu-id="19108-197">Block all other incoming traffic with a Deny all rule.</span></span>

* <span data-ttu-id="19108-198">Tillåt utgående trafik toohello internet för alla mål.</span><span class="sxs-lookup"><span data-stu-id="19108-198">Allow outbound traffic toohello internet for all destinations.</span></span>

## <a name="performance"></a><span data-ttu-id="19108-199">Prestanda</span><span class="sxs-lookup"><span data-stu-id="19108-199">Performance</span></span>

<span data-ttu-id="19108-200">**FRÅGOR. Hur stöder Programgateway hög tillgänglighet och skalbarhet?**</span><span class="sxs-lookup"><span data-stu-id="19108-200">**Q. How does Application Gateway support high availability and scalability?**</span></span>

<span data-ttu-id="19108-201">Application Gateway har stöd för scenarier med hög tillgänglighet när du har två eller fler instanser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="19108-201">Application Gateway supports high availability scenarios when you have two or more instances deployed.</span></span> <span data-ttu-id="19108-202">Azure distribuerar dessa instanser i uppdateringen och feltolerans domäner tooensure som alla instanser inte växlar vid hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="19108-202">Azure distributes these instances across update and fault domains tooensure that all instances do not fail at hello same time.</span></span> <span data-ttu-id="19108-203">Programgateway stöder skalbarhet genom att lägga till flera instanser av hello samma gateway tooshare hello belastningen.</span><span class="sxs-lookup"><span data-stu-id="19108-203">Application Gateway supports scalability by adding multiple instances of hello same gateway tooshare hello load.</span></span>

<span data-ttu-id="19108-204">**FRÅGOR. Hur jag uppnå katastrofåterställning över datacenter med Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-204">**Q. How do I achieve DR scenario across data centers with Application Gateway?**</span></span>

<span data-ttu-id="19108-205">Kunder kan använda Traffic Manager toodistribute trafik över flera Programgatewayer i olika datacenter.</span><span class="sxs-lookup"><span data-stu-id="19108-205">Customers can use Traffic Manager toodistribute traffic across multiple Application Gateways in different datacenters.</span></span>

<span data-ttu-id="19108-206">**FRÅGOR. Automatisk skalning stöds?**</span><span class="sxs-lookup"><span data-stu-id="19108-206">**Q. Is auto scaling supported?**</span></span>

<span data-ttu-id="19108-207">Nej, men Application Gateway har ett mått på genomflödet som kan använda tooalert när ett tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="19108-207">No, but Application Gateway has a throughput metric that can be used tooalert you when a threshold is reached.</span></span> <span data-ttu-id="19108-208">Manuellt lägga till instanser eller ändrar storlek startar inte om hello gateway och påverkar inte befintliga trafiken.</span><span class="sxs-lookup"><span data-stu-id="19108-208">Manually adding instances or changing size does not restart hello gateway and does not impact existing traffic.</span></span>

<span data-ttu-id="19108-209">**FRÅGOR. Stöder manuell skala upp eller ned orsak driftavbrott?**</span><span class="sxs-lookup"><span data-stu-id="19108-209">**Q. Does manual scale up/down cause downtime?**</span></span>

<span data-ttu-id="19108-210">Det finns inget driftstopp, instanser är fördelade på uppgraderingsdomäner och feldomäner.</span><span class="sxs-lookup"><span data-stu-id="19108-210">There is no downtime, instances are distributed across upgrade domains and fault domains.</span></span>

<span data-ttu-id="19108-211">**FRÅGOR. Kan jag ändra instansstorleken från medelhög toolarge utan avbrott?**</span><span class="sxs-lookup"><span data-stu-id="19108-211">**Q. Can I change instance size from medium toolarge without disruption?**</span></span>

<span data-ttu-id="19108-212">Ja, Azure distribuerar instanser i uppdateringen och feltolerans domäner tooensure som alla instanser inte växlar vid hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="19108-212">Yes, Azure distributes instances across update and fault domains tooensure that all instances do not fail at hello same time.</span></span> <span data-ttu-id="19108-213">Programmet Gateway har stöd för skalning genom att lägga till flera instanser av hello samma gateway tooshare hello belastningen.</span><span class="sxs-lookup"><span data-stu-id="19108-213">Application Gateway supports scaling by adding multiple instances of hello same gateway tooshare hello load.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="19108-214">SSL-konfiguration</span><span class="sxs-lookup"><span data-stu-id="19108-214">SSL Configuration</span></span>

<span data-ttu-id="19108-215">**FRÅGOR. Vilka certifikat som stöds på Programgateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-215">**Q. What certificates are supported on Application Gateway?**</span></span>

<span data-ttu-id="19108-216">Automatiskt signerat certifikat, CA-certifikat och certifikat jokertecken stöds.</span><span class="sxs-lookup"><span data-stu-id="19108-216">Self signed certs, CA certs, and wild-card certs are supported.</span></span> <span data-ttu-id="19108-217">EV certifikat stöds inte.</span><span class="sxs-lookup"><span data-stu-id="19108-217">EV certs are not supported.</span></span>

<span data-ttu-id="19108-218">**FRÅGOR. Vad är hello aktuella krypteringssviter som stöds av Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-218">**Q. What are hello current cipher suites supported by Application Gateway?**</span></span>

<span data-ttu-id="19108-219">hello följande är hello aktuella krypteringssviter som stöds av Programgateway.</span><span class="sxs-lookup"><span data-stu-id="19108-219">hello following are hello current cipher suites supported by application gateway.</span></span> <span data-ttu-id="19108-220">Besök: [Konfigurera SSL princip versioner och krypteringssviter på Programgateway](application-gateway-configure-ssl-policy-powershell.md) toolearn hur toocustomize SSL-alternativ.</span><span class="sxs-lookup"><span data-stu-id="19108-220">Visit: [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn how toocustomize SSL options.</span></span>

- <span data-ttu-id="19108-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="19108-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="19108-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="19108-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="19108-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="19108-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="19108-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="19108-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="19108-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="19108-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="19108-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="19108-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="19108-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="19108-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="19108-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="19108-233">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-233">TLS_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="19108-234">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-234">TLS_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="19108-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="19108-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="19108-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="19108-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="19108-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="19108-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="19108-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="19108-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="19108-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="19108-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="19108-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="19108-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="19108-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span></span>
- <span data-ttu-id="19108-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="19108-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span></span>

<span data-ttu-id="19108-247">**FRÅGOR. Stöder Programgateway också omkryptering av trafik toohello backend?**</span><span class="sxs-lookup"><span data-stu-id="19108-247">**Q. Does Application Gateway also support re-encryption of traffic toohello backend?**</span></span>

<span data-ttu-id="19108-248">Ja, Application Gateway har stöd för SSL-avlastning och end tooend SSL, som krypterar hello trafik toohello backend.</span><span class="sxs-lookup"><span data-stu-id="19108-248">Yes, Application Gateway supports SSL offload, and end tooend SSL, which re-encrypts hello traffic toohello backend.</span></span>

<span data-ttu-id="19108-249">**FRÅGOR. Kan jag konfigurera SSL princip toocontrol SSL-protokoll versioner?**</span><span class="sxs-lookup"><span data-stu-id="19108-249">**Q. Can I configure SSL policy toocontrol SSL Protocol versions?**</span></span>

<span data-ttu-id="19108-250">Ja, du kan konfigurera Programgateway toodeny TLS1.0, TLS1.1 och TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="19108-250">Yes, you can configure Application Gateway toodeny TLS1.0, TLS1.1, and TLS1.2.</span></span> <span data-ttu-id="19108-251">SSL 2.0 och 3.0 är redan inaktiverad som standard och kan inte konfigureras.</span><span class="sxs-lookup"><span data-stu-id="19108-251">SSL 2.0 and 3.0 are already disabled by default and are not configurable.</span></span>

<span data-ttu-id="19108-252">**FRÅGOR. Kan jag konfigurera chiffersviter och principen ordning?**</span><span class="sxs-lookup"><span data-stu-id="19108-252">**Q. Can I configure cipher suites and policy order?**</span></span>

<span data-ttu-id="19108-253">Ja, [konfiguration av krypteringssviter](application-gateway-ssl-policy-overview.md) stöds.</span><span class="sxs-lookup"><span data-stu-id="19108-253">Yes, [configuration of cipher suites](application-gateway-ssl-policy-overview.md) is supported.</span></span> <span data-ttu-id="19108-254">När du definierar en anpassad princip måste minst en av följande krypteringssviter hello aktiveras.</span><span class="sxs-lookup"><span data-stu-id="19108-254">When defining a custom policy, at least one of hello following cipher suites must be enabled.</span></span> <span data-ttu-id="19108-255">Programgateway använder SHA256 toofor backend management.</span><span class="sxs-lookup"><span data-stu-id="19108-255">Application gateway uses SHA256 toofor backend management.</span></span>

* <span data-ttu-id="19108-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span> 
* <span data-ttu-id="19108-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
* <span data-ttu-id="19108-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="19108-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="19108-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
* <span data-ttu-id="19108-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="19108-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>

<span data-ttu-id="19108-262">**FRÅGOR. Hur många SSL-certifikat som stöds?**</span><span class="sxs-lookup"><span data-stu-id="19108-262">**Q. How many SSL certificates are supported?**</span></span>

<span data-ttu-id="19108-263">In too20 SSL som certifikat stöds.</span><span class="sxs-lookup"><span data-stu-id="19108-263">Up too20 SSL certificates are supported.</span></span>

<span data-ttu-id="19108-264">**FRÅGOR. Hur många certifikat för klientautentisering för backend-omkryptering stöds?**</span><span class="sxs-lookup"><span data-stu-id="19108-264">**Q. How many authentication certificates for backend re-encryption are supported?**</span></span>

<span data-ttu-id="19108-265">In too10 stöds certifikat för klientautentisering med en standard på 5.</span><span class="sxs-lookup"><span data-stu-id="19108-265">Up too10 authentication certificates are supported with a default of 5.</span></span>

<span data-ttu-id="19108-266">**FRÅGOR. Kan Programgateway integreras med Azure Key Vault internt?**</span><span class="sxs-lookup"><span data-stu-id="19108-266">**Q. Does Application Gateway integrate with Azure Key Vault natively?**</span></span>

<span data-ttu-id="19108-267">Nej, det inte är integrerat med Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="19108-267">No, it is not integrated with Azure Key Vault.</span></span>

## <a name="web-application-firewall-waf-configuration"></a><span data-ttu-id="19108-268">Konfiguration av webbprogram Firewall (Brandvägg)</span><span class="sxs-lookup"><span data-stu-id="19108-268">Web Application Firewall (WAF) Configuration</span></span>

<span data-ttu-id="19108-269">**FRÅGOR. Erbjuder hello Brandvägg SKU alla hello funktioner som är tillgängliga med hello Standard SKU?**</span><span class="sxs-lookup"><span data-stu-id="19108-269">**Q. Does hello WAF SKU offer all hello features available with hello Standard SKU?**</span></span>

<span data-ttu-id="19108-270">Ja, Brandvägg stöder alla hello-funktioner i hello Standard-SKU.</span><span class="sxs-lookup"><span data-stu-id="19108-270">Yes, WAF supports all hello features in hello Standard SKU.</span></span>

<span data-ttu-id="19108-271">**FRÅGOR. Vad är hello CR version Application Gateway har stöd för?**</span><span class="sxs-lookup"><span data-stu-id="19108-271">**Q. What is hello CRS version Application Gateway supports?**</span></span>

<span data-ttu-id="19108-272">Programgateway stöder CR [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) och CR [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span><span class="sxs-lookup"><span data-stu-id="19108-272">Application Gateway supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span></span>

<span data-ttu-id="19108-273">**FRÅGOR. Hur övervakar Brandvägg?**</span><span class="sxs-lookup"><span data-stu-id="19108-273">**Q. How do I monitor WAF?**</span></span>

<span data-ttu-id="19108-274">Brandvägg övervakas via diagnostikloggning, mer information om diagnostikloggning kan hittas på [diagnostik loggning och mått för Programgateway](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="19108-274">WAF is monitored through diagnostic logging, more information on diagnostic logging can be found at [Diagnostics Logging and Metrics for Application Gateway](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="19108-275">**FRÅGOR. Blockerar identifieringsläget trafik?**</span><span class="sxs-lookup"><span data-stu-id="19108-275">**Q. Does detection mode block traffic?**</span></span>

<span data-ttu-id="19108-276">Nej, loggar identifieringsläget endast trafik som utlöses av en Brandvägg regel.</span><span class="sxs-lookup"><span data-stu-id="19108-276">No, detection mode only logs traffic, which triggered a WAF rule.</span></span>

<span data-ttu-id="19108-277">**FRÅGOR. Hur anpassar Brandvägg regler?**</span><span class="sxs-lookup"><span data-stu-id="19108-277">**Q. How do I customize WAF rules?**</span></span>

<span data-ttu-id="19108-278">Ja, Brandvägg regler kan anpassas för mer information om hur toocustomize dem finns [anpassa Brandvägg regelgrupper och regler](application-gateway-customize-waf-rules-portal.md)</span><span class="sxs-lookup"><span data-stu-id="19108-278">Yes, WAF rules are customizable, for more information on how toocustomize them visit [Customize WAF rule groups and rules](application-gateway-customize-waf-rules-portal.md)</span></span>

<span data-ttu-id="19108-279">**FRÅGOR. Vilka regler som är tillgängliga?**</span><span class="sxs-lookup"><span data-stu-id="19108-279">**Q. What rules are currently available?**</span></span>

<span data-ttu-id="19108-280">Brandvägg stöder för närvarande CR [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) och [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), som ger grundläggande säkerhet mot de flesta av hello topp 10 sårbarheter som identifieras av hello öppna Web Application säkerhet projekt (OWASP) finns här [OWASP topp 10 säkerhetsrisker](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span><span class="sxs-lookup"><span data-stu-id="19108-280">WAF currently supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), which provide baseline security against most of hello top 10 vulnerabilities identified by hello Open Web Application Security Project (OWASP) found here [OWASP top 10 Vulnerabilities](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span></span>

* <span data-ttu-id="19108-281">Skydd mot SQL-inmatning</span><span class="sxs-lookup"><span data-stu-id="19108-281">SQL injection protection</span></span>

* <span data-ttu-id="19108-282">Skydd mot skriptkörning över flera webbplatser</span><span class="sxs-lookup"><span data-stu-id="19108-282">Cross site scripting protection</span></span>

* <span data-ttu-id="19108-283">Skydd mot vanliga webbattacker, som kommandoinmatning, dold HTTP-begäran, delning av HTTP-svar och attack genom införande av fjärrfil</span><span class="sxs-lookup"><span data-stu-id="19108-283">Common Web Attacks Protection such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attack</span></span>

* <span data-ttu-id="19108-284">Skydd mot åtgärder som inte följer HTTP-protokollet</span><span class="sxs-lookup"><span data-stu-id="19108-284">Protection against HTTP protocol violations</span></span>

* <span data-ttu-id="19108-285">Skydd mot avvikelser i HTTP-protokollet som att användaragent för värden och accept-huvud saknas</span><span class="sxs-lookup"><span data-stu-id="19108-285">Protection against HTTP protocol anomalies such as missing host user-agent and accept headers</span></span>

* <span data-ttu-id="19108-286">Skydd mot robotar, crawlers och skannrar</span><span class="sxs-lookup"><span data-stu-id="19108-286">Prevention against bots, crawlers, and scanners</span></span>

* <span data-ttu-id="19108-287">Identifiering av program vanliga felinställningar (det vill säga Apache, IIS osv.)</span><span class="sxs-lookup"><span data-stu-id="19108-287">Detection of common application misconfigurations (that is, Apache, IIS, etc.)</span></span>

<span data-ttu-id="19108-288">**FRÅGOR. Stöder Brandvägg också DDoS förebyggande?**</span><span class="sxs-lookup"><span data-stu-id="19108-288">**Q. Does WAF also support DDoS prevention?**</span></span>

<span data-ttu-id="19108-289">Nej, Brandvägg ger inte några DDoS förebyggande.</span><span class="sxs-lookup"><span data-stu-id="19108-289">No, WAF does not provide DDoS prevention.</span></span>

## <a name="diagnostics-and-logging"></a><span data-ttu-id="19108-290">Diagnostik- och loggning</span><span class="sxs-lookup"><span data-stu-id="19108-290">Diagnostics and Logging</span></span>

<span data-ttu-id="19108-291">**FRÅGOR. Vilka typer av loggar är tillgängliga med Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-291">**Q. What types of logs are available with Application Gateway?**</span></span>

<span data-ttu-id="19108-292">Det finns tre loggarna för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="19108-292">There are three logs available for Application Gateway.</span></span> <span data-ttu-id="19108-293">Mer information om dessa loggar och andra diagnostiska funktioner finns [Backend hälsa och diagnostik loggar mått för Programgateway](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="19108-293">For more information on these logs and other diagnostic capabilities, visit [Backend health, diagnostics logs, and metrics for Application Gateway](application-gateway-diagnostics.md).</span></span>

- <span data-ttu-id="19108-294">**ApplicationGatewayAccessLog** -hello åtkomst loggen innehåller varje förfrågan har skickats toohello Programgateway klientdel.</span><span class="sxs-lookup"><span data-stu-id="19108-294">**ApplicationGatewayAccessLog** - hello access log contains each request submitted toohello Application Gateway frontend.</span></span> <span data-ttu-id="19108-295">hello data omfattar hello anroparen IP begärd, URL svar svarstid, returkod, byte in och ut. Åtkomstlogg samlas in varje 300 sekunder.</span><span class="sxs-lookup"><span data-stu-id="19108-295">hello data includes hello caller's IP, URL requested, response latency, return code, bytes in and out. Access log is collected every 300 seconds.</span></span> <span data-ttu-id="19108-296">Den här loggfilen innehåller en post per instans av Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="19108-296">This log contains one record per instance of Application Gateway.</span></span>
- <span data-ttu-id="19108-297">**ApplicationGatewayPerformanceLog** -hello prestanda loggen innehåller prestandainformation om för per instanser inklusive totala begäran skickades, genomflöde i byte, totalt antal begäranden som betjänats misslyckade begäranden antal felfria och feltillstånd backend-instanser.</span><span class="sxs-lookup"><span data-stu-id="19108-297">**ApplicationGatewayPerformanceLog** - hello performance log captures performance information on per instance basis including total request served, throughput in bytes, total requests served, failed request count, healthy and unhealthy back-end instance count.</span></span>
- <span data-ttu-id="19108-298">**ApplicationGatewayFirewallLog** -hello brandväggsloggen innehåller begäranden som loggas via identifiering eller förhindra läget för en Programgateway som är konfigurerad med Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="19108-298">**ApplicationGatewayFirewallLog** - hello firewall log contains requests that are logged through either detection or prevention mode of an application gateway that is configured with web application firewall.</span></span>

<span data-ttu-id="19108-299">**FRÅGOR. Hur vet jag om min backend poolmedlemmar felfri?**</span><span class="sxs-lookup"><span data-stu-id="19108-299">**Q. How do I know if my backend pool members are healthy?**</span></span>

<span data-ttu-id="19108-300">Du kan använda PowerShell-cmdlet för hello `Get-AzureRmApplicationGatewayBackendHealth` eller verifiera hälsotillstånd hello-portalen genom att besöka [Gateway Programdiagnostik](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="19108-300">You can use hello PowerShell cmdlet `Get-AzureRmApplicationGatewayBackendHealth` or verify health through hello portal by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="19108-301">**FRÅGOR. Vad är hello bevarandeprincip på hello diagnostik loggar?**</span><span class="sxs-lookup"><span data-stu-id="19108-301">**Q. What is hello retention policy on hello diagnostics logs?**</span></span>

<span data-ttu-id="19108-302">Diagnostikloggar flödet toohello kunder storage-konto och kunder kan ange hello bevarandeprincip baserat på deras inställningar.</span><span class="sxs-lookup"><span data-stu-id="19108-302">Diagnostic logs flow toohello customers storage account and customers can set hello retention policy based on their preference.</span></span> <span data-ttu-id="19108-303">Diagnostikloggar kan även skickas tooan Event Hub eller logganalys.</span><span class="sxs-lookup"><span data-stu-id="19108-303">Diagnostic logs can also be sent tooan Event Hub or Log Analytics.</span></span> <span data-ttu-id="19108-304">Besök [Gateway Programdiagnostik](application-gateway-diagnostics.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="19108-304">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) for more details.</span></span>

<span data-ttu-id="19108-305">**FRÅGOR. Hur skaffar jag granskningsloggarna för Programgateway**</span><span class="sxs-lookup"><span data-stu-id="19108-305">**Q. How do I get audit logs for Application Gateway?**</span></span>

<span data-ttu-id="19108-306">Granskningsloggar är tillgängliga för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="19108-306">Audit logs are available for Application Gateway.</span></span> <span data-ttu-id="19108-307">I hello-portalen klickar du på **aktivitetsloggen** i hello menyn bladet för en Programgateway tooaccess hello granskningslogg.</span><span class="sxs-lookup"><span data-stu-id="19108-307">In hello portal, click **Activity Log** in hello menu blade of an Application Gateway tooaccess hello audit log.</span></span> 

<span data-ttu-id="19108-308">**FRÅGOR. Kan jag ställa in aviseringar med Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="19108-308">**Q. Can I set alerts with Application Gateway?**</span></span>

<span data-ttu-id="19108-309">Ja, Application Gateway har stöd för varningar, aviseringar konfigureras av mått.</span><span class="sxs-lookup"><span data-stu-id="19108-309">Yes, Application Gateway does support alerts, alerts are configured off metrics.</span></span>  <span data-ttu-id="19108-310">Application Gateway har för närvarande ”dataflöde”, vilket kan vara konfigurerade tooalert för mått.</span><span class="sxs-lookup"><span data-stu-id="19108-310">Application Gateway currently has a metric of "throughput", which can be configured tooalert.</span></span> <span data-ttu-id="19108-311">toolearn mer om varningar, besök [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="19108-311">toolearn more about alerts, visit [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="19108-312">**FRÅGOR. Backend-hälsa returnerar okänd status, vilka kan vara orsaken till denna status?**</span><span class="sxs-lookup"><span data-stu-id="19108-312">**Q. Backend health returns unknown status, what could be causing this status?**</span></span>

<span data-ttu-id="19108-313">hello beror oftast åtkomst toohello backend blockeras av en NSG eller anpassade DNS.</span><span class="sxs-lookup"><span data-stu-id="19108-313">hello most common reason is access toohello backend is being blocked by an NSG or custom DNS.</span></span> <span data-ttu-id="19108-314">Besök [Backend hälsa, diagnostikloggning och mått för Programgateway](application-gateway-diagnostics.md) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="19108-314">Visit [Backend health, diagnostics logging, and metrics for Application Gateway](application-gateway-diagnostics.md) toolearn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19108-315">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="19108-315">Next Steps</span></span>

<span data-ttu-id="19108-316">Mer om Programgateway finns toolearn [introduktion tooApplication Gateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="19108-316">toolearn more about Application Gateway visit [Introduction tooApplication Gateway](application-gateway-introduction.md).</span></span>
