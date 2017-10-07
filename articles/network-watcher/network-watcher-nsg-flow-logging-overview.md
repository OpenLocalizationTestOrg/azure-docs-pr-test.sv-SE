---
title: "aaaIntroduction tooflow loggning för Nätverkssäkerhetsgrupper med Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan förklarar hur toouse NSG flödet loggar en funktion i Azure Nätverksbevakaren"
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
ms.openlocfilehash: da85e946147b14717144cb47d1c742057c6dfa24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooflow-logging-for-network-security-groups"></a><span data-ttu-id="7b68d-103">Introduktion tooflow loggning för Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="7b68d-103">Introduction tooflow logging for Network Security Groups</span></span>

<span data-ttu-id="7b68d-104">Nätverkssäkerhetsgruppen flöde loggarna är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="7b68d-104">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="7b68d-105">Loggarna flödet skrivs i json-format och visa utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP-källan/målet Port Protocol) och om hello trafik tilläts eller nekad.</span><span class="sxs-lookup"><span data-stu-id="7b68d-105">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

![Översikt över flödet-loggar][1]

<span data-ttu-id="7b68d-107">Medan flödet loggar mål Nätverkssäkerhetsgrupper, de visas inte hello samma som hello andra loggar.</span><span class="sxs-lookup"><span data-stu-id="7b68d-107">While flow logs target Network Security Groups, they are not displayed hello same as hello other logs.</span></span> <span data-ttu-id="7b68d-108">Flödet loggfilerna lagras i ett lagringskonto och följande hello Loggningssökvägen som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="7b68d-108">Flow logs are stored only within a storage account and following hello logging path as shown in hello following example:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="7b68d-109">Hej samma bevarandeprinciper som det visas på andra loggar tillämpa tooflow loggar.</span><span class="sxs-lookup"><span data-stu-id="7b68d-109">hello same retention policies as seen on other logs apply tooflow logs.</span></span> <span data-ttu-id="7b68d-110">Loggar har en bevarandeprincip som kan anges från 1 dag too365 dagar.</span><span class="sxs-lookup"><span data-stu-id="7b68d-110">Logs have a retention policy that can be set from 1 day too365 days.</span></span> <span data-ttu-id="7b68d-111">Om en bevarandeprincip inte har angetts bibehålls hello loggar alltid.</span><span class="sxs-lookup"><span data-stu-id="7b68d-111">If a retention policy is not set, hello logs are maintained forever.</span></span>

## <a name="log-file"></a><span data-ttu-id="7b68d-112">Loggfil</span><span class="sxs-lookup"><span data-stu-id="7b68d-112">Log file</span></span>

<span data-ttu-id="7b68d-113">Flödet loggar har flera egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7b68d-113">Flow logs have multiple properties.</span></span> <span data-ttu-id="7b68d-114">hello finns följande lista en lista över hello egenskaperna som returneras i hello NSG flödet loggen:</span><span class="sxs-lookup"><span data-stu-id="7b68d-114">hello following list is a listing of hello properties that are returned within hello NSG flow log:</span></span>

