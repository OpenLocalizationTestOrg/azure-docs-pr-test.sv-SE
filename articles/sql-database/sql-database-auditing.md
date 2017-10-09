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
# <a name="get-started-with-sql-database-auditing"></a><span data-ttu-id="cf53b-103">Kom igång med SQL-databasgranskning</span><span class="sxs-lookup"><span data-stu-id="cf53b-103">Get started with SQL database auditing</span></span>
<span data-ttu-id="cf53b-104">Azure SQL database auditing spårar databashändelser och skriver dem tooan granskningslogg i ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cf53b-104">Azure SQL database auditing tracks database events and writes them tooan audit log in your Azure storage account.</span></span> <span data-ttu-id="cf53b-105">Granskning också:</span><span class="sxs-lookup"><span data-stu-id="cf53b-105">Auditing also:</span></span>

* <span data-ttu-id="cf53b-106">Hjälper dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan tyda på affärsproblem eller potentiella säkerhetsöverträdelser.</span><span class="sxs-lookup"><span data-stu-id="cf53b-106">Helps you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate business concerns or suspected security violations.</span></span>

* <span data-ttu-id="cf53b-107">Aktiverar och underlättar följer toocompliance standarder, även om den inte garantera efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="cf53b-107">Enables and facilitates adherence toocompliance standards, although it doesn't guarantee compliance.</span></span> <span data-ttu-id="cf53b-108">Mer information om Azure-program som stöd för överensstämmelse med standarder, finns hello [Azure Säkerhetscenter](https://azure.microsoft.com/support/trust-center/compliance/).</span><span class="sxs-lookup"><span data-stu-id="cf53b-108">For more information about Azure programs that support standards compliance, see hello [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).</span></span>


## <span data-ttu-id="cf53b-109"><a id="subheading-1"></a>Azure SQL database granskning: översikt</span><span class="sxs-lookup"><span data-stu-id="cf53b-109"><a id="subheading-1"></a>Azure SQL database auditing overview</span></span>
<span data-ttu-id="cf53b-110">Du kan använda SQL database auditing till:</span><span class="sxs-lookup"><span data-stu-id="cf53b-110">You can use SQL database auditing to:</span></span>


* <span data-ttu-id="cf53b-111">**Behåll** redovisningsspårning markerade händelser.</span><span class="sxs-lookup"><span data-stu-id="cf53b-111">**Retain** an audit trail of selected events.</span></span> <span data-ttu-id="cf53b-112">Du kan definiera kategorier av databasen åtgärder toobe granskas.</span><span class="sxs-lookup"><span data-stu-id="cf53b-112">You can define categories of database actions toobe audited.</span></span>
* <span data-ttu-id="cf53b-113">**Rapporten** på Databasaktivitet.</span><span class="sxs-lookup"><span data-stu-id="cf53b-113">**Report** on database activity.</span></span> <span data-ttu-id="cf53b-114">Du kan använda förkonfigurerade rapporter och en instrumentpanel tooget snabbt igång med aktivitet och rapportera händelser.</span><span class="sxs-lookup"><span data-stu-id="cf53b-114">You can use preconfigured reports and a dashboard tooget started quickly with activity and event reporting.</span></span>
* <span data-ttu-id="cf53b-115">**Analysera** rapporter.</span><span class="sxs-lookup"><span data-stu-id="cf53b-115">**Analyze** reports.</span></span> <span data-ttu-id="cf53b-116">Du kan hitta misstänkta händelser, ovanliga aktiviteter och trender.</span><span class="sxs-lookup"><span data-stu-id="cf53b-116">You can find suspicious events, unusual activity, and trends.</span></span>

