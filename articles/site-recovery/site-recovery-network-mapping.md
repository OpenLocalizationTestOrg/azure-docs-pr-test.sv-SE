---
title: "aaaPlan nätverksmappningen för Hyper-V VM-replikering med Site Recovery | Microsoft Docs"
description: "Konfigurera nätverksmappning för replikering av Hyper-V virtuella datorer från ett lokalt datacenter tooAzure eller tooa sekundär plats."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: tysonn
ms.assetid: fcaa2f52-489d-4c1c-865f-9e78e000b351
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/23/2017
ms.author: raynew
ms.openlocfilehash: 86199b5840ea10fd33630bcc75d14340a49e01bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-network-mapping-for-hyper-v-vm-replication-with-site-recovery"></a>Planera nätverksmappningen för Hyper-V VM-replikering med Site Recovery



Den här artikeln hjälper dig att toounderstand och planera för nätverk mappning vid replikering av Hyper-V VMs tooAzure eller tooa sekundär plats, med hello [Azure Site Recovery-tjänsten](site-recovery-overview.md).

När du har läst den här artikeln efter eventuella kommentarer längst ned hello i den här artikeln eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-tooazure"></a>Nätverksmappningen för replikering tooAzure

Nätverksmappning används vid replikering av Hyper-V virtuella datorer (hanteras i VMM) tooAzure. Mappningar för mappning mellan Virtuella nätverk på en VMM-källservern för nätverk och Azure-målnätverken. Mappningen hello följande:

- **Nätverksanslutning**– säkerställer att replikerade virtuella Azure-datorer är anslutna toohello mappade nätverksenheter. Alla datorer som redundansväxlas hello samma nätverk kan ansluta tooeach andra, även om de har redundansväxlats i olika återställningsplaner.
- **Nätverksgateway**– om en nätverksgateway har konfigurerats på hello Azure-målnätverket kan virtuella datorer kan ansluta tooother lokala virtuella datorer.

Tänk på följande:

- Du kan mappa en källa VMM VM nätverket tooan virtuella Azure-nätverket.
- Efter redundans virtuella Azure-datorer i hello blir Källnätverk anslutna toohello mappade mål virtuellt nätverk.
- Nya virtuella datorer läggs toohello källnätverket ansluts toohello mappade Azure-nätverket när replikeringen sker.
- Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet som hello källa virtual machine finns så hello replikerade virtuella datorn ansluter toothat målundernätverket efter en redundansväxling.
- Om det inte finns något målundernät med ett matchande namn, ansluter hello virtuella toohello första undernätet i hello nätverk.


## <a name="network-mapping-for-replication-tooa-secondary-datacenter"></a>Nätverksmappningen för replikering tooa sekundärt datacenter

Nätverksmappning används vid replikering av Hyper-V virtuella datorer (hanteras i System Center Virtual Machine Manager (VMM)) tooa sekundärt datacenter. Nätverksmappningen mappar mellan VM-nätverk på en VMM-källservern och Virtuella datornätverk på en VMM-målservern. Mappningen hello följande:

- **Nätverksanslutning**– ansluter VMs tooappropriate nätverk efter redundansväxling. hello replikerade virtuella datorn kommer att vara anslutna toohello målnätverket som är mappade toohello Källnätverk.
- **Optimal placering**– optimalt platser hello Replikdatorerna på Hyper-V-värdservrar. Replikdatorerna placeras på värdar som kan åtkomst hello mappa Virtuella datornätverk.
- **Inga nätverksmappning**– om du inte konfigurerar nätverksmappning replikering VMs kommer inte att anslutna tooany VM-nätverk efter redundansväxling.

Tänk på följande:

- Nätverksmappning kan konfigureras mellan Virtuella nätverk på två VMM-servrar eller på en VMM-server om två platser hanteras av hello samma server.
- När mappningen är korrekt konfigurerad och replikering har aktiverats, en virtuell dator på hello primära platsen kommer att vara anslutna tooa nätverk och repliken på målplatsen hello ansluts mappas tooits nätverk.
-
- Om nätverk har konfigurerats korrekt i VMM när du väljer ett mål Virtuellt datornätverk under nätverksmappning, visas hello VMM källa moln som använder hello källnätverket, tillsammans med hello tillgängliga målservrar Virtuella datornätverk på hello mål moln som används för skydd.
- Om hello målnätverket har flera undernät och ett av dessa undernät har samma namn som hello undernätet på vilka hello virtuella källdatorn finns, sedan hello hello blir replikerade virtuella datorn anslutna toothat målundernätverket efter en redundansväxling. Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.



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



## <a name="next-steps"></a>Nästa steg

Lär dig mer om [planera hello nätverksinfrastruktur](site-recovery-network-design.md).
