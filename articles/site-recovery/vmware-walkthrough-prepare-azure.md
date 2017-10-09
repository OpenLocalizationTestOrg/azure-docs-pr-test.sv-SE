---
title: "aaaPrepare Azure-resurser tooreplicate lokala virtuella VMware-datorer tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Beskriver vad du behöver på plats i Azure innan du börjar replikera lokala virtuella VMware-datorer tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ac72fff0593783add789408ecfeb1812d70108b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-vmware-replication-tooazure"></a>Steg 5: Förbered Azure-resurser för tooAzure för VMWare-replikering


Använd hello instruktioner i den här artikeln tooprepare Azure resurser så att du kan replikera lokala datorer tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) service.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Innan du börjar

Kontrollera att du har läst hello [krav](vmware-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Konfigurera ett Azure-konto

- Hämta en [Microsoft Azure-konto](http://azure.microsoft.com/).
- Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
- Kontrollera hello stöds regioner för Site Recovery Under geografisk tillgänglighet i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).
- Lär dig mer om [priserna för Site Recovery](site-recovery-faq.md#pricing), och få hello [prisinformationen](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Skapa ett Azure-nätverk

- Skapa ett Azure-nätverk. Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.
- Site Recovery hello Azure-portalen kan använda nätverk i [Resource Manager](../resource-manager-deployment-model.md), eller i klassiskt läge.
- hello nätverket bör finnas i hello samma region som hello Recovery Services-valvet
- Lär dig mer om [priser för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network/).
- Lär dig mer om [Virtuella Azure-anslutningen](site-recovery-network-design.md) efter växling vid fel.


## <a name="set-up-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

- Site Recovery replikerar tooAzure lagring för lokala datorer. Virtuella Azure-datorer skapas från hello lagring efter redundansväxlingen.
- Konfigurera en [Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) för replikerade data.
- Site Recovery hello Azure-portalen kan använda storage-konton som skapas i Resource Manager eller i klassiskt läge.
- Hej lagringskontot kan vara standard eller [premium](../storage/common/storage-premium-storage.md).
- Om du konfigurerar ett premium-konto behöver du även en ytterligare standardkonto för loggdata.


## <a name="next-steps"></a>Nästa steg

Gå för[steg 6: förbereda VMware-resurser](vmware-walkthrough-prepare-vmware.md)
