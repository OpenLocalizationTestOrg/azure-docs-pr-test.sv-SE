---
title: ExpressRoute kunden router configuration prover | Microsoft Docs
description: "Den här sidan innehåller router config-exempel för Cisco och Juniper routrar."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 564826bc-017a-4683-a385-37c9fa814948
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: 032e584dc5abf59e9e3e8d80673b402f1fbf721b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a><span data-ttu-id="63e2a-103">Router configuration exempel för att konfigurera och hantera routning</span><span class="sxs-lookup"><span data-stu-id="63e2a-103">Router configuration samples to set up and manage routing</span></span>
<span data-ttu-id="63e2a-104">Den här sidan innehåller gränssnitt och routning configuration-exempel för Cisco IOS-XE och Juniper MX serien routrar.</span><span class="sxs-lookup"><span data-stu-id="63e2a-104">This page provides interface and routing configuration samples for Cisco IOS-XE and Juniper MX series routers.</span></span> <span data-ttu-id="63e2a-105">Dessa är avsedda att prover för endast vägledning och får inte användas eftersom.</span><span class="sxs-lookup"><span data-stu-id="63e2a-105">These are intended to be samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="63e2a-106">Du kan arbeta med leverantören för att få fram lämpliga konfigurationerna för nätverket.</span><span class="sxs-lookup"><span data-stu-id="63e2a-106">You can work with your vendor to come up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="63e2a-107">Exempel på den här sidan är avsedda att vara rent anvisningar.</span><span class="sxs-lookup"><span data-stu-id="63e2a-107">Samples in this page are intended to be purely for guidance.</span></span> <span data-ttu-id="63e2a-108">Du måste arbeta med leverantörens försäljning / tekniska teamet och teamet nätverk för att få fram lämpliga konfigurationer för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="63e2a-108">You must work with your vendor's sales / technical team and your networking team to come up with appropriate configurations to meet your needs.</span></span> <span data-ttu-id="63e2a-109">Microsoft stöder inte problem relaterade till konfigurationer som anges i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="63e2a-109">Microsoft will not support issues related to configurations listed in this page.</span></span> <span data-ttu-id="63e2a-110">För att lösa problem måste du kontakta enhetsleverantören av.</span><span class="sxs-lookup"><span data-stu-id="63e2a-110">You must contact your device vendor for support issues.</span></span>
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a><span data-ttu-id="63e2a-111">Inställningarna för MTU och TCP-MSS på router-gränssnitt</span><span class="sxs-lookup"><span data-stu-id="63e2a-111">MTU and TCP MSS settings on router interfaces</span></span>
* <span data-ttu-id="63e2a-112">MTU för ExpressRoute-gränssnittet är 1500, som är typiska standard-MTU för ett Ethernet-gränssnitt på en router.</span><span class="sxs-lookup"><span data-stu-id="63e2a-112">The MTU for the ExpressRoute interface is 1500, which is the typical default MTU for an Ethernet interface on a router.</span></span> <span data-ttu-id="63e2a-113">Om routern har en annan MTU som standard, finns det inget behov av att ange ett värde på router-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="63e2a-113">Unless your router has a different MTU by default, there is no need to specify a value on the router interface.</span></span>
* <span data-ttu-id="63e2a-114">Till skillnad från en Azure VPN-Gateway behöver inte TCP-MSS för en ExpressRoute-krets anges.</span><span class="sxs-lookup"><span data-stu-id="63e2a-114">Unlike an Azure VPN Gateway, the TCP MSS for an ExpressRoute circuit does not need to be specified.</span></span>

<span data-ttu-id="63e2a-115">Router configuration exemplen nedan gäller för alla peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="63e2a-115">Router configuration samples below apply to all peerings.</span></span> <span data-ttu-id="63e2a-116">Granska [ExpressRoute peerkopplingar](expressroute-circuit-peerings.md) och [ExpressRoute routningskrav](expressroute-routing.md) för mer information om routning.</span><span class="sxs-lookup"><span data-stu-id="63e2a-116">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute routing requirements](expressroute-routing.md) for more details on routing.</span></span>


## <a name="cisco-ios-xe-based-routers"></a><span data-ttu-id="63e2a-117">Cisco IOS-XE baserat routrar</span><span class="sxs-lookup"><span data-stu-id="63e2a-117">Cisco IOS-XE based routers</span></span>
<span data-ttu-id="63e2a-118">Exemplen i det här avsnittet gäller för en router som kör IOS XE OS-familjen.</span><span class="sxs-lookup"><span data-stu-id="63e2a-118">The samples in this section apply for any router running the IOS-XE OS family.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="63e2a-119">1. Konfigurera och underordnade gränssnitt</span><span class="sxs-lookup"><span data-stu-id="63e2a-119">1. Configuring interfaces and sub-interfaces</span></span>
<span data-ttu-id="63e2a-120">Du behöver en sub-gränssnittet per peering i varje router som du ansluter till Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63e2a-120">You will require a sub interface per peering in every router you connect to Microsoft.</span></span> <span data-ttu-id="63e2a-121">En sub-gränssnittet kan identifieras med ett VLAN-ID eller ett staplat par med VLAN-ID och en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="63e2a-121">A sub interface can be identified with a VLAN ID or a stacked pair of VLAN IDs and an IP address.</span></span>

