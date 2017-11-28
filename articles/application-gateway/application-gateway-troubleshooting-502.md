---
title: aaaTroubleshoot Azure Application Gateway felaktiga Gateway (502) fel | Microsoft Docs
description: "Lär dig hur tootroubleshoot programmet Gateway 502 fel"
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
ms.openlocfilehash: a50f736ac157256a53f6fbe3a208f0c11dd58f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a><span data-ttu-id="530c5-103">Felsöka Felaktig gateway-fel i Programgateway</span><span class="sxs-lookup"><span data-stu-id="530c5-103">Troubleshooting bad gateway errors in Application Gateway</span></span>

<span data-ttu-id="530c5-104">Lär dig hur tootroubleshoot Felaktig gateway (502) fel togs emot när du använder application gateway.</span><span class="sxs-lookup"><span data-stu-id="530c5-104">Learn how tootroubleshoot bad gateway (502) errors received when using application gateway.</span></span>

## <a name="overview"></a><span data-ttu-id="530c5-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="530c5-105">Overview</span></span>

<span data-ttu-id="530c5-106">När du har konfigurerat en Programgateway en hello-fel som kan användarna få är ”Serverfel: 502 - webbservern tog emot ett ogiltigt svar när den fungerade som en gateway eller proxy server”.</span><span class="sxs-lookup"><span data-stu-id="530c5-106">After configuring an application gateway, one of hello errors that users may encounter is "Server Error: 502 - Web server received an invalid response while acting as a gateway or proxy server".</span></span> <span data-ttu-id="530c5-107">Det här felet kan inträffa på grund av följande toohello huvudsakliga skälen:</span><span class="sxs-lookup"><span data-stu-id="530c5-107">This error may happen due toohello following main reasons:</span></span>

