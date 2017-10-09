---
title: aaaMulti huvuddatabasen arkitekturer med Azure Cosmos DB | Microsoft Docs
description: "Läs mer om hur toodesign programarkitekturer med lokala läser och skriver över flera geografiska områden med Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 706ced74-ea67-45dd-a7de-666c3c893687
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3269c8405afe16f75db69b42e576fe76e00a8e16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a><span data-ttu-id="52184-103">Flera master replikerad globalt databasarkitekturer med Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="52184-103">Multi-master globally replicated database architectures with Azure Cosmos DB</span></span>
<span data-ttu-id="52184-104">Azure Cosmos-DB stöder NYCKELFÄRDIGT [globala replikering](distribute-data-globally.md), vilket gör att du toodistribute toomultiple dataområden med låg latens åtkomst var som helst i hello arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="52184-104">Azure Cosmos DB supports turnkey [global replication](distribute-data-globally.md), which allows you toodistribute data toomultiple regions with low latency access anywhere in hello workload.</span></span> <span data-ttu-id="52184-105">Den här modellen används ofta för utgivaren/konsumenten arbetsbelastningar där det finns en skrivare i en geografisk region och globalt distribuerade läsare i andra (skrivskyddad) regioner.</span><span class="sxs-lookup"><span data-stu-id="52184-105">This model is commonly used for publisher/consumer workloads where there is a writer in a single geographic region and globally distributed readers in other (read) regions.</span></span> 

<span data-ttu-id="52184-106">Du kan också använda Azure Cosmos DB globala replikering stödja toobuild program som skrivare och läsare globalt distribueras.</span><span class="sxs-lookup"><span data-stu-id="52184-106">You can also use Azure Cosmos DB's global replication support toobuild applications in which writers and readers are globally distributed.</span></span> <span data-ttu-id="52184-107">Det här dokumentet beskrivs ett mönster som gör det möjligt att uppnå lokala Skriv- och lokala läsbehörighet för distribuerade skrivare med hjälp av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="52184-107">This document outlines a pattern that enables achieving local write and local read access for distributed writers using Azure Cosmos DB.</span></span>

## <span data-ttu-id="52184-108"><a id="ExampleScenario"></a>Publiceringen av innehållet - exempel</span><span class="sxs-lookup"><span data-stu-id="52184-108"><a id="ExampleScenario"></a>Content Publishing - an example scenario</span></span>
<span data-ttu-id="52184-109">Nu ska vi titta på ett verkligt scenario toodescribe hur du kan använda global flera-region/flera-master läsa skriva mönster med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="52184-109">Let's look at a real world scenario toodescribe how you can use globally distributed multi-region/multi-master read write patterns with Azure Cosmos DB.</span></span> <span data-ttu-id="52184-110">Överväg att en innehåll publishing plattform som bygger på Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="52184-110">Consider a content publishing platform built on Azure Cosmos DB.</span></span> <span data-ttu-id="52184-111">Här följer några krav som den här plattformen måste uppfylla för en bra användarupplevelse för både utgivare och konsumenter.</span><span class="sxs-lookup"><span data-stu-id="52184-111">Here are some requirements that this platform must meet for a great user experience for both publishers and consumers.</span></span>

* <span data-ttu-id="52184-112">Både författare och prenumeranter kan sträcka sig över hello world</span><span class="sxs-lookup"><span data-stu-id="52184-112">Both authors and subscribers are spread over hello world</span></span> 
* <span data-ttu-id="52184-113">Författare måste publicera (skriva) artiklar tootheir lokala (närmaste) region</span><span class="sxs-lookup"><span data-stu-id="52184-113">Authors must publish (write) articles tootheir local (closest) region</span></span>
* <span data-ttu-id="52184-114">Författare har läsare-/ prenumeranter sina artiklar som är fördelade på hello jordglob.</span><span class="sxs-lookup"><span data-stu-id="52184-114">Authors have readers/subscribers of their articles who are distributed across hello globe.</span></span> 
* <span data-ttu-id="52184-115">Prenumeranter bör få ett meddelande när nya artiklar publiceras.</span><span class="sxs-lookup"><span data-stu-id="52184-115">Subscribers should get a notification when new articles are published.</span></span>
* <span data-ttu-id="52184-116">Prenumeranter måste vara kan tooread artiklar från sin lokala region.</span><span class="sxs-lookup"><span data-stu-id="52184-116">Subscribers must be able tooread articles from their local region.</span></span> <span data-ttu-id="52184-117">De bör också vara kan tooadd granskningar toothese artiklar.</span><span class="sxs-lookup"><span data-stu-id="52184-117">They should also be able tooadd reviews toothese articles.</span></span> 
* <span data-ttu-id="52184-118">Alla inklusive hello författare hello artiklar ska kunna visa alla hello granskningar bifogade tooarticles från region med en lokal.</span><span class="sxs-lookup"><span data-stu-id="52184-118">Anyone including hello author of hello articles should be able view all hello reviews attached tooarticles from a local region.</span></span> 

