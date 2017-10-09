---
title: aaaMigrate tooAzure med Site Recovery | Microsoft Docs
description: "Den här artikeln innehåller en översikt över migrera virtuella datorer och fysiska servrar tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/05/2017
ms.author: raynew
ms.openlocfilehash: 6e65deee337c5371d441812ddb820dc8bc233684
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-tooazure-with-site-recovery"></a>Migrera tooAzure med Site Recovery

Den här artikeln för en översikt av hello Azure Site Recovery-tjänsten för migrering av virtuella datorer och fysiska servrar.

Site Recovery är en Azure-tjänst som stödjer tooyour BCDR-strategi genom att samordna replikeringen av lokala fysiska servrar och virtuella datorer toohello molnet (Azure) eller tooa sekundärt datacenter. Vid driftstopp på den primära platsen växlar du över toohello sekundär plats tookeep appar och arbetsbelastningar som är tillgängliga. Du växlar tillbaka tooyour primära platsen när den returnerar toonormal åtgärder. Läs mer i [Vad är Site Recovery?](site-recovery-overview.md) Du kan också använda Site Recovery toomigrate din befintliga lokala arbetsbelastningar tooAzure tooexpedite ditt moln körnings- och tillg hello uppsättning funktioner som Azure erbjuder.

För en snabb överblick över hur tooperform migrering, se toothis video.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]

Den här artikeln beskrivs distribution i hello [Azure-portalen](https://portal.azure.com). Hej [klassiska Azure-portalen](https://manage.windowsazure.com/) kan vara används toomaintain befintliga Site Recovery-valv men du kan inte skapa nya valv.

Skicka kommentarer längst ned hello i den här artikeln. Tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="what-do-we-mean-by-migration"></a>Vad menar vi med migrering?

Du kan distribuera Site Recovery för replikering av lokala virtuella datorer och fysiska servrar, tooAzure eller tooa sekundär plats. Du kan replikera datorer, redundansväxla dem från hello primär plats när avbrott inträffar och inte dem tillbaka toohello primär plats när den återställs. I tillägg toothis kan du använda Site Recovery toomigrate virtuella datorer och fysiska servrar tooAzure så att användare kan komma åt dem som virtuella Azure-datorer. Migrering innebär att replikering och redundans från hello primär plats tooAzure och en fullständig migrering gest.

## <a name="what-can-site-recovery-migrate"></a>Vad kan Site Recovery migrera?

Du kan:

- Migrera arbetsbelastningar som körs på lokala datorer med Hyper-V, virtuella VMware-datorer och fysiska servrar toorun på Azure Virtual Machines. Du kan också göra fullständig replikering och återställning i det här scenariot.
- Migrera [virtuella Azure IaaS-datorer](site-recovery-migrate-azure-to-azure.md) mellan Azure-regioner. För närvarande stöds endast migrering i detta scenario, vilket innebär att återställning efter fel inte stöds.
- Migrera [instanser i AWS Windows](site-recovery-migrate-aws-to-azure.md) tooAzure IaaS-VM. För närvarande stöds endast migrering i detta scenario, vilket innebär att återställning efter fel inte stöds.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Migrera lokala virtuella datorer och fysiska servrar

toomigrate lokala Hyper-V virtuella datorer, virtuella VMware-datorer och fysiska servrar kan du följa nästan hello samma steg som används för vanlig replikering.

1. Skapa ett Recovery Services-valv
2. Konfigurera hello krävs hanteringsservrar (VMware VMM Hyper-V - beroende på vad du vill toomigrate), lägga till dem toohello valvet och ange inställningar för replikering.
3. Aktivera replikering för hello datorer du vill toomigrate
4. Kör en snabb testa redundans tooensure att allt fungerar som den ska efter hello inledande migreringen.
5. När du har kontrollerat att din replikeringsmiljö fungerar, använder du en planerad eller oplanerad redundans beroende på [vad som stöds](site-recovery-failover.md) för scenariot. Vi rekommenderar att du använder en planerad redundans när så är möjligt.
6. För migrering kan du inte behöver toocommit en växling vid fel, eller ta bort den. I stället kan du välja hello **slutföra migreringen** alternativet för varje dator som du vill toomigrate.
     - I **replikerade objekt**och högerklicka på hello VM, klicka på **slutföra migreringen**. Klicka på **OK** toocomplete. Du kan följa förloppet i hello VM-egenskaper i genom att övervaka hello fullständig migreringsjobb i **Site Recovery-jobb**.
     - Hej **slutföra migreringen** åtgärden är klar in hello migreringsprocessen, tar bort replikeringen av hello datorn och stoppar Site Recovery-faktureringen för hello datorn.

![fullständig migrering](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## <a name="migrate-between-azure-regions"></a>Migrera mellan Azure-regioner

Du kan migrera virtuella Azure-datorer mellan regioner med hjälp av Site Recovery. I det här scenariot stöds bara migrering. Med andra ord kan du replikera hello Azure virtuella datorer och redundansväxla tooanother region, men du kan inte återställas. Distribuera en lokal configuration server toomanage replikering i det här scenariot som du ställer in ett Recovery Services-valv, lägga till den toohello valvet och ange replikeringsinställningarna. Du kan aktivera replikering för hello datorer du vill toomigrate och köra en snabb testa redundans. Sedan du kör en oplanerad redundans med hello **slutföra migreringen** alternativet.

## <a name="migrate-aws-tooazure"></a>Migrera AWS tooAzure

Du kan migrera AWS instanser tooAzure virtuella datorer. I det här scenariot stöds bara migrering. Med andra ord kan du replikera hello AWS instanser och redundansväxla tooAzure, men du kan inte återställas. AWS instanser hanteras i hello samma sätt som fysiska servrar för migrering. Du ställa in ett Recovery Services-valv, distribuera en lokal configuration server toomanage replikering, lägga till den toohello valvet och ange inställningar för replikering. Du kan aktivera replikering för hello datorer du vill toomigrate och köra en snabb testa redundans. Sedan du kör en oplanerad redundans med hello **slutföra migreringen** alternativet.




## <a name="next-steps"></a>Nästa steg

- [Migrera tooAzure för virtuella VMware-datorer](site-recovery-vmware-to-azure.md)
- [Migrera Hyper-V virtuella datorer i VMM-moln tooAzure](site-recovery-vmm-to-azure.md)
- [Migrera Hyper-V virtuella datorer utan VMM tooAzure](site-recovery-hyper-v-site-to-azure.md)
- [Migrera virtuella Azure-datorer mellan Azure-regioner](site-recovery-migrate-azure-to-azure.md)
- [Migrera AWS instanser tooAzure](site-recovery-migrate-aws-to-azure.md)
- [Förbereda migrerade datorer tooenable replikering](site-recovery-azure-to-azure-after-migration.md) tooanother region för disaster recovery behov.
- Börja skydda dina arbetsbelastningar genom att [replikera virtuella Azure datorer.](site-recovery-azure-to-azure.md)
