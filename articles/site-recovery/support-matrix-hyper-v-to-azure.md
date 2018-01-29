---
title: "Stöd matrix för Hyper-V-replikering Azure | Microsoft Docs"
description: "Sammanfattning av de stödda komponenter och krav för Hyper-V-replikering till Azure med Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 5918c56c2b7d01c884bf846e3a7d621b3393bb96
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/03/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>Stöd matrix för Hyper-V-replikering till Azure


Den här artikeln sammanfattar stöds komponenter och inställningar för katastrofåterställning för lokala Hyper-V virtuella datorer till Azure med hjälp av den [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="supported-scenarios"></a>Scenarier som stöds

**Scenario** | **Detaljer**
--- | --- 
**Hyper-V med VMM** | Du kan utföra katastrofåterställning till Azure för virtuella datorer som körs på Hyper-V-värdar som hanteras i infrastrukturresursen System Center Virtual Machine Manager (VMM)<br/><br/> Du kan distribuera det här scenariot i Azure-portalen eller med hjälp av PowerShell.<br/><br/> Du kan också utföra återställning till en sekundär lokal plats när Hyper-V-värdar som hanteras av VMM. Läs [i den här artikeln](tutorial-vmm-to-vmm.md) lära dig mer om det här scenariot.
**Hyper-V utan VMM** | Du kan utföra katastrofåterställning till Azure för virtuella datorer som körs på Hyper-V-värdar som inte hanteras av VMM.<br/><br/> Du kan distribuera det här scenariot i Azure-portalen eller med hjälp av Powershell. 


## <a name="on-premises-servers"></a>Lokala servrar

**Server** | **Krav** | **Detaljer**
--- | --- | ---
**Hyper-V (som körs utan VMM)** | Windows Server 2016, Windows Server 2012 R2 med de senaste uppdateringarna. | När du konfigurerar en Hyper-V-platsen i Site Recovery stöds blanda värdar som kör Windows Server 2016 och 2012 R2 inte.<br/><br/> För virtuella datorer finns på en värd som kör Windows Server 2016, stöds inte återställning till en alternativ plats.
**Hyper-V (som körs med VMM)** | VMM 2016, VMM 2012 R2 | Om VMM används ska Windows Server 2016 värdar hanteras i VMM 2016.<br/><br/> En VMM-moln som kombinerar Hyper-V-värdar som kör Windows Server 2016 och 2012 R2 stöds inte för närvarande.<br/><br/> Miljöer som innehåller en uppgradering av en befintlig VMM 2012 R2-server till 2016 stöds inte.


## <a name="replicated-vms"></a>Replikerade virtuella datorer


I följande tabell sammanfattas VM-stöd. Site Recovery har stöd för alla arbetsbelastningar som körs på ett operativsystem som stöds. 

 **Komponent** | **Detaljer**
--- | ---
VM-konfiguration | Virtuella datorer som replikeras till Azure måste uppfylla [krav för Azure](#failed-over-azure-vm-requirements).
Gästoperativsystemet | Alla gästoperativsystemet [stöds av Azure](https://technet.microsoft.com/library/cc794868.aspx).<br/><br/> Windows Server 2016 Nano Server stöds inte.




## <a name="hyper-v-network-configuration"></a>Konfiguration av Hyper-V-nätverk

**Komponent** | **Hyper-V med VMM** | **Hyper-V utan VMM**
--- | --- | ---
Värden nätverk: NIC-teamindelning | Ja
Värden nätverk: VLAN | Ja
Värden nätverk: IPv4 | Ja
Värden nätverk: IPv6 | Nej
Gäst VM-nätverket: NIC teaming | Nej
Gäst VM-nätverket: IPv4 | Ja
Gäst VM-nätverket: IPv6 | Nej
Gästen Virtuella nätverk: statisk IP (Windows) | Ja
Gästen Virtuella nätverk: statisk IP (Linux) | Nej
Gäst VM-nätverket: Multi-nätverkskort | Ja



## <a name="azure-vm-network-configuration-after-failover"></a>Nätverkskonfigurationen på Azure VM (efter växling vid fel)

**Komponent** | **Hyper-V med VMM** | **Hyper-V utan VMM**
--- | --- | ---
ExpressRoute | Ja | Ja
ILB | Ja | Ja
ELB | Ja | Ja
Traffic Manager | Ja | Ja
Flera nätverkskort | Ja | Ja
Reserverad IP | Ja | Ja
IPv4 | Ja | Ja
Behåll källans IP-adress | Ja | Ja
Slutpunkter för virtuella nätverk<br/><br/> (Azure storage brandväggar och virtuella nätverk) | Nej | Nej


## <a name="hyper-v-host-storage"></a>Lagring för Hyper-V-värd

**Storage** | **Hyper-V med VMM** | Hyper-V utan VMM
--- | --- | --- | ---
NFS | Ej tillämpligt | Ej tillämpligt
SMB 3.0 | Ja | Ja
SAN (ISCSI) | Ja | Ja
Multipath (MPIO). Testats med:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM för CLARiiON | Ja | Ja

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V VM gäst-lagring

**Storage** | **Hyper-V med VMM** | Hyper-V utan VMM
--- | --- | ---
VMDK | Ej tillämpligt | Ej tillämpligt
VHD-ELLER VHDX | Ja | Ja
Generation 2 VM | Ja | Ja
EFI/UEFI| Ja | Ja
Delad klusterdisk | Nej | Nej
Krypterade disk | Nej | Nej
NFS | Ej tillämpligt | Ej tillämpligt
SMB 3.0 | Nej | Nej
RDM | Ej tillämpligt | Ej tillämpligt
Disk > 1 TB | Ja, upp till 4095 GB | Ja, upp till 4095 GB
Disk: 4K logisk och fysisk sektor | Stöds inte: Gen 1/Gen 2 | Stöds inte: Gen 1/Gen 2
Disk: 4K logisk och fysisk 512 byte-sektor | Ja |  Ja
Volymen med stripe disk > 1 TB<br/><br/> Hantering av LVM logiska volymer | Ja | Ja
Lagringsutrymmen | Ja | Ja
Varm Lägg till/ta bort disken | Nej | Nej
Uteslut disk | Ja | Ja
Multipath (MPIO) | Ja | Ja

## <a name="azure-storage"></a>Azure-lagring

**Komponent** | **Hyper-V med VMM** | **Hyper-V utan VMM)**
--- | --- | ---
LRS | Ja | Ja
GRS | Ja | Ja
RA-GRS | Ja | Ja
Lågfrekvent | Nej | Nej
Frekvent| Nej | Nej
Blockblob-objekt | Nej | Nej
Kryptering i rest(SSE)| Ja | Ja
Premium Storage | Ja | Ja
Import/export service | Nej | Nej
VNET Tjänsteslutpunkter (Azure storage brandväggar och Vnet) på målet till cache storage-konto som används för data för replikering | Nej | Nej


## <a name="azure-compute-features"></a>Azure compute-funktioner

**Funktion** | **Hyper-V med VMM** | **Hyper-V utan VMM**
--- | --- | ---
Tillgänglighetsuppsättningar | Ja | Ja
HUBBEN | Ja | Ja  
Hanterade diskar | Ja, för växling vid fel<br/><br/> Återställning efter fel för hanterade diskar stöds inte | Ja, för växling vid fel<br/><br/> Återställning efter fel för hanterade diskar stöds inte

## <a name="azure-vm-requirements"></a>Krav för Azure VM

Lokala virtuella datorer som du replikerar till Azure måste uppfylla kraven för Azure VM som sammanfattas i den här tabellen.

**Komponent** | **Krav** | **Detaljer**
--- | --- | ---
**Gästoperativsystemet** | Site Recovery har stöd för alla operativsystem som är [stöds av Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Kravkontrollen misslyckas om stöds inte.
**Gästen operativsystemets arkitektur** | 64-bitars | Kravkontrollen misslyckas om stöds inte.
**Operativsystemdisken** | Upp till 2 048 GB för virtuella datorer i Generation 1.<br/><br/> Upp till 300 GB för generation 2 virtuella datorer.  | Kravkontrollen misslyckas om stöds inte.
**Operativsystemet disk antal** | 1 | Kravkontrollen misslyckas om stöds inte.
**Datadiskar** | 16 eller mindre  | Kravkontrollen misslyckas om stöds inte.
**Datadisken för virtuell Hårddisk** | Upp till 4095 GB | Kravkontrollen misslyckas om stöds inte.
**Nätverkskort** | Flera nätverkskort som stöds |
**Delad virtuell Hårddisk** | Stöds inte | Kravkontrollen misslyckas om stöds inte.
**FC-disk** | Stöds inte | Kravkontrollen misslyckas om stöds inte.
**Format för hårddisk** | VIRTUELL HÅRDDISK <br/><br/> VHDX | Site Recovery konverterar automatiskt VHDX till virtuell Hårddisk när du redundansväxlar till Azure. När du växlar tillbaka till lokala virtuella datorer att fortsätta att använda VHDX-format.
**BitLocker** | Stöds inte | BitLocker måste inaktiveras innan du aktiverar replikering för en virtuell dator.
**Namn på virtuell dator** | Mellan 1 och 63 tecken. Begränsat till bokstäver, siffror och bindestreck. VM-namn måste börja och sluta med en bokstav eller siffra. | Uppdatera värdet i VM-egenskaper i Site Recovery.
**VM-typ** | Generation 1<br/><br/> Generation 2 – Windows | Generation 2 virtuella datorer med en OS-disktyp av grundläggande (som innehåller en eller två datavolymer som formaterats som VHDX) och mindre än 300 GB diskutrymme stöds.<br></br>Linux Generation 2 virtuella datorer stöds inte. [Läs mer](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="recovery-services-vault-actions"></a>Recovery Services-valvet åtgärder

**Åtgärd** |  **Hyper-V med VMM** | **Hyper-V utan VMM**
--- | --- | --- 
Flytta valvet mellan resursgrupper<br/><br/> Inom och över prenumerationer | Nej | Nej 
Flytta lagring, nätverk, virtuella datorer i Azure över resursgrupper<br/><br/> Inom och över prenumerationer | Nej | Nej 


## <a name="provider-and-agent"></a>Providern och agenten

Kontrollera att du kör den senaste providern och agentversioner för att kontrollera distributionen är kompatibel med inställningarna i den här artikeln.

**Namn** | **Beskrivning** | **Detaljer**
--- | --- | --- | --- | ---
**Azure Site Recovery-providern** | Samordnar kommunikationen mellan lokala servrar och Azure <br/><br/> Hyper-V med VMM: installerat på VMM-servrar<br/><br/> Hyper-V utan VMM: installerad på Hyper-V-värdar| Senaste version: 5.1.2700.1 (tillgänglig från portalen)<br/><br/> [Senaste funktionerna och korrigeringarna](https://aka.ms/latest_asr_updates)
**Microsoft Azure Recovery Services MARS-agenten** | Samordnar replikering mellan Hyper-V virtuella datorer och Azure<br/><br/> Installerad på lokala Hyper-V-servrar (med eller utan VMM) | Senaste agenten som är tillgängliga från portalen






## <a name="next-steps"></a>Nästa steg
Lär dig hur du [förbereda Azure](tutorial-prepare-azure.md) för katastrofåterställning för lokala Hyper-V virtuella datorer. 
