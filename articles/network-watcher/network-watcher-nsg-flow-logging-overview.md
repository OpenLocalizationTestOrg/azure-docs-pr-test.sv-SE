---
title: "Introduktion till flödet loggning för Nätverkssäkerhetsgrupper med Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan förklarar hur du använder NSG flödet loggar en funktion i Azure Nätverksbevakaren"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b7a9162d6c6219b6b1c51a49cd34b9616e9d3e8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a><span data-ttu-id="7a81e-103">Introduktion till flödet loggning för Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="7a81e-103">Introduction to flow logging for Network Security Groups</span></span>

<span data-ttu-id="7a81e-104">Nätverkssäkerhetsgruppen flöde loggarna är en funktion i Nätverksbevakaren där du kan visa information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="7a81e-104">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="7a81e-105">Loggarna flödet skrivs i json-format och visa utgående och inkommande flöden på grundval av per regel, NIC flödet gäller för 5-tuppel information om flödet (källan/målet IP-källan/målet Port Protocol), och om trafiken tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="7a81e-105">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

![Översikt över flödet-loggar][1]

<span data-ttu-id="7a81e-107">Medan flödet loggar mål Nätverkssäkerhetsgrupper, som inte visas samma som de andra loggarna.</span><span class="sxs-lookup"><span data-stu-id="7a81e-107">While flow logs target Network Security Groups, they are not displayed the same as the other logs.</span></span> <span data-ttu-id="7a81e-108">Flödet loggfilerna lagras i ett lagringskonto och följa Loggningssökvägen som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="7a81e-108">Flow logs are stored only within a storage account and following the logging path as shown in the following example:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="7a81e-109">Samma bevarandeprinciper som visas på andra loggar gäller flödet loggar.</span><span class="sxs-lookup"><span data-stu-id="7a81e-109">The same retention policies as seen on other logs apply to flow logs.</span></span> <span data-ttu-id="7a81e-110">Loggar har en bevarandeprincip som kan anges från dag 1 och 365 dagar.</span><span class="sxs-lookup"><span data-stu-id="7a81e-110">Logs have a retention policy that can be set from 1 day to 365 days.</span></span> <span data-ttu-id="7a81e-111">Om en bevarandeprincip inte är inställd bevaras loggarna för evigt.</span><span class="sxs-lookup"><span data-stu-id="7a81e-111">If a retention policy is not set, the logs are maintained forever.</span></span>

## <a name="log-file"></a><span data-ttu-id="7a81e-112">Loggfil</span><span class="sxs-lookup"><span data-stu-id="7a81e-112">Log file</span></span>

<span data-ttu-id="7a81e-113">Flödet loggar har flera egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7a81e-113">Flow logs have multiple properties.</span></span> <span data-ttu-id="7a81e-114">Nedan följer en lista över de egenskaper som returneras i loggen för NSG-flöde:</span><span class="sxs-lookup"><span data-stu-id="7a81e-114">The following list is a listing of the properties that are returned within the NSG flow log:</span></span>

