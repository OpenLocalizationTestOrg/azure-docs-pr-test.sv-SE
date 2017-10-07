---
title: "aaaPrerequisites för VMware tooAzure replikering med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello krav för att replikera arbetsbelastningar som körs på virtuella VMware-datorer tooAzure, hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 14f8e9713b42a1d8da71223fbbb7a6b7c4303adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-vmware-tooazure-replication"></a>Steg 2: Granska hello krav för VMware tooAzure-replikering

Läsa hello kraven sammanfattas i följande tabell hello.

**Krav** | **Detaljer**
--- | ---
**Azure** | Lär dig mer om [krav för Azure](site-recovery-prereq.md#azure-requirements)
**Lokal konfigurationsserver** | Du behöver en VMware VM kör Windows Server 2012 R2 eller senare. Du konfigurerar den här servern under distributionen av Site Recovery.<br/><br/> Server- och huvudmålservern installeras som standard hello processen också på den här virtuella datorn. När du skalar upp, du kan behöva en separat processerver och hello har samma krav som hello konfigurationsservern.<br/><br/> Lär dig mer om komponenterna [här](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Lokal VMware-servrar** | En eller flera VMware vSphere-servrar, kör 6.5, 6.0, 5.5, 5.1 med senaste uppdateringarna. Servrarna måste finnas i samma nätverk som konfigurationsservern hello (eller separat processerver) hello.<br/><br/> Vi rekommenderar en vCenter-server toomanage värdar som kör 6.5, 6.0 eller 5.5 hello senaste uppdateringarna.
**Lokala virtuella datorer** | Virtuella datorer du vill ska köra tooreplicate en [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). VM ska ha VMware-verktygen körs.
**URL: er** | hello konfigurationsservern behöver åtkomst toothese URL: er:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.<br/></br> Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.<br/></br> Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).<br/><br/> Tillåt URL: en för hello MySQL hämtning: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Mobilitetstjänsten** | Installeras på varje replikerade virtuella datorn.




## <a name="limitations"></a>Begränsningar

Kontrollera att du förstår hello begränsningar som sammanfattas i hello tabell innan du distribuerar.

**Begränsning** | **Detaljer**
--- | ---
**Azure** | Lagring och nätverk konton måste finnas i hello samma region som hello valvet<br/><br/> Om du använder ett premiumlagringskonto måste du också en standard lagra replikeringsloggar toostore för kontot<br/><br/> Du kan replikera toopremium konton i Central och södra Indien.
**Lokal konfigurationsserver** | VMware VM korttypen ska VMXNET3. Om det inte är [installera denna uppdatering](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> vSphere PowerCLI 6.0 ska installeras.<br/><br> hello datorn får inte vara en domänkontrollant. hello datorn bör ha en statisk IP-adress.<br/><br/> hello värdnamn ska vara högst 15 tecken eller mindre, och operativsystemet ska vara på engelska.
**VMware** | Site Recovery har inte stöd för nya vCenter och vSphere 6.5 och 6.0 funktioner såsom mellan vCenter vMotion, virtuella volymerna och lagring DRS.
**Virtuella datorer** | Kontrollera [Virtuella Azure-begränsningar](site-recovery-prereq.md#azure-requirements)<br/><br/> Du kan replikera virtuella datorer med krypterade diskar eller virtuella datorer med UEFI/EFI-start.<br/><br> Delad disk inte stöds. Om Virtuella hello källdatorn har NIC-teamindelning kan konverteras tooa enkel NIC efter växling vid fel.<br/><br/> Om virtuella datorer har en iSCSI-disk, konverterar Site Recovery den tooa VHD-filen efter växling vid fel. Om hello iSCSI-målet kan nås av hello Azure VM, ansluter tooit och ser både den och hello VHD. Om det händer kan du koppla från hello iSCSI-mål.<br/><br/> Om du vill tooenable konsekvens för flera, vilket gör att virtuella datorer som kör samma arbetsbelastning toobe återställas hello peka tillsammans tooa konsekventa data, öppna porten 20004 på hello VM.<br/><br/> Windows måste installeras på hello C-enheten. hello OS-disk ska vara enkla och inte dynamiska. Hej datadisk kan vara dynamisk.<br/><br/> Linux/etc/hosts-filer på virtuella datorer bör innehålla poster som mappar hello lokal värd name tooIP adresser som är associerade med alla nätverkskort. Hej värdnamn, monteringspunkter, enhetsnamn, system-sökvägar och filnamn (/ etc; / usr) ska endast på engelska.<br/><br/> Vissa typer av [Linux lagring](site-recovery-support-matrix-to-azure.md#support-for-storage) stöds.<br/><br/>Skapa eller ange **disk.enableUUID=true** i hello VM-inställningar. Detta ger en konsekvent UUID toohello VMDK, så att den monterar korrekt och säkerställer att endast deltaändringar är överförda tillbaka tooon lokala under återställning efter fel utan fullständig replikering.


## <a name="next-steps"></a>Nästa steg

- Om du utför en fullständig distribution går för[steg3: Planera kapacitet](vmware-walkthrough-capacity.md)
- Om du utför en enkel testdistributionen gå för[steg 4: Planera nätverk](vmware-walkthrough-network.md).
