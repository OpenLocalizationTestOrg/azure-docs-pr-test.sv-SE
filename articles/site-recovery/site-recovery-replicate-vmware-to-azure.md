---
title: Duplicera program (VMware tooAzure) | Microsoft Docs
description: "Den här artikeln beskriver hur tooset duplicering av virtuella datorer som körs på VMware till Azure."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: asgang
ms.openlocfilehash: b07aabdacec521c7bd89e50f6a1427a774ff0287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-applications-running-on-vmware-vms-tooazure"></a>Replikera program som körs på virtuella VMware-datorer tooAzure



Den här artikeln beskriver hur tooset duplicering av virtuella datorer som körs på VMware till Azure.
## <a name="prerequisites"></a>Krav

hello artikeln förutsätter att du har redan

1.  [Konfigurera lokala källmiljön](site-recovery-set-up-vmware-to-azure.md)
2.  [Konfigurera målmiljön i Azure](site-recovery-prepare-target-vmware-to-azure.md)


## <a name="enable-replication"></a>Aktivera replikering
#### <a name="before-you-start"></a>Innan du börjar
Observera att när du replikerar virtuella VMware-datorer:

* Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en ny virtuell dator tooAzure.
* Virtuella VMware-datorer identifieras var 15: e minut. Det kan ta 15 minuter eller längre för dessa tooappear i hello portal efter identifiering. På samma sätt kan identifieringen ta 15 minuter eller mer när du lägger till en ny vCenter-servern eller vSphere-värd.
* Ändringar i miljön på hello virtuella datorn (till exempel VMware verktyg installation) kan ta 15 minuter eller mer toobe uppdateras i hello-portalen.
* Du kan kontrollera hello senast identifierade tid för VMwares virtuella datorer i hello **senaste kontakt på** för hello vCenter server/vSphere-värd, på hello **Konfigurationsservrar** bladet.
* tooadd datorer för replikering utan att vänta på hello schemalagda identifiering, markera hello konfigurationsservern (inte på den), och klicka på hello **uppdatera** knappen.
* När du aktiverar replikering, om hello datorn förbereds installerar hello processervern automatiskt hello mobilitetstjänsten på den.


**Aktivera replikering på följande sätt**:

1. Klicka på **Steg 2: Replikera program** > **Källa**. När du har aktiverat replikering för hello första gången, klickar du på **+ replikera** i hello valvet tooenable replikering för ytterligare datorer.
2. I hello **källa** bladet > **källa**väljer hello konfigurationsservern.
3. I **datorn typen**väljer **virtuella datorer** eller **fysiska datorer**.
4. I **vCenter/vSphere-Hypervisor**väljer hello vCenter-servern som hanterar hello vSphere-värd, eller hello värden. Den här inställningen är inte relevant om du replikerar fysiska datorer.
5. Välj hello processervern. Om du inte skapat några ytterligare servrar blir hello namnet på hello konfigurationsservern. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. I **mål** Välj hello prenumeration och hello resursgruppen där du vill att toocreate hello misslyckades för virtuella datorer. Välj hello distributionsmodell som du vill toouse i Azure (klassiska eller resource management) för hello misslyckades för virtuella datorer.
7. Välj hello Azure storage-konto som du vill använda toouse för replikering av data. Tänk på följande:

   * Du kan välja en premium eller standardlagringskonto. Om du väljer ett premium-konto behöver toospecify en ytterligare ett standardlagringskonto för pågående replikeringsloggar. Konton måste finnas i hello samma region som hello Recovery Services-valvet.
   * Om du vill toouse ett annat lagringskonto än de som du har kan du skapa en*Platshållarlänk för att skapa lagringskonto med hjälp av resource manager som beskrivs i komma igång*. toocreate ett lagringskonto med hjälp av hanteraren för filserverresurser, klickar du på **Skapa nytt**. Om du vill toocreate ett lagringskonto med hjälp av hello klassiska modellen måste du göra det [i hello Azure-portalen](../storage/common/storage-create-storage-account.md).

8. Välj hello Azure nätverk och undernät toowhich virtuella Azure-datorer ansluter när de efter en redundansväxling efter växling vid fel. hello nätverket måste finnas i hello samma region som hello Recovery Services-valvet. Välj **konfigurera nu för valda datorer**, tooapply hello nätverket inställningen tooall datorer som du väljer för skydd. Välj **konfigurera senare** tooselect hello Azure-nätverk per dator. Om du inte har ett nätverk måste för[skapar du en](#set-up-an-azure-network). toocreate ett nätverk med Resource Manager klickar du på **Skapa nytt**. Om du vill toocreate ett nätverk med hello klassiska modellen gör det [i hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Välj ett undernät om det behövs. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. I **virtuella datorer** > **Välj virtuella datorer**och på varje dator som du vill tooreplicate. Du kan bara välja datorer som stöder replikering. Klicka sedan på **OK**.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. I **egenskaper** > **konfigurera egenskaper**väljer hello-konto som ska användas av hello processen server tooautomatically installera hello mobilitetstjänsten på hello-datorn.  
11. Alla diskar replikeras som standard. tooexclude diskar från replikeringen, klickar du på **alla diskar** och avmarkera alla diskar som du inte vill tooreplicate.  Klicka sedan på **OK**. Du kan ange ytterligare egenskaper senare. [Lär dig mer](site-recovery-exclude-disk.md) om Exkludera diskar.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-replication6.png)

12. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, kontrollera att rätt replikeringsprincip väljs hello. Du kan ändra inställningar för replikering i **inställningar** > **replikeringsprinciper** > principnamn > **redigera inställningar för**. Du ändrar tooa princip kommer att tillämpas tooreplicating och nya datorer.
13. Aktivera **konsekvens för flera** om du vill toogather datorer i en replikeringsgrupp och ange ett namn för hello-gruppen. Klicka sedan på **OK**. Tänk på följande:

    * Datorer i replikeringen gruppera replikera och har delat kraschkonsekvent och programkonsekventa återställningspunkter när de växlar över.
    * Vi rekommenderar att du samla in virtuella datorer och fysiska servrar så att de speglar dina arbetsbelastningar. Aktivera konsekvens för flera kan påverka arbetsbelastningens prestanda och bör endast användas om datorer kör hello samma arbetsbelastning och du behöver enhetlighet.

    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/enable-replication7.png)
14. Klicka på **Aktivera replikering**. Du kan följa förloppet för hello **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**. Efter hello **Slutför skydd** jobbet körs hello datorn är redo för redundans.

> [!NOTE]
> Om hello datorn har förberetts för push-installation hello Mobility tjänstkomponent kommer att installeras när skydd är aktiverat. Komponenten har installerats på hello datorn ett skyddsjobb startar och misslyckas efter hello. När du hello fel toomanually måste starta om varje dator. Efter hello omstart hello skydd jobbet börjar igen och inledande replikering.
>
>

## <a name="view-and-manage-vm-properties"></a>Visa och hantera egenskaper för virtuella datorer

Vi rekommenderar att du kontrollerar hello egenskaper för hello källdatorn. Kom ihåg att hello Azure VM-namnet måste uppfylla [Azure virtuella datorns krav](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. Klicka på **inställningar** > **replikerade objekt** >, och välj hello-datorn. Hej **Essentials** bladet visar information om inställningar för datorer och status.
2. I **egenskaper**, kan du visa replikering och information om växling vid fel för hello VM.
3. I **beräknings- och nätverksinställningar** > **beräkna egenskaper**, kan du ange hello Azure VM-namn och mål för storlek. Ändra hello namnet toocomply med krav för Azure om du behöver.
    ![Aktivera replikering](./media/site-recovery-vmware-to-azure/VMProperties_AVSET.png)
 
4.  Du kan välja en [resursgruppen](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) för vilka datorn blir en del av post växla över. Du kan ändra den här inställningen före misslyckas över. Bokför växlar över, om du migrerar hello datorn tooa annan resursgrupp och sedan skyddsinställningar för en dator bryts.
5. Du kan välja en [tillgänglighetsuppsättning](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) om din dator krävs toobe vara en del av en post växlar över. När du väljer tillgänglighetsuppsättning du tänka på:

    * Endast tillgänglighetsuppsättningar tillhör toohello angivna visas resursgruppen  
    * Datorer med olika virtuella nätverk kan inte ingå i samma tillgänglighetsuppsättning
    * Endast virtuella datorer av samma storlek kan ingå i samma tillgänglighetsuppsättning
5. Du kan också visa och lägga till information om hello målnätverket, undernätet och IP-adress som ska tilldelas toohello Azure VM.
6. I **diskar**kan du visa hello operativsystem och datadiskar på hello virtuell dator som kommer att replikeras.

### <a name="network-adapters-and-ip-addressing"></a>Nätverkskort och IP-adresser 

- Du kan ange IP-adressen för hello mål. Om du inte anger någon adress använder hello redundansväxlade datorn DHCP. Om du anger en adress som inte är tillgänglig under växling vid fel, fungerar inte hello växling vid fel. hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.
- hello antalet nätverkskort beror hello storleken du anger för hello för den virtuella måldatorn enligt följande:
    - Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika med toohello antal nätverkskort som tillåts för hello måldatorn och sedan hello målet att ha hello samma antal kort som hello källa.
    - Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello tillåtna antal för hello målstorleken så hello högsta kommer att användas.
    - Till exempel om en källdator har två nätverkskort och hello måldatorn stöder fyra hello måldatorn att ha två kort. Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.
    - Om hello virtuella datorn har flera nätverkskort ansluts alla toohello samma nätverk.
    - Om hello virtuella datorn har flera nätverkskort hello först visas i listan hello blir hello *standard* hello Azure virtuella nätverkskort.
   



## <a name="common-issues"></a>Vanliga problem

* Varje disk måste vara mindre än 1TB i storlek.
* hello OS-disk ska vara en standarddisk och inte dynamisk disk
* För generation 2/UEFI aktiverat virtuella datorer hello operativsystemsfamilj bör vara Windows och startdisk ska vara mindre än 300GB

## <a name="next-steps"></a>Nästa steg

När hello skyddet har slutförts kan du försöka [redundansväxla](site-recovery-failover.md) toocheck om ditt program kommer i Azure eller inte.

Om du vill ha toodisable skydd, kontrollera hur för[Rensa inställningar för registrering och skydd](site-recovery-manage-registration-and-protection.md)
