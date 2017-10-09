---
title: "aaaPlan nätverksfunktioner för Hyper-V-replikering tooa sekundär VMM-plats med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskrivs planering vid replikering av Hyper-V VMs tooa sekundär System Center VMM plats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 095f2d73-994e-4fc0-96f2-e03a7d72e492
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 5934db4a661a2c697a1a799c3848852250ddb451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-networking-for-hyper-v-vm-replication-tooa-secondary-vmm-site"></a>Steg 3: Planera nätverk för Virtuella Hyper-V-replikering tooa sekundär VMM-plats

När du har granskat kraven för distribution, läsa den här artikeln tooplan nätverk vid replikering av Hyper-V virtuella datorer (VM) som hanteras i System Center Virtual Machine Manager (VMM) moln tooa sekundär plats använder [Azure Site Recovery](site-recovery-overview.md) i hello Azure-portalen. 

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-overview"></a>Översikt över-mappning

Nätverksmappning används vid replikering av Hyper-V virtuella datorer (hanteras i VMM) tooa sekundärt datacenter. Nätverksmappningen mappar mellan VM-nätverk på en VMM-källservern och Virtuella datornätverk på en VMM-målservern. Mappningen hello följande:

- **Nätverksanslutning**– ansluter VMs tooappropriate nätverk efter redundansväxling. hello replikerade virtuella datorn kommer att vara anslutna toohello målnätverket som är mappade toohello Källnätverk.
- **Optimal placering**– optimalt platser hello Replikdatorerna på Hyper-V-värdservrar. Replikdatorerna placeras på värdar som kan åtkomst hello mappa Virtuella datornätverk.
- **Inga nätverksmappning**– om du inte konfigurerar nätverksmappning replikering VMs kommer inte att anslutna tooany VM-nätverk efter redundansväxling.


### <a name="example"></a>Exempel

Här är ett exempel tooillustrate denna mekanism. Låt oss ta en organisation med två platser i New York och Chicago.

**Plats** | **VMM-server** | **Virtuella datornätverk** | **Mappas till**
---|---|---|---
New York | VMM-NewYork| VMNetwork1 NewYork | Mappade tooVMNetwork1 Chicago
 |  | VMNetwork2 NewYork | Inte mappad
Chicago | VMM-Chicago| VMNetwork1 Chicago | Mappade tooVMNetwork1 NewYork
 | | VMNetwork1 Chicago | Inte mappad

I det här exemplet:

- När en replikerad virtuell dator har skapats för alla virtuella datorer som är anslutna tooVMNetwork1 NewYork kommer att vara anslutna tooVMNetwork1 Chicago.
- När en replikerad virtuell dator har skapats för VMNetwork2 NewYork eller VMNetwork2 Chicago, blir inte ansluten tooany nätverk.

Här är hur VMM-moln ställs in i vårt exempelorganisation och hello logiska nätverk som är associerade med hello moln.

#### <a name="cloud-protection-settings"></a>Skyddsinställningarna för molnet

