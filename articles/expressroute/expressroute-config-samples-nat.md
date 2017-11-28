---
title: ExpressRoute kunden router configuration prover | Microsoft Docs
description: "Den här sidan innehåller router configuration-exempel för Cisco och Juniper routrar."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: d6ea716f-d5ee-4a61-92b0-640d6e7d6974
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: 83a7da2db537a3c900e90432455d59e8ac56d917
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="router-configuration-samples-to-set-up-and-manage-nat"></a><span data-ttu-id="2fbda-103">Router configuration exempel för att konfigurera och hantera NAT</span><span class="sxs-lookup"><span data-stu-id="2fbda-103">Router configuration samples to set up and manage NAT</span></span>
<span data-ttu-id="2fbda-104">Den här sidan innehåller NAT configuration-exempel för Cisco ASA och Juniper SRX serie routrar.</span><span class="sxs-lookup"><span data-stu-id="2fbda-104">This page provides NAT configuration samples for Cisco ASA and Juniper SRX series routers.</span></span> <span data-ttu-id="2fbda-105">Dessa är avsedda att prover för endast vägledning och får inte användas eftersom.</span><span class="sxs-lookup"><span data-stu-id="2fbda-105">These are intended to be samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="2fbda-106">Du kan arbeta med leverantören för att få fram lämpliga konfigurationerna för nätverket.</span><span class="sxs-lookup"><span data-stu-id="2fbda-106">You can work with your vendor to come up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="2fbda-107">Exempel på den här sidan är avsedda att vara rent anvisningar.</span><span class="sxs-lookup"><span data-stu-id="2fbda-107">Samples in this page are intended to be purely for guidance.</span></span> <span data-ttu-id="2fbda-108">Du måste arbeta med leverantörens försäljning / tekniska teamet och teamet nätverk för att få fram lämpliga konfigurationer för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="2fbda-108">You must work with your vendor's sales / technical team and your networking team to come up with appropriate configurations to meet your needs.</span></span> <span data-ttu-id="2fbda-109">Microsoft stöder inte problem relaterade till konfigurationer som anges i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="2fbda-109">Microsoft will not support issues related to configurations listed in this page.</span></span> <span data-ttu-id="2fbda-110">För att lösa problem måste du kontakta enhetsleverantören av.</span><span class="sxs-lookup"><span data-stu-id="2fbda-110">You must contact your device vendor for support issues.</span></span>
> 
> 

* <span data-ttu-id="2fbda-111">Router configuration exemplen nedan gäller för offentlig Azure och Microsoft peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="2fbda-111">Router configuration samples below apply to Azure Public and Microsoft peerings.</span></span> <span data-ttu-id="2fbda-112">Du måste inte konfigurera NAT för privat Azure-peering.</span><span class="sxs-lookup"><span data-stu-id="2fbda-112">You must not configure NAT for Azure private peering.</span></span> <span data-ttu-id="2fbda-113">Granska [ExpressRoute peerkopplingar](expressroute-circuit-peerings.md) och [ExpressRoute NAT krav](expressroute-nat.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="2fbda-113">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute NAT requirements](expressroute-nat.md) for more details.</span></span>

* <span data-ttu-id="2fbda-114">Du måste använda separata NAT IP-adresspooler för anslutning till internet och ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2fbda-114">You MUST use separate NAT IP pools for connectivity to the internet and ExpressRoute.</span></span> <span data-ttu-id="2fbda-115">Med hjälp av samma NAT IP-adresspool mellan internet och ExpressRoute leder asymmetriska Routning och anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="2fbda-115">Using the same NAT IP pool across the internet and ExpressRoute will result in asymmetric routing and loss of connectivity.</span></span>


## <a name="cisco-asa-firewalls"></a><span data-ttu-id="2fbda-116">Cisco ASA-brandväggar</span><span class="sxs-lookup"><span data-stu-id="2fbda-116">Cisco ASA firewalls</span></span>
### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a><span data-ttu-id="2fbda-117">PATRIK konfiguration för trafik från kundens nätverk till Microsoft</span><span class="sxs-lookup"><span data-stu-id="2fbda-117">PAT configuration for traffic from customer network to Microsoft</span></span>
    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>


    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>

    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>

    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>

    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2

    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a><span data-ttu-id="2fbda-118">PATRIK konfiguration för trafik från Microsoft till kundens nätverk</span><span class="sxs-lookup"><span data-stu-id="2fbda-118">PAT configuration for traffic from Microsoft to customer network</span></span>

<span data-ttu-id="2fbda-119">**Gränssnitt och riktning:**</span><span class="sxs-lookup"><span data-stu-id="2fbda-119">**Interfaces and Direction:**</span></span>

    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

<span data-ttu-id="2fbda-120">**Konfiguration:**</span><span class="sxs-lookup"><span data-stu-id="2fbda-120">**Configuration:**</span></span>

<span data-ttu-id="2fbda-121">NAT-Pool:</span><span class="sxs-lookup"><span data-stu-id="2fbda-121">NAT Pool:</span></span>

    object network outbound-PAT
        host <NAT-IP>

<span data-ttu-id="2fbda-122">Målservern:</span><span class="sxs-lookup"><span data-stu-id="2fbda-122">Target Server:</span></span>

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

