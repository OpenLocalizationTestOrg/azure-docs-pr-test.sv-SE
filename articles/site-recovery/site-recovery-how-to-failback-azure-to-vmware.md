---
title: "aaaHow toofail tillbaka från Azure tooVMware | Microsoft Docs"
description: "Du kan starta en återställning efter fel toobring virtuella datorer tillbaka tooon lokala efter en redundansväxling av virtuella datorer tooAzure. Lär dig hello steg för hur toofail tillbaka."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: b82abf6b15db9dccab49edbd14298b121e9fdc6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-from-azure-tooan-on-premises-site"></a>Växla tillbaka från Azure tooan lokal plats

Den här artikeln beskriver hur toofail säkerhetskopiera virtuella datorer från Azure Virtual Machines toohello lokal plats. Följ hello instruktionerna i den här artikeln toofail tillbaka din virtuella VMware-datorer eller fysiska Windows-/ Linux-servrar när de har redundansväxlats från hello lokal plats tooAzure med hjälp av hello [replikera VMware-datorer och fysiska servrar tooAzure med Azure Site Recovery](site-recovery-vmware-to-azure-classic.md) kursen.

> [!WARNING]
> Om du har [slutfört en migrering](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration)flyttade hello virtuell tooanother datorresurs grupp- eller borttagna hello Azure-dator, det går inte att återställning efter.

> [!NOTE]
> Om du redundansväxlade virtuella VMware-datorer kan återställning efter fel tooa Hyper-v-värden.

## <a name="overview-of-failback"></a>Översikt över återställning efter fel
Här är hur återställningen fungerar. När du har redundansväxlats tooAzure växlar du tillbaka tooyour lokal plats i några steg:

