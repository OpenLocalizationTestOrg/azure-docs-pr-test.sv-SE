---
title: "aaaReview hello krav för Hyper-V tooAzure replikering (med System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hello förutsättningar för att konfigurera replikering, redundans och återställning av lokala Hyper-V virtuella datorer i VMM-moln tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a1c30fd5-c979-473c-af44-4f725ad3e3ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: raynew
ms.openlocfilehash: de13a2d80b1a9a5d968a180d559f661ab11e70c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-hyper-v-with-vmm-tooazure-replication"></a>Steg 2: Granska hello krav för Hyper-V (med VMM) tooAzure-replikering

När du har granskat hello [scenariots arkitektur](vmm-to-azure-walkthrough-architecture.md), läsa den här artikeln toomake bör du förstå kraven för distribution av hello. 

## <a name="prerequisites-and-limitations"></a>Krav och begränsningar

**Krav** | **Detaljer**
--- | ---
**Azure-konto** | Du behöver ett [Microsoft Azure-konto](http://azure.microsoft.com/).
**Azure Storage** | Du behöver ett Azure storage-konto toostore replikerade data.<br/><br/> hello storage-konto måste vara i hello samma region som hello Azure Recovery Services-valvet.<br/><br/>Du kan använda [geo-redundant lagring](../storage/common/storage-redundancy.md#geo-redundant-storage) eller lokalt redundant lagring. Vi rekommenderar geo-redundant lagring. Data är flexibla med geo-redundant lagring, om ett regionalt strömavbrott, eller om hello primära region inte kan återställas.<br/><br/> Du kan använda ett Azure-standardlagringskonto eller använda Azure [Premium Storage](../storage/common/storage-premium-storage.md). Premium Storage stöder I/O-intensiva arbetsbelastningar och används vanligtvis för virtuella datorer som behöver konstant höga I/O-prestanda och låg latens. Om du använder premiumlagring för replikerade data behöver du även ett standardlagringskonto. Ett standardlagringskonto lagrar replikeringsloggar som samlar in löpande ändringar tooon lokala data.
**Azure-nätverk** | Du behöver ett [Azure-nätverk](../virtual-network/virtual-network-get-started-vnet-subnet.md), toowhich virtuella Azure-datorer ansluta efter växling vid fel. hello Azure nätverket måste finnas i hello samma region som hello Recovery Services-valvet.
**Lokala VMM-servrar** | Du behöver en eller flera VMM-servrar som kör System Center 2012 R2 eller senare.<br/><br/> Varje VMM-server måste ha ett eller flera privata moln. Varje moln behöver en eller flera värdgrupper.<br/><br/> hello VMM-servern behöver Internetåtkomst.
**Lokal Hyper-V** | Hyper-V-värdservrar måste köra minst Windows Server 2012 R2 med hello Hyper-V-rollen aktiverad eller Microsoft Hyper-V Server 2012 R2. hello senaste uppdateringar måste installeras.<br/><br/> hello Hyper-V-värden måste finnas i en värdgrupp i VMM (finns i ett VMM-moln).<br/><br/> En värd måste ha en eller flera virtuella datorer som du vill tooreplicated.<br/><br/> Hyper-V-värdar måste vara anslutna toohello internet för replikering tooAzure, direkt eller med en proxy. Hyper-V-servrar måste ha hello korrigeringar som beskrivs i artikel [2961977](https://support.microsoft.com/kb/2961977).
**Lokala virtuella Hyper-V-datorer** | Virtuella datorer du vill ska köra tooreplicate en [operativsystem som stöds](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions), och överensstämma med [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). hello namn kan ändras när replikering har aktiverats. 




## <a name="next-steps"></a>Nästa steg

Gå för[steg3: Planera kapacitet](vmm-to-azure-walkthrough-capacity.md)