<span data-ttu-id="52184-119">Under förutsättning att miljontals användare och utgivare med miljarder artiklar, snart vi har problem med tooconfront hello skalan tillsammans med garanterar ort åtkomst.</span><span class="sxs-lookup"><span data-stu-id="52184-119">Assuming millions of consumers and publishers with billions of articles, soon we have tooconfront hello problems of scale along with guaranteeing locality of access.</span></span> <span data-ttu-id="52184-120">Precis som med de flesta skalbarhetsproblem med, ligger hello lösning i en bra strategi för partitionering.</span><span class="sxs-lookup"><span data-stu-id="52184-120">As with most scalability problems, hello solution lies in a good partitioning strategy.</span></span> <span data-ttu-id="52184-121">Nu ska vi titta på hur toomodel artiklar, granska och meddelanden som dokument, konfigurera Azure Cosmos DB konton och implementera en dataåtkomstnivå.</span><span class="sxs-lookup"><span data-stu-id="52184-121">Next, let's look at how toomodel articles, review, and notifications as documents, configure Azure Cosmos DB accounts, and implement a data access layer.</span></span> 

<span data-ttu-id="52184-122">Om du vill ha mer information om partitionering och partitionsnycklar toolearn finns [partitionering och skalning i Azure Cosmos DB](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="52184-122">If you would like toolearn more about partitioning and partition keys, see [Partitioning and Scaling in Azure Cosmos DB](partition-data.md).</span></span>

## <span data-ttu-id="52184-123"><a id="ModelingNotifications"></a>Modeling meddelanden</span><span class="sxs-lookup"><span data-stu-id="52184-123"><a id="ModelingNotifications"></a>Modeling notifications</span></span>
<span data-ttu-id="52184-124">Meddelanden är data feeds specifika tooa användare.</span><span class="sxs-lookup"><span data-stu-id="52184-124">Notifications are data feeds specific tooa user.</span></span> <span data-ttu-id="52184-125">Därför är hello åtkomstmönster för meddelanden dokument alltid hello led i enanvändarläge.</span><span class="sxs-lookup"><span data-stu-id="52184-125">Therefore, hello access patterns for notifications documents are always in hello context of single user.</span></span> <span data-ttu-id="52184-126">Du kan till exempel ”efter en tooa aviseringsanvändare” eller ”hämta alla meddelanden för en viss användare”.</span><span class="sxs-lookup"><span data-stu-id="52184-126">For example, you would "post a notification tooa user" or "fetch all notifications for a given user".</span></span> <span data-ttu-id="52184-127">Därför hello föredra partitionering nyckeln för den här typen är `UserId`.</span><span class="sxs-lookup"><span data-stu-id="52184-127">So, hello optimal choice of partitioning key for this type would be `UserId`.</span></span>

    class Notification 
    { 
        // Unique ID for Notification. 
        public string Id { get; set; }

        // hello user Id for which notification is addressed to. 
        public string UserId { get; set; }

        // hello partition Key for hello resource. 
        public string PartitionKey 
        { 
            get 
            { 
                return this.UserId; 
            }
        }

        // Subscription for which this notification is raised. 
        public string SubscriptionFilter { get; set; }

        // Subject of hello notification. 
        public string ArticleId { get; set; } 
    }

## <span data-ttu-id="52184-128"><a id="ModelingSubscriptions"></a>Modeling prenumerationer</span><span class="sxs-lookup"><span data-stu-id="52184-128"><a id="ModelingSubscriptions"></a>Modeling subscriptions</span></span>
<span data-ttu-id="52184-129">Prenumerationer kan skapas för olika kriterier som en viss kategori med artiklar för specifika intresseområden eller en specifik utgivare.</span><span class="sxs-lookup"><span data-stu-id="52184-129">Subscriptions can be created for various criteria like a specific category of articles of interest, or a specific publisher.</span></span> <span data-ttu-id="52184-130">Därför hello `SubscriptionFilter` är ett bra alternativ för partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="52184-130">Hence hello `SubscriptionFilter` is a good choice for partition key.</span></span>

    class Subscriptions 
    { 
        // Unique ID for Subscription 
        public string Id { get; set; }

        // Subscription source. Could be Author | Category etc. 
        public string SubscriptionFilter { get; set; }

        // subscribing User. 
        public string UserId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.SubscriptionFilter; 
            } 
        } 
    }

