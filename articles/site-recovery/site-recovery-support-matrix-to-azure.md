---
title: "aaaAzure Site Recovery stöd matrix för att replikera tooAzure | Microsoft Docs"
description: "Sammanfattar hello stöds operativsystem och komponenter för Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajanaki
ms.openlocfilehash: eae1db2ff1392d272f6b2eb0e3410da19d09da7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-support-matrix-for-replicating-from-on-premises-tooazure"></a>Azure Site Recovery stöd matrix för replikering från lokala tooAzure


Den här artikeln beskriver konfigurationer som stöds och komponenter för Azure Site Recovery när replikering och återställer tooAzure. Mer information om krav för Azure Site Recovery finns hello [krav](site-recovery-prereq.md).


## <a name="support-for-deployment-options"></a>Stöd för distributionsalternativ

**Distribution** | **VMware/fysisk server** | **Hyper-V (med/utan Virtual Machine Manager)** |
--- | --- | ---
**Azure Portal** | Lokal lagring för virtuella VMware-datorer tooAzure, med Azure Resource Manager eller klassiska lagring och nätverk.<br/><br/> Redundans tooResource Manager-baserade eller klassiska virtuella datorer. | Lokal Hyper-V VMs tooAzure lagring, med Resource Manager eller klassiska lagring och nätverk.<br/><br/> Redundans tooResource Manager-baserade eller klassiska virtuella datorer.
**Klassisk portal** | Underhållsläge. Det går inte att skapa nya valv. | Underhållsläge.
**PowerShell** | Stöds inte för närvarande. | Stöds


## <a name="support-for-datacenter-management-servers"></a>Stöd för datacenter hanteringsservrar

### <a name="virtualization-management-entities"></a>Virtualisering hantering av enheter

**Distribution** | **Support**
--- | ---
**VMware-dator/fysisk server** | vCenter 6.5, 6.0 eller 5.5
**Hyper-V (med Virtual Machine Manager)** | System Center Virtual Machine Manager 2016 och System Center Virtual Machine Manager 2012 R2

  >[!Note]
  > Ett System Center Virtual Machine Manager 2016 moln med en blandning av Windows Server 2016 och 2012 R2-värdar stöds inte för närvarande.

### <a name="host-servers"></a>Värdservrar för fjärrskrivbordssessioner

**Distribution** | **Support**
--- | ---
**VMware-dator/fysisk server** | vSphere 6.5, 6.0, 5.5
**Hyper-V (med/utan Virtual Machine Manager)** | Windows Server 2016, Windows Server 2012 R2 med de senaste uppdateringarna.<br></br>Om SCVMM används ska Windows Server 2016 värdar hanteras av SCVMM 2016.


  >[!Note]
  >En Hyper-V-plats som kombinerar värdar som kör Windows Server 2016 och 2012 R2 stöds inte för närvarande. Tooan alternativ återställningsplats för virtuella datorer på en Windows Server 2016-värden stöds inte för närvarande.

## <a name="support-for-replicated-machine-os-versions"></a>Stöd för replikerade datorn OS-versioner

