---
title: "aaaTest resultat för Hyper-V-replikering mellan platser med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln innehåller information om prestandatestning för lokala tooon lokal replikering av Hyper-V virtuella datorer med hjälp av Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 96ff404f-0d88-43fa-a00b-2dffde93d192
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: raynew
ms.openlocfilehash: 3b37542fc88e0af05e05cee78183983667618816
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test-results-for-on-premises-tooon-premises-hyper-v-replication-with-site-recovery"></a>Testresultat för lokala tooon lokala Hyper-V-replikering med Site Recovery

Du kan använda Microsoft Azure Site Recovery tooorchestrate och hantera replikering av virtuella datorer och fysiska servrar tooAzure eller tooa sekundärt datacenter. Den här artikeln innehåller hello resultaten av prestandatester vi gjorde när replikering av Hyper-V virtuella datorer mellan två lokala datacenter.

## <a name="test-goals"></a>Testa mål

hello målet tester var tooexamine hur Azure Site Recovery utför under stabilt tillstånd. Stabilt tillstånd replikeringen sker när virtuella datorer har slutfört den inledande replikeringen och synkroniserar deltaändringar. Det är viktigt toomeasure prestanda med hjälp av stabilt tillstånd eftersom den är hello tillstånd där de flesta virtuella datorer förblir om oväntade avbrott inträffar.

Hej testdistributionen består av två lokala webbplatser med en VMM-server på varje plats. Den här testdistributionen är typiska för en head office/filialdistribution, med huvudkontoret som fungerar som hello primär plats och hello lokalkontor som hello sekundär eller återställning plats.

## <a name="what-we-did"></a>Vi gjorde

Här är vad vi i hello test klarade:

1. Skapa virtuella datorer med hjälp av VMM-mallar.
2. Starta virtuella datorer och avbilda baslinjen resultatmått än 12 timmar.
3. Skapade moln på den primära servern och återställa VMM-servrar.
4. Konfigurerade molnskydd i Azure Site Recovery, inklusive mappning av käll- och moln.
5. Aktivera skydd för virtuella datorer och ge dem toocomplete inledande replikering.
6. Väntade några timmar innan systemet stabilisering.
7. Övervakad prestandamått än 12 timmar att säkerställa att alla virtuella datorer befann sig i ett replikeringstillstånd som krävs för dessa 12 timmar.
8. Mät hello delta mellan hello baslinjen prestandamått och hello prestandamått för replikering.


## <a name="primary-server-performance"></a>Primär server-prestanda

* Hyper-V-replikering spårar asynkront ändringar tooa loggfil med minsta mo på hello primära servern.
* Hyper-V-replikering använder automatisk behålla cache toominimize IOPS minnesanvändningen för spårning. Den lagrar skrivningar toohello VHDX i minnet och rensningar dem till hello loggfilen innan hello tid hello loggen skickas toohello återställningsplatsen. En disk som tömning sker också om hello skrivningar nått gränsen för ett förutbestämt.
* hello diagrammet nedan visar hello stabilt tillstånd IOPS kostnader för replikering. Vi kan se att hello IOPS försämras på grund av tooreplication är cirka 5% som är mycket liten.

