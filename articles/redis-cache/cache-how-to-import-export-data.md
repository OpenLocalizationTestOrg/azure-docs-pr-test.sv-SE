---
title: aaaImport och exportera data i Azure Redis-Cache | Microsoft Docs
description: "Lär dig hur tooand för tooimport och exportera data från blob storage med dina instanser för premium Azure Redis-Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: sdanie
ms.openlocfilehash: f17464b207f1c652952f4da63ca147473fee2759
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-data-in-azure-redis-cache"></a><span data-ttu-id="098e1-103">Importera och exportera data i Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="098e1-103">Import and Export data in Azure Redis Cache</span></span>
<span data-ttu-id="098e1-104">Importera och exportera är en Azure Redis-Cache data management-åtgärd, som gör att du tooimport data till Azure Redis-Cache eller exportera data från Azure Redis-Cache genom att importera och exportera en ögonblicksbild för Redis-Cache databasen (RDB) från en premium cache tooa blob i en Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="098e1-104">Import/Export is an Azure Redis Cache data management operation, which allows you tooimport data into Azure Redis Cache or export data from Azure Redis Cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache tooa blob in an Azure Storage Account.</span></span> 

- <span data-ttu-id="098e1-105">**Exportera** -du kan exportera din Azure Redis-Cache RDB ögonblicksbilder tooa Sidblob.</span><span class="sxs-lookup"><span data-stu-id="098e1-105">**Export** - you can export your Azure Redis Cache RDB snapshots tooa Page Blob.</span></span>
- <span data-ttu-id="098e1-106">**Importera** -du kan importera Redis-Cache RDB-ögonblicksbilder från en Sidblob eller en Blockblob.</span><span class="sxs-lookup"><span data-stu-id="098e1-106">**Import** - you can import your Redis Cache RDB snapshots from either a Page Blob or a Block Blob.</span></span>

<span data-ttu-id="098e1-107">Import/Export kan du toomigrate mellan olika instanser av Azure Redis-Cache eller fylla hello cachen med data innan de används.</span><span class="sxs-lookup"><span data-stu-id="098e1-107">Import/Export enables you toomigrate between different Azure Redis Cache instances or populate hello cache with data before use.</span></span>

<span data-ttu-id="098e1-108">Den här artikeln innehåller en guide för import och export av data med Azure Redis-Cache samt hello svar toocommonly frågor och svar.</span><span class="sxs-lookup"><span data-stu-id="098e1-108">This article provides a guide for importing and exporting data with Azure Redis Cache and provides hello answers toocommonly asked questions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="098e1-109">Import/Export är i förhandsvisning och finns bara för [premiumnivån](cache-premium-tier-intro.md) cachelagrar.</span><span class="sxs-lookup"><span data-stu-id="098e1-109">Import/Export is in preview and is only available for [premium tier](cache-premium-tier-intro.md) caches.</span></span>
>
>

## <a name="import"></a><span data-ttu-id="098e1-110">Importera</span><span class="sxs-lookup"><span data-stu-id="098e1-110">Import</span></span>
<span data-ttu-id="098e1-111">Importera kan vara används toobring Redis kompatibel RDB filer från en Redis-server som kör i molnet eller miljö, inklusive Redis som körs på Linux, Windows eller valfri provider som molntjänster, till exempel Amazon Web Services och andra.</span><span class="sxs-lookup"><span data-stu-id="098e1-111">Import can be used toobring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="098e1-112">Importera data är ett enkelt sätt toocreate en cache med data i förväg.</span><span class="sxs-lookup"><span data-stu-id="098e1-112">Importing data is an easy way toocreate a cache with pre-populated data.</span></span> <span data-ttu-id="098e1-113">Under importen hello Azure Redis-Cache läser in hello RDB filer från Azure storage i minnet och infogar hello nycklar i hello cache.</span><span class="sxs-lookup"><span data-stu-id="098e1-113">During hello import process, Azure Redis Cache loads hello RDB files from Azure storage into memory and then inserts hello keys into hello cache.</span></span>

