---
title: "aaaSet hello källa och mål för fysisk server replication tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg tooset inställningar för källa och mål för replikering av fysiska servrar tooAzure lagring med hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e75ef23174d77509bf54380eba8f251a7337a45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-hello-source-and-target-for-physical-server-replication-tooazure"></a>Steg 7: Konfigurera hello källa och mål för fysisk server replication tooAzure

Den här artikeln beskriver hur tooconfigure käll- och inställningar vid replikering av lokala fysiska servrar tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Ställ in hello konfigurationsservern, registrera det i hello valvet och identifiera datorer.

1. Klicka på **Site Recovery** > **steg 1: förbereda infrastrukturen** > **källa**.
2. Om du inte har en konfigurationsserver, klickar du på **+ konfigurationsservern**.
3. I **Lägg till Server**, kontrollera att **konfigurationsservern** visas i **servertyp**.
4. Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.
5. Hämta hello valvregistreringsnyckel. Du behöver detta när du kör installationsprogrammet för enhetlig. hello nyckeln är giltig i fem dagar efter genereringen.

   ![Konfigurera källan](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a>Registrera hello konfigurationsservern i hello-valv

Hello följande innan du startar och kör installationsprogrammet för enhetlig tooinstall hello konfigurationsservern hello processervern och hello huvudmålservern. hello videon Beskriver inställning av hello komponenter för VMware VM tooAzure replikering. Dock är hello samma process giltig för fysisk server tooAzure replikering.

- Kontrollera att hello systemklockan är synkroniserad med i hello konfigurationsservern VM och en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Den måste matcha. Installationen misslyckas om det är 15 minuter framför eller bakom.
- Kör installationsprogrammet som en lokal administratör på hello configuration server-datorn.
- Kontrollera att TLS 1.0 är aktiverat på hello VM.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurationsservern kan också installeras [från kommandoraden hello](http://aka.ms/installconfigsrv).




## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön

Innan du konfigurerar hello målmiljön, kontrollera att du har ett Azure storage-konto och virtuella nätverk som har upprättats.

1. Klicka på **Förbered infrastruktur** > **mål**, och välj hello Azure-prenumeration du vill toouse.
2. Ange om ditt mål distributionsmodell är Resource Manager-baserade eller klassiska.
3. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

   ![mål](./media/physical-walkthrough-source-target/gs-target.png)

4. Om du inte har skapat ett lagringskonto eller nätverket, klickar du på **+ lagringskonto** eller **+ nätverk**, toocreate en Resource Manager-konto eller nätverket infogad.

## <a name="next-steps"></a>Nästa steg

Gå för[steg 8: skapa en replikeringsprincip](physical-walkthrough-replication.md)
