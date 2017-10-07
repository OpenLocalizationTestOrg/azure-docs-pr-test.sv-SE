---
title: "kapacitet för replikering av aaaEstimate i Azure | Microsoft Docs"
description: "Använd den här artikeln tooestimate kapacitet vid replikering med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: 54d10e50dd4fc1b875273c7fc0f38f0e85dadddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Planera kapacitet för att skydda virtuella datorer och fysiska servrar i Azure Site Recovery

hello hjälper Azure Site Recovery Capacity Planner dig att toofigure ut dina kapacitetskrav vid replikering av Hyper-V virtuella datorer, virtuella VMware-datorer och Windows-/ Linux fysiska servrar med Azure Site Recovery.

Använda hello Site Recovery Capacity Planner tooanalyze din källmiljö och arbetsbelastningar, beräkna bandbreddsbehov och serverresurser som du behöver för hello källplats och hello resurser (virtuella datorer och lagring osv), som du behöver i hello mål plats.

Du kan köra verktyget hello på ett par olika lägen:

* **Snabb planera**: kör hello-verktyget i det här läget tooget nätverks- och projektioner baserat på ett genomsnittligt antal virtuella datorer, diskar, lagring och förändringstakten.
* **Detaljerad planering**: kör hello-verktyget i det här läget och ange information för varje arbetsbelastning på VM-nivå. Analysera VM-kompatibilitet och får nätverks- och projektioner.

## <a name="before-you-start"></a>Innan du börjar


