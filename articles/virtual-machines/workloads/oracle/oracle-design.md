---
title: "aaaDesign och implementera en Oracle-databas på Azure | Microsoft Docs"
description: "Utforma och implementera en Oracle-databas i Azure-miljön."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/22/2017
ms.author: rclaus
ms.openlocfilehash: 8fa1207458695df1c7330ec626888b1b6b8d8939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Utforma och implementera en Oracle-databas i Azure

## <a name="assumptions"></a>Antaganden

- Du planerar toomigrate en Oracle-databas från lokala tooAzure.
- Du har en förståelse av hello olika mått i Oracle AWR rapporter.
- Du har en grundläggande förståelse för programmets prestanda och användning av plattform.

## <a name="goals"></a>Mål

- Förstå hur toooptimize Oracle distributionen i Azure.
- Utforska prestandajustering alternativ för en Oracle-databas i en Azure-miljö.

## <a name="hello-differences-between-an-on-premises-and-azure-implementation"></a>Hej skillnaderna mellan en lokal och Azure-implementering 

Följande är exempel på viktiga saker tookeep i åtanke när du migrerar lokala program tooAzure. 

En viktig skillnad är att resurser som virtuella datorer, diskar och virtuella nätverk i en Azure-implementering måste delas med andra klienter. Dessutom kan resurser vara begränsas baserat på hello krav. I stället för att fokusera på att undvika misslyckas (MTBF), fokuserar Azure mer på kvarvarande hello-fel (MTTR).

hello följande tabell visas några av hello skillnader mellan en lokal implementering och en Azure implementering av en Oracle-databas.

