---
title: "Felsöka Azure Application Gateway felaktiga Gateway (502) | Microsoft Docs"
description: "Lär dig att felsöka program Gateway 502"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 1d431ead-d318-47d8-b3ad-9c69f7e08813
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: cbf9c552c4818b3925f449081539f1db6d61918e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a><span data-ttu-id="d8c46-103">Felsöka Felaktig gateway-fel i Programgateway</span><span class="sxs-lookup"><span data-stu-id="d8c46-103">Troubleshooting bad gateway errors in Application Gateway</span></span>

<span data-ttu-id="d8c46-104">Lär dig hur du felsöker Felaktig gateway (502) felmeddelanden när du använder Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d8c46-104">Learn how to troubleshoot bad gateway (502) errors received when using application gateway.</span></span>

## <a name="overview"></a><span data-ttu-id="d8c46-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="d8c46-105">Overview</span></span>

<span data-ttu-id="d8c46-106">När du har konfigurerat en Programgateway är en av de fel som kan användarna få ”Serverfel: 502 - webbservern tog emot ett ogiltigt svar när den fungerade som en gateway eller proxy server”.</span><span class="sxs-lookup"><span data-stu-id="d8c46-106">After configuring an application gateway, one of the errors that users may encounter is "Server Error: 502 - Web server received an invalid response while acting as a gateway or proxy server".</span></span> <span data-ttu-id="d8c46-107">Det här felet kan bero på följande huvudsakliga orsaker:</span><span class="sxs-lookup"><span data-stu-id="d8c46-107">This error may happen due to the following main reasons:</span></span>

