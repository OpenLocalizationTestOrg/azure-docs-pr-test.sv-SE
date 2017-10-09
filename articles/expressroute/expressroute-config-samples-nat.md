---
title: aaaExpressRoute kunden router configuration exempel | Microsoft Docs
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
ms.openlocfilehash: b5faca0666bda6173e54abb0b6560d5f8bf8bfc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="router-configuration-samples-tooset-up-and-manage-nat"></a><span data-ttu-id="d9f87-103">Routerkonfiguration exempel tooset in och hantera NAT</span><span class="sxs-lookup"><span data-stu-id="d9f87-103">Router configuration samples tooset up and manage NAT</span></span>
<span data-ttu-id="d9f87-104">Den här sidan innehåller NAT configuration-exempel för Cisco ASA och Juniper SRX serie routrar.</span><span class="sxs-lookup"><span data-stu-id="d9f87-104">This page provides NAT configuration samples for Cisco ASA and Juniper SRX series routers.</span></span> <span data-ttu-id="d9f87-105">Dessa är avsedda toobe prover för endast vägledning och får inte användas eftersom.</span><span class="sxs-lookup"><span data-stu-id="d9f87-105">These are intended toobe samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="d9f87-106">Du kan arbeta med din leverantör toocome med lämpliga konfigurationer för nätverket.</span><span class="sxs-lookup"><span data-stu-id="d9f87-106">You can work with your vendor toocome up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="d9f87-107">Exempel på den här sidan är avsedda toobe rent anvisningar.</span><span class="sxs-lookup"><span data-stu-id="d9f87-107">Samples in this page are intended toobe purely for guidance.</span></span> <span data-ttu-id="d9f87-108">Du måste arbeta med leverantörens försäljning / tekniska teamet och ditt nätverk team toocome upp med lämpliga konfigurationer toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="d9f87-108">You must work with your vendor's sales / technical team and your networking team toocome up with appropriate configurations toomeet your needs.</span></span> <span data-ttu-id="d9f87-109">Microsoft stöder inte problem relaterade tooconfigurations som anges i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="d9f87-109">Microsoft will not support issues related tooconfigurations listed in this page.</span></span> <span data-ttu-id="d9f87-110">För att lösa problem måste du kontakta enhetsleverantören av.</span><span class="sxs-lookup"><span data-stu-id="d9f87-110">You must contact your device vendor for support issues.</span></span>
> 
> 

