---
title: "aaaEnable access tooAzure DC/OS-behållaren appen | Microsoft Docs"
description: "Hur tooenable offentliga åt tooDC/OS-behållare i Azure Container Service."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 1ba251ba5a176a6a5e1c7831655164e380a62b27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-public-access-tooan-azure-container-service-application"></a><span data-ttu-id="c6706-104">Aktivera allmän åtkomst tooan Azure Container Service-program</span><span class="sxs-lookup"><span data-stu-id="c6706-104">Enable public access tooan Azure Container Service application</span></span>
<span data-ttu-id="c6706-105">Alla DC/OS-behållare i hello ACS [offentlig agent poolen](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) är automatiskt synliga toohello internet.</span><span class="sxs-lookup"><span data-stu-id="c6706-105">Any DC/OS container in hello ACS [public agent pool](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) is automatically exposed toohello internet.</span></span> <span data-ttu-id="c6706-106">Som standard portarna **80**, **443**, **8080** öppnas, och (offentlig)-behållare som lyssnar på de portarna som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="c6706-106">By default, ports **80**, **443**, **8080** are opened, and any (public) container listening on those ports are accessible.</span></span> <span data-ttu-id="c6706-107">Den här artikeln visar hur tooopen fler portar för dina program i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="c6706-107">This article shows you how tooopen more ports for your applications in Azure Container Service.</span></span>

## <a name="open-a-port-portal"></a><span data-ttu-id="c6706-108">Öppna en port (portal)</span><span class="sxs-lookup"><span data-stu-id="c6706-108">Open a port (portal)</span></span>
<span data-ttu-id="c6706-109">Först måste vi vill tooopen hello-port.</span><span class="sxs-lookup"><span data-stu-id="c6706-109">First, we need tooopen hello port we want.</span></span>

1. <span data-ttu-id="c6706-110">Logga in toohello portal.</span><span class="sxs-lookup"><span data-stu-id="c6706-110">Log in toohello portal.</span></span>
2. <span data-ttu-id="c6706-111">Hitta hello resursgrupp som du har distribuerat hello Azure Container Service till.</span><span class="sxs-lookup"><span data-stu-id="c6706-111">Find hello resource group that you deployed hello Azure Container Service to.</span></span>
3. <span data-ttu-id="c6706-112">Välj hello-agentens belastningsutjämnare (som heter liknande för**XXXX-agent-lb-XXXX**).</span><span class="sxs-lookup"><span data-stu-id="c6706-112">Select hello agent load balancer (which is named similar too**XXXX-agent-lb-XXXX**).</span></span>
   
    ![Azure container service-belastningsutjämnare](./media/container-service-enable-public-access/agent-load-balancer.png)
4. <span data-ttu-id="c6706-114">Klicka på **avsökningar** och sedan **lägga till**.</span><span class="sxs-lookup"><span data-stu-id="c6706-114">Click **Probes** and then **Add**.</span></span>
   
    ![Azure container service-belastningsutjämnaren avsökningar](./media/container-service-enable-public-access/add-probe.png)
