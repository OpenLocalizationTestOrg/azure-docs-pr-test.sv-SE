---
title: Granskning i Azure SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: f851c82ebeaa647f663d499a4d327c3479e36121
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="auditing-in-azure-sql-data-warehouse"></a><span data-ttu-id="1fed5-103">Granskning i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="1fed5-103">Auditing in Azure SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1fed5-104">Granskning</span><span class="sxs-lookup"><span data-stu-id="1fed5-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="1fed5-105">Hotidentifiering</span><span class="sxs-lookup"><span data-stu-id="1fed5-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

<span data-ttu-id="1fed5-106">SQL Data Warehouse granskning kan du till post i databasen till en granskningslogg händelselogg i Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1fed5-106">SQL Data Warehouse auditing allows you to record events in your database to an audit log in your Azure Storage account.</span></span> <span data-ttu-id="1fed5-107">Granskning kan hjälpa dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.</span><span class="sxs-lookup"><span data-stu-id="1fed5-107">Auditing can help you maintain regulatory compliance, understand  database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span> <span data-ttu-id="1fed5-108">SQL Data Warehouse granskning är integrerat med Microsoft Power BI för nedåt rapportering och analys.</span><span class="sxs-lookup"><span data-stu-id="1fed5-108">SQL Data Warehouse auditing also integrates with Microsoft Power BI for drill-down reporting and analysis.</span></span>

<span data-ttu-id="1fed5-109">Granskningsverktyg aktivera och underlätta att efterlevnadsstandarder men garantera inte efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="1fed5-109">Auditing tools enable and facilitate adherence to compliance standards but don't guarantee compliance.</span></span> <span data-ttu-id="1fed5-110">Mer information om Azure-program som stöd för överensstämmelse med standarder, finns det <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Säkerhetscenter</a>.</span><span class="sxs-lookup"><span data-stu-id="1fed5-110">For more information about Azure programs that support standards compliance, see the <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Trust Center</a>.</span></span>

* <span data-ttu-id="1fed5-111">[Grunderna för granskning av databasen]</span><span class="sxs-lookup"><span data-stu-id="1fed5-111">[Database Auditing basics]</span></span>
* <span data-ttu-id="1fed5-112">[Konfigurera granskning för databasen]</span><span class="sxs-lookup"><span data-stu-id="1fed5-112">[Set up auditing for your database]</span></span>
* <span data-ttu-id="1fed5-113">[Analysera granskningsloggar och rapporter]</span><span class="sxs-lookup"><span data-stu-id="1fed5-113">[Analyze audit logs and reports]</span></span>

## <span data-ttu-id="1fed5-114"><a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing grunderna</span><span class="sxs-lookup"><span data-stu-id="1fed5-114"><a id="subheading-1"></a>Azure SQL Data Warehouse Database Auditing basics</span></span>
<span data-ttu-id="1fed5-115">Granskning av SQL Data Warehouse-databas kan du:</span><span class="sxs-lookup"><span data-stu-id="1fed5-115">SQL Data Warehouse database auditing allows you to:</span></span>

* <span data-ttu-id="1fed5-116">**Behåll** redovisningsspårning markerade händelser.</span><span class="sxs-lookup"><span data-stu-id="1fed5-116">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="1fed5-117">Du kan definiera typer av databasåtgärder som ska granskas.</span><span class="sxs-lookup"><span data-stu-id="1fed5-117">You can define categories of database actions  to be audited.</span></span>
* <span data-ttu-id="1fed5-118">**Rapporten** på Databasaktivitet.</span><span class="sxs-lookup"><span data-stu-id="1fed5-118">**Report** on database activity.</span></span> <span data-ttu-id="1fed5-119">Du kan använda förkonfigurerade rapporter och en instrumentpanel för att komma igång snabbt med aktivitet och rapportera händelser.</span><span class="sxs-lookup"><span data-stu-id="1fed5-119">You can use preconfigured reports and a dashboard to get started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="1fed5-120">**Analysera** rapporter.</span><span class="sxs-lookup"><span data-stu-id="1fed5-120">**Analyze** reports.</span></span> <span data-ttu-id="1fed5-121">Du kan hitta misstänkta händelser, ovanliga aktiviteter och trender.</span><span class="sxs-lookup"><span data-stu-id="1fed5-121">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="1fed5-122">Du kan konfigurera granskning för kategorier i följande:</span><span class="sxs-lookup"><span data-stu-id="1fed5-122">You can configure auditing for the following event categories:</span></span>

