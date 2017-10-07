---
title: aaaReplicate program med SQL Server och Azure Site Recovery | Microsoft Docs
description: "Den här artikeln beskriver hur tooreplicate SQL Server med Azure Site Recovery för SQL Server-funktioner för katastrofåterställning."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 99755f2cd2f7e924071f1e230ac4a0bda88f0a39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-sql-server-using-sql-server-disaster-recovery-and-azure-site-recovery"></a>Skydda SQL Server med SQL Server-katastrofåterställning och Azure Site Recovery

Den här artikeln beskriver hur tooprotect hello SQL Server serverdel för ett program som använder en kombination av SQL Server-affärskontinuitet och disaster recovery (BCDR)-teknik och [Azure Site Recovery](site-recovery-overview.md).

Innan du börjar, kontrollera att du förstår SQL Server funktioner för katastrofåterställning, inklusive redundanskluster, Always On-Tillgänglighetsgrupper, databasspegling och loggöverföring.


## <a name="sql-server-deployments"></a>SQL Server-distributioner

Många arbetsbelastningar använder SQL Server som grund och den kan integreras med appar som SharePoint, Dynamics och SAP, tooimplement data services.  SQL Server kan distribueras i flera olika sätt:

* **Fristående SQL Server**: SQL Server och alla databaser som finns på en enskild dator (fysisk eller virtuell). När en virtualiserad, används värden klustring för lokal hög tillgänglighet. Gästnivå hög tillgänglighet är inte implementerad.
* **SQL Server Failover Clustering instanser (alltid på FCI)**: två eller flera noder som kör SQL Server instanced med delade diskar som är konfigurerade i ett redundanskluster i Windows. Om en nod är nere kan kan hello kluster misslyckas SQL Server över tooanother instans. Den här konfigurationen är brukar användas tooimplement hög tillgänglighet på en primär plats. Den här distributionen skyddar inte mot fel eller avbrott i hello-lagret för delad lagring. En delad disk kan implementeras med hjälp av iSCSI, fibre channel eller delad vhdx.
* **SQL Always On-Tillgänglighetsgrupper**: två eller flera noder som har ställts in i en delad inget kluster med SQL Server-databaser som konfigurerats i en tillgänglighetsgrupp med synkron replikering och automatisk redundans.

 Den här artikeln använder hello efter egna SQL disaster recovery tekniker för att återställa databaser tooa fjärrplatsen:

* SQL Always On-Tillgänglighetsgrupper tooprovide för katastrofåterställning för SQL Server 2012 eller 2014 Enterprise-utgåvor.
* SQL databasspegling i läget för hög säkerhet för SQL Server Standard edition (alla versioner) eller för SQL Server 2008 R2.

## <a name="site-recovery-support"></a>Site Recovery-stöd

### <a name="supported-scenarios"></a>Scenarier som stöds
Site Recovery kan skydda SQL Server som sammanfattas i hello tabell.

**Scenario** | **tooa sekundär plats** | **tooAzure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysisk server** | Ja | Ja

### <a name="supported-sql-server-versions"></a>SQL Server-versioner som stöds
Dessa SQL Server-versioner som stöds för hello stöds scenarier:

* SQL Server 2016 Enterprise och Standard
* SQL Server 2014 Enterprise och Standard
* SQL Server 2012 Enterprise och Standard
* SQL Server 2008 R2 Enterprise och Standard

### <a name="supported-sql-server-integration"></a>Stöds SQL Server-integration

Site Recovery kan integreras med SQL Server BCDR-teknik som sammanfattas i tabellen hello tooprovide en lösning för katastrofåterställning.

