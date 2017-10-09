---
title: "aaaReplicate virtuella VMware-datorer och fysiska servrar tooAzure (klassiska äldre) | Microsoft Docs"
description: "Beskriver hur tooreplicate lokala virtuella datorer och Windows-/ Linux fysiska servrar tooAzure med hjälp av Azure Site Recovery i en äldre distribution i hello klassiska portalen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: f980fb52-8ec2-4dd9-85e2-8e52e449f1ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: raynew
ms.openlocfilehash: 64c6d906d54426fdd2b884350542a4562bb12528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-tooazure-with-azure-site-recovery-using-hello-classic-portal-legacy"></a>Replikera virtuella VMware-datorer och fysiska servrar tooAzure med Azure Site Recovery med hello klassiska portalen (bakåtkompatibelt)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Klassisk portal](site-recovery-vmware-to-azure-classic.md)
> * [Klassiska portalen (bakåtkompatibelt)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Välkommen tooAzure Site Recovery! Den här artikeln beskriver en äldre distribution för att replikera lokala virtuella VMware-datorer eller Windows-/ Linux fysiska servrar tooAzure med hjälp av Azure Site Recovery i hello klassiska portalen.

## <a name="overview"></a>Översikt
Organisationer behöver en BCDR-strategi som avgör hur appar, arbetsbelastningar och data fungerar och är tillgängliga under planerade och oplanerade driftavbrott och återställa toonormal drifttillstånd så snart som möjligt. Avsikten med en BCDR-strategi är att skydda affärsdata och se till att de kan återställas samt att säkerställa att arbetsbelastningar förblir tillgängliga i händelse av allvarliga fel.

Site Recovery är en Azure-tjänst som stödjer tooyour BCDR-strategi genom att samordna replikeringen av lokala fysiska servrar och virtuella datorer toohello molnet (Azure) eller tooa sekundärt datacenter. Vid driftstopp på den primära platsen växlar du över toohello sekundär plats tookeep appar och arbetsbelastningar som är tillgängliga. Du växlar tillbaka tooyour primära platsen när den returnerar toonormal åtgärder. Läs mer i [Vad är Azure Site Recovery?](site-recovery-overview.md)

> [!WARNING]
> Den här artikeln innehåller **äldre instruktioner**. Använd inte det för nya distributioner. I stället [följer du dessa instruktioner](site-recovery-vmware-to-azure.md) toodeploy Site Recovery i hello Azure-portalen eller [Följ dessa instruktioner](site-recovery-vmware-to-azure-classic.md) tooconfigure hello förbättrad distribution i hello klassiska portalen. Vi rekommenderar att du migrerar toohello förbättrad distribution i hello klassiska portalen om du redan har distribuerat med hello-metod som beskrivs i den här artikeln.
>
>

## <a name="migrate-toohello-enhanced-deployment"></a>Migrera toohello förbättrad distribution
Det här avsnittet gäller endast om du redan har distribuerat Site Recovery med hello instruktioner i den här artikeln.

toomigrate den befintliga distributionen måste du:

1. Distribuera nya Site Recovery-komponenter på den lokala platsen.
2. Konfigurera behörigheterna för automatisk identifiering av virtuella VMware-datorer på hello nya konfigurationsservern.
3. Identifiera hello VMware-servrar med hello nya konfigurationsservern.
4. Skapa en ny skyddsgrupp med hello nya konfigurationsservern.

Innan du börjar:

* Vi rekommenderar att du konfigurerar en underhållsperiod för migrering.
* Hej **migrera datorer** alternativet är bara tillgängligt om du har befintliga skyddsgrupper som skapades under en äldre distribution.
* När du har slutfört hello migreringssteg kan det ta 15 minuter eller längre toorefresh hello autentiseringsuppgifter och toodiscover och uppdatera virtuella datorer så att du kan lägga till dem tooa skyddsgruppen. Du kan uppdatera manuellt i stället för att vänta.

Migrera enligt följande:

1. Läs mer om [förbättrad distribution i hello klassiska portalen](site-recovery-vmware-to-azure-classic.md). Granska hello förbättrad [arkitektur](site-recovery-vmware-to-azure-classic.md), och [krav](site-recovery-vmware-to-azure-classic.md).
2. Avinstallera mobilitetstjänsten för hello från datorer som du replikerar. En ny version av hello service installeras på datorer som hello när du lägger till dem toohello ny skyddsgrupp.
3. Hämta en [valvregistreringsnyckel](site-recovery-vmware-to-azure-classic.md) och [kör installationsguiden för hello enhetlig](site-recovery-vmware-to-azure-classic.md) tooinstall hello konfigurationsservern och processervern master mål server-komponenter. Läs mer om [kapacitetsplanering](site-recovery-vmware-to-azure-classic.md).
4. [Ställ in autentiseringsuppgifter](site-recovery-vmware-to-azure-classic.md) som Site Recovery kan använda tooaccess VMware server tooautomatically identifiera virtuella VMware-datorer. Lär dig mer om [nödvändiga behörigheter](site-recovery-vmware-to-azure-classic.md).
5. Lägg till [vCenter-servrar eller vSphere värdar](site-recovery-vmware-to-azure-classic.md). Det kan ta 15 minuter för servrar tooappear i hello Site Recovery-portalen.
6. Skapa en [ny skyddsgrupp](site-recovery-vmware-to-azure-classic.md). Det kan ta upp too15 minuter för hello portal toorefresh så att hello virtuella datorer identifieras och visas. Om du inte vill toowait du kan markera namnet på hanteringsservern hello (inte på den) > **uppdatera**.
7. Klicka på under hello ny skyddsgrupp **migrera datorer**.

    ![Lägg till konto](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)
8. I **Välj datorer** väljer hello skyddsgrupp du vill använda toomigrate från och hello datorer som du vill toomigrate.

    ![Lägg till konto](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)
9. I **konfigurera Målinställningar** ange om du vill toouse hello samma inställningar för alla datorer och välj hello processervern och Azure storage-konto. Om du inte har någon separat processerver blir hello hello IP-adress för hello configuration server-server.

    ![Lägg till konto](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migrering av lagringskonton](../resource-group-move-resources.md) över resurs grupper i hello samma prenumeration eller alla prenumerationer stöds inte för storage-konton som används för att distribuera Site Recovery.

1. I **ange konton**, Välj hello-konto som du skapade för hello processen server tooaccess hello datorn toopush hello ny version av hello mobilitetstjänsten.

   ![Lägg till konto](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)
2. Site Recovery kommer att migrera dina replikerade data toohello Azure storage-konto som du angav. Alternativt kan du återanvända hello storage-konto som du använde i äldre hello-distribution.
3. Efter hello jobb synkroniseras automatiskt slutförs virtuella datorer. När synkroniseringen har slutförts kan du ta bort hello virtuella datorer från hello äldre skyddsgrupp.
4. Du kan ta bort hello äldre skyddsgruppen när alla datorer har migrerats.
5. Kom ihåg toospecify hello redundans egenskaper för datorer och hello Azure-nätverksinställningar när synkroniseringen är klar.
6. Om du har befintliga återställningsplaner, du kan migrera dem toohello förbättrad distribution med hello **migrera Recovery planera** alternativet. Du bör bara göra detta när alla skyddade datorer har migrerats.

   ![Lägg till konto](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

> [!NOTE]
> När du är klar med migreringen fortsätta med hello [förbättrad artikel](site-recovery-vmware-to-azure-classic.md). hello resten av äldre artikeln kommer inte längre att relevanta och du behöver inte toofollow någon av hello stegen som beskrivs i it **.
>
>

## <a name="what-do-i-need"></a>Vad behöver jag?
Det här diagrammet visar hello komponenter.

![Nytt valv](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Du behöver det här:

| **Komponent** | **Distribution** | **Detaljer** |
| --- | --- | --- |
| **Konfigurationsserver** |En Azure standard A3 virtuell dator i hello samma prenumeration som Site Recovery. |hello konfigurationsservern samordnar kommunikationen mellan skyddade datorer, hello processervern och huvudmålservern servrar i Azure. Den konfigurerar replikering och koordinater återställning i Azure vid en redundansväxling. |
| **Huvudmålservern** |En virtuell Azure-dator – antingen en Windows server baserat på en Windows Server 2012 R2-galleriet avbildning (tooprotect Windows-datorer) eller som en Linux-server baserat på en bild till galleriet OpenLogic CentOS 6.6 (tooprotect Linux-datorer).<br/><br/> Tre storlekar är tillgängliga – Standard A4, Standard D14 och Standard DS4.<br/><br/> hello-servern är ansluten toohello samma Azure-nätverk som hello konfigurationsservern.<br/><br/> Du ställer in i hello Site Recovery-portalen |Den tar emot och lagrar replikerade data från dina skyddade datorer med anslutna virtuella hårddiskar skapas på blob-lagring i ditt Azure storage-konto.<br/><br/> Välj Standard DS4 specifikt för att konfigurera skydd för arbetsbelastningar som kräver konsekvent hög prestanda och låg latens med Premium-Lagringskontot. |
| **Processervern** |En lokal virtuell eller fysisk server som kör Windows Server 2012 R2<br/><br/> Vi rekommenderar att den har placerats på hello samma nätverk och LAN-segment som hello datorer som du vill tooprotect, men det kan köras i ett annat nätverk så länge skyddade datorer har L3 nätverks synlighet tooit.<br/><br/> Du ställa in och registrera den toohello konfigurationsservern i hello Site Recovery-portalen. |Skyddade datorer skickar processerver för replikering data toohello lokalt. Den har en diskbaserad cacheminne toocache replikeringsdata som tas emot. Ett antal åtgärder utförs på dessa data.<br/><br/> Detta optimerar data efter cachelagring, komprimering och kryptering innan du skickar på toohello huvudmålservern.<br/><br/> Den hanterar push-installation av hello Mobilitetstjänsten.<br/><br/> Den utför automatisk identifiering av virtuella VMware-datorer. |
| **Lokala datorer** |Lokala virtuella VMware-datorer eller fysiska servrar som kör Windows eller Linux. |Du kan konfigurera replikeringsinställningar som gäller en eller flera datorer. Du kan växla över en enskild dator eller vanligare, flera datorer som du samla in en återställningsplan. |
| **Mobilitetstjänsten** |Installerad på varje virtuell dator eller fysisk server som vill du tooprotect<br/><br/> Kan installeras manuellt eller flyttas och installerats automatiskt av hello processervern när du aktiverar replikering för en dator. |Hej mobilitetstjänsten skickar data toohello processervern under den första replikeringen (synkronisera). När hello datorn är i ett skyddat läge (efter att synkronisering har slutförts) hello mobilitetstjänsten fångar skrivningar toodisk i minnet och skickar dem till toohello processervern. Applicationconsistency för Windows-servrar uppnås med VSS. |
| **Azure Site Recovery-valvet** |Du skapar ett Site Recovery-valv med en Azure-prenumeration och registrera servrarna i hello-valvet. |hello valvet samordnar och styr replikering, redundans och återställning mellan din lokala plats och Azure. |
| **Replikeringsmekanism** |**Över hello Internet**– kommunicerar och replikerar data från skyddade lokala servrar tooAzure med hjälp av säkra SSL/TLS-kanalen över hello internet. Det här är standardalternativet för hello.<br/><br/> **VPN/ExpressRoute**– kommunicerar och replikerar data mellan lokala servrar och Azure via en VPN-anslutning. Du behöver tooset upp en plats-till-plats-VPN eller en ExpressRoute-anslutning mellan lokal plats och Azure-nätverk.<br/><br/> Väljer du hur du vill tooreplicate under distributionen av Site Recovery. Du kan inte ändra hello mekanism när den har konfigurerats utan att påverka replikering av befintliga datorer. |Varken alternativet måste du tooopen alla inkommande nätverksportar på skyddade datorer. Alla nätverkskommunikation initieras från hello lokal plats. |

## <a name="capacity-planning"></a>Kapacitetsplanering
hello huvudområden behöver du tooconsider:

* **Källmiljö**– hello VMware-infrastruktur, inställningar för datakälla datorn och krav.
* **Komponentservrarna**– hello processervern och konfigurationsservern huvudmålservern

### <a name="considerations-for-hello-source-environment"></a>Överväganden för hello källmiljön
* **Maximala diskstorleken**– hello aktuella maxstorleken för hello-disk som bifogade tooa virtuell dator är 1 TB. Därför är hello maximala storleken på källdisken som kan replikeras också begränsad too1 TB.
* **Maximal storlek per källa**– hello maximal storlek för en enskild källdator är 31 TB (med 31 diskar) och en D14-instans som har etablerats för hello huvudmålservern.
* **Antal källor per huvudmålservern**– flera källdatorer som kan skyddas med en enda huvudmålserver. En enda källdatorn kan inte skyddas på flera huvudmålservern servrar eftersom som diskar replikera en virtuell Hårddisk som speglar hello hello diskens storlek skapas på Azure-blobblagring och bifogas som en data disk toohello huvudmålserver.  
* **Maximal dagliga förändringstakten per källa**– det finns tre faktorer som måste toobe beaktas när du överväger hello bör ändra frekvensen per källa. Hello mål baserat överväganden krävs två IOPS på hello måldisken för varje åtgärd i hello källa. Det beror på att en gamla läsning och skrivning av nya hello-data sker på hello måldisken.
  * **Varje dag ändrar som stöds av hello processervern**– en källdator kan sträcka sig över flera servrar. En enda process-server kan stöda upp dagliga förändringstakten too1 TB. Därför är 1 TB hello maximala dagliga data ändras som stöds för en källdator.
  * **Maximalt dataflöde som stöds av hello måldisken**– maximalt omsättning per källdisken får inte vara mer än 144 GB/dag (med 8 kB skrivåtgärder storlek). Se tabellen hello under hello huvudmålservern för hello genomflöde och IOPs hello mål för olika storlekar för skrivning. Siffran måste delas av två eftersom varje källa IOP genererar 2 IOPS på hello måldisken. Läs mer om [Azure skalbarhets- och prestandamål](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) när du konfigurerar hello mål för premium-lagringskonton.
  * **Maximalt dataflöde som stöds av hello lagringskonto**– en källa kan sträcka sig över flera lagringskonton. Att ett lagringskonto tar högst 20 000 begäranden per sekund och att varje källa IOP genererar 2 IOPS på hello huvudmålservern får rekommenderar vi att du behåller hello antalet IOPS över hello källa too10, 000. Läs mer om [Azure skalbarhets- och prestandamål](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) när du konfigurerar hello källa för premium-lagringskonton.

### <a name="considerations-for-component-servers"></a>Överväganden för komponentservrarna
Tabell 1 sammanfattar hello storlekar för virtuella datorer för hello konfiguration och huvudmålservern servrar.

| **Komponent** | **Distribuerade Azure-instanser** | **Kärnor** | **Minne** | **Maximalt antal diskar** | **Diskens storlek** |
| --- | --- | --- | --- | --- | --- |
| Konfigurationsservern |Standard A3 |4 |7 GB |8 |1 023 GB |
| Huvudmålservern |Standard A4 |8 |14 GB |16 |1 023 GB |
| Standard D14 |16 |112 GB |32 |1 023 GB | |
| Standard DS4 |8 |28 GB |16 |1 023 GB | |

**Tabell 1**

#### <a name="process-server-considerations"></a>Bearbeta server-överväganden
Processen server storlek beror vanligtvis på hello dagliga förändringstakten över alla skyddade arbetsbelastningar.

* Du behöver tillräckligt beräkning tooperform aktiviteter, till exempel infogade komprimering och kryptering.
* Hej processervern använder diskbaserade cachen. Kontrollera att hello rekommenderas cache-utrymme och diskgenomflödet är tillgängliga toofacilitate hello dataändringarna lagras i hello-händelse flaskhalsar i nätverket eller avbrott.
* Se till att tillräckligt med bandbredd så att hello processervern kan överföra hello data toohello huvudmålservern server tooprovide kontinuerligt dataskydd.

Tabell 2 innehåller en översikt över hello processen server riktlinjer.

| **Dataändringshastighet** | **CPU** | **Minne** | **Cachestorleken för disk** | **Cache-diskgenomflödet** | **Bandbredd ingång-/ utgång** |
| --- | --- | --- | --- | --- | --- |
| < 300 GB |4 vCPUs (2 sockets * 2 kärnor @ 2,5 GHz) |4 GB |600 GB |7 too10 MB per sekund |30 Mbit/s/21 Mbit/s |
| 300 too600 GB |8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz) |6 GB |600 GB |11 too15 MB per sekund |60 Mbit/s/42 Mbit/s |
| 600 GB too1 TB |12 vCPUs (2 sockets * @ 2,5 GHz-6 kärnor) |8 GB |600 GB |16 too20 MB per sekund |100 Mbit/s/70 Mbit/s |
| > 1 TB |Distribuera en annan processerver | | | | |

**Tabell 2**

Där:

* Ingång är download bandbredd (intranät mellan hello käll- och server).
* Utgående är Överför bandbredd (internet mellan hello processervern och huvudmålservern). Utgående siffror förutsätta genomsnittlig 30% processen server komprimering.
* Disk en separat OS-disk på minst 128 GB rekommenderas för alla servrar för cachen.
* För cache disk genomströmning hello efter lagring användes för prestandamätningar: 8 SAS-enheter på 10 K RPM med RAID 10-konfiguration.

#### <a name="configuration-server-considerations"></a>Överväganden vid konfiguration av servern
Varje configuration-server kan stöda upp too100 källdatorer med 3 och 4 volymer. Om distributionen är större rekommenderar vi att du distribuerar en annan konfigurationsservern. Se tabell 1 för hello virtuella standardegenskaperna för hello konfigurationsservern.

#### <a name="master-target-server-and-storage-account-considerations"></a>Huvudmålservern server- och överväganden för användarkonton
hello lagring för varje huvudmålservern innehåller en OS-disk, en kvarhållningsvolymen och datadiskar. Hej kvarhållningsenhetens underhåller hello journal diskändringarna för hello varaktighet hello kvarhållningsperiod som definierats i hello Site Recovery-portalen.  Läs tooTable 1 för hello virtuella egenskaper hello huvudmålservern. Tabell 3 visar hur hello diskar för A4 används.

| **Instans** | **OS-disk** | **Kvarhållning** | **Datadiskar** |
| --- | --- | --- | --- |
| **Kvarhållning** |**Datadiskar** | | |
| Standard A4 |1 disk (1 * 1 023 GB) |1 disk (1 * 1 023 GB) |15 diskar (15 * 1 023 GB) |
| Standard D14 |1 disk (1 * 1 023 GB) |1 disk (1 * 1 023 GB) |31 diskar (15 * 1 023 GB) |
| Standard DS4 |1 disk (1 * 1 023 GB) |1 disk (1 * 1 023 GB) |15 diskar (15 * 1 023 GB) |

**Tabell 3**

Kapacitetsplanering för hello huvudmålservern beror på:

* Azure lagringsprestanda och begränsningar
  * hello maxantalet hög utnyttjade diskar för en Standard-nivån VM, ungefär 40 (20 000/500 IOPS per disk) i ett enda lagringskonto. Läs mer om [skalbarhetsmål för standard lagringskonton](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) och [premiumlagringskonton](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
* Dagliga förändringstakten
* Kvarhållning volym lagring.

Tänk på följande:

* En källa kan sträcka sig över flera lagringskonton. Detta gäller toohello datadisk som går du valde när du konfigurerar skydd för toohello storage-konton. hello OS och hello kvarhållning diskar går vanligtvis toohello distribueras automatiskt storage-konto.
* hello kvarhållning lagringsvolym som krävs beror på hello dagliga förändringstakten och hello antal kvarhållning dagar. Hej kvarhållning lagringsutrymme som krävs per huvudmålservern = totala omsättningen från källdatorn per dag * antal kvarhållning dagar.
* Varje huvudmålservern har endast en kvarhållningsvolymen. Hej kvarhållningsvolymen delas mellan hello diskar anslutna toohello huvudmålservern. Exempel:
  * Om det finns en källdator med 5 diskar och varje disk genererar 120 IOPS (om 8K storlek) på hello källan, innebär detta too240 IOPS per disk (2 åtgärder på hello måldisken per källa IO). 240 IOPS ligger inom hello Azure per disk IOPS högst 500.
  * Hej kvarhållningsvolymen detta blir 120 * 5 = 600 IOPS och det kan bli en bottle utlopp. I det här scenariot skulle en bra strategi tooadd flera diskar toohello kvarhållningsvolymen och omfattar den över, som en stripe RAID-konfiguration. Detta förbättrar prestanda eftersom hello IOPS som är fördelade på flera enheter. hello antalet enheter toobe som lagts till toohello kvarhållningsvolymen är följande:
    * Totalt antal IOPS från källmiljön / 500
    * Totala omsättningen per dag från källmiljön (okomprimerade) / 287 GB. 287 GB är hello maximalt dataflöde som stöds av en måldisken per dag. Det här måttet varierar beroende på hello skrivåtgärder storlek med en faktor på 8 kB eftersom i det här fallet är 8 kB dig antas skrivåtgärder storlek. Till exempel om hello skriva storlek är 4K genomströmning ska vara 287/2. Och om hello skrivåtgärder storlek är 16 kB genomströmning kommer att vara 287 * 2.
* Hej antalet storage-konton som krävs = totala source IOPs/10000.

## <a name="before-you-start"></a>Innan du börjar
| **Komponent** | **Krav** | **Detaljer** |
| --- | --- | --- |
| **Azure-konto** |Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/). | |
| **Azure Storage** |Du behöver ett Azure storage-konto toostore replikerade data<br/><br/> Antingen hello-kontot ska vara en [Standard Geo-redundant Lagringskonto](../storage/common/storage-redundancy.md#geo-redundant-storage) eller [Premiumlagringskonto](../storage/common/storage-premium-storage.md).<br/><br/> Det måste i hello samma region som hello Azure Site Recovery-tjänsten och vara associerat med hello samma prenumeration. Vi stöder inte hello flytt av lagringskonton som skapats med hjälp av hello [nya Azure-portalen](../storage/common/storage-create-storage-account.md) över resursgrupper.<br/><br/> Läs mer toolearn [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md) | |
| **Virtuella Azure-nätverket** |Du behöver ett Azure-virtuella nätverk på vilka hello konfigurationsservern och huvudmålservern kommer att distribueras. Det bör vara i hello samma prenumeration och region som hello Azure Site Recovery-valvet. Om du inte vill tooreplicate data via en ExpressRoute- eller VPN-anslutning-hello Azure virtual måste network vara anslutna tooyour lokalt nätverk via en ExpressRoute-anslutning eller en plats-till-plats-VPN. | |
| **Azure-resurser** |Kontrollera att du har tillräckligt med Azure-resurser toodeploy alla komponenter. Läs mer i [Azure-prenumerationsbegränsningar](../azure-subscription-service-limits.md). | |
| **Azure Virtual Machines** |Virtuella datorer som du vill tooprotect måste uppfylla [krav för Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).<br/><br/> **Disken antal**– högst 31 diskar kan användas på en skyddad server<br/><br/> **Disken storlekar**– enskilda diskkapacitet får inte vara mer än 1 023 GB<br/><br/> **Klustring**– klustrade servrar stöds inte<br/><br/> **Start**– Extensible Firmware Interface UEFI (Unified) / Start Extensible Firmware via stöds inte<br/><br/> **Volymer**– Bitlocker-krypterade volymer inte stöds<br/><br/> **Servernamn**– namn måste innehålla mellan 1 och 63 tecken (bokstäver, siffror och bindestreck). hello namn måste börja med en bokstav eller siffra och sluta med en bokstav eller siffra. När en dator skyddas kan du ändra hello Azure namn. | |
| **Konfigurationsserver** |Standard A3 virtuell dator baserat på en bild för Azure Site Recovery Windows Server 2012 R2-galleriet kommer att skapas i din prenumeration för hello konfigurationsservern. Det skapas som hello första instans i en ny molntjänst. Om du väljer offentliga Internet eftersom hello anslutningstypen för hello configuration server hello Molntjänsten kommer att skapas med reserverade offentliga IP-adress.<br/><br/> hello installationssökvägen måste vara i engelska tecken. | |
| **Huvudmålservern** |Azure-dator, standard A4 D14 eller DS4.<br/><br/> hello installationssökvägen måste vara i engelska tecken. Till exempel hello-sökvägen måste ha **/usr/local/ASR** för en huvudmålserver som kör Linux. | |
| **Processervern** |Du kan distribuera hello processervern på fysisk eller virtuell dator som kör Windows Server 2012 R2 med hello senaste uppdateringarna. Installera på C: /.<br/><br/> Vi rekommenderar att du placerar hello server på hello samma nätverk och undernät som hello datorer som du vill tooprotect.<br/><br/> Installera VMware vSphere CLI 5.5.0 på hello processervern. hello VMware vSphere CLI komponenten krävs på hello processervern i ordning toodiscover virtuella datorer som hanteras av en vCenter-server eller virtuella datorer på ESXi-värd.<br/><br/> hello installationssökvägen måste vara i engelska tecken.<br/><br/> ReFS-filsystemet stöds inte. | |
| **VMware** |En VMware vCenter-servern hanterar VMware vSphere-hypervisor-program. Den bör köras vCenter version 5.1 eller 5.5 hello senaste uppdateringarna.<br/><br/> En eller flera vSphere-hypervisor-program som innehåller virtuella VMware-datorer vill du tooprotect. hello hypervisor-programmet ska köras ESX/ESXi version 5.1 eller 5.5 hello senaste uppdateringarna.<br/><br/> Virtuella VMware-datorer ska ha VMware-verktyg installerat och körs. | |
| **Windows-datorer** |Skyddade fysiska servrar eller VMware-datorer som kör Windows har ett antal krav.<br/><br/> En 64-bitars operativsystem som stöds: **Windows Server 2012 R2**, **Windows Server 2012**, eller **Windows Server 2008 R2 med på minst SP1**.<br/><br/> Hej värdnamn, monteringspunkter, enhetsnamn, Windows systemsökväg (t.ex: C:\Windows) ska endast på engelska.<br/><br/> hello operativsystemet ska installeras på enhet C:\.<br/><br/> Endast standarddiskar stöds. Dynamiska diskar stöds inte.<br/><br/> Brandväggsregler på skyddade datorer ska tillåta dem tooreach hello konfiguration och master målservrar i Azure.p ><p>Du behöver tooprovide ett administratörskontot (måste vara lokal administratör på hello Windows-dator) toopush installera hello Mobilitetstjänsten på Windows-servrar. Om hello som kontot är ett domänkonto måste toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator. toodo denna Lägg till hello LocalAccountTokenFilterPolicy DWORD registerpost med värdet 1 under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. tooadd hello registerposten från en CLI öppna cmd eller powershell och ange  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** . [Lär dig mer](https://msdn.microsoft.com/library/aa826699.aspx) om åtkomstkontroll.<br/><br/> Efter växling vid fel, om du vill ansluta tooWindows virtuella datorer i Azure med Fjärrskrivbord Se till att Fjärrskrivbord har aktiverats för hello lokal dator. Om du inte ansluter via VPN, brandväggsreglerna tillåter anslutningar till fjärrskrivbord över hello internet. | |
| **Linux-datorer** |En stöds 64-bitars operativsystem: **Centos 6.4, 6.5, 6.6**; **Oracle Enterprise Linux 6.4, 6.5 kör hello Red Hat kompatibel kernel eller Unbreakable Enterprise Kernel version 3 (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Brandväggsregler på skyddade datorer ska tillåta dem tooreach hello konfigurations- och huvudmålservern servrar i Azure.<br/><br/> / etc/hosts-filer på skyddade datorer bör innehålla poster som mappar hello lokal värd namn tooIP adresser som är associerade med alla nätverkskort <br/><br/> Om du vill tooconnect tooan virtuella Azure-datorn kör Linux efter redundans med en Secure Shell-klient (ssh), se till att hello Secure Shell-tjänsten på hello skyddade datorn anges toostart automatiskt på systemstart och att brandväggsreglerna tillåter en ssh anslutningen tooit.<br/><br/> hello värdnamn, monteringspunkter, enhetsnamn, och Linux system sökvägar och filnamn (t ex/etc /; / usr) ska endast på engelska.<br/><br/> Skydd kan aktiveras för lokala datorer med följande lagring hello:-<br>Filsystem: EXT3, ETX4, ReiserFS, XFS<br>Multipath programvara enhet Mapper (MPIO)<br>Volymhanterare: LVM2<br>Fysiska servrar med HP CCISS domänkontrollant lagring stöds inte. | |
| **Från tredje part** |Vissa distributionskomponenter i det här scenariot beror på programvara från tredje part toofunction korrekt. En fullständig lista finns [tredje parts programvara meddelanden och information](#third-party) | |

### <a name="network-connectivity"></a>Nätverksanslutning
Du har två alternativ när du konfigurerar nätverksanslutningen mellan din lokala plats och hello Azure-nätverk på vilka hello infrastrukturkomponenter (konfigurationsservern, huvudmålservern servrar) har distribuerats. Du behöver toodecide vilka network connectivity alternativet toouse innan du kan distribuera din server för konfiguration. Du behöver toochoose denna inställning på hello tiden för distributionen. Det går inte att ändra dem senare.

**Internet:** kommunikation och replikering av data mellan hello lokala servrar (processervern, skyddade datorer) och hello Azure komponenten infrastrukturservrar (konfigurationsservern, huvudmålserver) sker via en säker SSL / TLS-anslutningen från lokala toohello offentliga slutpunkter på hello-konfiguration och master målservrar. (hello enda undantaget är hello anslutning mellan hello processervern och hello huvudmålservern på TCP-port 9080 som är okrypterad. Endast kontrollinformation som rör toohello replikering protokoll för replikering installationsprogrammet utbyts på den här anslutningen.)

![Distribution av diagram internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: kommunikation och replikering av data mellan hello lokala servrar (processervern, skyddade datorer) och hello Azure komponenten infrastrukturservrar (konfigurationsservern, huvudmålserver) sker via en VPN-anslutning mellan ditt lokala nätverk och hello Azure virtuella nätverk på vilka hello konfigurationsservern och huvudmålservern-servrar distribueras. Kontrollera att ditt lokala nätverk är anslutna toohello virtuella Azure-nätverket av en ExpressRoute-anslutning eller en plats-till-plats VPN-anslutning.

![Distribution av diagram VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)

## <a name="step-1-create-a-vault"></a>Steg 1: Skapa ett valv
1. Logga in toohello [hanteringsportalen](https://portal.azure.com).
2. Expandera **datatjänster** > **återställningstjänster** och på **Site Recovery-valv**.
3. Klicka på **Skapa nytt** > **Snabbregistrering**.
4. I **namn**, ange ett eget namn tooidentify hello valv.
5. I **Region**, Välj hello geografiskt område för hello-valvet. toocheck stöds regioner finns under geografisk tillgänglighet i avsnittet [Azure Site Recovery-prisinformation](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Klicka på **Skapa valv**.

    ![Nytt valv](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Kontrollera hello status fältet tooconfirm som hello valvet har skapats. hello valvet visas som **Active** på hello huvudsakliga **återställningstjänster** sidan.

## <a name="step-2-deploy-a-configuration-server"></a>Steg 2: Distribuera en konfigurationsserver
### <a name="configure-server-settings"></a>Konfigurera serverinställningar
1. I hello **återställningstjänster** klickar du på hello valvet tooopen hello snabbstartsidan. Även du kan öppna Snabbstart när som helst med hello-ikonen.

    ![Snabbstartsikon](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)
2. Välj i listrutan hello **mellan en lokal plats med VMware/fysiska servrar och Azure**.
3. I **förbereda Target(Azure) resurser** klickar du på **distribuera konfigurationsservern**.

    ![Distribuera konfigurationsservern](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)
4. I **nya Server-konfigurationsinformation** ange:

   * Ett namn för hello configuration server och autentiseringsuppgifter tooconnect tooit.
   * Listrutan i hello nätverkstyp för anslutningen väljer **offentliga Internet** eller **VPN**. Observera att du inte kan ändra den här inställningen när den används.
   * Välj hello Azure-nätverk på vilka hello servern bör ligga. Om du använder VPN-Kontrollera att är hello Azure-nätverk anslutna tooyour lokalt nätverk som förväntat.
   * Ange hello interna IP-adress och nätmask som ska tilldelas toohello server. Observera att hello fyra första IP-adresser i varje undernät är reserverade för intern användning i Azure. Använd en annan tillgänglig IP-adress.

     ![Distribuera konfigurationsservern](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)
5. När du klickar på **OK** en standard A3 virtuell dator baserat på en bild för Azure Site Recovery Windows Server 2012 R2-galleriet kommer att skapas i din prenumeration för hello konfigurationsservern. Det skapas som hello första instans i en ny molntjänst. Om du har valt tooconnect över hello internet hello-Molntjänsten har skapats med reserverade offentliga IP-adress. Du kan övervaka förloppet på hello **jobb** fliken.

    ![Övervaka förloppet](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)
6. Om du ansluter via hello internet, när hello konfigurationsservern är distribuerade Obs hello offentliga IP-adress som tilldelats tooit på hello **virtuella datorer** sida i hello Azure-portalen. På hello **slutpunkter** fliken Obs hello offentlig HTTPS port mappas tooprivate port 443. Du behöver den här informationen senare när du registrerar hello huvudmålservern och servrar med hello konfigurationsservern. hello konfigurationsservern distribueras med dessa slutpunkter:

   * HTTPS: Är en offentlig port används toocoordinate kommunikation mellan komponentservrarna och Azure via hello internet. Privat port 443 finns används toocoordinate kommunikation mellan komponentservrarna och Azure via VPN.
   * Anpassad: En offentlig port används för återställning efter fel för kommunikation över hello internet. Privat port 9443 används för återställning efter fel för kommunikation via VPN.
   * PowerShell: Privat port 5986
   * Fjärrskrivbord: privat port 3389

   ![VM-slutpunkter](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

   > [!WARNING]
   > Inte ta bort eller ändra hello offentligt eller privat portnumret för alla slutpunkter som skapades under distribution av konfigurationsserver.
   >
   >

hello konfigurationsservern distribueras i en automatiskt skapad Azure-molntjänst med en reserverad IP-adress. hello reserverad adress är nödvändiga tooensure som hello configuration server cloud service IP-adressen förblir hello samma mellan omstarter av hello virtuella datorer (inklusive hello konfigurationsservern) på hello-Molntjänsten. hello reserverade offentliga IP-adressen måste toobe manuellt icke reserverade när hello konfigurationsservern är inaktiverat eller det kvar reserverade. Det finns en standard på högst 20 reserverade offentliga IP-adresser per prenumeration. [Lär dig mer](../virtual-network/virtual-networks-reserved-private-ip.md) om reserverade IP-adresser.

### <a name="register-hello-configuration-server-in-hello-vault"></a>Registrera hello konfigurationsservern i hello-valv
1. I hello **Snabbstart** klickar **förbereda Målresurser** > **ladda ned en registreringsnyckel**. hello nyckelfilen genereras automatiskt. Det är giltig i fem dagar efter det genereras. Kopiera den toohello konfigurationsservern.
2. I **virtuella datorer** väljer hello konfigurationsservern från hello lista över virtuella datorer. Öppna hello **instrumentpanelen** och klicka på **Anslut**. **Öppna** hello ned RDP-filen toolog till hello konfigurationsservern med hjälp av fjärrskrivbord. Om du använder VPN använda hello interna IP-adress (hello adressen du angav när du distribuerade hello konfigurationsservern) för anslutning till fjärrskrivbord från hello lokal plats. hello Azure Site Recovery Configuration guiden Konfigurera Server körs automatiskt när du loggar in för hello första gången.

    ![Registrering](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)
3. I **från tredje part Programvaruinstallation** klickar du på **jag accepterar** toodownload och installera MySQL.

    ![MySQL-installation](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)
4. I **MySQL serverinformation** skapa autentiseringsuppgifter toolog till hello MySQL server-instansen.

    ![MySQL-autentiseringsuppgifter](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)
5. I **Internetinställningar** ange hur hello konfigurationsservern ansluter toohello internet. Tänk på följande:

   * Om du vill toouse en anpassad proxyserver bör du konfigurera den innan du installerar hello providern.
   * När du klickar på **nästa** toocheck hello proxyanslutningen körs ett test.
   * Om du använder en anpassad proxy eller din standardproxy kräver autentisering måste tooenter hello proxyinformationen, inklusive hello-adress, port och autentiseringsuppgifter.
   * hello följande URL: er ska vara tillgänglig via hello proxy:
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Om du har IP-adressbaserade brandväggsregler kontrollerar att hello regler är inställda tooallow kommunikation från hello configuration toohello IP-adresser beskrivs i [IP-intervall för Azure-Datacenter](https://msdn.microsoft.com/library/azure/dn175718.aspx) och HTTPS (443)-protokollet. Du skulle ha toowhite listan IP-adressintervall i hello Azure-region som du planerar toouse och som västra USA.

     ![Proxyregistrering](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)
6. I **lokalisering providerinställningar fel meddelande** ange i vilket språk som du vill att fel meddelanden tooappear.

    ![Fel vid registrering för meddelandet](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)
7. I **Azure Site Recovery-registrering** Bläddra och välj hello nyckelfil som du kopierade toohello server.

    ![Nyckelfilen registrering](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)
8. Hello slutförts hello guiden Välj dessa alternativ:

   * Välj **starta konto Management dialogrutan** toospecify som hello hantera konton dialogrutan ska öppna när du slutför guiden hello.
   * Välj **skapa en skrivbordsikon för Cspsconfigtool** tooadd en genväg på skrivbordet på hello konfigurationsservern så att du kan öppna hello **hantera konton** dialogrutan när som helst utan att behöva toorerun hello guiden.

     ![Slutföra registreringen](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)
9. Klicka på **Slutför** toocomplete hello guiden. En lösenfras genereras. Kopiera den tooa säker plats. Du behöver den tooauthenticate och registrera hello processen och huvudmålservern servrar med hello konfigurationsservern. Det har också används tooensure kanal integritet i configuration server-kommunikation. Du kan återskapa hello lösenfras men måste toore-register hello huvudmålservern och servrar med hjälp av hello ny lösenfras.

    ![Lösenfrasen](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Efter registreringen hello konfigurationsservern visas på hello **Konfigurationsservrar** sida i hello-valvet.

### <a name="set-up-and-manage-accounts"></a>Konfigurera och hantera konton
Under distributionen av begär Site Recovery autentiseringsuppgifter för hello följande åtgärder:

* En VMware-konto så thatSite återställning kan automatiskt identifiering av virtuella datorer på vCenter-servrar eller vSphere-värdar.
* När du lägger till datorer för skydd, så att Site Recovery kan installera hello mobilitetstjänsten på dem..

När du har registrerat hello konfigurationsservern kan du öppna hello **hantera konton** dialogrutan tooadd och hantera konton som ska användas för dessa åtgärder. Det finns ett par sätt toodo detta:

* Öppna hello genvägen du valt toocreated för hello dialogrutan på hello sista sidan i installationsprogrammet för konfigurationsservern hello (cspsconfigtool).
* Dialogrutan Öppna hello på Slutför för konfiguration av servern.

1. I **hantera konton** klickar du på **Lägg till konto**. Du kan också ändra och ta bort befintliga konton.

    ![Hantera konton](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)
2. I **kontoinformation** ange ett konto namn toouse i Azure och autentiseringsuppgifter (domän/användarnamn).

    ![Hantera konton](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-toohello-configuration-server"></a>Ansluta toohello konfigurationsservern
Det finns två sätt tooconnect toohello konfigurationsservern:

* Via en VPN-plats-till-plats eller en ExpressRoute-anslutning
* Över hello internet

Tänk på följande:

* En internet-anslutning använder hello slutpunkter av hello virtuella tillsammans med hello offentliga virtuella IP-adressen hello servern.
* En VPN-anslutning använder hello interna serverns IP-adress hello tillsammans med hello endpoint privata portar.
* Det är en gång toodecide om tooconnect (kontroll och replikering av data) från din lokala servrar toohello olika komponentservrarna (konfigurationsservern, huvudmålserver) körs i Azure via en VPN-anslutning eller hello internet. Du kan inte ändra den här inställningen efteråt. Om du vill du ska tooredeploy hello scenario och skyddar dina datorer.  

## <a name="step-3-deploy-hello-master-target-server"></a>Steg 3: Distribuera hello huvudmålservern
1. Klicka på **förbereda Target(Azure) resurser** > **distribuera huvudmålservern**.
2. Ange serverinformation för hello huvudmålservern och autentiseringsuppgifter. hello-servern ska distribueras i hello samma Azure-nätverk som hello konfigurationsservern. När du klickar på toocomplete skapas en virtuell Azure-dator med en avbildning för Windows- eller Linux-galleriet.

    ![Mål-serverinställningar](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Observera att hello fyra första IP-adresser i varje undernät är reserverade för intern användning i Azure. Ange en annan tillgänglig IP-adress.

> [!NOTE]
> Välj Standard DS4 när du konfigurerar skydd för arbetsbelastningar som kräver konsekvent höga i/o-prestanda och låg fördröjning i ordning toohost i/o-intensiv arbetsbelastning med [Premiumlagringskonto](../storage/common/storage-premium-storage.md).
>
>

1. En Windows huvudservern mål VM skapas med de här slutpunkterna. Observera att offentliga slutpunkter skapas endast om din ansluter via hello internet.

   * Anpassad: Offentlig port som används av hello processen server toosend replikeringsdata över hello internet. Privat port 9443 används av hello processen server toosend replikering data toohello huvudmålservern via VPN.
   * Anpassad1: Offentliga porten används av hello processen servermetadata toosend över hello internet. Privat port 9080 används av hello processen server toosend metadata toohello huvudmålservern via VPN.
   * PowerShell: Privat port 5986
   * Fjärrskrivbord: privat port 3389
2. En Linux huvudmålserver VM skapas med de här slutpunkterna. Observera att offentliga slutpunkter skapas bara om du ansluter via hello internet.

   * Anpassad: Offentlig port som används av processen server toosend replikeringsdata över hello internet. Privat port 9443 används av hello processen server toosend replikering data toohello huvudmålservern via VPN.
   * Anpassad1: Offentliga porten används av hello processen servermetadata toosend över hello internet. Privat port 9080 används av hello processen server toosend metadata toohello huvudmålservern via VPN
   * SSH: Privat port 22

     > [!WARNING]
     > Inte ta bort eller ändra hello offentligt eller privat portnummer för någon av hello slutpunkter har skapats under hello huvudmålservern server-distributionen.
     >
     >
3. I **virtuella datorer** vänta hello virtuella toostart.

   * Om det är en Windows server-anteckning ned hello remote desktop information.
   * Om den är en Linux-server och du ansluter via VPN Obs hello interna IP-adress för hello virtuell dator. Om du vill ansluta över hello internet Obs hello offentliga IP-adress.
4. Logga in på hello serverinstallation toocomplete och registrera den med hello konfigurationsservern.
5. Om du kör Windows:

   1. Initiera en anslutning till fjärrskrivbord toohello virtuell dator. hello kan första gången du loggar in ett skript köras i ett PowerShell-fönster. Stäng inte den. När den är klar öppnas automatiskt hello värden Agent konfigurationsverktyg tooregister hello server.
   2. I **värden Agent Config** ange hello interna IP-adress för konfigurationsservern hello och port 443. Du kan använda hello interna adressen och privat port 443 även om du inte ansluter via VPN eftersom hello virtuella datorn är ansluten toohello samma Azure-nätverk som hello konfigurationsservern. Lämna **Använd HTTPS** aktiverat. Ange hello lösenfras för hello konfigurationsservern som du antecknade tidigare. Klicka på **OK** tooregister hello server. Du kan ignorera hello NAT-alternativ. De används inte.
   3. Om din uppskattade kvarhållning enheten krävs mer än 1 TB kan du konfigurera hello kvarhållningsvolymen (R:) med hjälp av en virtuell disk och [lagringsutrymmen](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx)

   ![Huvudmålservern för Windows](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)
6. Om du kör Linux:

   1. Kontrollera att du har installerat hello senaste Linux Integration Services (LIS) installerat innan du installerar hello huvudmålservern. Du kan hitta hello senaste versionen av LIS tillsammans med anvisningar om hur tooinstall [här](https://www.microsoft.com/download/details.aspx?id=46842). Starta om datorn för hello när hello LIS installerat.
   2. I **förbereda (Azure) Målresurser** klickar du på **ladda ned och installera ytterligare programvara (endast för Linux Huvudmålserver)**. Kopiera hello ned tar-filen toohello virtuell dator med en sftp-klient. Du kan också logga in distribuerade linux toohello huvudmålservern och använda *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* toodownload hello hello-filen.
   3. Logga in toohello servern med hjälp av en Secure Shell-klient. Om du är ansluten använda toohello Azure-nätverk via VPN hello interna IP-adress. Använd annars hello extern IP-adress och hello SSH offentlig slutpunkt.
   4. Extrahera hello filer från hello gzippade installer genom att köra: **tar – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
      ![huvudmålservern för Linux](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
   5. Kontrollera att du är i hello directory toowhich du extraherade hello innehållet i hello tar-filen.
   6. Kopiera hello server lösenfras tooa lokala konfigurationsfilen hello kommandot **echo  *`<passphrase>`*  > passphrase.txt**
   7. Kör kommandot hello ”**sudo. / install -t både - en värd -R MasterTarget -d /usr/local/ASR -i  *`<Configuration server internal IP address>`*  -p 443 -s y - c https -P passphrase.txt**”.

      ![Registrera målservern](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)
7. Vänta några minuter (10 – 15) och hello sidan kontrollerar att hello huvudmålservern visas som har registrerats i **servrar** > **Konfigurationsservrar** **-serverinformation** fliken. Om du kör Linux och det gick inte att registrera kör hello värden konfigurationsverktyg igen från /usr/local/ASR/Vx/bin/hostconfigcli. Du behöver tooset behörigheter genom att köra chmod som rot.

    ![Verifiera målserver](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

> [!NOTE]
> Det kan ta upp too15 minuter efter registreringen är klar för hello huvudmålservern server toobe som anges i hello-portalen. tooupdate omedelbart, klickar du på **uppdatera** på hello **Konfigurationsservrar** sidan.
>
>

## <a name="step-4-deploy-hello-on-premises-process-server"></a>Steg 4: Distribuera hello lokalt processervern
Innan du börjar rekommenderar vi att du konfigurerar en statisk IP-adress på hello processervern så att garanteras toobe beständiga tvärs över startas om.

1. Klickar du på Snabbstart > **installera Processervern lokala** > **ladda ned och installera hello processervern**.

    ![Installera processervern](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)
2. Kopiera hello hämtade zip toohello filserver som du kommer tooinstall hello processervern. hello zip-filen innehåller två installationsfiler:

   * Microsoft-ASR_CX_TP_8.4.0.0_Windows*
   * Microsoft-ASR_CX_8.4.0.0_Windows*
3. Packa upp hello arkivet och kopiera hello filer tooa installationsplatsen på hello-servern.
4. Kör hello **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** installationsfilen och följ hello instruktioner. Detta installerar tredjeparts-komponenter som behövs för hello-distribution.
5. Kör sedan **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. På hello **serverläge** markerar **Processervern**.
7. På hello **miljö information** sidan hello följande:

    - Om du vill tooprotect VMware virtuella datorer Klicka **Ja**
    - Om du bara vill tooprotect fysiska servrar och därför inte behöver VMware vCLI installerat på processervern hello. Klicka på **nr** och fortsätta.

1. Observera följande hello när du installerar VMware vCLI:

   * **Stöds bara VMware vSphere CLI 5.5.0**. Hej processervern fungerar inte med andra versioner eller uppdateringar av vSphere CLI.
   * Hämta vSphere CLI 5.5.0 från [här.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
   * Om du installerade vSphere CLI innan du startade installerar hello processervern och det inte att hitta den, vänta in toofive minuter innan du kör installationsprogrammet igen. Detta säkerställer att alla hello miljövariabler behövs för identifiering av vSphere CLI har initierats på rätt sätt.
2. I **NIC val för Processervern** väljer hello nätverkskort hello processen servern ska använda.

   ![Välj adapter](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)
3. I **Server konfigurationsinformation**:

   * För hello IP-adress och port, ange om du ansluter via VPN hello interna IP-adress hello konfigurationsservern och 443 för hello port. Ange annars hello offentliga virtuella IP-adressen och mappade offentlig HTTP-slutpunkt.
   * Skriv i hello lösenfras för hello konfigurationsservern.
   * Rensa **Kontrollera Mobility tjänsten software signatur** om du vill toodisable kontroll när du använder automatisk push tooinstall hello tjänst. Signaturverifiering måste internet-anslutning från hello processervern.
   * Klicka på **Nästa**.

   ![Registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)
4. I **Välj installationsenheten** välja en cacheenhet. Hej processervern måste en cacheenhet med minst 600 GB ledigt utrymme. Klicka på **Installera**.

   ![Registrera konfigurationsservern](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)
5. Observera att du kan behöva toorestart hello server toocomplete hello-installation. I **konfigurationsservern** > **serverinformation** Kontrollera hello processen servern visas och har registrerats i hello-valvet.

> [!NOTE]
> Det kan ta upp too15 minuter efter registreringen är klar för hello processen server tooappear som anges under hello konfigurationsservern. tooupdate omedelbart, uppdatera hello konfigurationsservern genom att klicka på hello uppdateringsknappen längst hello hello configuration server-sida
>
>

![Validera processervern](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Om du inte inaktiverar du signaturverifiering hello mobilitetstjänsten när du registrerade hello processervern göra du det senare på följande sätt:

1. Logga in på hello processervern som administratör och öppna filen hello C:\pushinstallsvc\pushinstaller.conf för redigering. Under avsnittet hello **[PushInstaller.transport]** Lägg till följande rad: **SignatureVerificationChecks = ”0”**. Spara och Stäng hello-filen.
2. Starta om hello InMage PushInstall service.

## <a name="step-5-update-site-recovery-components"></a>Steg 5: Uppdatera Site Recovery-komponenter
Site Recovery-komponenterna har uppdaterats från tid tootime. När nya uppdateringar är tillgängliga bör du installera dem i hello följande ordning:

1. Konfigurationsservern
2. Processervern
3. Huvudmålservern
4. Återställning av verktyget (vContinuum)

### <a name="obtain-and-install-hello-updates"></a>Hämta och installera hello uppdateringar
1. Du kan hämta uppdateringar för hello konfiguration, process- och huvudmålservern servrar från hello Site Recovery **instrumentpanelen**. Extrahera hello filer från hello gzippade installer för Linux-installationen och kör hello kommando ”sudo. / install” tooinstall hello uppdateringen.
2. [Hämta](http://go.microsoft.com/fwlink/?LinkID=533813) hello senaste uppdateringen för hello tool(vContinuum) för återställning efter fel.
3. Om du använder virtuella datorer eller fysiska servrar som redan har hello mobilitetstjänsten installeras, kan du hämta uppdateringar för hello-tjänsten på följande sätt:

   * **Alternativ 1**: ladda ned uppdateringar:
     * [Windows Server (endast 64-bitars)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
     * [CentOS 6.4,6.5,6.6 (endast 64-bitars)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
     * [Oracle Enterprise Linux 6.4,6.5 (endast 64-bitars)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
     * [SUSE Linux Enterprise Server SP3 (endast 64-bitars)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
     * När du har uppdaterat hello processen serverns hälsning blir uppdaterad version av hello mobilitetstjänsten tillgängligt i hello C:\pushinstallsvc\repository mapp på hello processervern.
   * **Alternativ 2**: Om du har en dator med en äldre version av hello mobilitetstjänsten installerat kan automatiskt uppgradera hello mobilitetstjänsten på hello datorn från hello-hanteringsportalen.

     1. Kontrollera att processervern hello uppdateras.
     2. Kontrollera att den skydda datorn hello uppfyller hello [krav](#install-the-mobility-service-automatically) för överföring hello mobilitetstjänsten automatiskt så att hello uppdatering fungerar som förväntat.
     3. Välj hello skyddsgrupp, markera hello skyddade datorn och klicka på **uppdatering mobilitetstjänsten**. Den här knappen är endast tillgänglig om det finns en nyare version av hello mobilitetstjänsten.

         ![Välj vCenter-servern](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Ange Hej administratör konto toobe används tooupdate hello mobilitetstjänsten på hello skyddade servern i Select-konton. Klicka på OK och vänta hello utlöses jobbet toocomplete.

## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Steg 6: Lägga till vCenter-servrar eller vSphere-värdar
1. Klicka på **servrar** > **Konfigurationsservrar** > konfigurationsservern >**lägga till vCenter-servern** tooadd en vCenter server eller vSphere-värd.

    ![Välj vCenter-servern](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)
2. Ange information för hello server eller värden och välj hello processen server som kommer att använda toodiscover den.

   * Ange hello portnummer som hello vCenter-servern körs om hello vCenter-servern inte körs på hello standardporten 443.
   * Hej processervern måste finnas på hello samma nätverk som hello vCenter server/vSphere-värd och ska ha VMware vSphere CLI 5.5.0 installerad.

     ![inställningar för vCenter-server](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)
3. När identifieringen har slutförts visas hello vCenter-servern under hello konfigurationsinformation för servern.

    ![inställningar för vCenter-server](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)
4. Om du använder en icke-administratörskonto tooadd hello server eller värd, kontrollera hello-konto har hello följande behörigheter:

   * vCenter-konton ska ha Datacenter, datalagret, mapp, värden, nätverk, resurs, lagring vyer, virtuell dator och vSphere distribuerade växeln behörigheterna.
   * vSphere-värd konton bör ha hello Datacenter, datalagret, mapp, värden, nätverk, resurs, virtuell dator och vSphere distribuerade växeln behörigheterna

## <a name="step-7-create-a-protection-group"></a>Steg 7: Skapa en skyddsgrupp
1. Öppna **skyddade objekt** > **Skyddsgrupp** > **skapa skyddsgrupp**.

    ![Skapa en skyddsgrupp](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)
2. På hello **ange inställningarna för Skyddsgruppen** ange ett namn för hello gruppen och välj hello konfigurationsservern som du vill använda toocreate hello grupp.

    ![Inställningarna för skyddsgruppen](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)
3. På hello **ange replikeringsinställningarna** sidan Konfigurera hello replikeringsinställningarna som ska användas för alla hello-datorer i hello grupp.

    ![Skydd grupp replikering](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)
4. Inställningar för:

   * **Multi VM konsekvenskontroll**: Om du aktiverar detta skapar den delade programkonsekventa återställningspunkter över hello datorer i hello skyddsgrupp. Den här inställningen är mycket användbar när alla hello datorer i skyddsgruppen hello kör hello samma arbetsbelastning. Alla datorer kommer att återställda toohello samma datapunkt. Endast tillgängligt för Windows-servrar.
   * **Tröskelvärde för Återställningspunktmål**: aviseringar genereras när hello kontinuerlig dataskyddsreplikering överskrider tröskelvärdet hello konfigurerats.
   * **Kvarhållningstid för återställningspunkten**: Anger hello kvarhållningsperiod. Skyddade datorer kan återställas tooany punkt inom det här fönstret.
   * **Programkonsekventa ögonblicksbilder frekvens**: Anger hur ofta återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.

Du kan övervaka hello skyddsgruppen som de skapas på hello **skyddade objekt** sidan.

## <a name="step-8-set-up-machines-you-want-tooprotect"></a>Steg 8: Konfigurera datorer vill du tooprotect
Du behöver tooinstall hello mobilitetstjänsten på virtuella datorer och fysiska servrar som du vill tooprotect. Du kan göra detta på två sätt:

* Push- och installera hello-tjänsten på varje dator från hello processervern automatiskt.
* Installera hello-tjänsten manuellt.

### <a name="install-hello-mobility-service-automatically"></a>Installera hello mobilitetstjänsten automatiskt
När du lägger till pushas datorer tooa skydd grupp hello mobilitetstjänsten automatiskt och installeras på varje dator med hello processervern.

**Automatiskt installera push hello mobilitetstjänsten på Windows-servrar:**

1. Installera hello senaste uppdateringarna för hello processervern enligt beskrivningen i [steg 5: installera senaste uppdateringarna](#step-5-install-latest-updates), och se till att processervern hello är tillgänglig.
2. Se till att det finns en nätverksanslutning mellan källdatorn hello och hello processervern och den hello källdatorn kan nås från processervern hello.  
3. Konfigurera hello Windows-brandväggen tooallow **File and Printer Sharing** och **Windows Management Instrumentation**. Välj hello alternativet ”Tillåt en app eller en funktion via brandväggen” under inställningar för Windows-brandväggen och välj hello program som hello bilden nedan. För datorer som tillhör tooa domän som du kan konfigurera hello brandväggsprincipen med ett grupprincipobjekt.

    ![Brandväggsinställningar](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)
4. hello konto som används för tooperform hello push-installation måste vara i hello administratörsgruppen på hello datorn du vill tooprotect. Dessa autentiseringsuppgifter används bara för push-installation av hello mobilitetstjänsten och du måste ange dem när du lägger till en dator tooa skyddsgrupp.
5. Om kontot är inte ett domänkonto hello måste toodisable fjärranslutna användare åtkomstkontroll på hello lokal dator. toodo denna Lägg till hello LocalAccountTokenFilterPolicy DWORD registerpost med värdet 1 under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. tooadd hello registerposten från en CLI öppna cmd eller powershell och ange  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .

**Automatiskt installera push hello mobilitetstjänsten på Linux-servrar:**

1. Installera hello senaste uppdateringarna för hello processervern enligt beskrivningen i [steg 5: installera senaste uppdateringarna](#step-5-install-latest-updates), och se till att processervern hello är tillgänglig.
2. Se till att det finns en nätverksanslutning mellan källdatorn hello och hello processervern och den hello källdatorn kan nås från processervern hello.  
3. Kontrollera att hello kontot är en rotanvändare på Linux-hello källservern.
4. Se till att hello/etc/hosts-filen på hello källa Linux-servern innehåller poster som mappar hello lokal värd namn tooIP adresser som är associerade med alla nätverkskort.
5. Installera hello senaste openssh, openssh-server, openssl paket på hello datorn du vill tooprotect.
6. Kontrollera att SSH är aktiverat och körs på port 22.
7. Aktivera autentisering med SFTP undersystemet och lösenord i hello sshd_config fil på följande sätt:

   * en) logga in som rot.
   * b) i hello filen/etc/ssh/sshd_config filen, hitta hello rad som börjar med **PasswordAuthentication**.
   * c) ta bort kommentarerna hello rad och ändra hello från ”Nej” för ”Ja”.

       ![Linux-mobility](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)
   * d) hitta hello rad som börjar med delsystemet och Avkommentera hello rad.

       ![Linux push mobility](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    
8. Kontrollera hello källa datorn Linux variant stöds.

### <a name="install-hello-mobility-service-manually"></a>Installera hello mobilitetstjänsten manuellt
hello programvarupaket används tooinstall hello Mobility tjänsten finns på hello processervern i C:\pushinstallsvc\repository. Logga in på hello processervern och kopiera hello lämplig installation paketet toohello källdatorn baserat på hello tabellen nedan:-

| Källoperativsystem | Mobility servicepaket på processervern |
| --- | --- |
| Windows Server (endast 64-bitars) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe` |
| CentOS 6.4, 6.5, 6.6 (endast 64 bitars) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz` |
| SUSE Linux Enterprise Server 11 SP3 (endast 64 bitars) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz` |
| Oracle Enterprise Linux 6.4, 6.5 (endast 64 bitars) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz` |

**tooinstall hello mobilitetstjänsten manuellt på en Windows server**, hello följande:

1. Kopiera hello **Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** paket från hello processen serversökväg som anges i hello tabellen ovan toohello källdatorn.
2. Installera hello mobilitetstjänsten genom att köra hello körbara på hello källdatorn.
3. Följ instruktionerna för hello installer.
4. Välj **mobilitetstjänsten** som hello-rollen och klicka på **nästa**.

    ![Installera mobilitetstjänsten](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)
5. Lämna hello installationskatalogen som hello standardsökvägen för installationen och klicka på **installera**.
6. I **värden Agent Config** anger hello IP-adress och HTTPS-port för hello konfigurationsservern.

   * Om du ansluter via internet ange hello hello offentliga virtuella IP-adressen och offentlig HTTPS-slutpunkt som hello port.
   * Om du ansluter via VPN ange hello interna IP-adress och 443 för hello port. Lämna **Använd HTTPS** markerad.

     ![Installera mobilitetstjänsten](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)
7. Ange hello konfigurationsserverns lösenfras och klicka på **OK** tooregister hello mobilitetstjänsten med hello konfigurationsservern.

**toorun från hello kommandorad:**

1. Kopiera hello lösenfras från hello CX toohello filen ”C:\connection.passphrase” på hello-servern och kör det här kommandot. I vårt exempel CX i 104.40.75.37 och hello HTTPS-porten är 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Installera hello mobilitetstjänsten manuellt på en Linux-server**:

1. Kopiera hello lämpliga tar-Arkiv baserat på hello tabellen ovan, från hello processen server toohello källdatorn.
2. Öppna ett shell-program och extrahera hello komprimerade tar-Arkiv tooa lokal sökväg genom att köra`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Skapa en passphrase.txt fil hello lokal katalog toowhich du extraherade hello innehållet i hello tar arkivet genom att ange  *`echo <passphrase> >passphrase.txt`*  från shell.
4. Installera hello mobilitetstjänsten genom att ange  *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`* .
5. Ange hello IP-adress och port:

   * Om du ansluter toohello konfigurationsservern över internet anger hello configuration serverns virtuella offentliga IP-adress och offentlig HTTPS-slutpunkt i hello `<IP address>` och `<port>`.
   * Om du ansluter via en VPN-anslutning ange hello interna IP-adress och 443.

**toorun från kommandoraden hello**:

1. Kopiera hello lösenfras från hello CX toohello filen ”passphrase.txt” på servern hello och kör det här kommandot. I vårt exempel CX i 104.40.75.37 och hello HTTPS-porten är 62519:

tooinstall på en produktionsserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

tooinstall på målservern för hello:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

> [!NOTE]
> När du lägger till datorer tooa skyddsgruppen som redan kör en lämplig version av hello mobilitetstjänsten sedan push-installation av hello hoppas över.
>
>

## <a name="step-9-enable-protection"></a>Steg 9: Aktivera skydd
tooenable skydd som du lägger till virtuella datorer och fysiska servrar tooa skyddsgruppen. Observera att innan du börjar:

* Virtuella datorer identifieras var 15: e minut och det kan ta upp too15 minuter för dem tooappear i Azure Site Recovery efter identifiering.
* Ändringar i miljön på hello virtuella datorn (till exempel VMware verktyg installation) kan också ta too15 minuter toobe uppdateras i Site Recovery.
* Du kan kontrollera hello senast identifierade tid i hello **senaste kontakt på** för hello vCenter server/ESXi-värd på hello **Konfigurationsservrar** sidan.
* Om du har en skyddsgrupp som redan har skapats och lägga till en vCenter Server eller ESXi-värd som det tar femton minuter för hello Azure Site Recovery-portalen toorefresh och virtuella datorer toobe som anges i hello **Lägg till datorer tooa skyddsgrupp**  dialogrutan.
* Om du vill att tooproceed omedelbart med att lägga till datorer tooprotection grupp utan att vänta på hello schemalagda identifiering, markera hello konfigurationsservern (inte på den) och klicka på hello **uppdatera** knappen.
* När du lägger till virtuella datorer eller skyddsgrupper för fysiska datorer tooa hello processervern push-meddelanden och automatiskt installerar hello mobilitetstjänsten på hello källservern om hello den inte redan är installerad.
* Toowork Kontrollera att du har konfigurerat dina skyddade datorer enligt beskrivningen i föregående steg i hello för hello automatisk push-funktion.

Lägg till datorer på följande sätt:

1. Klicka på **skyddade objekt** > **Skyddsgrupp** > **datorer** > **Lägg till datorer** . Som bästa praxis rekommenderar vi att skyddsgrupper bör spegla dina arbetsbelastningar så att du lägger till datorer som kör ett visst program toohello samma grupp.
2. I **Välj virtuella datorer** om du skyddar fysiska servrar i hello **lägga till fysiska datorer** guiden ange hello IP-adress och eget namn. Välj sedan hello operativsystemsfamiljen.

    ![Lägg till V Center-servern](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)
3. I **Välj virtuella datorer** om du skyddar virtuella VMware-datorer, Välj en vCenter-server som hanterar virtuella datorer (eller hello EXSi värden som de använder) och välj sedan hello datorer.

    ![Lägg till V Center-servern](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png)    
4. I **ange Målresurser** Välj hello huvudmålservern servrar och lagring toouse för replikering och välj om hello inställningar ska användas för alla arbetsbelastningar. Välj [Premiumlagringskonto](../storage/common/storage-premium-storage.md) vid konfiguration av skydd för arbetsbelastningar som kräver konsekvent höga i/o-prestanda och låg fördröjning i ordning toohost-i/o-intensiv arbetsbelastning. Om du vill toouse ett Premium Storage-konto för din arbetsbelastningsdiskar måste toouse hello Huvudmålet för DS-serien. Du kan inte använda Premium-lagring diskar med Huvudmålet för DS serier.

   > [!NOTE]
   > Vi stöder inte hello flytt av lagringskonton som skapats med hjälp av hello [nya Azure-portalen](../storage/common/storage-create-storage-account.md) över resursgrupper.
   >
   >

    ![vCenter-servern](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)
5. I **ange konton** Välj hello-konto som du vill toouse för att installera mobilitetstjänsten hello på skyddade datorer. hello autentiseringsuppgifter krävs för automatisk installation av hello mobilitetstjänsten. Om du inte kan välja ett konto kontrollerar du att konfigurerar du enligt beskrivningen i steg 2. Observera att det här kontot inte kan nås av Azure. För Windows bör hello serverkontot ha administratörsbehörighet på hello källservern. För Linux måste hello-kontot vara rot.

    ![Linux-autentiseringsuppgifter](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)
6. Klicka på hello markerat toofinish att lägga till datorer toohello skydd grupp och toostart inledande replikering för varje dator. Du kan övervaka status för hello **jobb** sidan.

    ![Lägg till V Center-servern](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)
7. Dessutom kan du övervaka skyddsstatus genom att klicka på **skyddade objekt** > skyddsgruppnamn > **virtuella datorer** . När den inledande replikeringen är klar och hello datorer synkroniserar data visas de **skyddade** status.

    ![Virtual machine-jobb](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)

### <a name="set-protected-machine-properties"></a>Ange skyddad dator egenskaper
1. När en dator har en **skyddade** status som du kan konfigurera egenskaperna för växling vid fel. I hello skydd gruppinformation Markera hello maskin- och öppna hello **konfigurera** fliken.
2. Du kan ändra hello-namn som kommer att få toohello datorn i Azure efter växling vid fel och hello Azure virtuella datorstorleken. Du kan också välja hello Azure-nätverk toowhich hello datorn ansluts efter växling vid fel.

   > [!NOTE]
   > [Migrering av nätverk](../resource-group-move-resources.md) över resurs grupper i hello samma prenumeration eller alla prenumerationer stöds inte för nätverk som används för att distribuera Site Recovery.
   >
   >

    ![Ange egenskaper för virtuell dator](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Tänk på följande:

* hello namnet på hello Azure datorn måste uppfylla krav för Azure.
* Som standard är den replikerade virtuella datorer i Azure inte anslutna tooan Azure-nätverk. Om du vill att den replikerade virtuella datorer toocommunicate Kontrollera hello tooset samma Azure-nätverk för dem.
* Om du ändrar storlek på en volym på en virtuell VMware-dator eller fysisk server som den försätts i ett kritiskt tillstånd. Om du behöver toomodify hello storlek hello följande:

  * en) ändra inställningen för hello.
  * b) i hello **virtuella datorer** väljer hello virtuella datorn och klicka på **ta bort**.
  * c) i **ta bort virtuell dator** alternativet hello **inaktivera skydd (Använd för återställningsgranskning och volymstorlek)**. Det här alternativet inaktiverar skydd, men behåller hello Återställningspunkter i Azure.

      ![Ange egenskaper för virtuell dator](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)
  * d) återaktivera skydd för hello virtuell dator. När du återaktivera skydd hello data för hello storlek volymen blir överförda tooAzure.

## <a name="step-10-run-a-failover"></a>Steg 10: Kör en växling vid fel
Du kan för närvarande bara köra oplanerad växling vid fel för skyddade virtuella VMware-datorer och fysiska servrar. Observera följande hello:

* Innan du startar en växling vid fel, se till att hello-konfiguration och master målservrar körs och är felfria. Annars misslyckas växlingen.
* Källdatorer stänga inte av som en del av en oplanerad redundans. Utföra en oplanerad redundansväxling stoppas datareplikeringen för hello skyddade servrar. Du behöver toodelete hello datorer från hello skyddsgrupp och lägga till dem igen i ordning toostart skydda datorer igen när hello oplanerad växling är klar.
* Om du vill ha toofail över utan att förlora data, kontrollerar du att hello primär plats virtuella datorer är avstängda innan du påbörjar redundans för hello.

1. På hello **Återställningsplaner** och lägger till en återställningsplan. Ange information för hello plan och välj **Azure** som hello mål.

    ![Konfigurera återställningsplan](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)
2. I **Välj virtuell dator** väljer du en skyddsgrupp och välj sedan datorer i återställningsplanen för hello grupp tooadd toohello. [Läs mer](site-recovery-create-recovery-plans.md) om återställningsplaner.

    ![Lägg till virtuella datorer](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)
3. Om det behövs kan du anpassa hello plan toocreate grupper och sekvens hello ordning i vilken datorer i hello återställning plan har redundansväxlats. Du kan också lägga till efterfrågas manuella åtgärder och skript. Hej skript när du återställer tooAzure kan läggas till med hjälp av [Azure Automation-Runbooks](site-recovery-runbook-automation.md).
4. I hello **Återställningsplaner** sidan Välj hello planen och klickar på **oplanerad växling**.
5. I **bekräfta redundans** Kontrollera hello redundansriktning (tooAzure) och välj hello recovery punkt toofail över att.
6. Vänta tills hello redundans jobbet toocomplete och kontrollera att hello redundans fungerade som förväntat och att hello replikeras start för virtuella datorer har i Azure.

## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Steg 11: Växla tillbaka misslyckades över datorer från Azure
[Lär dig mer](site-recovery-failback-azure-to-vmware-classic-legacy.md) hur toobring din misslyckade över datorer som körs i Azure tillbaka tooyour lokala miljö.

## <a name="manage-your-process-servers"></a>Hantera dina servrar
Hej processervern skickar replikering data toohello huvudmålservern i Azure och identifierar nya VMware virtuella datorer tillagda tooa vCenter-servern. Du kanske vill toochange hello processen server i distributionen i hello följande omständigheter:

* Om hello aktuella processervern kraschar
* Om din återställningspunkt stiger återställningspunktmål tooan acceptabel nivå för din organisation.

Om det behövs kan du flytta hello replikering av vissa eller alla dina lokala VMwares virtuella bearbeta datorer och fysiska servrar tooa olika server. Exempel:

* **Fel**– om en processerver misslyckas eller inte är tillgänglig kan du flytta skyddad dator replikering tooanother processervern. Metadata för hello källdatorn och replikdatorn blir flyttade toohello nya processervern och data synkroniseras. hello nya processervern ansluter automatiskt toohello vCenter server tooperform automatisk identifiering. Du kan övervaka hello tillståndet för servrar på hello Site Recovery-instrumentpanelen.
* **Belastningsutjämning tooadjust Återställningspunktmål**– för förbättrad belastningsutjämning du kan välja en annan processerver i hello Site Recovery-portalen och flyttar du replikeringen av en eller flera datorer tooit för manuell belastningsutjämning. Metadata för hello valda käll- och replikdatorerna är i det här fallet har flyttats toohello nya processervern. hello ursprungliga processervern förblir anslutna toohello vCenter-servern.

### <a name="monitor-hello-process-server"></a>Övervakaren hello processervern
Om en processervern är i ett kritiskt tillstånd visas en varning om status på hello Site Recovery-instrumentpanelen. Du kan klicka på fliken hello status tooopen hello händelser och sedan detaljnivån toospecific jobb på fliken för hello-jobb.

### <a name="modify-hello-process-server-used-for-replication"></a>Ändra hello processervern som används för replikering
1. Öppna **servrar** > **Konfigurationsservrar** > konfigurationsservern > **serverinformation**.
2. Klicka på **servrar** > **ändra Processerver** nästa toohello-server som du vill toomodify.

    ![Ändra Processerver 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)
3. I **ändra Processerver** > **Målprocesserver** Välj hello ny server som du vill toouse och välj sedan hello virtuella datorer som du vill tooreplicate toohello ny server. Klicka på hello information ikonen nästa toohello servernamnet för information om ledigt och använt minne. hello genomsnittlig diskutrymme som krävs tooreplicate varje valda virtuella datorn toohello nya processervern är visas toohelp som du gör att läsa in beslut.

    ![Ändra Processerver 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)
4. Klicka på hello markerat toobegin replikerar toohello nya processervern. Observera att om du tar bort alla virtuella datorer från en processerver som var kritiska bör inte längre visas en kritisk varning i hello instrumentpanelen.

## <a name="third-party-software-notices-and-information"></a>Meddelanden om programvara från tredje part och information
Inte översätta eller lokalisera

hello programvara och inbyggd programvara som körs i hello Microsoft-produkt eller tjänst som baseras på eller innehåller material från hello projekt nedan (gemensamt kallade ”kod från tredje part”).  Microsoft är hello inte ursprungliga författaren av hello kod från tredje part.  hello ursprungliga upphovsrättsmeddelandet och licensen, enligt vilka Microsoft fått sådan kod från tredje part, är anges nedan.

hello informationen i avsnittet A gäller kod från tredje part-komponenter från hello projekt som anges nedan. Sådana licenser och information tillhandahålls endast i informationssyfte.  Den här koden från tredje part är att relicensed tooyou av Microsoft under Microsofts licensvillkoren för hello Microsoft-produkt eller tjänst.  

hello informationen i avsnittet B gäller kod från tredje part-komponenter som blir tillgängliga tooyou av Microsoft under hello ursprungliga licensvillkoren.

hello hela filen finns på hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft förbehåller sig alla rättigheter som inte uttryckligen beviljas, vare sig indirekt, via estoppel eller på annat sätt.
