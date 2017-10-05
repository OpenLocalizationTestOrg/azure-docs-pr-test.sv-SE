---
title: "Stöd matrix för replikering till en sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattning av de operativsystem som stöds och komponenter för Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: raynew
ms.openlocfilehash: db7ee5251f2e2016081e55ca4b295e284c8b08cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="support-matrix-for-replication-to-a-secondary-site-with-azure-site-recovery"></a>Stöd matrix för replikering till en sekundär plats med Azure Site Recovery

Den här artikeln sammanfattar vad stöds när du använder Azure Site Recovery replikera till en sekundär lokal plats.

## <a name="deployment-options"></a>Distributionsalternativ

**Distribution** | **VMware/fysisk server** | **Hyper-V (med eller utan SCVMM)**
--- | --- | --- | ---
**Azure Portal** | Lokala virtuella VMware-datorer till sekundär VMware-plats.<br/><br/> Hämta den [InMage Scout användarhandboken](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) (inte tillgängligt i Azure portal). | Lokal Hyper-V virtuella datorer i VMM-moln till en sekundär VMM-moln.<br></br> Stöds inte utan VMM  <br/><br/> Standard Hyper-V-replikering endast. SAN som inte stöds.
**Klassisk portal** | Underhållsläge. Det går inte att skapa nya valv. | Endast underhållsläge<br></br> Stöds inte utan SCVMM
**PowerShell** | Stöds inte | Stöds<br></br> Stöds inte utan SCVMM

## <a name="on-premises-servers"></a>Lokala servrar

### <a name="virtualization-servers"></a>Virtualiseringsservrar

**Distribution** | **Support**
--- | ---
**VMware-dator/fysisk server** | vSphere 6.0, 5.5 eller 5.1 med senaste uppdateringen
**Hyper-V (med VMM)** | VMM 2016 och VMM 2012 R2

  >[!Note]
  > VMM 2016 moln med en blandning av Windows Server 2016 och 2012 R2-värdar stöds inte för närvarande.

### <a name="host-servers"></a>Värdservrar för fjärrskrivbordssessioner

**Distribution** | **Support**
--- | ---
**VMware-dator/fysisk server** | vCenter 5.5 eller 6.0 (stöd för 5.5 funktioner)
**Hyper-V (ingen VMM)** | Inte en konfiguration för att replikera till en sekundär plats som stöds
**Hyper-V med VMM** | Windows Server 2016 och Windows Server 2012 R2 med de senaste uppdateringarna.<br/><br/> Windows Server 2016 värdar ska hanteras av VMM 2016.

## <a name="support-for-replicated-machine-os-versions"></a>Stöd för replikerade datorn OS-versioner
I följande tabell sammanfattas operativsystem i olika distributionsscenarier uppstod med hjälp av Azure Site Recovery. Detta stöd gäller för alla arbetsbelastningar som körs på nämnda OS.

**VMware/fysisk server** | **Hyper-V (med VMM)**
--- | --- | ---
64-bitars Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 med på minst SP1<br/><br/> Red Hat Enterprise Linux 6.7, 7.1, 7.2 <br/><br/> Centos 6.5 6.6, 6.7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4 eller 6.5 kör Red Hat kompatibel kerneln eller Unbreakable Enterprise Kernel version 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 | Någon gästen operativsystemet [stöds av Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)

>[!Note]
>Linux-baserade datorer med följande lagringen kan replikeras: File system (EXT3, ETX4, ReiserFS, XFS); Multipath programvara enhet Mapper; Volymhanterare (LVM2).
>Fysiska servrar med HP CCISS domänkontrollant lagring stöds inte.
>Filsystemet ReiserFS stöds endast för SUSE Linux Enterprise Server 11 SP3.

## <a name="network-configuration"></a>Nätverkskonfiguration

### <a name="hosts"></a>Värdar

**Konfiguration** | **VMware/fysisk server** | **Hyper-V (med VMM)**
--- | --- | ---
NIC-teamindelning | Ja | Ja
VLAN | Ja | Ja
IPv4 | Ja | Ja
IPv6 | Nej | Nej

### <a name="guest-vms"></a>Gäst-VM

**Konfiguration** | **VMware/fysisk server** | **Hyper-V (med VMM)**
--- | --- | ---
NIC-teamindelning | Nej | Nej
IPv4 | Ja | Ja
IPv6 | Nej | Nej
Statisk IP-adress (Windows) | Ja | Ja
Statisk IP-adress (Linux) | Ja | Ja
Flera nätverkskort | Ja | Ja


## <a name="storage"></a>Lagring

### <a name="host-storage"></a>Värd storage

**Lagring (värd)** | **VMware/fysisk server** | **Hyper-V (med VMM)**
--- | --- | ---
NFS | Ja | Saknas
SMB 3.0 | Saknas | Ja
SAN (ISCSI) | Ja | Ja
Multipath (MPIO) | Ja | Ja

### <a name="guest-or-physical-server-storage"></a>Gästen eller fysisk serverlagring

**Konfiguration** | **VMware/fysisk server** | **Hyper-V (med VMM)**
--- | --- | ---
VMDK | Ja | Saknas
VHD-ELLER VHDX | Saknas | Ja (upp till 16 diskar)
Generation 2 VM | Saknas | Ja
Delad klusterdisk | Ja  | Nej
Krypterade disk | Nej | Nej
UEFI| Nej | Saknas
NFS | Nej | Nej
SMB 3.0 | Nej | Nej
RDM | Ja | Saknas
Disk > 1 TB | Nej | Ja
Volymen med stripe disk > 1 TB<br/><br/> LVM | Ja | Ja
Lagringsutrymmen | Nej | Ja
Varm Lägg till/ta bort disken | Nej | Nej
Utesluta disk | Nej | Ja
Multipath (MPIO) | Saknas | Ja

## <a name="vaults"></a>Valv

**Åtgärd** | **VMware/fysisk server** | **Hyper-V (med VMM)**
--- | --- | ---
Flytta valv mellan resursgrupper (inom eller mellan prenumerationer) | Nej | Nej
Flytta lagring, nätverk, virtuella datorer i Azure över resursgrupper (inom eller mellan prenumerationer) | Nej | Nej

## <a name="provider-and-agent"></a>Provider och agent

**Namn** | **Beskrivning** | **Senaste versionen** | **Detaljer**
--- | --- | --- | --- | ---
**Azure Site Recovery-providern** | Samordnar kommunikationen mellan lokala servrar och Azure <br/><br/> Installeras på lokal VMM-servrar, eller på Hyper-V-servrar om det finns ingen VMM-server | 5.1.19 ([tillgänglig från portalen](http://aka.ms/downloaddra)) | [Senaste funktionerna och korrigeringarna](https://support.microsoft.com/kb/3155002)
**Mobilitetstjänsten** | Samordnar replikering mellan lokala VMware-servrar eller fysiska servrar och den sekundära platsen<br/><br/> Installerad på VMware VM eller fysiska servrar som du vill replikera  | Ej tillämpligt (tillgänglig från portalen) | Saknas


## <a name="next-steps"></a>Nästa steg

- [Replikera virtuella Hyper-V-datorer i VMM-moln till en sekundär plats](site-recovery-vmm-to-vmm.md)
- [Replikera virtuella VMware-datorer och fysiska servrar till en sekundär plats](site-recovery-vmware-to-vmware.md)
