---
title: "aaaAzure Site Recovery stöd matrix för replikering från Azure tooAzure | Microsoft Docs"
description: "Sammanfattar hello stöds operativsystem och konfigurationer för Azure Site Recovery replikering av virtuella Azure-datorer (VM) från en region tooanother för disaster recovery (DR) behov."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/10/2017
ms.author: sujayt
ms.openlocfilehash: 75b2451b4c2069ca4b11deb0efe1329d43879eb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-azure-tooazure"></a>Azure Site Recovery stöd matrix för replikering från Azure tooAzure


>[!NOTE]
>
> Site Recovery replikering för virtuella Azure-datorer är för närvarande under förhandsgranskning.

Den här artikeln beskriver konfigurationer som stöds och komponenter för Azure Site Recovery när replikering och återställa virtuella Azure-datorer från en region tooanother region.

## <a name="user-interface-options"></a>Alternativ för användargränssnitt

**Användargränssnittet** |  **Stöds / stöds inte**
--- | ---
**Azure Portal** | Stöds
**Klassisk portal** | Stöds inte
**PowerShell** | Stöds för närvarande inte
**REST-API** | Stöds för närvarande inte
**CLI** | Stöds för närvarande inte


## <a name="resource-move-support"></a>Resursen move-support

**Flytta resurstypen** | **Stöds / stöds inte** | **Kommentarer**  
--- | --- | ---
**Flytta valvet mellan resursgrupper** | Stöds inte |Du kan inte flytta hello Recovery services-ventilen över resursgrupper.
**Flytta bearbetning, lagring och nätverk mellan resursgrupper** | Stöds inte |Om du flyttar en virtuell dator (eller andra komponenter, till exempel lagring och nätverk) när du har aktiverat replikering måste toodisable replikering och aktivera replikering för hello virtuella datorn igen.


## <a name="support-for-deployment-models"></a>Stöd för distributionsmodeller

**Distributionsmodell** | **Stöds / stöds inte** | **Kommentarer**  
--- | --- | ---
**Klassisk** | Stöds | Du kan endast replikera en klassisk virtuell dator och återställa den som en klassisk virtuell dator. Du kan inte återställa den som en virtuell dator i Resource Manager. Om du distribuerar en klassisk virtuell dator utan ett virtuellt nätverk och direkt tooan Azure-region, stöds den inte.
**Resource Manager** | Stöds |

## <a name="support-for-replicated-machine-os-versions"></a>Stöd för replikerade datorn OS-versioner

hello nedan stöd gäller för alla arbetsbelastningar som körs på hello nämns OS.

#### <a name="windows"></a>Windows

- Windows Server 2016 (Server Core och Server med Skrivbordsmiljö) *
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 med på minst SP1

>[!NOTE]
>
> \*Windows Server 2016 Nano Server stöds inte.

#### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 6.7, 6.8, 7.0, 7.1, 7.2, 7.3
- CentOS 6.5 6.6, 6.7, 6.8, 7.0, 7.1, 7.2, 7.3
- Ubuntu 14.04 LTS Server [ (kernel-versioner som stöds)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Ubuntu 16.04 LTS Server [ (kernel-versioner som stöds)](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
- Oracle Enterprise Linux 6.4, 6.5 kör hello Red Hat kompatibel kernel eller Unbreakable Enterprise Kernel version 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3

>[!NOTE]
>
> Ubuntu servrar med hjälp av lösenordet autentisering och inloggning, och använda hello molnet init paketet tooconfigure moln virtuella datorer kan har lösenord baserade inloggningen inaktiveras vid växling vid fel (beroende på hello cloudinit konfiguration.) Lösenordsbaserade inloggningen kan återaktiveras på den virtuella datorn hello genom att återställa lösenordet hello hello inställningsmenyn (under hello stöd + felsökning) av hello redundansväxlade virtuella datorn på hello Azure-portalen.

### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Ubuntu kernel-versioner som stöds för virtuella Azure-datorer

**Versionen** | **Mobilitetstjänstversionen** | **Kernel-version** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-Generic too3.13.0-117-generisk<br/>3.16.0-25-Generic too3.16.0-77-generisk<br/>3.19.0-18-Generic too3.19.0-80-generisk<br/>4.2.0-18-Generic too4.2.0-42-generisk<br/>4.4.0-21-Generic too4.4.0-75-generisk |
14.04 LTS | 9.10 | 3.13.0-24-Generic too3.13.0-121-generisk<br/>3.16.0-25-Generic too3.16.0-77-generisk<br/>3.19.0-18-Generic too3.19.0-80-generisk<br/>4.2.0-18-Generic too4.2.0-42-generisk<br/>4.4.0-21-Generic too4.4.0-81-generisk |
16.04 LTS | 9.10 | 4.4.0-21-Generic too4.4.0-81-generisk<br/>4.8.0-34-Generic too4.8.0-56-generisk<br/>4.10.0-14-Generic too4.10.0-24-generisk |

## <a name="supported-file-systems-and-guest-storage-configurations-on-azure-virtual-machines-running-linux-os"></a>Filsystem som stöds och Gäst lagringskonfigurationer på Azure virtuella datorer som kör Operativsystemet Linux

* Filsystem: ext3 ext4, ReiserFS (Suse Linux Enterprise Server bara), XFS
* Volymhanterare: LVM2
* Programvara för flera sökvägar: enheten Mapper

## <a name="region-support"></a>Region-support

Du kan replikera och återställa virtuella datorer mellan de två regionerna inom hello samma geografiska kluster.

**Geografisk kluster** | **Azure-regioner**
-- | --
Nordamerika | Kanada, Öst, Kanada Central, södra centrala USA, västra centrala USA, östra USA, östra USA 2, västra USA, västra USA 2, centrala USA, norra centrala USA
Europa | Storbritannien Väst Storbritannien, Syd Nordeuropa, Västeuropa
Asien | Södra Indien, centrala Indien, Sydostasien, östra Asien, östra samt västra och Korea-Central, Korea Syd
Australien   | Östra Australien, sydost

>[!NOTE]
>
> Du kan replikera för södra regionen och redundans tooone av södra centrala USA, västra centrala USA, östra USA, östra USA 2, västra USA, västra USA 2 och norra centrala USA regioner och misslyckas tillbaka.


## <a name="support-for-compute-configuration"></a>Stöd för beräkningskonfigurationen

**Konfiguration** | **Stöds/stöds ej** | **Kommentarer**
--- | --- | ---
Storlek | Alla Virtuella Azure-datorstorleken med minst 2 CPU-kärnor och 1 GB RAM-minne | Se för[storlekar för virtuella Azure-datorn](../virtual-machines/windows/sizes.md)
Tillgänglighetsuppsättningar | Stöds | Om du använder hello standardalternativet 'Aktivera replikering' steget i portalen är hello tillgänglighetsuppsättning automatiskt skapa baserat på källan region konfiguration. Du kan ändra hello mål tillgänglighetsuppsättning i ' replikerade objekt > Inställningar > beräkning och nätverk > tillgänglighetsuppsättning som helst.
Hybrid Använd förmånen (NAV) virtuella datorer | Stöds | Om Virtuella hello källdatorn har hubb licens aktiverad, hello testa redundans eller Failover VM också använder hello hubb licens.
Skalningsuppsättningar för virtuella datorer | Stöds inte |
Azure Galleriavbildningar - Microsoft publicerat | Stöds | Stöds så länge hello VM som körs på ett operativsystem som stöds av Site Recovery
Azure-galleriet bilder - publicerade från tredje part | Stöds | Stöd för så länge hello VM som körs på ett operativsystem som stöds av Site Recovery.
Anpassad bilder - publicerade från tredje part | Stöds | Stöd för så länge hello VM som körs på ett operativsystem som stöds av Site Recovery.
Virtuella datorer migreras med hjälp av Site Recovery | Stöds | Om det är en VMware/fysiska datorn migreras med hjälp av Site Recovery tooAzure, du behöver toouninstall hello äldre version av mobilitetstjänsten och starta om hello datorn innan du replikerar den tooanother Azure-region.

## <a name="support-for-storage-configuration"></a>Stöd för konfiguration för lagring

**Konfiguration** | **Stöds/stöds ej** | **Kommentarer**
--- | --- | ---
Maximal storlek för OS-disk | 1 023 GB | Se för[diskar som används av virtuella datorer.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Maximal datadiskstorleken | 1 023 GB | Se för[diskar som används av virtuella datorer.](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms)
Antalet datadiskar | Upp till 64 som stöds av en viss Azure VM-storlek | Se för[storlekar för virtuella Azure-datorn](../virtual-machines/windows/sizes.md)
Diskutrymme | Alltid uteslutas från replikering | Tillfällig disklagring har exkluderats från replikering alltid. Du bör inte spärra beständiga data diskutrymme enligt vägledning för Azure. Se för[diskutrymme på Azure Virtual Machines](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk) för mer information.
Data ändra frekvensen på hello disk | Högst 6 Mbit/s per disk | Om hello genomsnittlig data ändrar hastighet på hello disk är utöver 6 Mbit/s kontinuerligt, replikering ska inte fånga. Om det är en databearbetning burst och hello förändringstakten för data som är större än 6 Mbit/s under en viss tid och handlar, fånga replikering upp. I det här fallet kan du se något fördröjd återställningspunkter.
Diskar på standard storage-konton | Stöds |
Diskar på premium storage-konton | Stöds | Om en virtuell dator har diskar som är fördelade på premium- och standard storage-konton, kan du välja ett annat mål storage-konto för varje disk tooensure du har hello samma lagringskonfiguration i målregionen
Hanterad standarddiskar | Stöds inte |  
Hanterad premiumdiskar | Stöds inte |
Lagringsutrymmen | Stöds |         
Kryptering i vila (SSE) | Stöds | Du kan välja ett lagringskonto för SSE aktiverad för cache och mål för lagringskonton.     
Azure Disk Encryption (ADE) | Stöds inte |
Varm Lägg till/ta bort disken | Stöds inte | Om du lägger till eller ta bort datadisk på hello VM behöver toodisable replikering och aktivera replikering igen för hello VM.
Utesluta disk | Stöds inte|   Tillfällig disklagring har exkluderats som standard.
LRS | Stöds |
GRS | Stöds |
RA-GRS | Stöds |
ZRS | Stöds inte |  
Kall och het lagring | Stöds inte | Virtuella diskar stöds inte på kall och het lagring

>[!IMPORTANT]
> Se till att du följer hello [lagringsanvisningar](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) för käll-Azure virtuella datorer tooavoid eventuella prestandaproblem. Om du följer hello standardinställningarna skapar Site Recovery hello krävs lagringskonton baserat på konfigurationen av hello källa. Om du anpassar och välja egna inställningar, se till att du följer hello (... / storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) som dina virtuella källdatorer.

## <a name="support-for-network-configuration"></a>Stöd för konfigurering av nätverk
**Konfiguration** | **Stöds/stöds ej** | **Kommentarer**
--- | --- | ---
Nätverksgränssnitt (NIC) | Upp till maximalt antal nätverkskort som stöds av en viss Azure VM-storlek | Nätverkskort skapas när hello VM skapas som en del av testa redundans eller Redundansåtgärden. hello antalet nätverkskort på hello redundans VM beror på hello antal nätverkskort hello källa VM har hello när du aktiverar replikering. Om du lägger till/ta bort NIC när du har aktiverat replikering, påverkar inte antalet nätverkskort på hello redundans VM.
Internet-belastningsutjämnare | Stöds | Du behöver tooassociate hello förkonfigurerade belastningsutjämning med hjälp av en azure automation-skript i en återställningsplan.
Interna belastningsutjämnare | Stöds | Du behöver tooassociate hello förkonfigurerade belastningsutjämning med hjälp av en azure automation-skript i en återställningsplan.
Offentlig IP-adress| Stöds | Du behöver tooassociate en redan befintlig offentlig IP-toohello NIC eller skapa en och associera toohello NIC med hjälp av en azure automation-skript i en återställningsplan.
NSG på nätverkskortet (Resource Manager)| Stöds | Du måste tooassociate hello NSG toohello NIC med hjälp av en azure automation-skript i en återställningsplan.  
NSG för undernätet (Resource Manager och klassisk)| Stöds | Du måste tooassociate hello NSG toohello NIC med hjälp av en azure automation-skript i en återställningsplan.
NSG för den virtuella datorn (klassisk)| Stöds | Du måste tooassociate hello NSG toohello NIC med hjälp av en azure automation-skript i en återställningsplan.
Reserverade IP: N (statisk IP) / behålla käll-IP | Stöds | Om hello nätverkskortet på Virtuella hello källdatorn har statisk IP-konfiguration och hello mål undernät hello har samma IP som är tillgänglig, den är tilldelad toohello redundans VM. Om hello mål undernät inte har samma IP visas något av hello tillgängliga IP-adresser i hello är hello undernätet reserverad för den här virtuella datorn. Du kan ange en fast IP-adress för valfritt ' replikerade objekt > Inställningar > beräkning och nätverk > nätverksgränssnitt '. Du kan välja hello NIC och ange hello undernät och IP-önskat.
Dynamisk IP| Stöds | Om hello nätverkskortet på Virtuella hello källdatorn har dynamisk IP-konfiguration, hello nätverkskortet på hello redundans VM är också dynamisk som standard. Du kan ange en fast IP-adress för valfritt ' replikerade objekt > Inställningar > beräkning och nätverk > nätverksgränssnitt '. Du kan välja hello NIC och ange hello undernät och IP-önskat.
Traffic Manager-integrering | Stöds | Du konfigurera din traffic manager så att trafik som hello är routade toohello slutpunkt i källan region på regelbunden basis och toohello slutpunkt i målregionen vid redundans.
Azure hanterade DNS | Stöds |
Anpassad DNS  | Stöds |    
Oautentiserad Proxy | Stöds | Se för[nätverk riktlinjerna.](site-recovery-azure-to-azure-networking-guidance.md)    
Autentiserad proxyserver | Stöds inte | Om hello VM använder en autentiserad proxyserver för utgående anslutningar, kan inte replikeras med hjälp av Azure Site Recovery.  
Platsen tooSite VPN med lokalt (med eller utan ExpressRoute)| Stöds | Se till att hello udr: er och NSG: er har konfigurerats så att trafiken hello recovery inte är routade tooon lokala. Se för[nätverk riktlinjerna.](site-recovery-azure-to-azure-networking-guidance.md)  
VNET tooVNET anslutning | Stöds | Se för[nätverk riktlinjerna.](site-recovery-azure-to-azure-networking-guidance.md)  


## <a name="next-steps"></a>Nästa steg
- Lär dig mer om [nätverk vägledning för att replikera virtuella Azure-datorer](site-recovery-azure-to-azure-networking-guidance.md)
- Börja skydda dina arbetsbelastningar av [replikering av virtuella Azure-datorer](site-recovery-azure-to-azure.md)
