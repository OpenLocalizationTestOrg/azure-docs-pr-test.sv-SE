---
title: aaaBefore du starta replikering av virtuella datorer i Azure tooanother region | Microsoft Docs
description: "Sammanfattar hello stegen tootake innan du replikerar virtuella Azure-datorer mellan Azure-regioner, hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 7fa633075eeb52d0c184a1dd8d53ccc15644f518
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-before-you-start"></a>Steg 2: Innan du börjar

När du har granskat hello [arkitektur](azure-to-azure-walkthrough-architecture.md) för replikering av virtuella Azure-datorer (VM) mellan Azure-regioner med [Azure Site Recovery](site-recovery-overview.md), Använd den här artikeln tooverify krav. 

- När du är klar hello artikeln bör du ha en Rensa förståelse för vad behövs toomake hello distributionen fungerar och har utfört hello nödvändiga åtgärder.
- Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>
> Azure VM-replikering är för närvarande under förhandsgranskning.



## <a name="support-recommendations"></a>Rekommendationer

Granska hello tabellen nedan.

**Komponent** | **Krav**
--- | ---
**Recovery Services-valvet** | Vi rekommenderar att du skapar ett Recovery Services-valv i mål-hello Azure-region som du vill toouse för katastrofåterställning. Om du vill tooreplicate virtuella källdatorer i östra USA tooCentral USA kan du till exempel skapa hello valvet i centrala USA.
**Azure-prenumeration** | Din Azure-prenumeration ska vara aktiverade toocreate virtuella datorer i hello målplats som du vill toouse som hello disaster recovery region. Kontakta supporten tooenable hello krävs kvoten.
**Mål region kapacitet** | Hello prenumeration ska ha tillräckligt med kapacitet för virtuella datorer, lagringskonton och nätverkskomponenter i hello mål Azure-region.
**Storage** | Använd hello [lagringsanvisningar](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) för källan virtuella Azure-datorer tooavoid prestandaproblem.<br/><br/> Storage-konton måste vara i hello samma region som hello-valvet.<br/><br/> Du kan replikera toopremium konton i Central och södra Indien.<br/><br/> Om du distribuerar replikering med standardinställningar för hello skapar Site Recovery hello krävs lagringskonton baserat på konfigurationen av hello källa. Om du har ändrat inställningarna följer hello [skalbarhetsmål för Virtuella diskar](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Nätverk** | Du behöver tooallow utgående anslutning från virtuella Azure-datorer för specifika URL: er/IP-adressintervall.<br/><br/> Nätverkskonton måste finnas i hello samma region som hello-valvet. 
**Virtuell Azure-dator** | Kontrollera att alla hello senaste rotcertifikat finns i hello Windows-/ Linux virtuella Azure-datorn. Om de inte kan du inte kan tooregister hello VM i Site Recovery på grund av säkerhetsbegränsningar.
**Azure-konto** | Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en virtuell Azure-dator.

Hämta en fullständig lista över kraven för stöd i hello [supportmatrisen](site-recovery-support-matrix-azure-to-azure.md)


## <a name="set-permissions-on-hello-account"></a>Ange behörigheter för hello-konto

1. Läs mer om hello [behörigheter](site-recovery-role-based-linked-access-control.md) du behöver för replikering.
2. Följ dessa [instruktioner](../active-directory/role-based-access-control-configure.md#add-access) tooadd behörigheter.


## <a name="verify-target-resources"></a>Kontrollera target-resurser

1. Aktivera din Azure-prenumeration tooallow du toocreate virtuella datorer i hello rikta region som du vill använda toouse för katastrofåterställning som du vill toouse som hello disaster recovery region. Kontakta supporten tooenable hello krävs kvoten.
2. Kontrollera att din prenumeration har tillräckligt med resurser tooenable virtuella datorer med storlekar som matchar dina virtuella källdatorer. Som standard när ställer in replikering, Site Recovery plockningar hello samma storlek för hello mål VM eller hello närmaste möjliga storlek. Lär dig mer om [felsökning](site-recovery-azure-to-azure-troubleshoot-errors.md#azure-resource-quota-issues-error-code-150097) mål resurser.

## <a name="verify-azure-vm-certificates"></a>Verifiera Azure VM-certifikat

1. Kontrollera att alla hello senaste rotcertifikat finns på Windows hello eller Linux-datorer som du vill tooreplicate. Om hello senaste rotcertifikat inte finns, får inte hello VM vara registrerade tooSite återställning på grund av toosecurity begränsningar.
2. För virtuella Windows-datorer, hello följande:

    - Installera alla hello senaste Windows-uppdateringar på hello VM så att alla hello betrodda rotcertifikat på hello-datorn.
    - Följ hello Windows Update process-certifikatet uppdatering standardprocessen i din organisation, tooget hello senaste rotcertifikat och uppdaterade CRL på hello virtuella datorer i en frånkopplad miljö.
3. För Linux virtuella datorer, följer du hello riktlinjer som tillhandahålls av din Linux distributören tooget hello senaste betrodda rotcertifikat och hello senaste listan över återkallade certifikat på hello VM. Lär dig mer om [felsökning](site-recovery-azure-to-azure-troubleshoot-errors.md#trusted-root-certificates-error-code-151066) betrodda rot-problem.


## <a name="next-steps"></a>Nästa steg

Gå för[steg3: Planera nätverk](azure-to-azure-walkthrough-network.md) tooset upp utgående anslutningar.