**Skyddade moln** | **Skydda molnet** | **Logiskt nätverk (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>Ej tillämpligt</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>
SilverCloud2 | <p>Ej tillämpligt</p><p></p> | <p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Inställningar för logiska och Virtuella nätverk

**Plats** | **Logiskt nätverk** | **Associerat VM-nätverk**
---|---|---
New York | LogicalNetwork1 NewYork | VMNetwork1 NewYork
Chicago | LogicalNetwork1 Chicago | VMNetwork1 Chicago
 | LogicalNetwork2Chicago | VMNetwork2 Chicago

#### <a name="target-network-settings"></a>Nätverksinställningar för mål

Baserat på dessa inställningar när du väljer hello målnätverket VM, visar hello följande tabell hello-alternativ som är tillgängliga.

**Välj** | **Skyddade moln** | **Skydda molnet** | **Målnätverket som är tillgängliga**
---|---|---|---
VMNetwork1 Chicago | SilverCloud1 | SilverCloud2 | Tillgänglig
 | GoldCloud1 | GoldCloud2 | Tillgänglig
VMNetwork2 Chicago | SilverCloud1 | SilverCloud2 | Inte tillgänglig
 | GoldCloud1 | GoldCloud2 | Tillgänglig


Om hello målnätverket har flera undernät och ett av dessa undernät har samma namn som hello undernätet på vilka hello virtuella källdatorn finns, sedan hello hello blir replikerade virtuella datorn anslutna toothat målundernätverket efter en redundansväxling. Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.


#### <a name="failback-behavior"></a>Beteende för återställning efter fel

vad som händer i hello skiftläget för återställning efter fel (omvänd replikering) toosee vi antar att VMNetwork1 NewYork är mappade tooVMNetwork1-Chicago, med hello följande inställningar.


**Virtuell dator** | **Anslutna tooVM nätverk**
---|---
VM1 | VMNetwork1 nätverk
VM2 (replik av VM1) | VMNetwork1 Chicago

Med dessa inställningar nu ska vi se vad som händer på några möjliga scenarier.

**Scenario** | **Resultatet**
---|---
Ingen ändring i hello-egenskaper för VM-2 efter växling vid fel. | VM-1 förblir anslutna toohello Källnätverk.
Nätverksegenskaper för VM-2 har ändrats efter växling vid fel och har kopplats från. | VM-1 är frånkopplad.
Nätverksegenskaper för VM-2 har ändrats efter växling vid fel och är anslutna tooVMNetwork2 Chicago. | Om VMNetwork2 Chicago har inte mappats, kopplas VM-1.
Nätverksmappningen för VMNetwork1 Chicago ändras. | VM-1 blir anslutna toohello nu mappat nätverk tooVMNetwork1-Chicago.



## <a name="prepare-for-network-mapping"></a>Förbereda för nätverksmappning

1. På hello käll- och VMM-servrar, bör du ha ett logiskt nätverk som är associerade med hello källa och mål moln. 
2. Du bör ha ett logiskt nätverk för Virtuella nätverk länkade toohello i hello käll- och servrar.
3. Virtuella datorer på Hyper-V-värdar i hello källplats ska vara länkade toohello källnätverket. Om du bara använder en enda VMM-server, kan du konfigurera mappning mellan Virtuella nätverk på hello samma server.

Här är vad som händer när du konfigurerar nätverksmappning under distributionen av Site Recovery:

- När du konfigurera nätverksmappning och välj ett Målnätverk VM visas hello VMM källa moln som använder hello källnätverket, tillsammans med hello tillgängliga målservrar Virtuella datornätverk på hello mål moln.
- - När mappningen är korrekt konfigurerad och replikering har aktiverats, en källa VM tas anslutna tooits källnätverket och dess replik på målplatsen hello ansluts mappas tooits VM-nätverk.
- Om hello målnätverket har flera undernät och ett av dessa undernät har samma namn som hello undernätet på vilka hello virtuella källdatorn finns, sedan hello hello blir replikerade virtuella datorn anslutna toothat målundernätverket efter en redundansväxling. Om det inte finns något målundernät med ett matchande namn, blir hello VM anslutna toohello första undernätet i hello nätverk.

## <a name="connect-toovms-after-failover"></a>Ansluta tooVMs efter växling vid fel

När du planerar din replikering och redundansstrategi, en av hello viktiga frågor är hur tooconnect toohello repliken efter växling vid fel. Det finns ett par alternativ: 

- **Använd en annan IP-adress**: du kan välja toouse en annan IP-adress för hello replikerade virtuella datorn. I det här scenariot hello VM hämtar en ny IP-adress efter växling vid fel och en DNS-uppdatering krävs.
- **Behåll hello samma IP-adress**: du kanske vill toouse hello samma IP-adress för hello replikerade virtuella datorn. Att hålla hello samma IP-adresser förenklar hello återställning genom att minska nätverk problemen efter växling vid fel. 

## <a name="retain-ip-addresses"></a>Behåll IP-adresser

Om du vill tooretain hello IP-adresser från hello primära platsen efter redundans toohello sekundär plats, kan du göra en fullständig undernät växling vid fel och uppdatera vägar tooindicate hello nya plats hello IP-adresser, eller alternativt distribuera ett ambitiöst undernät mellan hello primära och hello återställningsplatser.

### <a name="stretched-subnet"></a>Ambitiöst undernät

I ett sträckta undernät är hello undernät tillgängliga samtidigt i båda hello primär och sekundär plats. Om du flyttar en server och den sekundära platsen för IP (nivå 3) konfiguration toohello dirigerar hello nätverket hello trafik toohello ny plats automatiskt. 

Du måste nätverksutrustning som kan hantera ett ambitiöst VLAN ur Lager2 (data-länk lager). Genom att sträcka ut hello VLAN utökar dessutom hello potentiella feldomän tooboth platser, i stort sett blir en enskild felpunkt. Detta är ett troligt, kan detta hända att broadcast storm igång och kan inte vara isolerad. 


### <a name="subnet-failover"></a>Undernät växling vid fel

Du kan köra ett undernät redundans tooobtain hello fördelarna med hello utsträckt undernät utan att faktiskt sträcka ut. I den här lösningen, ett undernät blir tillgängliga i hello käll- eller plats, men inte på båda samtidigt. toomaintain hello IP-adressutrymme i hello händelse av en växling vid fel, du kan via programmering ordna för hello router infrastruktur toomove hello undernät från en plats tooanother. När när redundans inträffar undernät skulle flyttas med associerade hello virtuella datorer. hello huvudsakliga Nackdelen är att i hello händelse av fel du toomove hello hela undernätet.

### <a name="example"></a>Exempel

Här är ett exempel på fullständiga undernät växling vid fel. hello primär webbplats har program som körs i undernätet 192.168.1.0/24. Vid redundans alla hello virtuella datorer i det här undernätet har redundansväxlats toohello sekundär plats och behålla sina IP-adresser. Vägar måste toobe ändrade tooreflect hello faktum att alla hello VM virtuella datorer som tillhör toosubnet 192.168.1.0/24 har nu flyttats toohello sekundär plats.

hello visar följande bilder hello undernät före och efter växling vid fel:

- Innan du redundans är undernät 192.168.0.1/24 aktiv på hello källplatsen aktiveras på hello sekundära platsen efter redundans.
- hello dirigerar mellan primära platsen och återställningsplatsen, tredje och primär plats och tredje platsen och återställningsplatsen måste toobe ändras på lämpligt sätt.

**Innan du redundans**

![Innan du redundans](./media/vmm-to-vmm-walkthrough-network/network-design2.png)

**Efter växling vid fel**

![Efter växling vid fel](./media/vmm-to-vmm-walkthrough-network/network-design3.png)

Efter växling vid fel är här vad som händer:

- Site Recovery tilldelar en IP-adress för varje nätverksgränssnitt på hello virtuell dator från hello statisk IP-adresspool i hello relevanta nätverk för varje VMM-instans.
- Om hello IP-adresspool på hello sekundär plats är samma som på Site Recovery i källwebbplatsen hello allokerar hello hello samma IP-adressen (hello källa VM) toohello replikerade virtuella datorn. hello IP-adressen är reserverad i VMM, men det är inte angivet som IP-adress för hello redundans på hello Hyper-V-värd. hello växling vid fel IP-adress på en Hyper-v-värden anges innan hello växling vid fel.
- Om hello samma IP-adress inte är tillgänglig, allokerar Site Recovery en annan tillgänglig IP-adress från hello poolen.
- Om virtuella datorer som använder DHCP hantera Site Recovery inte hello IP-adresser. Du behöver toocheck som hello DHCP server på hello sekundär plats kan allokera adress från hello samma intervall som hello källplats.

### <a name="validate-hello-ip-address"></a>Validera hello IP-adress

När du aktiverar skyddet för en virtuell dator kan du använda följande exempel skriptet tooverify hello-adress som tilldelats toohello VM. hello samma IP-adress ska anges som hello IP-adress för redundans och tilldelad toohello VM på hello tiden för växling vid fel:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="changing-ip-addresses"></a>Ändra IP-adresser

I det här scenariot ändras hello IP-adresser för virtuella datorer som växlar över. den här lösningen hello Nackdelen är hello underhåll. Vanligtvis uppdateras DNS när Replikdatorerna har startat. DNS-poster kan måste toobe ändras eller fluster i thenetwork och cachelagrade poster uppdateras. Detta kan leda till driftstopp. Driftstopp kan begränsas på följande sätt:

- Använd låg TTL-värden för intranätsprogram.
- Använd följande skript i en Site Recovery återställningsplan tooupdate hello DNS server tooensure en rimlig uppdatering hello. Du behöver inte hello skript om du använder dynamisk DNS-registrering.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Exempel 

Nu ska vi titta på ett scenario där du planerar toouse olika IP-adresser mellan hello primära och hello recovery platser. I det här exemplet har vi olika IP-adresser mellan primära och sekundära platser och där, s tredje plats från vilka program som finns på platsen för primära eller återställning av hello kan nås.

- Före redundans, appar är värdbaserade undernät 192.168.1.0/24 på hello primära platsen och är konfigurerade toobe i undernätet 172.16.1.0/24 på hello sekundära platsen efter en redundansväxling.
- VPN-anslutningar/nätverksvägar har konfigurerats på rätt sätt så att alla tre platser som har åtkomst till varandra.
- Appar kommer att återställas i hello recovery undernät efter redundans. I det här scenariot finns inget behov av toofail över hello hela undernätet och inga ändringar är nödvändiga tooreconfigure VPN eller nätverksvägar. Se till att program fortfarande är tillgänglig hello redundans och vissa DNS-uppdateringar.
- Om DNS är konfigurerade tooallow dynamiska uppdateringar, sedan hello virtuella datorer kommer att registrera sig med hello nya IP-adressen, när de startar efter växling vid fel.

**Innan du redundans**

![Olika IP - före redundans](./media/vmm-to-vmm-walkthrough-network/network-design10.png)

**Efter växling vid fel**

![Olika IP - efter växling vid fel](./media/vmm-to-vmm-walkthrough-network/network-design11.png)



## <a name="next-steps"></a>Nästa steg

Gå för[steg 4: Förbered VMM och Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md).