<span data-ttu-id="63e2a-122">**Dot1Q gränssnittsdefinition**</span><span class="sxs-lookup"><span data-stu-id="63e2a-122">**Dot1Q interface definition**</span></span>

<span data-ttu-id="63e2a-123">Det här exemplet innehåller underordnade gränssnittsdefinitionen för en underordnad gränssnittet med ett enda VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="63e2a-123">This sample provides the sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="63e2a-124">VLAN-ID är unikt för varje peering.</span><span class="sxs-lookup"><span data-stu-id="63e2a-124">The VLAN ID is unique per peering.</span></span> <span data-ttu-id="63e2a-125">Den sista oktetten av IPv4-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="63e2a-125">The last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

<span data-ttu-id="63e2a-126">**QinQ gränssnittsdefinition**</span><span class="sxs-lookup"><span data-stu-id="63e2a-126">**QinQ interface definition**</span></span>

<span data-ttu-id="63e2a-127">Det här exemplet innehåller underordnade gränssnittsdefinition för en underordnad gränssnitt med två VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="63e2a-127">This sample provides the sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="63e2a-128">Yttre VLAN-ID (s-tagg), om inte över alla peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="63e2a-128">The outer VLAN ID (s-tag), if used remains the same across all the peerings.</span></span> <span data-ttu-id="63e2a-129">Inre VLAN-ID (c-tagg) är unik för varje peering.</span><span class="sxs-lookup"><span data-stu-id="63e2a-129">The inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="63e2a-130">Den sista oktetten av IPv4-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="63e2a-130">The last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="63e2a-131">2. Ställa in eBGP-sessioner</span><span class="sxs-lookup"><span data-stu-id="63e2a-131">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="63e2a-132">Du måste konfigurera en BGP-session med Microsoft för varje peering.</span><span class="sxs-lookup"><span data-stu-id="63e2a-132">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="63e2a-133">Exemplet nedan kan du konfigurera en BGP-session med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63e2a-133">The sample below enables you to setup a BGP session with Microsoft.</span></span> <span data-ttu-id="63e2a-134">Om IPv4-adress som du använde för sub-gränssnittet har a.b.c.d, blir a.b.c.d+1 med IP-adressen för BGP-Grannens (Microsoft).</span><span class="sxs-lookup"><span data-stu-id="63e2a-134">If the IPv4 address you used for your sub interface was a.b.c.d, the IP address of the BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="63e2a-135">Den sista oktetten av IPv4-adress för BGP-Grannens kommer alltid att ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="63e2a-135">The last octet of the BGP neighbor's IPv4 address will always be an even number.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a><span data-ttu-id="63e2a-136">3. Konfigurera prefix för annonseras över BGP-sessionen</span><span class="sxs-lookup"><span data-stu-id="63e2a-136">3. Setting up prefixes to be advertised over the BGP session</span></span>
<span data-ttu-id="63e2a-137">Du kan konfigurera routern att annonsera väljer prefix till Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63e2a-137">You can configure your router to advertise select prefixes to Microsoft.</span></span> <span data-ttu-id="63e2a-138">Du kan göra det med hjälp av exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="63e2a-138">You can do so using the sample below.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a><span data-ttu-id="63e2a-139">4. Väg maps</span><span class="sxs-lookup"><span data-stu-id="63e2a-139">4. Route maps</span></span>
<span data-ttu-id="63e2a-140">Du kan använda vägen maps och prefix som visar filter prefix sprids till nätverket.</span><span class="sxs-lookup"><span data-stu-id="63e2a-140">You can use route-maps and prefix lists to filter prefixes propagated into your network.</span></span> <span data-ttu-id="63e2a-141">Du kan använda exemplet nedan för att utföra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="63e2a-141">You can use the sample below to accomplish the task.</span></span> <span data-ttu-id="63e2a-142">Se till att du har rätt prefix listor installationen.</span><span class="sxs-lookup"><span data-stu-id="63e2a-142">Ensure that you have appropriate prefix lists setup.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a><span data-ttu-id="63e2a-143">Juniper MX serien routrar</span><span class="sxs-lookup"><span data-stu-id="63e2a-143">Juniper MX series routers</span></span>
<span data-ttu-id="63e2a-144">Exemplen i det här avsnittet gäller för alla serier Juniper MX-routrar.</span><span class="sxs-lookup"><span data-stu-id="63e2a-144">The samples in this section apply for any Juniper MX series routers.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="63e2a-145">1. Konfigurera och underordnade gränssnitt</span><span class="sxs-lookup"><span data-stu-id="63e2a-145">1. Configuring interfaces and sub-interfaces</span></span>