* <span data-ttu-id="d9f87-111">Router configuration exemplen nedan gäller tooAzure offentliga och Microsoft peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="d9f87-111">Router configuration samples below apply tooAzure Public and Microsoft peerings.</span></span> <span data-ttu-id="d9f87-112">Du måste inte konfigurera NAT för privat Azure-peering.</span><span class="sxs-lookup"><span data-stu-id="d9f87-112">You must not configure NAT for Azure private peering.</span></span> <span data-ttu-id="d9f87-113">Granska [ExpressRoute peerkopplingar](expressroute-circuit-peerings.md) och [ExpressRoute NAT krav](expressroute-nat.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d9f87-113">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute NAT requirements](expressroute-nat.md) for more details.</span></span>

* <span data-ttu-id="d9f87-114">Du måste använda separata NAT IP-adresspooler för anslutningen toohello internet och ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="d9f87-114">You MUST use separate NAT IP pools for connectivity toohello internet and ExpressRoute.</span></span> <span data-ttu-id="d9f87-115">Med hjälp av samma NAT IP-poolen över hello hello internet och ExpressRoute leder asymmetriska Routning och anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="d9f87-115">Using hello same NAT IP pool across hello internet and ExpressRoute will result in asymmetric routing and loss of connectivity.</span></span>


## <a name="cisco-asa-firewalls"></a><span data-ttu-id="d9f87-116">Cisco ASA-brandväggar</span><span class="sxs-lookup"><span data-stu-id="d9f87-116">Cisco ASA firewalls</span></span>
### <a name="pat-configuration-for-traffic-from-customer-network-toomicrosoft"></a><span data-ttu-id="d9f87-117">PATRIK konfiguration för trafik från kundens nätverk tooMicrosoft</span><span class="sxs-lookup"><span data-stu-id="d9f87-117">PAT configuration for traffic from customer network tooMicrosoft</span></span>
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

### <a name="pat-configuration-for-traffic-from-microsoft-toocustomer-network"></a><span data-ttu-id="d9f87-118">PATRIK konfiguration för trafik från Microsoft toocustomer nätverk</span><span class="sxs-lookup"><span data-stu-id="d9f87-118">PAT configuration for traffic from Microsoft toocustomer network</span></span>

<span data-ttu-id="d9f87-119">**Gränssnitt och riktning:**</span><span class="sxs-lookup"><span data-stu-id="d9f87-119">**Interfaces and Direction:**</span></span>

    Source Interface (where hello traffic enters hello ASA): inside
    Destination Interface (where hello traffic exits hello ASA): outside

<span data-ttu-id="d9f87-120">**Konfiguration:**</span><span class="sxs-lookup"><span data-stu-id="d9f87-120">**Configuration:**</span></span>

<span data-ttu-id="d9f87-121">NAT-Pool:</span><span class="sxs-lookup"><span data-stu-id="d9f87-121">NAT Pool:</span></span>

    object network outbound-PAT
        host <NAT-IP>

<span data-ttu-id="d9f87-122">Målservern:</span><span class="sxs-lookup"><span data-stu-id="d9f87-122">Target Server:</span></span>

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

<span data-ttu-id="d9f87-123">Objekt-grupp för kundens IP-adresser</span><span class="sxs-lookup"><span data-stu-id="d9f87-123">Object Group for Customer IP Addresses</span></span>

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>

    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

<span data-ttu-id="d9f87-124">NAT-kommandon:</span><span class="sxs-lookup"><span data-stu-id="d9f87-124">NAT Commands:</span></span>

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a><span data-ttu-id="d9f87-125">Juniper SRX serie routrar</span><span class="sxs-lookup"><span data-stu-id="d9f87-125">Juniper SRX series routers</span></span>
### <a name="1-create-redundant-ethernet-interfaces-for-hello-cluster"></a><span data-ttu-id="d9f87-126">1. Skapa redundanta Ethernet-gränssnitt för hello-kluster</span><span class="sxs-lookup"><span data-stu-id="d9f87-126">1. Create redundant Ethernet interfaces for hello cluster</span></span>
    interfaces {
        reth0 {
            description "tooInternal Network";
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
            description "tooMicrosoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "tooMicrosoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a><span data-ttu-id="d9f87-127">2. Skapa två säkerhetszoner</span><span class="sxs-lookup"><span data-stu-id="d9f87-127">2. Create two security zones</span></span>
* <span data-ttu-id="d9f87-128">Förtroende-zonen för interna nätverket och Untrust zon för externa nätverk med routrar i utkanten</span><span class="sxs-lookup"><span data-stu-id="d9f87-128">Trust Zone for internal network and Untrust Zone for external network facing Edge Routers</span></span>
* <span data-ttu-id="d9f87-129">Tilldela lämpliga gränssnitt toohello zoner</span><span class="sxs-lookup"><span data-stu-id="d9f87-129">Assign appropriate interfaces toohello zones</span></span>
* <span data-ttu-id="d9f87-130">Tillåt tjänster på hello-gränssnitt</span><span class="sxs-lookup"><span data-stu-id="d9f87-130">Allow services on hello interfaces</span></span>

    <span data-ttu-id="d9f87-131">säkerhet {zoner {säkerhetszon förtroende {-inkommande-värdtrafik {-systemtjänster {ping;                   } protokoll {bgp;                   {reth0.100;}}-gränssnitt               }}-säkerhetszon Untrust {-inkommande-värdtrafik {-systemtjänster {ping;                   } protokoll {bgp;                   {reth1.100;}}-gränssnitt               }           }       }   }</span><span class="sxs-lookup"><span data-stu-id="d9f87-131">security {       zones {           security-zone Trust {               host-inbound-traffic {                   system-services {                       ping;                   }                   protocols {                       bgp;                   }               }               interfaces {                   reth0.100;               }           }           security-zone Untrust {               host-inbound-traffic {                   system-services {                       ping;                   }                   protocols {                       bgp;                   }               }               interfaces {                   reth1.100;               }           }       }   }</span></span>


### <a name="3-create-security-policies-between-zones"></a><span data-ttu-id="d9f87-132">3. Skapa säkerhetsprinciper mellan zoner</span><span class="sxs-lookup"><span data-stu-id="d9f87-132">3. Create security policies between zones</span></span>
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


### <a name="4-configure-nat-policies"></a><span data-ttu-id="d9f87-133">4. Konfigurera NAT-principer</span><span class="sxs-lookup"><span data-stu-id="d9f87-133">4. Configure NAT policies</span></span>
* <span data-ttu-id="d9f87-134">Skapa två NAT-pooler.</span><span class="sxs-lookup"><span data-stu-id="d9f87-134">Create two NAT pools.</span></span> <span data-ttu-id="d9f87-135">En kommer att använda tooNAT trafik utgående tooMicrosoft och andra från Microsoft toohello kund.</span><span class="sxs-lookup"><span data-stu-id="d9f87-135">One will be used tooNAT traffic outbound tooMicrosoft and other from Microsoft toohello customer.</span></span>
* <span data-ttu-id="d9f87-136">Skapa regler tooNAT hello respektive trafik</span><span class="sxs-lookup"><span data-stu-id="d9f87-136">Create rules tooNAT hello respective traffic</span></span>
  
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
                       toorouting-instance External-ExpressRoute;
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
                       toorouting-instance Internal;
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

### <a name="5-configure-bgp-tooadvertise-selective-prefixes-in-each-direction"></a><span data-ttu-id="d9f87-137">5. Konfigurera BGP tooadvertise selektiv prefix i varje riktning</span><span class="sxs-lookup"><span data-stu-id="d9f87-137">5. Configure BGP tooadvertise selective prefixes in each direction</span></span>
<span data-ttu-id="d9f87-138">Se toosamples i [routning configuration prover ](expressroute-config-samples-routing.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="d9f87-138">Refer toosamples in [Routing configuration samples ](expressroute-config-samples-routing.md) page.</span></span>

### <a name="6-create-policies"></a><span data-ttu-id="d9f87-139">6. Skapa principer</span><span class="sxs-lookup"><span data-stu-id="d9f87-139">6. Create policies</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="d9f87-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9f87-140">Next steps</span></span>
<span data-ttu-id="d9f87-141">Se hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d9f87-141">See hello [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>

