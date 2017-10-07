---
title: aaaDPM-/ Azure Backup server skydd av en SharePoint-servergrupp tooAzure | Microsoft Docs
description: "Den här artikeln innehåller en översikt över DPM-/ Azure Backup server skydd av en SharePoint-servergrupp tooAzure"
services: backup
documentationcenter: 
author: adigan
manager: Nkolli1
editor: 
ms.assetid: e0c0c252-dc1d-4072-b777-7222c13950b0
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: adigan;giridham;jimpark;trinadhk;markgal
ms.openlocfilehash: 726d59320b8d9f14b38e0f041308019eebcfb77b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-sharepoint-farm-tooazure"></a>Säkerhetskopiera en SharePoint-servergrupp tooAzure
Säkerhetskopiera en SharePoint webbservergrupp tooMicrosoft Azure med hjälp av System Center Data Protection Manager (DPM) i mycket hello samma sätt som du säkerhetskopiera andra datakällor. Azure-säkerhetskopiering ger flexibilitet i hello Säkerhetskopieringsschemat toocreate dag, vecka, månad eller årlig säkerhetskopiering punkter och ger alternativ för kvarhållning av princip för säkerhetskopiering punkter. DPM tillhandahåller hello kapaciteten toostore lokal diskkopior för snabb återställningstiden mål (RTO) och toostore kopierar tooAzure för ekonomiska och långsiktig kvarhållning.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint-versioner som stöds och relaterade skyddsscenarier
Azure Backup för DPM stöder hello följande scenarier:

| Arbetsbelastning | Version | Distribution av SharePoint | DPM-distributionstyp | DPM – System Center 2012 R2 | Skydd och återställning |
| --- | --- | --- | --- | --- | --- |
| SharePoint |SharePoint 2013, SharePoint 2010 SharePoint 2007 SharePoint 3.0 |SharePoint distribueras som en fysisk server eller VMware-Hyper-V virtuell dator <br> -------------- <br> SQL AlwaysOn |Fysisk server eller lokal Hyper-V virtuell dator |Har stöd för säkerhetskopiering tooAzure från Samlad uppdatering 5 |Skydda SharePoint-servergruppen återställningsalternativ: återställningsgruppen, databas och fil- eller listobjekt från diskåterställningspunkter.  Servergruppen och databasen återställning från återställningspunkter i Azure. |

## <a name="before-you-start"></a>Innan du börjar
Det finns några saker du behöver tooconfirm innan du säkerhetskopierar tooAzure en SharePoint-servergruppen.