<span data-ttu-id="1fed5-123">**Vanlig SQL** och **parametriserade SQL** som samlats in granskningsloggarna klassificeras som</span><span class="sxs-lookup"><span data-stu-id="1fed5-123">**Plain SQL** and **Parameterized SQL** for which the collected audit logs are classified as</span></span>  

* <span data-ttu-id="1fed5-124">**Åtkomst till data**</span><span class="sxs-lookup"><span data-stu-id="1fed5-124">**Access to data**</span></span>
* <span data-ttu-id="1fed5-125">**Schemaändringar (DDL)**</span><span class="sxs-lookup"><span data-stu-id="1fed5-125">**Schema changes (DDL)**</span></span>
* <span data-ttu-id="1fed5-126">**Dataändringar (DML)**</span><span class="sxs-lookup"><span data-stu-id="1fed5-126">**Data changes (DML)**</span></span>
* <span data-ttu-id="1fed5-127">**Konton, roller och behörigheter (DCL)**</span><span class="sxs-lookup"><span data-stu-id="1fed5-127">**Accounts, roles, and permissions (DCL)**</span></span>
* <span data-ttu-id="1fed5-128">**Lagrade proceduren**, **inloggning** och **transaktionshantering**.</span><span class="sxs-lookup"><span data-stu-id="1fed5-128">**Stored Procedure**, **Login** and, **Transaction Management**.</span></span>

<span data-ttu-id="1fed5-129">För varje händelsekategori granskning av **lyckade** och **fel** åtgärder har konfigurerats separat.</span><span class="sxs-lookup"><span data-stu-id="1fed5-129">For each Event Category, Auditing of **Success** and **Failure** operations are configured separately.</span></span>

<span data-ttu-id="1fed5-130">Mer information om aktiviteter och händelser som granskas finns i <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">referens till granskningslogg Format (doc Filhämtning)</a>.</span><span class="sxs-lookup"><span data-stu-id="1fed5-130">For more information about the activities and events audited, see the <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Audit Log Format Reference (doc file download)</a>.</span></span>

<span data-ttu-id="1fed5-131">Granskningsloggar lagras i Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1fed5-131">Audit logs are stored in your Azure storage account.</span></span> <span data-ttu-id="1fed5-132">Du kan definiera en Granska loggen Bevarandeperiod.</span><span class="sxs-lookup"><span data-stu-id="1fed5-132">You can define an audit log retention period.</span></span>

<span data-ttu-id="1fed5-133">En granskningsprincip kan definieras för en viss databas eller som en standardprincip för servern.</span><span class="sxs-lookup"><span data-stu-id="1fed5-133">An auditing policy can be defined for a specific database or as a default server policy.</span></span> <span data-ttu-id="1fed5-134">Granskningsprincip för servern som ska användas som standard gäller för alla databaser på en server som inte har en specifik åsidosättning databasen granskning princip definierad.</span><span class="sxs-lookup"><span data-stu-id="1fed5-134">A default server auditing policy applies to all databases on a server, which do not have a specific overriding database auditing policy defined.</span></span>

<span data-ttu-id="1fed5-135">Innan du konfigurerar audit granskning kontrollera om du använder en [”meddelandeklienter”.](sql-data-warehouse-auditing-downlevel-clients.md)</span><span class="sxs-lookup"><span data-stu-id="1fed5-135">Before setting up audit auditing check if you are using a ["Downlevel Client."](sql-data-warehouse-auditing-downlevel-clients.md)</span></span>

## <span data-ttu-id="1fed5-136"><a id="subheading-2"></a>Konfigurera granskning för databasen</span><span class="sxs-lookup"><span data-stu-id="1fed5-136"><a id="subheading-2"></a>Set up auditing for your database</span></span>
1. <span data-ttu-id="1fed5-137">Starta den <a href="https://portal.azure.com" target="_blank">Azure-portalen</a>.</span><span class="sxs-lookup"><span data-stu-id="1fed5-137">Launch the <a href="https://portal.azure.com" target="_blank">Azure portal</a>.</span></span>
2. <span data-ttu-id="1fed5-138">Gå till den **inställningar** bladet för SQL Data Warehouse som du vill granska.</span><span class="sxs-lookup"><span data-stu-id="1fed5-138">Go to the **Settings** blade of the SQL Data Warehouse you want to audit.</span></span> <span data-ttu-id="1fed5-139">I den **inställningar** bladet väljer **Auditing & Threat detection**.</span><span class="sxs-lookup"><span data-stu-id="1fed5-139">In the **Settings** blade, select **Auditing & Threat detection**.</span></span>
   
    ![][1]
