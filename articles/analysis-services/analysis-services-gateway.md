---
title: aaaOn lokala datagateway | Microsoft Docs
description: "En lokal gateway är nödvändigt om Analysis Services-server i Azure ansluter tooon lokala datakällor."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: fc7b9c69e6f81b41deb7a5d6d963225593845d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-tooon-premises-data-sources-with-azure-on-premises-data-gateway"></a><span data-ttu-id="711f4-103">Ansluta tooon lokala datakällor med Azure-lokala Data Gateway</span><span class="sxs-lookup"><span data-stu-id="711f4-103">Connecting tooon-premises data sources with Azure On-premises Data Gateway</span></span>
<span data-ttu-id="711f4-104">hello lokala datagateway fungerar som en brygga som tillhandahåller säker dataöverföring mellan lokala datakällor och dina Azure Analysis Services-servrar i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="711f4-104">hello on-premises data gateway acts as a bridge, providing secure data transfer between on-premises data sources and your Azure Analysis Services servers in hello cloud.</span></span> <span data-ttu-id="711f4-105">I tillägg tooworking med flera Azure Analysis Services-servrar i hello fungerar även samma region, hello senaste versionen av hello gateway med Azure Logikappar, Power BI, Power appar och Microsoft Flow.</span><span class="sxs-lookup"><span data-stu-id="711f4-105">In addition tooworking with multiple Azure Analysis Services servers in hello same region, hello latest version of hello gateway also works with Azure Logic Apps, Power BI, Power Apps, and Microsoft Flow.</span></span> <span data-ttu-id="711f4-106">Du kan associera flera tjänster i hello samma region med en enda gateway.</span><span class="sxs-lookup"><span data-stu-id="711f4-106">You can associate multiple services in hello same region with a single gateway.</span></span> 

 <span data-ttu-id="711f4-107">Azure Analysis Services kräver en gateway-resurs i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="711f4-107">Azure Analysis Services requires a gateway resource in hello same region.</span></span> <span data-ttu-id="711f4-108">Om du har Azure Analysis Services-servrar i hello östra USA 2 region måste till exempel en gateway-resurs i hello östra USA 2 region.</span><span class="sxs-lookup"><span data-stu-id="711f4-108">For example, if you have Azure Analysis Services servers in hello East US 2 region, you need a gateway resource in hello East US 2 region.</span></span> <span data-ttu-id="711f4-109">Flera servrar i östra USA 2 kan använda hello samma gateway.</span><span class="sxs-lookup"><span data-stu-id="711f4-109">Multiple servers in East US 2 can use hello same gateway.</span></span>

<span data-ttu-id="711f4-110">Hämtar installationsprogrammet med hello gateway hello första gången är en process i fyra delar:</span><span class="sxs-lookup"><span data-stu-id="711f4-110">Getting setup with hello gateway hello first time is a four-part process:</span></span>

- <span data-ttu-id="711f4-111">**Hämta och kör installationsprogrammet** -det här steget installerar gateway-tjänsten på en dator i din organisation.</span><span class="sxs-lookup"><span data-stu-id="711f4-111">**Download and run setup** - This step installs a gateway service on a computer in your organization.</span></span>

- <span data-ttu-id="711f4-112">**Registrera din gateway** – i det här steget kan du ange ett namn och återställning för din gateway och välj en region som registrerar din gateway med hello Gateway-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="711f4-112">**Register your gateway** - In this step, you specify a name and recovery key for your gateway, and select a region, registering your gateway with hello Gateway Cloud Service.</span></span>

- <span data-ttu-id="711f4-113">**Skapa en gateway-resurs i Azure** – i det här steget skapar du en gateway-resurs i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="711f4-113">**Create a gateway resource in Azure** - In this step, you create a gateway resource in your Azure subscription.</span></span>

- <span data-ttu-id="711f4-114">**Anslut dina servrar tooyour gatewayresursen** -när du har en gateway-resurs i din prenumeration kan du börja ansluta dina servrar tooit.</span><span class="sxs-lookup"><span data-stu-id="711f4-114">**Connect your servers tooyour gateway resource** - Once you have a gateway resource in your subscription, you can begin connecting your servers tooit.</span></span>

<span data-ttu-id="711f4-115">När du har en gateway-resurs som har konfigurerats för din prenumeration kan ansluta du flera servrar och andra tjänster tooit.</span><span class="sxs-lookup"><span data-stu-id="711f4-115">Once you have a gateway resource configured for your subscription, you can connect multiple servers, and other services tooit.</span></span> <span data-ttu-id="711f4-116">Du bara behöver tooinstall en annan gateway och skapa resurser för ytterligare en gateway om det finns servrar eller andra tjänster i en annan region.</span><span class="sxs-lookup"><span data-stu-id="711f4-116">You only need tooinstall a different gateway and create additional gateway resources if you have servers or other services in a different region.</span></span>

