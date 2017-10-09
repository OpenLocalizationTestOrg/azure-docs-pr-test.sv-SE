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
# <a name="multi-master-globally-replicated-database-architectures-with-azure-cosmos-db"></a>Flera master replikerad globalt databasarkitekturer med Azure Cosmos DB
Azure Cosmos-DB stöder NYCKELFÄRDIGT [globala replikering](distribute-data-globally.md), vilket gör att du toodistribute toomultiple dataområden med låg latens åtkomst var som helst i hello arbetsbelastning. Den här modellen används ofta för utgivaren/konsumenten arbetsbelastningar där det finns en skrivare i en geografisk region och globalt distribuerade läsare i andra (skrivskyddad) regioner. 

Du kan också använda Azure Cosmos DB globala replikering stödja toobuild program som skrivare och läsare globalt distribueras. Det här dokumentet beskrivs ett mönster som gör det möjligt att uppnå lokala Skriv- och lokala läsbehörighet för distribuerade skrivare med hjälp av Azure Cosmos DB.

## <a id="ExampleScenario"></a>Publiceringen av innehållet - exempel
Nu ska vi titta på ett verkligt scenario toodescribe hur du kan använda global flera-region/flera-master läsa skriva mönster med Azure Cosmos DB. Överväg att en innehåll publishing plattform som bygger på Azure Cosmos DB. Här följer några krav som den här plattformen måste uppfylla för en bra användarupplevelse för både utgivare och konsumenter.

* Både författare och prenumeranter kan sträcka sig över hello world 
* Författare måste publicera (skriva) artiklar tootheir lokala (närmaste) region
* Författare har läsare-/ prenumeranter sina artiklar som är fördelade på hello jordglob. 
* Prenumeranter bör få ett meddelande när nya artiklar publiceras.
* Prenumeranter måste vara kan tooread artiklar från sin lokala region. De bör också vara kan tooadd granskningar toothese artiklar. 
* Alla inklusive hello författare hello artiklar ska kunna visa alla hello granskningar bifogade tooarticles från region med en lokal. 

Under förutsättning att miljontals användare och utgivare med miljarder artiklar, snart vi har problem med tooconfront hello skalan tillsammans med garanterar ort åtkomst. Precis som med de flesta skalbarhetsproblem med, ligger hello lösning i en bra strategi för partitionering. Nu ska vi titta på hur toomodel artiklar, granska och meddelanden som dokument, konfigurera Azure Cosmos DB konton och implementera en dataåtkomstnivå. 

Om du vill ha mer information om partitionering och partitionsnycklar toolearn finns [partitionering och skalning i Azure Cosmos DB](partition-data.md).

## <a id="ModelingNotifications"></a>Modeling meddelanden
Meddelanden är data feeds specifika tooa användare. Därför är hello åtkomstmönster för meddelanden dokument alltid hello led i enanvändarläge. Du kan till exempel ”efter en tooa aviseringsanvändare” eller ”hämta alla meddelanden för en viss användare”. Därför hello föredra partitionering nyckeln för den här typen är `UserId`.

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

## <a id="ModelingSubscriptions"></a>Modeling prenumerationer
Prenumerationer kan skapas för olika kriterier som en viss kategori med artiklar för specifika intresseområden eller en specifik utgivare. Därför hello `SubscriptionFilter` är ett bra alternativ för partitionsnyckel.

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

## <a id="ModelingArticles"></a>Modeling artiklar
När en artikel identifieras via meddelanden efterföljande frågor vanligtvis utifrån hello `Article.Id`. Att välja `Article.Id` som partition hello nyckeln därför ger bästa hello-distribution för att lagra artiklar i en samling Azure Cosmos DB. 

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

## <a id="ModelingReviews"></a>Modellering granskar
Som artiklar, granskningar främst skrivs och läsas i hello kontext för artikeln. Att välja `ArticleId` som en partition nyckel ger bästa distribution och effektiv åtkomst av granskningar som är kopplad till artikeln. 

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

