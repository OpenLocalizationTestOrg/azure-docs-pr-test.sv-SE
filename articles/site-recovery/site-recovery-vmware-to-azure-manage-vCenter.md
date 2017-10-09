---
title: " Hantera en VMware vCenter-server i Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur lägga till och hantera VMware vCenter i Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 5be995f137d0c0efaf3050b5366a107098cae15a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-vmware-vcenter-server-in-azure-site-recovery"></a>Hantera VMware vCenter-servern i Azure Site Recovery
Den här artikeln beskrivs hello olika Site Recovery-åtgärder som kan utföras på en VMware vCenter.

## <a name="prerequisites"></a>Krav

**Stöd för VMware vCenter- och VMware vSphere ESX-värd** | **Detaljer** |
|--- | --- |
|**Lokal VMware-servrar** | En eller flera VMware vSphere-servrar, kör 6.0, 5.5, 5.1 med senaste uppdateringarna. Servrarna måste finnas i samma nätverk som konfigurationsservern hello (eller separat processerver) hello.<br/><br/> Vi rekommenderar en vCenter server toomanage-värdar som kör 6.0 eller 5.5 med hello senaste uppdateringarna. Funktioner som är tillgängliga i 5.5 stöds när du distribuerar version 6.0.|

## <a name="prepare-an-account-for-automatic-discovery"></a>Förbereda ett konto för automatisk identifiering
Site Recovery behöver åtkomst tooVMware för hello processen server tooautomatically Upptäck virtuella datorer och redundans och återställning av virtuella datorer.

* **Migrera**: Om du bara vill toomigrate VMware virtuella datorer tooAzure utan att någonsin återställas dem. Du kan använda en VMware-konto med en skrivskyddad roll. Sådana rollen kan köra växling vid fel, men det går inte att stänga av skyddade källdatorer. Detta är inte nödvändigt för migrering.
* **Replikera eller återställa**: Om du vill toodeploy fullständig replikering (replikering, redundans, återställning) hello kontot måste vara kan toorun åtgärder, till exempel skapa och ta bort diskar, startar på den virtuella datorn.
* **Automatisk identifiering**: krävs minst ett konto för skrivskyddad.


|**Uppgifter** | **Nödvändiga konto-rollen** | **Behörigheter** | **Detaljer**|
|--- | --- | --- | ---|
|**Processervern identifierar automatiskt virtuella VMware-datorer** | Du behöver minst en skrivskyddad användare | Data Center objektet –> sprida tooChild objekt, roll = skrivskyddad | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt, toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).|
|**Växling vid fel** | Du behöver minst en skrivskyddad användare | Data Center objektet –> sprida tooChild objekt, roll = skrivskyddad | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).<br/><br/> Användbart för migrering, men inte fullständig replikering, redundans och återställning efter fel.|
|**Redundans och återställning efter fel** | Vi rekommenderar att du skapar en roll (AzureSiteRecoveryRole) med behörigheter för hello som krävs och tilldela sedan hello rollen tooa VMware användare eller grupp | Data Center objektet –> sprida tooChild objekt, roll = AzureSiteRecoveryRole<br/><br/> DataStore -> allokera utrymme, bläddra datalagret, låg nivå filåtgärder, ta bort filen och uppdatera filer för virtuella datorer<br/><br/> Nätverk -> nätverk tilldela<br/><br/> Resurs -> Tilldela VM tooresource pool, migrera är avstängt VM, migrera driven på den virtuella datorn<br/><br/> Aktiviteter -> Skapa uppgift, uppdatera uppgift<br/><br/> Konfiguration av virtuell dator -><br/><br/> Virtual machine -> interagera -> fråga enhetsanslutning, konfigurera CD-skivor, konfigurera diskettenheter media, stänga av, slå på strömmen, installera för VMware-verktyg<br/><br/> Virtual machine -> Lager -> Skapa, registrera, avregistrera<br/><br/> Etablering av virtuell dator -> -> Tillåt virtuella hämtning, tillåter Överför filer för virtuella datorer<br/><br/> Virtual machine -> ögonblicksbilder -> Ta bort ögonblicksbilder | Användaren tilldelas på nivån för datacenter och har åtkomst tooall hello objekt i hello datacenter.<br/><br/> toorestrict åtkomst, tilldela hello **ingen åtkomst** roll med hello **sprida toochild** objekt, toohello underordnade objekt (vSphere-värdar, datastores, virtuella datorer och nätverk).|

## <a name="create-an-account-tooconnect-toovmware-vcenter-server-vmware-vsphere-exsi-host"></a>Skapa ett konto tooconnect tooVMware vCenter-servern / EXSi VMware vSphere-värd
1. Logga in på hello Configuration server och starta hello cspsconfigtool.exe med hello genvägen placerad hello skrivbordet.
2. Klicka på **Lägg till konto** på hello **Hantera konto** fliken.

  ![Lägg till konto](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Ange hello kontoinformation och klicka på OK tooadd hello-konto. hello kontot ska ha hello som anges i hello [förbereda ett konto för automatisk upptäckt](#prepare-an-account-for-automatic-discovery) avsnitt.

  >[!NOTE]
  Det tar ungefär 15 minuter för hello konto information toobe synkroniserats upp med hello Site Recovery-tjänsten.


## <a name="associate-a-vmware-vcenter-vmware-vsphere-esx-host-add-vcenter"></a>Associera en VMware vCenter / vSphere VMware ESX-värd (Lägg till vCenter)
* På hello Azure-portalen, bläddra för*YourRecoveryServicesVault* > **Site Recovery-infrastruktur** > **Configuration servrarna**  >  *ConfigurationServer*
* Hello Configuration server information på sidan klickar du på hello + vCenter-knappen.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials-used-tooconnect-toohello-vcenter-server-vsphere-esxi-host"></a>Ändra autentiseringsuppgifterna tooconnect toohello vCenter-server / vSphere ESXi-värd

1. Logga in på hello Configuration server och starta hello cspsconfigtool.exe
2. Klicka på **Lägg till konto** på hello **Hantera konto** fliken.

  ![Lägg till konto](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Ange hello nya kontoinformation och klicka på OK tooadd hello-konto. hello kontot ska ha hello som anges i hello [förbereda ett konto för automatisk upptäckt](#prepare-an-account-for-automatic-discovery) avsnitt.
4. På hello Azure-portalen, bläddra för*YourRecoveryServicesVault* > **Site Recovery-infrastruktur** > **Configuration servrarna**  >  *ConfigurationServer*
5. Klicka på hello hello Configuration server information på sidan **uppdatera Server** knappen.
6. Välj hello vCenter Server tooopen hello vCenter sammanfattningssidan när hello uppdateringsjobb server har slutförts.
7. Välj hello nyligen tillagda konto i hello **vCenter server/vSphere värdkontot** fältet och klickar på hello **spara** knappen.

  ![Ändra konto](./media/site-recovery-vmware-to-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-in-azure-site-recovery"></a>Ta bort en vCenter i Azure Site Recovery
1. På hello Azure-portalen, bläddra för*YourRecoveryServicesVault* > **Site Recovery-infrastruktur** > **Configuration servrarna**  >  *ConfigurationServer*
2. Välj hello vCenter Server tooopen hello vCenter sammanfattningssidan hello Configuration server information på sidan.
3. Klicka på hello **ta bort** knappen toodelete hello vCenter

  ![ta bort konto](./media/site-recovery-vmware-to-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Om du behöver toomodify hello Vcenter IP-adress/FQDN, Port information du behöver toodelete hello vCenter-servern och lägga till den igen.
