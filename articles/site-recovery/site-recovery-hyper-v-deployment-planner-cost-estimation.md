---
title: "Distributionshanteraren för Azure Site Recovery och kostnadsuppskattningar för Hyper-V till Azure| Microsoft Docs"
description: "I den här artikeln beskrivs den detaljerade kostnadsuppskattningen i rapporten som genereras i Distributionshanteraren för Azure Site Recovery för scenariot Hyper-V till Azure."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: b36e8064da2661e6424d68f11b1e08d5af96131f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/08/2017
---
# <a name="cost-estimation-report-of-azure-site-recovery-deployment-planner"></a>Rapporten Kostnadsuppskattning i Distributionshanteraren för Azure Site Recovery  

Rapporten i distributionshanteraren ger en översikt över kostnadsuppskattningen på bladet [Rekommendationer](site-recovery-hyper-v-deployment-planner-analyze-report.md#recommendations) och en detaljerad kostnadsanalys på bladet Kostnadsuppskattning. Där ges en detaljerad kostnadsanalys per virtuell dator. 

### <a name="cost-estimation-summary"></a>Sammanfattning av kostnadsuppskattning 
I diagrammet visas en sammanfattning av den uppskattade totala kostnaden för haveriberedskap (DR) till Azure i din valda målregion och i den valuta du har angett för rapporten.
![Sammanfattning av kostnadsuppskattning](media/site-recovery-hyper-v-deployment-planner-analyze-report/cost-estimation-summary-h2a.png) Sammanfattningen hjälper dig att förstå den kostnad som du behöver betala för lagring, beräkning, nätverk och licens när du skyddar alla dina kompatibla virtuella datorer till Azure med Azure Site Recovery. Kostnaden beräknas för kompatibla virtuella datorer och inte för alla profilerade virtuella datorer.  
 
Du kan visa kostnaden per månad eller per år. Läs mer om [målregioner som stöds](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) och [valutor som stöds](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies).

**Cost by components** (Kostnad per komponenter) Den totala DR-kostnaden delas upp i fyra komponenter: beräkning, lagring, nätverk och Azure Site Recovery-licenskostnad. Kostnaden beräknas baserat på förbrukningen som tillkommer under replikering och DR-testtiden för beräkning, lagring (premium och standard), ExpressRoute/VPN som har konfigurerats mellan den lokala platsen och Azure och Azure Site Recovery-licens.

**Cost by states** (Kostnad per tillstånd) Den totala kostnaden för haveriberedskap (DR) är kategorier baserat på två olika tillstånd – replikering och DR-test. 

**Replication cost** (Replikeringskostnad): Kostnaden som tillkommer under replikering. Det här täcker kostnaden för lagring, nätverk och Azure Site Recovery-licensen. 

**DR-Drill cost** (DR-testkostnad): Kostnaden som tillkommer under redundanstext. Azure Site Recovery startar virtuella datorer under redundanstest. DR-testkostnaden täcker beräkning och lagring för de virtuella datorer som körs. 

**Azure storage cost per Month/Year**: (Azure Storage-kostnad per månad/år) det här visar den totala lagringskostnad som tillkommer för premium- och standardlagring för replikering och redundanstest.

## <a name="detailed-cost-analysis"></a>Detaljerad kostnadsanalys
Azure-priser för beräkning, lagring, nätverk och så vidare varierar mellan olika Azure-regioner. Du kan generera rapporten Kostnadsuppskattning med de senaste priserna baserat på din prenumeration, erbjudandet som är associerat med din prenumeration och för den angivna Azure-målregionen i den angivna valutan. Som standard används Azure-regionen USA, västra 2 och valutan amerikanska dollar (USD) i verktyget. Om du använder någon annan region eller valuta används den senast använda regionen och valutan till kostnadsuppskattningen nästa gång du skapar en rapport utan prenumerations-ID, erbjudande-ID, målregionen eller valuta.
I det här avsnittet visas det prenumerations-ID och erbjudande-ID du använde till att generera rapporten.  Om de inte används är det tomt.

De grå cellerna i hela rapporten är skrivskyddade. Celler i vitt kan ändras efter behov.

![Detaljerad kostnadsuppskattning1](media/site-recovery-hyper-v-deployment-planner-cost-estimation/cost-estimation1-h2a.png)

### <a name="overall-dr-cost-by-components"></a>Övergripande kostnad för haveriberedskap per komponent
I det första avsnittet visas den sammanlagda kostnaden för haveriberedskap per komponent och kostnaden för haveriberedskap per tillstånd. 

**Databearbetning**: kostnaden för virtuella IaaS-datorer som körs på Azure för haveriberedskap. Här ingår de virtuella datorer som skapas av Azure Site Recovery under DR-tester (redundanstester) och de virtuella datorer som körs på Azure som SQL Server med AlwaysOn-tillgänglighetsgrupper och domänkontrollanter/DNS-servrar.

**Lagring**: kostnaden för Azure-lagring i samband med haveriberedskap. Här ingår användningen av lagringsutrymme för replikering under DR-tester.
Nätverk: kostnaden för ExpressRoute och VPN plats-till-plats i samband med haveriberedskap. 

**ASR-licens**: licenskostnaden för Azure Site Recovery för samtliga kompatibla virtuella datorer. Om du har angett en virtuell dator manuellt i tabellen med den detaljerade kostnadsanalysen ingår även licenskostnaden för Azure Site Recovery för den virtuella datorn.

### <a name="overall-dr-cost-by-states"></a>Övergripande kostnad för haveriberedskap per tillstånd
Den totala kostnaden för haveriberedskap kategoriseras utifrån två olika tillstånd – replikering och DR-test.

**Replikering**: kostnader som uppstår under replikering. Det här täcker kostnaden för lagring, nätverk och Azure Site Recovery-licensen. 

**DR-test**: kostnader som uppstår under DR-test. Azure Site Recovery startar virtuella datorer under DR-test. Kostnaden för DR-test täcker databearbetning och lagring för de virtuella datorer som körs.
Total varaktighet för DR-test under ett år = antalet DR-test x varaktigheten för respektive DR-test (dagar) Genomsnittlig kostnad för DR-test (per månad) = total kostnad för DR-test / 12

### <a name="storage-cost-table"></a>Tabellen Lagringskostnad:
I den här tabellen visas kostnaderna för premium- och standardlagring för replikering och DR-test med och utan rabatt.

### <a name="site-to-azure-network"></a>Lokalt till Azure-nätverket
Välj lämplig inställning efter behov. 

**ExpressRoute**: som standard väljer verktyget den närmaste ExpressRoute-plan som matchar nätverksbandbredden som krävs för deltareplikering. Du kan ändra planen efter behov.

**VPN-gateway**: välj VPN-gatewayen om du har en sådan i din miljö. Som standard är värdet Saknas.

**Målregion**: angiven Azure-region för haveriberedskap. Priset som används i rapporten för bearbetning, lagring, nätverk och licenser baseras på Azure-priserna i den angivna regionen. 

### <a name="vm-running-on-azure"></a>Virtuella datorer som körs i Azure
Om du har en domänkontrollant eller virtuell DNS- eller SQL Server-dator med AlwaysOn-tillgänglighetsgrupper som körs i Azure för haveriberedskap kan du ange antalet virtuella datorer och deras storlek om du vill ta med kostnaden för dem i den totala kostnaden för din haveriberedskap. 

### <a name="apply-overall-discount-if-applicable"></a>Tillämpa totalrabatt i förekommande fall
Om du är en Azure-partner eller en kund med rätt till rabatt på totalpriset för Azure kan du använda det här fältet. Verktyget tillämpar rabatten (i procent) på samtliga komponenter.

### <a name="number-of-virtual-machines-type-and-compute-cost-per-year"></a>Antal virtuella datorer av olika typer och beräkningskostnad (per år)
I den här tabellen visas antalet virtuella Windows-datorer och andra datorer samt beräkningskostnaden för dem i samband med DR-test.

### <a name="settings"></a>Inställningar 
**Hanterade diskar används**: anger om hanterade diskar används under DR-test. Standardinställningen är Ja. Om du har satt -UseManagedDisks till Nej används priset för ohanterade diskar i beräkningen.

**Valuta**: valutan som rapporten ska genereras i. Kostnadsperiod: du kan visa alla kostnader antingen för månaden eller för hela året. 

## <a name="detailed-cost-analysis-table"></a>Tabell med detaljerad kostnadsanalys
![Detaljerad kostnadsanalys](media/site-recovery-hyper-v-deployment-planner-cost-estimation/detailed-cost-analysis-h2a.png) Den här tabellen innehåller kostnadsdata för varje kompatibel virtuell dator. Du kan också använda den här tabellen till att hämta uppskattad Azure-kostnad för haveriberedskap för icke-profilerade virtuella datorer genom att lägga till virtuella datorer manuellt. Det här är användbart om du behöver uppskatta Azure-kostnader för en ny DR-distribution utan att någon detaljerad profilering har gjorts.
Så här lägger du till virtuella datorer manuellt: 
1.  Klicka på knappen Infoga rad så att du infogar en ny rad mellan start- och slutraden.

2.  Fyll i följande kolumner baserat på ungefärlig storlek och antalet virtuella datorer som matchar den här konfigurationen: 

* Antal virtuella datorer, IaaS-storlek (dina val)
* Lagringstyp (Standard/Premium)
* Totalt lagringsutrymme för virtuella datorer (GB)
* Antal DR-tester under ett år 
* Varaktighet för varje DR-test (dagar) 
* OS-typ
* Dataredundans 
* Azure Hybrid-förmånen

3.  Du kan använda samma värde för alla virtuella datorer i tabellen genom att klicka på knappen Använd för alla, för kolumnerna Antal DR-tester under ett år, Varaktighet för varje DR-test (dagar), Dataredundans och Azure Hybrid-förmånen.

4.  Klicka på alternativet för att beräkna kostnaden på nytt om du vill uppdatera kostnaden.

**Namn på virtuell dator**: namnet på den virtuella datorn.

**Antal virtuella datorer**: antalet virtuella datorer som matchar konfigurationen. Du kan uppdatera antalet befintliga virtuella datorer om virtuella datorer med liknande konfiguration inte står med i listan men kommer att skyddas.

**IaaS-storlek (rekommendation)**: det är storleken på de kompatibla virtuella datorer som verktyget rekommenderar för rollen. 

**IaaS-storlek (ditt val)**: som standard är det här samma som den rekommenderade storleken. Du kan ändra rollen efter behov. Den beräknade kostnaden baseras på vald storlek för rollen.

**Lagringstyp**: typen av lagring som används för den virtuella datorn. Det här är antingen Standard eller Premium.

**Totalt lagringsutrymme (GB) för virtuella datorer**: det totala lagringsutrymmet för den virtuella datorn.

**Antal DR-tester under ett år**: antalet gånger som du utför DR-tester under ett år. Standardvärdet är 4 gånger per år. Du kan ändra perioden för specifika virtuella datorer eller använda det nya värdet för alla virtuella datorer genom att ange det nya värdet på den översta raden och klicka på knappen Använd för alla. Den totala kostnaden för DR-tester är baserad på antalet DR-tester under ett år och varaktigheten för varje DR-test.  

**Varaktighet för varje DR-test (dagar)**: varaktigheten för varje DR-test. Standardvärdet är 7 dagar varje 90 dagar enligt förmånen [Disaster Recovery Software Assurance](https://azure.microsoft.com/en-in/pricing/details/site-recovery). Du kan ändra perioden för specifika virtuella datorer eller använda ett nytt värde för alla virtuella datorer genom att ange ett nytt värde på den översta raden och klicka på Använd för alla. Totalkostnad för DR-tester beräknas baserat på antalet DR-tester under ett år och varaktigheten för varje DR-testperiod.
  
**OS-typ**: den virtuella datorns operativsystem. Det här är antingen Windows eller Linux. Om OS-typen är Windows kan du använda Azure Hybrid-förmånen för den virtuella datorn. 

**Dataredundans**: det här kan vara något av följande – lokalt redundant lagring (LRS), geo-redundant lagring (GRS) eller geo-redundant lagring med läsbehörighet (RA-GRS). Standardvärdet är LRS. Du kan ändra typen baserat på lagringskontot för de olika virtuella datorerna, eller så kan du använda en ny typ för alla virtuella datorer genom att ändra typen på den översta raden och klicka på Använd för alla.  Lagringskostnaden för replikering beräknas baserat på priset för den dataredundans du har valt. 

**Azure Hybrid-förmånen**: du kan använda Azure Hybrid-förmånen för virtuella Windows-datorer i förekommande fall.  Standardvärdet är Ja. Du kan ändra inställningen för specifika virtuella datorer eller uppdatera alla virtuella datorer genom att klicka på knappen Använd för alla.

**Total Azure-förbrukning**: det här omfattar bearbetning, lagring och kostnaden för Azure Site Recovery-licenser för din haveriberedskap. Baserat på ditt val visas kostnaden antingen per månad eller år.

**Replikeringskostnad för stabilt tillstånd**: det här omfattar lagringskostnaden för replikering.

**Totalkostnad för DR-test (genomsnittlig)**: det här omfattar kostnaden för beräkning och lagring för DR-tester.

**Kostnad för ASR-licens**: kostnad för Azure Site Recovery-licensen.

## <a name="supported-target-regions"></a>Målregioner som stöds
Distributionshanteraren för Azure Site Recovery kan ge kostnadsuppskattningar för följande Azure-regioner. Om inte din region visas nedan kan du använda någon av följande regioner vars priser ligger närmast din region.

usaöstra, usaöstra2, usavästra, usacentrala, usanorracentrala, usasödracentrala, europanorra, europavästra, asienöstra, sydostasien, japanöstra, japanvästra, australienöstra, australiensydöstra, brasiliensödra, indiensödra, indiencentrala, indienvästra, kanadacentrala, kanadaöstra, usavästra2, usavästracentrala, södrastorbritannien, västrastorbritannien, koreacentrala, koreasödra 

## <a name="supported-currencies"></a>Valutor som stöds
Distributionshanteraren för Azure Site Recovery kan generera kostnadsrapporten i någon av följande valutor.

|Valuta|Namn||Valuta|Namn||Valuta|Namn|
|---|---|---|---|---|---|---|---|
|ARS|Argentinska pesos ($)||AUD|Australiensiska dollar ($)||BRL|Brasilianska Real (R$)|
|CAD|Kanadensiska dollar ($)||CHF|Schweiziska franc (chf)||DKK|Danska kronor (kr)|
|EUR|Euro (€)||GBP|Brittiska pund (£)||HKD|Hongkong Dollar (HK$)|
|IDR|Indonesiska rupier (Rp)||INR|Indiska rupier (₹)||JPY|Japanska yen (¥)|
|KRW|Sydkoreanska won (₩)||MXN|Mexikanska Peso (MX$)||MYR|Malaysiska Ringgit (RM$)|
|NOK|Norska kronor (kr)||NZD|Nya zeeländska dollar ($)||RUB|Ryska rubel (руб)|
|SAR|Saudiarabiska riyal (R)||SEK|Svenska kronor (kr)||TWD|Taiwanesiska dollar (NT$)|
|TRY|Turkiska lira (TL)||USD| Amerikanska dollar ($)||ZAR|Sydafrikanska rand (R)|

## <a name="next-steps"></a>Nästa steg
Lär dig mer om hur du skyddar [virtuella Hyper-V-datorer till Azure med Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-hyper-v-to-azure).