Virtuella datorer som skyddas måste uppfylla [krav för Azure](#failed-over-azure-vm-requirements) vid replikering av tooAzure.
hello följande tabell sammanfattas replikerade operativsystem i olika distributionsscenarier när du använder Azure Site Recovery. Detta stöd gäller för alla arbetsbelastningar som körs på hello nämns OS.

 **VMware/fysisk server** | **Hyper-V (med eller utan VMM)** |
--- | --- |
64-bitars Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 med på minst SP1<br/>*Windows Server 2016* – inte stöds för närvarande på virtuella VMware-datorer och fysiska servrar. <br/><br/> Red Hat Enterprise Linux: 5.2 too5.11, 6.1 too6.8, 7.0 too7.3 <br/><br/>Cent OS: 5.2 too5.11, 6.1 too6.8, 7.0 too7.3 <br/><br/>Ubuntu 14.04 LTS server[ (kernel-versioner som stöds)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Ubuntu 16.04 LTS server[ (kernel-versioner som stöds)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Oracle Enterprise Linux 6.4, 6.5 kör hello Red Hat kompatibel kernel eller Unbreakable Enterprise Kernel version 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> SUSE Linux Enterprise Server 11 SP4 <br/>(Uppgradering av replikering datorer från SLES 11 SP3 tooSLES 11 SP4 stöds inte. Om en replikerad dator har uppgraderats från SLES 11SP3 tooSLES 11 SP4, du behöver toodisable replikering och skydda hello dator igen efter uppgraderingen hello.) | Alla gästoperativsystemet [stöds av Azure](https://technet.microsoft.com/library/cc794868.aspx)


>[!IMPORTANT]
>(Gäller tooVMware/fysiska servrar som replikerar tooAzure)
>
> Red Hat Enterprise Linux Server 7 + och CentOS 7 + servrar stöds kernel version 3.10.0-514 på version 9.8 av hello Azure Site Recovery-mobilitetstjänsten från.<br/><br/>
> Kunder på hello 3.10.0-514 kernel med en version av hello mobilitetstjänsten som är lägre än versionen 9.8 är obligatoriska toodisable replikering, hello Uppdateringsversion av hello Mobility service tooversion 9.8 och sedan aktivera replikering igen.


### <a name="supported-ubuntu-kernel-versions-for-vmwarephysical-servers"></a>Ubuntu kernel-versioner som stöds för VMware/fysiska servrar

**Versionen** | **Mobilitetstjänstversionen** | **Kernel-version** |
--- | --- | --- |
14.04 LTS | 9.9 | 3.13.0-24-Generic too3.13.0-117-generisk<br/>3.16.0-25-Generic too3.16.0-77-generisk<br/>3.19.0-18-Generic too3.19.0-80-generisk<br/>4.2.0-18-Generic too4.2.0-42-generisk<br/>4.4.0-21-Generic too4.4.0-75-generisk |
14.04 LTS | 9.10 | 3.13.0-24-Generic too3.13.0-121-generisk<br/>3.16.0-25-Generic too3.16.0-77-generisk<br/>3.19.0-18-Generic too3.19.0-80-generisk<br/>4.2.0-18-Generic too4.2.0-42-generisk<br/>4.4.0-21-Generic too4.4.0-81-generisk |
16.04 LTS | 9.10 | 4.4.0-21-Generic too4.4.0-81-generisk<br/>4.8.0-34-Generic too4.8.0-56-generisk<br/>4.10.0-14-Generic too4.10.0-24-generisk |


## <a name="supported-file-systems-and-guest-storage-configurations-on-linux-vmwarephysical-servers"></a>Filsystem som stöds och Gäst lagringskonfigurationer på Linux (VMware/fysiska servrar)

hello följande fil system och lagring configuration programvaran stöds på Linux-servrar som körs på VMware eller fysiska servrar:
* Filsystem: ext3 ext4, ReiserFS (Suse Linux Enterprise Server bara), XFS
* Volymhanterare: LVM2
* Programvara för flera sökvägar: enheten Mapper

Paravirtualized lagringsenheter (exporteras av paravirtualized drivrutiner) stöds inte.<br/>
Flera kön blockera-i/o-enheter stöds inte.<br/>
Fysiska servrar med hello HP CCISS lagringsstyrenhet stöds inte.<br/>

>[!Note]
> På Linux-servrar hello följande kataloger (om konfigurerad som separata partitioner /-filsystem) måste vara på hello samma disk (hello OS-disk) på källservern för hello: / (rot), / Boot/usr, /usr/local, /var, / etc<br/><br/>
> XFSv5 funktioner på XFS filsystem, till exempel kontrollsumma metadata stöds från och med version 9.10 av hello mobilitetstjänsten. Om du använder XFSv5 funktioner, se till att du använder Mobilitetstjänsten version 9.10 eller senare. Du kan använda hello xfs_info verktyget toocheck hello XFS superblock för hello partition. Om ftype anges too1 sedan XFSv5 funktionerna används.
>


## <a name="support-for-network-configuration"></a>Stöd för konfigurering av nätverk
hello följande tabeller sammanfattar stöd för konfiguration av nätverk i olika distributionsscenarier som använder Azure Site Recovery tooreplicate tooAzure.

### <a name="host-network-configuration"></a>Nätverkskonfiguration för värden

**Konfiguration** | **VMware/fysisk server** | **Hyper-V (med/utan Virtual Machine Manager)**
--- | --- | ---
NIC-teamindelning | Ja<br/><br/>Stöds inte när fysiska datorer replikeras| Ja
VLAN | Ja | Ja
IPv4 | Ja | Ja
IPv6 | Nej | Nej

### <a name="guest-vm-network-configuration"></a>Konfiguration för gäst-VM-nätverk

**Konfiguration** | **VMware/fysisk server** | **Hyper-V (med/utan Virtual Machine Manager)**
--- | --- | ---
NIC-teamindelning | Nej | Nej
IPv4 | Ja | Ja
IPv6 | Nej | Nej
Statisk IP-adress (Windows) | Ja | Ja
Statisk IP-adress (Linux) | Ja <br/><br/>Virtuella datorer är konfigurerade toouse DHCP på återställning efter fel  | Nej
Flera nätverkskort | Ja | Ja

### <a name="failed-over-azure-vm-network-configuration"></a>Det gick inte över Azure VM nätverkskonfigurationen

**Azure-nätverk** | **VMware/fysisk server** | **Hyper-V (med/utan Virtual Machine Manager)**
--- | --- | ---
Express Route | Ja | Ja
ILB | Ja | Ja
ELB | Ja | Ja
Traffic Manager | Ja | Ja
Flera nätverkskort | Ja | Ja
Reserverad IP | Ja | Ja
IPv4 | Ja | Ja
Behåll käll-IP | Ja | Ja


## <a name="support-for-storage"></a>Stöd för lagring
hello följande tabeller sammanfattar stöd för konfiguration av lagringsutrymme i olika distributionsscenarier som använder Azure Site Recovery tooreplicate tooAzure.

### <a name="host-storage-configuration"></a>Värdkonfiguration för lagring

**Konfiguration** | **VMware/fysisk server** | **Hyper-V (med/utan Virtual Machine Manager)**
--- | --- | --- | ---
NFS | Ja för VMware<br/><br/> Nej för fysiska servrar | Saknas
SMB 3.0 | Saknas | Ja
SAN (ISCSI) | Ja | Ja
Multipath (MPIO)<br></br>Testats med: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM för CLARiiON | Ja | Ja

### <a name="guest-or-physical-server-storage-configuration"></a>Gäst eller fysisk server lagringskonfigurationen

**Konfiguration** | **VMware/fysisk server** | **Hyper-V (med/utan Virtual Machine Manager)**
--- | --- | ---
VMDK | Ja | Saknas
VHD-ELLER VHDX | Saknas | Ja
Generation 2 VM | Saknas | Ja
EFI/UEFI| Nej | Ja
Delad klusterdisk | Nej | Nej
Krypterade disk | Nej | Nej
NFS | Nej | Saknas
SMB 3.0 | Nej | Nej
RDM | Ja<br/><br/> Ej tillämpligt för fysiska servrar | Saknas
Disk > 1 TB | Ja<br/><br/>Upp till 4095 GB | Ja<br/><br/>Upp till 4095 GB
Disk med 4K sektorstorlek | Ja | Ja, stöd för Generation 1 virtuella datorer<br/><br/>Stöds inte för Generation 2 virtuella datorer.
Volymen med stripe disk > 1 TB<br/><br/> Hantering av LVM logiska volymer | Ja | Ja
Lagringsutrymmen | Nej | Ja
Varm Lägg till/ta bort disken | Nej | Nej
Utesluta disk | Ja | Ja
Multipath (MPIO) | Saknas | Ja

**Azure Storage** | **VMware/fysisk server** | **Hyper-V (med/utan Virtual Machine Manager)**
--- | --- | ---
LRS | Ja | Ja
GRS | Ja | Ja
RA-GRS | Ja | Ja
Lågfrekvent | Nej | Nej
Frekvent| Nej | Nej
Kryptering i rest(SSE)| Ja | Ja
Premium Storage | Ja | Ja
Import/export service | Nej | Nej


## <a name="support-for-azure-compute-configuration"></a>Stöd för Azure compute-konfiguration

**Compute-funktion** | **VMware/fysisk server** | **Hyper-V (med/utan Virtual Machine Manager)**
--- | --- | --- 
Tillgänglighetsuppsättningar | Ja | Ja
HUBBEN | Ja | Ja  
Hanterade diskar | Ja | Ja<br/><br/>Återställning efter fel tooon lokala från virtuella Azure-datorn med hanterade diskar stöds inte för närvarande.

## <a name="failed-over-azure-vm-requirements"></a>Det gick inte över Azure VM krav

Du kan distribuera Site Recovery tooreplicate virtuella datorer och fysiska servrar som kör ett operativsystem som stöds av Azure. Detta omfattar de flesta versioner av Windows och Linux. Lokala virtuella datorer som du vill tooreplicate måste överensstämma med följande krav för Azure vid replikering av tooAzure hello.

**Entitet** | **Krav** | **Detaljer**
--- | --- | ---
**Gästoperativsystemet** | TooAzure för Hyper-V-replikering: Site Recovery har stöd för alla operativsystem som är [stöds av Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> För VMware och fysiska servrar replikering: Kontrollera hello Windows och Linux [krav](site-recovery-vmware-to-azure-classic.md) | Kravkontrollen misslyckas om stöds inte.
**Gästen operativsystemets arkitektur** | 64-bitars | Kravkontrollen misslyckas om stöds inte
**Operativsystemdisken** | Upp too2048 GB om du replikerar **virtuella VMware-datorer eller fysiska servrar tooAzure**.<br/><br/>Upp till 2048 GB för **Hyper-V Generation 1** virtuella datorer.<br/><br/>Upp till 300 GB för **Hyper-V Generation 2 virtuella datorer**.  | Kravkontrollen misslyckas om stöds inte
**Operativsystemet disk antal** | 1 | Kravkontrollen misslyckas om stöds inte.
**Datadiskar** | 64 eller mindre om du replikerar **virtuella VMware-datorer tooAzure**; 16 eller mindre om du replikerar **tooAzure för Hyper-V virtuella datorer** | Kravkontrollen misslyckas om stöds inte
**Datadisken för virtuell Hårddisk** | Konfigurera too4095 GB | Kravkontrollen misslyckas om stöds inte
**Nätverkskort** | Flera nätverkskort som stöds |
**Delad virtuell Hårddisk** | Stöds inte | Kravkontrollen misslyckas om stöds inte
**FC-disk** | Stöds inte | Kravkontrollen misslyckas om stöds inte
**Format för hårddisk** | VIRTUELL HÅRDDISK <br/><br/> VHDX | Även om VHDX inte stöds för närvarande i Azure, konverterar Site Recovery automatiskt VHDX tooVHD när du redundansväxlar tooAzure. När du växlar tillbaka fortsätter tooon lokala hello virtuella datorer toouse hello VHDX-format.
**BitLocker** | Stöds inte | BitLocker måste inaktiveras innan du skyddar en virtuell dator.
**Namn på virtuell dator** | Mellan 1 och 63 tecken. Begränsad tooletters, siffror och bindestreck. hello VM-namn måste börja och sluta med en bokstav eller siffra. | Uppdatera hello värde i hello egenskaper för virtuell dator i Site Recovery.
**VM-typ** | Generation 1<br/><br/> Generation 2 – Windows | Generation 2 virtuella datorer med en OS-disktyp av grundläggande (som innehåller en eller två datavolymer som formaterats som VHDX) och mindre än 300 GB diskutrymme stöds.<br></br>Linux Generation 2 virtuella datorer stöds inte. [Läs mer](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="support-for-recovery-services-vault-actions"></a>Stöd för åtgärder som Recovery Services-valvet

**Åtgärd** | **VMware/fysisk server** | **Hyper-V (ingen Virtual Machine Manager)** | **Hyper-V (med Virtual Machine Manager)**
--- | --- | --- | ---
Flytta valvet mellan resursgrupper<br/><br/> Inom och över prenumerationer | Nej | Nej | Nej
Flytta lagring, nätverk, virtuella datorer i Azure över resursgrupper<br/><br/> Inom och över prenumerationer | Nej | Nej | Nej


## <a name="support-for-provider-and-agent"></a>Stöd för providern och agenten

**Namn** | **Beskrivning** | **Senaste versionen** | **Detaljer**
--- | --- | --- | --- | ---
**Azure Site Recovery-providern** | Samordnar kommunikationen mellan lokala servrar och Azure <br/><br/> Installerad på lokala Virtual Machine Manager-servrar eller på Hyper-V-servrar om det finns ingen Virtual Machine Manager-server | 5.1.19 ([tillgänglig från portalen](http://aka.ms/downloaddra)) | [Senaste funktionerna och korrigeringarna](https://support.microsoft.com/kb/3155002)
**Azure Site Recovery Unified installationsprogram (VMware tooAzure)** | Samordnar kommunikationen mellan lokala VMware-servrar och Azure <br/><br/> Installerad på lokal VMware-servrar | 9.3.4246.1 (tillgänglig från portalen) | [Senaste funktionerna och korrigeringarna](https://support.microsoft.com/kb/3155002)
**Mobilitetstjänsten** | Samordnar replikering mellan lokala VMware-servrar/fysiska servrar och Azure/sekundär plats<br/><br/> Installerad på VMware VM eller fysiska servrar vill du tooreplicate  | Ej tillämpligt (tillgänglig från portalen) | Saknas
**Microsoft Azure Recovery Services MARS-agenten** | Samordnar replikering mellan Hyper-V virtuella datorer och Azure<br/><br/> Installerad på lokala Hyper-V-servrar (med eller utan en Virtual Machine Manager-server) | Senaste agenten ([tillgänglig från portalen](http://aka.ms/latestmarsagent)) |






## <a name="next-steps"></a>Nästa steg
[Kontrollera krav](site-recovery-prereq.md)