<span data-ttu-id="63e2a-146">**Dot1Q gränssnittsdefinition**</span><span class="sxs-lookup"><span data-stu-id="63e2a-146">**Dot1Q interface definition**</span></span>

<span data-ttu-id="63e2a-147">Det här exemplet innehåller underordnade gränssnittsdefinitionen för en underordnad gränssnittet med ett enda VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="63e2a-147">This sample provides the sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="63e2a-148">VLAN-ID är unikt för varje peering.</span><span class="sxs-lookup"><span data-stu-id="63e2a-148">The VLAN ID is unique per peering.</span></span> <span data-ttu-id="63e2a-149">Den sista oktetten av IPv4-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="63e2a-149">The last octet of your IPv4 address will always be an odd number.</span></span>

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


<span data-ttu-id="63e2a-150">**QinQ gränssnittsdefinition**</span><span class="sxs-lookup"><span data-stu-id="63e2a-150">**QinQ interface definition**</span></span>

<span data-ttu-id="63e2a-151">Det här exemplet innehåller underordnade gränssnittsdefinition för en underordnad gränssnitt med två VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="63e2a-151">This sample provides the sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="63e2a-152">Yttre VLAN-ID (s-tagg), om inte över alla peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="63e2a-152">The outer VLAN ID (s-tag), if used remains the same across all the peerings.</span></span> <span data-ttu-id="63e2a-153">Inre VLAN-ID (c-tagg) är unik för varje peering.</span><span class="sxs-lookup"><span data-stu-id="63e2a-153">The inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="63e2a-154">Den sista oktetten av IPv4-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="63e2a-154">The last octet of your IPv4 address will always be an odd number.</span></span>

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="63e2a-155">2. Ställa in eBGP-sessioner</span><span class="sxs-lookup"><span data-stu-id="63e2a-155">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="63e2a-156">Du måste konfigurera en BGP-session med Microsoft för varje peering.</span><span class="sxs-lookup"><span data-stu-id="63e2a-156">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="63e2a-157">Exemplet nedan kan du konfigurera en BGP-session med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63e2a-157">The sample below enables you to setup a BGP session with Microsoft.</span></span> <span data-ttu-id="63e2a-158">Om IPv4-adress som du använde för sub-gränssnittet har a.b.c.d, blir a.b.c.d+1 med IP-adressen för BGP-Grannens (Microsoft).</span><span class="sxs-lookup"><span data-stu-id="63e2a-158">If the IPv4 address you used for your sub interface was a.b.c.d, the IP address of the BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="63e2a-159">Den sista oktetten av IPv4-adress för BGP-Grannens kommer alltid att ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="63e2a-159">The last octet of the BGP neighbor's IPv4 address will always be an even number.</span></span>

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a><span data-ttu-id="63e2a-160">3. Konfigurera prefix för annonseras över BGP-sessionen</span><span class="sxs-lookup"><span data-stu-id="63e2a-160">3. Setting up prefixes to be advertised over the BGP session</span></span>
<span data-ttu-id="63e2a-161">Du kan konfigurera routern att annonsera väljer prefix till Microsoft.</span><span class="sxs-lookup"><span data-stu-id="63e2a-161">You can configure your router to advertise select prefixes to Microsoft.</span></span> <span data-ttu-id="63e2a-162">Du kan göra det med hjälp av exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="63e2a-162">You can do so using the sample below.</span></span>

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a><span data-ttu-id="63e2a-163">4. Väg maps</span><span class="sxs-lookup"><span data-stu-id="63e2a-163">4. Route maps</span></span>
<span data-ttu-id="63e2a-164">Du kan använda vägen maps och prefix som visar filter prefix sprids till nätverket.</span><span class="sxs-lookup"><span data-stu-id="63e2a-164">You can use route-maps and prefix lists to filter prefixes propagated into your network.</span></span> <span data-ttu-id="63e2a-165">Du kan använda exemplet nedan för att utföra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="63e2a-165">You can use the sample below to accomplish the task.</span></span> <span data-ttu-id="63e2a-166">Se till att du har rätt prefix listor installationen.</span><span class="sxs-lookup"><span data-stu-id="63e2a-166">Ensure that you have appropriate prefix lists setup.</span></span>

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a><span data-ttu-id="63e2a-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="63e2a-167">Next Steps</span></span>
<span data-ttu-id="63e2a-168">Se [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="63e2a-168">See the [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>

