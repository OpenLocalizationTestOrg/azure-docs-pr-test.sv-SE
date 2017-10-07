---
title: "aaaReplicate en SharePoint-flernivåapp med hjälp av Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooreplicate en SharePoint-flernivåapp med hjälp av Azure Site Recovery-funktioner."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: sutalasi
ms.openlocfilehash: d856034ac2a3c95b0c1f0cf85e62c4e7a5a3210f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-sharepoint-application-for-disaster-recovery-using-azure-site-recovery"></a>Replikera en SharePoint-flernivåapp för haveriberedskap med hjälp av Azure Site Recovery

Den här artikeln beskrivs i detalj hur en SharePoint-program med tooprotect [Azure Site Recovery](site-recovery-overview.md).


## <a name="overview"></a>Översikt

Microsoft SharePoint är ett kraftfullt program som kan hjälpa en grupp eller avdelning ordna samarbeta och dela information. SharePoint kan ge intranät portaler, dokument och filhantering, samarbete, sociala nätverk, extranät, webbplatser, enterprise-sökningen och business intelligence. Det har också integrering, integrering och funktioner för automatisering av arbetsflödet. Normalt organisationer tycker att det som en nivå 1 programmet känsliga toodowntime och förlust av data.

Idag, tillhandahåller Microsoft SharePoint inte några funktioner för out box katastrofåterställning. Återställning innebär hello användning av ett vänteläge datacenter som du kan återställa hello grupp till oavsett hello-typen och skalan för en katastrofåterställning. Vänteläge Datacenter krävs för scenarier där lokala redundanta system och säkerhetskopieringar inte kan återställa från hello avbrott på hello primära datacenter.

En bra lösning för haveriberedskap ska tillåta modellering av återställningsplaner runt hello komplexa programarkitekturer, till exempel SharePoint. Det bör också ha hello möjlighet tooadd anpassade steg toohandle mappning mellan olika nivåer och därför att tillhandahålla ett enda musklick redundanskluster med en lägre RTO i hello-händelsen för en katastrofåterställning.

Den här artikeln beskrivs i detalj hur en SharePoint-program med tooprotect [Azure Site Recovery](site-recovery-overview.md). Den här artikeln beskriver bästa praxis för att replikera en tre nivåer SharePoint programmet tooAzure, gör du en katastrof återställningsgranskning och hur du kan redundans hello programmet tooAzure.

Du kan titta på hello nedan video om hur du återställer en flera nivåer program tooAzure.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/Disaster-Recovery-of-load-balanced-multi-tier-applications-using-Azure-Site-Recovery/player]


## <a name="prerequisites"></a>Krav

Innan du börjar, kontrollera att du förstår hello följande:

1. [Replikera en virtuell dator tooAzure](site-recovery-vmware-to-azure.md)
2. Hur för[utforma ett nätverk för återställning](site-recovery-network-design.md)
3. [Gör en testa redundans tooAzure](site-recovery-test-failover-to-azure.md)
4. [Gör en tooAzure för växling vid fel](site-recovery-failover.md)
5. Hur för[replikera en domänkontrollant](site-recovery-active-directory.md)
6. Hur för[replikera SQL Server](site-recovery-sql.md)

## <a name="sharepoint-architecture"></a>SharePoint-arkitektur

