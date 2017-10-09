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
# <a name="router-configuration-samples-tooset-up-and-manage-nat"></a>Routerkonfiguration exempel tooset in och hantera NAT
Den här sidan innehåller NAT configuration-exempel för Cisco ASA och Juniper SRX serie routrar. Dessa är avsedda toobe prover för endast vägledning och får inte användas eftersom. Du kan arbeta med din leverantör toocome med lämpliga konfigurationer för nätverket. 

> [!IMPORTANT]
> Exempel på den här sidan är avsedda toobe rent anvisningar. Du måste arbeta med leverantörens försäljning / tekniska teamet och ditt nätverk team toocome upp med lämpliga konfigurationer toomeet dina behov. Microsoft stöder inte problem relaterade tooconfigurations som anges i den här sidan. För att lösa problem måste du kontakta enhetsleverantören av.
> 
> 

* Router configuration exemplen nedan gäller tooAzure offentliga och Microsoft peerkopplingar. Du måste inte konfigurera NAT för privat Azure-peering. Granska [ExpressRoute peerkopplingar](expressroute-circuit-peerings.md) och [ExpressRoute NAT krav](expressroute-nat.md) för mer information.

* Du måste använda separata NAT IP-adresspooler för anslutningen toohello internet och ExpressRoute. Med hjälp av samma NAT IP-poolen över hello hello internet och ExpressRoute leder asymmetriska Routning och anslutningsproblem.


## <a name="cisco-asa-firewalls"></a>Cisco ASA-brandväggar
### <a name="pat-configuration-for-traffic-from-customer-network-toomicrosoft"></a>PATRIK konfiguration för trafik från kundens nätverk tooMicrosoft
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

### <a name="pat-configuration-for-traffic-from-microsoft-toocustomer-network"></a>PATRIK konfiguration för trafik från Microsoft toocustomer nätverk

**Gränssnitt och riktning:**

    Source Interface (where hello traffic enters hello ASA): inside
    Destination Interface (where hello traffic exits hello ASA): outside

**Konfiguration:**

NAT-Pool:

    object network outbound-PAT
        host <NAT-IP>

Målservern:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Objekt-grupp för kundens IP-adresser

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>

    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT-kommandon:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Juniper SRX serie routrar
### <a name="1-create-redundant-ethernet-interfaces-for-hello-cluster"></a>1. Skapa redundanta Ethernet-gränssnitt för hello-kluster
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


### <a name="2-create-two-security-zones"></a>2. Skapa två säkerhetszoner
* Förtroende-zonen för interna nätverket och Untrust zon för externa nätverk med routrar i utkanten
* Tilldela lämpliga gränssnitt toohello zoner
* Tillåt tjänster på hello-gränssnitt

    säkerhet {zoner {säkerhetszon förtroende {-inkommande-värdtrafik {-systemtjänster {ping;                   } protokoll {bgp;                   {reth0.100;}}-gränssnitt               }}-säkerhetszon Untrust {-inkommande-värdtrafik {-systemtjänster {ping;                   } protokoll {bgp;                   {reth1.100;}}-gränssnitt               }           }       }   }


### <a name="3-create-security-policies-between-zones"></a>3. Skapa säkerhetsprinciper mellan zoner
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


### <a name="4-configure-nat-policies"></a>4. Konfigurera NAT-principer
* Skapa två NAT-pooler. En kommer att använda tooNAT trafik utgående tooMicrosoft och andra från Microsoft toohello kund.
* Skapa regler tooNAT hello respektive trafik
  
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

### <a name="5-configure-bgp-tooadvertise-selective-prefixes-in-each-direction"></a>5. Konfigurera BGP tooadvertise selektiv prefix i varje riktning
Se toosamples i [routning configuration prover ](expressroute-config-samples-routing.md) sidan.

### <a name="6-create-policies"></a>6. Skapa principer
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

## <a name="next-steps"></a>Nästa steg
Se hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md) för mer information.

