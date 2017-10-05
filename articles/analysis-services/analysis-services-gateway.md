---
title: Lokala datagateway | Microsoft Docs
description: "En lokal gateway är nödvändigt om Analysis Services-server i Azure ansluter till lokala datakällor."
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
ms.openlocfilehash: 514b5404e8cbfa0baa657eb41736e20cad502638
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a><span data-ttu-id="ac641-103">Ansluter till lokala datakällor med Azure-lokala Data Gateway</span><span class="sxs-lookup"><span data-stu-id="ac641-103">Connecting to on-premises data sources with Azure On-premises Data Gateway</span></span>
<span data-ttu-id="ac641-104">Lokala datagateway fungerar som en brygga som tillhandahåller säker dataöverföring mellan lokala datakällor och dina Azure Analysis Services-servrar i molnet.</span><span class="sxs-lookup"><span data-stu-id="ac641-104">The on-premises data gateway acts as a bridge, providing secure data transfer between on-premises data sources and your Azure Analysis Services servers in the cloud.</span></span> <span data-ttu-id="ac641-105">Förutom att arbeta med flera Azure Analysis Services-servrar i samma region fungerar även den senaste versionen av gatewayen med Azure Logikappar, Power BI, Power appar och Microsoft Flow.</span><span class="sxs-lookup"><span data-stu-id="ac641-105">In addition to working with multiple Azure Analysis Services servers in the same region, the latest version of the gateway also works with Azure Logic Apps, Power BI, Power Apps, and Microsoft Flow.</span></span> <span data-ttu-id="ac641-106">Du kan associera flera tjänster i samma region med en enda gateway.</span><span class="sxs-lookup"><span data-stu-id="ac641-106">You can associate multiple services in the same region with a single gateway.</span></span> 

 <span data-ttu-id="ac641-107">Azure Analysis Services kräver en gateway-resurs i samma region.</span><span class="sxs-lookup"><span data-stu-id="ac641-107">Azure Analysis Services requires a gateway resource in the same region.</span></span> <span data-ttu-id="ac641-108">Du måste till exempel en gateway-resurs i östra USA 2 om du har Azure Analysis Services-servrar i östra USA 2.</span><span class="sxs-lookup"><span data-stu-id="ac641-108">For example, if you have Azure Analysis Services servers in the East US 2 region, you need a gateway resource in the East US 2 region.</span></span> <span data-ttu-id="ac641-109">Flera servrar i östra USA 2 kan använda samma gateway.</span><span class="sxs-lookup"><span data-stu-id="ac641-109">Multiple servers in East US 2 can use the same gateway.</span></span>

<span data-ttu-id="ac641-110">Hämtar installationsprogrammet gatewayen första gången är en process i fyra delar:</span><span class="sxs-lookup"><span data-stu-id="ac641-110">Getting setup with the gateway the first time is a four-part process:</span></span>

- <span data-ttu-id="ac641-111">**Hämta och kör installationsprogrammet** -det här steget installerar gateway-tjänsten på en dator i din organisation.</span><span class="sxs-lookup"><span data-stu-id="ac641-111">**Download and run setup** - This step installs a gateway service on a computer in your organization.</span></span>

- <span data-ttu-id="ac641-112">**Registrera din gateway** – i det här steget kan du ange ett namn och återställning för din gateway och välj en region som registrerar din gateway med Gateway-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ac641-112">**Register your gateway** - In this step, you specify a name and recovery key for your gateway, and select a region, registering your gateway with the Gateway Cloud Service.</span></span>

- <span data-ttu-id="ac641-113">**Skapa en gateway-resurs i Azure** – i det här steget skapar du en gateway-resurs i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ac641-113">**Create a gateway resource in Azure** - In this step, you create a gateway resource in your Azure subscription.</span></span>

- <span data-ttu-id="ac641-114">**Anslut dina servrar till din gateway-resurs** -när du har en gateway-resurs i din prenumeration kan du börja ansluter dina servrar.</span><span class="sxs-lookup"><span data-stu-id="ac641-114">**Connect your servers to your gateway resource** - Once you have a gateway resource in your subscription, you can begin connecting your servers to it.</span></span>

<span data-ttu-id="ac641-115">När du har en gateway-resurs som har konfigurerats för din prenumeration kan ansluta du flera servrar och andra tjänster till den.</span><span class="sxs-lookup"><span data-stu-id="ac641-115">Once you have a gateway resource configured for your subscription, you can connect multiple servers, and other services to it.</span></span> <span data-ttu-id="ac641-116">Du behöver bara installera en annan gateway och skapa resurser för ytterligare en gateway om det finns servrar eller andra tjänster i en annan region.</span><span class="sxs-lookup"><span data-stu-id="ac641-116">You only need to install a different gateway and create additional gateway resources if you have servers or other services in a different region.</span></span>

<span data-ttu-id="ac641-117">Om du vill komma igång nu direkt, se [installera och konfigurera lokala datagateway](analysis-services-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="ac641-117">To get started right away, see [Install and configure on-premises data gateway](analysis-services-gateway-install.md).</span></span>

## <span data-ttu-id="ac641-118"><a name="how-it-works"></a>Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="ac641-118"><a name="how-it-works"> </a>How it works</span></span>
<span data-ttu-id="ac641-119">Den gateway som du installerar på en dator i din organisation körs som en windowstjänst **lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="ac641-119">The gateway you install on a computer in your organization runs as a Windows service, **On-premises data gateway**.</span></span> <span data-ttu-id="ac641-120">Den här lokal tjänst har registrerats med Gateway-Molntjänsten via Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ac641-120">This local service is registered with the Gateway Cloud Service through Azure Service Bus.</span></span> <span data-ttu-id="ac641-121">Du kan sedan skapa en gateway-resurs Gateway Cloud Service för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ac641-121">You then create a gateway resource Gateway Cloud Service for your Azure subscription.</span></span> <span data-ttu-id="ac641-122">Azure Analysis Services-servrar är nu ansluten till din gateway-resurs.</span><span class="sxs-lookup"><span data-stu-id="ac641-122">Your Azure Analysis Services servers are then connected to your gateway resource.</span></span> <span data-ttu-id="ac641-123">När modeller på servern behöver ansluta till dina lokala data källor för frågor eller bearbetning, passerar fråga och dataflöde gatewayresursen, Azure Service Bus, lokala lokala data gateway-tjänsten och dina datakällor.</span><span class="sxs-lookup"><span data-stu-id="ac641-123">When models on your server need to connect to your on-premises data sources for queries or processing, a query and data flow traverses the gateway resource, Azure Service Bus, the local on-premises data gateway service, and your data sources.</span></span> 

![Hur det fungerar](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

<span data-ttu-id="ac641-125">Frågor och dataflöde:</span><span class="sxs-lookup"><span data-stu-id="ac641-125">Queries and data flow:</span></span>

1. <span data-ttu-id="ac641-126">En fråga skapas av Molntjänsten med krypterade autentiseringsuppgifter för den lokala datakällan.</span><span class="sxs-lookup"><span data-stu-id="ac641-126">A query is created by the cloud service with the encrypted credentials for the on-premises data source.</span></span> <span data-ttu-id="ac641-127">Sedan skickas till en kö att bearbeta-gateway.</span><span class="sxs-lookup"><span data-stu-id="ac641-127">It's then sent to a queue for the gateway to process.</span></span>
2. <span data-ttu-id="ac641-128">Gateway-Molntjänsten analyserar frågan och skickar en begäran om att den [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="ac641-128">The gateway cloud service analyzes the query and pushes the request to the [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).</span></span>
3. <span data-ttu-id="ac641-129">Lokala datagateway avsöker Azure Service Bus för väntande begäranden.</span><span class="sxs-lookup"><span data-stu-id="ac641-129">The on-premises data gateway polls the Azure Service Bus for pending requests.</span></span>
4. <span data-ttu-id="ac641-130">Gatewayen hämtar frågan dekrypterar autentiseringsuppgifterna och ansluter till datakällor med inloggningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="ac641-130">The gateway gets the query, decrypts the credentials, and connects to the data sources with those credentials.</span></span>
5. <span data-ttu-id="ac641-131">Gatewayen skickar frågan till datakällan för körning.</span><span class="sxs-lookup"><span data-stu-id="ac641-131">The gateway sends the query to the data source for execution.</span></span>
6. <span data-ttu-id="ac641-132">Resultatet skickas från datakällan, tillbaka till gateway och sedan på Molntjänsten och servern.</span><span class="sxs-lookup"><span data-stu-id="ac641-132">The results are sent from the data source, back to the gateway, and then onto the cloud service and your server.</span></span>

## <span data-ttu-id="ac641-133"><a name="windows-service-account"></a>För Windows-tjänstkontot</span><span class="sxs-lookup"><span data-stu-id="ac641-133"><a name="windows-service-account"> </a>Windows Service account</span></span>
<span data-ttu-id="ac641-134">Lokala datagateway är konfigurerad för att använda *NT SERVICE\PBIEgwService* för Windows-tjänsten inloggning autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ac641-134">The on-premises data gateway is configured to use *NT SERVICE\PBIEgwService* for the Windows service logon credential.</span></span> <span data-ttu-id="ac641-135">Som standard har rätt att logga in som en tjänst. i samband med den dator som du installerar en gateway på.</span><span class="sxs-lookup"><span data-stu-id="ac641-135">By default, it has the right of Logon as a service; in the context of the machine that you are installing the gateway on.</span></span> <span data-ttu-id="ac641-136">Den här autentiseringsuppgiften är inte samma konto som används för att ansluta till lokala datakällor eller ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ac641-136">This credential is not the same account used to connect to on-premises data sources or your Azure account.</span></span>  

<span data-ttu-id="ac641-137">Om du får problem med proxyservern på grund av autentisering du kanske vill ändra Windows-tjänstkontot till en domänanvändare eller Hanterat tjänstkonto.</span><span class="sxs-lookup"><span data-stu-id="ac641-137">If you encounter issues with your proxy server due to authentication, you may want to change the Windows service account to a domain user or managed service account.</span></span>

## <span data-ttu-id="ac641-138"><a name="ports"></a>Portar</span><span class="sxs-lookup"><span data-stu-id="ac641-138"><a name="ports"> </a>Ports</span></span>
<span data-ttu-id="ac641-139">Gatewayen skapar en utgående anslutning till Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ac641-139">The gateway creates an outbound connection to Azure Service Bus.</span></span> <span data-ttu-id="ac641-140">Den kommunicerar på utgående portar: TCP 443 (standard), 5671, 5672, 9350 via 9354.</span><span class="sxs-lookup"><span data-stu-id="ac641-140">It communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span>  <span data-ttu-id="ac641-141">Gatewayen kräver inte ingående portar.</span><span class="sxs-lookup"><span data-stu-id="ac641-141">The gateway does not require inbound ports.</span></span>

<span data-ttu-id="ac641-142">Vi rekommenderar att du listan över godkända IP-adresser för dina dataområdet i brandväggen.</span><span class="sxs-lookup"><span data-stu-id="ac641-142">We recommend you whitelist the IP addresses for your data region in your firewall.</span></span> <span data-ttu-id="ac641-143">Du kan hämta den [Microsoft Azure-Datacenter IP-lista](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="ac641-143">You can download the [Microsoft Azure Datacenter IP list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="ac641-144">Den här listan uppdateras varje vecka.</span><span class="sxs-lookup"><span data-stu-id="ac641-144">This list is updated weekly.</span></span>

> [!NOTE]
> <span data-ttu-id="ac641-145">IP-adresser i listan Azure Datacenter IP finns i CIDR-notering.</span><span class="sxs-lookup"><span data-stu-id="ac641-145">The IP Addresses listed in the Azure Datacenter IP list are in CIDR notation.</span></span> <span data-ttu-id="ac641-146">Till exempel innebär inte 10.0.0.0/24 10.0.0.0 via 10.0.0.24.</span><span class="sxs-lookup"><span data-stu-id="ac641-146">For example, 10.0.0.0/24 does not mean 10.0.0.0 through 10.0.0.24.</span></span> <span data-ttu-id="ac641-147">Lär dig mer om den [CIDR-notering](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="ac641-147">Learn more about the [CIDR notation](http://whatismyipaddress.com/cidr).</span></span>
>
>

<span data-ttu-id="ac641-148">Följande är fullständigt kvalificerat domännamn som används av gateway.</span><span class="sxs-lookup"><span data-stu-id="ac641-148">The following are the fully qualified domain names used by the gateway.</span></span>

| <span data-ttu-id="ac641-149">Domännamn</span><span class="sxs-lookup"><span data-stu-id="ac641-149">Domain names</span></span> | <span data-ttu-id="ac641-150">Utgående portar</span><span class="sxs-lookup"><span data-stu-id="ac641-150">Outbound ports</span></span> | <span data-ttu-id="ac641-151">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ac641-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ac641-152">*. powerbi.com</span><span class="sxs-lookup"><span data-stu-id="ac641-152">*.powerbi.com</span></span> |<span data-ttu-id="ac641-153">80</span><span class="sxs-lookup"><span data-stu-id="ac641-153">80</span></span> |<span data-ttu-id="ac641-154">HTTP används för att hämta installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ac641-154">HTTP used to download the installer.</span></span> |
| <span data-ttu-id="ac641-155">*. powerbi.com</span><span class="sxs-lookup"><span data-stu-id="ac641-155">*.powerbi.com</span></span> |<span data-ttu-id="ac641-156">443</span><span class="sxs-lookup"><span data-stu-id="ac641-156">443</span></span> |<span data-ttu-id="ac641-157">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ac641-157">HTTPS</span></span> |
| <span data-ttu-id="ac641-158">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="ac641-158">*.analysis.windows.net</span></span> |<span data-ttu-id="ac641-159">443</span><span class="sxs-lookup"><span data-stu-id="ac641-159">443</span></span> |<span data-ttu-id="ac641-160">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ac641-160">HTTPS</span></span> |
| <span data-ttu-id="ac641-161">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="ac641-161">*.login.windows.net</span></span> |<span data-ttu-id="ac641-162">443</span><span class="sxs-lookup"><span data-stu-id="ac641-162">443</span></span> |<span data-ttu-id="ac641-163">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ac641-163">HTTPS</span></span> |
| <span data-ttu-id="ac641-164">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="ac641-164">*.servicebus.windows.net</span></span> |<span data-ttu-id="ac641-165">5671-5672</span><span class="sxs-lookup"><span data-stu-id="ac641-165">5671-5672</span></span> |<span data-ttu-id="ac641-166">Avancerade Message Queuing-protokollet (AMQP)</span><span class="sxs-lookup"><span data-stu-id="ac641-166">Advanced Message Queuing Protocol (AMQP)</span></span> |
| <span data-ttu-id="ac641-167">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="ac641-167">*.servicebus.windows.net</span></span> |<span data-ttu-id="ac641-168">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="ac641-168">443, 9350-9354</span></span> |<span data-ttu-id="ac641-169">Lyssnare på Service Bus Relay via TCP (kräver 443 för åtkomstkontroll token)</span><span class="sxs-lookup"><span data-stu-id="ac641-169">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> |
| <span data-ttu-id="ac641-170">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="ac641-170">*.frontend.clouddatahub.net</span></span> |<span data-ttu-id="ac641-171">443</span><span class="sxs-lookup"><span data-stu-id="ac641-171">443</span></span> |<span data-ttu-id="ac641-172">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ac641-172">HTTPS</span></span> |
| <span data-ttu-id="ac641-173">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="ac641-173">*.core.windows.net</span></span> |<span data-ttu-id="ac641-174">443</span><span class="sxs-lookup"><span data-stu-id="ac641-174">443</span></span> |<span data-ttu-id="ac641-175">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ac641-175">HTTPS</span></span> |
| <span data-ttu-id="ac641-176">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="ac641-176">login.microsoftonline.com</span></span> |<span data-ttu-id="ac641-177">443</span><span class="sxs-lookup"><span data-stu-id="ac641-177">443</span></span> |<span data-ttu-id="ac641-178">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ac641-178">HTTPS</span></span> |
| <span data-ttu-id="ac641-179">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="ac641-179">*.msftncsi.com</span></span> |<span data-ttu-id="ac641-180">443</span><span class="sxs-lookup"><span data-stu-id="ac641-180">443</span></span> |<span data-ttu-id="ac641-181">Används för att testa internet-anslutningen om denna gateway inte kan nås av Power BI-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ac641-181">Used to test internet connectivity if the gateway is unreachable by the Power BI service.</span></span> |
| <span data-ttu-id="ac641-182">*.microsoftonline p.com</span><span class="sxs-lookup"><span data-stu-id="ac641-182">*.microsoftonline-p.com</span></span> |<span data-ttu-id="ac641-183">443</span><span class="sxs-lookup"><span data-stu-id="ac641-183">443</span></span> |<span data-ttu-id="ac641-184">Används för autentisering beroende på konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ac641-184">Used for authentication depending on configuration.</span></span> |

### <span data-ttu-id="ac641-185"><a name="force-https"></a>Tvinga HTTPS-kommunikation med Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="ac641-185"><a name="force-https"></a>Forcing HTTPS communication with Azure Service Bus</span></span>
<span data-ttu-id="ac641-186">Du kan tvinga gateway att kommunicera med Azure Service Bus med hjälp av HTTPS i stället för direkt TCP; men kan detta avsevärt minska prestanda.</span><span class="sxs-lookup"><span data-stu-id="ac641-186">You can force the gateway to communicate with Azure Service Bus by using HTTPS instead of direct TCP; however, doing so can greatly reduce performance.</span></span> <span data-ttu-id="ac641-187">Du kan ändra den *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* filen genom att ändra värdet från `AutoDetect` till `Https`.</span><span class="sxs-lookup"><span data-stu-id="ac641-187">You can modify the *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* file by changing the value from `AutoDetect` to `Https`.</span></span> <span data-ttu-id="ac641-188">Den här filen finns vanligtvis i *C:\Program Files\On lokala datagateway*.</span><span class="sxs-lookup"><span data-stu-id="ac641-188">This file is typically located at *C:\Program Files\On-premises data gateway*.</span></span>

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <span data-ttu-id="ac641-189"><a name="faq"></a>Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="ac641-189"><a name="faq"></a>Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="ac641-190">Allmänt</span><span class="sxs-lookup"><span data-stu-id="ac641-190">General</span></span>

<span data-ttu-id="ac641-191">**Q**: behöver jag en gateway för datakällor i molnet, till exempel Azure SQL Database?</span><span class="sxs-lookup"><span data-stu-id="ac641-191">**Q**: Do I need a gateway for data sources in the cloud, such as Azure SQL Database?</span></span> <br/><span data-ttu-id="ac641-192">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="ac641-192">
**A**: No.</span></span> <span data-ttu-id="ac641-193">En gateway ansluter till endast lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="ac641-193">A gateway connects to on-premises data sources only.</span></span>

<span data-ttu-id="ac641-194">**Q**: har en gateway installeras på samma dator som datakällan?</span><span class="sxs-lookup"><span data-stu-id="ac641-194">**Q**: Does the gateway have to be installed on the same machine as the data source?</span></span> <br/><span data-ttu-id="ac641-195">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="ac641-195">
**A**: No.</span></span> <span data-ttu-id="ac641-196">Gatewayen ansluter till datakällan med hjälp av informationen som angavs.</span><span class="sxs-lookup"><span data-stu-id="ac641-196">The gateway connects to the data source using the connection information that was provided.</span></span> <span data-ttu-id="ac641-197">Överväg att gatewayen som ett klientprogram på detta sätt.</span><span class="sxs-lookup"><span data-stu-id="ac641-197">Consider the gateway as a client application in this sense.</span></span> <span data-ttu-id="ac641-198">Gatewayen måste bara möjligheten att ansluta till namnet på servern som angavs, vanligtvis i samma nätverk.</span><span class="sxs-lookup"><span data-stu-id="ac641-198">The gateway just needs the capability to connect to the server name that was provided, typically on the same network.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="ac641-199">**Q**: Varför måste jag använda ett arbets- eller skolkonto konto för att logga in?</span><span class="sxs-lookup"><span data-stu-id="ac641-199">**Q**: Why do I need to use a work or school account to sign in?</span></span> <br/><span data-ttu-id="ac641-200">
**En**: du kan bara använda en Azure arbets eller skolkonto när du installerar den lokala datagatewayen.</span><span class="sxs-lookup"><span data-stu-id="ac641-200">
**A**: You can only use an Azure work or school account when you install the on-premises data gateway.</span></span> <span data-ttu-id="ac641-201">Din inloggning konto lagras i en klient som hanteras av Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ac641-201">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="ac641-202">Azure AD-kontot användarens huvudnamn (UPN) matchar vanligtvis e-postadress.</span><span class="sxs-lookup"><span data-stu-id="ac641-202">Usually, your Azure AD account's user principal name (UPN) matches the email address.</span></span>

<span data-ttu-id="ac641-203">**Q**: var lagras mina autentiseringsuppgifter?</span><span class="sxs-lookup"><span data-stu-id="ac641-203">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="ac641-204">
**En**: de autentiseringsuppgifter som du anger för en datakälla krypteras och lagras i Gateway-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ac641-204">
**A**: The credentials that you enter for a data source are encrypted and stored in the Gateway Cloud Service.</span></span> <span data-ttu-id="ac641-205">Autentiseringsuppgifterna dekrypteras på lokala datagateway.</span><span class="sxs-lookup"><span data-stu-id="ac641-205">The credentials are decrypted at the on-premises data gateway.</span></span>

<span data-ttu-id="ac641-206">**Q**: finns det några krav för nätverksbandbredd?</span><span class="sxs-lookup"><span data-stu-id="ac641-206">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="ac641-207">
**En**: det har rekommenderar nätverket anslutningen har bra genomströmning.</span><span class="sxs-lookup"><span data-stu-id="ac641-207">
**A**: It's recommend your network connection has good throughput.</span></span> <span data-ttu-id="ac641-208">Alla miljöer skiljer sig, och mängden data som skickas påverkar resultaten.</span><span class="sxs-lookup"><span data-stu-id="ac641-208">Every environment is different, and the amount of data being sent affects the results.</span></span> <span data-ttu-id="ac641-209">Med hjälp av ExpressRoute kan hjälpa till att garantera en nivå av dataflödet mellan lokala och Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="ac641-209">Using ExpressRoute could help to guarantee a level of throughput between on-premises and the Azure datacenters.</span></span>
<span data-ttu-id="ac641-210">Du kan använda verktyg från tredje part Azure hastighet testa appen för att mäta din genomflöde.</span><span class="sxs-lookup"><span data-stu-id="ac641-210">You can use the third-party tool Azure Speed Test app to help gauge your throughput.</span></span>

<span data-ttu-id="ac641-211">**Q**: Vad är svarstiden för att köra frågor till en datakälla från gateway?</span><span class="sxs-lookup"><span data-stu-id="ac641-211">**Q**: What is the latency for running queries to a data source from the gateway?</span></span> <span data-ttu-id="ac641-212">Vad är den bästa arkitekturen?</span><span class="sxs-lookup"><span data-stu-id="ac641-212">What is the best architecture?</span></span> <br/><span data-ttu-id="ac641-213">
**En**: Installera gatewayen för att minska Nätverksfördröjningen som nära datakällan som möjligt.</span><span class="sxs-lookup"><span data-stu-id="ac641-213">
**A**: To reduce network latency, install the gateway as close to the data source as possible.</span></span> <span data-ttu-id="ac641-214">Om du kan installera gatewayen på den faktiska datakällan, minimerar den här närhet svarstiden införs.</span><span class="sxs-lookup"><span data-stu-id="ac641-214">If you can install the gateway on the actual data source, this proximity minimizes the latency introduced.</span></span> <span data-ttu-id="ac641-215">Överväg att Datacenter för.</span><span class="sxs-lookup"><span data-stu-id="ac641-215">Consider the datacenters too.</span></span> <span data-ttu-id="ac641-216">Till exempel om din tjänst använder västra USA datacenter, och du har SQL Server finns i en Azure VM, ska Azure VM vara i västra USA för.</span><span class="sxs-lookup"><span data-stu-id="ac641-216">For example, if your service uses the West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in the West US too.</span></span> <span data-ttu-id="ac641-217">Den här närhet minimerar fördröjning och undviker utgång avgifter på Azure VM.</span><span class="sxs-lookup"><span data-stu-id="ac641-217">This proximity minimizes latency and avoids egress charges on the Azure VM.</span></span>

<span data-ttu-id="ac641-218">**Q**: hur resultatet skickas tillbaka till molnet?</span><span class="sxs-lookup"><span data-stu-id="ac641-218">**Q**: How are results sent back to the cloud?</span></span> <br/><span data-ttu-id="ac641-219">
**En**: resultat skickas via Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ac641-219">
**A**: Results are sent through the Azure Service Bus.</span></span>

<span data-ttu-id="ac641-220">**Q**: finns det några inkommande anslutningar till gatewayen från molnet?</span><span class="sxs-lookup"><span data-stu-id="ac641-220">**Q**: Are there any inbound connections to the gateway from the cloud?</span></span> <br/><span data-ttu-id="ac641-221">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="ac641-221">
**A**: No.</span></span> <span data-ttu-id="ac641-222">Gatewayen använder utgående anslutningar till Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="ac641-222">The gateway uses outbound connections to Azure Service Bus.</span></span>

<span data-ttu-id="ac641-223">**Q**: Vad händer om jag blockera utgående anslutningar?</span><span class="sxs-lookup"><span data-stu-id="ac641-223">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="ac641-224">Vad behöver jag att öppna?</span><span class="sxs-lookup"><span data-stu-id="ac641-224">What do I need to open?</span></span> <br/><span data-ttu-id="ac641-225">
**En**: Se portar och värdar som används av gateway-servern.</span><span class="sxs-lookup"><span data-stu-id="ac641-225">
**A**: See the ports and hosts that the gateway uses.</span></span>

<span data-ttu-id="ac641-226">**Q**: det faktiska Windows-tjänsten som kallas?</span><span class="sxs-lookup"><span data-stu-id="ac641-226">**Q**: What is the actual Windows service called?</span></span><br/><span data-ttu-id="ac641-227">
**En**: I tjänster gatewayen som kallas lokala data gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ac641-227">
**A**: In Services, the gateway is called On-premises data gateway service.</span></span>

<span data-ttu-id="ac641-228">**Q**: kan gateway Windows-tjänsten körs med ett Azure Active Directory-konto?</span><span class="sxs-lookup"><span data-stu-id="ac641-228">**Q**: Can the gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="ac641-229">
**En**: Nej</span><span class="sxs-lookup"><span data-stu-id="ac641-229">
**A**: No.</span></span> <span data-ttu-id="ac641-230">Windows-tjänsten måste ha ett giltigt Windows-konto.</span><span class="sxs-lookup"><span data-stu-id="ac641-230">The Windows service must have a valid Windows account.</span></span> <span data-ttu-id="ac641-231">Som standard körs tjänsten med tjänst-SID NT SERVICE\PBIEgwService.</span><span class="sxs-lookup"><span data-stu-id="ac641-231">By default, the service runs with the Service SID, NT SERVICE\PBIEgwService.</span></span>

### <span data-ttu-id="ac641-232"><a name="high-availability"></a>Hög tillgänglighet och disaster recovery</span><span class="sxs-lookup"><span data-stu-id="ac641-232"><a name="high-availability"></a>High availability and disaster recovery</span></span>

<span data-ttu-id="ac641-233">**Q**: vilka alternativ som är tillgängliga för katastrofåterställning?</span><span class="sxs-lookup"><span data-stu-id="ac641-233">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="ac641-234">
**En**: du kan använda återställningsnyckeln för att återställa eller flytta en gateway.</span><span class="sxs-lookup"><span data-stu-id="ac641-234">
**A**: You can use the recovery key to restore or move a gateway.</span></span> <span data-ttu-id="ac641-235">När du installerar en gateway måste du ange återställningsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="ac641-235">When you install the gateway, specify the recovery key.</span></span>

<span data-ttu-id="ac641-236">**Q**: Vad är fördelen med återställningsnyckeln?</span><span class="sxs-lookup"><span data-stu-id="ac641-236">**Q**: What is the benefit of the recovery key?</span></span> <br/><span data-ttu-id="ac641-237">
**En**: återställningsnyckeln är ett sätt att migrera eller återställa gatewayinställningarna efter en katastrof.</span><span class="sxs-lookup"><span data-stu-id="ac641-237">
**A**: The recovery key provides a way to migrate or recover your gateway settings after a disaster.</span></span>

## <span data-ttu-id="ac641-238"><a name="troubleshooting"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="ac641-238"><a name="troubleshooting"> </a>Troubleshooting</span></span>

<span data-ttu-id="ac641-239">**Q**: hur kan jag se vilka frågor som ska skickas till den lokala datakällan?</span><span class="sxs-lookup"><span data-stu-id="ac641-239">**Q**: How can I see what queries are being sent to the on-premises data source?</span></span> <br/><span data-ttu-id="ac641-240">
**En**: du kan aktivera frågespårning som innehåller de förfrågningar som skickas.</span><span class="sxs-lookup"><span data-stu-id="ac641-240">
**A**: You can enable query tracing, which includes the queries that are sent.</span></span> <span data-ttu-id="ac641-241">Kom ihåg att ändra frågan spåra tillbaka till det ursprungliga värdet när du är klar felsökning.</span><span class="sxs-lookup"><span data-stu-id="ac641-241">Remember to change query tracing back to the original value when done troubleshooting.</span></span> <span data-ttu-id="ac641-242">Lämna frågespårning aktiverad skapar större loggar.</span><span class="sxs-lookup"><span data-stu-id="ac641-242">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="ac641-243">Du kan också titta på Verktyg som datakällan har för spårning frågor.</span><span class="sxs-lookup"><span data-stu-id="ac641-243">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="ac641-244">Du kan till exempel använda Extended Events eller SQL Profiler för SQL Server och Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="ac641-244">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="ac641-245">**Q**: där är gateway-loggarna?</span><span class="sxs-lookup"><span data-stu-id="ac641-245">**Q**: Where are the gateway logs?</span></span> <br/><span data-ttu-id="ac641-246">
**En**: Se loggarna senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ac641-246">
**A**: See Logs later in this topic.</span></span>

### <span data-ttu-id="ac641-247"><a name="update"></a>Uppdatera till den senaste versionen</span><span class="sxs-lookup"><span data-stu-id="ac641-247"><a name="update"></a>Update to the latest version</span></span>

<span data-ttu-id="ac641-248">Många problem kan ansluta när gateway-versionen blir inaktuella.</span><span class="sxs-lookup"><span data-stu-id="ac641-248">Many issues can surface when the gateway version becomes outdated.</span></span> <span data-ttu-id="ac641-249">Kontrollera att du använder den senaste versionen som allmän bra.</span><span class="sxs-lookup"><span data-stu-id="ac641-249">As good general practice, make sure that you use the latest version.</span></span> <span data-ttu-id="ac641-250">Om du inte har uppdaterat en gateway för en månad eller längre, kan du du överväga att installera den senaste versionen av gatewayen och se om du kan återskapa problemet.</span><span class="sxs-lookup"><span data-stu-id="ac641-250">If you haven't updated the gateway for a month or longer, you might consider installing the latest version of the gateway, and see if you can reproduce the issue.</span></span>

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="ac641-251">Fel: Det gick inte att lägga till användaren i gruppen.</span><span class="sxs-lookup"><span data-stu-id="ac641-251">Error: Failed to add user to group.</span></span> <span data-ttu-id="ac641-252">(-2147463168 PBIEgwService användare)</span><span class="sxs-lookup"><span data-stu-id="ac641-252">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="ac641-253">Du kan få detta fel om du försöker installera gatewayen på en domänkontrollant, vilket inte stöds.</span><span class="sxs-lookup"><span data-stu-id="ac641-253">You might get this error if you try to install the gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="ac641-254">Se till att du distribuerar gatewayen på en dator som inte är en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="ac641-254">Make sure that you deploy the gateway on a machine that isn't a domain controller.</span></span>

## <span data-ttu-id="ac641-255"><a name="logs"></a>Loggar</span><span class="sxs-lookup"><span data-stu-id="ac641-255"><a name="logs"></a>Logs</span></span>

<span data-ttu-id="ac641-256">Loggfiler är en viktig resurs när du felsöker.</span><span class="sxs-lookup"><span data-stu-id="ac641-256">Log files are an important resource when troubleshooting.</span></span>

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="ac641-257">Enterprise gateway tjänstloggar</span><span class="sxs-lookup"><span data-stu-id="ac641-257">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a><span data-ttu-id="ac641-258">Av konfigurationsloggar</span><span class="sxs-lookup"><span data-stu-id="ac641-258">Configuration logs</span></span>

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a><span data-ttu-id="ac641-259">Händelseloggar</span><span class="sxs-lookup"><span data-stu-id="ac641-259">Event logs</span></span>

<span data-ttu-id="ac641-260">Du kan hitta loggar Data Management Gateway och PowerBIGateway under **program- och tjänstloggar**.</span><span class="sxs-lookup"><span data-stu-id="ac641-260">You can find the Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>


## <span data-ttu-id="ac641-261"><a name="telemetry"></a>Telemetri</span><span class="sxs-lookup"><span data-stu-id="ac641-261"><a name="telemetry"></a>Telemetry</span></span>
<span data-ttu-id="ac641-262">Telemetri kan användas för övervakning och felsökning.</span><span class="sxs-lookup"><span data-stu-id="ac641-262">Telemetry can be used for monitoring and troubleshooting.</span></span> <span data-ttu-id="ac641-263">Som standard</span><span class="sxs-lookup"><span data-stu-id="ac641-263">By default</span></span>

<span data-ttu-id="ac641-264">**Aktivera telemetri**</span><span class="sxs-lookup"><span data-stu-id="ac641-264">**To turn on telemetry**</span></span>

1.  <span data-ttu-id="ac641-265">Kontrollera katalogen lokala data gateway-klienten på datorn.</span><span class="sxs-lookup"><span data-stu-id="ac641-265">Check the On-premises data gateway client directory on the computer.</span></span> <span data-ttu-id="ac641-266">Vanligtvis är det **%systemdrive%\Program Files\On lokala datagateway**.</span><span class="sxs-lookup"><span data-stu-id="ac641-266">Typically, it is **%systemdrive%\Program Files\On-premises data gateway**.</span></span> <span data-ttu-id="ac641-267">Du kan öppna tjänstekonsolen och kontrollera sökvägen till körbar fil: en egenskap för lokala data gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ac641-267">Or, you can open a Services console and check the Path to executable: A property of the On-premises data gateway service.</span></span>
2.  <span data-ttu-id="ac641-268">I filen Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config från klienten directory.</span><span class="sxs-lookup"><span data-stu-id="ac641-268">In the Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config file from client directory.</span></span> <span data-ttu-id="ac641-269">Ändra SendTelemetry-inställningen till true.</span><span class="sxs-lookup"><span data-stu-id="ac641-269">Change the SendTelemetry setting to true.</span></span>
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  <span data-ttu-id="ac641-270">Spara dina ändringar och starta om tjänsten Windows: lokala data gateway-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ac641-270">Save your changes and restart the Windows service: On-premises data gateway service.</span></span>




## <a name="next-steps"></a><span data-ttu-id="ac641-271">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ac641-271">Next steps</span></span>
* [<span data-ttu-id="ac641-272">Hantera Analysis Services</span><span class="sxs-lookup"><span data-stu-id="ac641-272">Manage Analysis Services</span></span>](analysis-services-manage.md)
* [<span data-ttu-id="ac641-273">Hämta data från Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="ac641-273">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)