3. <span data-ttu-id="1fed5-140">Därefter aktiverar granskning genom att klicka på den **ON** knappen.</span><span class="sxs-lookup"><span data-stu-id="1fed5-140">Next, enable auditing by clicking the **ON** button.</span></span>
   
    ![][3]
4. <span data-ttu-id="1fed5-141">I bladet granskning configuration väljer **LAGRINGSINFORMATION** att öppna bladet granska loggarna lagring.</span><span class="sxs-lookup"><span data-stu-id="1fed5-141">In the auditing configuration blade, select **STORAGE DETAILS** to open the Audit Logs Storage blade.</span></span> <span data-ttu-id="1fed5-142">Välj Azure storage-konto där loggar sparas och kvarhållningsperioden.</span><span class="sxs-lookup"><span data-stu-id="1fed5-142">Select the Azure storage account where logs will be saved and, the retention period.</span></span> 
>[!TIP]
><span data-ttu-id="1fed5-143">Använda samma lagringskonto för alla granskad databaser för att få ut mesta av förkonfigurerade rapporter mallar.</span><span class="sxs-lookup"><span data-stu-id="1fed5-143">Use the same storage account for all audited databases to get the most out of the pre-configured reports templates.</span></span>
   
    ![][4]
5. <span data-ttu-id="1fed5-144">Klicka på den **OK** för att spara konfigurationen av information.</span><span class="sxs-lookup"><span data-stu-id="1fed5-144">Click the **OK** button to save the storage details configuration.</span></span>
6. <span data-ttu-id="1fed5-145">Under **loggning av HÄNDELSEN**, klickar du på **lyckade** och **fel** vill logga alla händelser eller enskilda kategorier.</span><span class="sxs-lookup"><span data-stu-id="1fed5-145">Under **LOGGING BY EVENT**, click **SUCCESS** and **FAILURE** to log all events, or choose individual event categories.</span></span>
7. <span data-ttu-id="1fed5-146">Om du konfigurerar granskning för en databas som du kan behöva ändra anslutningssträngen för din klient så data granskning korrekt avbildas.</span><span class="sxs-lookup"><span data-stu-id="1fed5-146">If you are configuring Auditing for a database, you may need to alter the connection string of your client to ensure data auditing is correctly captured.</span></span> <span data-ttu-id="1fed5-147">Kontrollera den [ändra Server FDQN i anslutningssträngen](sql-data-warehouse-auditing-downlevel-clients.md) avsnittet för äldre klientanslutningar.</span><span class="sxs-lookup"><span data-stu-id="1fed5-147">Check the [Modify Server FDQN in the connection string](sql-data-warehouse-auditing-downlevel-clients.md) topic for downlevel client connections.</span></span>
8. <span data-ttu-id="1fed5-148">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1fed5-148">Click **OK**.</span></span>

## <span data-ttu-id="1fed5-149"><a id="subheading-3"></a>Analysera granskningsloggar och rapporter</span><span class="sxs-lookup"><span data-stu-id="1fed5-149"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="1fed5-150">Granskningsloggar aggregeras i en samling av Store-tabeller med en **SQLDBAuditLogs** prefix i Azure storage-konto som du valde under installationen.</span><span class="sxs-lookup"><span data-stu-id="1fed5-150">Audit logs are aggregated in a collection of Store Tables with a **SQLDBAuditLogs** prefix in the Azure storage account you chose during setup.</span></span> <span data-ttu-id="1fed5-151">Du kan visa loggfiler med hjälp av ett verktyg som <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Lagringsutforskaren</a>.</span><span class="sxs-lookup"><span data-stu-id="1fed5-151">You can view log files using a tool such as <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Storage Explorer</a>.</span></span>