1. Samla in information om din miljö, inklusive virtuella datorer, diskar per virtuell dator lagringsutrymme per disk.
2. Identifiera dina dagliga (omsättningen) förändringstakten för replikerade data. toodo detta:

   * Om du replikerar virtuella Hyper-V-datorer och hämta sedan hello [Hyper-V kapacitetsplanering verktyget](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello förändringstakten. [Lär dig mer](site-recovery-capacity-planning-for-hyper-v-replication.md) om det här verktyget. Vi rekommenderar att du kör det här verktyget över en vecka toocapture medelvärden.
   * Om du replikerar virtuella VMware-datorer använder hello [Azure Site Recovery-distribution Planner](./site-recovery-deployment-planner.md) toofigure ut hello omsättningsuppdateringar hastighet.
   * Om du replikerar fysiska servrar måste tooestimate manuellt.

## <a name="run-hello-quick-planner"></a>Kör hello snabb Planner
1. Ladda ned och öppna hello [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) verktyget. Du behöver toorun makron, så Välj tooenable redigering och aktivera innehåll när du uppmanas.
2. I **ange planner** Välj **snabb Planner** hello listrutan.

   ![Komma igång](./media/site-recovery-capacity-planner/getting-started.png)
3. I hello **Capacity Planner** kalkylblad, ange information om hello krävs. Du måste fylla i alla fält för hello inringad i rött i hello skärmbilden nedan.

   * I **Välj ditt scenario**, Välj **Hyper-V tooAzure** eller **VMware/fysiska tooAzure**.
   * I **genomsnittliga dagliga data ändras hastighet (%)**, placera i hello information som du samlar in med hjälp av hello [Hyper-V kapacitetsplanering verktyget](site-recovery-capacity-planning-for-hyper-v-replication.md) eller hello [Azure Site Recovery-distribution Planner](./site-recovery-deployment-planner.md).  
   * **Komprimering** gäller bara toocompression erbjuds vid replikering av virtuella VMware-datorer eller fysiska servrar tooAzure. Vi uppskattar 30% eller mer, men du kan ändra inställningen hello som krävs. Du kan använda en enhet från tredje part, till exempel Riverbed för att replikera virtuella Hyper-V-datorer tooAzure komprimering.
   * I **kvarhållning indata**, ange hur länge replikerna ska behållas. Om du replikerar virtuella VMware- eller fysiska servrar, kan du ange hello värdet i dagar. Om du replikerar Hyper-V, kan du ange hello tid i timmar.
   * I **antalet timmar för inledande replikering för hello grupp med virtuella datorer bör slutföras** och **antalet virtuella datorer per batch för inledande replikering**, du kan ange inställningar som används krav för toocompute inledande replikering.  När Platsåterställningen har distribuerats ska hello hela inledande datauppsättning överföras.

   ![Indata](./media/site-recovery-capacity-planner/inputs.png)
4. När du har publicerat i hello värden för hello källmiljön innehåller visas utdata:

   * **Bandbredd som krävs för deltareplikering** (MB per sekund). Nätverkets bandbredd för deltareplikering beräknas på hello genomsnittliga dagliga förändringstakten data.
   * **Bandbredd som krävs för inledande replikering** (MB per sekund). Nätverkets bandbredd för inledande replikering beräknas på hello inledande replikering värden som du lägger till i.
   * **Lagringsutrymme som krävs (i GB)** är hello totala Azure-lagring krävs.
   * **Totalt antal IOPS på standardlagring konton** beräknas baserat på 8 kB IOPS storlek på hello totala standard storage-konton.  För hello snabb Planner beräknas hello antal baserat på alla hello virtuella datorer som källdiskarna och dagliga förändringstakten för data. Hej detaljerad Planner för hello talet är beräknat baserat på totala antalet virtuella datorer som är mappade toostandard virtuella Azure-datorer och data ändra på dessa virtuella datorer.
   * **Antalet standardlagring konton** ger hello Totalt antal standardlagring konton som behövs tooprotect hello virtuella datorer. Ett standardlagringskonto kan innehålla upp too20000 IOPS över alla hello virtuella datorer i en standardlagring och stöds högst 500 IOPS per disk.
   * **Antal blob-diskar som krävs för** ger hello antalet diskar som kommer att skapas på Azure-lagring.
   * **Antal premium storage-konton som krävs för** ger hello Totalt antal premium storage konton som behövs tooprotect hello virtuella datorer. En källa för virtuell dator med hög IOPS (större än 20000) måste ett premiumlagringskonto. Premium-lagringskonto kan innehålla upp too80000 IOPS.
   * **Totalt antal IOPS på premium-lagring** beräknas baserat på 256 kB IOPS storlek på hello totala premium storage-konton.  För hello snabb Planner beräknas hello antal baserat på alla hello virtuella datorer som källdiskarna och dagliga förändringstakten för data. Hej detaljerad Planner för hello talet är beräknat baserat på hello Totalt antal virtuella datorer som är mappade toopremium Azure VM (DS och GS-serien) och hello data ändra på dessa virtuella datorer.
   * **Antal configuration-servrar krävs** visar hur många konfigurationsservrar krävs för hello distributionen.
   * **Antal ytterligare servrar krävs** visar om ytterligare servrar krävs, i tillägg toohello processen server som körs på konfigurationsservern hello som standard.
   * **100% ytterligare lagringsutrymme på hello källan** visar om det krävs ytterligare lagringsutrymme i hello källplats.

   ![Resultat](./media/site-recovery-capacity-planner/output.png)

## <a name="run-hello-detailed-planner"></a>Kör hello detaljerad Planner

1. Ladda ned och öppna hello [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) verktyget. Du behöver toorun makron, så Välj tooenable redigering och aktivera innehåll när du uppmanas.
2. I **ange planner**väljer **detaljerad Planner** hello listrutan.

   ![Komma igång](./media/site-recovery-capacity-planner/getting-started-2.png)
3. I hello **arbetsbelastning kriteriet** kalkylblad, ange information om hello krävs. Du måste fylla i alla hello markerad fält.

   * I **processorkärnor**, ange hello Totalt antal kärnor på en källserver.
   * I **minnesallokering i MB**, ange hello RAM storleken på en källserver.
   * Hej **antalet nätverkskort**, ange hello antalet nätverkskort på en källserver.
   * I **totalt lagringsutrymme (i GB)**, ange hello total storlek på hello VM-lagring. Om källservern hello 3 diskar med 500 GB, är totalt lagringsutrymme 1 500 GB.
   * I **antalet diskar som är anslutna**, ange hello Totalt antal diskar på en källserver.
   * I **Disk kapacitetsutnyttjande**, ange hello genomsnittlig användning.
   * I **dagligen ändra hastighet (%)**, ange hello dagliga data ändra frekvensen för en källserver.
   * I **mappning Azure storlek**, ange hello Azure VM-storlek som du vill toomap. Om du inte vill toodo detta manuellt, klickar du på **Compute IaaS-VM**. Om du ange en manuell inställning och klicka sedan på Beräkna IaaS-VM, kan skrivas över hello manuell inställning eftersom hello beräkning automatiskt identifierar hello som bäst matchar på Azure VM-storlek.

   ![Arbetsbelastningen kriteriet](./media/site-recovery-capacity-planner/workload-qualification.png)
4. Om du klickar på **Compute IaaS-VM** här är syfte:

   * Verifierar hello obligatoriska indata.
   * Beräknar IOPS och föreslår hello bästa Azure VM aize matchning för varje virtuella datorer som lämpar sig för replikering tooAzure. Om en lämplig storlek på virtuella Azure-datorn inte går att identifiera ett fel utfärdas. Till exempel om hello antalet diskar som är ansluten i 65, utfärdas ett fel eftersom hello högsta storlek Azure VM är 64.
   * Tyder på ett lagringskonto som kan användas för en Azure VM.
   * Beräknar hello Totalt antal lagringskonton som standard och premium storage-konton som krävs för hello arbetsbelastning. Rulla ned tooview hello Azure lagringstyp och hello storage-konto som kan användas för en källserver.
   * Slutför och sorterar hello resten av hello tabell baserad på lagring (standard eller premium) kopplade typen för en virtuell dator och hello antalet diskar som är anslutna. Hej kolumn för alla virtuella datorer som uppfyller hello för Azure, **är VM kvalificerade?** visar **Ja**. Om en virtuell dator inte kan säkerhetskopieras tooAzure, visas ett fel.

Kolumner AA tooAE är resultatet och ange information för varje virtuell dator.

![Arbetsbelastningen kriteriet](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Exempel
Exempel för sex virtuella datorer med hello värdena som visas i tabellen hello hello verktyget beräknar och tilldelar hello Azure VM matchning och hello Azure lagringskrav.

![Arbetsbelastningen kriteriet](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Observera följande hello i hello exempel på utdata:

  * hello första kolumnen är en verifiering kolumn för hello virtuella datorer, diskar och omsättning.
  * Två standard storage-konton och en premium storage-konto krävs för fem virtuella datorer.
  * VM3 inte uppfyller kraven för skydd, eftersom en eller flera diskar är mer än 1 TB.
  * VM1 och VM2 kan använda hello första standardlagringskonto
  * VM4 kan använda hello andra standardlagringskonto.
  * VM5 och VM6 måste ett premiumlagringskonto och kan båda använda ett konto.

    > [!NOTE]
    > IOPS på standard och premium-lagring beräknas på hello VM-nivå och inte på disk nivå. En virtuell dator som standard kan hantera upp too500 IOPS per disk. Om IOPS för en disk som är större än 500, måste premium-lagring. Men om IOPS för en disk som är mer än 500, men IOPS för hello totala Virtuella diskar som är inom hello stöder standard gränser som virtuell Azure-dator (VM-storlek, antalet diskar, antalet nätverkskort, CPU, minne) hämtar sedan hello planner en standard virtuella datorn och inte hello DS eller GS-serien. Du vill toomanually uppdatering hello mappning Azure storlek med lämpliga DS eller GS-serien VM.


När alla hello-information är på plats klickar du på **skicka data toohello kapacitetsplaneringsverktyget för** tooopen hello **Capacity Planner** arbetsbelastningar är markerat tooshow oavsett om de är berättigade till skydd eller inte.

### <a name="submit-data-in-hello-capacity-planner"></a>Skicka data i hello Capacity Planner
1. När du öppnar hello **Capacity Planner** kalkylbladet fylls baserat på hello-inställningar som du har angett. Hej ordet ”arbetsbelastning' visas i hello **Infra indata källa** cell, tooshow som hello indata är hello **arbetsbelastning kriteriet** kalkylblad.
2. Om du vill toomake ändringar, behöver du toomodify hello **arbetsbelastning kriteriet** kalkylblad och klickar på **skicka data toohello kapacitetsplaneringsverktyget för** igen.  

   ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)