<span data-ttu-id="cf53b-117">Konfigurera granskning för olika typer av kategorier, enligt beskrivningen i hello [ställa in granskning för databasen](#subheading-2) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cf53b-117">You can configure auditing for different types of event categories, as explained in hello [Set up auditing for your database](#subheading-2) section.</span></span>

<span data-ttu-id="cf53b-118">Granskningsloggar skrivs tooAzure Blob storage på din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cf53b-118">Audit logs are written tooAzure Blob storage on your Azure subscription.</span></span>


## <span data-ttu-id="cf53b-119"><a id="subheading-8"></a>Definiera servernivå kontra databasnivå granskningsprincip</span><span class="sxs-lookup"><span data-stu-id="cf53b-119"><a id="subheading-8"></a>Define server-level vs. database-level auditing policy</span></span>

<span data-ttu-id="cf53b-120">En granskningsprincip kan definieras för en viss databas eller som en standardprincip för servern:</span><span class="sxs-lookup"><span data-stu-id="cf53b-120">An auditing policy can be defined for a specific database or as a default server policy:</span></span>

* <span data-ttu-id="cf53b-121">En Serverprincipen gäller tooall befintliga och nya databaser på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="cf53b-121">A server policy applies tooall existing and newly created databases on hello server.</span></span>

* <span data-ttu-id="cf53b-122">Om *server blobbgranskning är aktiverat*, den *alltid gäller toohello databasen* (det vill säga hello databasen kommer att granskas), oavsett hello databasen granskningsinställningar.</span><span class="sxs-lookup"><span data-stu-id="cf53b-122">If *server blob auditing is enabled*, it *always applies toohello database* (that is, hello database will be audited), regardless of hello database auditing settings.</span></span>

* <span data-ttu-id="cf53b-123">Aktivera blob granskning på hello databasen i tillägg tooenabling på hello servern kommer *inte* åsidosätta eller ändra hello inställningar av hello server blobbgranskning.</span><span class="sxs-lookup"><span data-stu-id="cf53b-123">Enabling blob auditing on hello database, in addition tooenabling it on hello server, will *not* override or change any of hello settings of hello server blob auditing.</span></span> <span data-ttu-id="cf53b-124">Båda granskningar kommer att finnas sida vid sida.</span><span class="sxs-lookup"><span data-stu-id="cf53b-124">Both audits will exist side by side.</span></span> <span data-ttu-id="cf53b-125">Med andra ord granskas hello-databasen parallellt två gånger (en gång genom hello serverprincip och en gång hello databasen princip).</span><span class="sxs-lookup"><span data-stu-id="cf53b-125">In other words, hello database will be audited twice in parallel (once by hello server policy and once by hello database policy).</span></span>

   > [!NOTE]
   > <span data-ttu-id="cf53b-126">Du bör undvika att aktivera både server blob gransknings- och databasen blobbgranskning tillsammans, såvida inte:</span><span class="sxs-lookup"><span data-stu-id="cf53b-126">You should avoid enabling both server blob auditing and database blob auditing together, unless:</span></span>
    > * <span data-ttu-id="cf53b-127">Du vill använda en annan toouse *lagringskonto* eller *kvarhållningsperioden* för en viss databas.</span><span class="sxs-lookup"><span data-stu-id="cf53b-127">You want toouse a different *storage account* or *retention period* for a specific database.</span></span>
    > * <span data-ttu-id="cf53b-128">Vill du tooaudit händelsetyper eller kategorier för en viss databas som skiljer sig från händelsetyper eller kategorier som granskas hello resten av hello databaser på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="cf53b-128">You want tooaudit event types or categories for a specific database that differ from event types or categories that are being audited for hello rest of hello databases on hello server.</span></span> <span data-ttu-id="cf53b-129">Du kan till exempel ha tabellen infogningar som behöver toobe granskas endast för en viss databas.</span><span class="sxs-lookup"><span data-stu-id="cf53b-129">For example, you might have table inserts that need toobe audited only for a specific database.</span></span>
   > 
   > <span data-ttu-id="cf53b-130">I annat fall rekommenderar vi att du aktiverar blobbgranskning endast servernivå och lämna hello databasnivå granskning inaktiveras för alla databaser.</span><span class="sxs-lookup"><span data-stu-id="cf53b-130">Otherwise, we recommended that you enable only server-level blob auditing and leave hello database-level auditing disabled for all databases.</span></span>


## <span data-ttu-id="cf53b-131"><a id="subheading-2"></a>Konfigurera granskning för databasen</span><span class="sxs-lookup"><span data-stu-id="cf53b-131"><a id="subheading-2"></a>Set up auditing for your database</span></span>
<span data-ttu-id="cf53b-132">hello beskrivs följande avsnitt hello konfigurationen av granskning genom att använda hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cf53b-132">hello following section describes hello configuration of auditing using hello Azure portal.</span></span>

1. <span data-ttu-id="cf53b-133">Gå toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cf53b-133">Go toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="cf53b-134">Gå toohello **inställningar** bladet för hello SQL-databas/SQL server som du vill tooaudit.</span><span class="sxs-lookup"><span data-stu-id="cf53b-134">Go toohello **Settings** blade of hello SQL database/SQL server you want tooaudit.</span></span> <span data-ttu-id="cf53b-135">I hello **inställningar** bladet väljer **Auditing & Threat detection**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-135">In hello **Settings** blade, select **Auditing & Threat detection**.</span></span>

    <span data-ttu-id="cf53b-136"><a id="auditing-screenshot"></a>![Navigeringsfönstret][1]</span><span class="sxs-lookup"><span data-stu-id="cf53b-136"><a id="auditing-screenshot"></a> ![Navigation pane][1]</span></span>
3. <span data-ttu-id="cf53b-137">Om du föredrar tooset upp en granskningsprincip för server (vilket gäller tooall befintliga och nya databaser på den här servern) kan du välja hello **visa inställningarna för** länken i hello databasbladet för granskning.</span><span class="sxs-lookup"><span data-stu-id="cf53b-137">If you prefer tooset up a server auditing policy (which will apply tooall existing and newly created databases on this server), you can select hello **View server settings** link in hello database auditing blade.</span></span> <span data-ttu-id="cf53b-138">Du kan sedan visa eller ändra hello granskningsinställningar för servern.</span><span class="sxs-lookup"><span data-stu-id="cf53b-138">You can then view or modify hello server auditing settings.</span></span>

    ![Navigeringsfönstret][2]
4. <span data-ttu-id="cf53b-140">Om du föredrar tooenable blobbgranskning på hello databasnivå (i tillägget tooor i stället för servernivå granskning), för **granskning**väljer **ON**, och för **granskning typen** , Välj **Blob**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-140">If you prefer tooenable blob auditing on hello database level (in addition tooor instead of server-level auditing), for **Auditing**, select **ON**, and for **Auditing type**, select  **Blob**.</span></span>

    <span data-ttu-id="cf53b-141">Om servern blobbgranskning är aktiverat, kommer att finnas hello databasen konfigurerad audit bredvid hello server blob granskning.</span><span class="sxs-lookup"><span data-stu-id="cf53b-141">If server blob auditing is enabled, hello database-configured audit will exist side by side with hello server blob audit.</span></span>  

    ![Navigeringsfönstret][3]
5. <span data-ttu-id="cf53b-143">tooopen hello **granska loggarna lagring** bladet väljer **lagringsinformation**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-143">tooopen hello **Audit Logs Storage** blade, select **Storage Details**.</span></span> <span data-ttu-id="cf53b-144">Välj hello Azure storage-konto där loggar sparas, och välj sedan hello kvarhållningsperiod vilka hello gamla loggarna tas bort.</span><span class="sxs-lookup"><span data-stu-id="cf53b-144">Select hello Azure storage account where logs will be saved, and then select hello retention period, after which hello old logs will be deleted.</span></span> <span data-ttu-id="cf53b-145">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-145">Then click **OK**.</span></span> 
   >[!TIP] 
   ><span data-ttu-id="cf53b-146">tooget hello mesta av hello granskning rapporter mallar, Använd hello samma lagringskonto för alla granskad databaser.</span><span class="sxs-lookup"><span data-stu-id="cf53b-146">tooget hello most out of hello auditing reports templates, use hello same storage account for all audited databases.</span></span> 

    <span data-ttu-id="cf53b-147"><a id="storage-screenshot"></a>![Navigeringsfönstret][4]</span><span class="sxs-lookup"><span data-stu-id="cf53b-147"><a id="storage-screenshot"></a> ![Navigation pane][4]</span></span>
6. <span data-ttu-id="cf53b-148">Om du vill toocustomize hello granskade händelser kan du göra detta via PowerShell eller hello REST API.</span><span class="sxs-lookup"><span data-stu-id="cf53b-148">If you want toocustomize hello audited events, you can do this via PowerShell or hello REST API.</span></span> <span data-ttu-id="cf53b-149">Mer information finns i hello [Automation (PowerShell/REST-API)](#subheading-7) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cf53b-149">For more details, see hello [Automation (PowerShell/REST API)](#subheading-7) section.</span></span>
7. <span data-ttu-id="cf53b-150">När du har konfigurerat inställningarna för granskning, kan du aktivera hello funktion nya hot och konfigurera e-postmeddelanden tooreceive säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="cf53b-150">After you've configured your auditing settings, you can turn on hello new threat detection feature and configure emails tooreceive security alerts.</span></span> <span data-ttu-id="cf53b-151">När du använder hotidentifiering får proaktiva varningar på avvikande databasaktiviteter som kan indikera potentiella hot mot säkerheten.</span><span class="sxs-lookup"><span data-stu-id="cf53b-151">When you use threat detection, you receive proactive alerts on anomalous database activities that can indicate potential security threats.</span></span> <span data-ttu-id="cf53b-152">Mer information finns i [komma igång med hotidentifiering](sql-database-threat-detection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cf53b-152">For more details, see [Getting started with threat detection](sql-database-threat-detection-get-started.md).</span></span>
8. <span data-ttu-id="cf53b-153">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-153">Click **Save**.</span></span>





## <span data-ttu-id="cf53b-154"><a id="subheading-3"></a>Analysera granskningsloggar och rapporter</span><span class="sxs-lookup"><span data-stu-id="cf53b-154"><a id="subheading-3"></a>Analyze audit logs and reports</span></span>
<span data-ttu-id="cf53b-155">Granskningsloggar samman i hello Azure storage-konto som du valde under installationen.</span><span class="sxs-lookup"><span data-stu-id="cf53b-155">Audit logs are aggregated in hello Azure storage account you chose during setup.</span></span> <span data-ttu-id="cf53b-156">Du kan utforska granskningsloggar genom att använda ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="cf53b-156">You can explore audit logs by using a tool such as [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

<span data-ttu-id="cf53b-157">BLOB-granskningsloggar sparas som en samling blob-filer i en behållare med namnet **sqldbauditlogs**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-157">Blob auditing logs are saved as a collection of blob files within a container named **sqldbauditlogs**.</span></span>

<span data-ttu-id="cf53b-158">Mer information om hello hierarkin hello blob granska loggarna lagringsmapp, blob namnkonventioner och loggformatet finns hello [Granska loggen Format Blobbreferens (.docx Filhämtning)](https://go.microsoft.com/fwlink/?linkid=829599).</span><span class="sxs-lookup"><span data-stu-id="cf53b-158">For further details about hello hierarchy of hello blob audit logs storage folder, blob naming conventions, and log format, see hello [Blob Audit Log Format Reference (.docx file download)](https://go.microsoft.com/fwlink/?linkid=829599).</span></span>

<span data-ttu-id="cf53b-159">Det finns flera metoder du kan använda tooview blob granskningsloggar:</span><span class="sxs-lookup"><span data-stu-id="cf53b-159">There are several methods you can use tooview blob auditing logs:</span></span>

* <span data-ttu-id="cf53b-160">Använd hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cf53b-160">Use hello [Azure portal](https://portal.azure.com).</span></span>  <span data-ttu-id="cf53b-161">Öppna hello relevanta databas.</span><span class="sxs-lookup"><span data-stu-id="cf53b-161">Open hello relevant database.</span></span> <span data-ttu-id="cf53b-162">AT hello överkant hello databasen **Auditing & Threat detection** bladet, klickar du på **visa granskningsloggarna**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-162">At hello top of hello database's **Auditing & Threat detection** blade, click **View audit logs**.</span></span>

    ![Navigeringsfönstret][7]

    <span data-ttu-id="cf53b-164">En **granskningsloggarna** öppnas bladet som du kommer att kunna tooview hello loggar.</span><span class="sxs-lookup"><span data-stu-id="cf53b-164">An **Audit records** blade opens, from which you'll be able tooview hello logs.</span></span>

    - <span data-ttu-id="cf53b-165">Du kan visa specifika datum genom att klicka på **Filter** hello överst i hello **granskningsloggarna** bladet.</span><span class="sxs-lookup"><span data-stu-id="cf53b-165">You can view specific dates by clicking **Filter** at hello top of hello **Audit records** blade.</span></span>
    - <span data-ttu-id="cf53b-166">Du kan växla mellan granskningsposter som har skapats av en server princip- eller princip för granskning.</span><span class="sxs-lookup"><span data-stu-id="cf53b-166">You can switch between audit records that were created by a server policy or database policy audit.</span></span>

       ![Navigeringsfönstret][8]

* <span data-ttu-id="cf53b-168">Använd hello systemfunktion **sys.fn_get_audit_file** (T-SQL) tooreturn hello granskningsdata i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="cf53b-168">Use hello system function **sys.fn_get_audit_file** (T-SQL) tooreturn hello audit log data in tabular format.</span></span> <span data-ttu-id="cf53b-169">Mer information om hur du använder den här funktionen finns hello [sys.fn_get_audit_file dokumentationen](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="cf53b-169">For more information on using this function, see hello [sys.fn_get_audit_file documentation](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).</span></span>


* <span data-ttu-id="cf53b-170">Använd **sammanfoga granskningsfilerna** i SQL Server Management Studio (startar med SSMS 17):</span><span class="sxs-lookup"><span data-stu-id="cf53b-170">Use **Merge Audit Files** in SQL Server Management Studio (starting with SSMS 17):</span></span>  
    1. <span data-ttu-id="cf53b-171">Hej SSMS-menyn, Välj **filen** > **öppna** > **sammanfoga granskningsfilerna**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-171">From hello SSMS menu, select **File** > **Open** > **Merge Audit Files**.</span></span>

        ![Navigeringsfönstret][9]
    2. <span data-ttu-id="cf53b-173">Hej **lägga till granskningsfilerna** öppnas.</span><span class="sxs-lookup"><span data-stu-id="cf53b-173">hello **Add Audit Files** dialog box opens.</span></span> <span data-ttu-id="cf53b-174">Välj en av hello **Lägg till** alternativ att välja om toomerge audit-filer från en lokal disk eller importera dem från Azure Storage (du kommer att vara nödvändiga tooprovide dina Azure Storage-information och kontonyckel).</span><span class="sxs-lookup"><span data-stu-id="cf53b-174">Select one of hello **Add** options to choose whether toomerge audit files from a local disk or import them from Azure Storage (you will be required tooprovide your Azure Storage details and account key).</span></span>

    3. <span data-ttu-id="cf53b-175">När alla filer toomerge har lagts till, klickar du på **OK** toocomplete hello merge-operation.</span><span class="sxs-lookup"><span data-stu-id="cf53b-175">After all files toomerge have been added, click **OK** toocomplete hello merge operation.</span></span>

    4. <span data-ttu-id="cf53b-176">hello kopplade filen öppnas i SSMS, där du kan visa och analysera den, samt exportera den tooan XEL eller CSV-fil eller tooa tabell.</span><span class="sxs-lookup"><span data-stu-id="cf53b-176">hello merged file opens in SSMS, where you can view and analyze it, as well as export it tooan XEL or CSV file or tooa table.</span></span>

* <span data-ttu-id="cf53b-177">Använd hello [synkronisera programmet](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) som vi har skapat.</span><span class="sxs-lookup"><span data-stu-id="cf53b-177">Use hello [sync application](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) that we have created.</span></span> <span data-ttu-id="cf53b-178">Den körs i Azure och använder Operations Management Suite (OMS) logganalys offentliga API: er toopush SQL granskningsloggar i OMS.</span><span class="sxs-lookup"><span data-stu-id="cf53b-178">It runs in Azure and utilizes Operations Management Suite (OMS) Log Analytics public APIs toopush SQL audit logs into OMS.</span></span> <span data-ttu-id="cf53b-179">hello synkronisera program push-meddelanden SQL granskningsloggar i OMS Log Analytics för förbrukning via hello OMS logganalys instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="cf53b-179">hello sync application pushes SQL audit logs into OMS Log Analytics for consumption via hello OMS Log Analytics dashboard.</span></span> 

* <span data-ttu-id="cf53b-180">Använd Powerbi.</span><span class="sxs-lookup"><span data-stu-id="cf53b-180">Use Power BI.</span></span> <span data-ttu-id="cf53b-181">Du kan visa och analysera granskningsdata i Power BI.</span><span class="sxs-lookup"><span data-stu-id="cf53b-181">You can view and analyze audit log data in Power BI.</span></span> <span data-ttu-id="cf53b-182">Lär dig mer om [Power BI och åtkomst till en mall för nedladdningsbar](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).</span><span class="sxs-lookup"><span data-stu-id="cf53b-182">Learn more about [Power BI, and access a downloadable template](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).</span></span>

* <span data-ttu-id="cf53b-183">Hämta loggfilerna från din Azure Storage blob-behållare via hello-portalen eller genom att använda ett verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="cf53b-183">Download log files from your Azure Storage blob container via hello portal or by using a tool such as [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
    * <span data-ttu-id="cf53b-184">När du har hämtat en loggfil lokalt, du kan dubbelklicka på hello filen tooopen, visa och analysera hello loggar i SSMS.</span><span class="sxs-lookup"><span data-stu-id="cf53b-184">After you have downloaded a log file locally, you can double-click hello file tooopen, view, and analyze hello logs in SSMS.</span></span>
    * <span data-ttu-id="cf53b-185">Du kan också hämta flera filer samtidigt via Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="cf53b-185">You can also download multiple files simultaneously via Azure Storage Explorer.</span></span> <span data-ttu-id="cf53b-186">Högerklicka på en viss undermapp (till exempel en undermapp som innehåller alla loggfiler för ett visst datum) och välj **Spara som** toosave i en lokal mapp.</span><span class="sxs-lookup"><span data-stu-id="cf53b-186">Right-click a specific subfolder (for example, a subfolder that includes all log files for a specific date) and select **Save as** toosave in a local folder.</span></span>

* <span data-ttu-id="cf53b-187">Ytterligare metoder:</span><span class="sxs-lookup"><span data-stu-id="cf53b-187">Additional methods:</span></span>
   * <span data-ttu-id="cf53b-188">När du har hämtat flera filer (eller en undermapp som innehåller loggfiler för en hel dag, enligt beskrivningen i föregående hello-objekt i listan), kan du kombinera dem lokalt som hello SSMS Merge granskningsfilerna anvisningarna ovan.</span><span class="sxs-lookup"><span data-stu-id="cf53b-188">After downloading several files (or a subfolder that includes log files for an entire day, as described in hello previous item in this list), you can merge them locally as described in hello SSMS Merge Audit Files instructions described earlier.</span></span>

   * <span data-ttu-id="cf53b-189">Visa blobbgranskning loggar programmässigt:</span><span class="sxs-lookup"><span data-stu-id="cf53b-189">View blob auditing logs programmatically:</span></span>

     * <span data-ttu-id="cf53b-190">Använd hello [utökade händelser Reader](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C#-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="cf53b-190">Use hello [Extended Events Reader](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C# library.</span></span>
     * <span data-ttu-id="cf53b-191">[Fråga utökade händelser filer](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cf53b-191">[Query Extended Events Files](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) by using PowerShell.</span></span>




## <span data-ttu-id="cf53b-192"><a id="subheading-5"></a>Produktionsmetoder för</span><span class="sxs-lookup"><span data-stu-id="cf53b-192"><a id="subheading-5"></a>Production practices</span></span>
<!--hello description in this section refers toopreceding screen captures.-->

### <span data-ttu-id="cf53b-193"><a id="subheading-6">Granskning geo-replikerade databaser</a></span><span class="sxs-lookup"><span data-stu-id="cf53b-193"><a id="subheading-6">Auditing geo-replicated databases</a></span></span>
<span data-ttu-id="cf53b-194">När du använder geo-replikerade databaser är möjliga tooset granskning av hello primära databasen, hello sekundär databas eller både och, beroende på vilken typ av hello granskning.</span><span class="sxs-lookup"><span data-stu-id="cf53b-194">When you use geo-replicated databases, it is possible tooset up auditing on either hello primary database, hello secondary database, or both, depending on hello audit type.</span></span>

<span data-ttu-id="cf53b-195">Följ dessa instruktioner (Kom ihåg att blobbgranskning kan vara aktiverat eller inaktiverat endast från hello primära databasen granskningsinställningar):</span><span class="sxs-lookup"><span data-stu-id="cf53b-195">Follow these instructions (remember that blob auditing can be turned on or off only from hello primary database auditing settings):</span></span>

* <span data-ttu-id="cf53b-196">**Primära databasen**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-196">**Primary database**.</span></span> <span data-ttu-id="cf53b-197">Aktivera blobbgranskning, antingen på hello server eller hello-databasen, enligt beskrivningen i hello [ställa in granskning för databasen](#subheading-2) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cf53b-197">Turn on blob auditing, either on hello server or on hello database itself, as described in hello [Set up auditing for your database](#subheading-2) section.</span></span>
* <span data-ttu-id="cf53b-198">**Sekundär databas**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-198">**Secondary database**.</span></span> <span data-ttu-id="cf53b-199">Aktivera blobbgranskning på hello primära databasen, enligt beskrivningen i hello [ställa in granskning för databasen](#subheading-2) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cf53b-199">Turn on blob auditing on hello primary database, as described in hello [Set up auditing for your database](#subheading-2) section.</span></span> 
   * <span data-ttu-id="cf53b-200">Blobbgranskning måste vara aktiverat på hello *primära databasen själva*, inte hello-server.</span><span class="sxs-lookup"><span data-stu-id="cf53b-200">Blob auditing must be enabled on hello *primary database itself*, not hello server.</span></span>
   * <span data-ttu-id="cf53b-201">När blobbgranskning är aktiverat på hello primära databasen kan aktiveras den också på den sekundära hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="cf53b-201">After blob auditing is enabled on hello primary database, it will also become enabled on hello secondary database.</span></span>

     >[!IMPORTANT]
     ><span data-ttu-id="cf53b-202">Som standard blir hello lagringsinställningarna för hello sekundär databas identiska toothose av hello primära databasen, orsakar mellan regionala trafik.</span><span class="sxs-lookup"><span data-stu-id="cf53b-202">By default, hello storage settings for hello secondary database will be identical toothose of hello primary database, causing cross-regional traffic.</span></span> <span data-ttu-id="cf53b-203">Du kan undvika detta genom att aktivera blobbgranskning på hello sekundär server och konfigurera lokal lagring i inställningarna för hello sekundär server.</span><span class="sxs-lookup"><span data-stu-id="cf53b-203">You can avoid this by enabling blob auditing on hello secondary server and configuring local storage in hello secondary server storage settings.</span></span> <span data-ttu-id="cf53b-204">Det åsidosätter hello lagringsplats för hello sekundära databas och resulterar i varje databas sparar granska loggarna toolocal lagring.</span><span class="sxs-lookup"><span data-stu-id="cf53b-204">This will override hello storage location for hello secondary database and result in each database saving its audit logs toolocal storage.</span></span>  
<br>

### <span data-ttu-id="cf53b-205"><a id="subheading-6">Sessionsnycklar för lagring</a></span><span class="sxs-lookup"><span data-stu-id="cf53b-205"><a id="subheading-6">Storage key regeneration</a></span></span>
<span data-ttu-id="cf53b-206">I produktion är förmodligen toorefresh lagringen nycklar med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="cf53b-206">In production, you are likely toorefresh your storage keys periodically.</span></span> <span data-ttu-id="cf53b-207">När du uppdaterar dina nycklar måste tooresave hello granskningsprincip.</span><span class="sxs-lookup"><span data-stu-id="cf53b-207">When refreshing your keys, you need tooresave hello auditing policy.</span></span> <span data-ttu-id="cf53b-208">hello-processen är som följer:</span><span class="sxs-lookup"><span data-stu-id="cf53b-208">hello process is as follows:</span></span>

1. <span data-ttu-id="cf53b-209">Öppna hello **lagringsinformation** bladet.</span><span class="sxs-lookup"><span data-stu-id="cf53b-209">Open hello **Storage Details** blade.</span></span> <span data-ttu-id="cf53b-210">I hello **Lagringsåtkomstnyckel** väljer **sekundära**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-210">In hello **Storage Access Key** box, select **Secondary**, and click **OK**.</span></span> <span data-ttu-id="cf53b-211">Klicka på **spara** hello överst i hello granskning configuration-bladet.</span><span class="sxs-lookup"><span data-stu-id="cf53b-211">Then click **Save** at hello top of hello auditing configuration blade.</span></span>

    ![Navigeringsfönstret][5]
2. <span data-ttu-id="cf53b-213">Gå toohello lagring configuration-bladet och återskapa hello primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="cf53b-213">Go toohello storage configuration blade and regenerate hello primary access key.</span></span>

    ![Navigeringsfönstret][6]
3. <span data-ttu-id="cf53b-215">Gå tillbaka toohello granskning configuration bladet växla hello lagringsåtkomstnyckel från sekundär tooprimary och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf53b-215">Go back toohello auditing configuration blade, switch hello storage access key from secondary tooprimary, and then click **OK**.</span></span> <span data-ttu-id="cf53b-216">Klicka på **spara** hello överst i hello granskning configuration-bladet.</span><span class="sxs-lookup"><span data-stu-id="cf53b-216">Then click **Save** at hello top of hello auditing configuration blade.</span></span>
4. <span data-ttu-id="cf53b-217">Gå tillbaka toohello lagring configuration-bladet och generera hello sekundära åtkomstnyckeln (som förberedelse för hello nästa nyckel uppdateringscykeln).</span><span class="sxs-lookup"><span data-stu-id="cf53b-217">Go back toohello storage configuration blade and regenerate hello secondary access key (in preparation for hello next key's refresh cycle).</span></span>

## <span data-ttu-id="cf53b-218"><a id="subheading-7"></a>Automation (PowerShell/REST API)</span><span class="sxs-lookup"><span data-stu-id="cf53b-218"><a id="subheading-7"></a>Automation (PowerShell/REST API)</span></span>
<span data-ttu-id="cf53b-219">Du kan också konfigurera granskning i Azure SQL Database med hjälp av följande verktyg för automatisering hello:</span><span class="sxs-lookup"><span data-stu-id="cf53b-219">You can also configure auditing in Azure SQL Database by using hello following automation tools:</span></span>

* <span data-ttu-id="cf53b-220">**PowerShell-cmdlets**:</span><span class="sxs-lookup"><span data-stu-id="cf53b-220">**PowerShell cmdlets**:</span></span>

   * <span data-ttu-id="cf53b-221">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span><span class="sxs-lookup"><span data-stu-id="cf53b-221">[Get-AzureRMSqlDatabaseAuditingPolicy][101]</span></span>
   * <span data-ttu-id="cf53b-222">[Get-AzureRMSqlServerAuditingPolicy][102]</span><span class="sxs-lookup"><span data-stu-id="cf53b-222">[Get-AzureRMSqlServerAuditingPolicy][102]</span></span>
   * <span data-ttu-id="cf53b-223">[Ta bort AzureRMSqlDatabaseAuditing][103]</span><span class="sxs-lookup"><span data-stu-id="cf53b-223">[Remove-AzureRMSqlDatabaseAuditing][103]</span></span>
   * <span data-ttu-id="cf53b-224">[Ta bort AzureRMSqlServerAuditing][104]</span><span class="sxs-lookup"><span data-stu-id="cf53b-224">[Remove-AzureRMSqlServerAuditing][104]</span></span>
   * <span data-ttu-id="cf53b-225">[Ange AzureRMSqlDatabaseAuditingPolicy][105]</span><span class="sxs-lookup"><span data-stu-id="cf53b-225">[Set-AzureRMSqlDatabaseAuditingPolicy][105]</span></span>
   * <span data-ttu-id="cf53b-226">[Ange AzureRMSqlServerAuditingPolicy][106]</span><span class="sxs-lookup"><span data-stu-id="cf53b-226">[Set-AzureRMSqlServerAuditingPolicy][106]</span></span>
   * <span data-ttu-id="cf53b-227">[Använd AzureRMSqlServerAuditingPolicy][107]</span><span class="sxs-lookup"><span data-stu-id="cf53b-227">[Use-AzureRMSqlServerAuditingPolicy][107]</span></span>

   <span data-ttu-id="cf53b-228">Ett exempel på skript finns [konfigurera granskning och hotidentifiering identifiering med hjälp av PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="cf53b-228">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

* <span data-ttu-id="cf53b-229">**REST API - blobbgranskning**:</span><span class="sxs-lookup"><span data-stu-id="cf53b-229">**REST API - Blob auditing**:</span></span>

   * [<span data-ttu-id="cf53b-230">Skapa eller uppdatera databasen Blob granskningsprincip</span><span class="sxs-lookup"><span data-stu-id="cf53b-230">Create or Update Database Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [<span data-ttu-id="cf53b-231">Skapa eller uppdatera Server Blob granskningsprincip</span><span class="sxs-lookup"><span data-stu-id="cf53b-231">Create or Update Server Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [<span data-ttu-id="cf53b-232">Hämta databasen Blob granskningsprincip</span><span class="sxs-lookup"><span data-stu-id="cf53b-232">Get Database Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [<span data-ttu-id="cf53b-233">Hämta Server Blob granskningsprincip</span><span class="sxs-lookup"><span data-stu-id="cf53b-233">Get Server Blob Auditing Policy</span></span>](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [<span data-ttu-id="cf53b-234">Hämta Server Blob granskning resultat</span><span class="sxs-lookup"><span data-stu-id="cf53b-234">Get Server Blob Auditing Operation Result</span></span>](https://msdn.microsoft.com/library/azure/mt771862.aspx)


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
