---
title: aaaUpgrade toohello senaste elastisk databas klientbiblioteket | Microsoft Docs
description: "Uppgradera appar och bibliotek med hjälp av Nuget"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 0a546510-76e7-465e-9271-f15ff0cfa959
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: cc2c9179be4c53ca59cd24d832127cf277c6e695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-app-toouse-hello-latest-elastic-database-client-library"></a><span data-ttu-id="126a9-103">Uppgradera en app toouse hello senaste elastisk databas klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="126a9-103">Upgrade an app toouse hello latest elastic database client library</span></span>
<span data-ttu-id="126a9-104">Nya versioner av hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md) är tillgängliga via NuGetand hello NuGetPackage Manager-gränssnittet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="126a9-104">New versions of hello [Elastic Database client library](sql-database-elastic-database-client-library.md) are  available through NuGetand hello NuGetPackage Manager interface in Visual Studio.</span></span> <span data-ttu-id="126a9-105">Uppgraderingar innehåller felkorrigeringar och stöd för nya funktioner i hello klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="126a9-105">Upgrades contain bug fixes and support for new capabilities of hello client library.</span></span>

<span data-ttu-id="126a9-106">**Senaste versionen av hello:** gå för[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="126a9-106">**For hello latest version:** Go too[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span>

<span data-ttu-id="126a9-107">Återskapar ditt program med hello nya bibliotek, samt ändra din befintliga Fragmentera kartan Manager metadata som lagras i din Azure SQL-databaser toosupport nya funktioner.</span><span class="sxs-lookup"><span data-stu-id="126a9-107">Rebuild your application with hello new library, as well as change your existing Shard Map Manager metadata stored in your Azure SQL Databases toosupport new features.</span></span>

<span data-ttu-id="126a9-108">Utför stegen i ordning säkerställer att äldre versioner av klientbiblioteket hello inte längre finns i din miljö när metadataobjekt uppdateras, vilket innebär att gamla version metadataobjekt inte skapas efter uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="126a9-108">Performing these steps in order ensures that old versions of hello client library are no longer present in your environment when metadata objects are updated, which means that old-version metadata objects won’t be created after upgrade.</span></span>   

## <a name="upgrade-steps"></a><span data-ttu-id="126a9-109">Uppgradering</span><span class="sxs-lookup"><span data-stu-id="126a9-109">Upgrade steps</span></span>
<span data-ttu-id="126a9-110">**1. Uppgradera dina program.**</span><span class="sxs-lookup"><span data-stu-id="126a9-110">**1. Upgrade your applications.**</span></span> <span data-ttu-id="126a9-111">I Visual Studio, hämtning och referens hello senaste biblioteket klientversionen i alla dina utvecklingsprojekt som använder hello library; sedan återskapa och distribuera.</span><span class="sxs-lookup"><span data-stu-id="126a9-111">In Visual Studio, download and reference hello latest client library version into all of your development projects that use hello library; then rebuild and deploy.</span></span> 

* <span data-ttu-id="126a9-112">Välj i Visual Studio-lösning **verktyg** --> **NuGet Package Manager** -->  **hantera NuGet-paket för lösningen**.</span><span class="sxs-lookup"><span data-stu-id="126a9-112">In your Visual Studio solution, select **Tools** --> **NuGet Package Manager** -->  **Manage NuGet Packages for Solution**.</span></span> 
* <span data-ttu-id="126a9-113">(Visual Studio 2013) Markera hello vänstra panelen **uppdateringar**, och välj sedan hello **uppdatering** knappen hello paketet **Azure SQL Database-klientbiblioteket med elastisk skala** som visas i hello fönstret.</span><span class="sxs-lookup"><span data-stu-id="126a9-113">(Visual Studio 2013) In hello left panel, select **Updates**, and then select hello **Update** button on hello package **Azure SQL Database Elastic Scale Client Library** that appears in hello window.</span></span>
* <span data-ttu-id="126a9-114">(Visual Studio 2015) Ange hello filterfältet för**uppgradera tillgängliga**.</span><span class="sxs-lookup"><span data-stu-id="126a9-114">(Visual Studio 2015) Set hello Filter box too**Upgrade available**.</span></span> <span data-ttu-id="126a9-115">Välj hello paketet tooupdate och klicka på hello **uppdatering** knappen.</span><span class="sxs-lookup"><span data-stu-id="126a9-115">Select hello package tooupdate, and click hello **Update** button.</span></span>
* <span data-ttu-id="126a9-116">(Visual Studio 2017) Hello över hello dialogrutan Välj **uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="126a9-116">(Visual Studio 2017) At hello top of hello dialog, select **Updates**.</span></span> <span data-ttu-id="126a9-117">Välj hello paketet tooupdate och klicka på hello **uppdatering** knappen.</span><span class="sxs-lookup"><span data-stu-id="126a9-117">Select hello package tooupdate, and click hello **Update** button.</span></span>
* <span data-ttu-id="126a9-118">Skapa och distribuera.</span><span class="sxs-lookup"><span data-stu-id="126a9-118">Build and Deploy.</span></span> 

<span data-ttu-id="126a9-119">**2. Uppgradera ditt skript.**</span><span class="sxs-lookup"><span data-stu-id="126a9-119">**2. Upgrade your scripts.**</span></span> <span data-ttu-id="126a9-120">Om du använder **PowerShell** skript toomanage shards [hämta hello ny biblioteket version](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) och kopierar den till hello katalog där du kan köra skript.</span><span class="sxs-lookup"><span data-stu-id="126a9-120">If you are using **PowerShell** scripts toomanage shards, [download hello new library version](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) and copy it into hello directory from which you execute scripts.</span></span> 

<span data-ttu-id="126a9-121">**3. Uppgradera din delade kopplingstjänsten.**</span><span class="sxs-lookup"><span data-stu-id="126a9-121">**3. Upgrade your split-merge service.**</span></span> <span data-ttu-id="126a9-122">Om du använder hello elastisk databas för delade sökvägssammanslagning tooreorganize shardade data, [hämta och distribuera hello senaste versionen av verktyget hello](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span><span class="sxs-lookup"><span data-stu-id="126a9-122">If you use hello elastic database split-merge tool tooreorganize sharded data, [download and deploy hello latest version of hello tool](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/).</span></span> <span data-ttu-id="126a9-123">Detaljerad Uppgraderingsstegen för hello Service hittar [här](sql-database-elastic-scale-overview-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="126a9-123">Detailed upgrade steps for hello Service can be found [here](sql-database-elastic-scale-overview-split-and-merge.md).</span></span> 

<span data-ttu-id="126a9-124">**4. Uppgradera din Fragmentera kartan Manager-databaser**.</span><span class="sxs-lookup"><span data-stu-id="126a9-124">**4. Upgrade your Shard Map Manager databases**.</span></span> <span data-ttu-id="126a9-125">Uppgradera hello metadata stöder Fragmentera-Maps i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="126a9-125">Upgrade hello metadata supporting your Shard Maps in Azure SQL Database.</span></span>  <span data-ttu-id="126a9-126">Det finns två sätt som du kan göra detta, med PowerShell eller C#.</span><span class="sxs-lookup"><span data-stu-id="126a9-126">There are two ways you can accomplish this, using PowerShell or C#.</span></span> <span data-ttu-id="126a9-127">Båda alternativen visas nedan.</span><span class="sxs-lookup"><span data-stu-id="126a9-127">Both options are shown below.</span></span>

<span data-ttu-id="126a9-128">***Alternativ 1: Uppgradera metadata med hjälp av PowerShell***</span><span class="sxs-lookup"><span data-stu-id="126a9-128">***Option 1: Upgrade metadata using PowerShell***</span></span>

1. <span data-ttu-id="126a9-129">Hämta hello senaste kommandoradsverktyg för NuGet från [här](http://nuget.org/nuget.exe) och spara tooa mapp.</span><span class="sxs-lookup"><span data-stu-id="126a9-129">Download hello latest command-line utility for NuGet from [here](http://nuget.org/nuget.exe) and save tooa folder.</span></span> 
2. <span data-ttu-id="126a9-130">Öppna en kommandotolk, navigerar toohello samma mapp samt problemet hello kommando:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span><span class="sxs-lookup"><span data-stu-id="126a9-130">Open a Command Prompt, navigate toohello same folder, and issue hello command: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`</span></span>
3. <span data-ttu-id="126a9-131">Navigera toohello undermapp som innehåller hello ny DLL klientversion du har precis har laddat ned, till exempel:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span><span class="sxs-lookup"><span data-stu-id="126a9-131">Navigate toohello subfolder containing hello new client DLL version you have just downloaded, for example: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`</span></span>
4. <span data-ttu-id="126a9-132">Hämta hello elastisk databas klienten uppgradera skriptlet från hello [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), och spara det i hello samma mapp som innehåller hello DLL-filen.</span><span class="sxs-lookup"><span data-stu-id="126a9-132">Download hello elastic database client upgrade scriptlet from hello [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), and save it into hello same folder containing hello DLL.</span></span>
5. <span data-ttu-id="126a9-133">Kör ”PowerShell.\upgrade.ps1” från hello kommandotolk och följ hello anvisningarna från den mappen.</span><span class="sxs-lookup"><span data-stu-id="126a9-133">From that folder, run “PowerShell .\upgrade.ps1” from hello command prompt and follow hello prompts.</span></span>

<span data-ttu-id="126a9-134">***Alternativ 2: Uppgradera metadata med hjälp av C#***</span><span class="sxs-lookup"><span data-stu-id="126a9-134">***Option 2: Upgrade metadata using C#***</span></span>

<span data-ttu-id="126a9-135">Du kan också skapa ett Visual Studio-program som öppnar din ShardMapManager itererar över alla delar och utför hello metadata för uppgraderingen genom att anropa metoder hello [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) och [ UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="126a9-135">Alternatively, create a Visual Studio application that opens your ShardMapManager, iterates over all shards, and performs hello metadata upgrade by calling hello methods [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) and [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) as in this example:</span></span> 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

<span data-ttu-id="126a9-136">Dessa tekniker för metadata uppgraderingar kan användas flera gånger utan att skada.</span><span class="sxs-lookup"><span data-stu-id="126a9-136">These techniques for metadata upgrades can be applied multiple times without harm.</span></span> <span data-ttu-id="126a9-137">Till exempel om en äldre klientversion skapar en Fragmentera oavsiktligt när du har uppdaterat, kan du köra uppgraderingen igen över alla delar tooensure som hello senaste metadata-versionen finns i hela infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="126a9-137">For example, if an older client version inadvertently creates a shard after you have already updated, you can run upgrade again across all shards tooensure that hello latest metadata version is present throughout your infrastructure.</span></span> 

<span data-ttu-id="126a9-138">**Obs:** nya versioner av klientbiblioteket hello publicerade hittills fortsätta toowork med tidigare versioner av hello Fragmentera kartan Manager metadata på Azure SQL DB och vice versa.</span><span class="sxs-lookup"><span data-stu-id="126a9-138">**Note:**  New versions of hello client library published to-date continue toowork with prior versions of hello Shard Map Manager metadata on Azure SQL DB, and vice-versa.</span></span>   <span data-ttu-id="126a9-139">Tootake nytta av hello nya funktionerna i hello senaste klienten metadata måste dock toobe uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="126a9-139">However tootake advantage of some of hello new features in hello latest client, metadata needs toobe upgraded.</span></span>   <span data-ttu-id="126a9-140">Observera att metadata uppgraderingar inte påverkar användardata och programspecifika data endast objekt som skapas och används av hello Fragmentera kartan Manager.</span><span class="sxs-lookup"><span data-stu-id="126a9-140">Note that metadata upgrades will not affect any user-data or application-specific data, only objects created and used by hello Shard Map Manager.</span></span>  <span data-ttu-id="126a9-141">Och programmen toooperate via hello uppgraderingsordningen som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="126a9-141">And applications continue toooperate through hello upgrade sequence described above.</span></span> 

## <a name="elastic-database-client-version-history"></a><span data-ttu-id="126a9-142">Versionshistorik för klienten för elastisk databas</span><span class="sxs-lookup"><span data-stu-id="126a9-142">Elastic database client version history</span></span>
<span data-ttu-id="126a9-143">Tidigare versioner, gå för[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span><span class="sxs-lookup"><span data-stu-id="126a9-143">For version history, go too[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