* <span data-ttu-id="7b68d-115">**tid** - tid när hello händelsen loggades</span><span class="sxs-lookup"><span data-stu-id="7b68d-115">**time** - Time when hello event was logged</span></span>
* <span data-ttu-id="7b68d-116">**system-ID** -Nätverkssäkerhetsgruppen resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="7b68d-116">**systemId** - Network Security Group resource Id.</span></span>
* <span data-ttu-id="7b68d-117">**kategori** -hello kategori för hello händelse, är det alltid vara NetworkSecurityGroupFlowEvent</span><span class="sxs-lookup"><span data-stu-id="7b68d-117">**category** - hello category of hello event, this is always be NetworkSecurityGroupFlowEvent</span></span>
* <span data-ttu-id="7b68d-118">**ResourceId** -hello resurs-Id för hello NSG</span><span class="sxs-lookup"><span data-stu-id="7b68d-118">**resourceid** - hello resource Id of hello NSG</span></span>
* <span data-ttu-id="7b68d-119">**operationName** -alltid NetworkSecurityGroupFlowEvents</span><span class="sxs-lookup"><span data-stu-id="7b68d-119">**operationName** - Always NetworkSecurityGroupFlowEvents</span></span>
* <span data-ttu-id="7b68d-120">**Egenskaper för** – en samling av egenskaper i hello-flöde</span><span class="sxs-lookup"><span data-stu-id="7b68d-120">**properties** - A collection of properties of hello flow</span></span>
    * <span data-ttu-id="7b68d-121">**Version** -versionsnumret för hello logga Flow Händelseschema</span><span class="sxs-lookup"><span data-stu-id="7b68d-121">**Version** - Version number of hello Flow Log event schema</span></span>
    * <span data-ttu-id="7b68d-122">**flöden** – en samling av flöden.</span><span class="sxs-lookup"><span data-stu-id="7b68d-122">**flows** - A collection of flows.</span></span> <span data-ttu-id="7b68d-123">Den här egenskapen har flera poster för olika regler</span><span class="sxs-lookup"><span data-stu-id="7b68d-123">This property has multiple entries for different rules</span></span>
        * <span data-ttu-id="7b68d-124">**regeln** -regel för vilka hello flöden visas</span><span class="sxs-lookup"><span data-stu-id="7b68d-124">**rule** - Rule for which hello flows are listed</span></span>
            * <span data-ttu-id="7b68d-125">**flöden** – en samling av flöden</span><span class="sxs-lookup"><span data-stu-id="7b68d-125">**flows** - a collection of flows</span></span>
                * <span data-ttu-id="7b68d-126">**Mac** -hello MAC-adressen för hello NIC för hello VM där hello flödet har hämtats</span><span class="sxs-lookup"><span data-stu-id="7b68d-126">**mac** - hello MAC address of hello NIC for hello VM where hello flow was collected</span></span>
                * <span data-ttu-id="7b68d-127">**flowTuples** -en sträng som innehåller flera egenskaper för hello flödet tuppel i CSV-format</span><span class="sxs-lookup"><span data-stu-id="7b68d-127">**flowTuples** - A string that contains multiple properties for hello flow tuple in comma-separated format</span></span>
                    * <span data-ttu-id="7b68d-128">**Tidsstämpel** -värdet är hello tidsstämpeln för när hello flödet uppstod i UNIX EPOK format</span><span class="sxs-lookup"><span data-stu-id="7b68d-128">**Time Stamp** - This value is hello time stamp of when hello flow occurred in UNIX EPOCH format</span></span>
                    * <span data-ttu-id="7b68d-129">**Käll-IP** - hello käll-IP</span><span class="sxs-lookup"><span data-stu-id="7b68d-129">**Source IP** - hello source IP</span></span>
                    * <span data-ttu-id="7b68d-130">**Mål IP** -hello mål-IP</span><span class="sxs-lookup"><span data-stu-id="7b68d-130">**Destination IP** - hello destination IP</span></span>
                    * <span data-ttu-id="7b68d-131">**Källport** - hello källport</span><span class="sxs-lookup"><span data-stu-id="7b68d-131">**Source Port** - hello source port</span></span>
                    * <span data-ttu-id="7b68d-132">**Målport** -hello målport</span><span class="sxs-lookup"><span data-stu-id="7b68d-132">**Destination Port** - hello destination Port</span></span>
                    * <span data-ttu-id="7b68d-133">**Protokollet** -hello protokollet för hello flödet.</span><span class="sxs-lookup"><span data-stu-id="7b68d-133">**Protocol** - hello protocol of hello flow.</span></span> <span data-ttu-id="7b68d-134">Giltiga värden är **T** för TCP och **U** för UDP</span><span class="sxs-lookup"><span data-stu-id="7b68d-134">Valid values are **T** for TCP and **U** for UDP</span></span>
                    * <span data-ttu-id="7b68d-135">**Infrastrukturtrafiken rör** -hello flödesriktning hello trafik.</span><span class="sxs-lookup"><span data-stu-id="7b68d-135">**Traffic Flow** - hello direction of hello traffic flow.</span></span> <span data-ttu-id="7b68d-136">Giltiga värden är **jag** för inkommande och **O** för utgående.</span><span class="sxs-lookup"><span data-stu-id="7b68d-136">Valid values are **I** for inbound and **O** for outbound.</span></span>
                    * <span data-ttu-id="7b68d-137">**Trafik** – oavsett om trafik tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="7b68d-137">**Traffic** - Whether traffic was allowed or denied.</span></span> <span data-ttu-id="7b68d-138">Giltiga värden är **A** för tillåtna och **D** för nekad.</span><span class="sxs-lookup"><span data-stu-id="7b68d-138">Valid values are **A** for allowed and **D** for denied.</span></span>


<span data-ttu-id="7b68d-139">hello följande är ett exempel på en logg i flödet.</span><span class="sxs-lookup"><span data-stu-id="7b68d-139">hello following is an example of a Flow log.</span></span> <span data-ttu-id="7b68d-140">Som du kan se att det finns flera poster som följer beskrivs i föregående avsnitt hello hello-egenskapslistan.</span><span class="sxs-lookup"><span data-stu-id="7b68d-140">As you can see there are multiple records that follow hello property list described in hello preceding section.</span></span> 

> [!NOTE]
> <span data-ttu-id="7b68d-141">Värdena i hello flowTuples egenskapen är en kommaavgränsad lista.</span><span class="sxs-lookup"><span data-stu-id="7b68d-141">Values in hello flowTuples property are a comma-separated list.</span></span>
 
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

## <a name="next-steps"></a><span data-ttu-id="7b68d-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7b68d-142">Next steps</span></span>

<span data-ttu-id="7b68d-143">Lär dig hur tooenable flöde loggar genom att besöka [aktiverar flöda loggning](network-watcher-nsg-flow-logging-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7b68d-143">Learn how tooenable Flow logs by visiting [Enabling Flow logging](network-watcher-nsg-flow-logging-portal.md).</span></span>

<span data-ttu-id="7b68d-144">Lär dig mer om NSG loggning genom att besöka [logga analytics för nätverkssäkerhetsgrupper (NSG: er)](../virtual-network/virtual-network-nsg-manage-log.md).</span><span class="sxs-lookup"><span data-stu-id="7b68d-144">Learn about NSG logging by visiting [Log analytics for network security groups (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md).</span></span>

<span data-ttu-id="7b68d-145">Ta reda på om trafik tillåts eller nekas på en virtuell dator genom att besöka [Kontrollera Kontrollera trafik med IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7b68d-145">Find out if traffic is allowed or denied on a VM by visiting [Verify traffic with IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