<span data-ttu-id="1fed5-152">Rapportmall förkonfigurerade instrumentpanelen är tillgänglig som en <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">nedladdningsbara Excel-kalkylblad</a> som hjälper dig att snabbt analysera loggdata.</span><span class="sxs-lookup"><span data-stu-id="1fed5-152">A preconfigured dashboard report template is available as a <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadable Excel spreadsheet</a> to help you quickly analyze log data.</span></span> <span data-ttu-id="1fed5-153">Om du vill använda mallen på din granskningsloggar, behöver du Excel 2013 eller senare och Power Query, som du kan hämta <a href="http://www.microsoft.com/download/details.aspx?id=39379">här</a>.</span><span class="sxs-lookup"><span data-stu-id="1fed5-153">To use the template on your audit logs, you need Excel 2013 or later and Power Query, which you can download <a href="http://www.microsoft.com/download/details.aspx?id=39379">here</a>.</span></span>

<span data-ttu-id="1fed5-154">Mallen har fiktiva exempeldata i den och du kan ställa in Power Query för att importera din granskningsloggen direkt från Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1fed5-154">The template has fictional sample data in it, and you can set up Power Query to import your audit log directly from your Azure storage account.</span></span>

## <span data-ttu-id="1fed5-155"><a id="subheading-4"></a>Sessionsnycklar för lagring</span><span class="sxs-lookup"><span data-stu-id="1fed5-155"><a id="subheading-4"></a>Storage key regeneration</span></span>
<span data-ttu-id="1fed5-156">Det är sannolikt att uppdatera din lagringsnycklar regelbundet i produktionen.</span><span class="sxs-lookup"><span data-stu-id="1fed5-156">In production, you are likely to refresh your storage keys periodically.</span></span> <span data-ttu-id="1fed5-157">När du uppdaterar dina nycklar, måste du spara principen.</span><span class="sxs-lookup"><span data-stu-id="1fed5-157">When refreshing your keys, you need to save the policy.</span></span> <span data-ttu-id="1fed5-158">Processen är följande:</span><span class="sxs-lookup"><span data-stu-id="1fed5-158">The process is as follows:</span></span>

1. <span data-ttu-id="1fed5-159">I bladet granskning konfiguration (beskrivs ovan i inställningarna för granskning avsnitt) växla den **Lagringsåtkomstnyckel** från *primära* till *sekundära* och **spara**.</span><span class="sxs-lookup"><span data-stu-id="1fed5-159">In the auditing configuration blade (described above in the setup auditing section) switch the **Storage Access Key** from *Primary* to *Secondary* and **SAVE**.</span></span>

   ![][4]
2. <span data-ttu-id="1fed5-160">Gå till bladet storage-konfiguration och **återskapa** den *primära åtkomstnyckeln*.</span><span class="sxs-lookup"><span data-stu-id="1fed5-160">Go to the storage configuration blade and **regenerate** the *Primary Access Key*.</span></span>
3. <span data-ttu-id="1fed5-161">Gå tillbaka till bladet granskning konfiguration, växlar den **Lagringsåtkomstnyckel** från *sekundära* till *primära* och tryck på **spara**.</span><span class="sxs-lookup"><span data-stu-id="1fed5-161">Go back to the auditing configuration blade, switch the **Storage Access Key** from *Secondary* to *Primary* and press **SAVE**.</span></span>
4. <span data-ttu-id="1fed5-162">Gå tillbaka till lagringen UI och **återskapa** den *sekundära åtkomstnyckel* (som förberedelse för nästa nycklar uppdateringscykel.</span><span class="sxs-lookup"><span data-stu-id="1fed5-162">Go back to the storage UI and **regenerate** the *Secondary Access Key* (as preparation for the next keys refresh cycle.</span></span>

## <span data-ttu-id="1fed5-163"><a id="subheading-5"></a>Automation (PowerShell/REST API)</span><span class="sxs-lookup"><span data-stu-id="1fed5-163"><a id="subheading-5"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="1fed5-164">Du kan också konfigurera granskning i Azure SQL Data Warehouse med hjälp av följande verktyg för automatisering:</span><span class="sxs-lookup"><span data-stu-id="1fed5-164">You can also configure auditing in Azure SQL Data Warehouse by using the following automation tools:</span></span>

* <span data-ttu-id="1fed5-165">**PowerShell-cmdlets**:</span><span class="sxs-lookup"><span data-stu-id="1fed5-165">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="1fed5-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="1fed5-166">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="1fed5-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="1fed5-167">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="1fed5-168">[Ta bort AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="1fed5-168">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="1fed5-169">[Ta bort AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="1fed5-169">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="1fed5-170">[Ange AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="1fed5-170">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="1fed5-171">[Ange AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="1fed5-171">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="1fed5-172">[Använd AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="1fed5-172">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

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