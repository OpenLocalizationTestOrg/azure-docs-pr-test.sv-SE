---
title: Migrera Azure IaaS-VM mellan Azure-regioner | Microsoft Docs
description: "Använd Azure Site Recovery för att migrera Azure IaaS-virtuella datorer från en Azure-region till en annan."
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
ms.openlocfilehash: ef2972c077a2b1dd2b2fd6ce53cc6560520ea870
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrera Azure IaaS-virtuella datorer mellan Azure-regioner med Azure Site Recovery
## <a name="overview"></a>Översikt
Välkommen till Azure Site Recovery! Använd den här artikeln om du vill migrera virtuella datorer i Azure mellan Azure-regioner. Observera att innan du börjar:

* Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: Azure Resource Manager och klassisk. Azure har också två portaler – den klassiska Azure-portalen som stöder den klassiska distributionsmodellen och Azure-portalen som stöder båda distributionsmodellerna. De grundläggande stegen för migrering är samma oavsett om du konfigurerar Site Recovery i Resource Manager eller i klassiskt. Men Användargränssnittet anvisningar och skärmdumpar i den här artikeln gäller för Azure-portalen.
* **För närvarande kan du bara migrera från en region till en annan. Du kan växla över virtuella datorer från en Azure-region till en annan, men du kan inte återställas dem igen.**

Skriv dina kommentarer eller frågor längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Krav
Här är vad du behöver för den här distributionen:

* **IaaS-virtuella datorer**: på virtuella datorer som du vill migrera. Du kan migrera dessa virtuella datorer genom att behandla dem som fysiska datorer.

## <a name="deployment-steps"></a>Distributionssteg
Det här avsnittet beskrivs stegen för distributionen i den nya Azure-portalen.

1. [Skapa ett valv](site-recovery-vmware-to-azure.md).
2. [Aktivera replikering](site-recovery-vmware-to-azure.md). Aktivera replikering för virtuella datorer du vill migrera och välja Azure som källa. 
3. [Kör en oplanerad redundansväxling](site-recovery-failover.md). När den inledande replikeringen är klar kan köra du en oplanerad redundans från en Azure-region till en annan. Alternativt kan du skapa en återställningsplan och kör en oplanerad redundans för att migrera flera virtuella datorer mellan regioner. [Lär dig mer](site-recovery-create-recovery-plans.md) om återställningsplaner.

## <a name="next-steps"></a>Nästa steg
Mer information om andra replikeringarna i [vad är Azure Site Recovery?](site-recovery-overview.md)