1. [Skapa nytt](site-recovery-how-to-reprotect.md) hello virtuella datorer i Azure så att de startar tooreplicate tooVMware virtuella datorer i den lokala platsen. Som en del av den här processen kan behöva du också:
    1. Konfigurera ett lokalt huvudmålservern: Windows huvudmålservern för den virtuella Windows-datorer och [Linux-huvudmålserverns](site-recovery-how-to-install-linux-master-target.md) för Linux virtuella datorer.
    2. Konfigurera en [processervern](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Initiera [skyddar](site-recovery-how-to-reprotect.md). Detta inaktiverar hello lokala virtuella datorn och synkronisera hello Azure data för virtuella datorer med hello lokala diskar.
5. När dina virtuella datorer på Azure replikerar tooyour lokal plats, startar du en misslyckas över från Azure toohello lokal plats.

När data har misslyckats tillbaka, skyddar hello lokala virtuella datorer som du inte tillbaka till så att de startar tooreplicate tooAzure.

Titta på hello följande video om hur toofail över från Azure tooan lokal plats för en snabb överblick.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

### <a name="fail-back-toohello-original-or-alternate-location"></a>Växla tillbaka toohello ursprungliga eller en annan plats

Om du redundansväxlade en virtuell VMware-dator kan du växla tillbaka toohello samma källa lokala virtuella datorn om det fortfarande finns. I det här scenariot replikeras endast hello ändringar tillbaka. Det här scenariot kallas för återställning av ursprunglig plats. Om hello lokala virtuella datorn inte finns, är hello scenariot en återställning till alternativ plats.

> [!NOTE]
> Du kan endast återställning toohello ursprungliga vCenter och konfigurationsservern. Du kan inte distribuera en ny konfigurationsservern och återställning med hjälp av den. Du kan också, Lägg till en ny vCenter toohello befintliga konfigurationsservern och återställning efter fel i hello nya vCenter.

#### <a name="original-location-recovery"></a>Återställning av ursprunglig plats

Om du växlar tillbaka toohello ursprungliga virtuella datorn krävs hello följande villkor:
* Om hello virtuella datorn hanteras av en vCenter-server, ska sedan hello master målets ESX-värden ha åtkomst toohello virtuella datorns datalagret.
* Om hello virtuella datorn finns på en ESX-värd men inte hanteras av vCenter, hello hello virtuella datorns hårddisk måste vara i ett datalager hello master målobjektets värden kan komma åt.
* Om den virtuella datorn finns på en ESX-värd och inte använder vCenter, bör du genomföra identifieringen av hello ESX-värd för hello huvudmålservern innan du skyddar. Detta gäller att om du växlar tillbaka fysiska servrar för.
* Du kan växla tillbaka tooa virtuella SAN-nätverk (vSAN) eller en disk som baseras på raw-enhet mappning (RDM) om hello diskar finns redan och är anslutna toohello lokala virtuella datorn.

#### <a name="alternate-location-recovery"></a>Återställning alternativ plats
Om hello lokala virtuella datorn inte finns innan skydda hello virtuella kallas hello scenariot för en återställning till alternativ plats. hello skyddar arbetsflödet skapar hello lokala virtuella datorn igen. Detta medför även fullständiga data hämtas.

* När du växlar tillbaka tooan alternativ plats hello virtuella datorn kommer att återställda toohello samma ESX-värd på vilka hello huvudmålservern har distribuerats. hello datalager som har använt toocreate hello disk kommer att hello samma datalager som valdes när skydda hello virtuell dator.
* Du kan växla tillbaka endast tooa virtuella file system (VMFS) datalagret. Om du har ett virtuellt SAN-nätverk eller RDM fungerar inte skyddar och återställning efter fel.
* Skapa nytt innebär en stor inledande dataöverföringen som följs av hello ändringar. Den här processen finns eftersom hello virtuella dator inte finns lokalt. hello fullständig data måste replikeras tillbaka toobe. Detta skyddar också tar längre tid än en återställning av ursprunglig plats.
* Du kan inte växla tillbaka toovSAN - eller RDM-baserade diskar. Den nya virtuella diskar (VMDKs) kan bara skapas på en VMFS-datalagret.

En fysisk dator när redundansväxlats tooAzure, kan flyttas tillbaka endast som en virtuell VMware-dator (även hänvisade tooas P2A2V). Detta flöde faller under återställning av hello alternativ plats.

* En fysisk server i Windows Server 2008 R2 SP1 kan inte om skyddade och redundansväxlats tooAzure, flyttas tillbaka.
* Se till att du upptäcker att minst en huvudmålservern server och hello nödvändiga ESX/ESXi-värdar toowhich måste toofail tillbaka.

## <a name="have-you-completed-reprotection"></a>Har du slutfört återaktivera skydd?
Innan du går vidare skyddar fullständig hello steg så att hello virtuella datorer är i tillståndet replikerade och du kan starta en växling vid fel tillbaka tooan lokal plats. Mer information finns i [hur tooreprotect från Azure tooon lokala](site-recovery-how-to-reprotect.md).

## <a name="prerequisites"></a>Krav

* En konfigurationsserver krävs lokalt när du gör en återställning efter fel. Under återställning efter fel, hello virtuella datorn måste finnas i hello configuration server-databas eller återställning lyckas inte. Se därför till att du vidtar regelbundna schemalagda säkerhetskopieringar av servern. Om det var en katastrof, behöver du toorestore hello server med hello samma IP-adress för återställning efter fel toowork.
* Hej huvudmålservern ska inte ha eventuella ögonblicksbilder innan återställning.

## <a name="steps-toofail-back"></a>Steg toofail tillbaka

> [!IMPORTANT]
> Se till att du har slutfört återaktivera skydd för hello virtuella datorer innan du startar återställning efter fel. hello virtuella datorer ska vara i ett skyddat läge och deras hälsa bör vara **OK**. tooreprotect hello virtuella datorer, läsa [hur tooreprotect](site-recovery-how-to-reprotect.md).

1. I hello replikerade objekt väljer hello virtuell dator, och högerklicka på den tooselect **oplanerad växling**.
2. I **bekräfta redundans**, kontrollera hello redundansriktning (från Azure) och välj sedan hello återställningspunkt (senaste eller hello senaste appkonsekvent) som du vill toouse hello växling vid fel. hello app konsekvent finns bakom hello senaste punkten i tid och gör att data kan gå förlorade.
3. Site Recovery stängs av hello virtuella datorer i Azure under växling vid fel. När du har kontrollerat att återställningen har slutförts korrekt kan du kontrollera att hello virtuella datorer på Azure har stängts av.

### <a name="toowhat-recovery-point-can-i-fail-back-hello-virtual-machines"></a>toowhat återställningspunkt kan jag växla tillbaka hello virtuella datorer?

Under återställning efter fel har du två alternativ toofail tillbaka hello/återställning av virtuell dator planera.

Om du väljer hello senaste bearbetas punkt i tiden kommer alla virtuella datorer att redundansväxlas tootheir senaste tillgängliga punkt i tiden. Om det finns en replikeringsgrupp inom hello återställningsplan, misslyckas varje virtuell dator i hello replikgrupp över tooits oberoende senaste punkten i tid.

Du kan inte växla tillbaka en virtuell dator tills det har minst en återställningspunkt. Du kan inte växla tillbaka en återställningsplan tills alla virtuella datorer har minst en återställningspunkt.

> [!NOTE]
> Senaste återställningspunkt är en kraschkonsekvent återställningspunkt.

Om du väljer hello programkonsekvent återställningspunkt återställs återställning av en enskild virtuell dator tooits senaste tillgängliga programkonsekventa återställningspunkten. En återställningsplan med en replikeringsgrupp hello gäller återställer varje replikeringsgrupp tooits vanliga tillgängliga återställningspunkten.
Observera att programkonsekventa återställningspunkter kan vara bakom i tid, och det är möjligt att förlust av data.

### <a name="what-happens-toovmware-tools-post-failback"></a>Vad händer tooVMware verktyg efter återställning efter fel?

Under växling vid fel tooAzure kan inte hello VMware-verktyg köras på hello virtuella Azure-datorn. Vid en Windows-dator inaktiverar automatisk Systemåterställning hello VMware-verktyg under växling vid fel. Vid virtuell Linux-dator avinstallerar ASR hello VMware-verktyg under växling vid fel.

Hello VMware-verktyg kan återaktiveras när återställning efter fel under återställning efter fel för hello Windows-dator. På samma sätt för en virtuell dator linux installeras hello VMware-verktyg om på hello datorn vid återställning.

## <a name="next-steps"></a>Nästa steg

När återställningen är klar måste toocommit hello virtuella tooensure som återställde virtuella datorer i Azure tas bort.

### <a name="commit"></a>Checka in
Commit är obligatoriska tooremove hello redundansväxlade virtuella datorn från Azure.
Högerklicka på hello skyddade objekt och klicka sedan på **genomför**. Ett jobb tar bort hello redundansväxling av virtuella datorer i Azure.

### <a name="reprotect-from-on-premises-tooazure"></a>Skydda igen från lokala tooAzure

När genomförande har slutförts, den virtuella datorn är tillbaka på hello lokal plats, men det kommer inte att skydda. toostart tooreplicate tooAzure igen, hello följande:

1. I **valvet** > **inställningen** > **replikerade objekt**, Välj hello virtuella datorer som har misslyckats tillbaka och klicka sedan på  **Skydda igen**.
2. Ge hello värdet hello processervern som behöver toobe används toosend data tillbaka tooAzure.
3. Klicka på **OK** toobegin hello Skapa nytt jobb.

> [!NOTE]
> När en lokal virtuell dator startas, tar det lite tid för hello agent tooregister tillbaka toohello konfigurationsservern (upp too15 minuter). Skapa nytt misslyckas och returnerar ett felmeddelande om att hello-agenten inte installeras under denna tid. Vänta några minuter och försök sedan skyddar igen.

När hello skyddar jobbet har slutförts, hello virtuella datorn replikeras tillbaka tooAzure och du kan göra en växling vid fel.

## <a name="common-issues"></a>Vanliga problem
Kontrollera att vCenter hello är i anslutet tillstånd innan du gör en återställning efter fel. Kopplar från diskar och koppla dem tillbaka toohello annars misslyckas virtuella datorn.