<span data-ttu-id="711f4-117">tooget igång nu direkt se [installera och konfigurera lokala datagateway](analysis-services-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="711f4-117">tooget started right away, see [Install and configure on-premises data gateway](analysis-services-gateway-install.md).</span></span>

## <span data-ttu-id="711f4-118"><a name="how-it-works"></a>Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="711f4-118"><a name="how-it-works"> </a>How it works</span></span>
<span data-ttu-id="711f4-119">hello-gateway som du installerar på en dator i din organisation körs som en windowstjänst **lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="711f4-119">hello gateway you install on a computer in your organization runs as a Windows service, **On-premises data gateway**.</span></span> <span data-ttu-id="711f4-120">Den här lokal tjänst har registrerats med hello Gateway Cloud Service via Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="711f4-120">This local service is registered with hello Gateway Cloud Service through Azure Service Bus.</span></span> <span data-ttu-id="711f4-121">Du kan sedan skapa en gateway-resurs Gateway Cloud Service för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="711f4-121">You then create a gateway resource Gateway Cloud Service for your Azure subscription.</span></span> <span data-ttu-id="711f4-122">Din Azure Analysis Services servrar är sedan ansluten tooyour gatewayresursen.</span><span class="sxs-lookup"><span data-stu-id="711f4-122">Your Azure Analysis Services servers are then connected tooyour gateway resource.</span></span> <span data-ttu-id="711f4-123">När modeller på din server behöver tooconnect tooyour lokala datakällor för frågor eller bearbetning, hello lokala lokala data gateway-tjänsten och dina datakällor i en fråga och data flödet går hello gateway resurs, Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="711f4-123">When models on your server need tooconnect tooyour on-premises data sources for queries or processing, a query and data flow traverses hello gateway resource, Azure Service Bus, hello local on-premises data gateway service, and your data sources.</span></span> 

![Hur det fungerar](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

<span data-ttu-id="711f4-125">Frågor och dataflöde:</span><span class="sxs-lookup"><span data-stu-id="711f4-125">Queries and data flow:</span></span>

1. <span data-ttu-id="711f4-126">En fråga skapas av hello molntjänst med hello krypterade autentiseringsuppgifter för datakälla för hello lokalt.</span><span class="sxs-lookup"><span data-stu-id="711f4-126">A query is created by hello cloud service with hello encrypted credentials for hello on-premises data source.</span></span> <span data-ttu-id="711f4-127">Har skickas sedan tooa kön för hello gateway tooprocess.</span><span class="sxs-lookup"><span data-stu-id="711f4-127">It's then sent tooa queue for hello gateway tooprocess.</span></span>
2. <span data-ttu-id="711f4-128">hello gateway-Molntjänsten analyserar hello frågan och skickar hello begäran toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="711f4-128">hello gateway cloud service analyzes hello query and pushes hello request toohello [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span></span>
3. <span data-ttu-id="711f4-129">hello lokala datagateway avsöker hello Azure Service Bus för väntande begäranden.</span><span class="sxs-lookup"><span data-stu-id="711f4-129">hello on-premises data gateway polls hello Azure Service Bus for pending requests.</span></span>
4. <span data-ttu-id="711f4-130">hello gateway hämtar hello frågan dekrypterar hello autentiseringsuppgifter och ansluter toohello datakällor med inloggningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="711f4-130">hello gateway gets hello query, decrypts hello credentials, and connects toohello data sources with those credentials.</span></span>
5. <span data-ttu-id="711f4-131">hello gateway skickar hello toohello frågedatakällan för körning.</span><span class="sxs-lookup"><span data-stu-id="711f4-131">hello gateway sends hello query toohello data source for execution.</span></span>
6. <span data-ttu-id="711f4-132">hello resultat skickas från hello datakälla, bakre toohello gateway, och sedan på hello Molntjänsten och servern.</span><span class="sxs-lookup"><span data-stu-id="711f4-132">hello results are sent from hello data source, back toohello gateway, and then onto hello cloud service and your server.</span></span>

## <span data-ttu-id="711f4-133"><a name="windows-service-account"></a>För Windows-tjänstkontot</span><span class="sxs-lookup"><span data-stu-id="711f4-133"><a name="windows-service-account"> </a>Windows Service account</span></span>
<span data-ttu-id="711f4-134">hello lokala datagateway är konfigurerade toouse *NT SERVICE\PBIEgwService* för hello Windows service inloggning autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="711f4-134">hello on-premises data gateway is configured toouse *NT SERVICE\PBIEgwService* for hello Windows service logon credential.</span></span> <span data-ttu-id="711f4-135">Som standard har hello till höger i inloggning som en tjänst. hello gäller hello-dator som du installerar hello gateway på.</span><span class="sxs-lookup"><span data-stu-id="711f4-135">By default, it has hello right of Logon as a service; in hello context of hello machine that you are installing hello gateway on.</span></span> <span data-ttu-id="711f4-136">Den här autentiseringsuppgiften hello är inte samma konto som används för tooconnect tooon lokala datakällor eller ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="711f4-136">This credential is not hello same account used tooconnect tooon-premises data sources or your Azure account.</span></span>  

<span data-ttu-id="711f4-137">Om du får problem med proxyservern på grund av tooauthentication, vill du kanske toochange hello Windows tjänst konto tooa domänanvändare eller hanteras tjänstkonto.</span><span class="sxs-lookup"><span data-stu-id="711f4-137">If you encounter issues with your proxy server due tooauthentication, you may want toochange hello Windows service account tooa domain user or managed service account.</span></span>

## <span data-ttu-id="711f4-138"><a name="ports"></a>Portar</span><span class="sxs-lookup"><span data-stu-id="711f4-138"><a name="ports"> </a>Ports</span></span>
<span data-ttu-id="711f4-139">hello gateway skapar en utgående anslutning tooAzure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="711f4-139">hello gateway creates an outbound connection tooAzure Service Bus.</span></span> <span data-ttu-id="711f4-140">Den kommunicerar på utgående portar: TCP 443 (standard), 5671, 5672, 9350 via 9354.</span><span class="sxs-lookup"><span data-stu-id="711f4-140">It communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span>  <span data-ttu-id="711f4-141">hello gateway kräver inte ingående portar.</span><span class="sxs-lookup"><span data-stu-id="711f4-141">hello gateway does not require inbound ports.</span></span>

<span data-ttu-id="711f4-142">Vi rekommenderar att du godkända hello IP-adresser för dina dataområdet i brandväggen.</span><span class="sxs-lookup"><span data-stu-id="711f4-142">We recommend you whitelist hello IP addresses for your data region in your firewall.</span></span> <span data-ttu-id="711f4-143">Du kan hämta hello [Microsoft Azure-Datacenter IP-lista](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="711f4-143">You can download hello [Microsoft Azure Datacenter IP list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="711f4-144">Den här listan uppdateras varje vecka.</span><span class="sxs-lookup"><span data-stu-id="711f4-144">This list is updated weekly.</span></span>

> [!NOTE]
> <span data-ttu-id="711f4-145">hello IP-adresser som anges i hello Azure Datacenter IP-adresser finns i CIDR-notering.</span><span class="sxs-lookup"><span data-stu-id="711f4-145">hello IP Addresses listed in hello Azure Datacenter IP list are in CIDR notation.</span></span> <span data-ttu-id="711f4-146">Till exempel innebär inte 10.0.0.0/24 10.0.0.0 via 10.0.0.24.</span><span class="sxs-lookup"><span data-stu-id="711f4-146">For example, 10.0.0.0/24 does not mean 10.0.0.0 through 10.0.0.24.</span></span> <span data-ttu-id="711f4-147">Mer information om hello [CIDR-notering](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="711f4-147">Learn more about hello [CIDR notation](http://whatismyipaddress.com/cidr).</span></span>
>
>

<span data-ttu-id="711f4-148">hello följande är hello fullständigt kvalificerade domännamn som används av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="711f4-148">hello following are hello fully qualified domain names used by hello gateway.</span></span>

| <span data-ttu-id="711f4-149">Domännamn</span><span class="sxs-lookup"><span data-stu-id="711f4-149">Domain names</span></span> | <span data-ttu-id="711f4-150">Utgående portar</span><span class="sxs-lookup"><span data-stu-id="711f4-150">Outbound ports</span></span> | <span data-ttu-id="711f4-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="711f4-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="711f4-152">*. powerbi.com</span><span class="sxs-lookup"><span data-stu-id="711f4-152">*.powerbi.com</span></span> |<span data-ttu-id="711f4-153">80</span><span class="sxs-lookup"><span data-stu-id="711f4-153">80</span></span> |<span data-ttu-id="711f4-154">HTTP används toodownload hello installer.</span><span class="sxs-lookup"><span data-stu-id="711f4-154">HTTP used toodownload hello installer.</span></span> |
| <span data-ttu-id="711f4-155">*. powerbi.com</span><span class="sxs-lookup"><span data-stu-id="711f4-155">*.powerbi.com</span></span> |<span data-ttu-id="711f4-156">443</span><span class="sxs-lookup"><span data-stu-id="711f4-156">443</span></span> |<span data-ttu-id="711f4-157">HTTPS</span><span class="sxs-lookup"><span data-stu-id="711f4-157">HTTPS</span></span> |
| <span data-ttu-id="711f4-158">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="711f4-158">*.analysis.windows.net</span></span> |<span data-ttu-id="711f4-159">443</span><span class="sxs-lookup"><span data-stu-id="711f4-159">443</span></span> |<span data-ttu-id="711f4-160">HTTPS</span><span class="sxs-lookup"><span data-stu-id="711f4-160">HTTPS</span></span> |
| <span data-ttu-id="711f4-161">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="711f4-161">*.login.windows.net</span></span> |<span data-ttu-id="711f4-162">443</span><span class="sxs-lookup"><span data-stu-id="711f4-162">443</span></span> |<span data-ttu-id="711f4-163">HTTPS</span><span class="sxs-lookup"><span data-stu-id="711f4-163">HTTPS</span></span> |
| <span data-ttu-id="711f4-164">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="711f4-164">*.servicebus.windows.net</span></span> |<span data-ttu-id="711f4-165">5671-5672</span><span class="sxs-lookup"><span data-stu-id="711f4-165">5671-5672</span></span> |<span data-ttu-id="711f4-166">Avancerade Message Queuing-protokollet (AMQP)</span><span class="sxs-lookup"><span data-stu-id="711f4-166">Advanced Message Queuing Protocol (AMQP)</span></span> |
| <span data-ttu-id="711f4-167">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="711f4-167">*.servicebus.windows.net</span></span> |<span data-ttu-id="711f4-168">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="711f4-168">443, 9350-9354</span></span> |<span data-ttu-id="711f4-169">Lyssnare på Service Bus Relay via TCP (kräver 443 för åtkomstkontroll token)</span><span class="sxs-lookup"><span data-stu-id="711f4-169">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> |
| <span data-ttu-id="711f4-170">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="711f4-170">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="711f4-171">443</span><span class="sxs-lookup"><span data-stu-id="711f4-171">443</span></span> |<span data-ttu-id="711f4-172">HTTPS</span><span class="sxs-lookup"><span data-stu-id="711f4-172">HTTPS</span></span> |
| <span data-ttu-id="711f4-173">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="711f4-173">*.core.windows.net</span></span> |<span data-ttu-id="711f4-174">443</span><span class="sxs-lookup"><span data-stu-id="711f4-174">443</span></span> |<span data-ttu-id="711f4-175">HTTPS</span><span class="sxs-lookup"><span data-stu-id="711f4-175">HTTPS</span></span> |
| <span data-ttu-id="711f4-176">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="711f4-176">login.microsoftonline.com</span></span> |<span data-ttu-id="711f4-177">443</span><span class="sxs-lookup"><span data-stu-id="711f4-177">443</span></span> |<span data-ttu-id="711f4-178">HTTPS</span><span class="sxs-lookup"><span data-stu-id="711f4-178">HTTPS</span></span> |
| <span data-ttu-id="711f4-179">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="711f4-179">*.msftncsi.com</span></span> |<span data-ttu-id="711f4-180">443</span><span class="sxs-lookup"><span data-stu-id="711f4-180">443</span></span> |<span data-ttu-id="711f4-181">Använda tootest Internetanslutning om hello gateway inte kan nås av hello Power BI-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="711f4-181">Used tootest internet connectivity if hello gateway is unreachable by hello Power BI service.</span></span> |
| <span data-ttu-id="711f4-182">*.microsoftonline p.com</span><span class="sxs-lookup"><span data-stu-id="711f4-182">*.microsoftonline-p.com</span></span> |<span data-ttu-id="711f4-183">443</span><span class="sxs-lookup"><span data-stu-id="711f4-183">443</span></span> |<span data-ttu-id="711f4-184">Används för autentisering beroende på konfiguration.</span><span class="sxs-lookup"><span data-stu-id="711f4-184">Used for authentication depending on configuration.</span></span> |

### <span data-ttu-id="711f4-185"><a name="force-https"></a>Tvinga HTTPS-kommunikation med Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="711f4-185"><a name="force-https"></a>Forcing HTTPS communication with Azure Service Bus</span></span>
<span data-ttu-id="711f4-186">Du kan tvinga hello gateway toocommunicate med Azure Service Bus med hjälp av HTTPS i stället för direkt TCP; men kan detta avsevärt minska prestanda.</span><span class="sxs-lookup"><span data-stu-id="711f4-186">You can force hello gateway toocommunicate with Azure Service Bus by using HTTPS instead of direct TCP; however, doing so can greatly reduce performance.</span></span> <span data-ttu-id="711f4-187">Du kan ändra hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* filen genom att ändra hello-värde från `AutoDetect` för`Https`.</span><span class="sxs-lookup"><span data-stu-id="711f4-187">You can modify hello *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* file by changing hello value from `AutoDetect` too`Https`.</span></span> <span data-ttu-id="711f4-188">Den här filen finns vanligtvis i *C:\Program Files\On lokala datagateway*.</span><span class="sxs-lookup"><span data-stu-id="711f4-188">This file is typically located at *C:\Program Files\On-premises data gateway*.</span></span>

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <span data-ttu-id="711f4-189"><a name="faq"></a>Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="711f4-189"><a name="faq"></a>Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="711f4-190">Allmänt</span><span class="sxs-lookup"><span data-stu-id="711f4-190">General</span></span>

<span data-ttu-id="711f4-191">**Q**: behöver jag en gateway för datakällor i hello molnet, till exempel Azure SQL Database?</span><span class="sxs-lookup"><span data-stu-id="711f4-191">**Q**: Do I need a gateway for data sources in hello cloud, such as Azure SQL Database?</span></span> <br/><span data-ttu-id="711f4-192">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="711f4-192">
**A**: No.</span></span> <span data-ttu-id="711f4-193">En gateway ansluter bara tooon lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="711f4-193">A gateway connects tooon-premises data sources only.</span></span>

<span data-ttu-id="711f4-194">**Q**: har hello gateway toobe installerad på detsamma datorn som datakälla för hello hello?</span><span class="sxs-lookup"><span data-stu-id="711f4-194">**Q**: Does hello gateway have toobe installed on hello same machine as hello data source?</span></span> <br/><span data-ttu-id="711f4-195">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="711f4-195">
**A**: No.</span></span> <span data-ttu-id="711f4-196">hello gateway ansluter toohello datakälla med hjälp av hello anslutningsinformationen som angavs.</span><span class="sxs-lookup"><span data-stu-id="711f4-196">hello gateway connects toohello data source using hello connection information that was provided.</span></span> <span data-ttu-id="711f4-197">Överväg att hello gateway som ett klientprogram på detta sätt.</span><span class="sxs-lookup"><span data-stu-id="711f4-197">Consider hello gateway as a client application in this sense.</span></span> <span data-ttu-id="711f4-198">hello gateway behövs bara hello kapaciteten tooconnect toohello namn på servern som har angetts, vanligtvis på hello samma nätverk.</span><span class="sxs-lookup"><span data-stu-id="711f4-198">hello gateway just needs hello capability tooconnect toohello server name that was provided, typically on hello same network.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="711f4-199">**Q**: Varför jag behöver toouse ett arbets eller skolkonto konto toosign i?</span><span class="sxs-lookup"><span data-stu-id="711f4-199">**Q**: Why do I need toouse a work or school account toosign in?</span></span> <br/><span data-ttu-id="711f4-200">
**En**: du kan bara använda en Azure arbets- eller skolkonto när du installerar hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="711f4-200">
**A**: You can only use an Azure work or school account when you install hello on-premises data gateway.</span></span> <span data-ttu-id="711f4-201">Din inloggning konto lagras i en klient som hanteras av Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="711f4-201">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="711f4-202">Azure AD-kontot användarens huvudnamn (UPN) matchar vanligtvis hello e-postadress.</span><span class="sxs-lookup"><span data-stu-id="711f4-202">Usually, your Azure AD account's user principal name (UPN) matches hello email address.</span></span>

<span data-ttu-id="711f4-203">**Q**: var lagras mina autentiseringsuppgifter?</span><span class="sxs-lookup"><span data-stu-id="711f4-203">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="711f4-204">
**En**: hello-autentiseringsuppgifter som du anger för en datakälla krypteras och lagras i hello Gateway-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="711f4-204">
**A**: hello credentials that you enter for a data source are encrypted and stored in hello Gateway Cloud Service.</span></span> <span data-ttu-id="711f4-205">hello autentiseringsuppgifter dekrypteras på hello lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="711f4-205">hello credentials are decrypted at hello on-premises data gateway.</span></span>

<span data-ttu-id="711f4-206">**Q**: finns det några krav för nätverksbandbredd?</span><span class="sxs-lookup"><span data-stu-id="711f4-206">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="711f4-207">
**En**: det har rekommenderar nätverket anslutningen har bra genomströmning.</span><span class="sxs-lookup"><span data-stu-id="711f4-207">
**A**: It's recommend your network connection has good throughput.</span></span> <span data-ttu-id="711f4-208">Alla miljöer skiljer sig, och hello mängden data som skickas påverkar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="711f4-208">Every environment is different, and hello amount of data being sent affects hello results.</span></span> <span data-ttu-id="711f4-209">Med hjälp av ExpressRoute kan hjälpa tooguarantee en nivå av dataflödet mellan lokala och hello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="711f4-209">Using ExpressRoute could help tooguarantee a level of throughput between on-premises and hello Azure datacenters.</span></span>
<span data-ttu-id="711f4-210">Du kan använda hello verktyg från tredje part hastighet testning i Azure app toohelp mätaren din genomflöde.</span><span class="sxs-lookup"><span data-stu-id="711f4-210">You can use hello third-party tool Azure Speed Test app toohelp gauge your throughput.</span></span>

<span data-ttu-id="711f4-211">**Q**: Vad är hello svarstid för att köra frågor tooa datakällan från hello gateway?</span><span class="sxs-lookup"><span data-stu-id="711f4-211">**Q**: What is hello latency for running queries tooa data source from hello gateway?</span></span> <span data-ttu-id="711f4-212">Vad är bästa hello-arkitektur?</span><span class="sxs-lookup"><span data-stu-id="711f4-212">What is hello best architecture?</span></span> <br/><span data-ttu-id="711f4-213">
**En**: tooreduce Nätverksfördröjningen, installera hello gateway som Stäng toohello datakälla som möjligt.</span><span class="sxs-lookup"><span data-stu-id="711f4-213">
**A**: tooreduce network latency, install hello gateway as close toohello data source as possible.</span></span> <span data-ttu-id="711f4-214">Om du kan installera hello gateway på hello faktiska datakällan, minimerar den här närhet hello latens införs.</span><span class="sxs-lookup"><span data-stu-id="711f4-214">If you can install hello gateway on hello actual data source, this proximity minimizes hello latency introduced.</span></span> <span data-ttu-id="711f4-215">Överväg att hello Datacenter för.</span><span class="sxs-lookup"><span data-stu-id="711f4-215">Consider hello datacenters too.</span></span> <span data-ttu-id="711f4-216">Till exempel om din tjänst använder hello västra USA datacenter, och du har SQL Server finns i en Azure VM, ska Azure VM vara i hello västra USA för.</span><span class="sxs-lookup"><span data-stu-id="711f4-216">For example, if your service uses hello West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in hello West US too.</span></span> <span data-ttu-id="711f4-217">Den här närhet minimerar fördröjning och undviker utgång avgifter för hello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="711f4-217">This proximity minimizes latency and avoids egress charges on hello Azure VM.</span></span>

<span data-ttu-id="711f4-218">**Q**: hur är skickade tillbaka toohello molnet?</span><span class="sxs-lookup"><span data-stu-id="711f4-218">**Q**: How are results sent back toohello cloud?</span></span> <br/><span data-ttu-id="711f4-219">
**En**: resultat skickas via hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="711f4-219">
**A**: Results are sent through hello Azure Service Bus.</span></span>

<span data-ttu-id="711f4-220">**Q**: finns det några inkommande anslutningar toohello gateway från hello molnet?</span><span class="sxs-lookup"><span data-stu-id="711f4-220">**Q**: Are there any inbound connections toohello gateway from hello cloud?</span></span> <br/><span data-ttu-id="711f4-221">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="711f4-221">
**A**: No.</span></span> <span data-ttu-id="711f4-222">hello gateway använder utgående anslutningar tooAzure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="711f4-222">hello gateway uses outbound connections tooAzure Service Bus.</span></span>

<span data-ttu-id="711f4-223">**Q**: Vad händer om jag blockera utgående anslutningar?</span><span class="sxs-lookup"><span data-stu-id="711f4-223">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="711f4-224">Vad gör jag behöver tooopen?</span><span class="sxs-lookup"><span data-stu-id="711f4-224">What do I need tooopen?</span></span> <br/><span data-ttu-id="711f4-225">
**En**: Se hello portar och värdar som hello gateway används.</span><span class="sxs-lookup"><span data-stu-id="711f4-225">
**A**: See hello ports and hosts that hello gateway uses.</span></span>

<span data-ttu-id="711f4-226">**Q**: det faktiska Windows hello-tjänsten som kallas?</span><span class="sxs-lookup"><span data-stu-id="711f4-226">**Q**: What is hello actual Windows service called?</span></span><br/><span data-ttu-id="711f4-227">
**En**: I tjänster, hello gateway kallas lokala data gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="711f4-227">
**A**: In Services, hello gateway is called On-premises data gateway service.</span></span>

<span data-ttu-id="711f4-228">**Q**: kan hello gateway Windows-tjänsten körs med ett Azure Active Directory-konto?</span><span class="sxs-lookup"><span data-stu-id="711f4-228">**Q**: Can hello gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="711f4-229">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="711f4-229">
**A**: No.</span></span> <span data-ttu-id="711f4-230">hello Windows-tjänst måste ha ett giltigt Windows-konto.</span><span class="sxs-lookup"><span data-stu-id="711f4-230">hello Windows service must have a valid Windows account.</span></span> <span data-ttu-id="711f4-231">Som standard körs hello-tjänsten med hello tjänst-SID NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="711f4-231">By default, hello service runs with hello Service SID, NT SERVICE\PBIEgwService.</span></span>

### <span data-ttu-id="711f4-232"><a name="high-availability"></a>Hög tillgänglighet och disaster recovery</span><span class="sxs-lookup"><span data-stu-id="711f4-232"><a name="high-availability"></a>High availability and disaster recovery</span></span>

<span data-ttu-id="711f4-233">**Q**: vilka alternativ som är tillgängliga för katastrofåterställning?</span><span class="sxs-lookup"><span data-stu-id="711f4-233">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="711f4-234">
**En**: du kan använda hello recovery viktiga toorestore eller flytta en gateway.</span><span class="sxs-lookup"><span data-stu-id="711f4-234">
**A**: You can use hello recovery key toorestore or move a gateway.</span></span> <span data-ttu-id="711f4-235">När du installerar hello gateway, ange hello återställningsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="711f4-235">When you install hello gateway, specify hello recovery key.</span></span>

<span data-ttu-id="711f4-236">**Q**: Vad är hello fördelen hello återställningsnyckeln?</span><span class="sxs-lookup"><span data-stu-id="711f4-236">**Q**: What is hello benefit of hello recovery key?</span></span> <br/><span data-ttu-id="711f4-237">
**En**: hello återställningsnyckeln ger ett sätt toomigrate eller återställa gatewayinställningarna efter en katastrof.</span><span class="sxs-lookup"><span data-stu-id="711f4-237">
**A**: hello recovery key provides a way toomigrate or recover your gateway settings after a disaster.</span></span>

## <span data-ttu-id="711f4-238"><a name="troubleshooting"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="711f4-238"><a name="troubleshooting"> </a>Troubleshooting</span></span>

<span data-ttu-id="711f4-239">**Q**: hur kan jag se vilka frågor skickas toohello lokala datakällan?</span><span class="sxs-lookup"><span data-stu-id="711f4-239">**Q**: How can I see what queries are being sent toohello on-premises data source?</span></span> <br/><span data-ttu-id="711f4-240">
**En**: du kan aktivera frågespårning, vilket innefattar hello frågor som skickas.</span><span class="sxs-lookup"><span data-stu-id="711f4-240">
**A**: You can enable query tracing, which includes hello queries that are sent.</span></span> <span data-ttu-id="711f4-241">Kom ihåg toochange frågan spårning tillbaka toohello ursprungliga värdet när du är klar felsökning.</span><span class="sxs-lookup"><span data-stu-id="711f4-241">Remember toochange query tracing back toohello original value when done troubleshooting.</span></span> <span data-ttu-id="711f4-242">Lämna frågespårning aktiverad skapar större loggar.</span><span class="sxs-lookup"><span data-stu-id="711f4-242">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="711f4-243">Du kan också titta på Verktyg som datakällan har för spårning frågor.</span><span class="sxs-lookup"><span data-stu-id="711f4-243">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="711f4-244">Du kan till exempel använda Extended Events eller SQL Profiler för SQL Server och Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="711f4-244">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="711f4-245">**Q**: där är hello gateway-loggarna?</span><span class="sxs-lookup"><span data-stu-id="711f4-245">**Q**: Where are hello gateway logs?</span></span> <br/><span data-ttu-id="711f4-246">
**En**: Se loggarna senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="711f4-246">
**A**: See Logs later in this topic.</span></span>

### <span data-ttu-id="711f4-247"><a name="update"></a>Uppdatera toohello senaste versionen</span><span class="sxs-lookup"><span data-stu-id="711f4-247"><a name="update"></a>Update toohello latest version</span></span>

<span data-ttu-id="711f4-248">Många problem kan ansluta när hello gatewayversionen blir inaktuella.</span><span class="sxs-lookup"><span data-stu-id="711f4-248">Many issues can surface when hello gateway version becomes outdated.</span></span> <span data-ttu-id="711f4-249">Kontrollera att du använder hello senaste versionen som allmän bra.</span><span class="sxs-lookup"><span data-stu-id="711f4-249">As good general practice, make sure that you use hello latest version.</span></span> <span data-ttu-id="711f4-250">Om du inte har uppdaterat hello gateway för en månad eller längre, kan du Överväg att installera hello senaste versionen av hello gateway och se om du kan återskapa hello problemet.</span><span class="sxs-lookup"><span data-stu-id="711f4-250">If you haven't updated hello gateway for a month or longer, you might consider installing hello latest version of hello gateway, and see if you can reproduce hello issue.</span></span>

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="711f4-251">Fel: Det gick inte tooadd användaren toogroup.</span><span class="sxs-lookup"><span data-stu-id="711f4-251">Error: Failed tooadd user toogroup.</span></span> <span data-ttu-id="711f4-252">(-2147463168 PBIEgwService användare)</span><span class="sxs-lookup"><span data-stu-id="711f4-252">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="711f4-253">Du kan få detta fel om du försöker tooinstall hello gateway på en domänkontrollant, vilket inte stöds.</span><span class="sxs-lookup"><span data-stu-id="711f4-253">You might get this error if you try tooinstall hello gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="711f4-254">Se till att du distribuerar hello gateway på en dator som inte är en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="711f4-254">Make sure that you deploy hello gateway on a machine that isn't a domain controller.</span></span>

## <span data-ttu-id="711f4-255"><a name="logs"></a>Loggar</span><span class="sxs-lookup"><span data-stu-id="711f4-255"><a name="logs"></a>Logs</span></span>

<span data-ttu-id="711f4-256">Loggfiler är en viktig resurs när du felsöker.</span><span class="sxs-lookup"><span data-stu-id="711f4-256">Log files are an important resource when troubleshooting.</span></span>

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="711f4-257">Enterprise gateway tjänstloggar</span><span class="sxs-lookup"><span data-stu-id="711f4-257">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a><span data-ttu-id="711f4-258">Av konfigurationsloggar</span><span class="sxs-lookup"><span data-stu-id="711f4-258">Configuration logs</span></span>

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a><span data-ttu-id="711f4-259">Händelseloggar</span><span class="sxs-lookup"><span data-stu-id="711f4-259">Event logs</span></span>

<span data-ttu-id="711f4-260">Du kan hitta hello loggar Data Management Gateway och PowerBIGateway under **program- och tjänstloggar**.</span><span class="sxs-lookup"><span data-stu-id="711f4-260">You can find hello Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>


## <span data-ttu-id="711f4-261"><a name="telemetry"></a>Telemetri</span><span class="sxs-lookup"><span data-stu-id="711f4-261"><a name="telemetry"></a>Telemetry</span></span>
<span data-ttu-id="711f4-262">Telemetri kan användas för övervakning och felsökning.</span><span class="sxs-lookup"><span data-stu-id="711f4-262">Telemetry can be used for monitoring and troubleshooting.</span></span> <span data-ttu-id="711f4-263">Som standard</span><span class="sxs-lookup"><span data-stu-id="711f4-263">By default</span></span>

<span data-ttu-id="711f4-264">**tooturn telemetri**</span><span class="sxs-lookup"><span data-stu-id="711f4-264">**tooturn on telemetry**</span></span>

1.  <span data-ttu-id="711f4-265">Kontrollera hello lokala gateway-klienten datakatalog på hello dator.</span><span class="sxs-lookup"><span data-stu-id="711f4-265">Check hello On-premises data gateway client directory on hello computer.</span></span> <span data-ttu-id="711f4-266">Vanligtvis är det **%systemdrive%\Program Files\On lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="711f4-266">Typically, it is **%systemdrive%\Program Files\On-premises data gateway**.</span></span> <span data-ttu-id="711f4-267">Du kan öppna tjänstekonsolen och kontrollera hello sökvägen tooexecutable: en egenskap för hello lokala data gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="711f4-267">Or, you can open a Services console and check hello Path tooexecutable: A property of hello On-premises data gateway service.</span></span>
2.  <span data-ttu-id="711f4-268">I hello Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config filen från katalogen för klienten.</span><span class="sxs-lookup"><span data-stu-id="711f4-268">In hello Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config file from client directory.</span></span> <span data-ttu-id="711f4-269">Ändra hello SendTelemetry inställningen tootrue.</span><span class="sxs-lookup"><span data-stu-id="711f4-269">Change hello SendTelemetry setting tootrue.</span></span>
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  <span data-ttu-id="711f4-270">Spara dina ändringar och starta om tjänsten för Windows hello: lokala data gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="711f4-270">Save your changes and restart hello Windows service: On-premises data gateway service.</span></span>




## <a name="next-steps"></a><span data-ttu-id="711f4-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="711f4-271">Next steps</span></span>
* [<span data-ttu-id="711f4-272">Hantera Analysis Services</span><span class="sxs-lookup"><span data-stu-id="711f4-272">Manage Analysis Services</span></span>](analysis-services-manage.md)
* [<span data-ttu-id="711f4-273">Hämta data från Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="711f4-273">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)
