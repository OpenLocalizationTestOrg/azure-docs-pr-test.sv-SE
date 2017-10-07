---
title: aaaAuditing i Azure SQL Data Warehouse | Microsoft Docs
description: "Kom igång med granskning i Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 08/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 948de74fa052ef206cf1aa65c0d81f084b18cb00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a><span data-ttu-id="ebb9f-103">Granskning i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="ebb9f-103">Auditing in Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ebb9f-104">Granskning</span><span class="sxs-lookup"><span data-stu-id="ebb9f-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="ebb9f-105">Hotidentifiering</span><span class="sxs-lookup"><span data-stu-id="ebb9f-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

<span data-ttu-id="ebb9f-106">SQL Data Warehouse granskning kan du toorecord händelser i din databas tooan granskningslogg i ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-106">SQL Data Warehouse auditing allows you toorecord events in your database tooan audit log in your Azure Storage account.</span></span> <span data-ttu-id="ebb9f-107">Granskning kan hjälpa dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-107">Auditing can help you maintain regulatory compliance, understand  database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span> <span data-ttu-id="ebb9f-108">SQL Data Warehouse granskning är integrerat med Microsoft Power BI för nedåt rapportering och analys.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-108">SQL Data Warehouse auditing also integrates with Microsoft Power BI for drill-down reporting and analysis.</span></span>

<span data-ttu-id="ebb9f-109">Granskningsverktyg aktivera och underlätta följer toocompliance standarder men garantera inte efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-109">Auditing tools enable and facilitate adherence toocompliance standards but don't guarantee compliance.</span></span> <span data-ttu-id="ebb9f-110">Mer information om Azure-program som stöd för överensstämmelse med standarder, finns hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Säkerhetscenter</a>.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-110">For more information about Azure programs that support standards compliance, see hello <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Trust Center</a>.</span></span>

* <span data-ttu-id="ebb9f-111">[Grunderna för granskning av databasen]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-111">[Database Auditing basics]</span></span>
* <span data-ttu-id="ebb9f-112">[Konfigurera granskning för databasen]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-112">[Set up auditing for your database]</span></span>
* <span data-ttu-id="ebb9f-113">[Analysera granskningsloggar och rapporter]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-113">[Analyze audit logs and reports]</span></span>

## <span data-ttu-id="ebb9f-114"><a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing grunderna</span><span class="sxs-lookup"><span data-stu-id="ebb9f-114"><a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing basics</span></span>
<span data-ttu-id="ebb9f-115">Granskning av SQL Data Warehouse-databas kan du:</span><span class="sxs-lookup"><span data-stu-id="ebb9f-115">SQL Data Warehouse database auditing allows you to:</span></span>

* <span data-ttu-id="ebb9f-116">**Behåll** redovisningsspårning markerade händelser.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-116">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="ebb9f-117">Du kan definiera kategorier av databasen åtgärder toobe granskas.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-117">You can define categories of database actions  toobe audited.</span></span>
* <span data-ttu-id="ebb9f-118">**Rapporten** på Databasaktivitet.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-118">**Report** on database activity.</span></span> <span data-ttu-id="ebb9f-119">Du kan använda förkonfigurerade rapporter och en instrumentpanel tooget snabbt igång med aktivitet och rapportera händelser.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-119">You can use preconfigured reports and a dashboard tooget started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="ebb9f-120">**Analysera** rapporter.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-120">**Analyze** reports.</span></span> <span data-ttu-id="ebb9f-121">Du kan hitta misstänkta händelser, ovanliga aktiviteter och trender.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-121">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="ebb9f-122">Du kan konfigurera granskning för hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="ebb9f-122">You can configure auditing for hello following event categories:</span></span>

<span data-ttu-id="ebb9f-123">**Vanlig SQL** och **parametriserade SQL** för vilka hello insamlade granskningsloggar klassificeras som</span><span class="sxs-lookup"><span data-stu-id="ebb9f-123">**Plain SQL** and **Parameterized SQL** for which hello collected audit logs are classified as</span></span>  

* <span data-ttu-id="ebb9f-124">**Åtkomst toodata**</span><span class="sxs-lookup"><span data-stu-id="ebb9f-124">**Access toodata**</span></span>
* <span data-ttu-id="ebb9f-125">**Schemaändringar (DDL)**</span><span class="sxs-lookup"><span data-stu-id="ebb9f-125">**Schema changes (DDL)**</span></span>
* <span data-ttu-id="ebb9f-126">**Dataändringar (DML)**</span><span class="sxs-lookup"><span data-stu-id="ebb9f-126">**Data changes (DML)**</span></span>
* <span data-ttu-id="ebb9f-127">**Konton, roller och behörigheter (DCL)**</span><span class="sxs-lookup"><span data-stu-id="ebb9f-127">**Accounts, roles, and permissions (DCL)**</span></span>
* <span data-ttu-id="ebb9f-128">**Lagrade proceduren**, **inloggning** och **transaktionshantering**.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-128">**Stored Procedure**, **Login** and, **Transaction Management**.</span></span>

