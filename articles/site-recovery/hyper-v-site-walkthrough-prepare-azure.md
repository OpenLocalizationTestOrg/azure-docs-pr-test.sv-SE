---
title: "aaaPrepare Azure-resurser tooreplicate Hyper-V virtuella datorer (utan VMM för System Center) tooAzure med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Beskriver vad du behöver på plats i Azure innan du börjar replikera virtuella Hyper-V-datorer (utan VMM) tooAzure med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: f659e300c39253b0eaf7218bee9d39b11682edb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-tooazure"></a>Steg 5: Förbered Azure-resurser för tooAzure för Hyper-V-replikering

Använd hello instruktioner i den här artikeln tooprepare Azure resurser så att du kan replikera lokala virtuella Hyper-V-datorer (utan System Center VMM) med hello tooAzure [Azure Site Recovery](site-recovery-overview.md) service.

När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Innan du börjar

Kontrollera att du har läst hello [krav](hyper-v-site-walkthrough-prerequisites.md)

## <a name="set-up-an-azure-account"></a>Konfigurera ett Azure-konto

- Hämta en [Microsoft Azure-konto](http://azure.microsoft.com/).
- Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
- Kontrollera hello stöds regioner för Site Recovery Under geografisk tillgänglighet i [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/).
- Lär dig mer om [priserna för Site Recovery](site-recovery-faq.md#pricing), och få hello [prisinformationen](https://azure.microsoft.com/pricing/details/site-recovery/).


## <a name="set-up-an-azure-network"></a>Skapa ett Azure-nätverk

- Skapa ett Azure-nätverk. Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.
- hello nätverket bör finnas i hello samma region som hello Recovery Services-valvet
- Site Recovery hello Azure-portalen kan använda nätverk i [Resource Manager](../resource-manager-deployment-model.md), eller i klassiskt läge.
- Vi rekommenderar att du konfigurerar ett nätverk innan du börjar. Om du inte behöver du toodo under distributionen av Site Recovery.
- Lär dig mer om [priser för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

- Site Recovery replikerar tooAzure lagring för lokala datorer. Virtuella Azure-datorer skapas från hello lagring efter redundansväxlingen.
- Konfigurera standard/premium [Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold data replikeras tooAzure.
- [Premium-lagring](../storage/common/storage-premium-storage.md) används vanligtvis för virtuella datorer som behöver en konsekvent höga i/o-prestanda och låg latens toohost-i/o-intensiv arbetsbelastning.
- Om du vill toouse en premium konto toostore replikerade data måste du även en standardlagring konto toostore replikeringsloggar som avbilda pågående ändringar tooon lokala data.
- Beroende på hello resursmodell du vill att toouse för redundansväxlade virtuella Azure-datorer kan du konfigurera ett konto i [Resource Manager-läget](../storage/common/storage-create-storage-account.md), eller [klassiskt läge](../storage/common/storage-create-storage-account.md).
- Vi rekommenderar att du ställer in ett storage-konto innan du börjar. Om du inte behöver toodo under distributionen av Site Recovery. hello konton måste finnas i hello samma region som hello Recovery Services-valvet.
- Du kan inte flytta storage-konton används av Site Recovery över resursgrupper i hello samma prenumeration, eller mellan olika prenumerationer.


## <a name="next-steps"></a>Nästa steg

Gå för[steg 6: förbereda Hyper-V-resurser](hyper-v-site-walkthrough-prepare-hyper-v.md)