> [!NOTE]
> <span data-ttu-id="098e1-114">Se till att din Redis-databasen (RDB) eller filer överförs till sidan eller blockera BLOB-objekt i Azure storage i hello samma innan du påbörjar hello importen, region och prenumerationen som Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="098e1-114">Before beginning hello import operation, ensure that your Redis Database (RDB) file or files are uploaded into page or block blobs in Azure storage, in hello same region and subscription as your Azure Redis Cache instance.</span></span> <span data-ttu-id="098e1-115">Mer information finns i [komma igång med Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="098e1-115">For more information, see [Get started with Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="098e1-116">Om du har exporterat RDB filen med hello [Azure Redis-Cache exportera](#export) funktionen RDB filen lagras redan i en sidblob och är redo för att importera.</span><span class="sxs-lookup"><span data-stu-id="098e1-116">If you exported your RDB file using hello [Azure Redis Cache Export](#export) feature, your RDB file is already stored in a page blob and is ready for importing.</span></span>
>
>

1. <span data-ttu-id="098e1-117">tooimport en eller flera exporteras cache blobbar [Bläddra tooyour cache](cache-configure.md#configure-redis-cache-settings) i hello Azure-portalen och klicka på **dataimport** från hello **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="098e1-117">tooimport one or more exported cache blobs, [browse tooyour cache](cache-configure.md#configure-redis-cache-settings) in hello Azure portal and click **Import data** from hello **Resource menu**.</span></span>

    ![Importera data][cache-import-data]
2. <span data-ttu-id="098e1-119">Klicka på **Välj Blob(s)** och välj hello storage-konto som innehåller hello data tooimport.</span><span class="sxs-lookup"><span data-stu-id="098e1-119">Click **Choose Blob(s)** and select hello storage account that contains hello data tooimport.</span></span>

    ![Välj lagringskonto][cache-import-choose-storage-account]
3. <span data-ttu-id="098e1-121">Klicka på hello-behållare som innehåller hello data tooimport.</span><span class="sxs-lookup"><span data-stu-id="098e1-121">Click hello container that contains hello data tooimport.</span></span>

    ![Välj behållare][cache-import-choose-container]
4. <span data-ttu-id="098e1-123">Markera en eller flera blobbar tooimport genom att klicka på hello area toohello vänster om hello blob namn och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="098e1-123">Select one or more blobs tooimport by clicking hello area toohello left of hello blob name, and then click **Select**.</span></span>

    ![Välj blobbar][cache-import-choose-blobs]
5. <span data-ttu-id="098e1-125">Klicka på **importera** toobegin hello importen.</span><span class="sxs-lookup"><span data-stu-id="098e1-125">Click **Import** toobegin hello import process.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="098e1-126">hello cachelagret är inte tillgänglig av cacheklienter under importen hello och alla befintliga data i hello cache tas bort.</span><span class="sxs-lookup"><span data-stu-id="098e1-126">hello cache is not accessible by cache clients during hello import process, and any existing data in hello cache is deleted.</span></span>
   >
   >

    ![Importera][cache-import-blobs]

    <span data-ttu-id="098e1-128">Du kan övervaka hello hello importen av följande hello meddelanden från hello Azure-portalen eller genom att visa hello händelser i hello [granskningsloggen](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="098e1-128">You can monitor hello progress of hello import operation by following hello notifications from hello Azure portal, or by viewing hello events in hello [audit log](../azure-resource-manager/resource-group-audit.md).</span></span>

    ![Importera pågår][cache-import-data-import-complete]

## <a name="export"></a><span data-ttu-id="098e1-130">Exportera</span><span class="sxs-lookup"><span data-stu-id="098e1-130">Export</span></span>
<span data-ttu-id="098e1-131">Exportera kan tooexport hello data som lagras i Azure Redis-Cache tooRedis kompatibel RDB fil(er).</span><span class="sxs-lookup"><span data-stu-id="098e1-131">Export allows you tooexport hello data stored in Azure Redis Cache tooRedis compatible RDB file(s).</span></span> <span data-ttu-id="098e1-132">Du kan använda den här funktionen toomove data från en Azure Redis-Cache-instansen tooanother eller tooanother Redis-servern.</span><span class="sxs-lookup"><span data-stu-id="098e1-132">You can use this feature toomove data from one Azure Redis Cache instance tooanother or tooanother Redis server.</span></span> <span data-ttu-id="098e1-133">Under exportprocessen hello skapas en temporär fil på hello VM att värdar hello Azure Redis-Cache server-instansen och hello fil överförda toohello avses storage-konto.</span><span class="sxs-lookup"><span data-stu-id="098e1-133">During hello export process, a temporary file is created on hello VM that hosts hello Azure Redis Cache server instance, and hello file is uploaded toohello designated storage account.</span></span> <span data-ttu-id="098e1-134">När hello exporten har slutförts med antingen en status för lyckad eller misslyckad tas hello tillfälliga filen bort.</span><span class="sxs-lookup"><span data-stu-id="098e1-134">When hello export operation completes with either a status of success or failure, hello temporary file is deleted.</span></span>

1. <span data-ttu-id="098e1-135">tooexport hello aktuella innehållet i hello cache toostorage [Bläddra tooyour cache](cache-configure.md#configure-redis-cache-settings) i hello Azure-portalen och på **exportera data** från hello **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="098e1-135">tooexport hello current contents of hello cache toostorage, [browse tooyour cache](cache-configure.md#configure-redis-cache-settings) in hello Azure portal and click **Export data** from hello **Resource menu**.</span></span>

    ![Välj lagringsbehållare][cache-export-data-choose-storage-container]
2. <span data-ttu-id="098e1-137">Klicka på **Välj lagringsbehållaren** och välj önskad hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="098e1-137">Click **Choose Storage Container** and select hello desired storage account.</span></span> <span data-ttu-id="098e1-138">hello storage-konto måste vara i hello samma prenumeration och region som ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="098e1-138">hello storage account must be in hello same subscription and region as your cache.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="098e1-139">Exportera fungerar med sidblobbar som stöds av både klassiska eller Resource Manager storage-konton, men stöds inte av [Blob storage-konton](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) just nu.</span><span class="sxs-lookup"><span data-stu-id="098e1-139">Export works with page blobs, which are supported by both classic and Resource Manager storage accounts, but are not supported by [Blob storage accounts](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) at this time.</span></span>
   >
   >

    ![Lagringskonto][cache-export-data-choose-account]
3. <span data-ttu-id="098e1-141">Välj hello önskad blob-behållaren och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="098e1-141">Choose hello desired blob container and click **Select**.</span></span> <span data-ttu-id="098e1-142">toouse nya behållaren, klicka på **lägga till behållaren** tooadd den första och välj sedan den hello-listan.</span><span class="sxs-lookup"><span data-stu-id="098e1-142">toouse new a container, click **Add Container** tooadd it first and then select it from hello list.</span></span>

    ![Välj lagringsbehållare][cache-export-data-container]
4. <span data-ttu-id="098e1-144">Ange en **Blob namnprefix** och på **exportera** toostart hello exporten.</span><span class="sxs-lookup"><span data-stu-id="098e1-144">Type a **Blob name prefix** and click **Export** toostart hello export process.</span></span> <span data-ttu-id="098e1-145">hello blob prefixet är används tooprefix hello namnen på de filer som genererats av den här exporten.</span><span class="sxs-lookup"><span data-stu-id="098e1-145">hello blob name prefix is used tooprefix hello names of files generated by this export operation.</span></span>

    ![Exportera][cache-export-data]

    <span data-ttu-id="098e1-147">Du kan övervaka hello hello export åtgärdens förlopp genom följande hello meddelanden från hello Azure-portalen eller genom att visa hello händelser i hello [granskningsloggen](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="098e1-147">You can monitor hello progress of hello export operation by following hello notifications from hello Azure portal, or by viewing hello events in hello [audit log](../azure-resource-manager/resource-group-audit.md).</span></span>

    ![Exportera data är klar][cache-export-data-export-complete]

    <span data-ttu-id="098e1-149">Cacheminnen är tillgängliga för användning under hello exporten.</span><span class="sxs-lookup"><span data-stu-id="098e1-149">Caches remain available for use during hello export process.</span></span>

## <a name="importexport-faq"></a><span data-ttu-id="098e1-150">Importera och exportera vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="098e1-150">Import/Export FAQ</span></span>
<span data-ttu-id="098e1-151">Det här avsnittet innehåller vanliga frågor och svar om hello Import/Export-funktionen.</span><span class="sxs-lookup"><span data-stu-id="098e1-151">This section contains frequently asked questions about hello Import/Export feature.</span></span>

* [<span data-ttu-id="098e1-152">Priser för vilka nivåer använder Import/Export?</span><span class="sxs-lookup"><span data-stu-id="098e1-152">What pricing tiers can use Import/Export?</span></span>](#what-pricing-tiers-can-use-importexport)
* [<span data-ttu-id="098e1-153">Kan jag importera data från Redis-servern?</span><span class="sxs-lookup"><span data-stu-id="098e1-153">Can I import data from any Redis server?</span></span>](#can-i-import-data-from-any-redis-server)
* [<span data-ttu-id="098e1-154">Vilka RDB versioner kan importera?</span><span class="sxs-lookup"><span data-stu-id="098e1-154">What RDB versions can I import?</span></span>](#what-rdb-versions-can-i-import)
* [<span data-ttu-id="098e1-155">Är Mina cache under en Import/Export-åtgärden?</span><span class="sxs-lookup"><span data-stu-id="098e1-155">Is my cache available during an Import/Export operation?</span></span>](#is-my-cache-available-during-an-importexport-operation)
* [<span data-ttu-id="098e1-156">Kan jag använda Import/Export med Redis-kluster?</span><span class="sxs-lookup"><span data-stu-id="098e1-156">Can I use Import/Export with Redis cluster?</span></span>](#can-i-use-importexport-with-redis-cluster)
* [<span data-ttu-id="098e1-157">Hur fungerar Import/Export med en anpassad databaser inställningen?</span><span class="sxs-lookup"><span data-stu-id="098e1-157">How does Import/Export work with a custom databases setting?</span></span>](#how-does-importexport-work-with-a-custom-databases-setting)
* [<span data-ttu-id="098e1-158">Hur skiljer sig Import/Export från Redis-persistence?</span><span class="sxs-lookup"><span data-stu-id="098e1-158">How is Import/Export different from Redis persistence?</span></span>](#how-is-importexport-different-from-redis-persistence)
* [<span data-ttu-id="098e1-159">Kan jag automatisera Import/Export med PowerShell, CLI eller annan av hanteringsklienter?</span><span class="sxs-lookup"><span data-stu-id="098e1-159">Can I automate Import/Export using PowerShell, CLI, or other management clients?</span></span>](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [<span data-ttu-id="098e1-160">Jag har fått ett timeout-fel under min Import/Export-åtgärd. Vad innebär det?</span><span class="sxs-lookup"><span data-stu-id="098e1-160">I received a timeout error during my Import/Export operation. What does it mean?</span></span>](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [<span data-ttu-id="098e1-161">Jag fick ett fel när du exporterar Mina data tooAzure Blob Storage. Vad hände?</span><span class="sxs-lookup"><span data-stu-id="098e1-161">I got an error when exporting my data tooAzure Blob Storage. What happened?</span></span>](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a><span data-ttu-id="098e1-162">Priser för vilka nivåer använder Import/Export?</span><span class="sxs-lookup"><span data-stu-id="098e1-162">What pricing tiers can use Import/Export?</span></span>
<span data-ttu-id="098e1-163">Import/Export är endast tillgänglig i hello premium-prisnivån.</span><span class="sxs-lookup"><span data-stu-id="098e1-163">Import/Export is available only in hello premium pricing tier.</span></span>

### <a name="can-i-import-data-from-any-redis-server"></a><span data-ttu-id="098e1-164">Kan jag importera data från Redis-servern?</span><span class="sxs-lookup"><span data-stu-id="098e1-164">Can I import data from any Redis server?</span></span>
<span data-ttu-id="098e1-165">Ja, dessutom tooimporting-data som har exporterats från Azure Redis-Cache instanser du kan importera RDB filer från en Redis-server som kör i molnet eller miljö, till exempel Linux, Windows eller molntjänstleverantörer som Amazon Web Services.</span><span class="sxs-lookup"><span data-stu-id="098e1-165">Yes, in addition tooimporting data exported from Azure Redis Cache instances, you can import RDB files from any Redis server running in any cloud or environment, such as Linux, Windows, or cloud providers such as Amazon Web Services.</span></span> <span data-ttu-id="098e1-166">toodo detta överför hello RDB filen från hello önskad Redis-servern till en sida eller block-blob i ett Azure Storage-konto och sedan importera den till din premium Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="098e1-166">toodo this, upload hello RDB file from hello desired Redis server into a page or block blob in an Azure Storage Account, and then import it into your premium Azure Redis Cache instance.</span></span> <span data-ttu-id="098e1-167">Du kan till exempel vill tooexport hello data från ditt cacheminne för produktion och importera den till en cache som används som en del av en mellanlagringsmiljön för testning eller migrering.</span><span class="sxs-lookup"><span data-stu-id="098e1-167">For example, you may want tooexport hello data from your production cache and import it into a cache used as part of a staging environment for testing or migration.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="098e1-168">toosuccessfully importera data som har exporterats från Redis-servrar än Azure Redis-Cache när med en sidblob hello blob sidstorleken måste vara lika justerade på en 512 byte-gräns.</span><span class="sxs-lookup"><span data-stu-id="098e1-168">toosuccessfully import data exported from Redis servers other than Azure Redis Cache when using a page blob, hello page blob size must be aligned on a 512 byte boundary.</span></span> <span data-ttu-id="098e1-169">För exempel kod tooperform alla obligatoriska byte utfyllnad, se [exempel sidan blogg överför](https://github.com/JimRoberts-MS/SamplePageBlobUpload).</span><span class="sxs-lookup"><span data-stu-id="098e1-169">For sample code tooperform any required byte padding, see [Sample page blog upload](https://github.com/JimRoberts-MS/SamplePageBlobUpload).</span></span>
> 
> 

### <a name="what-rdb-versions-can-i-import"></a><span data-ttu-id="098e1-170">Vilka RDB versioner kan importera?</span><span class="sxs-lookup"><span data-stu-id="098e1-170">What RDB versions can I import?</span></span>

<span data-ttu-id="098e1-171">Azure Redis-Cache stöder RDB importera upp via RDB version 7.</span><span class="sxs-lookup"><span data-stu-id="098e1-171">Azure Redis Cache supports RDB import up through RDB version 7.</span></span>

### <a name="is-my-cache-available-during-an-importexport-operation"></a><span data-ttu-id="098e1-172">Är Mina cache under en Import/Export-åtgärden?</span><span class="sxs-lookup"><span data-stu-id="098e1-172">Is my cache available during an Import/Export operation?</span></span>
* <span data-ttu-id="098e1-173">**Exportera** - cacheminnen fortfarande är tillgängliga och du kan fortsätta toouse ditt cacheminne när du exporterar.</span><span class="sxs-lookup"><span data-stu-id="098e1-173">**Export** - Caches remain available and you can continue toouse your cache during an export operation.</span></span>
* <span data-ttu-id="098e1-174">**Importera** - cacheminnet blir inte tillgänglig när en importåtgärden startas och blir tillgängliga för användning när hello importen är klar.</span><span class="sxs-lookup"><span data-stu-id="098e1-174">**Import** - Caches become unavailable when an import operation starts, and become available for use when hello import operation completes.</span></span>

### <a name="can-i-use-importexport-with-redis-cluster"></a><span data-ttu-id="098e1-175">Kan jag använda Import/Export med Redis-kluster?</span><span class="sxs-lookup"><span data-stu-id="098e1-175">Can I use Import/Export with Redis cluster?</span></span>
<span data-ttu-id="098e1-176">Ja, och du kan importera och exportera mellan en klustrad cache och en icke-klustrade cache.</span><span class="sxs-lookup"><span data-stu-id="098e1-176">Yes, and you can import/export between a clustered cache and a non-clustered cache.</span></span> <span data-ttu-id="098e1-177">Eftersom Redis cluster [endast stöder databasen 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), inte importera alla data i databaser än 0.</span><span class="sxs-lookup"><span data-stu-id="098e1-177">Since Redis cluster [only supports database 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), any data in databases other than 0 isn't imported.</span></span> <span data-ttu-id="098e1-178">När du importerar Cachedata för klustrade, omdistribueras hello nycklar bland hello shards hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="098e1-178">When clustered cache data is imported, hello keys are redistributed among hello shards of hello cluster.</span></span>

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a><span data-ttu-id="098e1-179">Hur fungerar Import/Export med en anpassad databaser inställningen?</span><span class="sxs-lookup"><span data-stu-id="098e1-179">How does Import/Export work with a custom databases setting?</span></span>
<span data-ttu-id="098e1-180">Vissa prisnivåer har olika [databaser gränser](cache-configure.md#databases), så det finns vissa saker när du importerar om du har konfigurerat ett anpassat värde för hello `databases` anger under Skapa cache.</span><span class="sxs-lookup"><span data-stu-id="098e1-180">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when importing if you configured a custom value for hello `databases` setting during cache creation.</span></span>

* <span data-ttu-id="098e1-181">När du importerar tooa prisnivån med en lägre `databases` gränsen än hello nivå som du exporterade:</span><span class="sxs-lookup"><span data-stu-id="098e1-181">When importing tooa pricing tier with a lower `databases` limit than hello tier from which you exported:</span></span>
  * <span data-ttu-id="098e1-182">Om du använder hello standardantalet `databases`, vilket är 16 för alla prisnivåer, inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="098e1-182">If you are using hello default number of `databases`, which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="098e1-183">Om du använder ett anpassat antal `databases` att nivån faller inom hello gränser för hello toowhich som du importerar, inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="098e1-183">If you are using a custom number of `databases` that falls within hello limits for hello tier toowhich you are importing, no data is lost.</span></span>
  * <span data-ttu-id="098e1-184">Om din exporterade data innehöll data i en databas som överskrider hello gränserna för hello ny nivå, importeras inte hello data från de högre databaserna.</span><span class="sxs-lookup"><span data-stu-id="098e1-184">If your exported data contained data in a database that exceeds hello limits of hello new tier, hello data from those higher databases is not imported.</span></span>

### <a name="how-is-importexport-different-from-redis-persistence"></a><span data-ttu-id="098e1-185">Hur skiljer sig Import/Export från Redis-persistence?</span><span class="sxs-lookup"><span data-stu-id="098e1-185">How is Import/Export different from Redis persistence?</span></span>
<span data-ttu-id="098e1-186">Azure Redis-Cache beständiga kan toopersist data som lagras i Redis tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="098e1-186">Azure Redis Cache persistence allows you toopersist data stored in Redis tooAzure Storage.</span></span> <span data-ttu-id="098e1-187">När beständiga konfigureras kvarstår en ögonblicksbild av hello Redis-cache i en Redis binärformat toodisk baserat på en konfigurerbar säkerhetskopieringsfrekvens Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="098e1-187">When persistence is configured, Azure Redis Cache persists a snapshot of hello Redis cache in a Redis binary format toodisk based on a configurable backup frequency.</span></span> <span data-ttu-id="098e1-188">Om ett allvarligt fel inträffar som inaktiverar såväl hello primär replik cache återställs hello cachelagrade data automatiskt med hello senaste ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="098e1-188">If a catastrophic event occurs that disables both hello primary and replica cache, hello cache data is restored automatically using hello most recent snapshot.</span></span> <span data-ttu-id="098e1-189">Mer information finns i [hur tooconfigure datapersistence för Premium Azure Redis-Cache](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="098e1-189">For more information, see [How tooconfigure data persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>

<span data-ttu-id="098e1-190">Import / Export kan du toobring data till eller exporteras från Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="098e1-190">Import/ Export allows you toobring data into or export from Azure Redis Cache.</span></span> <span data-ttu-id="098e1-191">Det inte konfigurera säkerhetskopiering och återställning med hjälp av Redis-persistence.</span><span class="sxs-lookup"><span data-stu-id="098e1-191">It does not configure backup and restore using Redis persistence.</span></span>

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a><span data-ttu-id="098e1-192">Kan jag automatisera Import/Export med PowerShell, CLI eller annan av hanteringsklienter?</span><span class="sxs-lookup"><span data-stu-id="098e1-192">Can I automate Import/Export using PowerShell, CLI, or other management clients?</span></span>
<span data-ttu-id="098e1-193">Ja, PowerShell instruktioner finns i [tooimport Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) och [tooexport Redis-cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).</span><span class="sxs-lookup"><span data-stu-id="098e1-193">Yes, for PowerShell instructions see [tooimport a Redis cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) and [tooexport a Redis cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).</span></span>

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a><span data-ttu-id="098e1-194">Jag har fått ett timeout-fel under min Import/Export-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="098e1-194">I received a timeout error during my Import/Export operation.</span></span> <span data-ttu-id="098e1-195">Vad innebär det?</span><span class="sxs-lookup"><span data-stu-id="098e1-195">What does it mean?</span></span>
<span data-ttu-id="098e1-196">Om du finns kvar i hello **dataimport** eller **exportera data** bladet längre än 15 minuter innan du påbörjar hello åtgärden ett felmeddelande med ett fel meddelande liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="098e1-196">If you remain on hello **Import data** or **Export data** blade for longer than 15 minutes before initiating hello operation, you receive an error with an error message similar toohello following example:</span></span>

    hello request tooimport data into cache 'contoso55' failed with status 'error' and error 'One of hello SAS URIs provided could not be used for hello following reason: hello SAS token end time (se) must be at least 1 hour from now and hello start time (st), if given, must be at least 15 minutes in hello past.

<span data-ttu-id="098e1-197">tooresolve, initiera hello importera eller exportera åtgärden innan 15 minuter har gått ut.</span><span class="sxs-lookup"><span data-stu-id="098e1-197">tooresolve this, initiate hello import or export operation before 15 minutes has elapsed.</span></span>

### <a name="i-got-an-error-when-exporting-my-data-tooazure-blob-storage-what-happened"></a><span data-ttu-id="098e1-198">Jag fick ett fel när du exporterar Mina data tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="098e1-198">I got an error when exporting my data tooAzure Blob Storage.</span></span> <span data-ttu-id="098e1-199">Vad hände?</span><span class="sxs-lookup"><span data-stu-id="098e1-199">What happened?</span></span>
<span data-ttu-id="098e1-200">Export fungerar bara med RDB filer som lagras som sidblobar.</span><span class="sxs-lookup"><span data-stu-id="098e1-200">Export works only with RDB files stored as page blobs.</span></span> <span data-ttu-id="098e1-201">Andra blob-typer stöds för närvarande inte, inklusive blob storage-konton med frekvent och lågfrekvent nivåer.</span><span class="sxs-lookup"><span data-stu-id="098e1-201">Other blob types are not currently supported, including blob storage accounts with hot and cool tiers.</span></span> <span data-ttu-id="098e1-202">Mer information finns i [Blob storage-konton](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).</span><span class="sxs-lookup"><span data-stu-id="098e1-202">For more information, see [Blob storage accounts](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).</span></span>

## <a name="next-steps"></a><span data-ttu-id="098e1-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="098e1-203">Next steps</span></span>
<span data-ttu-id="098e1-204">Lär dig hur toouse mer premium cachelagra funktioner.</span><span class="sxs-lookup"><span data-stu-id="098e1-204">Learn how toouse more premium cache features.</span></span>

* [<span data-ttu-id="098e1-205">Introduktion toohello Azure Redis Cache Premium-nivån</span><span class="sxs-lookup"><span data-stu-id="098e1-205">Introduction toohello Azure Redis Cache Premium tier</span></span>](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