**Funktion** | **Detaljer** | **SQL Server** |
--- | --- | ---
**Always On-tillgänglighetsgrupp** | Flera fristående instanser av SQL Server körs i ett redundanskluster med flera noder.<br/><br/>Databaser kan grupperas i failover-grupper som kan kopieras (speglad) på SQL Server-instanser så att ingen delad lagring krävs.<br/><br/>Ger katastrofåterställning mellan en primär plats och en eller flera sekundära platser. Två noder kan ställas in i en delad inget kluster med SQL Server-databaser som konfigurerats i en tillgänglighetsgrupp med synkron replikering och automatisk redundans. | SQL Server 2014 & 2012 Enterprise edition
**Failover-kluster (alltid på FCI)** | SQL Server använder Windows-redundanskluster för hög tillgänglighet för lokala SQL Server-arbetsbelastningar.<br/><br/>Noder som kör instanser av SQL Server med delade diskar har konfigurerats i ett redundanskluster. Om en instans är nere växlar hello klustret toodifferent en.<br/><br/>hello kluster skyddar inte mot fel eller avbrott i den delade lagringen. hello delad disk kan implementeras med iSCSI, fibre channel eller delade vhdx-diskar. | SQL Server Enterprise Edition<br/><br/>SQL Server Standard edition (endast begränsad tootwo noder)
**Databasspegling (hög säkerhet läge)** | Skyddar en enskild databas tooa sekundär kopia. I båda hög säkerhet (synkron) och hög prestanda (asynkron) replikering lägen. Kräver inte ett failover-kluster. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise alla versioner
**Fristående SQL Server** | hello SQL Server och databasen finns på en server (fysisk eller virtuell). Om hello server är virtuell används värden kluster för hög tillgänglighet. Inga gästnivå hög tillgänglighet. | Enterprise eller Standard edition

## <a name="deployment-recommendations"></a>Distributionsrekommendationer

Den här tabellen sammanfattas våra rekommendationer för integrering av SQL Server BCDR-teknik med Site Recovery.

| **Version** | **Utgåva** | **Distribution** | **Lokal tooon lokal** | **Lokal tooAzure** |
| --- | --- | --- | --- | --- |
| SQLServer 2014 eller 2012 |Enterprise |Redundansklusterinstans |Always On-Tillgänglighetsgrupper |Always On-Tillgänglighetsgrupper |
|| Enterprise |Always On-Tillgänglighetsgrupper för hög tillgänglighet |Always On-Tillgänglighetsgrupper |Always On-Tillgänglighetsgrupper | |
|| Standard |Redundansklusterinstans (FCI) |Site Recovery replikering med lokala spegling |Site Recovery replikering med lokala spegling | |
|| Enterprise eller Standard |Fristående |Site Recovery replikering |Site Recovery replikering | |
| SQL Server 2008 R2 eller 2008 |Enterprise eller Standard |Redundansklusterinstans (FCI) |Site Recovery replikering med lokala spegling |Site Recovery replikering med lokala spegling |
|| Enterprise eller Standard |Fristående |Site Recovery replikering |Site Recovery replikering | |
| SQL Server (alla versioner) |Enterprise eller Standard |Redundansklusterinstans - DTC-program |Site Recovery replikering |Stöds inte |

## <a name="deployment-prerequisites"></a>Distributionskrav

