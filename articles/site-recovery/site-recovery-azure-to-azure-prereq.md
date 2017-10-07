---
title: "aaaPrerequisites för replikering tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln sammanfattar krav för att replikera virtuella datorer och fysiska datorer tooAzure med hjälp av hello Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: rajanaki
ms.openlocfilehash: c66cea8b097a872bae57e7b42e659e58c4b0b1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="prerequisites-for-replicating-azure-virtual-machines-tooanother-region-by-using-azure-site-recovery"></a>Krav för att replikera virtuella datorer i Azure tooanother region med hjälp av Azure Site Recovery

> [!div class="op_single_selector"]
> * [Replikera från Azure tooAzure](site-recovery-azure-to-azure-prereq.md)
> * [Replikera från lokala tooAzure](site-recovery-prereq.md)

hello Azure Site Recovery-tjänsten bidrar tooyour affärskontinuitet och haveriberedskap (BCDR) genom att samordna:
* Replikering av virtuella datorer i Azure tooanother Azure-region.
* Replikeringen av lokala fysiska servrar och virtuella datorer tooAzure eller tooa sekundärt datacenter. 

När avbrott inträffar på den primära platsen, kan du växla över tooa sekundär plats tookeep appar och arbetsbelastningar som är tillgängliga. När den returnerar toonormal åtgärder kan du växla tillbaka tooyour primära platsen. Mer information om Platsåterställning finns i [vad är Site Recovery?](site-recovery-overview.md).

Den här artikeln sammanfattar hello krav krävs toobegin Site Recovery replikering från lokala tooAzure.

Bokför eventuella kommentarer längst ned hello hello artikel eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="azure-requirements"></a>Krav för Azure

**Krav** | **Detaljer**
--- | ---
**Azure-konto** | En [Microsoft Azure](http://azure.microsoft.com/) konto.<br/><br/> Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
**Site Recovery-tjänsten** | Mer information om priserna för Site Recovery finns [priserna för Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). Vi rekommenderar att du skapar ett Recovery Services-valv i mål-hello Azure-region som du vill toouse som en återställningsplats för katastrofåterställning. Till exempel om du vill tooreplicate tooCentral oss dina virtuella källdatorer körs i östra USA, rekommenderar vi att du skapar hello valvet i centrala USA.|
**Azure kapacitet** | För hello mål Azure-region som du vill toouse som din disaster recovery-plats, behöver du toohave en prenumeration med tillräckligt med kapacitet för virtuella datorer, lagringskonton och nätverkskomponenter. Du kan kontakta support tooadd högre kapacitet.
**Vägledning för lagring** | Se till att du följer hello [lagringsanvisningar](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) för käll-Azure virtuella datorer tooavoid eventuella prestandaproblem. Om du följer hello standardinställningarna skapar Site Recovery hello krävs lagringskonton baserat på konfigurationen av hello källa. Om du anpassar och välja egna inställningar, se till att du följer hello [skalbarhetsmål för virtuella diskar](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Vägledning för nätverk** | Du behöver toowhitelist hello utgående anslutning från Azure-VM för specifika URL-adresser eller IP-adressintervall. Mer information finns i toohello [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md) artikel.
**Virtuell Azure-dator** | Se till att alla hello senaste rotcertifikat finns på Hej Windows eller Linux-VM. Om hello senaste rotcertifikat inte finns, får inte hello VM vara registrerade tooSite återställning på grund av toosecurity begränsningar.

>[!NOTE]
>Mer information om stöd för specifika konfigurationer finns hello [supportmatrisen](site-recovery-support-matrix-azure-to-azure.md).

## <a name="next-steps"></a>Nästa steg
- Lär dig mer om [nätverk vägledning för att replikera virtuella datorer i Azure](site-recovery-azure-to-azure-networking-guidance.md).
- Börja skydda dina arbetsbelastningar av [replikering av Azure virtuella datorer](site-recovery-azure-to-azure.md).
