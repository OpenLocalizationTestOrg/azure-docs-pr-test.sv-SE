---
title: aaaMigrate Azure IaaS-VM mellan Azure-regioner | Microsoft Docs
description: "Använd Azure Site Recovery toomigrate Azure IaaS-virtuella datorer från en Azure-region tooanother."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: c84dc77716b8d19969eab60707ed1332ca39b893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrera Azure IaaS-virtuella datorer mellan Azure-regioner med Azure Site Recovery
## <a name="overview"></a>Översikt
Välkommen tooAzure Site Recovery! Använd den här artikeln om du vill toomigrate Azure virtuella datorer mellan Azure-regioner. Observera att innan du börjar:

* Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: Azure Resource Manager och klassisk. Azure har också två portaler – hello klassiska Azure-portalen som stöder hello klassiska distributionsmodellen och hello Azure-portalen med stöd för båda distributionsmodellerna. hello grundläggande anvisningar hello för migrering är samma om du konfigurerar Site Recovery i Resource Manager eller i klassiskt. Men hello UI anvisningar och skärmdumpar i den här artikeln är relevanta för hello Azure-portalen.
* **Du kan för närvarande endast migrera från en region tooanother. Du kan växla över virtuella datorer från en Azure-region tooanother, men du kan inte återställas dem igen.**

Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Krav
Här är vad du behöver för den här distributionen:

* **IaaS-virtuella datorer**: hello virtuella datorer du vill toomigrate. Du kan migrera dessa virtuella datorer genom att behandla dem som fysiska datorer.

## <a name="deployment-steps"></a>Distributionssteg
Det här avsnittet beskrivs hello distributionsstegen i hello nya Azure-portalen.

1. [Skapa ett valv](site-recovery-vmware-to-azure.md).
2. [Aktivera replikering](site-recovery-vmware-to-azure.md). Aktivera replikering för hello VMs toomigrate, och välj Azure som källa. 
3. [Kör en oplanerad redundansväxling](site-recovery-failover.md). När den inledande replikeringen är klar kan köra du en oplanerad redundans från en Azure-region tooanother. Om du vill kan du skapa en återställningsplan och kör en oplanerad redundans toomigrate flera virtuella datorer mellan regioner. [Lär dig mer](site-recovery-create-recovery-plans.md) om återställningsplaner.

## <a name="next-steps"></a>Nästa steg
Mer information om andra replikeringarna i [vad är Azure Site Recovery?](site-recovery-overview.md)
