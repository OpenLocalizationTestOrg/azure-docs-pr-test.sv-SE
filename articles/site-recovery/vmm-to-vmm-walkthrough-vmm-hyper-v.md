---
title: "aaaSet in VMM och Hyper-V för replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooset in System Center VMM-servrar och Hyper-V-värdar för replikering tooa sekundär VMM-plats."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d0389e3b-3737-496c-bda6-77152264dd98
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 677bf6d38328ccc425e3b0f056d03159a52da428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-vmm-and-hyper-v-for-hyper-v-vm-replication-tooa-secondary-site"></a>Steg 4: Konfigurera VMM och Hyper-V för Hyper-V VM replikering tooa sekundär plats 

När du har förberett för nätverk, ställa in System Center Virtual Machine Manager (VMM) servrar och Hyper-V-värdar för Hyper-V virtuell dator (VM) replikering tooa sekundär plats, med hjälp av [Azure Site Recovery](site-recovery-overview.md) i hello Azure-portalen. 

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="prepare-vmm-servers"></a>Förbereda VMM-servrar 

tooprepare för distribution:


1. Kontrollera att VMM-servrar uppfyller hello [supportkrav](site-recovery-support-matrix-to-sec-site.md#on-premises-servers), och [kraven för distribution av](vmm-to-vmm-walkthrough-prerequisites.md).
2. Kontrollera att VMM-servrarna är anslutna toohello internet och har åtkomst toothese URL: er.
    
    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.
    - Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.
    - Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).