* En lokal SQL Server-distributionen, en SQL Server-version som stöds. Vanligtvis behöver du också Active Directory för SQLServer.
* hello krav för hello scenario som du vill toodeploy. Mer information om kraven för stöd för [replikering tooAzure](site-recovery-support-matrix-to-azure.md) och [lokalt](site-recovery-support-matrix.md), och [kraven för distribution av](site-recovery-prereq.md).
* tooset återställningsåtgärder i Azure, kör hello [Azure virtuella Readiness Assessment](http://www.microsoft.com/download/details.aspx?id=40898) verktyget på virtuella datorer för din SQL Server toomake att de är kompatibla med Azure och Site Recovery.

## <a name="set-up-active-directory"></a>Konfigurera Active Directory

Konfigurera Active Directory i hello sekundära återställningsplatsen, för SQL Server toorun korrekt.

* **Litet företag**– med ett litet antal program och en enda domänkontrollant för hello lokal plats, om du vill ha toofail över hello hela platsen, rekommenderar vi du använder Site Recovery replikering tooreplicate hello-domänkontrollant toohello sekundärt datacenter eller tooAzure.
* **Medelhög toolarge enterprise**– om du har ett stort antal program, en Active Directory-skog och vill toofail över program eller arbetsbelastning, vi rekommenderar att du konfigurerar ytterligare en domänkontrollant i hello sekundärt datacenter eller i Azure. Om du använder alltid på tillgänglighet grupper toorecover tooa fjärrplatsen rekommenderar vi att du ställer in en annan ytterligare en domänkontrollant på hello sekundär plats eller i Azure, toouse hello återställa SQL Server-instansen.

hello anvisningarna i den här artikeln förutsätter att en domänkontrollant är tillgänglig på hello sekundär plats. [Läs mer](site-recovery-active-directory.md) om att skydda Active Directory med Site Recovery.


## <a name="integrate-with-sql-server-always-on-for-replication-tooazure"></a>Integrera med SQL Server alltid aktiverad för replikering tooAzure

Här är vad du behöver toodo:

1. Importera skript till Azure Automation-konto. Innehåller hello skript toofailover SQL-tillgänglighetsgrupp i en [Resource Manager virtuella](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) och en [klassiska virtuella](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Distribuera tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. Lägg till ASR-SQL-FailoverAG som en pre-åtgärd på en hello första grupp hello återställningsplan.

1. Följ hello instruktioner finns i hello skriptet toocreate ett automation-variabel tooprovide hello namn för hello Tillgänglighetsgrupper.

### <a name="steps-toodo-a-test-failover"></a>Steg toodo ett redundanstest

SQL Always On inte har inbyggt stöd testa redundans. Därför rekommenderar vi hello följande:

1. Ställ in [Azure Backup](../backup/backup-azure-vms.md) på hello virtuell dator som är värd för hello tillgänglighetsgruppsrepliker i Azure.

1. Innan test av redundansväxling hello återställningsplan, återställa hello virtuell dator från hello säkerhetskopior som gjorts i hello föregående steg.

    ![Återställa från Azure-säkerhetskopiering ](./media/site-recovery-sql/restore-from-backup.png)

1. [Framtvinga ett kvorum](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) i hello virtuell dator återställts från säkerhetskopian.

1. Uppdatera IP hello lyssnare tooan IP finns i hello nätverket.

    ![Uppdatera lyssnare IP](./media/site-recovery-sql/update-listener-ip.png)

1. Skapa lyssnare online.

    ![Anslut lyssnare](./media/site-recovery-sql/bring-listener-online.png)

1. Skapa en belastningsutjämnare med en IP-skapade under klientdelens IP-adresspool motsvarande tooeach tillgänglighetsgruppens lyssnare och hello virtuella SQL-datorn lades till i hello serverdelspool.

     ![Skapa belastningsutjämnaren - Frontend-IP-pool ](./media/site-recovery-sql/create-load-balancer1.png)

    ![Skapa belastningsutjämnaren - serverdelspool ](./media/site-recovery-sql/create-load-balancer2.png)

1. Göra ett redundanstest av hello återställningsplan.

### <a name="steps-toodo-a-failover"></a>Steg toodo en växling vid fel

När du har lagt till hello skript i hello återställningsplan och verifierade hello återställningsplan genom att göra ett redundanstest kan göra du redundans för hello återställningsplan.


## <a name="integrate-with-sql-server-always-on-for-replication-tooa-secondary-on-premises-site"></a>Integreras med SQL Server alltid aktiverad för replikering tooa sekundär lokal plats

Om hello SQL Server använder Tillgänglighetsgrupper för hög tillgänglighet (eller en FCI), bör du använda Tillgänglighetsgrupper på hello återställningsplatsen samt. Observera att detta gäller tooapps som inte använder distribuerade transaktioner.

1. [Konfigurera databaserna för](https://msdn.microsoft.com/library/hh213078.aspx) i Tillgänglighetsgrupper.
1. Skapa ett virtuellt nätverk på hello sekundär plats.
1. Konfigurera en plats-till-plats VPN-anslutning mellan hello virtuella nätverk och hello primär plats.
1. Skapa en virtuell dator på hello återställningsplatsen och installera SQL Server på den.
1. Utöka hello befintliga alltid på tillgänglighet grupper toohello nya SQL Server-VM. Konfigurera SQL Server-instansen som en asynkron replikering kopia.
1. Skapa en tillgänglighetsgruppslyssnare eller uppdatera hello befintliga lyssnaren tooinclude hello asynkron replikerade virtuella datorn.
1. Kontrollera att programmet hello servergruppen har konfigurerats med hello-lyssnaren. Om den har konfigurerats med hello Databasservernamnet, uppdatera det toouse hello lyssnare, så att du inte behöver tooreconfigure den efter hello växling vid fel.

För program som använder distribuerade transaktioner, rekommenderar vi du distribuerar Site Recovery med [VMware/fysisk server plats-till-plats-replikering](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Planera överväganden
1. Lägg till det här exemplet skriptet toohello VMM-biblioteket, på hello primära och sekundära platser.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

1. När du skapar en återställningsplan för programmet hello lägga till ett före åtgärden tooGroup 1 skriptbaserade steg, som anropar hello skriptet toofail över Tillgänglighetsgrupper.

## <a name="protect-a-standalone-sql-server"></a>Skydda en fristående SQL Server

I det här scenariot rekommenderar vi att du använder Site Recovery replikering tooprotect hello SQL Server-dator. Hej hur beror på om SQL Server är en virtuell dator eller en fysisk server, och om du vill tooreplicate tooAzure eller en sekundär lokal plats. Lär dig mer om [Site Recovery scenarier](site-recovery-overview.md).

## <a name="protect-a-sql-server-cluster-standard-editionwindows-server-2008-r2"></a>Skydda SQL Server-kluster (standard edition och Windows Server 2008 R2)

För ett kluster som kör SQL Server Standard edition eller SQL Server 2008 R2, rekommenderar vi att du använder Site Recovery replikering tooprotect SQL Server.

### <a name="on-premises-tooon-premises"></a>Lokala tooon lokala

* Om hello app använder distribuerade transaktioner vi rekommenderar att du distribuerar [Site Recovery med SAN-replikering](site-recovery-vmm-san.md) för en Hyper-V-miljö eller [VMware/fysisk server tooVMware](site-recovery-vmware-to-vmware.md) för en VMware-miljön.
* Använd hello senare metoden toorecover hello klustret som en fristående server genom att använda en lokal hög säkerhet DB spegling för icke-DTC-program.

### <a name="on-premises-tooazure"></a>Lokala tooAzure

Site Recovery inte ger gäst klusterstöd vid replikering av tooAzure. SQL Server ge inte också en lösning för katastrofåterställning för låg kostnad för Standard edition. I det här scenariot rekommenderar vi du skydda hello lokala SQL Server-kluster tooa fristående SQL Server och återställa den i Azure.

1. Konfigurera en ytterligare fristående SQL Server-instans på hello lokal plats.
1. Konfigurera hello instans tooserve som en spegling för hello-databaser du vill tooprotect. Konfigurera spegling i läget för hög säkerhet.
1. Konfigurera Site Recovery på hello lokal plats för ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) eller [VMware virtuella datorer/fysiska servrar)](site-recovery-vmware-to-azure-classic.md).
1. Använd Site Recovery replikering tooreplicate hello nya SQL Server-instansen tooAzure. Eftersom det är en hög säkerhet speglingskopia kommer kommer att synkroniseras med hello primära kluster, men det replikerade tooAzure med Site Recovery replikering.


![Standard-kluster](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Överväganden för återställning efter fel

För SQL Server Standard kluster kräver en SQL server-säkerhetskopiering och återställning från hello spegling instans toohello ursprungliga kluster med reestablishment för hello spegeln återställning efter en oplanerad redundans.

## <a name="next-steps"></a>Nästa steg
[Lär dig mer](site-recovery-components.md) om Site Recovery-arkitekturen.