* <span data-ttu-id="d8c46-108">Azure Application Gateway [backend-adresspool har inte konfigurerats eller tom](#empty-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="d8c46-108">Azure Application Gateway's [back-end pool is not configured or empty](#empty-backendaddresspool).</span></span>
* <span data-ttu-id="d8c46-109">Inga virtuella datorer eller instanser i [Skaluppsättning är felfria](#unhealthy-instances-in-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="d8c46-109">None of the VMs or instances in [VM Scale Set are healthy](#unhealthy-instances-in-backendaddresspool).</span></span>
* <span data-ttu-id="d8c46-110">Backend-virtuella datorer eller instanser av Skaluppsättning [inte svarar på hälsoavsökningen standard](#problems-with-default-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="d8c46-110">Back-end VMs or instances of VM Scale Set are [not responding to the default health probe](#problems-with-default-health-probe.md).</span></span>
* <span data-ttu-id="d8c46-111">Ogiltig eller felaktig [konfigurationen av anpassade hälsoavsökningar](#problems-with-custom-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="d8c46-111">Invalid or improper [configuration of custom health probes](#problems-with-custom-health-probe.md).</span></span>
* <span data-ttu-id="d8c46-112">[Begär timeout eller anslutningsproblem](#request-time-out) med användarförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="d8c46-112">[Request time out or connectivity issues](#request-time-out) with user requests.</span></span>

## <a name="empty-backendaddresspool"></a><span data-ttu-id="d8c46-113">Tom BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="d8c46-113">Empty BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="d8c46-114">Orsak</span><span class="sxs-lookup"><span data-stu-id="d8c46-114">Cause</span></span>

<span data-ttu-id="d8c46-115">Om Application Gateway inte har några virtuella datorer eller VM Scale Set konfigurerats i backend-adresspool, det går inte att vidarebefordra alla kundbegäran och genererar en felaktig gateway-fel.</span><span class="sxs-lookup"><span data-stu-id="d8c46-115">If the Application Gateway has no VMs or VM Scale Set configured in the back-end address pool, it cannot route any customer request and throws a bad gateway error.</span></span>

### <a name="solution"></a><span data-ttu-id="d8c46-116">Lösning</span><span class="sxs-lookup"><span data-stu-id="d8c46-116">Solution</span></span>

<span data-ttu-id="d8c46-117">Kontrollera att backend-adresspool inte är tom.</span><span class="sxs-lookup"><span data-stu-id="d8c46-117">Ensure that the back-end address pool is not empty.</span></span> <span data-ttu-id="d8c46-118">Detta kan göras antingen via PowerShell, CLI eller portal.</span><span class="sxs-lookup"><span data-stu-id="d8c46-118">This can be done either via PowerShell, CLI, or portal.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

<span data-ttu-id="d8c46-119">Utdata från den föregående cmdleten bör innehålla icke-tom backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="d8c46-119">The output from the preceding cmdlet should contain non-empty back-end address pool.</span></span> <span data-ttu-id="d8c46-120">Följande är ett exempel där två pooler returneras som har konfigurerats med FQDN eller IP-adresser för serverdel virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d8c46-120">Following is an example where two pools are returned which are configured with FQDN or IP addresses for backend VMs.</span></span> <span data-ttu-id="d8c46-121">Etablering tillståndet för BackendAddressPool måste vara ”lyckades”.</span><span class="sxs-lookup"><span data-stu-id="d8c46-121">The provisioning state of the BackendAddressPool must be 'Succeeded'.</span></span>

<span data-ttu-id="d8c46-122">BackendAddressPoolsText:</span><span class="sxs-lookup"><span data-stu-id="d8c46-122">BackendAddressPoolsText:</span></span>

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a><span data-ttu-id="d8c46-123">Feltillstånd instanser i BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="d8c46-123">Unhealthy instances in BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="d8c46-124">Orsak</span><span class="sxs-lookup"><span data-stu-id="d8c46-124">Cause</span></span>

<span data-ttu-id="d8c46-125">Om alla instanser av BackendAddressPool är ohälsosamt kan sedan Programgateway inte alla serverdelar på vägen begäran om användaren.</span><span class="sxs-lookup"><span data-stu-id="d8c46-125">If all the instances of BackendAddressPool are unhealthy, then Application Gateway would not have any back-end to route user request to.</span></span> <span data-ttu-id="d8c46-126">Detta kan också vara fallet när backend-instanser är felfria men har inte programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="d8c46-126">This could also be the case when back-end instances are healthy but do not have the required application deployed.</span></span>

### <a name="solution"></a><span data-ttu-id="d8c46-127">Lösning</span><span class="sxs-lookup"><span data-stu-id="d8c46-127">Solution</span></span>

<span data-ttu-id="d8c46-128">Kontrollera att instanserna är felfri och programmet har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="d8c46-128">Ensure that the instances are healthy and the application is properly configured.</span></span> <span data-ttu-id="d8c46-129">Kontrollera om backend-instanserna är kan svara på ett ping från en annan virtuell dator i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d8c46-129">Check if the back-end instances are able to respond to a ping from another VM in the same VNet.</span></span> <span data-ttu-id="d8c46-130">Se till att begäran från en webbläsare till webbprogrammet är användbar om konfigurerats med en offentlig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d8c46-130">If configured with a public end point, ensure that a browser request to the web application is serviceable.</span></span>

## <a name="problems-with-default-health-probe"></a><span data-ttu-id="d8c46-131">Problem med standard hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="d8c46-131">Problems with default health probe</span></span>

### <a name="cause"></a><span data-ttu-id="d8c46-132">Orsak</span><span class="sxs-lookup"><span data-stu-id="d8c46-132">Cause</span></span>

<span data-ttu-id="d8c46-133">502 fel kan också vara ofta indikatorer hälsoavsökningen standard inte är kan nå backend-virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d8c46-133">502 errors can also be frequent indicators that the default health probe is not able to reach back-end VMs.</span></span> <span data-ttu-id="d8c46-134">När en Programgateway instans etableras, konfigurerar den automatiskt en standard hälsoavsökningen till varje BackendAddressPool med hjälp av egenskaperna för BackendHttpSetting.</span><span class="sxs-lookup"><span data-stu-id="d8c46-134">When an Application Gateway instance is provisioned, it automatically configures a default health probe to each BackendAddressPool using properties of the BackendHttpSetting.</span></span> <span data-ttu-id="d8c46-135">Inga indata från användaren krävs för att ange den här avsökningen.</span><span class="sxs-lookup"><span data-stu-id="d8c46-135">No user input is required to set this probe.</span></span> <span data-ttu-id="d8c46-136">När en regel för belastningsutjämning konfigureras görs mer specifikt en association mellan en BackendHttpSetting och BackendAddressPool.</span><span class="sxs-lookup"><span data-stu-id="d8c46-136">Specifically, when a load balancing rule is configured, an association is made between a BackendHttpSetting and BackendAddressPool.</span></span> <span data-ttu-id="d8c46-137">En standard-avsökning har konfigurerats för var och en av dessa associationer och Programgateway initierar en regelbundna hälsokontroller Kontrollera anslutning till varje instans i BackendAddressPool på porten som anges i BackendHttpSetting-elementet.</span><span class="sxs-lookup"><span data-stu-id="d8c46-137">A default probe is configured for each of these associations and Application Gateway initiates a periodic health check connection to each instance in the BackendAddressPool at the port specified in the BackendHttpSetting element.</span></span> <span data-ttu-id="d8c46-138">Följande tabell visas de värden som är associerade med hälsoavsökningen standard.</span><span class="sxs-lookup"><span data-stu-id="d8c46-138">Following table lists the values associated with the default health probe.</span></span>

| <span data-ttu-id="d8c46-139">Avsökningen egenskapen</span><span class="sxs-lookup"><span data-stu-id="d8c46-139">Probe property</span></span> | <span data-ttu-id="d8c46-140">Värde</span><span class="sxs-lookup"><span data-stu-id="d8c46-140">Value</span></span> | <span data-ttu-id="d8c46-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d8c46-141">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d8c46-142">URL för webbavsökning</span><span class="sxs-lookup"><span data-stu-id="d8c46-142">Probe URL</span></span> |<span data-ttu-id="d8c46-143">http://127.0.0.1/</span><span class="sxs-lookup"><span data-stu-id="d8c46-143">http://127.0.0.1/</span></span> |<span data-ttu-id="d8c46-144">URL-sökväg</span><span class="sxs-lookup"><span data-stu-id="d8c46-144">URL path</span></span> |
| <span data-ttu-id="d8c46-145">intervall</span><span class="sxs-lookup"><span data-stu-id="d8c46-145">Interval</span></span> |<span data-ttu-id="d8c46-146">30</span><span class="sxs-lookup"><span data-stu-id="d8c46-146">30</span></span> |<span data-ttu-id="d8c46-147">Avsökningsintervall i sekunder</span><span class="sxs-lookup"><span data-stu-id="d8c46-147">Probe interval in seconds</span></span> |
| <span data-ttu-id="d8c46-148">Timeout</span><span class="sxs-lookup"><span data-stu-id="d8c46-148">Time-out</span></span> |<span data-ttu-id="d8c46-149">30</span><span class="sxs-lookup"><span data-stu-id="d8c46-149">30</span></span> |<span data-ttu-id="d8c46-150">Avsökningen tidsgräns i sekunder</span><span class="sxs-lookup"><span data-stu-id="d8c46-150">Probe time-out in seconds</span></span> |
| <span data-ttu-id="d8c46-151">Tröskelvärde för ohälsosamt värde</span><span class="sxs-lookup"><span data-stu-id="d8c46-151">Unhealthy threshold</span></span> |<span data-ttu-id="d8c46-152">3</span><span class="sxs-lookup"><span data-stu-id="d8c46-152">3</span></span> |<span data-ttu-id="d8c46-153">Avsökning för antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="d8c46-153">Probe retry count.</span></span> <span data-ttu-id="d8c46-154">Backend-server markeras när efterföljande avsökningen Felberäkning når tröskelvärde för ohälsosamt värde.</span><span class="sxs-lookup"><span data-stu-id="d8c46-154">The back-end server is marked down after the consecutive probe failure count reaches the unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="d8c46-155">Lösning</span><span class="sxs-lookup"><span data-stu-id="d8c46-155">Solution</span></span>

* <span data-ttu-id="d8c46-156">Se till att en standardwebbplats har konfigurerats och lyssnar på 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="d8c46-156">Ensure that a default site is configured and is listening at 127.0.0.1.</span></span>
* <span data-ttu-id="d8c46-157">Om BackendHttpSetting anger en annan port än 80, ska standardwebbplatsen konfigureras för att lyssna på porten.</span><span class="sxs-lookup"><span data-stu-id="d8c46-157">If BackendHttpSetting specifies a port other than 80, the default site should be configured to listen at that port.</span></span>
* <span data-ttu-id="d8c46-158">Anropet till http://127.0.0.1:port ska returnera ett HTTP-Resultatkod 200.</span><span class="sxs-lookup"><span data-stu-id="d8c46-158">The call to http://127.0.0.1:port should return an HTTP result code of 200.</span></span> <span data-ttu-id="d8c46-159">Detta ska returneras inom tidsgränsen 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="d8c46-159">This should be returned within the 30 sec time-out period.</span></span>
* <span data-ttu-id="d8c46-160">Se till att konfigurerade porten är öppen och att det inte finns några brandväggsregler Azure Nätverkssäkerhetsgrupperna, som blockerar inkommande eller utgående trafik på den konfigurerade porten.</span><span class="sxs-lookup"><span data-stu-id="d8c46-160">Ensure that port configured is open and that there are no firewall rules or Azure Network Security Groups, which block incoming or outgoing traffic on the port configured.</span></span>
* <span data-ttu-id="d8c46-161">Om klassiska virtuella Azure-datorer eller tjänst i molnet används med FQDN eller offentlig IP-adress, se till att motsvarande [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) öppnas.</span><span class="sxs-lookup"><span data-stu-id="d8c46-161">If Azure classic VMs or Cloud Service is used with FQDN or Public IP, ensure that the corresponding [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) is opened.</span></span>
* <span data-ttu-id="d8c46-162">Om den virtuella datorn är konfigurerad via Azure Resource Manager och ligger utanför VNet där Application Gateway har distribuerats, [Nätverkssäkerhetsgruppen](../virtual-network/virtual-networks-nsg.md) måste konfigureras för att tillåta åtkomst på önskad port.</span><span class="sxs-lookup"><span data-stu-id="d8c46-162">If the VM is configured via Azure Resource Manager and is outside the VNet where Application Gateway is deployed, [Network Security Group](../virtual-network/virtual-networks-nsg.md) must be configured to allow access on the desired port.</span></span>

## <a name="problems-with-custom-health-probe"></a><span data-ttu-id="d8c46-163">Problem med anpassade hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="d8c46-163">Problems with custom health probe</span></span>

### <a name="cause"></a><span data-ttu-id="d8c46-164">Orsak</span><span class="sxs-lookup"><span data-stu-id="d8c46-164">Cause</span></span>

<span data-ttu-id="d8c46-165">Anpassade hälsoavsökningar ger ytterligare flexibilitet till standardvärdet sökning beteende.</span><span class="sxs-lookup"><span data-stu-id="d8c46-165">Custom health probes allow additional flexibility to the default probing behavior.</span></span> <span data-ttu-id="d8c46-166">När du använder anpassade avsökningar kan användare konfigurera avsökningsintervall, URL-Adressen och sökvägen för att testa och hur många misslyckade svar att godkänna innan du markerar backend-pool-instans som ohälsosamt.</span><span class="sxs-lookup"><span data-stu-id="d8c46-166">When using custom probes, users can configure the probe interval, the URL, and path to test, and how many failed responses to accept before marking the back-end pool instance as unhealthy.</span></span> <span data-ttu-id="d8c46-167">Följande ytterligare egenskaper läggs till.</span><span class="sxs-lookup"><span data-stu-id="d8c46-167">The following additional properties are added.</span></span>

| <span data-ttu-id="d8c46-168">Avsökningen egenskapen</span><span class="sxs-lookup"><span data-stu-id="d8c46-168">Probe property</span></span> | <span data-ttu-id="d8c46-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d8c46-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d8c46-170">Namn</span><span class="sxs-lookup"><span data-stu-id="d8c46-170">Name</span></span> |<span data-ttu-id="d8c46-171">Namnet på avsökningen.</span><span class="sxs-lookup"><span data-stu-id="d8c46-171">Name of the probe.</span></span> <span data-ttu-id="d8c46-172">Det här namnet används för att referera till avsökning i backend-HTTP-inställningar.</span><span class="sxs-lookup"><span data-stu-id="d8c46-172">This name is used to refer to the probe in back-end HTTP settings.</span></span> |
| <span data-ttu-id="d8c46-173">Protokoll</span><span class="sxs-lookup"><span data-stu-id="d8c46-173">Protocol</span></span> |<span data-ttu-id="d8c46-174">Protokoll som används för att skicka avsökningen.</span><span class="sxs-lookup"><span data-stu-id="d8c46-174">Protocol used to send the probe.</span></span> <span data-ttu-id="d8c46-175">Avsökningen används protokollet som definieras i backend-HTTP-inställningar</span><span class="sxs-lookup"><span data-stu-id="d8c46-175">The probe uses the protocol defined in the back-end HTTP settings</span></span> |
| <span data-ttu-id="d8c46-176">Värd</span><span class="sxs-lookup"><span data-stu-id="d8c46-176">Host</span></span> |<span data-ttu-id="d8c46-177">Värdnamn för att skicka avsökningen.</span><span class="sxs-lookup"><span data-stu-id="d8c46-177">Host name to send the probe.</span></span> <span data-ttu-id="d8c46-178">Gäller endast när flera platser har konfigurerats på Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="d8c46-178">Applicable only when multi-site is configured on Application Gateway.</span></span> <span data-ttu-id="d8c46-179">Detta skiljer sig från den virtuella datorns värdnamn.</span><span class="sxs-lookup"><span data-stu-id="d8c46-179">This is different from VM host name.</span></span> |
| <span data-ttu-id="d8c46-180">Sökväg</span><span class="sxs-lookup"><span data-stu-id="d8c46-180">Path</span></span> |<span data-ttu-id="d8c46-181">Avsökningen relativ sökväg.</span><span class="sxs-lookup"><span data-stu-id="d8c46-181">Relative path of the probe.</span></span> <span data-ttu-id="d8c46-182">Ogiltig sökväg startar från '/'.</span><span class="sxs-lookup"><span data-stu-id="d8c46-182">The valid path starts from '/'.</span></span> <span data-ttu-id="d8c46-183">Avsökningen skickas till \<protokollet\>://\<värden\>:\<port\>\<sökväg\></span><span class="sxs-lookup"><span data-stu-id="d8c46-183">The probe is sent to \<protocol\>://\<host\>:\<port\>\<path\></span></span> |
| <span data-ttu-id="d8c46-184">intervall</span><span class="sxs-lookup"><span data-stu-id="d8c46-184">Interval</span></span> |<span data-ttu-id="d8c46-185">Avsökning intervall i sekunder.</span><span class="sxs-lookup"><span data-stu-id="d8c46-185">Probe interval in seconds.</span></span> <span data-ttu-id="d8c46-186">Detta är tidsintervallet mellan två på varandra följande avsökningar.</span><span class="sxs-lookup"><span data-stu-id="d8c46-186">This is the time interval between two consecutive probes.</span></span> |
| <span data-ttu-id="d8c46-187">Timeout</span><span class="sxs-lookup"><span data-stu-id="d8c46-187">Time-out</span></span> |<span data-ttu-id="d8c46-188">Avsökning tidsgräns i sekunder.</span><span class="sxs-lookup"><span data-stu-id="d8c46-188">Probe time-out in seconds.</span></span> <span data-ttu-id="d8c46-189">Om ett giltigt svar inte emot inom denna tidsgräns, markeras avsökningen som misslyckad.</span><span class="sxs-lookup"><span data-stu-id="d8c46-189">If a valid response is not received within this time-out period, the probe is marked as failed.</span></span> |
| <span data-ttu-id="d8c46-190">Tröskelvärde för ohälsosamt värde</span><span class="sxs-lookup"><span data-stu-id="d8c46-190">Unhealthy threshold</span></span> |<span data-ttu-id="d8c46-191">Avsökning för antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="d8c46-191">Probe retry count.</span></span> <span data-ttu-id="d8c46-192">Backend-server markeras när efterföljande avsökningen Felberäkning når tröskelvärde för ohälsosamt värde.</span><span class="sxs-lookup"><span data-stu-id="d8c46-192">The back-end server is marked down after the consecutive probe failure count reaches the unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="d8c46-193">Lösning</span><span class="sxs-lookup"><span data-stu-id="d8c46-193">Solution</span></span>

<span data-ttu-id="d8c46-194">Verifiera att avsökning för anpassad hälsa är korrekt konfigurerad som tabellen ovan.</span><span class="sxs-lookup"><span data-stu-id="d8c46-194">Validate that the Custom Health Probe is configured correctly as the preceding table.</span></span> <span data-ttu-id="d8c46-195">Utöver föregående felsökningsstegen också kontrollera följande:</span><span class="sxs-lookup"><span data-stu-id="d8c46-195">In addition to the preceding troubleshooting steps, also ensure the following:</span></span>

* <span data-ttu-id="d8c46-196">Kontrollera att sonden har angetts korrekt enligt den [guiden](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d8c46-196">Ensure that the probe is correctly specified as per the [guide](application-gateway-create-probe-ps.md).</span></span>
* <span data-ttu-id="d8c46-197">Om Application Gateway har konfigurerats för en viss plats, som standard värden ska namn anges som 127.0.0.1, såvida inte annat konfigurerade i anpassade avsökning.</span><span class="sxs-lookup"><span data-stu-id="d8c46-197">If Application Gateway is configured for a single site, by default the Host name should be specified as '127.0.0.1', unless otherwise configured in custom probe.</span></span>
* <span data-ttu-id="d8c46-198">Se till att ett anrop till http://\<värden\>:\<port\>\<sökväg\> returnerar ett HTTP-Resultatkod 200.</span><span class="sxs-lookup"><span data-stu-id="d8c46-198">Ensure that a call to http://\<host\>:\<port\>\<path\> returns an HTTP result code of 200.</span></span>
* <span data-ttu-id="d8c46-199">Kontrollera att intervallet, timeout och UnhealtyThreshold är inom intervallen som är godkända.</span><span class="sxs-lookup"><span data-stu-id="d8c46-199">Ensure that Interval, Time-out and UnhealtyThreshold are within the acceptable ranges.</span></span>
* <span data-ttu-id="d8c46-200">Om du använder en HTTPS-avsökning ska du se till att kräver backend-servern SNI genom att konfigurera ett fallback-certifikatet på själva backend-servern.</span><span class="sxs-lookup"><span data-stu-id="d8c46-200">If using an HTTPS probe, make sure that the backend server doesn't require SNI by configuring a fallback certificate on the backend server itself.</span></span> 
* <span data-ttu-id="d8c46-201">Kontrollera att intervallet, timeout och UnhealtyThreshold är inom intervallen som är godkända.</span><span class="sxs-lookup"><span data-stu-id="d8c46-201">Ensure that Interval, Time-out, and UnhealtyThreshold are within the acceptable ranges.</span></span>

## <a name="request-time-out"></a><span data-ttu-id="d8c46-202">Tidsgräns för begäran</span><span class="sxs-lookup"><span data-stu-id="d8c46-202">Request time out</span></span>

### <a name="cause"></a><span data-ttu-id="d8c46-203">Orsak</span><span class="sxs-lookup"><span data-stu-id="d8c46-203">Cause</span></span>

<span data-ttu-id="d8c46-204">När en begäran tas emot Programgateway gäller de konfigurerade reglerna för begäran och skickar den till en backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="d8c46-204">When a user request is received, Application Gateway applies the configured rules to the request and routes it to a back-end pool instance.</span></span> <span data-ttu-id="d8c46-205">Det väntar på en konfigurerbar tidsperiod för svar från backend-instansen.</span><span class="sxs-lookup"><span data-stu-id="d8c46-205">It waits for a configurable interval of time for a response from the back-end instance.</span></span> <span data-ttu-id="d8c46-206">Det här intervallet är som standard **30 sekunder**.</span><span class="sxs-lookup"><span data-stu-id="d8c46-206">By default, this interval is **30 seconds**.</span></span> <span data-ttu-id="d8c46-207">Om Application Gateway inte får ett svar från serverprogrammet i det här intervallet visas användarbegäran ett 502 fel.</span><span class="sxs-lookup"><span data-stu-id="d8c46-207">If Application Gateway does not receive a response from back-end application in this interval, user request would see a 502 error.</span></span>

### <a name="solution"></a><span data-ttu-id="d8c46-208">Lösning</span><span class="sxs-lookup"><span data-stu-id="d8c46-208">Solution</span></span>

<span data-ttu-id="d8c46-209">Programgateway tillåter användare att konfigurera den här inställningen via BackendHttpSetting, som sedan kan tillämpas på olika pooler.</span><span class="sxs-lookup"><span data-stu-id="d8c46-209">Application Gateway allows users to configure this setting via BackendHttpSetting, which can be then applied to different pools.</span></span> <span data-ttu-id="d8c46-210">Olika backend-pooler kan ha olika BackendHttpSetting och därför olika begäran-timeout konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="d8c46-210">Different back-end pools can have different BackendHttpSetting and hence different request time out configured.</span></span>

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a><span data-ttu-id="d8c46-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d8c46-211">Next steps</span></span>

<span data-ttu-id="d8c46-212">Om de föregående stegen inte löser problemet kan du öppna en [stöder biljett](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="d8c46-212">If the preceding steps do not resolve the issue, open a [support ticket](https://azure.microsoft.com/support/options/).</span></span>