<span data-ttu-id="ebb9f-129">För varje händelsekategori granskning av **lyckade** och **fel** åtgärder har konfigurerats separat.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-129">For each Event Category, Auditing of **Success** and **Failure** operations are configured separately.</span></span>

<span data-ttu-id="ebb9f-130">Mer information om hello aktiviteter och händelser som granskas finns hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">referens till granskningslogg Format (doc Filhämtning)</a>.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-130">For more information about hello activities and events audited, see hello <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Audit Log Format Reference (doc file download)</a>.</span></span>

<span data-ttu-id="ebb9f-131">Granskningsloggar lagras i Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-131">Audit logs are stored in your Azure storage account.</span></span> <span data-ttu-id="ebb9f-132">Du kan definiera en Granska loggen Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-132">You can define an audit log retention period.</span></span>

<span data-ttu-id="ebb9f-133">En granskningsprincip kan definieras för en viss databas eller som en standardprincip för servern.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-133">An auditing policy can be defined for a specific database or as a default server policy.</span></span> <span data-ttu-id="ebb9f-134">Granskningsprincip för servern som ska användas som standard tillämpar tooall databaser på en server som inte har en specifik åsidosättning databasen granskning princip definierad.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-134">A default server auditing policy applies tooall databases on a server, which do not have a specific overriding database auditing policy defined.</span></span>

<span data-ttu-id="ebb9f-135">Innan du konfigurerar audit granskning kontrollera om du använder en [”meddelandeklienter”.](sql-data-warehouse-auditing-downlevel-clients.md)</span><span class="sxs-lookup"><span data-stu-id="ebb9f-135">Before setting up audit auditing check if you are using a ["Downlevel Client."](sql-data-warehouse-auditing-downlevel-clients.md)</span></span>

## <span data-ttu-id="ebb9f-136"><a id="subheading-2"></a>Konfigurera granskning för databasen</span><span class="sxs-lookup"><span data-stu-id="ebb9f-136"><a id="subheading-2"></a>Set up auditing for your database</span></span>
1. <span data-ttu-id="ebb9f-137">Starta hello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a>.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-137">Launch hello <a href="https://portal.azure.com" target="_blank">Azure portal</a>.</span></span>
2. <span data-ttu-id="ebb9f-138">Gå toohello **inställningar** bladet för hello SQL Data Warehouse som du vill tooaudit.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-138">Go toohello **Settings** blade of hello SQL Data Warehouse you want tooaudit.</span></span> <span data-ttu-id="ebb9f-139">I hello **inställningar** bladet väljer **Auditing & Threat detection**.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-139">In hello **Settings** blade, select **Auditing & Threat detection**.</span></span>
   
    ![][1]
3. <span data-ttu-id="ebb9f-140">Därefter aktiverar granskning genom att klicka på hello **ON** knappen.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-140">Next, enable auditing by clicking hello **ON** button.</span></span>
   
    ![][3]
4. <span data-ttu-id="ebb9f-141">Välj i hello granskning configuration-bladet, **LAGRINGSINFORMATION** tooopen hello granska loggarna lagring-bladet.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-141">In hello auditing configuration blade, select **STORAGE DETAILS** tooopen hello Audit Logs Storage blade.</span></span> <span data-ttu-id="ebb9f-142">Välj hello Azure storage-konto där loggar sparas och hello Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-142">Select hello Azure storage account where logs will be saved and, hello retention period.</span></span> 
>[!TIP]
><span data-ttu-id="ebb9f-143">Använd hello samma lagringskonto för alla granskad databaser tooget hello mesta av hello förkonfigurerade rapporter mallar.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-143">Use hello same storage account for all audited databases tooget hello most out of hello pre-configured reports templates.</span></span>
   
    ![][4]
5. <span data-ttu-id="ebb9f-144">Klicka på hello **OK** information för knappen toosave hello lagringskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-144">Click hello **OK** button toosave hello storage details configuration.</span></span>
6. <span data-ttu-id="ebb9f-145">Under **loggning av HÄNDELSEN**, klickar du på **lyckade** och **fel** toolog alla händelser eller välja enskilda kategorier.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-145">Under **LOGGING BY EVENT**, click **SUCCESS** and **FAILURE** toolog all events, or choose individual event categories.</span></span>
7. <span data-ttu-id="ebb9f-146">Om du konfigurerar granskning för en databas, kanske du måste tooalter hello anslutningssträngen för din klient tooensure data granskning korrekt avbildas.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-146">If you are configuring Auditing for a database, you may need tooalter hello connection string of your client tooensure data auditing is correctly captured.</span></span> <span data-ttu-id="ebb9f-147">Kontrollera hello [ändra Server FDQN i anslutningssträngen för hello](sql-data-warehouse-auditing-downlevel-clients.md) avsnittet för äldre klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-147">Check hello [Modify Server FDQN in hello connection string](sql-data-warehouse-auditing-downlevel-clients.md) topic for downlevel client connections.</span></span>
8. <span data-ttu-id="ebb9f-148">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-148">Click **OK**.</span></span>