> 
> |  | **Lokal implementering** | **Azure-implementering** |
> | --- | --- | --- |
> | **Nätverk** |LAN/WAN  |SDN (programvarudefinierat nätverk)|
> | **Säkerhetsgrupp** |IP-port begränsning verktyg |[Nätverkssäkerhetsgrupp (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Återhämtning** |MTBF (Genomsnittlig tid mellan fel) |MTTR (tiden toorecovery)|
> | **Planerat underhåll** |Uppdatering/uppgradering|[Tillgänglighetsuppsättningar](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (korrigering/uppgraderingar hanteras av Azure) |
> | **Resurs** |Dedikerad  |Delas med andra klienter|
> | **Regioner** |Datacenter |[Region-par](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | **Storage** |SAN/fysiska diskar |[Hanteras av Azure storage](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Skalning** |Lodrät skala |Horisontell skalning|


### <a name="requirements"></a>Krav

- Bestämma hello databasen storlek och tillväxt.
- Fastställa hello IOPS krav som du kan beräkna baserat på Oracle AWR rapporter eller andra verktyg för nätverksövervakning.

## <a name="configuration-options"></a>Konfigurationsalternativ

Det finns fyra möjliga problemområden att du kan finjustera tooimprove prestanda i en Azure-miljö:

- Storlek på virtuell dator
- Dataflödet i nätverket
- Disktyper och konfigurationer
- Inställningar för cachelagring av disk

### <a name="generate-an-awr-report"></a>Generera en rapport för AWR

Om du har en befintlig en Oracle-databas och planerar toomigrate tooAzure, har du flera alternativ. Du kan köra hello Oracle AWR rapporten tooget hello mått (IOPS, Mbit/s, GiBs och så vidare). Välj hello VM baserat på hello mått som du samlade in. Eller så kan du kontakta din infrastruktur team tooget liknande information.

Du kan även köra rapporten AWR under både vanliga och högsta antal arbetsbelastningar, så kan du jämföra. Baserat på de här rapporterna kan du ändra storlek hello virtuella datorer baserat på hello genomsnittlig arbetsbelastning eller hello maximal arbetsbelastning.

Följande är ett exempel på hur toogenerate en AWR rapport:

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>Viktiga mått

Följande är hello mått som du kan hämta från hello AWR rapporten:

- Totalt antal kärnor
- CPU-klockfrekvens
- Totalt minne i GB
- CPU-användning
- Högsta överföringshastighet
- Antalet i/o-ändringar (läsa/skriva)
- Gör om loggen hastighet (MBPs)
- Dataflödet i nätverket
- Nätverket latens hastighet (låg/hög)
- Databasens storlek i GB
- Byte som tagits emot via SQL * Net från / tooclient

### <a name="virtual-machine-size"></a>Storlek på virtuell dator

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-hello-awr-report"></a>1. Beräkna VM-storlek baserat på CPU, minne och i/o-användning från hello AWR rapport

En sak som du kan titta på är hello översta fem tidsinställd förgrunden händelser som indikerar om hello system flaskhalsar är.

I följande diagram hello, är exempelvis hello loggen filsynkronisering hello överst. Den anger hello väntar som krävs innan hello LGWR skriver hello buffert toohello gör om loggen loggfilen. Dessa resultat indikerar att bättre prestanda lagring eller diskar är nödvändiga. Hello diagram visar dessutom också hello antalet CPU (kärnor) och hello mängden minne.

![Skärmbild av sidan hello AWR](./media/oracle-design/cpu_memory_info.png)

hello visar följande diagram hello total i/o för läsning och skrivning. Det fanns 59 läsa och 247.3 GB skrivs under hello tiden för hello-rapport.

![Skärmbild av sidan hello AWR](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Välj en virtuell dator

Baserat på hello information du samlade in från hello AWR rapporten är hello nästa steg toochoose en virtuell dator med samma storlek som uppfyller dina krav. Du hittar en lista med tillgängliga virtuella datorer i hello artikel [Minnesoptimerade](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).

#### <a name="3-fine-tune-hello-vm-sizing-with-a-similar-vm-series-based-on-hello-acu"></a>3. Finjustera hello VM-storlek med liknande VM baserat på hello ACU

När du har valt hello VM betala uppmärksamhet toohello ACU för hello VM. Du kan välja en annan virtuell dator baserat på hello ACU-värde som passar dina behov bättre. Mer information finns i [Azure compute enhet](https://docs.microsoft.com/azure/virtual-machines/windows/acu).

![Skärmbild av sidan för hello ACU enheter](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Dataflödet i nätverket

följande diagram visar hello relationen mellan genomflöde och IOPS hello:

![Skärmbild av dataflöde](./media/oracle-design/throughput.png)

hello totala genomflödet beräknas utifrån hello följande information:
- SQL * Net trafik
- Mbit/s x antal servrar (till exempel Oracle Data Guard utgående ström)
- Andra faktorer, till exempel program replikering

![Skärmbild av hello SQL * Net genomflöde](./media/oracle-design/sqlnet_info.png)

Baserat på din kraven på nätverksbandbredd finns olika typer av gateway för toochoose från. Dessa inkluderar basic VpnGw och Azure ExpressRoute. Mer information finns i hello [VPN-gateway sida med priser](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).

**Rekommendationer**

- Nätverksfördröjningen är högre jämfört med tooan lokal distribution. Minskar nätverket avrunda resor kan kraftigt förbättra prestanda.
- tooreduce turer konsolidera program som har hög transaktioner eller ”chatty” appar i hello samma virtuella dator.

### <a name="disk-types-and-configurations"></a>Disktyper och konfigurationer

- *OS-diskar som standard*: dessa disktyper erbjuder beständiga data och cachelagring. De är optimerade för OS åtkomst vid start och inte har utformats för antingen transaktionella eller datalager (Analytiska) arbetsbelastningar.

- *Ohanterad diskar*: dessa disktyper du hantera hello storage-konton som lagrar hello virtuell hårddisk (VHD)-filer som motsvarar tooyour Virtuella diskar. VHD-filer lagras som sidblobbar i Azure storage-konton.

- *Hanterade diskar*: Azure hanterar hello storage-konton som du använder för din Virtuella diskar. Anger hello disktyp (premium eller standard) och hello storleken på hello-disk som du behöver. Azure skapar och hanterar hello disk du.

- *Premium-lagringsdiskar*: dessa disktyper som passar bäst för produktionsarbetsbelastningar. Premium storage stöder Virtuella diskar som kan vara ansluten toospecific storlek-serien virtuella datorer, till exempel DS, DSv2, GS och F serien virtuella datorer. hello premium disk levereras med olika storlekar och du kan välja mellan diskar från 32 GB too4, 096 GB. Varje diskstorleken har sin egen prestandakrav. Du kan koppla en eller flera diskar tooyour VM beroende på kraven för application.

När du skapar en ny hanterade disk från hello portal, kan du välja hello **kontotyp** hello typ av disk du vill ha toouse. Tänk på att inte alla tillgängliga diskar visas i hello nedrullningsbara menyn. När du väljer en viss VM-storlek, visar hello-menyn endast hello tillgängliga premium-lagring SKU: er som är baserade på att VM-storlek.

![Skärmbild av sidan för hello hanterade diskar](./media/oracle-design/premium_disk01.png)

Mer information finns i [Premium-lagring med hög prestanda och hanterade diskar för virtuella datorer](https://docs.microsoft.com/azure/storage/storage-premium-storage).

När du har konfigurerat din lagring på en virtuell dator kanske du vill tooload test hello diskar innan du skapar en databas. Att veta hello i/o-hastighet på både svarstid och genomströmning kan hjälpa dig att avgöra om hello virtuella datorer stöder hello förväntades dataflöde med latens mål.

Det finns ett antal verktyg programtest belastning, till exempel Oracle Orion, Sysbench och Fio.

Kör hello belastningstest igen när du har distribuerat en Oracle-databas. Starta din regelbundet och högsta antal arbetsbelastningar och hello resultatet visar du hello baslinje för din miljö.

Det kan vara viktigare toosize hello-lagring baseras på hello IOPS hastighet i stället för hello lagringsstorlek. Till exempel om hello krävs IOPS är 5 000, men du behöver bara 200 GB, kan du fortfarande få hello P30 klassen premium disk trots att den innehåller fler än 200 GB lagringsutrymme.

hello IOPS hastighet kan hämtas från hello AWR rapporten. Det bestäms av hello gör om loggen, fysiska läsningar och skrivningar snabbt.

![Skärmbild av sidan hello AWR](./media/oracle-design/awr_report.png)

Till exempel är hello gör om 12,200,000 byte per sekund, vilket är lika too11.63 Mbit/s.
hello IOPS är 12 200 000 / 2,358 = 5,174.

När du har en tydlig bild av hello i/o-kraven kan välja du en kombination av enheter som är bäst lämpade toomeet dessa krav.

**Rekommendationer**

- Sprider sig hello i/o-belastning över ett antal diskar med hjälp av hanterade lagringspoolen eller Oracle ASM för data tabellutrymmet.
- Hello i/o-blockstorlek ökar för läsintensiva- och skrivåtgärder-intensiva, lägga till flera datadiskar.
- Öka hello blockstorlek för stora sekventiella processer.
- Använd data komprimering tooreduce i/o (för både data och index).
- Avgränsa gör om loggfiler, system och temps och ångra TS på separata hårddiskar.
- Placera inte några programfiler på standard OS-diskar (/ dev/sda). Diskarna inte är optimerad för snabb VM Start gånger, och de kan inte ange goda prestanda för ditt program.

### <a name="disk-cache-settings"></a>Inställningar för cachelagring av disk

Det finns tre alternativ för värdcachelagring:

- *Skrivskyddad*: alla begäranden cachelagras för framtida läsningar. Alla skrivningar sparas direkt tooAzure Blob storage.

- *Skrivskyddad*: Detta är en ”read-ahead” algoritm. hello läser och skriver cachelagras för framtida läsningar. Icke skrivning via skrivningar är beständiga toohello lokalt cacheminne först. För SQL Server är skrivningar beständiga tooAzure lagring eftersom den använder skrivning via. Det ger också hello lägsta disk fördröjningen för enstaka arbetsbelastningar.

- *Ingen* (inaktiverat): med det här alternativet kan du kringgå hello cache. Alla hello data överförda toodisk och beständiga tooAzure lagring. Den här metoden ger du hello högsta i/o-hastighet för i/o-intensiv arbetsbelastning. Du måste också tootake ”transaktionen kostnad” i beräkningen.

**Rekommendationer**

toomaximize hello dataflöde, rekommenderar vi att du börjar med **ingen** för cachelagring av värden. För Premium-lagring, Tänk på att du måste inaktivera hello ”barriärer” när du monterar hello filsystem med hello **ReadOnly** eller **ingen** alternativ. Uppdatera hello /etc/fstab filen med hello UUID toohello diskar.

Mer information finns i [Premium-lagring för virtuella Linux-datorer](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).

![Skärmbild av sidan för hello hanterade diskar](./media/oracle-design/premium_disk02.png)

- Använd standard för OS diskar **läsning och skrivning** cachelagring.
- Använd för SYSTEM, TEMP och ångra **ingen** för cachelagring.
- DATA, Använd **ingen** för cachelagring. Men om databasen är skrivskyddad eller läsintensiva använder **skrivskyddad** cachelagring.

När inställningen disk data sparas, kan du inte ändra hello värden cache-inställningen om du inte demontera hello enheten vid hello OS-nivån och sedan montera när du har gjort hello ändra.


## <a name="security"></a>Säkerhet

När du har skapat och konfigurerat Azure-miljön, hello nästa steg är toosecure nätverket. Här följer några rekommendationer:

- *NSG princip*: NSG kan definieras av ett undernät eller nätverkskort. Det är enklare toocontrol åtkomst på hello undernätverksnivå både för säkerhet och kraft routning för till exempel brandväggar för programmet.

- *Jumpbox*: för säkrare åtkomst administratörer bör inte ansluter direkt toohello programtjänsten eller databas. En jumpbox används som en media mellan Hej administratör dator och Azure-resurser.
![Skärmbild av hello Jumpbox topologi sida](./media/oracle-design/jumpbox.png)

    Hej administratör datorn bör erbjuda IP tillgång toohello jumpbox. Hej jumpbox ska ha åtkomst toohello programmet och databasen.

- *Privat nätverk* (undernät): Vi rekommenderar att du har hello programtjänsten och databasen på olika undernät så bättre kontroll kan anges av NSG principen.


## <a name="additional-reading"></a>Ytterligare resurser

- [Konfigurera Oracle ASM](configure-oracle-asm.md)
- [Konfigurera Oracle Data Guard](configure-oracle-dataguard.md)
- [Konfigurera Oracle guld Gate](configure-oracle-golden-gate.md)
- [Oracle-säkerhetskopiering och återställning](oracle-backup-recovery.md)

## <a name="next-steps"></a>Nästa steg

- [Självstudier: Skapa högtillgängliga virtuella datorer](../../linux/create-cli-complete.md)
- [Utforska VM distribution Azure CLI-exempel](../../linux/cli-samples.md)
