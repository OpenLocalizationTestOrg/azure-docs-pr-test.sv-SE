---
title: "aaaReview hello krav för Hyper-V-replikering tooa sekundär VMM-plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hello krav för att replikera virtuella Hyper-V-datorer tooa sekundär VMM-plats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 21ff0545-8be5-4495-9804-78ab6e24add6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 1bd945fdda36c3cce5d159209abbd3c98a7e3682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-and-limitations-for-hyper-v-vm-replication-tooa-secondary-vmm-site"></a>Steg 2: Granska hello förutsättningar och begränsningar för Virtuella Hyper-V-replikering tooa sekundär VMM-plats


När du har granskat hello [scenariots arkitektur](vmm-to-vmm-walkthrough-architecture.md), läsa den här artikeln toomake du bör förstå kraven för distribution av hello när replikera lokala Hyper-V virtuella datorer (VM) hanteras i System Center Virtual Machine Manager (VMM) moln, tooa sekundära platsen med [Azure Site Recovery](site-recovery-overview.md) i hello Azure-portalen.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites-and-limitations"></a>Krav och begränsningar

**Krav** | **Detaljer**
--- | ---
**Azure** | En [Microsoft Azure](http://azure.microsoft.com/) prenumeration.<br/><br/> Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> [Lär dig mer](https://azure.microsoft.com/pricing/details/site-recovery/) om priserna för Site Recovery.<br/><br/> Kontrollera hello stöds regioner för Site Recovery Under geografisk tillgänglighet i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).
**VMM-servrar** | Vi rekommenderar att du har två VMM-servrar, en hello primär plats och en i hello sekundär.<br/><br/> Replikera mellan moln på en enda VMM-servern stöds.<br/><br/> VMM-servrar måste köra minst System Center 2012 SP1 med hello senaste uppdateringarna.<br/><br/> VMM-servrar måste internet-åtkomst.
**VMM-moln** | Varje VMM-servern måste ha ett eller flera moln och alla moln måste ha hello Hyper-V kapacitetsprofilen anges. <br/><br/>Molnen måste innehålla en eller flera VMM-värdgrupper.<br/><br/> Om du bara har en VMM-servern måste ha minst två moln, tooact som primär och sekundär.
**Hyper-V** | Hyper-V-servrar måste köra minst Windows Server 2012 med hello Hyper-V-rollen och har hello senaste uppdateringarna installerade.<br/><br/> En Hyper-V-server måste innehålla en eller flera virtuella datorer.<br/><br/>  Hyper-V-värdservrar ska finnas i värdgrupper i hello primära och sekundära VMM-moln.<br/><br/> Om du kör Hyper-V i ett kluster på Windows Server 2012 R2, installera [uppdatera 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Om du kör Hyper-V i ett kluster på Windows Server 2012 skapas inte klusterutjämning automatiskt om du har en statisk IP-adress-kluster. Konfigurera hello klusterutjämning manuellt. [Läs mer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Hyper-V-servrar måste internet-åtkomst.




## <a name="next-steps"></a>Nästa steg

Gå för[steg3: Planera nätverk](vmm-to-vmm-walkthrough-network.md).
