---
title: aaaExpressRoute kunden router configuration exempel | Microsoft Docs
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
ms.openlocfilehash: 5c91f24e6082e01c3e8df91b4fcfda46a6c29fa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="router-configuration-samples-tooset-up-and-manage-routing"></a><span data-ttu-id="b9b82-103">Routerkonfiguration exempel tooset in och hantera routning</span><span class="sxs-lookup"><span data-stu-id="b9b82-103">Router configuration samples tooset up and manage routing</span></span>
<span data-ttu-id="b9b82-104">Den här sidan innehåller gränssnitt och routning configuration-exempel för Cisco IOS-XE och Juniper MX serien routrar.</span><span class="sxs-lookup"><span data-stu-id="b9b82-104">This page provides interface and routing configuration samples for Cisco IOS-XE and Juniper MX series routers.</span></span> <span data-ttu-id="b9b82-105">Dessa är avsedda toobe prover för endast vägledning och får inte användas eftersom.</span><span class="sxs-lookup"><span data-stu-id="b9b82-105">These are intended toobe samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="b9b82-106">Du kan arbeta med din leverantör toocome med lämpliga konfigurationer för nätverket.</span><span class="sxs-lookup"><span data-stu-id="b9b82-106">You can work with your vendor toocome up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b9b82-107">Exempel på den här sidan är avsedda toobe rent anvisningar.</span><span class="sxs-lookup"><span data-stu-id="b9b82-107">Samples in this page are intended toobe purely for guidance.</span></span> <span data-ttu-id="b9b82-108">Du måste arbeta med leverantörens försäljning / tekniska teamet och ditt nätverk team toocome upp med lämpliga konfigurationer toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="b9b82-108">You must work with your vendor's sales / technical team and your networking team toocome up with appropriate configurations toomeet your needs.</span></span> <span data-ttu-id="b9b82-109">Microsoft stöder inte problem relaterade tooconfigurations som anges i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="b9b82-109">Microsoft will not support issues related tooconfigurations listed in this page.</span></span> <span data-ttu-id="b9b82-110">För att lösa problem måste du kontakta enhetsleverantören av.</span><span class="sxs-lookup"><span data-stu-id="b9b82-110">You must contact your device vendor for support issues.</span></span>
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a><span data-ttu-id="b9b82-111">Inställningarna för MTU och TCP-MSS på router-gränssnitt</span><span class="sxs-lookup"><span data-stu-id="b9b82-111">MTU and TCP MSS settings on router interfaces</span></span>
* <span data-ttu-id="b9b82-112">hello MTU för hello ExpressRoute-gränssnittet är 1500, som är typiska standard MTU-hello för Ethernet-gränssnitt på en router.</span><span class="sxs-lookup"><span data-stu-id="b9b82-112">hello MTU for hello ExpressRoute interface is 1500, which is hello typical default MTU for an Ethernet interface on a router.</span></span> <span data-ttu-id="b9b82-113">Om routern har en annan MTU som standard, finns det inget behov av toospecify ett värde på hello router-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="b9b82-113">Unless your router has a different MTU by default, there is no need toospecify a value on hello router interface.</span></span>
* <span data-ttu-id="b9b82-114">Till skillnad från en Azure VPN-Gateway behöver inte hello TCP-MSS för en ExpressRoute-krets toobe anges.</span><span class="sxs-lookup"><span data-stu-id="b9b82-114">Unlike an Azure VPN Gateway, hello TCP MSS for an ExpressRoute circuit does not need toobe specified.</span></span>

<span data-ttu-id="b9b82-115">Router configuration exemplen nedan gäller tooall peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="b9b82-115">Router configuration samples below apply tooall peerings.</span></span> <span data-ttu-id="b9b82-116">Granska [ExpressRoute peerkopplingar](expressroute-circuit-peerings.md) och [ExpressRoute routningskrav](expressroute-routing.md) för mer information om routning.</span><span class="sxs-lookup"><span data-stu-id="b9b82-116">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute routing requirements](expressroute-routing.md) for more details on routing.</span></span>


## <a name="cisco-ios-xe-based-routers"></a><span data-ttu-id="b9b82-117">Cisco IOS-XE baserat routrar</span><span class="sxs-lookup"><span data-stu-id="b9b82-117">Cisco IOS-XE based routers</span></span>
<span data-ttu-id="b9b82-118">hello prover i det här avsnittet gäller för alla hello IOS XE OS-familjen-routern.</span><span class="sxs-lookup"><span data-stu-id="b9b82-118">hello samples in this section apply for any router running hello IOS-XE OS family.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="b9b82-119">1. Konfigurera och underordnade gränssnitt</span><span class="sxs-lookup"><span data-stu-id="b9b82-119">1. Configuring interfaces and sub-interfaces</span></span>
<span data-ttu-id="b9b82-120">Du behöver en sub-gränssnittet per peering i varje router ansluta tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="b9b82-120">You will require a sub interface per peering in every router you connect tooMicrosoft.</span></span> <span data-ttu-id="b9b82-121">En sub-gränssnittet kan identifieras med ett VLAN-ID eller ett staplat par med VLAN-ID och en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b9b82-121">A sub interface can be identified with a VLAN ID or a stacked pair of VLAN IDs and an IP address.</span></span>