## <span data-ttu-id="52184-131"><a id="ModelingArticles"></a>Modeling artiklar</span><span class="sxs-lookup"><span data-stu-id="52184-131"><a id="ModelingArticles"></a>Modeling articles</span></span>
<span data-ttu-id="52184-132">När en artikel identifieras via meddelanden efterföljande frågor vanligtvis utifrån hello `Article.Id`.</span><span class="sxs-lookup"><span data-stu-id="52184-132">Once an article is identified through notifications, subsequent queries are typically based on hello `Article.Id`.</span></span> <span data-ttu-id="52184-133">Att välja `Article.Id` som partition hello nyckeln därför ger bästa hello-distribution för att lagra artiklar i en samling Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="52184-133">Choosing `Article.Id` as partition hello key thus provides hello best distribution for storing articles inside an Azure Cosmos DB collection.</span></span> 

    class Article 
    { 
        // Unique ID for Article 
        public string Id { get; set; }
        
        public string PartitionKey 
        { 
            get 
            { 
                return this.Id; 
            } 
        }
        
        // Author of hello article
        public string Author { get; set; }

        // Category/genre of hello article
        public string Category { get; set; }

        // Tags associated with hello article
        public string[] Tags { get; set; }

        // Title of hello article
        public string Title { get; set; }
        
        //... 
    }

## <span data-ttu-id="52184-134"><a id="ModelingReviews"></a>Modellering granskar</span><span class="sxs-lookup"><span data-stu-id="52184-134"><a id="ModelingReviews"></a>Modeling reviews</span></span>
<span data-ttu-id="52184-135">Som artiklar, granskningar främst skrivs och läsas i hello kontext för artikeln.</span><span class="sxs-lookup"><span data-stu-id="52184-135">Like articles, reviews are mostly written and read in hello context of article.</span></span> <span data-ttu-id="52184-136">Att välja `ArticleId` som en partition nyckel ger bästa distribution och effektiv åtkomst av granskningar som är kopplad till artikeln.</span><span class="sxs-lookup"><span data-stu-id="52184-136">Choosing `ArticleId` as a partition key provides best distribution and efficient access of reviews associated with article.</span></span> 

    class Review 
    { 
        // Unique ID for Review 
        public string Id { get; set; }

        // Article Id of hello review 
        public string ArticleId { get; set; }

        public string PartitionKey 
        { 
            get 
            { 
                return this.ArticleId; 
            } 
        }
        
        //Reviewer Id 
        public string UserId { get; set; }
        public string ReviewText { get; set; }
        
        public int Rating { get; set; } }
    }

## <span data-ttu-id="52184-137"><a id="DataAccessMethods"></a>Metoder för dataåtkomst lager</span><span class="sxs-lookup"><span data-stu-id="52184-137"><a id="DataAccessMethods"></a>Data access layer methods</span></span>
<span data-ttu-id="52184-138">Nu ska vi titta på hello viktigaste data åtkomstmetoder vi behöver tooimplement.</span><span class="sxs-lookup"><span data-stu-id="52184-138">Now let's look at hello main data access methods we need tooimplement.</span></span> <span data-ttu-id="52184-139">Här är hello lista över metoder som hello `ContentPublishDatabase` måste:</span><span class="sxs-lookup"><span data-stu-id="52184-139">Here's hello list of methods that hello `ContentPublishDatabase` needs:</span></span>

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <span data-ttu-id="52184-140"><a id="Architecture"></a>Konfigurationen av Azure DB Cosmos-konto</span><span class="sxs-lookup"><span data-stu-id="52184-140"><a id="Architecture"></a>Azure Cosmos DB account configuration</span></span>
<span data-ttu-id="52184-141">tooguarantee lokala läser och skriver, vi måste partitionera data inte bara på partitionsnyckel, men även baserat på hello geografiska åtkomst till regioner.</span><span class="sxs-lookup"><span data-stu-id="52184-141">tooguarantee local reads and writes, we must partition data not just on partition key, but also based on hello geographical access pattern into regions.</span></span> <span data-ttu-id="52184-142">hello modellen är beroende av att ha ett geo-replikerade Azure Cosmos DB databaskonto för varje region.</span><span class="sxs-lookup"><span data-stu-id="52184-142">hello model relies on having a geo-replicated Azure Cosmos DB database account for each region.</span></span> <span data-ttu-id="52184-143">Till exempel med två regioner är här en konfiguration för flera regioner skrivningar:</span><span class="sxs-lookup"><span data-stu-id="52184-143">For example, with two regions, here's a setup for multi-region writes:</span></span>

| <span data-ttu-id="52184-144">Kontonamn</span><span class="sxs-lookup"><span data-stu-id="52184-144">Account Name</span></span> | <span data-ttu-id="52184-145">Skriva Region</span><span class="sxs-lookup"><span data-stu-id="52184-145">Write Region</span></span> | <span data-ttu-id="52184-146">Läs Region</span><span class="sxs-lookup"><span data-stu-id="52184-146">Read Region</span></span> |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

<span data-ttu-id="52184-147">hello följande diagram visar hur läsning och skrivning utförs i en typisk program med de här inställningarna:</span><span class="sxs-lookup"><span data-stu-id="52184-147">hello following diagram shows how reads and writes are performed in a typical application with this setup:</span></span>

![Azure multimaster Cosmos-DB-arkitektur](./media/multi-region-writers/multi-master.png)

<span data-ttu-id="52184-149">Här är ett kodfragment som visar hur tooinitialize hello klienter i en DAL körs i hello `West US` region.</span><span class="sxs-lookup"><span data-stu-id="52184-149">Here is a code snippet showing how tooinitialize hello clients in a DAL running in hello `West US` region.</span></span>
    
    ConnectionPolicy writeClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    writeClientPolicy.PreferredLocations.Add(LocationNames.WestUS);
    writeClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

    DocumentClient writeClient = new DocumentClient(
        new Uri("https://contentpubdatabase-usa.documents.azure.com"), 
        writeRegionAuthKey,
        writeClientPolicy);

    ConnectionPolicy readClientPolicy = new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp };
    readClientPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
    readClientPolicy.PreferredLocations.Add(LocationNames.WestUS);

    DocumentClient readClient = new DocumentClient(
        new Uri("https://contentpubdatabase-europe.documents.azure.com"),
        readRegionAuthKey,
        readClientPolicy);

<span data-ttu-id="52184-150">Med hello före installationen, vidarebefordra hello dataåtkomstnivå alla skrivningar toohello lokalt konto baserat på där den distribueras.</span><span class="sxs-lookup"><span data-stu-id="52184-150">With hello preceding setup, hello data access layer can forward all writes toohello local account based on where it is deployed.</span></span> <span data-ttu-id="52184-151">Läser utförs av läsning från både konton tooget hello globala vyn av data.</span><span class="sxs-lookup"><span data-stu-id="52184-151">Reads are performed by reading from both accounts tooget hello global view of data.</span></span> <span data-ttu-id="52184-152">Den här metoden kan vara utökad tooas många regioner som krävs.</span><span class="sxs-lookup"><span data-stu-id="52184-152">This approach can be extended tooas many regions as required.</span></span> <span data-ttu-id="52184-153">Här är till exempel en installation med tre geografiska områden:</span><span class="sxs-lookup"><span data-stu-id="52184-153">For example, here's a setup with three geographic regions:</span></span>

| <span data-ttu-id="52184-154">Kontonamn</span><span class="sxs-lookup"><span data-stu-id="52184-154">Account Name</span></span> | <span data-ttu-id="52184-155">Skriva Region</span><span class="sxs-lookup"><span data-stu-id="52184-155">Write Region</span></span> | <span data-ttu-id="52184-156">Läs Region 1</span><span class="sxs-lookup"><span data-stu-id="52184-156">Read Region 1</span></span> | <span data-ttu-id="52184-157">Läs Region 2</span><span class="sxs-lookup"><span data-stu-id="52184-157">Read Region 2</span></span> |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <span data-ttu-id="52184-158"><a id="DataAccessImplementation"></a>Data access layer implementering</span><span class="sxs-lookup"><span data-stu-id="52184-158"><a id="DataAccessImplementation"></a>Data access layer implementation</span></span>
<span data-ttu-id="52184-159">Nu ska vi titta på hello implementering av hello dataåtkomstnivå (DAL) för ett program med två skrivbar regioner.</span><span class="sxs-lookup"><span data-stu-id="52184-159">Now let's look at hello implementation of hello data access layer (DAL) for an application with two writable regions.</span></span> <span data-ttu-id="52184-160">hello DAL måste implementera hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52184-160">hello DAL must implement hello following steps:</span></span>

* <span data-ttu-id="52184-161">Skapa flera instanser av `DocumentClient` för varje konto.</span><span class="sxs-lookup"><span data-stu-id="52184-161">Create multiple instances of `DocumentClient` for each account.</span></span> <span data-ttu-id="52184-162">Med två regioner varje DAL-instans har en `writeClient` och ett `readClient`.</span><span class="sxs-lookup"><span data-stu-id="52184-162">With two regions, each DAL instance has one `writeClient` and one `readClient`.</span></span> 
* <span data-ttu-id="52184-163">Baserat på hello distribueras den region där programmet hello konfigurera hello slutpunkter för `writeclient` och `readClient`.</span><span class="sxs-lookup"><span data-stu-id="52184-163">Based on hello deployed region of hello application, configure hello endpoints for `writeclient` and `readClient`.</span></span> <span data-ttu-id="52184-164">Till exempel hello DAL distribuerats i `West US` använder `contentpubdatabase-usa.documents.azure.com` för att utföra skrivningar.</span><span class="sxs-lookup"><span data-stu-id="52184-164">For example, hello DAL deployed in `West US` uses `contentpubdatabase-usa.documents.azure.com` for performing writes.</span></span> <span data-ttu-id="52184-165">hello DAL distribuerats i `NorthEurope` använder `contentpubdatabase-europ.documents.azure.com` för skrivningar.</span><span class="sxs-lookup"><span data-stu-id="52184-165">hello DAL deployed in `NorthEurope` uses `contentpubdatabase-europ.documents.azure.com` for writes.</span></span>

<span data-ttu-id="52184-166">Med hello före installationen, kan du implementera hello metoder för dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="52184-166">With hello preceding setup, hello data access methods can be implemented.</span></span> <span data-ttu-id="52184-167">Skriva operations vidarebefordra hello skrivåtgärder toohello motsvarande `writeClient`.</span><span class="sxs-lookup"><span data-stu-id="52184-167">Write operations forward hello write toohello corresponding `writeClient`.</span></span>

    public async Task CreateSubscriptionAsync(string userId, string category)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Subscriptions
        {
            UserId = userId,
            SubscriptionFilter = category
        });
    }

    public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating)
    {
        await this.writeClient.CreateDocumentAsync(this.contentCollection, new Review
        {
            UserId = userId,
            ArticleId = articleId,
            ReviewText = reviewText,
            Rating = rating
        });
    }

