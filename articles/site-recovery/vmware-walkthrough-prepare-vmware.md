---
title: "aaaPrepare lokal VMware-resurser för replikering tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen för att replikera arbetsbelastningar som körs på virtuella VMware-datorer tooAzure lagring"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 09d81f15f6ee764135a62f5555e458c55fa30048
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-on-premises-vmware-replication-tooazure"></a>Steg 6: Förbereda tooAzure för lokal VMware-replikering

Använd hello instruktioner i den här artikeln tooprepare lokal VMware-servrar toointeract med Azure Site Recovery och förbereda virtuella VMWare-datorer för installation av hello mobilitetstjänsten. Hej mobilitetstjänstagenten måste installeras på alla lokala virtuella datorer som du vill tooreplicate tooAzure.

## <a name="prepare-for-automatic-discovery"></a>Förbereda för automatisk identifiering

Site Recovery upptäcker automatiskt virtuella datorer som körs på vSphere ESXi-värdar (med eller utan en vCenter-server). För automatisk identifiering, Site Recovery måste ett konto tooaccess värdar och -servrar:

1. toouse ett särskilt konto skapar du en roll (på hello vCenter nivå med hello behörigheterna som beskrivs i hello nedan. Ge det ett namn som **Azure_Site_Recovery**.
2. Sedan skapa en användare på hello vSphere-värd/vCenter-servern och tilldela hello rollen toohello användare. Du kan ange det här användarkontot under distributionen av Site Recovery.


### <a name="vmware-account-permissions"></a>Behörighet för VMware

Site Recovery behöver åtkomst tooVMware för hello processen server tooautomatically identifiera virtuella datorer och redundans och återställning av virtuella datorer.

- **Migrera**: Om du bara vill toomigrate virtuella VMware-datorer tooAzure utan att någonsin återställas dem. Du kan använda en VMware-konto med en skrivskyddad roll. Sådana rollen kan köra växling vid fel, men det går inte att stänga av skyddade källdatorer. Detta är inte nödvändigt för migrering.
- **Replikera eller återställa**: Om du vill toodeploy fullständig replikering (replikering, redundans, återställning) hello kontot måste vara kan toorun åtgärder, till exempel skapa och ta bort diskar, startar VMs osv.
- **Automatisk identifiering**: krävs minst ett konto för skrivskyddad.


**Aktivitet** | **Nödvändiga konto-rollen** | **Behörigheter** | **Detaljer**
--- | --- | --- | ---
**Processervern identifierar automatiskt virtuella VMware-datorer** | Du behöver minst en skrivskyddad användare | Data Center objektet –> sprida tooChild objekt, roll = skrivskyddad | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).
**Växling vid fel** | Du behöver minst en skrivskyddad användare | Data Center objektet –> sprida tooChild objekt, roll = skrivskyddad | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).<br/><br/> Användbart för migrering, men inte fullständig replikering, redundans och återställning efter fel.
**Redundans och återställning efter fel** | Vi rekommenderar att du skapar en roll (Azure_Site_Recovery) med behörigheter för hello som krävs och tilldela sedan hello rollen tooa VMware användare eller grupp | Data Center objektet –> sprida tooChild objekt, roll = Azure_Site_Recovery<br/><br/> DataStore -> allokera utrymme, bläddra datalagret, låg nivå filåtgärder, ta bort filen och uppdatera filer för virtuella datorer<br/><br/> Nätverk -> nätverk tilldela<br/><br/> Resurs -> Tilldela VM tooresource pool, migrera är avstängt VM, migrera driven på den virtuella datorn<br/><br/> Aktiviteter -> Skapa uppgift, uppdatera uppgift<br/><br/> Konfiguration av virtuell dator -><br/><br/> Virtual machine -> interagera -> fråga enhetsanslutning, konfigurera CD-skivor, konfigurera diskettenheter media, stänga av, slå på strömmen, installera för VMware-verktyg<br/><br/> Virtual machine -> Lager -> Skapa, registrera, avregistrera<br/><br/> Etablering av virtuell dator -> -> Tillåt virtuella hämtning, tillåter Överför filer för virtuella datorer<br/><br/> Virtual machine -> ögonblicksbilder -> Ta bort ögonblicksbilder | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).


## <a name="prepare-for-push-installation-of-hello-mobility-service"></a>Förbereda för push-installation av hello mobilitetstjänsten

Hej mobilitetstjänsten måste vara installerad på alla virtuella datorer du vill tooreplicate. Det finns ett antal sätt tooinstall hello tjänsten, inklusive manuell installation, push-installation från hello Site Recovery processervern och metoder som System Center Configuration Manager-installationen.

Om du vill toouse push-installation, måste tooprepare ett konto som Site Recovery kan använda tooaccess hello VM.

- Du kan använda en domän eller lokalt konto
- För Windows, om du inte använder ett domänkonto måste toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator. toodo, hello registrera under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, lägga till DWORD-posten för hello **LocalAccountTokenFilterPolicy**, med värdet 1.
- Om du vill tooadd hello registerposten för Windows från en CLI, skriver du:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- För Linux ska hello kontot vara rot på hello Linux källservern.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 7: skapa ett valv](vmware-walkthrough-create-vault.md)