* <span data-ttu-id="7a81e-115">**tid** - tid när händelsen loggades.</span><span class="sxs-lookup"><span data-stu-id="7a81e-115">**time** - Time when the event was logged</span></span>
* <span data-ttu-id="7a81e-116">**system-ID** -Nätverkssäkerhetsgruppen resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="7a81e-116">**systemId** - Network Security Group resource Id.</span></span>
* <span data-ttu-id="7a81e-117">**kategori** -kategorin av händelsen, är det alltid vara NetworkSecurityGroupFlowEvent</span><span class="sxs-lookup"><span data-stu-id="7a81e-117">**category** - The category of the event, this is always be NetworkSecurityGroupFlowEvent</span></span>
* <span data-ttu-id="7a81e-118">**ResourceId** -resurs-Id för NSG: N</span><span class="sxs-lookup"><span data-stu-id="7a81e-118">**resourceid** - The resource Id of the NSG</span></span>
* <span data-ttu-id="7a81e-119">**operationName** -alltid NetworkSecurityGroupFlowEvents</span><span class="sxs-lookup"><span data-stu-id="7a81e-119">**operationName** - Always NetworkSecurityGroupFlowEvents</span></span>
* <span data-ttu-id="7a81e-120">**Egenskaper för** – en samling av egenskaper i flöde</span><span class="sxs-lookup"><span data-stu-id="7a81e-120">**properties** - A collection of properties of the flow</span></span>
    * <span data-ttu-id="7a81e-121">**Version** -versionsnumret för Händelseschema flöda logg</span><span class="sxs-lookup"><span data-stu-id="7a81e-121">**Version** - Version number of the Flow Log event schema</span></span>
    * <span data-ttu-id="7a81e-122">**flöden** – en samling av flöden.</span><span class="sxs-lookup"><span data-stu-id="7a81e-122">**flows** - A collection of flows.</span></span> <span data-ttu-id="7a81e-123">Den här egenskapen har flera poster för olika regler</span><span class="sxs-lookup"><span data-stu-id="7a81e-123">This property has multiple entries for different rules</span></span>
        * <span data-ttu-id="7a81e-124">**regeln** -regel för flöden är listade</span><span class="sxs-lookup"><span data-stu-id="7a81e-124">**rule** - Rule for which the flows are listed</span></span>
            * <span data-ttu-id="7a81e-125">**flöden** – en samling av flöden</span><span class="sxs-lookup"><span data-stu-id="7a81e-125">**flows** - a collection of flows</span></span>
                * <span data-ttu-id="7a81e-126">**Mac** -MAC-adressen för nätverkskortet för den virtuella datorn där flödet har hämtats</span><span class="sxs-lookup"><span data-stu-id="7a81e-126">**mac** - The MAC address of the NIC for the VM where the flow was collected</span></span>
                * <span data-ttu-id="7a81e-127">**flowTuples** -en sträng som innehåller flera egenskaper för flöde tuppel i CSV-format</span><span class="sxs-lookup"><span data-stu-id="7a81e-127">**flowTuples** - A string that contains multiple properties for the flow tuple in comma-separated format</span></span>
                    * <span data-ttu-id="7a81e-128">**Tidsstämpel** -värdet är tidsstämpeln för när flödet uppstod i UNIX EPOK format</span><span class="sxs-lookup"><span data-stu-id="7a81e-128">**Time Stamp** - This value is the time stamp of when the flow occurred in UNIX EPOCH format</span></span>
                    * <span data-ttu-id="7a81e-129">**Käll-IP** -käll-IP</span><span class="sxs-lookup"><span data-stu-id="7a81e-129">**Source IP** - The source IP</span></span>
                    * <span data-ttu-id="7a81e-130">**Mål IP** -mål-IP</span><span class="sxs-lookup"><span data-stu-id="7a81e-130">**Destination IP** - The destination IP</span></span>
                    * <span data-ttu-id="7a81e-131">**Källport** -källport</span><span class="sxs-lookup"><span data-stu-id="7a81e-131">**Source Port** - The source port</span></span>
                    * <span data-ttu-id="7a81e-132">**Målport** -målet Port</span><span class="sxs-lookup"><span data-stu-id="7a81e-132">**Destination Port** - The destination Port</span></span>
                    * <span data-ttu-id="7a81e-133">**Protokollet** -protokollet för flödet.</span><span class="sxs-lookup"><span data-stu-id="7a81e-133">**Protocol** - The protocol of the flow.</span></span> <span data-ttu-id="7a81e-134">Giltiga värden är **T** för TCP och **U** för UDP</span><span class="sxs-lookup"><span data-stu-id="7a81e-134">Valid values are **T** for TCP and **U** for UDP</span></span>
                    * <span data-ttu-id="7a81e-135">**Infrastrukturtrafiken rör** -riktningen för trafikflödet.</span><span class="sxs-lookup"><span data-stu-id="7a81e-135">**Traffic Flow** - The direction of the traffic flow.</span></span> <span data-ttu-id="7a81e-136">Giltiga värden är **jag** för inkommande och **O** för utgående.</span><span class="sxs-lookup"><span data-stu-id="7a81e-136">Valid values are **I** for inbound and **O** for outbound.</span></span>
                    * <span data-ttu-id="7a81e-137">**Trafik** – oavsett om trafik tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="7a81e-137">**Traffic** - Whether traffic was allowed or denied.</span></span> <span data-ttu-id="7a81e-138">Giltiga värden är **A** för tillåtna och **D** för nekad.</span><span class="sxs-lookup"><span data-stu-id="7a81e-138">Valid values are **A** for allowed and **D** for denied.</span></span>


<span data-ttu-id="7a81e-139">Följande är ett exempel på en logg i flödet.</span><span class="sxs-lookup"><span data-stu-id="7a81e-139">The following is an example of a Flow log.</span></span> <span data-ttu-id="7a81e-140">Som du kan se att det finns flera poster som följer egenskapslistan som beskrivs i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7a81e-140">As you can see there are multiple records that follow the property list described in the preceding section.</span></span> 

> [!NOTE]
> <span data-ttu-id="7a81e-141">Värden i egenskapen flowTuples är en kommaavgränsad lista.</span><span class="sxs-lookup"><span data-stu-id="7a81e-141">Values in the flowTuples property are a comma-separated list.</span></span>
 
```json
{
    "records": 
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a><span data-ttu-id="7a81e-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a81e-142">Next steps</span></span>

<span data-ttu-id="7a81e-143">Lär dig hur du aktiverar flödet loggarna genom att besöka [aktiverar flöda loggning](network-watcher-nsg-flow-logging-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7a81e-143">Learn how to enable Flow logs by visiting [Enabling Flow logging](network-watcher-nsg-flow-logging-portal.md).</span></span>

<span data-ttu-id="7a81e-144">Lär dig mer om NSG loggning genom att besöka [logga analytics för nätverkssäkerhetsgrupper (NSG: er)](../virtual-network/virtual-network-nsg-manage-log.md).</span><span class="sxs-lookup"><span data-stu-id="7a81e-144">Learn about NSG logging by visiting [Log analytics for network security groups (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md).</span></span>

<span data-ttu-id="7a81e-145">Ta reda på om trafik tillåts eller nekas på en virtuell dator genom att besöka [Kontrollera Kontrollera trafik med IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7a81e-145">Find out if traffic is allowed or denied on a VM by visiting [Verify traffic with IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