* <span data-ttu-id="530c5-108">Azure Application Gateway [backend-adresspool har inte konfigurerats eller tom](#empty-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="530c5-108">Azure Application Gateway's [back-end pool is not configured or empty](#empty-backendaddresspool).</span></span>
* <span data-ttu-id="530c5-109">Inga virtuella datorer hello eller instanser i [Skaluppsättning är felfria](#unhealthy-instances-in-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="530c5-109">None of hello VMs or instances in [VM Scale Set are healthy](#unhealthy-instances-in-backendaddresspool).</span></span>
* <span data-ttu-id="530c5-110">Backend-virtuella datorer eller instanser av Skaluppsättning [svarar inte toohello standard hälsoavsökningen](#problems-with-default-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="530c5-110">Back-end VMs or instances of VM Scale Set are [not responding toohello default health probe](#problems-with-default-health-probe.md).</span></span>
* <span data-ttu-id="530c5-111">Ogiltig eller felaktig [konfigurationen av anpassade hälsoavsökningar](#problems-with-custom-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="530c5-111">Invalid or improper [configuration of custom health probes](#problems-with-custom-health-probe.md).</span></span>
* <span data-ttu-id="530c5-112">[Begär timeout eller anslutningsproblem](#request-time-out) med användarförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="530c5-112">[Request time out or connectivity issues](#request-time-out) with user requests.</span></span>

## <a name="empty-backendaddresspool"></a><span data-ttu-id="530c5-113">Tom BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="530c5-113">Empty BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="530c5-114">Orsak</span><span class="sxs-lookup"><span data-stu-id="530c5-114">Cause</span></span>

<span data-ttu-id="530c5-115">Om hello Application Gateway har inga virtuella datorer eller VM Scale Set i hello backend-adresspool, det går inte att vidarebefordra alla kundbegäran och genererar en felaktig gateway-fel.</span><span class="sxs-lookup"><span data-stu-id="530c5-115">If hello Application Gateway has no VMs or VM Scale Set configured in hello back-end address pool, it cannot route any customer request and throws a bad gateway error.</span></span>

### <a name="solution"></a><span data-ttu-id="530c5-116">Lösning</span><span class="sxs-lookup"><span data-stu-id="530c5-116">Solution</span></span>

<span data-ttu-id="530c5-117">Se till att hello backend-adresspool inte är tom.</span><span class="sxs-lookup"><span data-stu-id="530c5-117">Ensure that hello back-end address pool is not empty.</span></span> <span data-ttu-id="530c5-118">Detta kan göras antingen via PowerShell, CLI eller portal.</span><span class="sxs-lookup"><span data-stu-id="530c5-118">This can be done either via PowerShell, CLI, or portal.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

<span data-ttu-id="530c5-119">hello utdata från föregående cmdlet hello ska innehålla icke-tom backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="530c5-119">hello output from hello preceding cmdlet should contain non-empty back-end address pool.</span></span> <span data-ttu-id="530c5-120">Följande är ett exempel där två pooler returneras som har konfigurerats med FQDN eller IP-adresser för serverdel virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="530c5-120">Following is an example where two pools are returned which are configured with FQDN or IP addresses for backend VMs.</span></span> <span data-ttu-id="530c5-121">hello Etableringsstatus för hello BackendAddressPool måste vara ”lyckades”.</span><span class="sxs-lookup"><span data-stu-id="530c5-121">hello provisioning state of hello BackendAddressPool must be 'Succeeded'.</span></span>

<span data-ttu-id="530c5-122">BackendAddressPoolsText:</span><span class="sxs-lookup"><span data-stu-id="530c5-122">BackendAddressPoolsText:</span></span>

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

## <a name="unhealthy-instances-in-backendaddresspool"></a><span data-ttu-id="530c5-123">Feltillstånd instanser i BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="530c5-123">Unhealthy instances in BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="530c5-124">Orsak</span><span class="sxs-lookup"><span data-stu-id="530c5-124">Cause</span></span>

<span data-ttu-id="530c5-125">Om alla hello instanser av BackendAddressPool är ohälsosamt kan sedan Programgateway inte någon backend-tooroute användarbegäran om att.</span><span class="sxs-lookup"><span data-stu-id="530c5-125">If all hello instances of BackendAddressPool are unhealthy, then Application Gateway would not have any back-end tooroute user request to.</span></span> <span data-ttu-id="530c5-126">Detta kan också vara fallet hello när backend-instanser är felfria men har inte hello krävs programmet distribuerades.</span><span class="sxs-lookup"><span data-stu-id="530c5-126">This could also be hello case when back-end instances are healthy but do not have hello required application deployed.</span></span>

### <a name="solution"></a><span data-ttu-id="530c5-127">Lösning</span><span class="sxs-lookup"><span data-stu-id="530c5-127">Solution</span></span>

<span data-ttu-id="530c5-128">Kontrollera att hello instanser är felfria och hello programmet har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="530c5-128">Ensure that hello instances are healthy and hello application is properly configured.</span></span> <span data-ttu-id="530c5-129">Kontrollera om hello backend-instanserna är kan toorespond tooa ping från en annan virtuell dator i hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="530c5-129">Check if hello back-end instances are able toorespond tooa ping from another VM in hello same VNet.</span></span> <span data-ttu-id="530c5-130">Se till att ett webbprogram för webbläsaren begäran toohello är användbar om konfigurerats med en offentlig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="530c5-130">If configured with a public end point, ensure that a browser request toohello web application is serviceable.</span></span>

## <a name="problems-with-default-health-probe"></a><span data-ttu-id="530c5-131">Problem med standard hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="530c5-131">Problems with default health probe</span></span>

### <a name="cause"></a><span data-ttu-id="530c5-132">Orsak</span><span class="sxs-lookup"><span data-stu-id="530c5-132">Cause</span></span>

<span data-ttu-id="530c5-133">502 fel kan också vara ofta indikatorer som hello standard hälsoavsökningen och är inte tooreach backend-virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="530c5-133">502 errors can also be frequent indicators that hello default health probe is not able tooreach back-end VMs.</span></span> <span data-ttu-id="530c5-134">När en Programgateway instans etableras, konfigurerar den automatiskt en standard hälsa avsökningen tooeach BackendAddressPool med hjälp av egenskaper för hello BackendHttpSetting.</span><span class="sxs-lookup"><span data-stu-id="530c5-134">When an Application Gateway instance is provisioned, it automatically configures a default health probe tooeach BackendAddressPool using properties of hello BackendHttpSetting.</span></span> <span data-ttu-id="530c5-135">Inga användarindata är obligatoriska tooset den här avsökningen.</span><span class="sxs-lookup"><span data-stu-id="530c5-135">No user input is required tooset this probe.</span></span> <span data-ttu-id="530c5-136">När en regel för belastningsutjämning konfigureras görs mer specifikt en association mellan en BackendHttpSetting och BackendAddressPool.</span><span class="sxs-lookup"><span data-stu-id="530c5-136">Specifically, when a load balancing rule is configured, an association is made between a BackendHttpSetting and BackendAddressPool.</span></span> <span data-ttu-id="530c5-137">En standard-avsökning har konfigurerats för var och en av dessa kopplingar och Programgateway initierar en regelbundna hälsokontroller Kontrollera anslutningen tooeach instans i hello BackendAddressPool på hello-porten som anges i hello BackendHttpSetting element.</span><span class="sxs-lookup"><span data-stu-id="530c5-137">A default probe is configured for each of these associations and Application Gateway initiates a periodic health check connection tooeach instance in hello BackendAddressPool at hello port specified in hello BackendHttpSetting element.</span></span> <span data-ttu-id="530c5-138">Följande tabell visar hello värden som är associerade med hello standard hälsoavsökningen.</span><span class="sxs-lookup"><span data-stu-id="530c5-138">Following table lists hello values associated with hello default health probe.</span></span>

| <span data-ttu-id="530c5-139">Avsökningen egenskapen</span><span class="sxs-lookup"><span data-stu-id="530c5-139">Probe property</span></span> | <span data-ttu-id="530c5-140">Värde</span><span class="sxs-lookup"><span data-stu-id="530c5-140">Value</span></span> | <span data-ttu-id="530c5-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="530c5-141">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="530c5-142">URL för webbavsökning</span><span class="sxs-lookup"><span data-stu-id="530c5-142">Probe URL</span></span> |<span data-ttu-id="530c5-143">http://127.0.0.1/</span><span class="sxs-lookup"><span data-stu-id="530c5-143">http://127.0.0.1/</span></span> |<span data-ttu-id="530c5-144">URL-sökväg</span><span class="sxs-lookup"><span data-stu-id="530c5-144">URL path</span></span> |
| <span data-ttu-id="530c5-145">intervall</span><span class="sxs-lookup"><span data-stu-id="530c5-145">Interval</span></span> |<span data-ttu-id="530c5-146">30</span><span class="sxs-lookup"><span data-stu-id="530c5-146">30</span></span> |<span data-ttu-id="530c5-147">Avsökningsintervall i sekunder</span><span class="sxs-lookup"><span data-stu-id="530c5-147">Probe interval in seconds</span></span> |
| <span data-ttu-id="530c5-148">Timeout</span><span class="sxs-lookup"><span data-stu-id="530c5-148">Time-out</span></span> |<span data-ttu-id="530c5-149">30</span><span class="sxs-lookup"><span data-stu-id="530c5-149">30</span></span> |<span data-ttu-id="530c5-150">Avsökningen tidsgräns i sekunder</span><span class="sxs-lookup"><span data-stu-id="530c5-150">Probe time-out in seconds</span></span> |
| <span data-ttu-id="530c5-151">Tröskelvärde för ohälsosamt värde</span><span class="sxs-lookup"><span data-stu-id="530c5-151">Unhealthy threshold</span></span> |<span data-ttu-id="530c5-152">3</span><span class="sxs-lookup"><span data-stu-id="530c5-152">3</span></span> |<span data-ttu-id="530c5-153">Avsökning för antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="530c5-153">Probe retry count.</span></span> <span data-ttu-id="530c5-154">hello backend-server markeras när hello efterföljande avsökningen Felberäkning når hello tröskelvärde för ohälsosamt värde.</span><span class="sxs-lookup"><span data-stu-id="530c5-154">hello back-end server is marked down after hello consecutive probe failure count reaches hello unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="530c5-155">Lösning</span><span class="sxs-lookup"><span data-stu-id="530c5-155">Solution</span></span>

* <span data-ttu-id="530c5-156">Se till att en standardwebbplats har konfigurerats och lyssnar på 127.0.0.1.</span><span class="sxs-lookup"><span data-stu-id="530c5-156">Ensure that a default site is configured and is listening at 127.0.0.1.</span></span>
* <span data-ttu-id="530c5-157">Om BackendHttpSetting anger en annan port än 80 vara hello standardwebbplats konfigurerade toolisten på denna port.</span><span class="sxs-lookup"><span data-stu-id="530c5-157">If BackendHttpSetting specifies a port other than 80, hello default site should be configured toolisten at that port.</span></span>
* <span data-ttu-id="530c5-158">hello ska anropet toohttp://127.0.0.1:port returnera ett HTTP-Resultatkod 200.</span><span class="sxs-lookup"><span data-stu-id="530c5-158">hello call toohttp://127.0.0.1:port should return an HTTP result code of 200.</span></span> <span data-ttu-id="530c5-159">Detta ska returneras inom tidsgränsen för hello 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="530c5-159">This should be returned within hello 30 sec time-out period.</span></span>
* <span data-ttu-id="530c5-160">Se till att konfigurerade porten är öppen och att det inte finns några brandväggsregler Azure Nätverkssäkerhetsgrupperna, som blockerar inkommande eller utgående trafik på hello konfigurerade porten.</span><span class="sxs-lookup"><span data-stu-id="530c5-160">Ensure that port configured is open and that there are no firewall rules or Azure Network Security Groups, which block incoming or outgoing traffic on hello port configured.</span></span>
* <span data-ttu-id="530c5-161">Om klassiska virtuella Azure-datorer eller tjänst i molnet används med FQDN eller offentlig IP-adress, kontrollerar du att hello motsvarande [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) öppnas.</span><span class="sxs-lookup"><span data-stu-id="530c5-161">If Azure classic VMs or Cloud Service is used with FQDN or Public IP, ensure that hello corresponding [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) is opened.</span></span>
* <span data-ttu-id="530c5-162">Om hello VM konfigureras via Azure Resource Manager och ligger utanför hello VNet där Application Gateway har distribuerats, [Nätverkssäkerhetsgruppen](../virtual-network/virtual-networks-nsg.md) måste konfigureras tooallow åtkomst på hello önskad port.</span><span class="sxs-lookup"><span data-stu-id="530c5-162">If hello VM is configured via Azure Resource Manager and is outside hello VNet where Application Gateway is deployed, [Network Security Group](../virtual-network/virtual-networks-nsg.md) must be configured tooallow access on hello desired port.</span></span>

## <a name="problems-with-custom-health-probe"></a><span data-ttu-id="530c5-163">Problem med anpassade hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="530c5-163">Problems with custom health probe</span></span>

### <a name="cause"></a><span data-ttu-id="530c5-164">Orsak</span><span class="sxs-lookup"><span data-stu-id="530c5-164">Cause</span></span>

<span data-ttu-id="530c5-165">Anpassade hälsoavsökningar kan ytterligare flexibilitet toohello standard sökning beteende.</span><span class="sxs-lookup"><span data-stu-id="530c5-165">Custom health probes allow additional flexibility toohello default probing behavior.</span></span> <span data-ttu-id="530c5-166">När du använder anpassade avsökningar kan användare konfigurera hello avsökningsintervall, hello URL, och sökvägen tootest och hur många misslyckade svar tooaccept innan du markerar hello backend-pool-instans som ohälsosamt.</span><span class="sxs-lookup"><span data-stu-id="530c5-166">When using custom probes, users can configure hello probe interval, hello URL, and path tootest, and how many failed responses tooaccept before marking hello back-end pool instance as unhealthy.</span></span> <span data-ttu-id="530c5-167">hello följande ytterligare egenskaper läggs till.</span><span class="sxs-lookup"><span data-stu-id="530c5-167">hello following additional properties are added.</span></span>

| <span data-ttu-id="530c5-168">Avsökningen egenskapen</span><span class="sxs-lookup"><span data-stu-id="530c5-168">Probe property</span></span> | <span data-ttu-id="530c5-169">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="530c5-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="530c5-170">Namn</span><span class="sxs-lookup"><span data-stu-id="530c5-170">Name</span></span> |<span data-ttu-id="530c5-171">Namnet på hello avsökning.</span><span class="sxs-lookup"><span data-stu-id="530c5-171">Name of hello probe.</span></span> <span data-ttu-id="530c5-172">Det här namnet är används toorefer toohello avsökning i backend-HTTP-inställningar.</span><span class="sxs-lookup"><span data-stu-id="530c5-172">This name is used toorefer toohello probe in back-end HTTP settings.</span></span> |
| <span data-ttu-id="530c5-173">Protokoll</span><span class="sxs-lookup"><span data-stu-id="530c5-173">Protocol</span></span> |<span data-ttu-id="530c5-174">Protokoll som används toosend hello avsökning.</span><span class="sxs-lookup"><span data-stu-id="530c5-174">Protocol used toosend hello probe.</span></span> <span data-ttu-id="530c5-175">hello avsökningen använder hello-protokollet som definieras i hello backend-HTTP-inställningar</span><span class="sxs-lookup"><span data-stu-id="530c5-175">hello probe uses hello protocol defined in hello back-end HTTP settings</span></span> |
| <span data-ttu-id="530c5-176">Värd</span><span class="sxs-lookup"><span data-stu-id="530c5-176">Host</span></span> |<span data-ttu-id="530c5-177">Värden namnet toosend hello avsökning.</span><span class="sxs-lookup"><span data-stu-id="530c5-177">Host name toosend hello probe.</span></span> <span data-ttu-id="530c5-178">Gäller endast när flera platser har konfigurerats på Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="530c5-178">Applicable only when multi-site is configured on Application Gateway.</span></span> <span data-ttu-id="530c5-179">Detta skiljer sig från den virtuella datorns värdnamn.</span><span class="sxs-lookup"><span data-stu-id="530c5-179">This is different from VM host name.</span></span> |
| <span data-ttu-id="530c5-180">Sökväg</span><span class="sxs-lookup"><span data-stu-id="530c5-180">Path</span></span> |<span data-ttu-id="530c5-181">Relativ sökväg hello avsökning.</span><span class="sxs-lookup"><span data-stu-id="530c5-181">Relative path of hello probe.</span></span> <span data-ttu-id="530c5-182">hello giltig sökväg börjar från '/'.</span><span class="sxs-lookup"><span data-stu-id="530c5-182">hello valid path starts from '/'.</span></span> <span data-ttu-id="530c5-183">hello avsökningen skickas för\<protokollet\>://\<värden\>:\<port\>\<sökväg\></span><span class="sxs-lookup"><span data-stu-id="530c5-183">hello probe is sent too\<protocol\>://\<host\>:\<port\>\<path\></span></span> |
| <span data-ttu-id="530c5-184">intervall</span><span class="sxs-lookup"><span data-stu-id="530c5-184">Interval</span></span> |<span data-ttu-id="530c5-185">Avsökning intervall i sekunder.</span><span class="sxs-lookup"><span data-stu-id="530c5-185">Probe interval in seconds.</span></span> <span data-ttu-id="530c5-186">Detta är hello tidsintervallet mellan två på varandra följande avsökningar.</span><span class="sxs-lookup"><span data-stu-id="530c5-186">This is hello time interval between two consecutive probes.</span></span> |
| <span data-ttu-id="530c5-187">Timeout</span><span class="sxs-lookup"><span data-stu-id="530c5-187">Time-out</span></span> |<span data-ttu-id="530c5-188">Avsökning tidsgräns i sekunder.</span><span class="sxs-lookup"><span data-stu-id="530c5-188">Probe time-out in seconds.</span></span> <span data-ttu-id="530c5-189">Om ett giltigt svar inte emot inom denna tidsgräns, markeras hello avsökningen som misslyckad.</span><span class="sxs-lookup"><span data-stu-id="530c5-189">If a valid response is not received within this time-out period, hello probe is marked as failed.</span></span> |
| <span data-ttu-id="530c5-190">Tröskelvärde för ohälsosamt värde</span><span class="sxs-lookup"><span data-stu-id="530c5-190">Unhealthy threshold</span></span> |<span data-ttu-id="530c5-191">Avsökning för antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="530c5-191">Probe retry count.</span></span> <span data-ttu-id="530c5-192">hello backend-server markeras när hello efterföljande avsökningen Felberäkning når hello tröskelvärde för ohälsosamt värde.</span><span class="sxs-lookup"><span data-stu-id="530c5-192">hello back-end server is marked down after hello consecutive probe failure count reaches hello unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="530c5-193">Lösning</span><span class="sxs-lookup"><span data-stu-id="530c5-193">Solution</span></span>

<span data-ttu-id="530c5-194">Verifiera att hello avsökning för anpassad hälsa är korrekt konfigurerad som hello föregående tabell.</span><span class="sxs-lookup"><span data-stu-id="530c5-194">Validate that hello Custom Health Probe is configured correctly as hello preceding table.</span></span> <span data-ttu-id="530c5-195">Dessutom toohello föregående felsökning, se också till hello följande:</span><span class="sxs-lookup"><span data-stu-id="530c5-195">In addition toohello preceding troubleshooting steps, also ensure hello following:</span></span>

* <span data-ttu-id="530c5-196">Kontrollera att hello-avsökning har angetts korrekt enligt hello [guiden](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="530c5-196">Ensure that hello probe is correctly specified as per hello [guide](application-gateway-create-probe-ps.md).</span></span>
* <span data-ttu-id="530c5-197">Om Application Gateway har konfigurerats för en viss plats, som standard hello värden ska namn anges som 127.0.0.1, såvida inte annat konfigurerade i anpassade avsökning.</span><span class="sxs-lookup"><span data-stu-id="530c5-197">If Application Gateway is configured for a single site, by default hello Host name should be specified as '127.0.0.1', unless otherwise configured in custom probe.</span></span>
* <span data-ttu-id="530c5-198">Se till att ett anrop toohttp: / /\<värden\>:\<port\>\<sökväg\> returnerar ett HTTP-Resultatkod 200.</span><span class="sxs-lookup"><span data-stu-id="530c5-198">Ensure that a call toohttp://\<host\>:\<port\>\<path\> returns an HTTP result code of 200.</span></span>
* <span data-ttu-id="530c5-199">Kontrollera att intervallet, timeout och UnhealtyThreshold är inom acceptabel hello-intervall.</span><span class="sxs-lookup"><span data-stu-id="530c5-199">Ensure that Interval, Time-out and UnhealtyThreshold are within hello acceptable ranges.</span></span>
* <span data-ttu-id="530c5-200">Om du använder en HTTPS-avsökning, se till att hello backend-servern kräver SNI genom att konfigurera ett fallback-certifikatet på hello backend-servern.</span><span class="sxs-lookup"><span data-stu-id="530c5-200">If using an HTTPS probe, make sure that hello backend server doesn't require SNI by configuring a fallback certificate on hello backend server itself.</span></span> 
* <span data-ttu-id="530c5-201">Kontrollera att intervallet, timeout och UnhealtyThreshold är inom acceptabel hello-intervall.</span><span class="sxs-lookup"><span data-stu-id="530c5-201">Ensure that Interval, Time-out, and UnhealtyThreshold are within hello acceptable ranges.</span></span>

## <a name="request-time-out"></a><span data-ttu-id="530c5-202">Tidsgräns för begäran</span><span class="sxs-lookup"><span data-stu-id="530c5-202">Request time out</span></span>

### <a name="cause"></a><span data-ttu-id="530c5-203">Orsak</span><span class="sxs-lookup"><span data-stu-id="530c5-203">Cause</span></span>

<span data-ttu-id="530c5-204">När en begäran tas emot Programgateway gäller hello konfigurerade regler toohello begäran och dirigerar den tooa backend-adresspool instans.</span><span class="sxs-lookup"><span data-stu-id="530c5-204">When a user request is received, Application Gateway applies hello configured rules toohello request and routes it tooa back-end pool instance.</span></span> <span data-ttu-id="530c5-205">Det väntar på en konfigurerbar tidsperiod på svar från hello backend-instans.</span><span class="sxs-lookup"><span data-stu-id="530c5-205">It waits for a configurable interval of time for a response from hello back-end instance.</span></span> <span data-ttu-id="530c5-206">Det här intervallet är som standard **30 sekunder**.</span><span class="sxs-lookup"><span data-stu-id="530c5-206">By default, this interval is **30 seconds**.</span></span> <span data-ttu-id="530c5-207">Om Application Gateway inte får ett svar från serverprogrammet i det här intervallet visas användarbegäran ett 502 fel.</span><span class="sxs-lookup"><span data-stu-id="530c5-207">If Application Gateway does not receive a response from back-end application in this interval, user request would see a 502 error.</span></span>

### <a name="solution"></a><span data-ttu-id="530c5-208">Lösning</span><span class="sxs-lookup"><span data-stu-id="530c5-208">Solution</span></span>

<span data-ttu-id="530c5-209">Programgateway kan användare tooconfigure inställningen via BackendHttpSetting som kan vara och sedan använda toodifferent pooler.</span><span class="sxs-lookup"><span data-stu-id="530c5-209">Application Gateway allows users tooconfigure this setting via BackendHttpSetting, which can be then applied toodifferent pools.</span></span> <span data-ttu-id="530c5-210">Olika backend-pooler kan ha olika BackendHttpSetting och därför olika begäran-timeout konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="530c5-210">Different back-end pools can have different BackendHttpSetting and hence different request time out configured.</span></span>

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a><span data-ttu-id="530c5-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="530c5-211">Next steps</span></span>

<span data-ttu-id="530c5-212">Om hello föregående stegen inte löser problemet hello kan öppna en [stöder biljett](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="530c5-212">If hello preceding steps do not resolve hello issue, open a [support ticket](https://azure.microsoft.com/support/options/).</span></span>