SharePoint kan distribueras på en eller flera servrar med hjälp av nivåindelade topologier och server roller tooimplement en servergrupp design som uppfyller specifika mål och -mål. En typisk stora, hög SharePoint-servergrupp som stöder ett stort antal samtidiga användare och ett stort antal innehållsobjekt använda tjänsten gruppering som en del av deras skalbarhet-strategi. Den här metoden innebär att du kör tjänster på dedicerade servrar, gruppera dessa tjänster och sedan skala ut hello servrar som en grupp. hello illustrerar följande topologi hello-tjänsten och server gruppering för ett tre skikt SharePoint-servergrupp. Se tooSharePoint dokumentation och produkten rad arkitekturer för detaljerad information om olika SharePoint-topologier. Du hittar mer information om distribution av SharePoint 2013 i [dokumentet](https://technet.microsoft.com/en-us/library/cc303422.aspx).



![Distribution av mönstret 1](./media/site-recovery-sharepoint/sharepointarch.png)


## <a name="site-recovery-support"></a>Site Recovery-stöd

VMware-datorer med Windows Server 2012 R2 Enterprise användes för att skapa den här artikeln. SharePoint 2013 Enterprise edition och SQL server 2014 Enterprise edition användes. Eftersom Site Recovery replikering oberoende av programmet hello rekommendationer som anges här förväntade toohold på samt följande scenarier.

### <a name="source-and-target"></a>Källa och mål

**Scenario** | **tooa sekundär plats** | **tooAzure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysisk server** | Ja | Ja

### <a name="sharepoint-versions"></a>SharePoint-versioner
hello följande SharePoint server-versioner stöds.

* SharePoint server 2013 Standard
* SharePoint server 2013 Enterprise
* SharePoint server 2016 Standard
* SharePoint server 2016 Enterprise

### <a name="things-tookeep-in-mind"></a>Saker tookeep i åtanke

Om du använder en delad disk-kluster som en nivå i ditt program så att du kommer inte att kunna toouse Site Recovery replikering tooreplicate de virtuella datorerna. Du kan använda inbyggda replikering som tillhandahålls av programmet hello och sedan använda en [återställningsplanen](site-recovery-create-recovery-plans.md) toofailover alla nivåer.

## <a name="replicating-virtual-machines"></a>Replikering av virtuella datorer

Följ [vägledningen](site-recovery-vmware-to-azure.md) toostart replikerar hello virtuella tooAzure.

* När hello replikeringen är klar, måste du gå tooeach virtuell dator för varje nivå och välj samma tillgänglighetsuppsättning i ' replikerade objekt > Inställningar > Egenskaper > beräkning och nätverk ”. Till exempel om din webbnivå har 3 virtuella datorer, se till att alla hello 3 virtuella datorer är konfigurerade toobe tillhör samma tillgänglighetsuppsättning i Azure.

    ![Ange Tillgänglighetsuppsättning](./media/site-recovery-sharepoint/select-av-set.png)

* För anvisningar om hur du skyddar Active Directory och DNS, se för[skydda Active Directory och DNS](site-recovery-active-directory.md) dokumentet.

* För anvisningar om hur du skyddar databasnivå som körs på SQLServer, se för[skydda SQL Server](site-recovery-active-directory.md) dokumentet.

## <a name="networking-configuration"></a>Nätverkskonfiguration

### <a name="network-properties"></a>Egenskaper för nätverk

* Konfigurera nätverksinställningar i Azure-portalen för hello App och webbnivå virtuella datorer, så att hello VMs hämta bifogade toohello rätt DR-nätverket efter redundans.

    ![Välj nätverk](./media/site-recovery-sharepoint/select-network.png)


* Om du använder en statisk IP-adress och ange sedan hello IP-adress som du vill hello virtuella tootake i hello **mål-IP** fält

    ![Ange statisk IP-adress](./media/site-recovery-sharepoint/set-static-ip.png)

### <a name="dns-and-traffic-routing"></a>DNS- och routning av nätverkstrafik

För platser, mot internet [skapa en trafikhanterarprofil av typen 'Priority'](../traffic-manager/traffic-manager-create-profile.md) i hello Azure-prenumeration. Och sedan konfigurera DNS- och Traffic Manager-profilen i hello följande sätt.


| **Där** | **Källa** | **Mål**|
| --- | --- | --- |
| Offentliga DNS | Offentliga DNS för SharePoint-webbplatser <br/><br/> Ex: sharepoint.contoso.com | Traffic Manager <br/><br/> contososharepoint.trafficmanager.NET |
| Lokal DNS | sharepointonprem.contoso.com | Offentliga IP-adress på hello lokal grupp |


I hello Traffic Manager-profilen [skapa hello primära platsen och återställningsplatsen slutpunkter](../traffic-manager/traffic-manager-configure-priority-routing-method.md). Använd hello externa slutpunkten för den lokala slutpunkten och offentliga IP för Azure slutpunkt. Se till att hello prioriteten högre tooon lokala slutpunkt.

Värden en testsida på en viss port (t.ex, 800) i hello SharePoint webbnivå för Traffic Manager tooautomatically identifiera tillgänglighet efter växling vid fel. Detta är en lösning om du inte kan aktivera anonym autentisering på någon av SharePoint-webbplatser.

[Konfigurera hello trafikhanterarprofil](../traffic-manager/traffic-manager-configure-priority-routing-method.md) med hello under inställningar.

* Routningsmetoden - 'Priority'
* DNS tid toolive (TTL) - 30 sekunder:
* Övervakaren slutpunktsinställningar - om du kan aktivera anonym autentisering kan du ge en viss webbplats slutpunkt. Eller så kan du använda en testsida på en viss port (t.ex, 800).

## <a name="creating-a-recovery-plan"></a>Skapa en återställningsplan

En återställningsplan kan ordningsföljd hello växling vid fel på olika nivåer i en flernivåapp, därför kan upprätthålla programkonsekvens. Följ hello nedanstående steg när du skapar en återställningsplan för ett webbprogram med flera nivåer. [Lär dig mer om hur du skapar en återställningsplan](site-recovery-runbook-automation.md#customize-the-recovery-plan).

### <a name="adding-virtual-machines-toofailover-groups"></a>Lägga till virtuella datorer toofailover grupper

1. Skapa en återställningsplan genom att lägga till hello App och webbnivå virtuella datorer.
2. Klicka på 'Anpassa' toogroup hello virtuella datorer. Som standard är alla virtuella datorer som en del av grupp 1.

    ![Anpassa RP](./media/site-recovery-sharepoint/rp-groups.png)

3. Skapar en ny grupp (Grupp2) och flytta hello webbnivå virtuella datorer till hello ny grupp. App-nivå VMs ska vara en del av grupp 1 och webbnivå VMs ska vara en del av Grupp2. Detta är tooensure som hello App nivå virtuella datorer starta först följt av Web nivå virtuella datorer.


### <a name="adding-scripts-toohello-recovery-plan"></a>Lägga till skript toohello återställningsplan

Du kan distribuera de vanligaste hello Azure Site Recovery skript i ditt Automation-konto genom att klicka på hello ”distribuera tooAzure” nedan. När du använder alla publicerade skript Se till att du följer hello riktlinjerna i hello skript.

[![Distribuera tooAzure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. Lägg till en före åtgärdsskriptet too'Group 1 toofailover SQL-tillgänglighetsgrupp. Använda hello 'ASR-SQL-FailoverAG' skript som publiceras i hello exempelskript. Se till att du följer hello riktlinjerna i hello skript och ändra hello som krävs i hello skript på lämpligt sätt.

    ![Lägg till-AG-skript för-steg-1](./media/site-recovery-sharepoint/add-ag-script-step1.png)

    ![Lägg till-AG-skript för-steg-2](./media/site-recovery-sharepoint/add-ag-script-step2.png)

2. Lägg till post åtgärd skriptet tooattach belastningsutjämning på hello misslyckades för virtuella datorer av webbnivå (Grupp2). Använda hello 'ASR AddSingleLoadBalancer' skript som publiceras i hello exempelskript. Se till att du följer hello riktlinjerna i hello skript och ändra hello som krävs i hello skript på lämpligt sätt.

    ![Lägg till-LB-skript för-steg-1](./media/site-recovery-sharepoint/add-lb-script-step1.png)

    ![Lägg till-LB-skript för-steg-2](./media/site-recovery-sharepoint/add-lb-script-step2.png)

3. Lägg till en manuell åtgärd tooupdate hello DNS-poster toopoint toohello ny grupp i Azure.

    * För internet facing platser, inga DNS-uppdateringar som är nödvändiga efter växling vid fel. Följ hello stegen som beskrivs i hello vägledning för nätverk avsnittet tooconfigure Traffic Manager. Om hello Traffic Manager-profil har konfigurerats enligt beskrivningen i föregående avsnitt i hello, lägga till en skriptet tooopen dummy port (800 i hello exempel) på hello Azure VM.

    * Lägg till en manuell åtgärd tooupdate hello DNS-poster toopoint toohello nya Web nivå virtuell dators belastningen belastningsutjämnaren IP för interna Internetriktade platser.

4. Lägg till ett manuellt steg toorestore sökprogram från en säkerhetskopia eller starta en ny search-tjänsten.

5. Följ nedanstående steg för att återställa Sök efter tjänstprogram från en säkerhetskopia.

    * Den här metoden förutsätter att en säkerhetskopia av hello Sök-tjänstprogrammet har utförts innan hello oåterkallelig händelse och hello säkerhetskopian finns på hello DR-webbplats.
    * Detta kan enkelt uppnås genom att schemalägga hello säkerhetskopiering (till exempel, en gång om dagen) och använda en procedur tooplace hello säkerhetskopiering på hello DR-plats. Kopiera procedurer kan omfatta skriptbaserade program, till exempel AzCopy (Azure kopia) eller konfigurera DFSR (Distributed File Services Replication).
    * Nu att hello SharePoint-servergruppen kör går hello Central Administration ”säkerhetskopiera och återställa” och väljer återställning. hello återställning interrogates hello säkerhetskopieringsplatsen angetts (du kanske måste tooupdate hello värdet). Välj hello Sök efter tjänstprogram säkerhetskopia som toorestore.
    * Sökningen har återställts. Tänk på att hello återställning förväntar toofind hello samma topologi (samma antal servrar) och samma hårddisk bokstäver tilldelade toothose servrar. Mer information finns i [återställa Sök-tjänstprogrammet i SharePoint 2013](https://technet.microsoft.com/library/ee748654.aspx) dokumentet.


6. Följ nedanstående steg för för att starta med ett nytt program för Search-tjänsten.

    * Den här metoden förutsätter att en säkerhetskopia av hello ”sökadministration” databasen är tillgänglig på hello DR-plats.
    * Eftersom hello andra Sök efter tjänstprogram databaser inte replikeras, måste de toobe skapas på nytt. toodo så gå tooCentral Administration och ta bort hello Sök-tjänstprogrammet. Vilka värden hello sökindex, ta bort hello indexfiler på alla servrar.
    * Återskapa hello Sök efter tjänstprogram och den här återskapar hello-databaser. Det rekommenderas toohave förberedda skript som skapar detta tjänstprogram eftersom den inte är möjligt tooperform alla åtgärder via hello GUI. Till exempel är att hello index enhetsplats och konfigurera hello söktopologi bara möjligt med SharePoint PowerShell-cmdlets. Använd hello Windows PowerShell-cmdleten återställning SPEnterpriseSearchServiceApplication och ange hello loggen levererade och replikerad databas för Search-Administration, Search_Service__DB. Denna cmdlet ger hello sökkonfigurationen, schema, hanterade egenskaper, regler och källor och skapar en standarduppsättning hello andra komponenter.
    * När hello Sök-tjänstprogrammet har skapas på nytt, måste du starta en fullständig crawl för varje innehållskälla toorestore hello Search-tjänsten. Du förlorar analytics information från hello lokalt servergrupp, till exempel sökning rekommendationer.

7. När alla hello steg har slutförts, spara hello återställningsplan och hello slutliga återställningsplan ser ut följande.

    ![Sparade RP](./media/site-recovery-sharepoint/saved-rp.png)

## <a name="doing-a-test-failover"></a>Gör ett redundanstest
Följ [vägledningen](site-recovery-test-failover-to-azure.md) toodo testa redundans.

1.  Gå tooAzure portal och välj Recovery-tjänsten-valvet.
2.  Klicka på hello återställningsplan som skapats för SharePoint-programmet.
3.  Klicka på Testa redundans.
4.  Välj återställningspunkten och den virtuella Azure-nätverket toostart hello test failover-processen.
5.  Du kan utföra din verifieringar när hello sekundära miljön är upp.
6.  När hello verifieringar har slutförts kan du klicka på ”Rensa redundanstestet' på hello återställningsplan och hello redundanstestmiljön har rensats.

Anvisningar om hur du gör testa redundans för AD och DNS, finns för[Redundanstestning för AD- och DNS](site-recovery-active-directory.md#test-failover-considerations) dokumentet.

För anvisningar om hur du gör testa redundans för SQL Always ON-Tillgänglighetsgrupper, se för[gör testa redundans för SQL Server alltid aktiverad](site-recovery-sql.md#steps-to-do-a-test-failover) dokumentet.

## <a name="doing-a-failover"></a>Genomför en redundansväxling enligt
Följ [vägledningen](site-recovery-failover.md) för att göra en växling vid fel.

1.  Gå tooAzure portal och välj Recovery Services-valvet.
2.  Klicka på hello återställningsplan som skapats för SharePoint-programmet.
3.  Klicka på 'Redundans'.
4.  Välj återställningsprocessen punkt toostart hello växling vid fel.

## <a name="next-steps"></a>Nästa steg
Du kan lära dig mer om [replikering av andra program](site-recovery-workload.md) med Site Recovery.