5. <span data-ttu-id="c6706-116">Fyll i hello avsökningen formuläret och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6706-116">Fill out hello probe form and click **OK**.</span></span>
   
   | <span data-ttu-id="c6706-117">Fält</span><span class="sxs-lookup"><span data-stu-id="c6706-117">Field</span></span> | <span data-ttu-id="c6706-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c6706-118">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="c6706-119">Namn</span><span class="sxs-lookup"><span data-stu-id="c6706-119">Name</span></span> |<span data-ttu-id="c6706-120">Ett beskrivande namn på hello avsökning.</span><span class="sxs-lookup"><span data-stu-id="c6706-120">A descriptive name of hello probe.</span></span> |
   | <span data-ttu-id="c6706-121">Port</span><span class="sxs-lookup"><span data-stu-id="c6706-121">Port</span></span> |<span data-ttu-id="c6706-122">hello behållaren tootest hello-port.</span><span class="sxs-lookup"><span data-stu-id="c6706-122">hello port of hello container tootest.</span></span> |
   | <span data-ttu-id="c6706-123">Sökväg</span><span class="sxs-lookup"><span data-stu-id="c6706-123">Path</span></span> |<span data-ttu-id="c6706-124">(När i HTTP-läge) hello relativa webbplats sökvägen tooprobe.</span><span class="sxs-lookup"><span data-stu-id="c6706-124">(When in HTTP mode) hello relative website path tooprobe.</span></span> <span data-ttu-id="c6706-125">HTTPS stöds inte.</span><span class="sxs-lookup"><span data-stu-id="c6706-125">HTTPS not supported.</span></span> |
   | <span data-ttu-id="c6706-126">intervall</span><span class="sxs-lookup"><span data-stu-id="c6706-126">Interval</span></span> |<span data-ttu-id="c6706-127">hello tidslängd mellan avsökningen försök i sekunder.</span><span class="sxs-lookup"><span data-stu-id="c6706-127">hello amount of time between probe attempts, in seconds.</span></span> |
   | <span data-ttu-id="c6706-128">Tröskelvärde för ohälsosamt värde</span><span class="sxs-lookup"><span data-stu-id="c6706-128">Unhealthy threshold</span></span> |<span data-ttu-id="c6706-129">Antal på varandra följande avsökningen försöker innan hello behållaren feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="c6706-129">Number of consecutive probe attempts before considering hello container unhealthy.</span></span> |
6. <span data-ttu-id="c6706-130">Tillbaka hello egenskaper för hello-agentens belastningsutjämnare, klickar du på **belastningsutjämningsregler** och sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c6706-130">Back at hello properties of hello agent load balancer, click **Load balancing rules** and then **Add**.</span></span>
   
    ![Azure container service regler för inläsning av belastningsutjämnare](./media/container-service-enable-public-access/add-balancer-rule.png)
7. <span data-ttu-id="c6706-132">Fyll i formuläret om hello belastningen belastningsutjämnaren och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6706-132">Fill out hello load balancer form and click **OK**.</span></span>
   
   | <span data-ttu-id="c6706-133">Fält</span><span class="sxs-lookup"><span data-stu-id="c6706-133">Field</span></span> | <span data-ttu-id="c6706-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c6706-134">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="c6706-135">Namn</span><span class="sxs-lookup"><span data-stu-id="c6706-135">Name</span></span> |<span data-ttu-id="c6706-136">Ett beskrivande namn på hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="c6706-136">A descriptive name of hello load balancer.</span></span> |
   | <span data-ttu-id="c6706-137">Port</span><span class="sxs-lookup"><span data-stu-id="c6706-137">Port</span></span> |<span data-ttu-id="c6706-138">hello offentliga inkommande port.</span><span class="sxs-lookup"><span data-stu-id="c6706-138">hello public incoming port.</span></span> |
   | <span data-ttu-id="c6706-139">Backend-port</span><span class="sxs-lookup"><span data-stu-id="c6706-139">Backend port</span></span> |<span data-ttu-id="c6706-140">hello interna offentlig port av hello behållaren tooroute trafik till.</span><span class="sxs-lookup"><span data-stu-id="c6706-140">hello internal-public port of hello container tooroute traffic to.</span></span> |
   | <span data-ttu-id="c6706-141">Serverdelspool</span><span class="sxs-lookup"><span data-stu-id="c6706-141">Backend pool</span></span> |<span data-ttu-id="c6706-142">hello behållare i den här poolen kommer att hello mål för den här belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="c6706-142">hello containers in this pool will be hello target for this load balancer.</span></span> |
   | <span data-ttu-id="c6706-143">Avsökningen</span><span class="sxs-lookup"><span data-stu-id="c6706-143">Probe</span></span> |<span data-ttu-id="c6706-144">Hej avsökningen används toodetermine om ett mål i hello **serverdelspool** är felfri.</span><span class="sxs-lookup"><span data-stu-id="c6706-144">hello probe used toodetermine if a target in hello **Backend pool** is healthy.</span></span> |
   | <span data-ttu-id="c6706-145">Persistence för session</span><span class="sxs-lookup"><span data-stu-id="c6706-145">Session persistence</span></span> |<span data-ttu-id="c6706-146">Anger hur trafik från en klient ska hanteras för hello varaktighet hello-sessionen.</span><span class="sxs-lookup"><span data-stu-id="c6706-146">Determines how traffic from a client should be handled for hello duration of hello session.</span></span><br><br><span data-ttu-id="c6706-147">**Ingen**: efterföljande förfrågningar från samma klient kan hanteras av alla behållare hello.</span><span class="sxs-lookup"><span data-stu-id="c6706-147">**None**: Successive requests from hello same client can be handled by any container.</span></span><br><span data-ttu-id="c6706-148">**Klienten IP**: efterföljande förfrågningar från hello samma klient-IP hanteras av hello samma behållare.</span><span class="sxs-lookup"><span data-stu-id="c6706-148">**Client IP**: Successive requests from hello same client IP are handled by hello same container.</span></span><br><span data-ttu-id="c6706-149">**Klienten IP och protokoll**: efterföljande förfrågningar från samma klient-IP- och protokollkombination hanteras av hello hello samma behållare.</span><span class="sxs-lookup"><span data-stu-id="c6706-149">**Client IP and protocol**: Successive requests from hello same client IP and protocol combination are handled by hello same container.</span></span> |
   | <span data-ttu-id="c6706-150">Tidsgränsen för inaktivitet</span><span class="sxs-lookup"><span data-stu-id="c6706-150">Idle timeout</span></span> |<span data-ttu-id="c6706-151">(TCP) I minuter, hello tid tookeep en TCP/HTTP-klient öppna utan *keep-alive* meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c6706-151">(TCP only) In minutes, hello time tookeep a TCP/HTTP client open without relying on *keep-alive* messages.</span></span> |

