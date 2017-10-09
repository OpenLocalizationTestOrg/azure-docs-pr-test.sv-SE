---
title: 'Azure Cosmos DB: Utveckla med hello tabell API i .NET | Microsoft Docs'
description: "Lär dig hur toodevelop med Azure Cosmos DB tabell-API med hjälp av .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 70c6985a1dffdbcdb07e377f8ad10355bb97712a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-table-api-in-net"></a><span data-ttu-id="6b1d5-103">Azure Cosmos DB: Utveckla med hello tabell API i .NET</span><span class="sxs-lookup"><span data-stu-id="6b1d5-103">Azure Cosmos DB: Develop with hello Table API in .NET</span></span>

<span data-ttu-id="6b1d5-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="6b1d5-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span>

<span data-ttu-id="6b1d5-106">Den här kursen ingår hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="6b1d5-106">This tutorial covers hello following tasks:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="6b1d5-107">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="6b1d5-107">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="6b1d5-108">Aktivera funktionen i hello app.config-fil</span><span class="sxs-lookup"><span data-stu-id="6b1d5-108">Enable functionality in hello app.config file</span></span> 
> * <span data-ttu-id="6b1d5-109">Skapa en tabell med hello [tabell API](table-introduction.md) (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="6b1d5-109">Create a table using hello [Table API](table-introduction.md) (preview)</span></span>
> * <span data-ttu-id="6b1d5-110">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="6b1d5-110">Add an entity tooa table</span></span> 
> * <span data-ttu-id="6b1d5-111">Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="6b1d5-111">Insert a batch of entities</span></span> 
> * <span data-ttu-id="6b1d5-112">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-112">Retrieve a single entity</span></span> 
> * <span data-ttu-id="6b1d5-113">Fråga entiteter med hjälp av automatisk sekundärindex</span><span class="sxs-lookup"><span data-stu-id="6b1d5-113">Query entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="6b1d5-114">Ersätta en entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-114">Replace an entity</span></span> 
> * <span data-ttu-id="6b1d5-115">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-115">Delete an entity</span></span> 
> * <span data-ttu-id="6b1d5-116">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="6b1d5-116">Delete a table</span></span>
 
## <a name="tables-in-azure-cosmos-db"></a><span data-ttu-id="6b1d5-117">Tabeller i Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6b1d5-117">Tables in Azure Cosmos DB</span></span> 

<span data-ttu-id="6b1d5-118">Azure Cosmos-DB tillhandahåller hello [tabell API](table-introduction.md) (förhandsgranskning) för program som behöver ett nyckel / värde-Arkiv med en mindre schema-design.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-118">Azure Cosmos DB provides hello [Table API](table-introduction.md) (preview) for applications that need a key-value store with a schema-less design.</span></span> <span data-ttu-id="6b1d5-119">[Azure Table storage](../storage/common/storage-introduction.md) SDK: er och REST API: er kan använda toowork med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-119">[Azure Table storage](../storage/common/storage-introduction.md) SDKs and REST APIs can be used toowork with Azure Cosmos DB.</span></span> <span data-ttu-id="6b1d5-120">Du kan använda Azure Cosmos DB toocreate tabeller med krav på hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-120">You can use Azure Cosmos DB toocreate tables with high throughput requirements.</span></span> <span data-ttu-id="6b1d5-121">Azure Cosmos DB stöder dataflödesoptimerade tabeller (kallas informellt "premiumtabeller"), för närvarande i allmänt tillgängliga förhandsversioner.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-121">Azure Cosmos DB supports throughput-optimized tables (informally called "premium tables"), currently in public preview.</span></span> 

<span data-ttu-id="6b1d5-122">Du kan fortsätta toouse Azure Table storage för tabeller med hög lagring och lägre genomströmning krav.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-122">You can continue toouse Azure Table storage for tables with high storage and lower throughput requirements.</span></span> <span data-ttu-id="6b1d5-123">Azure Cosmos-DB vi lägger till stöd för storage optimerat tabeller i en kommande uppdatering och befintliga och nya Azure Table storage-konton kommer att sömlöst uppgraderas tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-123">Azure Cosmos DB will introduce support for storage-optimized tables in a future update, and existing and new Azure Table storage accounts will be seamlessly upgraded tooAzure Cosmos DB.</span></span>

<span data-ttu-id="6b1d5-124">Om du använder Azure Table storage, får du följande fördelar med hello ”premium table” förhandsgranskning hello:</span><span class="sxs-lookup"><span data-stu-id="6b1d5-124">If you currently use Azure Table storage, you gain hello following benefits with hello "premium table" preview:</span></span>

- <span data-ttu-id="6b1d5-125">Nyckelfärdig [global distributionsplatsen](distribute-data-globally.md) med multihoming och [automatisk och manuell växling vid fel](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="6b1d5-125">Turn-key [global distribution](distribute-data-globally.md) with multi-homing and [automatic and manual failovers](regional-failover.md)</span></span>
- <span data-ttu-id="6b1d5-126">Stöd för automatisk schema-oberoende indexering mot alla egenskaper (”sekundärindex”) och snabb frågor</span><span class="sxs-lookup"><span data-stu-id="6b1d5-126">Support for automatic schema-agnostic indexing against all properties ("secondary indexes"), and fast queries</span></span> 
- <span data-ttu-id="6b1d5-127">Stöd för [oberoende skalning av lagring och genomströmning](partition-data.md), till alla områden</span><span class="sxs-lookup"><span data-stu-id="6b1d5-127">Support for [independent scaling of storage and throughput](partition-data.md), across any number of regions</span></span>
- <span data-ttu-id="6b1d5-128">Stöd för [dedikerad genomströmning per tabell](request-units.md) som kan skalas från hundratals toomillions begäranden per sekund</span><span class="sxs-lookup"><span data-stu-id="6b1d5-128">Support for [dedicated throughput per table](request-units.md) that can be scaled from hundreds toomillions of requests per second</span></span>
- <span data-ttu-id="6b1d5-129">Stöd för [fem justerbara konsekvensnivåer](consistency-levels.md) tootrade av tillgänglighet, svarstid och konsekvenskontroll utifrån behoven för ditt program</span><span class="sxs-lookup"><span data-stu-id="6b1d5-129">Support for [five tunable consistency levels](consistency-levels.md) tootrade off availability, latency, and consistency based on your application needs</span></span>
- <span data-ttu-id="6b1d5-130">99,99% tillgänglighet inom en enskild region och möjlighet tooadd flera regioner för högre tillgänglighet och [branschledande omfattande SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) på allmän tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-130">99.99% availability within a single region, and ability tooadd more regions for higher availability, and [industry-leading comprehensive SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) on general availability</span></span>
- <span data-ttu-id="6b1d5-131">Arbeta med hello befintliga Azure storage .NET SDK och inget kod ändringar tooyour program</span><span class="sxs-lookup"><span data-stu-id="6b1d5-131">Work with hello existing Azure storage .NET SDK, and no code changes tooyour application</span></span>

<span data-ttu-id="6b1d5-132">Hello förhandsversionen hello Azure Cosmos DB stöder tabell API: et med hello .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-132">During hello preview, Azure Cosmos DB supports hello Table API using hello .NET SDK.</span></span> <span data-ttu-id="6b1d5-133">Du kan hämta hello [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget) från NuGet, som har hello samma klasser och metoden signaturer som hello [Azure Storage SDK: N](https://www.nuget.org/packages/WindowsAzure.Storage), men kan också ansluta tooAzure Cosmos DB konton med hjälp av hello Tabell-API.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-133">You can download hello [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget) from NuGet, that has hello same classes and method signatures as hello [Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), but also can connect tooAzure Cosmos DB accounts using hello Table API.</span></span>

<span data-ttu-id="6b1d5-134">toolearn mer om komplexa lagringsuppgifter för Azure Table, se:</span><span class="sxs-lookup"><span data-stu-id="6b1d5-134">toolearn more about complex Azure Table storage tasks, see:</span></span>

* [<span data-ttu-id="6b1d5-135">Introduktion tooAzure Cosmos DB: tabell-API</span><span class="sxs-lookup"><span data-stu-id="6b1d5-135">Introduction tooAzure Cosmos DB: Table API</span></span>](table-introduction.md)
* <span data-ttu-id="6b1d5-136">Hej tabell referensdokumentationen för kötjänsten fullständig information om tillgängliga API: er [Storage-klientbibliotek för .NET-referens](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="6b1d5-136">hello Table service reference documentation for complete details about available APIs [Storage Client Library for .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="6b1d5-137">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="6b1d5-137">About this tutorial</span></span>
<span data-ttu-id="6b1d5-138">Den här kursen används för utvecklare som är bekant med hello Azure Table storage SDK och vill toouse hello premium-funktioner tillgängliga Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-138">This tutorial is for developers who are familiar with hello Azure Table storage SDK, and would like toouse hello premium features available using Azure Cosmos DB.</span></span> <span data-ttu-id="6b1d5-139">Den är baserad på [komma igång med Azure Table storage med hjälp av .NET](table-storage-how-to-use-dotnet.md) och visar hur tootake nytta av ytterligare funktioner som sekundärindex, dataflöde och multihoming.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-139">It is based on [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) and shows how tootake advantage of additional capabilities like secondary indexes, provisioned throughput, and multi-homing.</span></span> <span data-ttu-id="6b1d5-140">Vi upp hur toouse hello Azure portal toocreate ett Azure DB som Cosmos-konto och sedan skapa och distribuera ett program för tabellen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-140">We cover how toouse hello Azure portal toocreate an Azure Cosmos DB account, and then build and deploy a Table application.</span></span> <span data-ttu-id="6b1d5-141">Vi också igenom .NET-exempel för att skapa och tar bort en tabell och infoga, uppdatera, ta bort och frågar tabelldata.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-141">We also walk through .NET examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span> 

<span data-ttu-id="6b1d5-142">Om du inte redan har Visual Studio 2017 installerat, du kan hämta och använda hello **ledigt** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6b1d5-142">If you don't already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="6b1d5-143">Kontrollera att du aktiverar **Azure-utveckling** under installationen av hello Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-143">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="6b1d5-144">Skapa ett databaskonto</span><span class="sxs-lookup"><span data-stu-id="6b1d5-144">Create a database account</span></span>

<span data-ttu-id="6b1d5-145">Börja med att skapa ett Azure DB som Cosmos-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-145">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="6b1d5-146">Redan har ett Azure DB som Cosmos-konto?</span><span class="sxs-lookup"><span data-stu-id="6b1d5-146">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="6b1d5-147">I så fall, kan du gå vidare för[konfigurera Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="6b1d5-147">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>
> * <span data-ttu-id="6b1d5-148">Hade du ett Azure DocumentDB-konto?</span><span class="sxs-lookup"><span data-stu-id="6b1d5-148">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="6b1d5-149">Om ditt konto är nu en Azure DB som Cosmos-konto och du gå vidare för så[konfigurera Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="6b1d5-149">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="6b1d5-150">Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[ställa in din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="6b1d5-150">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span>
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting tooany location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-hello-sample-application"></a><span data-ttu-id="6b1d5-151">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-151">Clone hello sample application</span></span>

<span data-ttu-id="6b1d5-152">Nu ska vi klona en tabell app från github, ange hello anslutningssträngen och kör den.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-152">Now let's clone a Table app from github, set hello connection string, and run it.</span></span>

1. <span data-ttu-id="6b1d5-153">Öppna ett git terminalfönster, till exempel git bash och `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-153">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="6b1d5-154">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-154">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. <span data-ttu-id="6b1d5-155">Öppna sedan hello lösningsfilen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-155">Then open hello solution file in Visual Studio.</span></span>

## <a name="update-your-connection-string"></a><span data-ttu-id="6b1d5-156">Uppdatera din anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="6b1d5-156">Update your connection string</span></span>

<span data-ttu-id="6b1d5-157">Gå tillbaka toohello Azure portal tooget din Anslutningssträngsinformation nu och kopierar den till hello app.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-157">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="6b1d5-158">I hello [Azure-portalen](http://portal.azure.com/), i din Azure Cosmos DB konto, i hello vänstra navigeringsrutan klickar du på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-158">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="6b1d5-159">I hello app.config-fil i hello nästa steg ska du använda hello kopiera knappar hello höger på hello skärmen toocopy hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-159">You'll use hello copy buttons on hello right side of hello screen toocopy hello connection string into hello app.config file in hello next step.</span></span>

2. <span data-ttu-id="6b1d5-160">Öppna hello app.config-filen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-160">In Visual Studio, open hello app.config file.</span></span> 

3. <span data-ttu-id="6b1d5-161">Kopiera URI-värdet från hello-portal (med hello kopieringsknappen) och gör den hello värdet för hello konto-nyckeln i app.config. Använda hello-kontonamn som skapats tidigare för kontonamnet i app.config.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-161">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello account-key in app.config. Use hello account name created earlier for account-name in app.config.</span></span>
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> <span data-ttu-id="6b1d5-162">toouse den här appen med standard Azure Table Storage, behöver du toochange hello anslutningssträngen i `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-162">toouse this app with standard Azure Table Storage, you need toochange hello connection string in `app.config file`.</span></span> <span data-ttu-id="6b1d5-163">Använda hello kontonamn som tabellen kontonamnet och nyckeln som primärnyckel för Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-163">Use hello account name as Table-account name and key as Azure Storage Primary key.</span></span> <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-hello-app"></a><span data-ttu-id="6b1d5-164">Skapa och distribuera hello app</span><span class="sxs-lookup"><span data-stu-id="6b1d5-164">Build and deploy hello app</span></span>
1. <span data-ttu-id="6b1d5-165">Högerklicka på hello-projekt i Visual Studio **Solution Explorer** och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-165">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="6b1d5-166">I hello NuGet **Bläddra** skriver ***WindowsAzure.Storage PremiumTable***.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-166">In hello NuGet **Browse** box, type ***WindowsAzure.Storage-PremiumTable***.</span></span> <span data-ttu-id="6b1d5-167">Kontrollera **är förhandsversioner**.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-167">Check **Include prerelease versions**.</span></span>

3. <span data-ttu-id="6b1d5-168">Hello resultat för att installera hello **WindowsAzure.Storage PremiumTable** och välj hello förhandsversionen `0.0.1-preview`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-168">From hello results, install hello **WindowsAzure.Storage-PremiumTable** and choose hello preview build `0.0.1-preview`.</span></span> <span data-ttu-id="6b1d5-169">Den här åtgärden installerar hello Azure Table storage paketet och alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-169">This action installs hello Azure Table storage package and all dependencies.</span></span>

4. <span data-ttu-id="6b1d5-170">Klicka på CTRL + F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-170">Click CTRL + F5 toorun hello application.</span></span> 

<span data-ttu-id="6b1d5-171">Du kan nu gå tillbaka tooData Explorer Se fråga, ändra och arbeta med tabelldata för den här.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-171">You can now go back tooData Explorer and see query, modify, and work with this table data.</span></span> 

> [!NOTE]
> <span data-ttu-id="6b1d5-172">toouse appen med en Azure Cosmos DB-emulatorn du bara behöver toochange hello anslutningssträngen i `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-172">toouse this app with an Azure Cosmos DB Emulator, you just need toochange hello connection string in `app.config file`.</span></span> <span data-ttu-id="6b1d5-173">Använd hello mindre än värdet för emulatorn.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-173">Use hello below value for emulator.</span></span> <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a><span data-ttu-id="6b1d5-174">Azure DB Cosmos-funktioner</span><span class="sxs-lookup"><span data-stu-id="6b1d5-174">Azure Cosmos DB capabilities</span></span>
<span data-ttu-id="6b1d5-175">Azure Cosmos-DB stöder ett antal funktioner som inte är tillgängliga i hello Azure Table storage API.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-175">Azure Cosmos DB supports a number of capabilities that are not available in hello Azure Table storage API.</span></span> <span data-ttu-id="6b1d5-176">hello nya funktioner kan aktiveras via hello följande `appSettings` konfigurationsvärden.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-176">hello new functionality can be enabled via hello following `appSettings` configuration values.</span></span> <span data-ttu-id="6b1d5-177">Vi inte införa några nya signaturer eller överlagringar toohello Förhandsgranska Azure Storage SDK: N.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-177">We did not introduce any new signatures or overloads toohello preview Azure Storage SDK.</span></span> <span data-ttu-id="6b1d5-178">Detta ger dig tooconnect tooboth standard och premium-tabeller och fungerar med andra Azure Storage-tjänster som Blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-178">This allows you tooconnect tooboth standard and premium tables, and work with other Azure Storage services like Blobs and Queues.</span></span> 


| <span data-ttu-id="6b1d5-179">Nyckel</span><span class="sxs-lookup"><span data-stu-id="6b1d5-179">Key</span></span> | <span data-ttu-id="6b1d5-180">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6b1d5-180">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6b1d5-181">TableConnectionMode</span><span class="sxs-lookup"><span data-stu-id="6b1d5-181">TableConnectionMode</span></span>  | <span data-ttu-id="6b1d5-182">Azure Cosmos-DB stöder två lägen för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-182">Azure Cosmos DB supports two connectivity modes.</span></span> <span data-ttu-id="6b1d5-183">I `Gateway` läge begäran görs alltid toohello Azure DB som Cosmos-gateway som vidarebefordrar det toohello partitioner för motsvarande data.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-183">In `Gateway` mode, requests are always made toohello Azure Cosmos DB gateway, which forwards it toohello corresponding data partitions.</span></span> <span data-ttu-id="6b1d5-184">I `Direct` anslutningsläget, hello klienten hämtar hello mappningen av tabeller toopartitions och begäranden som görs direkt mot datapartitioner.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-184">In `Direct` connectivity mode, hello client fetches hello mapping of tables toopartitions, and requests are made directly against data partitions.</span></span> <span data-ttu-id="6b1d5-185">Vi rekommenderar `Direct`, hello standard.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-185">We recommend `Direct`, hello default.</span></span>  |
| <span data-ttu-id="6b1d5-186">TableConnectionProtocol</span><span class="sxs-lookup"><span data-stu-id="6b1d5-186">TableConnectionProtocol</span></span> | <span data-ttu-id="6b1d5-187">Azure Cosmos-DB stöder två anslutningsprotokoll - `Https` och `Tcp`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-187">Azure Cosmos DB supports two connection protocols - `Https` and `Tcp`.</span></span> <span data-ttu-id="6b1d5-188">`Tcp`hello standard och rekommenderas eftersom det är mer lightweight.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-188">`Tcp` is hello default, and recommended because it is more lightweight.</span></span> |
| <span data-ttu-id="6b1d5-189">TablePreferredLocations</span><span class="sxs-lookup"><span data-stu-id="6b1d5-189">TablePreferredLocations</span></span> | <span data-ttu-id="6b1d5-190">Kommaavgränsad lista över önskade (flera homing) platser för läsning.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-190">Comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="6b1d5-191">Varje Azure DB som Cosmos-kontot kan vara associerat med 1 – 30 + regioner.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-191">Each Azure Cosmos DB account can be associated with 1-30+ regions.</span></span> <span data-ttu-id="6b1d5-192">Varje klientinstans kan ange en delmängd av dessa regioner hello rekommenderas för låg latens läsningar.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-192">Each client instance can specify a subset of these regions in hello preferred order for low latency reads.</span></span> <span data-ttu-id="6b1d5-193">hello regioner måste namnges med deras [visningsnamn](https://msdn.microsoft.com/library/azure/gg441293.aspx), till exempel `West US`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-193">hello regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span> <span data-ttu-id="6b1d5-194">Se även [multihoming API: er](tutorial-global-distribution-table.md).</span><span class="sxs-lookup"><span data-stu-id="6b1d5-194">Also see [Multi-homing APIs](tutorial-global-distribution-table.md).</span></span>
| <span data-ttu-id="6b1d5-195">TableConsistencyLevel</span><span class="sxs-lookup"><span data-stu-id="6b1d5-195">TableConsistencyLevel</span></span> | <span data-ttu-id="6b1d5-196">Du kan handel av mellan svarstid, konsekvens och tillgänglighet genom att välja mellan fem väldefinierade konsekvensnivåer: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, och `Eventual`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-196">You can trade off between latency, consistency, and availability by choosing between five well-defined consistency levels: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, and `Eventual`.</span></span> <span data-ttu-id="6b1d5-197">Standardvärdet är `Session`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-197">Default is `Session`.</span></span> <span data-ttu-id="6b1d5-198">hello valet av konsekvensnivå gör betydande prestanda skillnader i flera regioner inställningar.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-198">hello choice of consistency level makes a significant performance difference in multi-region setups.</span></span> <span data-ttu-id="6b1d5-199">Se [konsekvensnivåer](consistency-levels.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-199">See [Consistency levels](consistency-levels.md) for details.</span></span> |
| <span data-ttu-id="6b1d5-200">TableThroughput</span><span class="sxs-lookup"><span data-stu-id="6b1d5-200">TableThroughput</span></span> | <span data-ttu-id="6b1d5-201">Reserverat dataflöde för hello tabellen uttryckt i frågeenheter (RU) per sekund.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-201">Reserved throughput for hello table expressed in request units (RU) per second.</span></span> <span data-ttu-id="6b1d5-202">Enskild tabeller har stöd för 100-tal miljontals RU/s.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-202">Single tables can support 100s-millions of RU/s.</span></span> <span data-ttu-id="6b1d5-203">Se [programbegäran](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="6b1d5-203">See [Request units](request-units.md).</span></span> <span data-ttu-id="6b1d5-204">Standardvärdet är`400`</span><span class="sxs-lookup"><span data-stu-id="6b1d5-204">Default is `400`</span></span> |
| <span data-ttu-id="6b1d5-205">TableIndexingPolicy</span><span class="sxs-lookup"><span data-stu-id="6b1d5-205">TableIndexingPolicy</span></span> | <span data-ttu-id="6b1d5-206">Konsekvent och automatisk sekundära indexering av alla kolumner i tabeller</span><span class="sxs-lookup"><span data-stu-id="6b1d5-206">Consistent and automatic secondary indexing of all columns within tables</span></span> | <span data-ttu-id="6b1d5-207">JSON-sträng förväntad toohello indexering principspecifikationen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-207">JSON string conforming toohello indexing policy specification.</span></span> <span data-ttu-id="6b1d5-208">Se [indexering princip](indexing-policies.md) toosee hur du kan ändra indexering princip tooinclude/exkludera specifika kolumner.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-208">See [Indexing Policy](indexing-policies.md) toosee how you can change indexing policy tooinclude/exclude specific columns.</span></span> | <span data-ttu-id="6b1d5-209">Automatisk indexering av alla egenskaper (hash för strängar) och intervallet för tal</span><span class="sxs-lookup"><span data-stu-id="6b1d5-209">Automatic indexing of all properties (hash for strings, and range for numbers)</span></span> |
| <span data-ttu-id="6b1d5-210">TableQueryMaxItemCount</span><span class="sxs-lookup"><span data-stu-id="6b1d5-210">TableQueryMaxItemCount</span></span> | <span data-ttu-id="6b1d5-211">Konfigurera hello maximalt antal objekt som returneras per fråga i en enda onödig kommunikation.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-211">Configure hello maximum number of items returned per table query in a single round trip.</span></span> <span data-ttu-id="6b1d5-212">Standardvärdet är `-1`, vilket gör att Azure Cosmos DB dynamiskt bestämma hello värdet vid körning.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-212">Default is `-1`, which lets Azure Cosmos DB dynamically determine hello value at runtime.</span></span> |
| <span data-ttu-id="6b1d5-213">TableQueryEnableScan</span><span class="sxs-lookup"><span data-stu-id="6b1d5-213">TableQueryEnableScan</span></span> | <span data-ttu-id="6b1d5-214">Om hello frågan inte kan använda hello index för filter, kör det ändå via en genomsökning.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-214">If hello query cannot use hello index for any filter, then run it anyway via a scan.</span></span> <span data-ttu-id="6b1d5-215">Standardvärdet är `false`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-215">Default is `false`.</span></span>|
| <span data-ttu-id="6b1d5-216">TableQueryMaxDegreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="6b1d5-216">TableQueryMaxDegreeOfParallelism</span></span> | <span data-ttu-id="6b1d5-217">hello grad av parallellitet för körning av en fråga för cross-partition.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-217">hello degree of parallelism for execution of a cross-partition query.</span></span> <span data-ttu-id="6b1d5-218">`0`är seriell med ingen före hämtning, `1` är seriell med före hämtning och högre värden öka hello frekvensen parallellitet.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-218">`0` is serial with no pre-fetching, `1` is serial with pre-fetching, and higher values increase hello rate of parallelism.</span></span> <span data-ttu-id="6b1d5-219">Standardvärdet är `-1`, vilket gör att Azure Cosmos DB dynamiskt bestämma hello värdet vid körning.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-219">Default is `-1`, which lets Azure Cosmos DB dynamically determine hello value at runtime.</span></span> |

<span data-ttu-id="6b1d5-220">Standardvärdet för toochange hello, öppna hello `app.config` filen i Solutions Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-220">toochange hello default value, open hello `app.config` file from Solution Explorer in Visual Studio.</span></span> <span data-ttu-id="6b1d5-221">Lägg till hello innehållet i hello `<appSettings>` elementet enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-221">Add hello contents of hello `<appSettings>` element shown below.</span></span> <span data-ttu-id="6b1d5-222">Ersätt `account-name` med hello namnet på ditt lagringskonto och `account-key` med din åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-222">Replace `account-name` with hello name of your storage account, and `account-key` with your account access key.</span></span> 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

<span data-ttu-id="6b1d5-223">Låt oss göra en snabb genomgång av vad som händer i hello app.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-223">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="6b1d5-224">Öppna hello `Program.cs` fil och du hittar att dessa rader med kod skapar hello tabellen resurser.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-224">Open hello `Program.cs` file and you find that these lines of code create hello Table resources.</span></span> 

## <a name="create-hello-table-client"></a><span data-ttu-id="6b1d5-225">Skapa hello tabell klient</span><span class="sxs-lookup"><span data-stu-id="6b1d5-225">Create hello table client</span></span>
<span data-ttu-id="6b1d5-226">Du har initierat en `CloudTableClient` tooconnect toohello tabell konto.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-226">You initialize a `CloudTableClient` tooconnect toohello table account.</span></span>

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
<span data-ttu-id="6b1d5-227">Den här klienten har initierats med hello `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, och `TablePreferredLocations` konfiguration värden om anges i inställningarna för hello-app.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-227">This client is initialized using hello `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, and `TablePreferredLocations` configuration values if specified in hello app settings.</span></span>
    
## <a name="create-a-table"></a><span data-ttu-id="6b1d5-228">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="6b1d5-228">Create a table</span></span>
<span data-ttu-id="6b1d5-229">Sedan kan du skapa en tabell med hjälp av `CloudTable`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-229">Then, you create a table using `CloudTable`.</span></span> <span data-ttu-id="6b1d5-230">Tabeller i Azure Cosmos DB kan skalas oberoende vad gäller lagring och genomflöde och partitionering hanteras automatiskt av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-230">Tables in Azure Cosmos DB can scale independently in terms of storage and throughput, and partitioning is handled automatically by hello service.</span></span> <span data-ttu-id="6b1d5-231">Azure Cosmos-DB stöder både med fast storlek och obegränsat antal tabeller.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-231">Azure Cosmos DB supports both fixed size and unlimited tables.</span></span> <span data-ttu-id="6b1d5-232">Se [partitionering i Azure Cosmos DB](partition-data.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-232">See [Partitioning in Azure Cosmos DB](partition-data.md) for details.</span></span> 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

<span data-ttu-id="6b1d5-233">Det finns en viktig skillnad i hur du skapar tabeller.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-233">There is an important difference in how tables are created.</span></span> <span data-ttu-id="6b1d5-234">Azure Cosmos-DB reserverar dataflöde, till skillnad från Azure storage förbrukningsbaserad modell för transaktioner.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-234">Azure Cosmos DB reserves throughput, unlike Azure storage's consumption-based model for transactions.</span></span> <span data-ttu-id="6b1d5-235">hello reservation modellen har två viktiga fördelar:</span><span class="sxs-lookup"><span data-stu-id="6b1d5-235">hello reservation model has two key benefits:</span></span>

* <span data-ttu-id="6b1d5-236">Din genomströmning reserveras dedikerad /, så du får aldrig begränsas om din frågehastigheten är vid eller under din dataflöde</span><span class="sxs-lookup"><span data-stu-id="6b1d5-236">Your throughput is dedicated/reserved, so you never get throttled if your request rate is at or below your provisioned throughput</span></span>
* <span data-ttu-id="6b1d5-237">hello reservation modellen är mer [kostnadseffektivt för genomströmning tunga arbetsbelastningar](key-value-store-cost.md)</span><span class="sxs-lookup"><span data-stu-id="6b1d5-237">hello reservation model is more [cost effective for throughput-heavy workloads](key-value-store-cost.md)</span></span>

<span data-ttu-id="6b1d5-238">Du kan konfigurera hello standard genomströmning genom att konfigurera hello inställning för `TableThroughput` vad gäller RU (frågeenheter) per sekund.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-238">You can configure hello default throughput by configuring hello setting for `TableThroughput` in terms of RU (request units) per second.</span></span> 

<span data-ttu-id="6b1d5-239">En läsning av en 1 KB entitet är normaliserat som 1 RU och andra åtgärder är normaliserat tooa fast RU värde baserat på deras förbrukning av CPU, minne och IOPS.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-239">A read of a 1-KB entity is normalized as 1 RU, and other operations are normalized tooa fixed RU value based on their CPU, memory, and IOPS consumption.</span></span> <span data-ttu-id="6b1d5-240">Lär dig mer om [programbegäran i Azure Cosmos DB](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="6b1d5-240">Learn more about [Request units in Azure Cosmos DB](request-units.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6b1d5-241">Medan tabellagring SDK inte stöder för närvarande ändra dataflöde, kan du ändra hello genomströmning omedelbart när som helst med hello Azure-portalen eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-241">While Table storage SDK does not currently support modifying throughput, you can change hello throughput instantaneously at any time using hello Azure portal or Azure CLI.</span></span>

<span data-ttu-id="6b1d5-242">Sedan vi gå igenom hello enkla läsa och skriva CRUD-åtgärder med hjälp av hello Azure Table storage SDK.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-242">Next, we walk through hello simple read and write (CRUD) operations using hello Azure Table storage SDK.</span></span> <span data-ttu-id="6b1d5-243">Den här kursen visar förutsägbar låg en siffra millisekunders latens och snabb frågor som tillhandahålls av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-243">This tutorial demonstrates predictable low single-digit millisecond latencies and fast queries provided by Azure Cosmos DB.</span></span>

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="6b1d5-244">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="6b1d5-244">Add an entity tooa table</span></span>
<span data-ttu-id="6b1d5-245">Entiteter i Azure Table storage utöka från hello `TableEntity` klassen och måste ha `PartitionKey` och `RowKey` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-245">Entities in Azure Table storage extend from hello `TableEntity` class and must have `PartitionKey` and `RowKey` properties.</span></span> <span data-ttu-id="6b1d5-246">Här är ett exempel definitionen för en kundentitet.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-246">Here's a sample definition for a customer entity.</span></span>

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

<span data-ttu-id="6b1d5-247">hello följande utdrag visar hur tooinsert en entitet med hello Azure storage SDK.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-247">hello following snippet shows how tooinsert an entity with hello Azure storage SDK.</span></span> <span data-ttu-id="6b1d5-248">Azure Cosmos-DB är utformad för garanteras låg latens i alla skalor över hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-248">Azure Cosmos DB is designed for guaranteed low latency at any scale, across hello world.</span></span>

<span data-ttu-id="6b1d5-249">Slutför skrivningar < 15 ms på p99 och ~ 6 ms på p 50 för program som körs hello samma region som hello Azure DB som Cosmos-konto.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-249">Writes complete <15 ms at p99 and ~6 ms at p50 for applications running in hello same region as hello Azure Cosmos DB account.</span></span> <span data-ttu-id="6b1d5-250">Och varaktigheten konton för hello fakta som skriver är bekräftade tillbaka toohello klienten när de synkront replikeras, varaktigt allokerat och allt innehåll som är indexerad.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-250">And this duration accounts for hello fact that writes are acknowledged back toohello client only after they are synchronously replicated, durably committed, and all content is indexed.</span></span>

<span data-ttu-id="6b1d5-251">hello tabell API: et för Azure Cosmos DB finns i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-251">hello Table API for Azure Cosmos DB is in preview.</span></span> <span data-ttu-id="6b1d5-252">Vid allmän tillgänglighet backas hello p99 svarstid garantier upp av SLA: er som andra Azure Cosmos DB-API: er.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-252">At general availability, hello p99 latency guarantees are backed by SLAs like other Azure Cosmos DB APIs.</span></span> 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute hello insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="6b1d5-253">Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="6b1d5-253">Insert a batch of entities</span></span>
<span data-ttu-id="6b1d5-254">Azure Table storage stöder en batch åtgärden API, där du kan kombinera borttagningar, uppdateringar och infogningar i hello samma batchåtgärd.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-254">Azure Table storage supports a batch operation API, that lets you combine updates, deletes, and inserts in hello same single batch operation.</span></span> <span data-ttu-id="6b1d5-255">Azure Cosmos-DB har inte några hello begränsningar på hello batch API som Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-255">Azure Cosmos DB does not have some of hello limitations on hello batch API as Azure Table storage.</span></span> <span data-ttu-id="6b1d5-256">Exempelvis kan du utföra flera läsningar inom en grupp, kan du utföra flera skrivningar toohello samma entitet i en batch och det finns ingen gräns på 100-åtgärder per batch.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-256">For example, you can perform multiple reads within a batch, you can perform multiple writes toohello same entity within a batch, and there is no limit on 100 operations per batch.</span></span> 

```csharp
// Create hello batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it toohello table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it toohello table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities toohello batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute hello batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a><span data-ttu-id="6b1d5-257">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-257">Retrieve a single entity</span></span>
<span data-ttu-id="6b1d5-258">Hämtar (hämtar) i Azure Cosmos DB fullständig < 10 ms på p99 och ~ 1 ms på p 50 i hello samma Azure-region.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-258">Retrieves (GETs) in Azure Cosmos DB complete <10 ms at p99 and ~1 ms at p50 in hello same Azure region.</span></span> <span data-ttu-id="6b1d5-259">Du kan lägga till så många regioner tooyour för låg latens läsningar och distribuera program tooread från sin lokala region (”multi-homed”) genom att ange `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-259">You can add as many regions tooyour account for low latency reads, and deploy applications tooread from their local region ("multi-homed") by setting `TablePreferredLocations`.</span></span> 

<span data-ttu-id="6b1d5-260">Du kan hämta en enda entitet med hjälp av följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="6b1d5-260">You can retrieve a single entity using hello following snippet:</span></span>

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> <span data-ttu-id="6b1d5-261">Lär dig mer om flera API: er på [utveckling med flera regioner](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="6b1d5-261">Learn about multi-homing APIs at [Developing with multiple regions](tutorial-global-distribution-table.md)</span></span>
>

## <a name="query-entities-using-automatic-secondary-indexes"></a><span data-ttu-id="6b1d5-262">Fråga entiteter med hjälp av automatisk sekundärindex</span><span class="sxs-lookup"><span data-stu-id="6b1d5-262">Query entities using automatic secondary indexes</span></span>
<span data-ttu-id="6b1d5-263">Tabeller kan efterfrågas med hello `TableQuery` klass.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-263">Tables can be queried using hello `TableQuery` class.</span></span> <span data-ttu-id="6b1d5-264">Azure Cosmos-DB har en Skriv-optimerad databasmotor som indexerar automatiskt alla kolumner i tabellen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-264">Azure Cosmos DB has a write-optimized database engine that automatically indexes all columns within your table.</span></span> <span data-ttu-id="6b1d5-265">Indexering i Azure Cosmos DB är storleksoberoende tooschema.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-265">Indexing in Azure Cosmos DB is agnostic tooschema.</span></span> <span data-ttu-id="6b1d5-266">Även om schemat skiljer sig mellan rader, eller om hello schemat utvecklas över tid, indexeras det därför automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-266">Therefore, even if your schema is different between rows, or if hello schema evolves over time, it is automatically indexed.</span></span> <span data-ttu-id="6b1d5-267">Eftersom Azure Cosmos DB har stöd för automatisk sekundärindex, frågor mot en egenskap kan använda hello index och hanteras effektivt.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-267">Since Azure Cosmos DB supports automatic secondary indexes, queries against any property can use hello index and be served efficiently.</span></span>

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

<span data-ttu-id="6b1d5-268">Under förhandsgranskning, Azure Cosmos DB stöder hello samma fråga funktioner som Azure Table storage för hello tabell API.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-268">In preview, Azure Cosmos DB supports hello same query functionality as Azure Table storage for hello Table API.</span></span> <span data-ttu-id="6b1d5-269">Azure Cosmos-DB också stöd för sortering, aggregeringar, geospatiala frågan, hierarki och en mängd olika inbyggda funktioner.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-269">Azure Cosmos DB also supports sorting, aggregates, geospatial query, hierarchy, and a wide range of built-in functions.</span></span> <span data-ttu-id="6b1d5-270">hello ytterligare funktioner ska anges i hello tabell API i en framtida tjänstuppdatering.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-270">hello additional functionality will be provided in hello Table API in a future service update.</span></span> <span data-ttu-id="6b1d5-271">Se [Azure Cosmos DB-fråga](documentdb-sql-query.md) en översikt över dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-271">See [Azure Cosmos DB query](documentdb-sql-query.md) for an overview of these capabilities.</span></span> 

## <a name="replace-an-entity"></a><span data-ttu-id="6b1d5-272">Ersätta en entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-272">Replace an entity</span></span>
<span data-ttu-id="6b1d5-273">Hämta den från tabelltjänsten hello tooupdate en entitet, ändra hello enhetsobjekt och sedan tillbaka toohello tabelltjänsten spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-273">tooupdate an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="6b1d5-274">hello ändrar följande kod en befintlig kunds telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-274">hello following code changes an existing customer's phone number.</span></span> 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
<span data-ttu-id="6b1d5-275">På liknande sätt kan du utföra `InsertOrMerge` eller `Merge` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-275">Similarly, you can perform `InsertOrMerge` or `Merge` operations.</span></span>  

## <a name="delete-an-entity"></a><span data-ttu-id="6b1d5-276">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-276">Delete an entity</span></span>
<span data-ttu-id="6b1d5-277">Du kan enkelt ta bort en enhet när du har hämtat den genom att använda hello samma mönster för att uppdatera en entitet.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-277">You can easily delete an entity after you have retrieved it by using hello same pattern shown for updating an entity.</span></span> <span data-ttu-id="6b1d5-278">hello följande kod hämtar och tar bort en kundentitet.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-278">hello following code retrieves and deletes a customer entity.</span></span>

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a><span data-ttu-id="6b1d5-279">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="6b1d5-279">Delete a table</span></span>
<span data-ttu-id="6b1d5-280">Följande kodexempel hello tar slutligen bort en tabell från ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-280">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="6b1d5-281">Du kan ta bort och återskapa en tabell omedelbart med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-281">You can delete and recreate a table immediately with Azure Cosmos DB.</span></span>

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a><span data-ttu-id="6b1d5-282">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="6b1d5-282">Clean up resources</span></span> 

<span data-ttu-id="6b1d5-283">Om du inte kommer toocontinue toouse den här appen, använda hello följande steg toodelete alla resurser som skapats av den här kursen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-283">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span>   

1. <span data-ttu-id="6b1d5-284">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-284">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span>  
2. <span data-ttu-id="6b1d5-285">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-285">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6b1d5-286">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6b1d5-286">Next steps</span></span>

<span data-ttu-id="6b1d5-287">Vi beskrivs hur tooget igång med Azure Cosmos DB med hello tabell API i den här självstudiekursen och du har gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="6b1d5-287">In this tutorial, we covered how tooget started using Azure Cosmos DB with hello Table API, and you've done hello following:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="6b1d5-288">Skapa ett Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="6b1d5-288">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="6b1d5-289">Funktioner som aktiveras i hello app.config-fil</span><span class="sxs-lookup"><span data-stu-id="6b1d5-289">Enabled functionality in hello app.config file</span></span> 
> * <span data-ttu-id="6b1d5-290">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="6b1d5-290">Created a table</span></span> 
> * <span data-ttu-id="6b1d5-291">Lägga till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="6b1d5-291">Added an entity tooa table</span></span> 
> * <span data-ttu-id="6b1d5-292">Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="6b1d5-292">Inserted a batch of entities</span></span> 
> * <span data-ttu-id="6b1d5-293">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-293">Retrieved a single entity</span></span> 
> * <span data-ttu-id="6b1d5-294">Efterfrågade enheter med hjälp av automatisk sekundärindex</span><span class="sxs-lookup"><span data-stu-id="6b1d5-294">Queried entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="6b1d5-295">Ersätta en entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-295">Replaced an entity</span></span> 
> * <span data-ttu-id="6b1d5-296">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="6b1d5-296">Deleted an entity</span></span> 
> * <span data-ttu-id="6b1d5-297">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="6b1d5-297">Deleted a table</span></span>  

<span data-ttu-id="6b1d5-298">Du kan nu fortsätta toohello nästa kurs och lär dig mer om frågar tabelldata.</span><span class="sxs-lookup"><span data-stu-id="6b1d5-298">You can now proceed toohello next tutorial and learn more about querying table data.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="6b1d5-299">Frågan med hello tabell-API</span><span class="sxs-lookup"><span data-stu-id="6b1d5-299">Query with hello Table API</span></span>](tutorial-query-table.md)
