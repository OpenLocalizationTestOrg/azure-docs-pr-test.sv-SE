---
title: "aaaSet in hello källan och målet för VMware replikering tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg tooset inställningar för källa och mål för replikering av virtuella VMware-datorer tooAzure lagring med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ef33a44bc5da17afb0442be63f576925f5b9a8b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-vmware-replication-tooazure"></a>Steg 8: Ställ in hello källan och målet för VMware replikering tooAzure

Den här artikeln beskriver hur tooconfigure käll- och inställningar när du replikerar lokalt VMware virtuella datorer tooAzure med hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Ställ in hello konfigurationsservern, registrera det i hello valvet och identifiera virtuella datorer.

1. Klicka på **Site Recovery** > **steg 1: förbereda infrastrukturen** > **källa**.
2. Om du inte har en konfigurationsserver, klickar du på **+ konfigurationsservern**.
3. I **Lägg till Server**, kontrollera att **konfigurationsservern** visas i **servertyp**.
4. Hämta installationsfilen för hello Unified installationsprogram för Site Recovery.
5. Hämta hello valvregistreringsnyckel. Du behöver detta när du kör installationsprogrammet för enhetlig. hello nyckeln är giltig i fem dagar efter genereringen.

   ![Konfigurera källan](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a>Registrera hello konfigurationsservern i hello-valv

Hello följande innan du startar och kör installationsprogrammet för enhetlig tooinstall hello konfigurationsservern hello processervern och hello huvudmålservern.
    - Få en snabb överblick av video

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Kontrollera att hello systemklockan är synkroniserad med i hello konfigurationsservern VM och en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Den måste matcha. Installationen misslyckas om det är 15 minuter framför eller bakom.
    - Kör installationsprogrammet som en lokal administratör på hello konfigurationsservern VM.
    - Kontrollera att TLS 1.0 är aktiverat på hello VM.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurationsservern kan också installeras [från kommandoraden hello](http://aka.ms/installconfigsrv).



## <a name="connect-toovmware-servers"></a>Anslut tooVMware servrar

tooallow Azure Site Recovery toodiscover virtuella datorer som körs i din lokala miljö, behöver du tooconnect din VMware vCenter-servern eller vSphere ESXi-värdar med Site Recovery. Observera följande hello innan du börjar:

- Om du lägger till hello vCenter-servern eller vSphere värdar tooSite återställning med ett konto utan administratörsbehörighet på servern hello måste hello kontot dessa behörigheter aktiveras:
    - Datacenter, datalagret, mapp, värd, nätverk, resurs, virtuella datorn vSphere distribuerade växel.
    - Hej vCenter-servern måste lagring vyer behörigheter.
- När du lägger till VMware-servrar tooSite återställning kan det ta 15 minuter eller längre för dessa tooappear hello-portalen.

### <a name="add-hello-account-for-automatic-discovery"></a>Lägg till hello konto för automatisk identifiering

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a>Upprätta en anslutning

Anslut tooservers på följande sätt:

1. Välj **+ vCenter** toostart ansluter en VMware vCenter-server eller en VMware vSphere ESXi-värd.
2. I **lägga till vCenter**, ange ett eget namn för hello vSphere-värd eller vCenter server och sedan ange hello IP-adress eller FQDN för hello-servern.
3. Lämna hello port som 443 om VMware-servrar är konfigurerade toolisten för begäranden på en annan port. Välj hello-konto som är tooconnect toohello VMware vCenter eller vSphere ESXi-servern. Klicka på **OK**.
4. Site Recovery ansluter tooVMware servrar med hjälp av hello angett inställningar och identifierar virtuella datorer.

> [!NOTE]
> Om du vill lägga till en server eller en värd med ett konto som inte har administratörsbehörighet på hello vCenter eller värden, kontrollera att hello-tjänstkontot har dessa behörigheter aktiveras: Datacenter, datalagret, mapp, värd, nätverk, resurs, virtuell dator, och vSphere distribuerade växel. Hello VMware vCenter-servern måste dessutom hello lagring vyer privilegiet aktiverat.


## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön

Innan du konfigurerar hello målmiljön, kontrollera att du har ett Azure storage-konto och virtuella nätverk som har upprättats.

1. Klicka på **Förbered infrastruktur** > **mål**, och välj hello Azure-prenumeration du vill toouse.
2. Ange om ditt mål distributionsmodell är Resource Manager-baserade eller klassiska.
3. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

   ![mål](./media/vmware-walkthrough-source-target/gs-target.png)
4. Om du inte har skapat ett lagringskonto eller nätverket, klickar du på **+ lagringskonto** eller **+ nätverk**, toocreate en Resource Manager-konto eller nätverket infogad.

## <a name="next-steps"></a>Nästa steg

Gå för[steg 9: Konfigurera en replikeringsprincip](vmware-walkthrough-replication.md)