## <a name="add-a-security-rule-portal"></a><span data-ttu-id="c6706-152">Lägg till en säkerhetsregel (portal)</span><span class="sxs-lookup"><span data-stu-id="c6706-152">Add a security rule (portal)</span></span>
<span data-ttu-id="c6706-153">Därefter måste tooadd en anslutningssäkerhetsregel som dirigerar trafik från våra öppna port hello-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="c6706-153">Next, we need tooadd a security rule that routes traffic from our opened port through hello firewall.</span></span>

1. <span data-ttu-id="c6706-154">Logga in toohello portal.</span><span class="sxs-lookup"><span data-stu-id="c6706-154">Log in toohello portal.</span></span>
2. <span data-ttu-id="c6706-155">Hitta hello resursgrupp som du har distribuerat hello Azure Container Service till.</span><span class="sxs-lookup"><span data-stu-id="c6706-155">Find hello resource group that you deployed hello Azure Container Service to.</span></span>
3. <span data-ttu-id="c6706-156">Välj hello **offentliga** agent nätverkssäkerhetsgruppen (som heter liknande för**XXXX-agent-offentliga-nsg-XXXX**).</span><span class="sxs-lookup"><span data-stu-id="c6706-156">Select hello **public** agent network security group (which is named similar too**XXXX-agent-public-nsg-XXXX**).</span></span>
   
    ![Azure container service-nätverkssäkerhetsgrupp](./media/container-service-enable-public-access/agent-nsg.png)
4. <span data-ttu-id="c6706-158">Välj **inkommande säkerhetsregler** och sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c6706-158">Select **Inbound security rules** and then **Add**.</span></span>
   
    ![Azure container service regler för nätverkssäkerhetsgrupper](./media/container-service-enable-public-access/add-firewall-rule.png)