<span data-ttu-id="b9b82-122">**Dot1Q gränssnittsdefinition**</span><span class="sxs-lookup"><span data-stu-id="b9b82-122">**Dot1Q interface definition**</span></span>

<span data-ttu-id="b9b82-123">Det här exemplet innehåller hello underordnade gränssnittsdefinition för en underordnad gränssnittet med ett enda VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="b9b82-123">This sample provides hello sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="b9b82-124">hello VLAN-ID är unikt för varje peering.</span><span class="sxs-lookup"><span data-stu-id="b9b82-124">hello VLAN ID is unique per peering.</span></span> <span data-ttu-id="b9b82-125">hello sista oktetten av IPv4-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="b9b82-125">hello last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

<span data-ttu-id="b9b82-126">**QinQ gränssnittsdefinition**</span><span class="sxs-lookup"><span data-stu-id="b9b82-126">**QinQ interface definition**</span></span>

<span data-ttu-id="b9b82-127">Det här exemplet innehåller hello underordnade gränssnittsdefinition för en underordnad gränssnitt med två VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="b9b82-127">This sample provides hello sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="b9b82-128">hello yttre VLAN-ID (s-tagg), om används förblir hello samma över alla hello peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="b9b82-128">hello outer VLAN ID (s-tag), if used remains hello same across all hello peerings.</span></span> <span data-ttu-id="b9b82-129">hello inre VLAN-ID (c-tagg) är unik för varje peering.</span><span class="sxs-lookup"><span data-stu-id="b9b82-129">hello inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="b9b82-130">hello sista oktetten av IPv4-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="b9b82-130">hello last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="b9b82-131">2. Ställa in eBGP-sessioner</span><span class="sxs-lookup"><span data-stu-id="b9b82-131">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="b9b82-132">Du måste konfigurera en BGP-session med Microsoft för varje peering.</span><span class="sxs-lookup"><span data-stu-id="b9b82-132">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="b9b82-133">hello exemplet nedan kan du toosetup en BGP-session med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9b82-133">hello sample below enables you toosetup a BGP session with Microsoft.</span></span> <span data-ttu-id="b9b82-134">Om hello IPv4-adress som du använde för sub-gränssnittet var a.b.c.d, kommer hello IP-adressen hello BGP-Grannens (Microsoft) att a.b.c.d+1.</span><span class="sxs-lookup"><span data-stu-id="b9b82-134">If hello IPv4 address you used for your sub interface was a.b.c.d, hello IP address of hello BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="b9b82-135">hello sista oktetten av hello BGP granne IPv4-adress ska alltid vara ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="b9b82-135">hello last octet of hello BGP neighbor's IPv4 address will always be an even number.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a><span data-ttu-id="b9b82-136">3. Konfigurera prefix toobe annonserade över hello BGP-sessionen</span><span class="sxs-lookup"><span data-stu-id="b9b82-136">3. Setting up prefixes toobe advertised over hello BGP session</span></span>
<span data-ttu-id="b9b82-137">Du kan konfigurera din router tooadvertise väljer prefix tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="b9b82-137">You can configure your router tooadvertise select prefixes tooMicrosoft.</span></span> <span data-ttu-id="b9b82-138">Du kan göra det med hjälp av hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="b9b82-138">You can do so using hello sample below.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a><span data-ttu-id="b9b82-139">4. Väg maps</span><span class="sxs-lookup"><span data-stu-id="b9b82-139">4. Route maps</span></span>
<span data-ttu-id="b9b82-140">Du kan använda vägen maps och prefix visar toofilter prefix sprids till nätverket.</span><span class="sxs-lookup"><span data-stu-id="b9b82-140">You can use route-maps and prefix lists toofilter prefixes propagated into your network.</span></span> <span data-ttu-id="b9b82-141">Du kan använda hello exemplet nedan tooaccomplish hello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b9b82-141">You can use hello sample below tooaccomplish hello task.</span></span> <span data-ttu-id="b9b82-142">Se till att du har rätt prefix listor installationen.</span><span class="sxs-lookup"><span data-stu-id="b9b82-142">Ensure that you have appropriate prefix lists setup.</span></span>

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


## <a name="juniper-mx-series-routers"></a><span data-ttu-id="b9b82-143">Juniper MX serien routrar</span><span class="sxs-lookup"><span data-stu-id="b9b82-143">Juniper MX series routers</span></span>
<span data-ttu-id="b9b82-144">hello prover i det här avsnittet gäller för alla serier Juniper MX-routrar.</span><span class="sxs-lookup"><span data-stu-id="b9b82-144">hello samples in this section apply for any Juniper MX series routers.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="b9b82-145">1. Konfigurera och underordnade gränssnitt</span><span class="sxs-lookup"><span data-stu-id="b9b82-145">1. Configuring interfaces and sub-interfaces</span></span>