<span data-ttu-id="2fbda-123">Objekt-grupp för kundens IP-adresser</span><span class="sxs-lookup"><span data-stu-id="2fbda-123">Object Group for Customer IP Addresses</span></span>

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>

    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

<span data-ttu-id="2fbda-124">NAT-kommandon:</span><span class="sxs-lookup"><span data-stu-id="2fbda-124">NAT Commands:</span></span>

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a><span data-ttu-id="2fbda-125">Juniper SRX serie routrar</span><span class="sxs-lookup"><span data-stu-id="2fbda-125">Juniper SRX series routers</span></span>
### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a><span data-ttu-id="2fbda-126">1. Skapa redundanta Ethernet-gränssnitt för klustret</span><span class="sxs-lookup"><span data-stu-id="2fbda-126">1. Create redundant Ethernet interfaces for the cluster</span></span>
    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a><span data-ttu-id="2fbda-127">2. Skapa två säkerhetszoner</span><span class="sxs-lookup"><span data-stu-id="2fbda-127">2. Create two security zones</span></span>
* <span data-ttu-id="2fbda-128">Förtroende-zonen för interna nätverket och Untrust zon för externa nätverk med routrar i utkanten</span><span class="sxs-lookup"><span data-stu-id="2fbda-128">Trust Zone for internal network and Untrust Zone for external network facing Edge Routers</span></span>
* <span data-ttu-id="2fbda-129">Tilldela lämpliga gränssnitt till zoner</span><span class="sxs-lookup"><span data-stu-id="2fbda-129">Assign appropriate interfaces to the zones</span></span>
* <span data-ttu-id="2fbda-130">Tillåt tjänster på gränssnitt</span><span class="sxs-lookup"><span data-stu-id="2fbda-130">Allow services on the interfaces</span></span>

    <span data-ttu-id="2fbda-131">säkerhet {zoner {säkerhetszon förtroende {-inkommande-värdtrafik {-systemtjänster {ping;                   } protokoll {bgp;                   {reth0.100;}}-gränssnitt               }}-säkerhetszon Untrust {-inkommande-värdtrafik {-systemtjänster {ping;                   } protokoll {bgp;                   {reth1.100;}}-gränssnitt               }           }       }   }</span><span class="sxs-lookup"><span data-stu-id="2fbda-131">security {       zones {           security-zone Trust {               host-inbound-traffic {                   system-services {                       ping;                   }                   protocols {                       bgp;                   }               }               interfaces {                   reth0.100;               }           }           security-zone Untrust {               host-inbound-traffic {                   system-services {                       ping;                   }                   protocols {                       bgp;                   }               }               interfaces {                   reth1.100;               }           }       }   }</span></span>


### <a name="3-create-security-policies-between-zones"></a><span data-ttu-id="2fbda-132">3. Skapa säkerhetsprinciper mellan zoner</span><span class="sxs-lookup"><span data-stu-id="2fbda-132">3. Create security policies between zones</span></span>
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a><span data-ttu-id="2fbda-133">4. Konfigurera NAT-principer</span><span class="sxs-lookup"><span data-stu-id="2fbda-133">4. Configure NAT policies</span></span>
* <span data-ttu-id="2fbda-134">Skapa två NAT-pooler.</span><span class="sxs-lookup"><span data-stu-id="2fbda-134">Create two NAT pools.</span></span> <span data-ttu-id="2fbda-135">En används för NAT-trafik utgående till Microsoft och andra från Microsoft till kunden.</span><span class="sxs-lookup"><span data-stu-id="2fbda-135">One will be used to NAT traffic outbound to Microsoft and other from Microsoft to the customer.</span></span>
* <span data-ttu-id="2fbda-136">Skapa regler för NAT respektive trafik</span><span class="sxs-lookup"><span data-stu-id="2fbda-136">Create rules to NAT the respective traffic</span></span>
  
       security {
           nat {
               source {
                   pool SNAT-To-ExpressRoute {
                       routing-instance {
                           External-ExpressRoute;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   pool SNAT-From-ExpressRoute {
                       routing-instance {
                           Internal;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   rule-set Outbound_NAT {
                       from routing-instance Internal;
                       to routing-instance External-ExpressRoute;
                       rule SNAT-Out {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-To-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
                   rule-set Inbound-NAT {
                       from routing-instance External-ExpressRoute;
                       to routing-instance Internal;
                       rule SNAT-In {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-From-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
               }
           }
       }

### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a><span data-ttu-id="2fbda-137">5. Konfigurera BGP att annonsera selektiv prefix i varje riktning</span><span class="sxs-lookup"><span data-stu-id="2fbda-137">5. Configure BGP to advertise selective prefixes in each direction</span></span>
<span data-ttu-id="2fbda-138">Referera till exempel i [routning configuration prover ](expressroute-config-samples-routing.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="2fbda-138">Refer to samples in [Routing configuration samples ](expressroute-config-samples-routing.md) page.</span></span>

### <a name="6-create-policies"></a><span data-ttu-id="2fbda-139">6. Skapa principer</span><span class="sxs-lookup"><span data-stu-id="2fbda-139">6. Create policies</span></span>
    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a><span data-ttu-id="2fbda-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2fbda-140">Next steps</span></span>
<span data-ttu-id="2fbda-141">Se [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="2fbda-141">See the [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>

