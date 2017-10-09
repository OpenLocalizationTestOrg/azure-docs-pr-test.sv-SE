---
title: "aaaGet igång med Azure SQL database auditing | Microsoft Docs"
description: "Kom igång med Azure SQL database auditing"
services: sql-database
documentationcenter: 
author: giladm
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: giladm
ms.openlocfilehash: 5494c602d702ac41992520f900c393a98cc7c989
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-auditing"></a>Kom igång med SQL-databasgranskning
Azure SQL database auditing spårar databashändelser och skriver dem tooan granskningslogg i ditt Azure storage-konto. Granskning också:

* Hjälper dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.

* Aktiverar och underlättar följer toocompliance standarder, även om den inte garantera efterlevnad. Mer information om Azure-program som stöd för överensstämmelse med standarder, finns hello [Azure Säkerhetscenter](https://azure.microsoft.com/support/trust-center/compliance/).


## <a id="subheading-1"></a>Azure SQL database granskning: översikt
Du kan använda SQL database auditing till:


* **Behåll** redovisningsspårning markerade händelser. Du kan definiera kategorier av databasen åtgärder toobe granskas.
* **Rapporten** på Databasaktivitet. Du kan använda förkonfigurerade rapporter och en instrumentpanel tooget snabbt igång med aktivitet och rapportera händelser.
* **Analysera** rapporter. Du kan hitta misstänkta händelser, ovanliga aktiviteter och trender.

Konfigurera granskning för olika typer av kategorier, enligt beskrivningen i hello [ställa in granskning för databasen](#subheading-2) avsnitt.

Granskningsloggar skrivs tooAzure Blob storage på din Azure-prenumeration.


## <a id="subheading-8"></a>Definiera servernivå kontra databasnivå granskningsprincip

En granskningsprincip kan definieras för en viss databas eller som en standardprincip för servern:

* En Serverprincipen gäller tooall befintliga och nya databaser på hello-servern.

* Om *server blobbgranskning är aktiverat*, den *alltid gäller toohello databasen* (det vill säga hello databasen kommer att granskas), oavsett hello databasen granskningsinställningar.

* Aktivera blob granskning på hello databasen i tillägg tooenabling på hello servern kommer *inte* åsidosätta eller ändra hello inställningar av hello server blobbgranskning. Båda granskningar kommer att finnas sida vid sida. Med andra ord granskas hello-databasen parallellt två gånger (en gång genom hello serverprincip och en gång hello databasen princip).

   > [!NOTE]
   > Du bör undvika att aktivera både server blob gransknings- och databasen blobbgranskning tillsammans, såvida inte:
    > * Du vill använda en annan toouse *lagringskonto* eller *kvarhållningsperioden* för en viss databas.
    > * Vill du tooaudit händelsetyper eller kategorier för en viss databas som skiljer sig från händelsetyper eller kategorier som granskas hello resten av hello databaser på hello-servern. Du kan till exempel ha tabellen infogningar som behöver toobe granskas endast för en viss databas.
   > 
   > I annat fall rekommenderar vi att du aktiverar blobbgranskning endast servernivå och lämna hello databasnivå granskning inaktiveras för alla databaser.


## <a id="subheading-2"></a>Konfigurera granskning för databasen
hello beskrivs följande avsnitt hello konfigurationen av granskning genom att använda hello Azure-portalen.

1. Gå toohello [Azure-portalen](https://portal.azure.com).
2. Gå toohello **inställningar** bladet för hello SQL-databas/SQL server som du vill tooaudit. I hello **inställningar** bladet väljer **Auditing & Threat detection**.

    <a id="auditing-screenshot"></a>![Navigeringsfönstret][1]
3. Om du föredrar tooset upp en granskningsprincip för server (vilket gäller tooall befintliga och nya databaser på den här servern) kan du välja hello **visa inställningarna för** länken i hello databasbladet för granskning. Du kan sedan visa eller ändra hello granskningsinställningar för servern.

    ![Navigeringsfönstret][2]
4. Om du föredrar tooenable blobbgranskning på hello databasnivå (i tillägget tooor i stället för servernivå granskning), för **granskning**väljer **ON**, och för **granskning typen** , Välj **Blob**.

    Om servern blobbgranskning är aktiverat, kommer att finnas hello databasen konfigurerad audit bredvid hello server blob granskning.  

    ![Navigeringsfönstret][3]
5. tooopen hello **granska loggarna lagring** bladet väljer **lagringsinformation**. Välj hello Azure storage-konto där loggar sparas, och välj sedan hello kvarhållningsperiod vilka hello gamla loggarna tas bort. Klicka sedan på **OK**. 
   >[!TIP] 
   >tooget hello mesta av hello granskning rapporter mallar, Använd hello samma lagringskonto för alla granskad databaser. 

    <a id="storage-screenshot"></a>![Navigeringsfönstret][4]
6. Om du vill toocustomize hello granskade händelser kan du göra detta via PowerShell eller hello REST API. Mer information finns i hello [Automation (PowerShell/REST-API)](#subheading-7) avsnitt.
7. När du har konfigurerat inställningarna för granskning, kan du aktivera hello funktion nya hot och konfigurera e-postmeddelanden tooreceive säkerhetsaviseringar. När du använder hotidentifiering får proaktiva varningar på avvikande databasaktiviteter som kan indikera potentiella hot mot säkerheten. Mer information finns i [komma igång med hotidentifiering](sql-database-threat-detection-get-started.md).
8. Klicka på **Spara**.





## <a id="subheading-3"></a>Analysera granskningsloggar och rapporter
Granskningsloggar samman i hello Azure storage-konto som du valde under installationen. Du kan utforska granskningsloggar genom att använda ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/).

BLOB-granskningsloggar sparas som en samling blob-filer i en behållare med namnet **sqldbauditlogs**.

Mer information om hello hierarkin hello blob granska loggarna lagringsmapp, blob namnkonventioner och loggformatet finns hello [Granska loggen Format Blobbreferens (.docx Filhämtning)](https://go.microsoft.com/fwlink/?linkid=829599).

Det finns flera metoder du kan använda tooview blob granskningsloggar:

* Använd hello [Azure-portalen](https://portal.azure.com).  Öppna hello relevanta databas. AT hello överkant hello databasen **Auditing & Threat detection** bladet, klickar du på **visa granskningsloggarna**.

    ![Navigeringsfönstret][7]

    En **granskningsloggarna** öppnas bladet som du kommer att kunna tooview hello loggar.

    - Du kan visa specifika datum genom att klicka på **Filter** hello överst i hello **granskningsloggarna** bladet.
    - Du kan växla mellan granskningsposter som har skapats av en server princip- eller princip för granskning.

       ![Navigeringsfönstret][8]

* Använd hello systemfunktion **sys.fn_get_audit_file** (T-SQL) tooreturn hello granskningsdata i tabellformat. Mer information om hur du använder den här funktionen finns hello [sys.fn_get_audit_file dokumentationen](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).


* Använd **sammanfoga granskningsfilerna** i SQL Server Management Studio (startar med SSMS 17):  
    1. Hej SSMS-menyn, Välj **filen** > **öppna** > **sammanfoga granskningsfilerna**.

        ![Navigeringsfönstret][9]
    2. Hej **lägga till granskningsfilerna** öppnas. Välj en av hello **Lägg till** alternativ att välja om toomerge audit-filer från en lokal disk eller importera dem från Azure Storage (du kommer att vara nödvändiga tooprovide dina Azure Storage-information och kontonyckel).

    3. När alla filer toomerge har lagts till, klickar du på **OK** toocomplete hello merge-operation.

    4. hello kopplade filen öppnas i SSMS, där du kan visa och analysera den, samt exportera den tooan XEL eller CSV-fil eller tooa tabell.

* Använd hello [synkronisera programmet](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) som vi har skapat. Den körs i Azure och använder Operations Management Suite (OMS) logganalys offentliga API: er toopush SQL granskningsloggar i OMS. hello synkronisera program push-meddelanden SQL granskningsloggar i OMS Log Analytics för förbrukning via hello OMS logganalys instrumentpanelen. 

* Använd Powerbi. Du kan visa och analysera granskningsdata i Power BI. Lär dig mer om [Power BI och åtkomst till en mall för nedladdningsbar](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Hämta loggfilerna från din Azure Storage blob-behållare via hello-portalen eller genom att använda ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/).
    * När du har hämtat en loggfil lokalt, du kan dubbelklicka på hello filen tooopen, visa och analysera hello loggar i SSMS.
    * Du kan också hämta flera filer samtidigt via Azure Lagringsutforskaren. Högerklicka på en viss undermapp (till exempel en undermapp som innehåller alla loggfiler för ett visst datum) och välj **Spara som** toosave i en lokal mapp.

* Ytterligare metoder:
   * När du har hämtat flera filer (eller en undermapp som innehåller loggfiler för en hel dag, enligt beskrivningen i föregående hello-objekt i listan), kan du kombinera dem lokalt som hello SSMS Merge granskningsfilerna anvisningarna ovan.

   * Visa blobbgranskning loggar programmässigt:

     * Använd hello [utökade händelser Reader](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C#-biblioteket.
     * [Fråga utökade händelser filer](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) med hjälp av PowerShell.




## <a id="subheading-5"></a>Produktionsmetoder för
<!--hello description in this section refers toopreceding screen captures.-->

### <a id="subheading-6">Granskning geo-replikerade databaser</a>
När du använder geo-replikerade databaser är möjliga tooset granskning av hello primära databasen, hello sekundär databas eller både och, beroende på vilken typ av hello granskning.

Följ dessa instruktioner (Kom ihåg att blobbgranskning kan vara aktiverat eller inaktiverat endast från hello primära databasen granskningsinställningar):

* **Primära databasen**. Aktivera blobbgranskning, antingen på hello server eller hello-databasen, enligt beskrivningen i hello [ställa in granskning för databasen](#subheading-2) avsnitt.
* **Sekundär databas**. Aktivera blobbgranskning på hello primära databasen, enligt beskrivningen i hello [ställa in granskning för databasen](#subheading-2) avsnitt. 
   * Blobbgranskning måste vara aktiverat på hello *primära databasen själva*, inte hello-server.
   * När blobbgranskning är aktiverat på hello primära databasen kan aktiveras den också på den sekundära hello-databasen.

     >[!IMPORTANT]
     >Som standard blir hello lagringsinställningarna för hello sekundär databas identiska toothose av hello primära databasen, orsakar mellan regionala trafik. Du kan undvika detta genom att aktivera blobbgranskning på hello sekundär server och konfigurera lokal lagring i inställningarna för hello sekundär server. Det åsidosätter hello lagringsplats för hello sekundära databas och resulterar i varje databas sparar granska loggarna toolocal lagring.  
<br>

### <a id="subheading-6">Sessionsnycklar för lagring</a>
I produktion är förmodligen toorefresh lagringen nycklar med jämna mellanrum. När du uppdaterar dina nycklar måste tooresave hello granskningsprincip. hello-processen är som följer:

1. Öppna hello **lagringsinformation** bladet. I hello **Lagringsåtkomstnyckel** väljer **sekundära**, och klicka på **OK**. Klicka på **spara** hello överst i hello granskning configuration-bladet.

    ![Navigeringsfönstret][5]
2. Gå toohello lagring configuration-bladet och återskapa hello primärnyckeln.

    ![Navigeringsfönstret][6]
3. Gå tillbaka toohello granskning configuration bladet växla hello lagringsåtkomstnyckel från sekundär tooprimary och klicka sedan på **OK**. Klicka på **spara** hello överst i hello granskning configuration-bladet.
4. Gå tillbaka toohello lagring configuration-bladet och generera hello sekundära åtkomstnyckeln (som förberedelse för hello nästa nyckel uppdateringscykeln).

## <a id="subheading-7"></a>Automation (PowerShell/REST API)
Du kan också konfigurera granskning i Azure SQL Database med hjälp av följande verktyg för automatisering hello:

* **PowerShell-cmdlets**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Ta bort AzureRMSqlDatabaseAuditing][103]
   * [Ta bort AzureRMSqlServerAuditing][104]
   * [Ange AzureRMSqlDatabaseAuditingPolicy][105]
   * [Ange AzureRMSqlServerAuditingPolicy][106]
   * [Använd AzureRMSqlServerAuditingPolicy][107]

   Ett exempel på skript finns [konfigurera granskning och hotidentifiering identifiering med hjälp av PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

* **REST API - blobbgranskning**:

   * [Skapa eller uppdatera databasen Blob granskningsprincip](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [Skapa eller uppdatera Server Blob granskningsprincip](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [Hämta databasen Blob granskningsprincip](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [Hämta Server Blob granskningsprincip](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [Hämta Server Blob granskning resultat](https://msdn.microsoft.com/library/azure/mt771862.aspx)


<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)  

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy
