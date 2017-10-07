---
title: "aaaReplicate en Dynamics AX-distribution för flera nivåer som med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooreplicate och skydda Dynamics AX med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/24/2017
ms.author: asgang
ms.openlocfilehash: b974315ec50ab2ec43846b3d3f95c7de88b72fc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a>Replikera en Dynamics AX flernivåapp med hjälp av Azure Site Recovery

## <a name="overview"></a>Översikt


Microsoft Dynamics AX är en av hello populäraste ERP-lösning mellan företag toostandardized process mellan platser, hantera resurser och förenkla efterlevnad. Considering hello programmet är kritiska tooan organisationen är det mycket viktigt att toobe som om en katastrof som programmet ska vara igång i minimitid.

Microsoft Dynamics AX ger idag inte några funktioner för out box katastrofåterställning. Microsoft Dynamics AX består av många server-komponenter som objektet programservern, Active Directory (AD), SQL Database-Server, SharePoint Server, etc. Reporting Server toomanage hello katastrofåterställning för var och en av dessa komponenter manuellt är inte bara billigare men även felbenägna.

Den här artikeln innehåller detaljerad information om hur du skapar en lösning för katastrofåterställning för din Dynamics AX-program med [Azure Site Recovery](site-recovery-overview.md). Den omfattar också planerad/oplanerad/redundanstestning med ett klick återställningsplan, konfigurationer som stöds och förutsättningar.
Azure Site Recovery-baserad lösning för katastrofåterställning är fullständigt testade certifierad och rekommenderas av Microsoft Dynamics AX.



## <a name="prerequisites"></a>Krav

Implementera haveriberedskap för Dynamics AX-program med hjälp av Azure Site Recovery kräver hello följande förutsättningar slutfördes.

• En lokal Dynamics AX-distribution har konfigurerats

• Azure Site Recovery Services-valvet har skapats i Microsoft Azure-prenumeration

• Om Azure är din återställningsplatsen kör hello Azure virtuella Readiness Assessment tool på virtuella datorer tooensure som de är kompatibla med virtuella Azure-datorer och Azure Site Recovery Services


## <a name="site-recovery-support"></a>Site Recovery-stöd

För hello syftet med att skapa den här artikeln, användes VMware-datorer med Dynamics AX 2012R3 på Windows Server 2012 R2 Enterprise. Eftersom site recovery replikering oberoende av programmet hello rekommendationer som anges här förväntade toohold på samt följande scenarier.

### <a name="source-and-target"></a>Källa och mål

**Scenario** | **tooa sekundär plats** | **tooAzure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysisk server** | Ja | Ja

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a>Aktivera DR Dynamics AX-program med hjälp av Azure Site Recovery
### <a name="protect-your-dynamics-ax-application"></a>Skydda din Dynamics AX-program
Varje komponent i hello Dynamics AX behov toobe skyddade tooenable hello hela appen replikering och återställning. Det här avsnittet beskriver:

**1. Skydd av Active Directory**

**2. Skydd av SQL-nivå**

**3. Skydd av appen och webb-nivåer**

**4. Nätverkskonfiguration**

**5. Återställningsplan**

### <a name="1-setup-ad-and-dns-replication"></a>1. Installationsprogrammet för AD- och DNS-replikering

Active Directory krävs för hello DR-plats för toofunction för Dynamics AX-program. Det finns två rekommenderade alternativ baserat på hello komplexitet hello kundens lokala miljö.

**Alternativ 1**

Om hello kunden har ett litet antal program och en enda domänkontrollant för sin hela lokal plats och kommer att redundansväxla hello hela webbplatsen tillsammans, och vi rekommenderar att du använder ASR replikering tooreplicate hello DC datorn toosecondary plats ( gäller för både plats tooSite och platsen tooAzure).

**Alternativ 2**

Om hello kunden har ett stort antal program och kör en Active Directory-skog och kommer failover några program i taget, så rekommenderar vi att ställa in ytterligare en domänkontrollant på hello DR-plats (sekundär plats eller i Azure).

Se för[handbok för att göra en domänkontrollant tillgänglig på DR-plats](site-recovery-active-directory.md). I resten av det här dokumentet kommer vi antar att en Domänkontrollant är tillgänglig på DR-plats.