## <span data-ttu-id="ebb9f-149"><a id="subheading-3"></a>Analysera granskningsloggar och rapporter</span><span class="sxs-lookup"><span data-stu-id="ebb9f-149"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="ebb9f-150">Granskningsloggar aggregeras i en samling av Store-tabeller med en **SQLDBAuditLogs** prefix i hello Azure storage-konto som du valde under installationen.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-150">Audit logs are aggregated in a collection of Store Tables with a **SQLDBAuditLogs** prefix in hello Azure storage account you chose during setup.</span></span> <span data-ttu-id="ebb9f-151">Du kan visa loggfiler med hjälp av ett verktyg som <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Lagringsutforskaren</a>.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-151">You can view log files using a tool such as <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.</span></span>

<span data-ttu-id="ebb9f-152">Rapportmall förkonfigurerade instrumentpanelen är tillgänglig som en <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">nedladdningsbara Excel-kalkylblad</a> toohelp snabbt analysera loggdata.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-152">A preconfigured dashboard report template is available as a <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadable Excel spreadsheet</a> toohelp you quickly analyze log data.</span></span> <span data-ttu-id="ebb9f-153">toouse hello mallen på din granskningsloggar, behöver du Excel 2013 eller senare och Power Query, som du kan hämta <a href="http://www.microsoft.com/download/details.aspx?id=39379">här</a>.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-153">toouse hello template on your audit logs, you need Excel 2013 or later and Power Query, which you can download <a href="http://www.microsoft.com/download/details.aspx?id=39379">here</a>.</span></span>

<span data-ttu-id="ebb9f-154">hello mallen har fiktiva exempeldata i den och du kan ställa in Power Query tooimport din granskningsloggen direkt från Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-154">hello template has fictional sample data in it, and you can set up Power Query tooimport your audit log directly from your Azure storage account.</span></span>

## <span data-ttu-id="ebb9f-155"><a id="subheading-4"></a>Sessionsnycklar för lagring</span><span class="sxs-lookup"><span data-stu-id="ebb9f-155"><a id="subheading-4"></a>Storage key regeneration</span></span>
<span data-ttu-id="ebb9f-156">I produktion är förmodligen toorefresh lagringen nycklar med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-156">In production, you are likely toorefresh your storage keys periodically.</span></span> <span data-ttu-id="ebb9f-157">När du uppdaterar dina nycklar måste toosave hello princip.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-157">When refreshing your keys, you need toosave hello policy.</span></span> <span data-ttu-id="ebb9f-158">hello-processen är som följer:</span><span class="sxs-lookup"><span data-stu-id="ebb9f-158">hello process is as follows:</span></span>

1. <span data-ttu-id="ebb9f-159">Växla hello i hello granskning configuration bladet (beskrivs ovan i hello installationsprogrammet granskning avsnitt) **Lagringsåtkomstnyckel** från *primära* för*sekundära* och  **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-159">In hello auditing configuration blade (described above in hello setup auditing section) switch hello **Storage Access Key** from *Primary* too*Secondary* and **SAVE**.</span></span>

   ![][4]
2. <span data-ttu-id="ebb9f-160">Gå toohello lagring configuration-bladet och **återskapa** hello *primära åtkomstnyckeln*.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-160">Go toohello storage configuration blade and **regenerate** hello *Primary Access Key*.</span></span>
3. <span data-ttu-id="ebb9f-161">Gå tillbaka toohello granskning configuration bladet växeln hello **Lagringsåtkomstnyckel** från *sekundära* för*primära* och tryck på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-161">Go back toohello auditing configuration blade, switch hello **Storage Access Key** from *Secondary* too*Primary* and press **SAVE**.</span></span>
4. <span data-ttu-id="ebb9f-162">Gå tillbaka toohello lagring UI och **återskapa** hello *sekundära åtkomstnyckel* (som förberedelse för hello uppdatera nycklar för nästa cykel.</span><span class="sxs-lookup"><span data-stu-id="ebb9f-162">Go back toohello storage UI and **regenerate** hello *Secondary Access Key* (as preparation for hello next keys refresh cycle.</span></span>

## <span data-ttu-id="ebb9f-163"><a id="subheading-5"></a>Automation (PowerShell/REST API)</span><span class="sxs-lookup"><span data-stu-id="ebb9f-163"><a id="subheading-5"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="ebb9f-164">Du kan också konfigurera granskning i Azure SQL Data Warehouse med hjälp av följande verktyg för automatisering hello:</span><span class="sxs-lookup"><span data-stu-id="ebb9f-164">You can also configure auditing in Azure SQL Data Warehouse by using hello following automation tools:</span></span>

* <span data-ttu-id="ebb9f-165">**PowerShell-cmdlets**:</span><span class="sxs-lookup"><span data-stu-id="ebb9f-165">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="ebb9f-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="ebb9f-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="ebb9f-168">[Ta bort AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-168">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="ebb9f-169">[Ta bort AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-169">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="ebb9f-170">[Ange AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="ebb9f-171">[Ange AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="ebb9f-172">[Använd AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="ebb9f-172">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

<!--Anchors-->
[Grunderna för granskning av databasen]: #subheading-1
[Konfigurera granskning för databasen]: #subheading-2
[Analysera granskningsloggar och rapporter]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy