---
title: 'Azure Cosmos DB: Utveckla med hello DocumentDB API i .NET | Microsoft Docs'
description: "Lär dig hur toodevelop med Azure Cosmos DB DocumentDB-API med hjälp av .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 0d3d17afa782054c8fdf3cbac421e5a5d0a6800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmosdb-develop-with-hello-documentdb-api-in-net"></a><span data-ttu-id="0d2c6-103">Azure CosmosDB: Utveckla med hello DocumentDB API i .NET</span><span class="sxs-lookup"><span data-stu-id="0d2c6-103">Azure CosmosDB: Develop with hello DocumentDB API in .NET</span></span>

<span data-ttu-id="0d2c6-104">Azure Cosmos DB är Microsofts globalt distribuerade databastjänst för flera datamodeller.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="0d2c6-105">Du kan snabbt skapa och fråga dokument och nyckel/värde-diagrammet databaser, som omfattas av hello global distributionsplatsen och skala horisontellt funktionerna i hello kärnan i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="0d2c6-106">Den här kursen visar hur toocreate ett Azure DB som Cosmos-kontot med hello Azure-portalen och sedan skapa ett dokument databas och samling med en [partitionsnyckel](documentdb-partition-data.md#partition-keys) med hello [DocumentDB .NET API](documentdb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-106">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal, and then create a document database and collection with a [partition key](documentdb-partition-data.md#partition-keys) using hello [DocumentDB .NET API](documentdb-introduction.md).</span></span> <span data-ttu-id="0d2c6-107">Genom att definiera en partitionsnyckel när du skapar en samling kan förbereds programmet tooscale utan problem när dina data växer.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-107">By defining a partition key when you create a collection, your application is prepared tooscale effortlessly as your data grows.</span></span> 

<span data-ttu-id="0d2c6-108">Den här självstudiekursen omfattar hello följande uppgifter med hjälp av hello [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="0d2c6-108">This tutorial covers hello following tasks by using hello [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0d2c6-109">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="0d2c6-109">Create an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="0d2c6-110">Skapa en databas och samling med en partitionsnyckel</span><span class="sxs-lookup"><span data-stu-id="0d2c6-110">Create a database and collection with a partition key</span></span>
> * <span data-ttu-id="0d2c6-111">Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="0d2c6-111">Create JSON documents</span></span>
> * <span data-ttu-id="0d2c6-112">Uppdatera ett dokument</span><span class="sxs-lookup"><span data-stu-id="0d2c6-112">Update a document</span></span>
> * <span data-ttu-id="0d2c6-113">Fråga partitionerade samlingar</span><span class="sxs-lookup"><span data-stu-id="0d2c6-113">Query partitioned collections</span></span>
> * <span data-ttu-id="0d2c6-114">Köra lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="0d2c6-114">Run stored procedures</span></span>
> * <span data-ttu-id="0d2c6-115">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="0d2c6-115">Delete a document</span></span>
> * <span data-ttu-id="0d2c6-116">Ta bort en databas</span><span class="sxs-lookup"><span data-stu-id="0d2c6-116">Delete a database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d2c6-117">Krav</span><span class="sxs-lookup"><span data-stu-id="0d2c6-117">Prerequisites</span></span>
<span data-ttu-id="0d2c6-118">Kontrollera att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="0d2c6-118">Please make sure you have hello following:</span></span>

* <span data-ttu-id="0d2c6-119">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-119">An active Azure account.</span></span> <span data-ttu-id="0d2c6-120">Om du inte har ett kan du registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-120">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="0d2c6-121">Du kan också använda hello [Azure Cosmos DB emulatorn](local-emulator.md) för den här självstudiekursen om du vill att toouse en lokal miljö som emulerar hello Azure DocumentDB-tjänsten för utveckling.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-121">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial if you'd like toouse a local environment that emulates hello Azure DocumentDB service for development purposes.</span></span>
* <span data-ttu-id="0d2c6-122">[Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-122">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="0d2c6-123">Skapa ett Azure Cosmos DB-konto</span><span class="sxs-lookup"><span data-stu-id="0d2c6-123">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="0d2c6-124">Börja med att skapa ett Azure DB som Cosmos-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-124">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>

> [!TIP]
> * <span data-ttu-id="0d2c6-125">Redan har ett Azure DB som Cosmos-konto?</span><span class="sxs-lookup"><span data-stu-id="0d2c6-125">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="0d2c6-126">I så fall, kan du gå vidare för[konfigurera Visual Studio-lösning](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="0d2c6-126">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="0d2c6-127">Hade du ett Azure DocumentDB-konto?</span><span class="sxs-lookup"><span data-stu-id="0d2c6-127">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="0d2c6-128">Om ditt konto är nu en Azure DB som Cosmos-konto och du gå vidare för så[konfigurera Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-128">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="0d2c6-129">Om du använder hello Azure Cosmos DB-emulatorn, följ hello stegen i [Azure Cosmos DB emulatorn](local-emulator.md) toosetup hello emulatorn och gå vidare för[ställa in din Visual Studio-lösning](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-129">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="0d2c6-130"><a id="SetupVS"></a>Konfigurera din Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="0d2c6-130"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="0d2c6-131">Öppna **Visual Studio** på datorn.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-131">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="0d2c6-132">På hello **filen** väljer du **ny**, och välj sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-132">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="0d2c6-133">I hello **nytt projekt** markerar **mallar** / **Visual C#** / **Konsolapp (.NET Framework)**namnge projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-133">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="0d2c6-134">![Skärmbild som visar hello-fönstret nytt projekt](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="0d2c6-134">![Screen shot of hello New Project window](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span></span>

4. <span data-ttu-id="0d2c6-135">I hello **Solution Explorer**, högerklicka på den nya konsolappen, som är under din Visual Studio-lösning, och klicka sedan på **hantera NuGet-paket...**</span><span class="sxs-lookup"><span data-stu-id="0d2c6-135">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Skärmbild som visar hello höger Clicked menyn för hello projekt](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="0d2c6-137">I hello **NuGet** klickar du på **Bläddra**, och skriv **documentdb** i hello sökrutan.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-137">In hello **NuGet** tab, click **Browse**, and type **documentdb** in hello search box.</span></span>
<!---stopped here--->
6. <span data-ttu-id="0d2c6-138">I hello resultat hitta **Microsoft.Azure.DocumentDB** och på **installera**.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-138">Within hello results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="0d2c6-139">hello paket-ID för hello Azure Cosmos DB-klientbiblioteket är [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-139">hello package ID for hello Azure Cosmos DB Client Library is [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span></span>
   <span data-ttu-id="0d2c6-140">![Skärmbild som visar hello NuGet-menyn för att hitta Azure Cosmos DB klient-SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="0d2c6-140">![Screen shot of hello NuGet Menu for finding Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="0d2c6-141">Om du får ett meddelande om att granska ändringar toohello lösning, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-141">If you get a message about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="0d2c6-142">Om du får ett meddelande om godkännande av licens klickar du på **Jag godkänner**.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-142">If you get a message about license acceptance, click **I accept**.</span></span>

## <span data-ttu-id="0d2c6-143"><a id="Connect"></a>Lägga till referenser tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="0d2c6-143"><a id="Connect"></a>Add references tooyour project</span></span>
<span data-ttu-id="0d2c6-144">hello återstående steg i den här självstudiekursen ange hello DocumentDB API kod kodavsnitt toocreate och uppdatera Azure Cosmos DB resurser som krävs i projektet.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-144">hello remaining steps in this tutorial provide hello DocumentDB API code snippets required toocreate and update Azure Cosmos DB resources in your project.</span></span>

<span data-ttu-id="0d2c6-145">Lägg först till dessa referenser tooyour program.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-145">First, add these references tooyour application.</span></span>
<!---These aren't added by default when you install hello pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <span data-ttu-id="0d2c6-146"><a id="add-references"></a>Anslut appen</span><span class="sxs-lookup"><span data-stu-id="0d2c6-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="0d2c6-147">Lägg till nedanstående två konstanter och dina *klienten* variabeln i ditt program.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-147">Next, add these two constants and your *client* variable in your application.</span></span>

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

<span data-ttu-id="0d2c6-148">Sedan head tillbaka toohello [Azure-portalen](https://portal.azure.com) tooretrieve din slutpunkts-URL och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-148">Then, head back toohello [Azure portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="0d2c6-149">hello slutpunkts-URL och primärnyckel behövs för ditt program toounderstand där tooconnect till med Azure Cosmos DB tootrust appens anslutning.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-149">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="0d2c6-150">I hello Azure portal, navigerar tooyour Azure Cosmos DB konto, klickar på **nycklar**, och klicka sedan på **skrivskyddad nycklar**.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-150">In hello Azure portal, navigate tooyour Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span>

<span data-ttu-id="0d2c6-151">Kopiera hello URI från hello-portalen och via `<your endpoint URL>` i hello program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-151">Copy hello URI from hello portal and paste it over `<your endpoint URL>` in hello program.cs file.</span></span> <span data-ttu-id="0d2c6-152">Kopiera hello PRIMÄRNYCKEL från hello-portalen och klistra in den över `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-152">Then copy hello PRIMARY KEY from hello portal and paste it over `<your primary key>`.</span></span> <span data-ttu-id="0d2c6-153">Vara säker på att tooremove hello `<` och `>` från värden.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-153">Be sure tooremove hello `<` and `>` from your values.</span></span>

![Skärmbild som visar hello Azure-portalen som används av hello NoSQL-självstudiekursen toocreate ett C#-konsolprogram.](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <span data-ttu-id="0d2c6-156"><a id="instantiate"></a>Skapa en instans av hello DocumentClient</span><span class="sxs-lookup"><span data-stu-id="0d2c6-156"><a id="instantiate"></a>Instantiate hello DocumentClient</span></span>

<span data-ttu-id="0d2c6-157">Nu skapa en ny instans av hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-157">Now, create a new instance of hello **DocumentClient**.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
```

## <span data-ttu-id="0d2c6-158"><a id="create-database"></a>Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="0d2c6-158"><a id="create-database"></a>Create a database</span></span>

<span data-ttu-id="0d2c6-159">Skapa sedan en Azure-Cosmos-DB [databasen](documentdb-resources.md#databases) med hjälp av hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metod eller [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metod för hello  **DocumentClient** klass från hello [.NET DocumentDB SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-159">Next, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class from hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="0d2c6-160">En databas är hello logisk behållare för JSON dokumentlagring, partitionerad över samlingarna.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-160">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a><span data-ttu-id="0d2c6-161">Besluta om en partitionsnyckel</span><span class="sxs-lookup"><span data-stu-id="0d2c6-161">Decide on a partition key</span></span> 

<span data-ttu-id="0d2c6-162">Samlingar är behållare för att lagra dokument.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-162">Collections are containers for storing documents.</span></span> <span data-ttu-id="0d2c6-163">De logiska resurser och kan [omfattar en eller flera fysiska partitioner](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-163">They are logical resources and can [span one or more physical partitions](partition-data.md).</span></span> <span data-ttu-id="0d2c6-164">En [partitionsnyckel](documentdb-partition-data.md) är en egenskap (eller sökväg) i dokument som har använt toodistribute data mellan hello servrar eller partitioner.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-164">A [partition key](documentdb-partition-data.md) is a property (or path) within your documents that is used toodistribute your data among hello servers or partitions.</span></span> <span data-ttu-id="0d2c6-165">Alla dokument med samma partitionsnyckel lagras i hello hello samma partition.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-165">All documents with hello same partition key are stored in hello same partition.</span></span> 

<span data-ttu-id="0d2c6-166">Fastställa en partitionsnyckel är ett viktigt beslut toomake innan du skapar en samling.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-166">Determining a partition key is an important decision toomake before you create a collection.</span></span> <span data-ttu-id="0d2c6-167">Partitionsnycklar är en egenskap (sökväg) i dokument som kan vara används av Azure Cosmos DB toodistribute data mellan flera servrar eller partitioner.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-167">Partition keys are a property (or path) within your documents that can be used by Azure Cosmos DB toodistribute your data among multiple servers or partitions.</span></span> <span data-ttu-id="0d2c6-168">Cosmos DB hashar hello partitionsnyckelvärde och använder hello hashas resultatet toodetermine hello partition i vilket toostore hello-dokument.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-168">Cosmos DB hashes hello partition key value and uses hello hashed result toodetermine hello partition in which toostore hello document.</span></span> <span data-ttu-id="0d2c6-169">Alla dokument med samma partitionsnyckel lagras i hello hello samma partition och partitionsnycklar kan inte ändras när en samling har skapats.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-169">All documents with hello same partition key are stored in hello same partition, and partition keys cannot be changed once a collection is created.</span></span> 

<span data-ttu-id="0d2c6-170">Den här självstudiekursen kommer vi tooset hello partitionsnyckel för`/deviceId` så som hello alla hello-data för en enskild enhet lagras i en enda partition.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-170">For this tutorial, we're going tooset hello partition key too`/deviceId` so that hello all hello data for a single device is stored in a single partition.</span></span> <span data-ttu-id="0d2c6-171">Du vill toochoose en partitionsnyckel som har ett stort antal värden, som används vid om hello samma frekvens tooensure Cosmos DB kan belastningsutjämna när dina data växer och uppnå hello totala genomflödet i hello samling.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-171">You want toochoose a partition key that has a large number of values, each of which are used at about hello same frequency tooensure Cosmos DB can load balance as your data grows and achieve hello full throughput of hello collection.</span></span> 

<span data-ttu-id="0d2c6-172">Mer information om partitionering finns [hur toopartition och skala i Azure Cosmos DB?](partition-data.md)</span><span class="sxs-lookup"><span data-stu-id="0d2c6-172">For more information about partitioning, see [How toopartition and scale in Azure Cosmos DB?](partition-data.md)</span></span> 

## <span data-ttu-id="0d2c6-173"><a id="CreateColl"></a>Skapa en samling</span><span class="sxs-lookup"><span data-stu-id="0d2c6-173"><a id="CreateColl"></a>Create a collection</span></span> 

<span data-ttu-id="0d2c6-174">Nu när vi vet våra partitionsnyckel `/deviceId`, kan du skapa en [samling](documentdb-resources.md#collections) med hjälp av hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metod eller [ CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-174">Now that we know our partition key, `/deviceId`, lets create a [collection](documentdb-resources.md#collections) by using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="0d2c6-175">En samling är en behållare för JSON-dokument och alla associerade JavaScript-programlogik.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-175">A collection is a container of JSON documents and any associated JavaScript application logic.</span></span> 

> [!WARNING]
> <span data-ttu-id="0d2c6-176">Skapa en samling har prissättning effekter, som du reserverar genomströmning för hello programmet toocommunicate med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-176">Creating a collection has pricing implications, as you are reserving throughput for hello application toocommunicate with Azure Cosmos DB.</span></span> <span data-ttu-id="0d2c6-177">Mer information finns våra [sida med priser](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="0d2c6-177">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

```csharp
// Collection for device telemetry. Here hello JSON property deviceId is used  
// as hello partition key toospread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

<span data-ttu-id="0d2c6-178">Den här metoden gör en REST-API anropa tooAzure Cosmos DB och hello tjänsten tillhandahåller ett antal partitioner baserat på hello begärda genomflöde.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-178">This method makes a REST API call tooAzure Cosmos DB, and hello service provisions a number of partitions based on hello requested throughput.</span></span> <span data-ttu-id="0d2c6-179">Du kan ändra hello genomströmningen i en samling som prestandan måste utvecklas med hjälp av hello SDK eller hello [Azure-portalen](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-179">You can change hello throughput of a collection as your performance needs evolve using hello SDK or hello [Azure portal](set-throughput.md).</span></span>

## <span data-ttu-id="0d2c6-180"><a id="CreateDoc"></a>Skapa JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="0d2c6-180"><a id="CreateDoc"></a>Create JSON documents</span></span>
<span data-ttu-id="0d2c6-181">Vi infoga vissa JSON-dokument i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-181">Let's insert some JSON documents into Azure Cosmos DB.</span></span> <span data-ttu-id="0d2c6-182">En [dokument](documentdb-resources.md#documents) kan skapas med hjälp av hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metod för hello **DocumentClient** klass.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-182">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="0d2c6-183">Dokument är användardefinierat (godtyckligt) JSON-innehåll.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-183">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="0d2c6-184">Detta exempel på klass innehåller en enhet läsning och en anropet tooCreateDocumentAsync tooinsert en ny enhet som läses in en samling.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-184">This sample class contains a device reading, and a call tooCreateDocumentAsync tooinsert a new device reading into a collection.</span></span>

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here hello partition key is extracted 
// as "XMS-0001" based on hello collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a><span data-ttu-id="0d2c6-185">Läsa data</span><span class="sxs-lookup"><span data-stu-id="0d2c6-185">Read data</span></span>

<span data-ttu-id="0d2c6-186">Nu ska vi läsa hello dokumentet av sin partitionsnyckel och Id hello ReadDocumentAsync metoden.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-186">Let's read hello document by its partition key and Id using hello ReadDocumentAsync method.</span></span> <span data-ttu-id="0d2c6-187">Observera att hello läsningar innehåller ett värde för PartitionKey (motsvarande toohello `x-ms-documentdb-partitionkey` huvudet i begäran i hello REST-API).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-187">Note that hello reads include a PartitionKey value (corresponding toohello `x-ms-documentdb-partitionkey` request header in hello REST API).</span></span>

```csharp
// Read document. Needs hello partition key and hello Id toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a><span data-ttu-id="0d2c6-188">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="0d2c6-188">Update data</span></span>

<span data-ttu-id="0d2c6-189">Nu ska vi uppdatera vissa data hello ReplaceDocumentAsync metoden.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-189">Now let's update some data using hello ReplaceDocumentAsync method.</span></span>

```csharp
// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a><span data-ttu-id="0d2c6-190">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="0d2c6-190">Delete data</span></span>

<span data-ttu-id="0d2c6-191">Nu kan du ta bort ett dokument med partitionsnyckel och id med hjälp av hello DeleteDocumentAsync metod.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-191">Now lets delete a document by partition key and id by using hello DeleteDocumentAsync method.</span></span>

```csharp
// Delete a document. hello partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a><span data-ttu-id="0d2c6-192">Fråga partitionerade samlingar</span><span class="sxs-lookup"><span data-stu-id="0d2c6-192">Query partitioned collections</span></span>

<span data-ttu-id="0d2c6-193">När du frågar efter data i partitionerade samlingar, Azure Cosmos DB automatiskt hello vägar toohello frågepartitioner motsvarande toohello partitionsnyckelvärden som angavs i hello filter (om det finns några).</span><span class="sxs-lookup"><span data-stu-id="0d2c6-193">When you query data in partitioned collections, Azure Cosmos DB automatically routes hello query toohello partitions corresponding toohello partition key values specified in hello filter (if there are any).</span></span> <span data-ttu-id="0d2c6-194">Den här frågan är till exempel routade toojust hello partition som innehåller hello partitionsnyckel ”XMS 0001”.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-194">For example, this query is routed toojust hello partition containing hello partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="0d2c6-195">hello följande fråga har inte ett filter på hello partitionsnyckel (DeviceId) och fanned ut tooall partitioner där den körs mot hello partition index.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-195">hello following query does not have a filter on hello partition key (DeviceId) and is fanned out tooall partitions where it is executed against hello partition's index.</span></span> <span data-ttu-id="0d2c6-196">Observera att du har toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` i hello REST-API) toohave hello SDK tooexecute en fråga över partitioner.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-196">Note that you have toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in hello REST API) toohave hello SDK tooexecute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a><span data-ttu-id="0d2c6-197">Parallell frågekörning</span><span class="sxs-lookup"><span data-stu-id="0d2c6-197">Parallel query execution</span></span>
<span data-ttu-id="0d2c6-198">hello Azure Cosmos DB DocumentDB SDK: er 1.9.0 och ovan stöd parallell körning frågealternativ, som gör att du tooperform låg latens frågor mot partitionerade samlingar, även när de behöver tootouch ett stort antal partitioner.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-198">hello Azure Cosmos DB DocumentDB SDKs 1.9.0 and above support parallel query execution options, which allow you tooperform low latency queries against partitioned collections, even when they need tootouch a large number of partitions.</span></span> <span data-ttu-id="0d2c6-199">Följande fråga hello är till exempel konfigurerade toorun parallellt över partitioner.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-199">For example, hello following query is configured toorun in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="0d2c6-200">Du kan hantera parallell frågekörning genom att justera hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="0d2c6-200">You can manage parallel query execution by tuning hello following parameters:</span></span>

* <span data-ttu-id="0d2c6-201">Genom att ange `MaxDegreeOfParallelism`, du kan styra hello grad av parallellitet d.v.s. hello högsta tillåtna antalet samtidiga nätverket anslutningar toohello mängdens partitioner.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-201">By setting `MaxDegreeOfParallelism`, you can control hello degree of parallelism i.e., hello maximum number of simultaneous network connections toohello collection's partitions.</span></span> <span data-ttu-id="0d2c6-202">Om du väljer för 1, hanteras hello grad av parallellitet av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-202">If you set this too-1, hello degree of parallelism is managed by hello SDK.</span></span> <span data-ttu-id="0d2c6-203">Om hello `MaxDegreeOfParallelism` har inte angetts eller ange too0, vilket är standardvärdet för hello finns ett enda nätverk anslutning toohello mängdens partitioner.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-203">If hello `MaxDegreeOfParallelism` is not specified or set too0, which is hello default value, there will be a single network connection toohello collection's partitions.</span></span>
* <span data-ttu-id="0d2c6-204">Genom att ange `MaxBufferedItemCount`, du kan handlar av frågan latens och klientsidan minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-204">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="0d2c6-205">Om du utelämnar den här parametern eller Ställ in för 1 hanteras hello antal objekt som buffras under parallell frågekörning av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-205">If you omit this parameter or set this too-1, hello number of items buffered during parallel query execution is managed by hello SDK.</span></span>

<span data-ttu-id="0d2c6-206">Angivna hello samma tillstånd mängden hello en parallell fråga returnerar resultat i hello samma ordning som seriella körning.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-206">Given hello same state of hello collection, a parallel query will return results in hello same order as in serial execution.</span></span> <span data-ttu-id="0d2c6-207">När du utför fråga cross-partition som innehåller sortering (ORDER BY och/eller TOP), hello DocumentDB SDK skickar hello fråga parallellt över partitioner och sammanfogar delvis sorterade resulterar i hello klienten sida tooproduce globalt sorterade resultat.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-207">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), hello DocumentDB SDK issues hello query in parallel across partitions and merges partially sorted results in hello client side tooproduce globally ordered results.</span></span>

## <a name="execute-stored-procedures"></a><span data-ttu-id="0d2c6-208">Köra lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="0d2c6-208">Execute stored procedures</span></span>
<span data-ttu-id="0d2c6-209">Slutligen kan du köra atomiska transaktioner mot dokument med hello samma enhets-ID, t.ex. Om du underhålla mängder eller hello senaste tillståndet för en enhet i ett enskilt dokument genom att lägga till hello följande kod tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-209">Lastly, you can execute atomic transactions against documents with hello same device ID, e.g. if you're maintaining aggregates or hello latest state of a device in a single document by adding hello following code tooyour project.</span></span>

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

<span data-ttu-id="0d2c6-210">Och det!</span><span class="sxs-lookup"><span data-stu-id="0d2c6-210">And that's it!</span></span> <span data-ttu-id="0d2c6-211">de är hello huvudkomponenterna i ett Azure DB som Cosmos-program som använder en partition viktiga tooefficiently skala Datadistribution över partitioner.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-211">those are hello main components of an Azure Cosmos DB application that uses a partition key tooefficiently scale data distribution across partitions.</span></span>  

## <a name="clean-up-resources"></a><span data-ttu-id="0d2c6-212">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="0d2c6-212">Clean up resources</span></span>

<span data-ttu-id="0d2c6-213">Om du inte kommer toocontinue toouse den här appen, tar du bort alla resurser som skapats av den här kursen i hello Azure-portalen med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0d2c6-213">If you're not going toocontinue toouse this app, delete all resources created by this tutorial in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="0d2c6-214">Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello unika namn hello resurs du skapat.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-214">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello unique name of hello resource you created.</span></span> 
2. <span data-ttu-id="0d2c6-215">På din resurs gruppen klickar du på **ta bort**typnamn hello för hello resurs toodelete i hello textrutan och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-215">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d2c6-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d2c6-216">Next steps</span></span>

<span data-ttu-id="0d2c6-217">I den här kursen har du gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="0d2c6-217">In this tutorial, you've done hello following:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="0d2c6-218">Skapa ett Azure DB som Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="0d2c6-218">Created an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="0d2c6-219">Skapa en databas och samling med en partitionsnyckel</span><span class="sxs-lookup"><span data-stu-id="0d2c6-219">Created a database and collection with a partition key</span></span>
> * <span data-ttu-id="0d2c6-220">Skapade JSON-dokument</span><span class="sxs-lookup"><span data-stu-id="0d2c6-220">Created JSON documents</span></span>
> * <span data-ttu-id="0d2c6-221">Uppdatera ett dokument</span><span class="sxs-lookup"><span data-stu-id="0d2c6-221">Updated a document</span></span>
> * <span data-ttu-id="0d2c6-222">Efterfrågade partitionerade samlingar</span><span class="sxs-lookup"><span data-stu-id="0d2c6-222">Queried partitioned collections</span></span>
> * <span data-ttu-id="0d2c6-223">Körde en lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="0d2c6-223">Ran a stored procedure</span></span>
> * <span data-ttu-id="0d2c6-224">Ta bort ett dokument</span><span class="sxs-lookup"><span data-stu-id="0d2c6-224">Deleted a document</span></span>
> * <span data-ttu-id="0d2c6-225">Ta bort en databas</span><span class="sxs-lookup"><span data-stu-id="0d2c6-225">Deleted a database</span></span>

<span data-ttu-id="0d2c6-226">Du kan nu fortsätta toohello nästa kurs och importera ytterligare data tooyour Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="0d2c6-226">You can now proceed toohello next tutorial and import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="0d2c6-227">Importera data till Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0d2c6-227">Import data into Azure Cosmos DB</span></span>](import-data.md)