### <a name="prerequisites"></a>Krav
Innan du går vidare kontrollerar du att du har uppfyllt alla hello [krav för att använda Microsoft Azure Backup](backup-azure-dpm-introduction.md#prerequisites) tooprotect arbetsbelastningar. Vissa krav omfattar: skapa ett säkerhetskopieringsvalv, hämta autentiseringsuppgifter för valv, installera Azure Backup-agenten och registrera DPM-/ Azure Backup-Server med hello valv.

### <a name="dpm-agent"></a>DPM-agenten
hello DPM-agenten måste installeras på hello-server som kör SharePoint hello-servrar som kör SQL Server och alla andra servrar som ingår i hello SharePoint-servergruppen. Mer information om hur tooset in hello protection agent finns [installationsprogrammet Protection Agent](https://technet.microsoft.com/library/hh758034\(v=sc.12\).aspx).  hello enda undantaget är att du installerar hello agenten endast på en enda webbserver för klientdel (WFE). DPM måste hello-agenten på en WFE-servern endast tooserve som hello startpunkt för skydd.

### <a name="sharepoint-farm"></a>SharePoint-grupp
För per 10 miljoner objekt i hello servergruppen måste det finnas minst 2 GB utrymme på hello volym där hello DPM mappen finns. Den här utrymme som krävs för kataloggenerering. För DPM toorecover specifika objekt (webbplatssamlingar, platser, listor, dokumentbibliotek, mappar, enskilda dokument och listobjekt) skapas kataloggenerering en lista över hello webbadresser som ingår i varje innehållsdatabas. Du kan visa hello lista över URL: er hello återställningsbara objekt i fönstret i hello **Recovery** aktivitetsområdet DPM-administratörskonsolen.

### <a name="sql-server"></a>SQL Server
DPM som körs som LocalSystem-kontot. tooback in SQL Server-databaser DPM behöver sysadmin-behörigheter på det kontot för hello-server som kör SQL Server. Ställa in NT AUTHORITY\SYSTEM också*sysadmin* på hello-server som kör SQL Server innan du säkerhetskopiera den.

Om hello SharePoint-servergruppen har SQL Server-databaser som är konfigurerade med SQL Server-alias, installerar du hello SQL Server-klientkomponenterna på hello frontwebbserver som DPM ska skydda.

### <a name="sharepoint-server"></a>SharePoint Server
Medan prestanda beror på många faktorer, till exempel storleken på SharePoint-servergruppen, som en allmän vägledning kan en DPM-server skydda en SharePoint-grupp med 25 TB.

### <a name="dpm-update-rollup-5"></a>DPM Samlad uppdatering 5
toobegin skydd av en SharePoint-servergrupp tooAzure, behöver du tooinstall DPM Samlad uppdatering 5 eller senare. Samlad uppdatering 5 innehåller hello möjlighet tooprotect tooAzure en SharePoint-grupp om hello servergruppen har konfigurerats med hjälp av SQL AlwaysOn.
Mer information finns i hello blogginlägget som introducerar [DPM Samlad uppdatering 5](http://blogs.technet.com/b/dpm/archive/2015/02/11/update-rollup-5-for-system-center-2012-r2-data-protection-manager-is-now-available.aspx)

### <a name="whats-not-supported"></a>Vad som inte stöds
* DPM som skyddar en SharePoint-servergrupp skyddar inte sökindexen eller databaser för tjänsten. Du behöver tooconfigure hello skyddet av databaserna separat.
* DPM ger inte någon säkerhetskopia av SharePoint SQL Server-databaser som finns på filresurser för skalbar filserver (SOFS).

## <a name="configure-sharepoint-protection"></a>Konfigurera SharePoint-skydd
Innan du kan använda DPM tooprotect SharePoint, måste du konfigurera hello SharePoint VSS Writer-tjänsten (WSS Writer) med hjälp av **ConfigureSharePoint.exe**.

Du kan hitta **ConfigureSharePoint.exe** i hello [DPM-installationssökvägen] \bin mappen på hello frontwebbservern. Det här verktyget innehåller hello skyddsagenten med hello autentiseringsuppgifter för hello SharePoint-servergruppen. Du kan köra den på en enskild WFE-server. Om du har flera WFE-servrar kan du bara välja en när du konfigurerar en skyddsgrupp.

### <a name="tooconfigure-hello-sharepoint-vss-writer-service"></a>tooconfigure hello SharePoint VSS Writer-tjänsten
1. Hello WFE-serven i Kommandotolken, gå för [DPM-installationsplatsen] \bin\
2. Ange ConfigureSharePoint - EnableSharePointProtection.
3. Ange hello-autentiseringsuppgifter som gruppadministratör. Det här kontot ska vara medlem i hello lokala administratörsgruppen på hello WFE-servern. Om hello servergruppsadministratören är inte en lokal administratör bevilja hello följande behörigheter på WFE-serven hello:
   * Bevilja hello WSS_Admin_WPG-gruppen fullständig kontroll toohello DPM mappen (% Program Files%\Microsoft Data Protection Manager\DPM).
   * Bevilja hello WSS_Admin_WPG-gruppen läsbehörighet toohello DPM registernyckeln (HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager).

> [!NOTE]
> Du behöver toorerun ConfigureSharePoint.exe när en ändring i hello SharePoint-autentiseringsuppgifter som gruppadministratör.
> 
> 

## <a name="back-up-a-sharepoint-farm-by-using-dpm"></a>Säkerhetskopiera en SharePoint-servergrupp med hjälp av DPM
När du har konfigurerat DPM och hello som tidigare förklarats SharePoint-servergrupp, skyddas SharePoint av DPM.

### <a name="tooprotect-a-sharepoint-farm"></a>tooprotect en SharePoint-grupp
1. Från hello **skydd** för hello DPM-administratörskonsolen klickar du på **ny**.
    ![Skyddsfliken](./media/backup-azure-backup-sharepoint/dpm-new-protection-tab.png)
2. På hello **Välj typ av Skyddsgrupp** sidan hello **Skapa ny Skyddsgrupp** guiden, Välj **servrar**, och klicka sedan på **nästa** .
   
    ![Välj Skyddsgruppen typ](./media/backup-azure-backup-sharepoint/select-protection-group-type.png)
3. På hello **Välj gruppmedlemmar** skärmen, Välj hello kryssrutan för hello SharePoint-server du vill tooprotect och på **nästa**.
   
    ![Välj gruppmedlemmar](./media/backup-azure-backup-sharepoint/select-group-members2.png)
   
   > [!NOTE]
   > Med hello DPM-agenten är installerad, kan du se hello server i hello guiden. DPM visar även dess struktur. Eftersom du körde ConfigureSharePoint.exe DPM kommunicerar med hello SharePoint VSS Writer-tjänsten och dess motsvarande SQL Server-databaser och identifierar hello SharePoint-servergrupp struktur, hello associerade innehållsdatabaser och motsvarande objekt.
   > 
   > 
4. På hello **Välj Dataskyddsmetod** anger hello namnet på hello **Skyddsgrupp**, och välj *skyddsmetod*. Klicka på **Nästa**.
   
    ![Välj dataskyddsmetod](./media/backup-azure-backup-sharepoint/select-data-protection-method1.png)
   
   > [!NOTE]
   > hello disk skyddsmetod hjälper toomeet kort återställningstiden mål. Azure är en ekonomiska och långsiktigt skydd mål jämfört med tootapes. Mer information finns i [Använd Azure Backup tooreplace infrastrukturen band](https://azure.microsoft.com/documentation/articles/backup-azure-backup-cloud-as-tape/)
   > 
   > 
5. På hello **ange kortvariga mål** sidan, Välj **Kvarhållningsintervall** och identifiera när du vill att säkerhetskopieringar toooccur.
   
    ![Ange kortsiktiga mål](./media/backup-azure-backup-sharepoint/specify-short-term-goals2.png)
   
   > [!NOTE]
   > Eftersom återställningen krävs oftast för data som är mindre än fem dagar gammal, vi valt ett Kvarhållningsintervall på fem dagar på disken och kontrollerat att hello säkerhetskopiering sker under icke-produktionstider för det här exemplet.
   > 
   > 
6. Granska hello lagring lagringspoolens allokerade diskutrymme för hello skyddsgrupp och sedan på **nästa**.
7. DPM allokerar disk space toostore och hantera repliker för varje skyddsgrupp. DPM måste nu skapa en kopia av hello valda data. Välj hur och när du vill hello replik som har skapats och klickar sedan på **nästa**.
   
    ![Välj metod för skapande av replik](./media/backup-azure-backup-sharepoint/choose-replica-creation-method.png)
   
   > [!NOTE]
   > toomake till att inte sker nätverkstrafik, Välj en tid utanför produktionstider.
   > 
   > 
8. DPM garanterar dataintegriteten genom att utföra konsekvenskontroller på hello replik. Det finns två tillgängliga alternativ. Du kan definiera ett schema toorun konsekvenskontroller eller DPM kan köra konsekvenskontroller automatiskt på hello replik när det blir inkonsekvent. Välj önskade alternativ och klicka sedan på **nästa**.
   
    ![Konsekvenskontroll](./media/backup-azure-backup-sharepoint/consistency-check.png)
9. På hello **ange Onlineskyddsdata** väljer hello SharePoint-grupp som du vill tooprotect och klicka sedan på **nästa**.
   
    ![DPM SharePoint Protection1](./media/backup-azure-backup-sharepoint/select-online-protection1.png)
10. På hello **Ange schema för Online säkerhetskopiering** , väljer önskat schema, och klickar sedan på **nästa**.
    
    ![Online_backup_schedule](./media/backup-azure-backup-sharepoint/specify-online-backup-schedule.png)
    
    > [!NOTE]
    > DPM ger högst två dagliga säkerhetskopior tooAzure vid olika tidpunkter. Azure Backup kan också styra hello mängden WAN-bandbredd som kan användas för säkerhetskopieringar i belastning och låg belastning via [Azure Backup nätverksbegränsning](https://azure.microsoft.com/en-in/documentation/articles/backup-configure-vault/#enable-network-throttling).
    > 
    > 
11. Beroende på hello säkerhetskopieringsschema som du valde på hello **ange bevarandeprincip** sidan Välj hello bevarandeprincip för dagliga, veckovisa, månatliga och årliga säkerhetskopiering punkter.
    
    ![Online_retention_policy](./media/backup-azure-backup-sharepoint/specify-online-retention.png)
    
    > [!NOTE]
    > DPM använder ett farfar-pappa-son kvarhållning-schema som du kan välja en annan bevarandeprincip för olika tidpunkter som säkerhetskopieringen.
    > 
    > 
12. Liknande toodisk en referens för inledande punkt replik måste toobe som skapats i Azure. Välj din önskade alternativ toocreate tooAzure en första säkerhetskopiering och klicka sedan på **nästa**.
    
    ![Online_replica](./media/backup-azure-backup-sharepoint/online-replication.png)
13. Granska de angivna inställningarna på hello **sammanfattning** , och klickar sedan på **Skapa grupp**. Ett meddelande visas när hello skyddsgruppen har skapats.
    
    ![Sammanfattning](./media/backup-azure-backup-sharepoint/summary.png)

## <a name="restore-a-sharepoint-item-from-disk-by-using-dpm"></a>Återställa SharePoint-objektet från disken med hjälp av DPM
I följande exempel hello, hello *återställa SharePoint-objektet* har tagits bort av misstag och måste toobe återställs.
![DPM SharePoint Protection4](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection5.png)

1. Öppna hello **DPM-administratörskonsolen**. Alla SharePoint-servergrupper som skyddas av DPM visas i hello **skydd** fliken.
   
    ![DPM SharePoint Protection3](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection4.png)
2. toobegin toorecover hello artikel, Välj hello **Recovery** fliken.
   
    ![DPM SharePoint Protection5](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection6.png)
3. Du kan söka SharePoint för *återställa SharePoint-objektet* peka intervall genom att använda en sökning med jokertecken-baserade inom en återställning.
   
    ![DPM SharePoint Protection6](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection7.png)
4. Markera hello lämpliga återställningspunkt från hello sökresultat, högerklicka hello objektet och välj sedan **återställa**.
5. Du kan också bläddra igenom olika återställningspunkter och välja en databas eller objektet toorecover. Välj **datum > återställningstid**, och välj sedan hello rätt **databasen > SharePoint-servergruppen > återställningspunkt > objektet**.
   
    ![DPM SharePoint Protection7](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection8.png)
6. Högerklicka på hello objekt och välj sedan **återställa** tooopen hello **Återställningsguiden**. Klicka på **Nästa**.
   
    ![Granska val av återställning](./media/backup-azure-backup-sharepoint/review-recovery-selection.png)
7. Välj hello typ av återställning du vill tooperform och klicka sedan på **nästa**.
   
    ![Typ av återställning](./media/backup-azure-backup-sharepoint/select-recovery-type.png)
   
   > [!NOTE]
   > Hej val av **återställa toooriginal** i hello exempel återställer hello-objektet toohello ursprungliga SharePoint-webbplatsen.
   > 
   > 
8. Välj hello **återställningsprocessen** som du vill toouse.
   
   * Välj **återställa utan en återställningsgrupp** om hello SharePoint-servergruppen inte har ändrats och hello samma som hello recovery peka som återställs.
   * Välj **återställa en återställningsgrupp** om hello SharePoint-servergruppen har ändrats sedan hello återställningspunkten skapades.
     
     ![Återställningsprocessen](./media/backup-azure-backup-sharepoint/recovery-process.png)
9. Ange en fristående SQL Server-instansen plats toorecover hello-databas tillfälligt och ger en fristående filresurs hello DPM-servern och hello-server som kör SharePoint toorecover hello-objektet.
   
    ![Mellanlagring Location1](./media/backup-azure-backup-sharepoint/staging-location1.png)
   
    DPM bifogar hello innehållsdatabasen som är värd för hello SharePoint objektet toohello tillfällig SQL Server-instans. Från hello innehållsdatabas hello DPM-servern återställer hello-objekt och placeras på hello mellanlagringsplatsen filen på hello DPM-servern. hello måste återställda objektet som finns på hello mellanlagringsplatsen för hello DPM-servern nu exporteras toobe toohello mellanlagringsplatsen på hello SharePoint-servergruppen.
   
    ![Mellanlagring Location2](./media/backup-azure-backup-sharepoint/staging-location2.png)
10. Välj **Ange återställningsalternativ**, och säkerhet inställningar toohello SharePoint-servergrupp eller använda hello säkerhetsinställningar hello återställningspunkt. Klicka på **Nästa**.
    
    ![Återställningsalternativ](./media/backup-azure-backup-sharepoint/recovery-options.png)
    
    > [!NOTE]
    > Du kan välja toothrottle hello användningen av nätverksbandbredd. Detta minskar inverkan toohello produktionsservern under produktionstider.
    > 
    > 
11. Granska hello sammanfattningsinformation och klicka sedan på **återställa** toobegin återställning av hello-filen.
    
    ![Översikt över återställning](./media/backup-azure-backup-sharepoint/recovery-summary.png)
12. Nu välja hello **övervakning** fliken i hello **DPM-administratörskonsolen** tooview hello **Status** av hello återställning.
    
    ![Status för återställning](./media/backup-azure-backup-sharepoint/recovery-monitoring.png)
    
    > [!NOTE]
    > hello filen återställs nu. Du kan uppdatera hello SharePoint site toocheck hello återställa filen.
    > 
    > 

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>Återställa en SharePoint-databas från Azure med hjälp av DPM
1. toorecover en SharePoint-innehållsdatabas Bläddra igenom olika återställningspunkter (som visas tidigare) och välj hello återställningspunkt som du vill toorestore.
   
    ![DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Dubbelklicka på hello SharePoint punkt tooshow hello tillgängliga SharePoint-katalogen återställningsinformation.
   
   > [!NOTE]
   > Eftersom hello SharePoint-servergruppen skyddas för långsiktig kvarhållning i Azure, är ingen kataloginformation (metadata) tillgänglig på hello DPM-servern. Därför när en tidpunkt i SharePoint-innehållsdatabas måste toobe återställas, måste toocatalog hello SharePoint-gruppen igen.
   > 
   > 
3. Klicka på **omkatalogiseringen**.
   
    ![DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)
   
    Hej **moln katalogisera** status öppnas.
   
    ![DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)
   
    När omkatalogiseras är klar, hello status ändras för*lyckade*. Klicka på **Stäng**.
   
    ![DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. Klicka på hello SharePoint-objekt som visas i hello DPM **Recovery** fliken tooget hello innehållsdatabasen struktur. Högerklicka på hello objekt och klicka sedan på **återställa**.
   
    ![DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Nu följer hello [recovery stegen ovan](#restore-a-sharepoint-item-from-disk-using-dpm) toorecover en SharePoint-innehållsdatabas från disken.

## <a name="faqs"></a>Vanliga frågor och svar
F: vilka versioner av DPM har stöd för SQL Server 2014 och SQL 2012 (SP2)?<br>
S: DPM 2012 R2 med Samlad uppdatering 4 stöder både.

F: kan jag återställa en SharePoint objektet toohello ursprungliga plats om SharePoint konfigureras med hjälp av SQL AlwaysOn (med skydd på disk)?<br>
S: Ja kan hello-objekt vara återställda toohello ursprungliga SharePoint-webbplatsen.

F: kan jag återställa en SharePoint-databasen toohello ursprungliga plats om SharePoint konfigureras med hjälp av SQL AlwaysOn?<br>
S: eftersom SharePoint-databaserna har konfigurerats i SQL AlwaysOn, kan de inte ändras om inte hello tillgänglighetsgruppen tas bort. Därför kan inte DPM återställa en databas toohello ursprungliga plats. Du kan återställa en SQL Server-databasen tooanother SQL Server-instans.

## <a name="next-steps"></a>Nästa steg
* Mer information om DPM-skydd av SharePoint - Se [Video serie - DPM-skydd av SharePoint](http://channel9.msdn.com/Series/Azure-Backup/Microsoft-SCDPM-Protection-of-SharePoint-1-of-2-How-to-create-a-SharePoint-Protection-Group)
* Granska [viktig information för System Center 2012 – Data Protection Manager](https://technet.microsoft.com/library/jj860415.aspx)
* Granska [viktig information för Data Protection Manager i System Center 2012 SP1](https://technet.microsoft.com/library/jj860394.aspx)