## <a id="DataAccessMethods"></a>Metoder för dataåtkomst lager
Nu ska vi titta på hello viktigaste data åtkomstmetoder vi behöver tooimplement. Här är hello lista över metoder som hello `ContentPublishDatabase` måste:

    class ContentPublishDatabase 
    { 
        public async Task CreateSubscriptionAsync(string userId, string category);
    
        public async Task<IEnumerable<Notification>> ReadNotificationFeedAsync(string userId);
    
        public async Task<Article> ReadArticleAsync(string articleId);
    
        public async Task WriteReviewAsync(string articleId, string userId, string reviewText, int rating);
    
        public async Task<IEnumerable<Review>> ReadReviewsAsync(string articleId); 
    }

## <a id="Architecture"></a>Konfigurationen av Azure DB Cosmos-konto
tooguarantee lokala läser och skriver, vi måste partitionera data inte bara på partitionsnyckel, men även baserat på hello geografiska åtkomst till regioner. hello modellen är beroende av att ha ett geo-replikerade Azure Cosmos DB databaskonto för varje region. Till exempel med två regioner är här en konfiguration för flera regioner skrivningar:

| Kontonamn | Skriva Region | Läs Region |
| --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |

hello följande diagram visar hur läsning och skrivning utförs i en typisk program med de här inställningarna:

![Azure multimaster Cosmos-DB-arkitektur](./media/multi-region-writers/multi-master.png)

Här är ett kodfragment som visar hur tooinitialize hello klienter i en DAL körs i hello `West US` region.
    
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

Med hello före installationen, vidarebefordra hello dataåtkomstnivå alla skrivningar toohello lokalt konto baserat på där den distribueras. Läser utförs av läsning från både konton tooget hello globala vyn av data. Den här metoden kan vara utökad tooas många regioner som krävs. Här är till exempel en installation med tre geografiska områden:

| Kontonamn | Skriva Region | Läs Region 1 | Läs Region 2 |
| --- | --- | --- | --- |
| `contentpubdatabase-usa.documents.azure.com` | `West US` |`North Europe` |`Southeast Asia` |
| `contentpubdatabase-europe.documents.azure.com` | `North Europe` |`West US` |`Southeast Asia` |
| `contentpubdatabase-asia.documents.azure.com` | `Southeast Asia` |`North Europe` |`West US` |

## <a id="DataAccessImplementation"></a>Data access layer implementering
Nu ska vi titta på hello implementering av hello dataåtkomstnivå (DAL) för ett program med två skrivbar regioner. hello DAL måste implementera hello följande steg:

* Skapa flera instanser av `DocumentClient` för varje konto. Med två regioner varje DAL-instans har en `writeClient` och ett `readClient`. 
* Baserat på hello distribueras den region där programmet hello konfigurera hello slutpunkter för `writeclient` och `readClient`. Till exempel hello DAL distribuerats i `West US` använder `contentpubdatabase-usa.documents.azure.com` för att utföra skrivningar. hello DAL distribuerats i `NorthEurope` använder `contentpubdatabase-europ.documents.azure.com` för skrivningar.

Med hello före installationen, kan du implementera hello metoder för dataåtkomst. Skriva operations vidarebefordra hello skrivåtgärder toohello motsvarande `writeClient`.

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

För att läsa meddelanden och granskningar, måste du läsa från både regioner och union hello resultat som visas i följande fragment hello:

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

Genom att välja en bra partitionsnyckel och statiska konto-baserade partitionering, kan du därför uppnå flera regioner lokala skrivningar och läsningar med hjälp av Azure Cosmos DB.

## <a id="NextSteps"></a>Nästa steg
I den här artikeln beskrivs hur du kan använda global flera regioner läsa skriva mönster med Azure Cosmos-databasen med publiceringen av innehållet som ett exempelscenario.

* Lär dig mer om hur Azure Cosmos DB stöder [global distributionsplatsen](distribute-data-globally.md)
* Lär dig mer om [automatisk och manuell växling vid fel i Azure Cosmos DB](regional-failover.md)
* Lär dig mer om [global konsekvens med Azure Cosmos DB](consistency-levels.md)
* Utveckla med flera regioner med hello [Azure DB Cosmos - API för DocumentDB](tutorial-global-distribution-documentdb.md)
* Utveckla med flera regioner med hello [Azure DB Cosmos - MongoDB-API](tutorial-global-distribution-MongoDB.md)
* Utveckla med flera regioner med hello [Azure DB Cosmos - tabellen API](tutorial-global-distribution-table.md)