![Primär resultat](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

Hyper-V-replikering använder minne på hello primärservern toooptimize diskprestanda. Minnet försämras på alla servrar i hello primära klustret är marginell i hello följande diagram visas. hello memory omkostnader är hello procentandelen minne som används av replikeringen jämfört med toohello totala installerat minne på hello Hyper-V-servern.

![Primär resultat](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

Hyper-V-replikering har minimal CPU-belastning. Som visas i diagrammet hello är replikering hello intervallet för % 2-3.

![Primär resultat](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

## <a name="secondary-recovery-server-performance"></a>Sekundär (återställning) serverprestanda

Hyper-V-replikering använder en liten mängd minne på hello recovery server toooptimize hello antalet lagringsåtgärder. hello diagrammet sammanfattar hello minnesanvändningen för hello återställningsservern. hello memory omkostnader är hello procentandelen minne som används av replikeringen jämfört med toohello totala installerat minne på hello Hyper-V-servern.

![Sekundär resultat](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

hello mycket i/o-åtgärder på hello återställningsplatsen är en funktion av hello antalet skrivåtgärder på hello primära platsen. Nu ska vi titta på hello total i/o-åtgärder på hello återställningsplatsen jämfört med hello total i/o-åtgärder och skrivåtgärder i hello primär plats. hello diagrammen visar hello summan IOPs på hello återställningsplatsen

* Cirka 1,5 gånger hello skriva IOPS på hello primära.
* 37% av hello total runt IOPS på hello primära platsen.

![Sekundär resultat](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Sekundär resultat](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

## <a name="effect-on-network-utilization"></a>Effekt på belastningen på nätverket

Ett genomsnitt av 275 Mb per sekund av nätverkets bandbredd användes mellan hello primära och återställning noder (med aktiverad komprimering) mot en befintlig bandbredd på 5 Gb per sekund.

![Resultaten nätverksanvändning](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

## <a name="effect-on-vm-performance"></a>Inverkan på prestandan för VM

Ett viktigt övervägande är hello effekten av replikering för produktionsarbetsbelastningar som körs på hello virtuella datorer. Om hello primär plats etableras på lämpligt sätt för replikering, får inte det finnas någon effekt på hello arbetsbelastningar. Hyper-V-replikering lightweight spårning mekanism säkerställer att arbetsbelastningar som körs i hello virtuella datorer inte påverkas under stabilt tillstånd. Detta illustreras i följande diagram hello.

Det här diagrammet visar IOPS som utförs av virtuella datorer som kör olika arbetsbelastningar före och efter att replikering har aktiverats. Du kan se att det finns ingen skillnad mellan hello två.

![Replik effekt resultat](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

hello visar följande diagram hello genomströmning av virtuella datorer som kör olika arbetsbelastningar före och efter att replikering har aktiverats. Du kan se att replikeringen har ingen inverkan.

![Resultaten replik effekter](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

## <a name="conclusion"></a>Slutsats

hello resultatet visar tydligt att Azure Site Recovery, tillsammans med Hyper-V-replikering ska skalas med minsta omkostnader för ett kluster för stora.  Azure Site Recovery tillhandahåller enkel distribution, replikering, hantering och övervakning. Hyper-V-replikering innehåller hello infrastruktur som behövs för replikering som lyckades skalning. För att planera en optimal distribution vi rekommenderar att du hämtar hello [Hyper-V Replica Capacity Planner](https://www.microsoft.com/download/details.aspx?id=39057).

## <a name="test-environment-details"></a>Testa miljön information

### <a name="primary-site"></a>Primär plats

* hello primär plats har ett kluster som innehåller fem Hyper-V-servrar som kör 470 virtuella datorer.
* hello virtuella datorer som kör olika arbetsbelastningar och alla ha Azure Site Recovery-skydd är aktiverat.
* Lagring för hello klusternod som en iSCSI SAN-nätverk. Modellen – Hitachi HUS130.
* Varje klusterserver har fyra nätverkskort (NIC) av en Gbps.
* Två av hello nätverkskort är anslutet tooan iSCSI privata nätverk och två är anslutna tooan externa företagsnätverk. En av hello externa nätverk är reserverad för endast klusterkommunikation.

![Primär maskinvarukrav](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

| Server | RAM | Modellen | Processor | Antal processorer | NÄTVERKSKORT | Programvara |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V-servrar i klustret: <br />ESTLAB HOST11<br />ESTLAB HOST12<br />ESTLAB HOST13<br />ESTLAB HOST14<br />ESTLAB HOST25 |128ESTLAB HOST25 har 256 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz |4 |Jag Gbit/s x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rollen |
| VMM-servern |2 | | |2 |1 Gbit/s |Windows Server-databasen 2012 R2 (x 64) + VMM 2012 R2 |

### <a name="secondary-recovery-site"></a>Platsen för sekundära (återställning)

* hello sekundär plats har ett kluster med sex noder.
* Lagring för hello klusternod som en iSCSI SAN-nätverk. Modellen – Hitachi HUS130.

![Primär maskinvarukraven](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

| Server | RAM | Modellen | Processor | Antal processorer | NÄTVERKSKORT | Programvara |
| --- | --- | --- | --- | --- | --- | --- |
| Hyper-V-servrar i klustret: <br />ESTLAB HOST07<br />ESTLAB HOST08<br />ESTLAB HOST09<br />ESTLAB HOST10 |96 |Dell™ PowerEdge™ R720 |Intel(R) Xeon(R) CPU E5-2630 0 @ 2,30 GHz |2 |Jag Gbit/s x 4 |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rollen |
| ESTLAB HOST17 |128 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz |4 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rollen |
| ESTLAB HOST24 |256 |Dell™ PowerEdge™ R820 |Intel(R) Xeon(R) CPU E5-4620 0 @ 2,20 GHz |2 | |Windows Server Datacenter 2012 R2 (x64) + Hyper-V-rollen |
| VMM-servern |2 | | |2 |1 Gbit/s |Windows Server-databasen 2012 R2 (x 64) + VMM 2012 R2 |

### <a name="server-workloads"></a>Server-arbetsbelastningar

* För testning plockats vi arbetsbelastningar som ofta används i kunden företagsscenarier.
* Vi använder [IOMeter](http://www.iometer.org) med hello arbetsbelastning egenskap sammanfattas i hello tabell för att simulera.
* Alla IOMeter profiler ställs toowrite slumpmässiga byte toosimulate sämsta skrivåtgärder mönster för arbetsbelastningar.

| Arbetsbelastning | I/o-storlek (KB) | % Åtkomst | % Läs | Utestående I/o | I/o-mönster |
| --- | --- | --- | --- | --- | --- |
| Filserver |48163264 |60%20%5%5%10% |80%80%80%80%80% |88888 |Alla 100% slumpmässiga |
| SQL Server (volym 1) SQL Server (volume 2) |864 |100%100% |70%0% |88 |100% random100% sekventiella |
| Exchange |32 |100% |67% |8 |100% slumpmässiga |
| Arbetsstation/VDI |464 |66%34% |70%95% |11 |Både slumpmässiga 100% |
| Webbserver-fil |4864 |33%34%33% |95%95%95% |888 |Alla 75% slumpmässiga |

### <a name="vm-configuration"></a>VM-konfiguration

* 470 virtuella datorer på hello primära klustret.
* Alla virtuella datorer med VHDX-disken.
* Virtuella datorer som kör arbetsbelastningar som sammanfattas i hello tabell. Alla har skapats med VMM-mallar.

| Arbetsbelastning | # Virtuella datorer | Minsta RAM-minne (GB) | Maximalt RAM-minne (GB) | Storleken för logisk disk (GB) per VM | Högsta IOPS |
| --- | --- | --- | --- | --- | --- |
| SQL Server |51 |1 |4 |167 |10 |
| Exchange Server |71 |1 |4 |552 |10 |
| Filserver |50 |1 |2 |552 |22 |
| VDI |149 |.5 |1 |80 |6 |
| Webbserver |149 |.5 |1 |80 |6 |
| TOTALT |470 | | |96.83 TB |4108 |

### <a name="site-recovery-settings"></a>Site Recovery-inställningarna

* Azure Site Recovery har konfigurerats för lokala tooon mellan lokala platser
* hello VMM-servern har fyra moln som har konfigurerats, som innehåller servrar i hello Hyper-V-kluster och deras virtuella datorer.

| Primära VMM-moln | Skyddade virtuella datorer i hello moln | Replikeringsfrekvens | Ytterligare återställningspunkter |
| --- | --- | --- | --- |
| PrimaryCloudRpo15m |142 |15 minuter |Ingen |
| PrimaryCloudRpo30s |47 |30 sekunder |Ingen |
| PrimaryCloudRpo30sArp1 |47 |30 sekunder |1 |
| PrimaryCloudRpo5m |235 |5 minuter |Ingen |

### <a name="performance-metrics"></a>Prestandamått

hello tabell sammanfattas hello prestandamått och räknare som har mätt i hello-distribution.

| Mått | Räknaren |
| --- | --- |
| Processor |\Processor(_Total)\% processortid |
| Tillgängligt minne |\Memory\Available megabyte |
| IOPS |\PhysicalDisk (_Total) \Disk disköverföringar/sek |
| VM läsåtgärder (IOPS) per sekund |\Hyper-V virtuell lagringsenhet (<VHD>) \Read åtgärder/sek |
| VM skrivåtgärder (IOPS) per sekund |\Hyper-V virtuell lagringsenhet (<VHD>) \Write åtgärder/S |
| VM läsa genomflöde |\Hyper-V virtuell lagringsenhet (<VHD>) \Read byte/sek |
| Genomströmning för skrivning till VM |\Hyper-V virtuell lagringsenhet (<VHD>) \Write byte/sek |

## <a name="next-steps"></a>Nästa steg

[Konfigurera replikering mellan två lokala VMM platser](site-recovery-vmm-to-vmm.md)