<span data-ttu-id="52184-168">För att läsa meddelanden och granskningar, måste du läsa från både regioner och union hello resultat som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="52184-168">For reading notifications and reviews, you must read from both regions and union hello results as shown in hello following snippet:</span></span>

    public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId)
    {
        IDocumentQuery<Notification> writeAccountNotification = (
            from notification in this.writeClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();
        
        IDocumentQuery<Notification> readAccountNotification = (
            from notification in this.readClient.CreateDocumentQuery<Notification>(this.contentCollection) 
            where notification.UserId == userId 
            select notification).AsDocumentQuery();

        List<Notification> notifications = new List<Notification>();

        while (writeAccountNotification.HasMoreResults || readAccountNotification.HasMoreResults)
        {
            IList<Task<FeedResponse<Notification>>> results = new List<Task<FeedResponse<Notification>>>();

            if (writeAccountNotification.HasMoreResults)
            {
                results.Add(writeAccountNotification.ExecuteNextAsync<Notification>());
            }

            if (readAccountNotification.HasMoreResults)
            {
                results.Add(readAccountNotification.ExecuteNextAsync<Notification>());
            }

            IList<FeedResponse<Notification>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Notification> feed in notificationFeedResult)
            {
                notifications.AddRange(feed);
            }
        }
        return notifications;
    }

    public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId)
    {
        IDocumentQuery<Review> writeAccountReviews = (
            from review in this.writeClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();
        
        IDocumentQuery<Review> readAccountReviews = (
            from review in this.readClient.CreateDocumentQuery<Review>(this.contentCollection) 
            where review.ArticleId == articleId 
            select review).AsDocumentQuery();

        List<Review> reviews = new List<Review>();
        
        while (writeAccountReviews.HasMoreResults || readAccountReviews.HasMoreResults)
        {
            IList<Task<FeedResponse<Review>>> results = new List<Task<FeedResponse<Review>>>();

            if (writeAccountReviews.HasMoreResults)
            {
                results.Add(writeAccountReviews.ExecuteNextAsync<Review>());
            }

            if (readAccountReviews.HasMoreResults)
            {
                results.Add(readAccountReviews.ExecuteNextAsync<Review>());
            }

            IList<FeedResponse<Review>> notificationFeedResult = await Task.WhenAll(results);

            foreach (FeedResponse<Review> feed in notificationFeedResult)
            {
                reviews.AddRange(feed);
            }
        }

        return reviews;
    }

<span data-ttu-id="52184-169">Genom att välja en bra partitionsnyckel och statiska konto-baserade partitionering, kan du därför uppnå flera regioner lokala skrivningar och läsningar med hjälp av Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="52184-169">Thus, by choosing a good partitioning key and static account-based partitioning, you can achieve multi-region local writes and reads using Azure Cosmos DB.</span></span>

## <span data-ttu-id="52184-170"><a id="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="52184-170"><a id="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="52184-171">I den här artikeln beskrivs hur du kan använda global flera regioner läsa skriva mönster med Azure Cosmos-databasen med publiceringen av innehållet som ett exempelscenario.</span><span class="sxs-lookup"><span data-stu-id="52184-171">In this article, we described how you can use globally distributed multi-region read write patterns with Azure Cosmos DB using content publishing as a sample scenario.</span></span>

* <span data-ttu-id="52184-172">Lär dig mer om hur Azure Cosmos DB stöder [global distributionsplatsen](distribute-data-globally.md)</span><span class="sxs-lookup"><span data-stu-id="52184-172">Learn about how Azure Cosmos DB supports [global distribution](distribute-data-globally.md)</span></span>
* <span data-ttu-id="52184-173">Lär dig mer om [automatisk och manuell växling vid fel i Azure Cosmos DB](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="52184-173">Learn about [automatic and manual failovers in Azure Cosmos DB](regional-failover.md)</span></span>
* <span data-ttu-id="52184-174">Lär dig mer om [global konsekvens med Azure Cosmos DB](consistency-levels.md)</span><span class="sxs-lookup"><span data-stu-id="52184-174">Learn about [global consistency with Azure Cosmos DB](consistency-levels.md)</span></span>
* <span data-ttu-id="52184-175">Utveckla med flera regioner med hello [Azure DB Cosmos - API för DocumentDB](tutorial-global-distribution-documentdb.md)</span><span class="sxs-lookup"><span data-stu-id="52184-175">Develop with multiple regions using hello [Azure Cosmos DB - DocumentDB API](tutorial-global-distribution-documentdb.md)</span></span>
* <span data-ttu-id="52184-176">Utveckla med flera regioner med hello [Azure DB Cosmos - MongoDB-API](tutorial-global-distribution-MongoDB.md)</span><span class="sxs-lookup"><span data-stu-id="52184-176">Develop with multiple regions using hello [Azure Cosmos DB - MongoDB API](tutorial-global-distribution-MongoDB.md)</span></span>
* <span data-ttu-id="52184-177">Utveckla med flera regioner med hello [Azure DB Cosmos - tabellen API](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="52184-177">Develop with multiple regions using hello [Azure Cosmos DB - Table API](tutorial-global-distribution-table.md)</span></span>