### <a name="2-setup-sql-server-replication"></a>2. Konfigurera SQL Server-replikering
Mer detaljerad teknisk information om hello rekommenderas för att skydda finns toocompanion guide [SQL nivå](site-recovery-sql.md).

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a>3. Aktivera skydd för Dynamics AX-klienten och AOS virtuella datorer
Konfigurera relevanta Azure Site Recovery baserat på om hello virtuella datorer distribueras på [Hyper-V](site-recovery-hyper-v-site-to-azure.md) eller på [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Rekommenderade krascher konsekvent frekvens tooconfigure är 15 minuter.
>

hello nedan ögonblicksbild visar hello skydd för virtuella datorer Dynamics-komponenten i 'VMware plats tooAzure' skyddsscenariet.
![Skyddade objekt](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Konfigurera nätverksfunktioner
Konfigurera VM beräknings- och nätverksinställningar

Konfigurera nätverksinställningar i Azure Site Recovery hello AX-klienten och AOS virtuella datorer så att hello Virtuella datornätverk hämta bifogade toohello rätt DR-nätverket efter redundans. Kontrollera hello DR-nätverk för de här nivåerna är dirigerbara toohello SQL-nivå.

Du kan välja hello VM i hello replikerade objekt tooconfigure hello nätverksinställningar enligt hello ögonblicksbild nedan.

* För AOS-servrar väljer hello korrekt tillgänglighet.

* Om du använder en statisk IP-adress och sedan ange hello IP-adress som du vill hello virtuella tootake i hello **mål-IP** fältet ![nätverksinställningar](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)



### <a name="5-creating-a-recovery-plan"></a>5. Skapa en återställningsplan

Du kan skapa en återställningsplan i Azure Site Recovery tooautomate hello failover-processen. Lägg till app-nivå och webbnivå i hello planera för återställning. Ordna dem i olika grupper så som hello frontend avstängning innan app-nivå.

1)  Välj hello Azure Site Recovery-valvet i din prenumeration och klicka på 'Återställningsplaner' sida vid sida.

2)  Klicka på ”+ Recovery planera och ange ett namn.

3)  Välj hello 'Source' och 'Target'. hello mål kan vara Azure eller sekundär plats. Om du väljer Azure måste du ange hello distributionsmodell

![Skapa återställningsplan](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  Välj hello AOS och klienten VMs toohello återställningsplan och klicka på ✓.
![Skapa återställningsplan](./media/site-recovery-dynamics-ax/selectvms.png)


![Återställningsplan](./media/site-recovery-dynamics-ax/recoveryplan.png)

Du kan anpassa hello återställningsplan för Dynamics AX-program genom att lägga till olika steg som förklaras nedan. hello ovan ögonblicksbild visar hello fullständig återställningsplan när du lägger till alla hello steg.

*Så här:*

*1. SQL Server växlar över steg*

Referera för[lösning för SQL Server DR](site-recovery-sql.md) handbok för mer information om återställningsservern steg specifika tooSQL.

*2. Redundans grupp 1: Växla över hello AOS virtuella datorer*

Se till att hello vald återställningspunkt är så nära som möjligt toohello databasen PIT men inte vidare.

*3. Skript: Lägg till belastningsutjämnare (endast E-A)* lägga till ett skript (via Azure automation) efter AOS VM gruppen medföljer tooadd en load balancer tooit. Du kan använda ett skript toodo den här uppgiften. Läs artikeln [hur tooadd belastningsutjämnare för flera nivåer program Katastrofåterställning](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)

*4. Redundans, grupp 2: Växla över hello AX-klienten virtuella datorer.*
Växla över hello webbnivå virtuella datorer som en del av hello återställningsplan.


### <a name="doing-a-test-failover"></a>Gör ett redundanstest

Se too'AD DR Solution ' och tilläggsguiderna för Katastrofåterställning av SQL Server-lösning för överväganden specifika tooAD och SQLServer respektive under Redundanstestningen.

1.  Gå tooAzure portal och välj Site Recovery-valvet.
2.  Klicka på hello återställningsplan som skapats för Dynamics AX.
3.  Klicka på Testa redundans.
4.  Välj hello virtuellt nätverk toostart hello test failover process.
5.  Du kan utföra din verifieringar när hello sekundära miljön är upp.
6.  När hello verifieringar har slutförts, kan du välja verifieringar slutföra och hello redundanstestmiljön kommer att rensas.

Följ [vägledningen](site-recovery-test-failover-to-azure.md) toodo testa redundans.

### <a name="doing-a-failover"></a>Genomför en redundansväxling enligt

1.  Gå tooAzure portal och välj Site Recovery-valvet.
2.  Klicka på hello återställningsplan som skapats för Dynamics AX.
3.  Klicka på 'Redundans' och välj 'Redundans'.
4.  Välj hello målnätverket och klicka ✓ toostart hello failover-processen.

Följ [vägledningen](site-recovery-failover.md) när du gör en redundansväxling.

### <a name="perform-a-failback"></a>Utför en återställning efter fel

Se too'SQL Server DR lösning ' handbok för överväganden specifika tooSQL server vid återställning.

1.  Gå tooAzure portal och välj Site Recovery-valvet.
2.  Klicka på hello återställningsplan som skapats för Dynamics AX.
3.  Klicka på 'Redundans' och välj växling vid fel.
4.  Klicka på Ändra-riktningen.
5.  Välj hello lämpliga alternativ - synkronisering och alternativ för att skapa VM
6.  Klicka på ✓ toostart hello 'Återställning efter fel' process.


Följ [vägledningen](site-recovery-failback-azure-to-vmware.md) när du gör en återställning efter fel.

##<a name="summary"></a>Sammanfattning
Med Azure Site Recovery kan skapa du en fullständig automatiserad haveriberedskapsplan för Dynamics AX-program. Du kan initiera hello redundans inom några sekunder från var som helst i hello händelse av ett avbrott och få programmet hello igång i minuter.

## <a name="next-steps"></a>Nästa steg
Läs [vilka arbetsbelastningar kan jag skydda?](site-recovery-workload.md) toolearn mer om hur du skyddar arbetsbelastningar på företag med Azure Site Recovery.