5. <span data-ttu-id="c6706-160">Fyll i hello brandväggen regeln tooallow den offentliga porten och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6706-160">Fill out hello firewall rule tooallow your public port and click **OK**.</span></span>
   
   | <span data-ttu-id="c6706-161">Fält</span><span class="sxs-lookup"><span data-stu-id="c6706-161">Field</span></span> | <span data-ttu-id="c6706-162">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c6706-162">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="c6706-163">Namn</span><span class="sxs-lookup"><span data-stu-id="c6706-163">Name</span></span> |<span data-ttu-id="c6706-164">Ett beskrivande namn på hello brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="c6706-164">A descriptive name of hello firewall rule.</span></span> |
   | <span data-ttu-id="c6706-165">Prioritet</span><span class="sxs-lookup"><span data-stu-id="c6706-165">Priority</span></span> |<span data-ttu-id="c6706-166">Prioritet rangordningen för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="c6706-166">Priority rank for hello rule.</span></span> <span data-ttu-id="c6706-167">hello lägre hello nummer hello högre hello prioritet.</span><span class="sxs-lookup"><span data-stu-id="c6706-167">hello lower hello number hello higher hello priority.</span></span> |
   | <span data-ttu-id="c6706-168">Källa</span><span class="sxs-lookup"><span data-stu-id="c6706-168">Source</span></span> |<span data-ttu-id="c6706-169">Begränsa hello inkommande IP-adressintervallet toobe tillåts eller nekas av den här regeln.</span><span class="sxs-lookup"><span data-stu-id="c6706-169">Restrict hello incoming IP address range toobe allowed or denied by this rule.</span></span> <span data-ttu-id="c6706-170">Använd **alla** toonot ange en begränsning.</span><span class="sxs-lookup"><span data-stu-id="c6706-170">Use **Any** toonot specify a restriction.</span></span> |
   | <span data-ttu-id="c6706-171">Tjänst</span><span class="sxs-lookup"><span data-stu-id="c6706-171">Service</span></span> |<span data-ttu-id="c6706-172">Välj en uppsättning fördefinierade tjänster som denna regel gäller.</span><span class="sxs-lookup"><span data-stu-id="c6706-172">Select a set of predefined services this security rule is for.</span></span> <span data-ttu-id="c6706-173">Använd annars **anpassad** toocreate egna.</span><span class="sxs-lookup"><span data-stu-id="c6706-173">Otherwise use **Custom** toocreate your own.</span></span> |
   | <span data-ttu-id="c6706-174">Protokoll</span><span class="sxs-lookup"><span data-stu-id="c6706-174">Protocol</span></span> |<span data-ttu-id="c6706-175">Begränsa trafik utifrån **TCP** eller **UDP**.</span><span class="sxs-lookup"><span data-stu-id="c6706-175">Restrict traffic based on **TCP** or **UDP**.</span></span> <span data-ttu-id="c6706-176">Använd **alla** toonot ange en begränsning.</span><span class="sxs-lookup"><span data-stu-id="c6706-176">Use **Any** toonot specify a restriction.</span></span> |
   | <span data-ttu-id="c6706-177">Portintervall</span><span class="sxs-lookup"><span data-stu-id="c6706-177">Port range</span></span> |<span data-ttu-id="c6706-178">När **Service** är **anpassade**, anger hello portintervall som påverkas av regeln.</span><span class="sxs-lookup"><span data-stu-id="c6706-178">When **Service** is **Custom**, specifies hello range of ports that this rule affects.</span></span> <span data-ttu-id="c6706-179">Du kan använda en enskild port, till exempel **80**, eller ett intervall som **1024 1500**.</span><span class="sxs-lookup"><span data-stu-id="c6706-179">You can use a single port, such as **80**, or a range like **1024-1500**.</span></span> |
   | <span data-ttu-id="c6706-180">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="c6706-180">Action</span></span> |<span data-ttu-id="c6706-181">Tillåt eller neka trafik som uppfyller hello villkor.</span><span class="sxs-lookup"><span data-stu-id="c6706-181">Allow or deny traffic that meets hello criteria.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c6706-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6706-182">Next steps</span></span>
<span data-ttu-id="c6706-183">Lär dig mer om hello skillnaden mellan [offentliga och privata DC/OS-agenterna](container-service-dcos-agents.md).</span><span class="sxs-lookup"><span data-stu-id="c6706-183">Learn about hello difference between [public and private DC/OS agents](container-service-dcos-agents.md).</span></span>

<span data-ttu-id="c6706-184">Läs mer om [hantera DC/OS-behållare](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="c6706-184">Read more information about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