<span data-ttu-id="b9b82-146">**Dot1Q gränssnittsdefinition**</span><span class="sxs-lookup"><span data-stu-id="b9b82-146">**Dot1Q interface definition**</span></span>

<span data-ttu-id="b9b82-147">Det här exemplet innehåller hello underordnade gränssnittsdefinition för en underordnad gränssnittet med ett enda VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="b9b82-147">This sample provides hello sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="b9b82-148">hello VLAN-ID är unikt för varje peering.</span><span class="sxs-lookup"><span data-stu-id="b9b82-148">hello VLAN ID is unique per peering.</span></span> <span data-ttu-id="b9b82-149">hello sista oktetten av IPv4-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="b9b82-149">hello last octet of your IPv4 address will always be an odd number.</span></span>

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


<span data-ttu-id="b9b82-150">**QinQ gränssnittsdefinition**</span><span class="sxs-lookup"><span data-stu-id="b9b82-150">**QinQ interface definition**</span></span>

<span data-ttu-id="b9b82-151">Det här exemplet innehåller hello underordnade gränssnittsdefinition för en underordnad gränssnitt med två VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="b9b82-151">This sample provides hello sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="b9b82-152">hello yttre VLAN-ID (s-tagg), om används förblir hello samma över alla hello peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="b9b82-152">hello outer VLAN ID (s-tag), if used remains hello same across all hello peerings.</span></span> <span data-ttu-id="b9b82-153">hello inre VLAN-ID (c-tagg) är unik för varje peering.</span><span class="sxs-lookup"><span data-stu-id="b9b82-153">hello inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="b9b82-154">hello sista oktetten av IPv4-adress kommer alltid att ett udda tal.</span><span class="sxs-lookup"><span data-stu-id="b9b82-154">hello last octet of your IPv4 address will always be an odd number.</span></span>

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

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="b9b82-155">2. Ställa in eBGP-sessioner</span><span class="sxs-lookup"><span data-stu-id="b9b82-155">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="b9b82-156">Du måste konfigurera en BGP-session med Microsoft för varje peering.</span><span class="sxs-lookup"><span data-stu-id="b9b82-156">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="b9b82-157">hello exemplet nedan kan du toosetup en BGP-session med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b9b82-157">hello sample below enables you toosetup a BGP session with Microsoft.</span></span> <span data-ttu-id="b9b82-158">Om hello IPv4-adress som du använde för sub-gränssnittet var a.b.c.d, kommer hello IP-adressen hello BGP-Grannens (Microsoft) att a.b.c.d+1.</span><span class="sxs-lookup"><span data-stu-id="b9b82-158">If hello IPv4 address you used for your sub interface was a.b.c.d, hello IP address of hello BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="b9b82-159">hello sista oktetten av hello BGP granne IPv4-adress ska alltid vara ett jämnt tal.</span><span class="sxs-lookup"><span data-stu-id="b9b82-159">hello last octet of hello BGP neighbor's IPv4 address will always be an even number.</span></span>

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

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a><span data-ttu-id="b9b82-160">3. Konfigurera prefix toobe annonserade över hello BGP-sessionen</span><span class="sxs-lookup"><span data-stu-id="b9b82-160">3. Setting up prefixes toobe advertised over hello BGP session</span></span>
<span data-ttu-id="b9b82-161">Du kan konfigurera din router tooadvertise väljer prefix tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="b9b82-161">You can configure your router tooadvertise select prefixes tooMicrosoft.</span></span> <span data-ttu-id="b9b82-162">Du kan göra det med hjälp av hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="b9b82-162">You can do so using hello sample below.</span></span>

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


### <a name="4-route-maps"></a><span data-ttu-id="b9b82-163">4. Väg maps</span><span class="sxs-lookup"><span data-stu-id="b9b82-163">4. Route maps</span></span>
<span data-ttu-id="b9b82-164">Du kan använda vägen maps och prefix visar toofilter prefix sprids till nätverket.</span><span class="sxs-lookup"><span data-stu-id="b9b82-164">You can use route-maps and prefix lists toofilter prefixes propagated into your network.</span></span> <span data-ttu-id="b9b82-165">Du kan använda hello exemplet nedan tooaccomplish hello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b9b82-165">You can use hello sample below tooaccomplish hello task.</span></span> <span data-ttu-id="b9b82-166">Se till att du har rätt prefix listor installationen.</span><span class="sxs-lookup"><span data-stu-id="b9b82-166">Ensure that you have appropriate prefix lists setup.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b9b82-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9b82-167">Next Steps</span></span>
<span data-ttu-id="b9b82-168">Se hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="b9b82-168">See hello [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>

