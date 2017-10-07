---
title: "aaaSet hello källa och mål för Hyper-V-replikering tooAzure (utan System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg tooset inställningar för källa och mål för replikering av Hyper-V VMs tooAzure lagring med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 105b90e6ac053d5b842c54a36c460a26d0f5c2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-replication-tooazure"></a>Steg 8: Ställ in hello källa och mål för tooAzure för Hyper-V-replikering

Den här artikeln beskriver hur tooconfigure käll- och inställningar när du replikerar lokalt Hyper-V virtuella datorer (utan VMM för System Center) tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Ställ in hello Hyper-V-platsen, installera hello Azure Site Recovery-providern och hello Azure Recovery Services-agenten på Hyper-V-värdar och registrera hello-webbplats i hello-valvet.

1. I **Förbered infrastrukturen**, klickar du på **källa**. tooadd en ny Hyper-V-plats som en behållare för dina Hyper-V-värdar eller kluster, klicka på **+ Hyper-V-platsen**.

    ![Konfigurera källan](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. I **skapa Hyper-V-platsen**, ange ett namn för hello-platsen. Klicka sedan på **OK**. Nu väljer hello plats du har skapat och klicka på **+ Hyper-V Server** tooadd en server toohello plats.

    ![Konfigurera källan](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. I **Lägg till Server** > **servertyp**, kontrollera att **Hyper-V server** visas.

    - Kontrollera att hello Hyper-V-server som du vill tooadd uppfyller hello [krav](#on-premises-prerequisites), och är kan tooaccess hello angivna URL: er.
    - Hämta installationsfilen för hello Azure Site Recovery-providern. Du kör den här filen tooinstall hello providern och hello Recovery Services-agenten på varje Hyper-V-värd.

    ![Konfigurera källan](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-hello-provider-and-agent"></a>Installera hello Provider och agent

1. Kör installationsfilen för hello-providern på varje värd du lagt till toohello Hyper-V-platsen. Om du installerar på en Hyper-V-kluster, kan du köra installationsprogrammet på varje nod i klustret. Installera och registrera varje klusternod för Hyper-V säkerställer att virtuella datorer är skyddade, även om de migrera mellan noder.
2. I **Microsoft Update** kan du välja uppdateringar så att provideruppdateringarna installeras i enlighet med din Microsoft Update-princip.
3. I **Installation**, acceptera eller ändra hello standardinstallationsplatsen för providern och klickar på **installera**.
4. I **Valvinställningar**, klickar du på **Bläddra** tooselect hello valvnyckelfilen som du hämtat. Ange hello Azure Site Recovery-prenumerationen, hello valvnamnet och hello Hyper-V platsservern toowhich hello Hyper-V tillhör.

    ![Serverregistrering](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. I **proxyinställningar**, ange hur providern som körs på Hyper-V-värdar som ansluter tooAzure Site Recovery via hello hello internet.

    * Om du vill att hello providern tooconnect direkt väljer **ansluter direkt tooAzure Site Recovery utan en proxy**.
    * Om den befintliga proxyservern kräver autentisering eller om du vill använda toouse en anpassad proxyserver för hello leverantörsanslutning väljer **ansluta tooAzure Platsåterställningen genom att använda en proxyserver**.
    * Om du använder en proxyserver:
        - Ange hello-adress, port och autentiseringsuppgifter
        - Kontrollera att hello webbadresser som beskrivs i hello [krav](#prerequisites) tillåts via hello proxy.

    ![Internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. När installationen är klar klickar du på **registrera** tooregister hello server i hello-valvet.

    ![Installationsplats](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. När registreringen är klar, hämtas metadata från hello Hyper-V-servern av Azure Site Recovery och hello server visas i **Site Recovery-infrastruktur** > **Hyper-V-värdar**.


## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön

Ange hello Azure storage-konto för replikering och ansluter hello Azure-nätverk toowhich virtuella Azure-datorer efter redundans.

1. Klicka på **Förbered infrastruktur** > **mål**.
2. Välj hello prenumeration och hello resursgrupp som du vill toocreate hello virtuella Azure-datorer efter redundans. Välj hello distribution modell som du vill toouse i Azure (klassiska eller resource management) för hello virtuella datorer.

3. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

    - Om du inte har ett lagringskonto, klickar du på **+ lagring** toocreate en Resource Manager-baserade konto infogad. 
    - Om du inte har ett Azure-nätverk, klickar du på **+ nätverk** toocreate en infogad Resource Manager-baserat nätverk.






## <a name="next-steps"></a>Nästa steg

Gå för[steg 9: Konfigurera en replikeringsprincip](hyper-v-site-walkthrough-replication.md)