3. Kontrollera att hello VMM-servern är [förbereda för nätverksmappning](vmm-to-vmm-walkthrough-network.md#prepare-for-network-mapping)


## <a name="prepare-hyper-v-hostsclusters"></a>Förbereda Hyper-V-värdar/kluster

1. Kontrollera att Hyper-V-värdar/kluster uppfyller hello [supportkrav](site-recovery-support-matrix-to-sec-site.md#on-premises-servers), och [kraven för distribution av](vmm-to-vmm-walkthrough-prerequisites.md).
2. Verifiera hello krav för [Hyper-V virtuella datorer](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)
3. Kontrollera [nätverk](site-recovery-support-matrix-to-sec-site.md#network-configuration) och [lagring](site-recovery-support-matrix-to-sec-site.md#storage) krav.
4. Kontrollera att Hyper-V-värdar är anslutna toohello internet och har åtkomst toothese URL: er.
    
    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Om du har IP-adressbaserade brandväggsregler, se till att de tillåter kommunikation tooAzure.
    - Tillåt hello [IP-intervall för Azure-Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653), och hello HTTPS (443)-port.
    - Tillåt IP-adressintervall för hello Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och Identity Management).

## <a name="prepare-for-single-server-deployment"></a>Förbereda för distribution på enskild server


Om du bara har en enda VMM-server kan du replikera virtuella datorer i Hyper-V-värdar i hello VMM-moln för[Azure](hyper-v-site-walkthrough-overview.md) eller tooa sekundära VMM-moln, som beskrivs i det här dokumentet. Vi rekommenderar hello första alternativet eftersom replikerar mellan moln inte sömlös.

Om du vill tooreplicate mellan moln, kan du replikera med en enda fristående VMM-server eller med en enda VMM-server som distribueras i ett sträckta Windows-kluster

### <a name="replicate-with-a-standalone-vmm-server"></a>Replikera med en fristående VMM-server

I det här scenariot distribuera hello enda VMM-servern som en virtuell dator i hello primär plats och replikera den virtuella dator tooa sekundära platsen med hjälp av Site Recovery och Hyper-V-replikering.

1. **Konfigurera VMM på en Hyper-V VM**. Vi rekommenderar att du samplacera hälsningspaket SQL Server-instans som används av VMM på hello samma virtuella dator. Detta sparar tid eftersom endast en virtuell dator har toobe skapas. Om du vill toouse fjärrinstansen av SQL Server och ett avbrott inträffar, måste toorecover instansen innan du kan återställa VMM.
2. **Kontrollera hello VMM-servern har minst två moln som konfigurerats**. Ett moln innehåller hello virtuella datorer du vill använda tooreplicate och hello andra moln fungerar som hello sekundär plats. Hej moln som innehåller hello virtuella datorer du vill följa tooprotect [krav](#prerequisites).
3. Ställa in Site Recovery som beskrivs i den här artikeln. Skapa och registrera hello VMM-servern i ett valv, ställa in en replikeringsprincip och aktivera replikering. hello käll- och VMM-namn kommer hello samma. Ange den inledande replikeringen sker över hello nätverk.
4. När du konfigurerar nätverksmappning kan du mappa hello Virtuellt datornätverk för hello primära molnet toohello Virtuellt datornätverk för hello sekundära molnet.
5. Aktivera Hyper-V-replikering på hello Hyper-V-värden som innehåller hello VMM VM i hello Hyper-V Manager-konsolen och aktivera replikering på hello VM. Kontrollera att du inte lägger till hello VMM virtuella tooclouds som skyddas av Site Recovery tooensure att Hyper-V-replikeringsinställningar inte åsidosättas av Site Recovery.
6. Om du skapar återställningsplaner för redundans som du använder hello samma VMM-server för källa och mål.
7. I en fullständig avbrott växla över och återställa på följande sätt:

   1. Kör en oplanerad redundans i hello Hyper-V Manager-konsolen hello sekundär plats, toofail över hello primära VMM VM toohello sekundär plats.
   2. Kontrollera att hello VMM VM är igång och körs och i hello valvet, kör en oplanerad växling toofail över hello virtuella datorer från primära toosecondary moln. Genomför hello redundans och välj en annan återställningspunkt om det behövs.
   3. När hello oplanerad växling är klar, kan alla resurser nås från hello primära platsen igen.
   4. Aktivera omvänd replikering för hello VMM VM när hello primära platsen är tillgänglig igen i hello Hyper-V Manager-konsolen på hello sekundär plats. Replikering för hello VM startas från sekundär tooprimary.
   5. Kör en planerad redundans i hello Hyper-V Manager-konsolen hello sekundär plats, toofail över hello VMM VM toohello primär plats. Bekräfta hello växling vid fel. Aktivera sedan omvänd replikering så att hello VMM VM igen replikering från primär toosecondary.
   6. Aktivera omvänd replikering för hello arbetsbelastning virtuella datorer, toostart replikering dem från sekundär tooprimary i hello Recovery Services-valvet.
   7. I hello Recovery Services-valvet, kör en planerad redundans toofail tillbaka hello arbetsbelastning VMs toohello primär plats. Genomför hello redundans toocomplete den. Aktivera omvänd replikering toostart replikering hello virtuella datorerna i arbetsbelastningen från primära toosecondary.

### <a name="replicate-with-a-stretched-vmm-cluster"></a>Replikera med ett ambitiöst VMM-kluster

I stället för att distribuera en fristående VMM-server som en virtuell dator som replikerar tooa sekundär plats, kan du VMM hög tillgänglighet genom att distribuera den som en virtuell dator i ett redundanskluster i Windows. Det ger arbetsbelastningsåterhämtning och skydd mot maskinvarufel. toodeploy med Site Recovery hello VMM VM ska distribueras i ett kluster för stretch på olika platser. toodo detta:

1. Installera VMM på en virtuell dator i ett redundanskluster i Windows och välj hello alternativet toorun hello server med hög tillgänglighet under installationen.
2. hello SQL Server-instans som används av VMM ska replikeras med SQL Server AlwaysOn-Tillgänglighetsgrupper, så att det finns en replik av hello databas i hello sekundär plats.
3. Följ hello instruktionerna i den här artikeln toocreate ett valv, registrera hello-servern och konfigurera skydd. Du måste tooregister varje VMM-servern i hello-klustret i hello Recovery Services-valvet. toodo kan du installera hello Provider på en aktiv nod och registrera hello VMM-servern. Du kan installera hello providern på andra noder.
4. När ett avbrott inträffar är hello VMM-servern och dess motsvarande SQL Server-databas redundansväxling och nås från hello sekundär plats.



## <a name="next-steps"></a>Nästa steg

Gå för[steg 5: konfigurera ett valv](vmm-to-vmm-walkthrough-create-vault.md).
