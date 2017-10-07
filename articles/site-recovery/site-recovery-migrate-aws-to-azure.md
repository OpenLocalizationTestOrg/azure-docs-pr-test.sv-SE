---
title: "aaaMigrate virtuella datorer från AWS tooAzure | Microsoft Docs"
description: "Den här artikeln beskriver hur toomigrate virtuella datorer som körs i Amazon Web Services (AWS) tooAzure med hjälp av Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: bsiva
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: c99b781ec9cca5b8f9a847d3fc48408062b120b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-virtual-machines-in-amazon-web-services-aws-tooazure-with-azure-site-recovery"></a>Migrera virtuella datorer i Amazon Web Services (AWS) tooAzure med Azure Site Recovery

Den här artikeln beskriver hur toomigrate AWS Windows instanser tooAzure virtuella datorer med hello [Azure Site Recovery](site-recovery-overview.md) service.

Migrering är en växling från AWS tooAzure. Det går inte att återställning av datorer tooAWS och det finns ingen pågående replikering. Den här artikeln beskriver hello steg för migrering i hello Azure-portalen och baseras på hello instruktioner för [replikerar en fysisk dator tooAzure](site-recovery-vmware-to-azure.md).

Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

## <a name="supported-operating-systems"></a>Operativsystem som stöds

Site Recovery kan vara används toomigrate EC2 instanser som kör något av följande operativsystem hello:

- Windows (endast 64-bitars)
    - Windows Server 2008 R2 SP1 + (Citrix NV drivrutiner eller AWS NV-drivrutiner. **Kör RedHat NV drivrutiner instanser stöds inte**) Windows Server 2012 Windows Server 2012 R2
- Linux (endast 64-bitars)
    - Red Hat Enterprise Linux 6.7 (endast HVM virtualiserade instanser)

## <a name="prerequisites"></a>Krav

Här är vad du behöver för den här distributionen:

* **Konfigurationsservern**: en Amazon EC2 virtuell dator som kör Windows Server 2012 R2 distribueras som hello konfigurationsservern. Som standard installeras hello andra Azure Site Recovery-komponenter (processervern och huvudmålservern) när du distribuerar hello konfigurationsservern. Den här artikeln beskriver hello steg för migrering i hello Azure-portalen och baseras på hello instruktioner för [Läs mer](site-recovery-components.md)

* **EC2 instanser**: hello Amazon EC2 virtuella datorer instanser som du vill toomigrate.

## <a name="deployment-steps"></a>Distributionssteg

1. Skapa ett Recovery Services-valv.
2. hello säkerhetsgruppen för dina EC2 instanser ska ha regler har konfigurerat tooallow kommunikation mellan hello EC2-instans som du vill toomigrate och hello-instans som du planerar toodeploy hello konfigurationsservern.

3. Distribuera en ASR konfigurationsservern på hello samma Amazon virtuella privata moln som dina EC2-instanser. Se hello VMware / fysiskt tooAzure krav för konfigurationskrav för server-distributionen.

    ![DeployCS](./media/site-recovery-migrate-aws-to-azure/migration_pic2.png)

4.  När din konfigurationsservern distribueras i AWS och registreras Recovery Services-valvet, bör du se hello konfigurationsservern och processervern under Site Recovery-infrastruktur som visas nedan:

    ![CSinVault](./media/site-recovery-migrate-aws-to-azure/migration_pic3.png)

5. När du har distribuerat hello konfigurationsservern (det kan ta upp too15 minustes för den tooappear), verifiera att den kan kommunicera med hello virtuella datorer som du vill toomigrate.

6. [Konfigurera replikeringsinställningar](site-recovery-setup-replication-settings-vmware.md).

7. Aktivera replikering: Aktivera replikering för hello virtuella datorer du vill toomigrate. Du kan identifiera hello EC2 instanser som använder hello privata IP-adresser, som du kan hämta från hello EC2-konsolen.

    ![SelectVM](./media/site-recovery-migrate-aws-to-azure/migration_pic4.png)

8. När hello EC2 instanser som har skyddats och hello replikering tooAzure är klar, [köra ett Redundanstest](site-recovery-test-failover-to-azure.md) toovalidate programprestanda i Azure.

    ![TFI](./media/site-recovery-migrate-aws-to-azure/migration_pic5.png)

9. Kör en växling vid fel från AWS tooAzure för varje virtuell dator. Alternativt kan du skapa en återställningsplan kör en Redundansväxling toomigrate flera virtuella datorer från AWS tooAzure [Lär dig mer](site-recovery-create-recovery-plans.md) om återställningsplaner.

10. För migrering behöver du inte toocommit en växling vid fel. I stället du väljer alternativet för fullständig migrering av hello för varje dator som du vill toomigrate. hello slutföra migreringen åtgärden är klar in hello migreringsprocessen, tar du bort replikeringen av hello datorn och stoppar Site Recovery fakturering för hello datorn.

    ![Migrera](./media/site-recovery-migrate-aws-to-azure/migration_pic6.png)

## <a name="next-steps"></a>Nästa steg

- [Förbereda migrerade datorer tooenable replikering](site-recovery-azure-to-azure-after-migration.md) tooanother region för disaster recovery behov.
- Börja skydda dina arbetsbelastningar genom att [replikera virtuella Azure datorer.](site-recovery-azure-to-azure.md)
