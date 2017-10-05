---
title: "Modeling dokumentdata för en NoSQL-databas | Microsoft Docs"
description: "Lär dig mer om att utforma data för NoSQL-databaser"
keywords: Modeling data
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig1
documentationcenter: 
ms.assetid: 69521eb9-590b-403c-9b36-98253a4c88b5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2016
ms.author: arramac
ms.openlocfilehash: 16c387fe574243544cf54cf283c7713ddcaa1942
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="modeling-document-data-for-nosql-databases"></a><span data-ttu-id="27949-104">Modeling dokumentdata för NoSQL-databaser</span><span class="sxs-lookup"><span data-stu-id="27949-104">Modeling document data for NoSQL databases</span></span>
<span data-ttu-id="27949-105">Medan schemafria databaser som Azure Cosmos DB gör det super enkelt att omfatta ändringar i datamodellen du bör fortfarande tillbringar vissa tid tro om dina data.</span><span class="sxs-lookup"><span data-stu-id="27949-105">While schema-free databases, like Azure Cosmos DB, make it super easy to embrace changes to your data model you should still spend some time thinking about your data.</span></span> 

<span data-ttu-id="27949-106">Hur data ska lagras?</span><span class="sxs-lookup"><span data-stu-id="27949-106">How is data going to be stored?</span></span> <span data-ttu-id="27949-107">Hur programmet ska hämta och frågar efter data?</span><span class="sxs-lookup"><span data-stu-id="27949-107">How is your application going to retrieve and query data?</span></span> <span data-ttu-id="27949-108">Är programmet läsa tjockt, eller Skriv tunga?</span><span class="sxs-lookup"><span data-stu-id="27949-108">Is your application read heavy, or write heavy?</span></span> 

<span data-ttu-id="27949-109">När du har läst den här artikeln kommer du att kunna svara på följande frågor:</span><span class="sxs-lookup"><span data-stu-id="27949-109">After reading this article, you will be able to answer the following questions:</span></span>

* <span data-ttu-id="27949-110">Hur ska tycker om ett dokument i en dokumentdatabasen?</span><span class="sxs-lookup"><span data-stu-id="27949-110">How should I think about a document in a document database?</span></span>
* <span data-ttu-id="27949-111">Vad är datamodellering och varför ska jag hand?</span><span class="sxs-lookup"><span data-stu-id="27949-111">What is data modeling and why should I care?</span></span> 
* <span data-ttu-id="27949-112">Hur skiljer sig modellering data i en databas för dokument i en relationsdatabas?</span><span class="sxs-lookup"><span data-stu-id="27949-112">How is modeling data in a document database different to a relational database?</span></span>
* <span data-ttu-id="27949-113">Hur jag express data relationer i en icke-relationell databas?</span><span class="sxs-lookup"><span data-stu-id="27949-113">How do I express data relationships in a non-relational database?</span></span>
* <span data-ttu-id="27949-114">När bädda data och när länkar jag data?</span><span class="sxs-lookup"><span data-stu-id="27949-114">When do I embed data and when do I link to data?</span></span>

## <a name="embedding-data"></a><span data-ttu-id="27949-115">Bädda in data</span><span class="sxs-lookup"><span data-stu-id="27949-115">Embedding data</span></span>
<span data-ttu-id="27949-116">När du startar modellerar data i ett dokumentarkiv, till exempel Azure Cosmos DB försöker behandla entiteter som **fristående dokument** representeras i JSON.</span><span class="sxs-lookup"><span data-stu-id="27949-116">When you start modeling data in a document store, such as Azure Cosmos DB, try to treat your entities as **self-contained documents** represented in JSON.</span></span>

<span data-ttu-id="27949-117">Innan vi fördjupa dig för mycket mer Låt oss ta ett par steg tillbaka och ta en titt på hur vi kan modellen något i en relationsdatabas, ett ämne som många av oss redan är bekant med.</span><span class="sxs-lookup"><span data-stu-id="27949-117">Before we dive in too much further, let us take a few steps back and have a look at how we might model something in a relational database, a subject many of us are already familiar with.</span></span> <span data-ttu-id="27949-118">I följande exempel visas hur en person kan lagras i en relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="27949-118">The following example shows how a person might be stored in a relational database.</span></span> 

![Relationsdatabas modellen](./media/documentdb-modeling-data/relational-data-model.png)

<span data-ttu-id="27949-120">När du arbetar med relationsdatabaser vi har blivit vilka undervisning förekommer i år att normalisera, normalisera, normalisera.</span><span class="sxs-lookup"><span data-stu-id="27949-120">When working with relational databases, we've been taught for years to normalize, normalize, normalize.</span></span>

<span data-ttu-id="27949-121">Normaliserar data normalt omfattar tar en entitet, till exempel en person, och dela upp frågan separata delar av data.</span><span class="sxs-lookup"><span data-stu-id="27949-121">Normalizing your data typically involves taking an entity, such as a person, and breaking it down in to discrete pieces of data.</span></span> <span data-ttu-id="27949-122">I exemplet ovan kan kan en person ha flera poster för kontakt-information samt flera poster.</span><span class="sxs-lookup"><span data-stu-id="27949-122">In the example above, a person can have multiple contact detail records as well as multiple address records.</span></span> <span data-ttu-id="27949-123">Vi även ta ytterligare ett steg och redovisar kontaktuppgifter genom att extrahera ytterligare vanliga fält som typen.</span><span class="sxs-lookup"><span data-stu-id="27949-123">We even go one step further and break down contact details by further extracting common fields like a type.</span></span> <span data-ttu-id="27949-124">Samma för adress, varje post här har en typ som *Start* eller *företag*</span><span class="sxs-lookup"><span data-stu-id="27949-124">Same for address, each record here has a type like *Home* or *Business*</span></span> 

<span data-ttu-id="27949-125">Den guidar premise när normalisera data är att **lagra inte redundanta data** på varje registrera och i stället referera till data.</span><span class="sxs-lookup"><span data-stu-id="27949-125">The guiding premise when normalizing data is to **avoid storing redundant data** on each record and rather refer to data.</span></span> <span data-ttu-id="27949-126">I det här exemplet för att läsa en person med sina kontaktuppgifter och adresser, måste du använda kopplingar för att effektivt aggregera dina data vid körning.</span><span class="sxs-lookup"><span data-stu-id="27949-126">In this example, to read a person, with all their contact details and addresses, you need to use JOINS to effectively aggregate your data at run time.</span></span>

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

<span data-ttu-id="27949-127">Uppdatering av en person med kontaktinformation och adresser kräver skrivåtgärder över många enskilda tabeller.</span><span class="sxs-lookup"><span data-stu-id="27949-127">Updating a single person with their contact details and addresses requires write operations across many individual tables.</span></span> 

<span data-ttu-id="27949-128">Nu ska vi ta en titt på hur vi skulle modeller för samma data som en fristående enhet i en databas för dokumentet.</span><span class="sxs-lookup"><span data-stu-id="27949-128">Now let's take a look at how we would model the same data as a self-contained entity in a document database.</span></span>

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {            
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email: "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ] 
    }

<span data-ttu-id="27949-129">Med metoden ovan har vi nu **Avnormaliserade** personen som registrerar var vi **inbäddade** all information som rör till den här personen som deras kontaktuppgifter och adresser i ett enda JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-129">Using the approach above we have now **denormalized** the person record where we **embedded** all the information relating to this person, such as their contact details and addresses, in to a single JSON document.</span></span>
<span data-ttu-id="27949-130">Vi har också möjlighet att utföra åtgärder som att ha kontaktuppgifterna med olika former helt eftersom vi inte är begränsad till ett fast schema.</span><span class="sxs-lookup"><span data-stu-id="27949-130">In addition, because we're not confined to a fixed schema we have the flexibility to do things like having contact details of different shapes entirely.</span></span> 

<span data-ttu-id="27949-131">Hämtar en fullständig personpost från databasen är nu en enda Läsåtgärd mot en enda samling och för ett enskilt dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-131">Retrieving a complete person record from the database is now a single read operation against a single collection and for a single document.</span></span> <span data-ttu-id="27949-132">Uppdatera en personpost med kontaktuppgifter och adresser, är också en enda skrivåtgärd mot ett enskilt dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-132">Updating a person record, with their contact details and addresses, is also a single write operation against a single document.</span></span>

<span data-ttu-id="27949-133">Av denormalizing data, kan programmet behöva utfärda färre frågor och uppdateringar för att slutföra vanliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="27949-133">By denormalizing data, your application may need to issue fewer queries and updates to complete common operations.</span></span> 

### <a name="when-to-embed"></a><span data-ttu-id="27949-134">Bädda in</span><span class="sxs-lookup"><span data-stu-id="27949-134">When to embed</span></span>
<span data-ttu-id="27949-135">I allmänhet använder inbäddade data modeller när:</span><span class="sxs-lookup"><span data-stu-id="27949-135">In general, use embedded data models when:</span></span>

* <span data-ttu-id="27949-136">Det finns **innehåller** relationer mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="27949-136">There are **contains** relationships between entities.</span></span>
* <span data-ttu-id="27949-137">Det finns **en till några** relationer mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="27949-137">There are **one-to-few** relationships between entities.</span></span>
* <span data-ttu-id="27949-138">Det finns inbäddade data som **ändras sällan**.</span><span class="sxs-lookup"><span data-stu-id="27949-138">There is embedded data that **changes infrequently**.</span></span>
* <span data-ttu-id="27949-139">Det är inbäddat data inte växa **utan gräns**.</span><span class="sxs-lookup"><span data-stu-id="27949-139">There is embedded data won't grow **without bound**.</span></span>
* <span data-ttu-id="27949-140">Det finns inbäddade data som är **integrerad** till data i ett dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-140">There is embedded data that is **integral** to data in a document.</span></span>

> [!NOTE]
> <span data-ttu-id="27949-141">Vanligtvis Avnormaliserade data modeller får du bättre **läsa** prestanda.</span><span class="sxs-lookup"><span data-stu-id="27949-141">Typically denormalized data models provide better **read** performance.</span></span>
> 
> 

### <a name="when-not-to-embed"></a><span data-ttu-id="27949-142">När du inte att bädda in</span><span class="sxs-lookup"><span data-stu-id="27949-142">When not to embed</span></span>
<span data-ttu-id="27949-143">Tumregel i dokumentet databas är att denormalize allt och bädda in alla data i ett dokument, kan detta leda till vissa situationer bör undvikas.</span><span class="sxs-lookup"><span data-stu-id="27949-143">While the rule of thumb in a document database is to denormalize everything and embed all data in to a single document, this can lead to some situations that should be avoided.</span></span>

<span data-ttu-id="27949-144">Ta den här JSON-fragment.</span><span class="sxs-lookup"><span data-stu-id="27949-144">Take this JSON snippet.</span></span>

    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

<span data-ttu-id="27949-145">Det kan vara vad en post entitet med inbäddade kommentarer skulle se ut som om vi modellera en typisk blogg eller CMS, system.</span><span class="sxs-lookup"><span data-stu-id="27949-145">This might be what a post entity with embedded comments would look like if we were modeling a typical blog, or CMS, system.</span></span> <span data-ttu-id="27949-146">Problemet med det här exemplet är att kommentarer matrisen **unbounded**, vilket innebär att det finns ingen () gräns för antalet kommentarer som en enda post kan ha.</span><span class="sxs-lookup"><span data-stu-id="27949-146">The problem with this example is that the comments array is **unbounded**, meaning that there is no (practical) limit to the number of comments any single post can have.</span></span> <span data-ttu-id="27949-147">Detta blir ett problem som storleken på dokumentet kan växa avsevärt.</span><span class="sxs-lookup"><span data-stu-id="27949-147">This will become a problem as the size of the document could grow significantly.</span></span>

<span data-ttu-id="27949-148">När storleken på dokumentet växer möjlighet att överföra data under överföring samt läsning och uppdatering av dokument i skala, påverkas.</span><span class="sxs-lookup"><span data-stu-id="27949-148">As the size of the document grows the ability to transmit the data over the wire as well as reading and updating the document, at scale, will be impacted.</span></span>

<span data-ttu-id="27949-149">I det här fallet kan det vara bättre att tänka på följande modell.</span><span class="sxs-lookup"><span data-stu-id="27949-149">In this case it would be better to consider the following model.</span></span>

    Post document:
    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from the field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": "1"
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

<span data-ttu-id="27949-150">Den här modellen har de senaste tre kommentarer inbäddade för sig själv, vilket är en matris med en fast bunden nu.</span><span class="sxs-lookup"><span data-stu-id="27949-150">This model has the three most recent comments embedded on the post itself, which is an array with a fixed bound this time.</span></span> <span data-ttu-id="27949-151">Andra kommentarer är grupperade i till batchar av 100 kommentarer och lagras i separata dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-151">The other comments are grouped in to batches of 100 comments and stored in separate documents.</span></span> <span data-ttu-id="27949-152">Storlek på batch valdes som 100 eftersom vårt fiktiva program används att läsa in 100 kommentarer i taget.</span><span class="sxs-lookup"><span data-stu-id="27949-152">The size of the batch was chosen as 100 because our fictitious application allows the user to load 100 comments at a time.</span></span>  

<span data-ttu-id="27949-153">Ett annat fall där inbäddning data inte är en bra idé är när den inbäddade data används ofta mellan dokument och kommer att ändras ofta.</span><span class="sxs-lookup"><span data-stu-id="27949-153">Another case where embedding data is not a good idea is when the embedded data is used often across documents and will change frequently.</span></span> 

<span data-ttu-id="27949-154">Ta den här JSON-fragment.</span><span class="sxs-lookup"><span data-stu-id="27949-154">Take this JSON snippet.</span></span>

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

<span data-ttu-id="27949-155">Detta kan vara en person portfölj.</span><span class="sxs-lookup"><span data-stu-id="27949-155">This could represent a person's stock portfolio.</span></span> <span data-ttu-id="27949-156">Vi har valt att bädda in lager informationen i varje portfölj dokumentet.</span><span class="sxs-lookup"><span data-stu-id="27949-156">We have chosen to embed the stock information in to each portfolio document.</span></span> <span data-ttu-id="27949-157">I en miljö där relaterade data ändras ofta kommer som en börs handel program, bädda in data som ändras ofta att innebära att du kontinuerligt uppdaterar varje portfölj dokumentet varje gång en börs säljs.</span><span class="sxs-lookup"><span data-stu-id="27949-157">In an environment where related data is changing frequently, like a stock trading application, embedding data that changes frequently is going to mean that you are constantly updating each portfolio document every time a stock is traded.</span></span>

<span data-ttu-id="27949-158">Börs *zaza* får säljas många hundratals gånger i en enda dag och tusentals användare kan ha *zaza* på sina portfölj.</span><span class="sxs-lookup"><span data-stu-id="27949-158">Stock *zaza* may be traded many hundreds of times in a single day and thousands of users could have *zaza* on their portfolio.</span></span> <span data-ttu-id="27949-159">Med en datamodell som anges ovan måste vi uppdatera tusentals portfölj dokument många gånger varje dag, vilket leder till ett system som inte skala mycket bra.</span><span class="sxs-lookup"><span data-stu-id="27949-159">With a data model like the above we would have to update many thousands of portfolio documents many times every day leading to a system that won't scale very well.</span></span> 

## <span data-ttu-id="27949-160"><a id="Refer"></a>Refererar till data</span><span class="sxs-lookup"><span data-stu-id="27949-160"><a id="Refer"></a>Referencing data</span></span>
<span data-ttu-id="27949-161">Därför bädda in data fungerar bra i många fall, men det är tydligt att det finns scenarier när denormalizing data medför flera problem än lönar.</span><span class="sxs-lookup"><span data-stu-id="27949-161">So, embedding data works nicely for many cases but it is clear that there are scenarios when denormalizing your data will cause more problems than it is worth.</span></span> <span data-ttu-id="27949-162">Så vad vi gör nu?</span><span class="sxs-lookup"><span data-stu-id="27949-162">So what do we do now?</span></span> 

<span data-ttu-id="27949-163">Relationsdatabaser är inte den enda plats där du kan skapa relationer mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="27949-163">Relational databases are not the only place where you can create relationships between entities.</span></span> <span data-ttu-id="27949-164">I en databas i dokumentet kan du ha information i ett dokument som verkligen är kopplad till data i andra dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-164">In a document database you can have information in one document that actually relates to data in other documents.</span></span> <span data-ttu-id="27949-165">Nu kan jag inte arbeta för ett även en minut att vi bygger system som skulle vara bättre passar relationsdatabaser i Azure Cosmos DB eller andra dokumentdatabasen, men enkla relationer är skadad och kan vara användbar.</span><span class="sxs-lookup"><span data-stu-id="27949-165">Now, I am not advocating for even one minute that we build systems that would be better suited to a relational database in Azure Cosmos DB, or any other document database, but simple relationships are fine and can be very useful.</span></span> 

<span data-ttu-id="27949-166">I JSON nedan vi valde att använda exempel på en portfölj från tidigare men den här gången avser vi lager artikeln på portfölj i stället för att bädda in den.</span><span class="sxs-lookup"><span data-stu-id="27949-166">In the JSON below we chose to use the example of a stock portfolio from earlier but this time we refer to the stock item on the portfolio instead of embedding it.</span></span> <span data-ttu-id="27949-167">Det här sättet när objektet lager som ändras ofta under dagen endast dokument som behöver uppdateras är i ett lager dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-167">This way, when the stock item changes frequently throughout the day the only document that needs to be updated is the single stock document.</span></span> 

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }

    Stock documents:
    {
        "id": "1",
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": "2",
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }


<span data-ttu-id="27949-168">En omedelbar Nackdelen med att den här metoden är dock om ditt program krävs för att visa information om varje lager som hålls kvar när du visar en persons portfölj; i det här fallet skulle du behöva göra flera resor till databasen för att läsa in informationen för varje lager dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-168">An immediate downside to this approach though is if your application is required to show information about each stock that is held when displaying a person's portfolio; in this case you would need to make multiple trips to the database to load the information for each stock document.</span></span> <span data-ttu-id="27949-169">Här har vi gjort ett beslut att förbättra effektiviteten för skrivåtgärder som inträffa ofta under dagen, men som i sin tur komprometteras på läsåtgärder som eventuellt få mindre påverkan på prestandan för viss systemet.</span><span class="sxs-lookup"><span data-stu-id="27949-169">Here we've made a decision to improve the efficiency of write operations, which happen frequently throughout the day, but in turn compromised on the read operations that potentially have less impact on the performance of this particular system.</span></span>

> [!NOTE]
> <span data-ttu-id="27949-170">Normaliserade datamodeller **kan kräva mer sändningar** till servern.</span><span class="sxs-lookup"><span data-stu-id="27949-170">Normalized data models **can require more round trips** to the server.</span></span>
> 
> 

### <a name="what-about-foreign-keys"></a><span data-ttu-id="27949-171">Nyheter om externa nycklar?</span><span class="sxs-lookup"><span data-stu-id="27949-171">What about foreign keys?</span></span>
<span data-ttu-id="27949-172">Eftersom det inte finns något begrepp om en begränsning, foreign key- eller på annat sätt, relationer mellan dokument som du har i dokument är effektivt ”svaga länkar” och kommer inte att verifiera genom att själva databasen.</span><span class="sxs-lookup"><span data-stu-id="27949-172">Because there is currently no concept of a constraint, foreign-key or otherwise, any inter-document relationships that you have in documents are effectively "weak links" and will not be verified by the database itself.</span></span> <span data-ttu-id="27949-173">Om du vill säkerställa att data i ett dokument refererar till verkligen finns måste du göra i ditt program, eller genom att använda serversidan utlösare eller lagrade procedurer på Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="27949-173">If you want to ensure that the data a document is referring to actually exists, then you need to do this in your application, or through the use of server-side triggers or stored procedures on Azure Cosmos DB.</span></span>

### <a name="when-to-reference"></a><span data-ttu-id="27949-174">När du ska referera till</span><span class="sxs-lookup"><span data-stu-id="27949-174">When to reference</span></span>
<span data-ttu-id="27949-175">I allmänhet använder normaliserade data modeller när:</span><span class="sxs-lookup"><span data-stu-id="27949-175">In general, use normalized data models when:</span></span>

* <span data-ttu-id="27949-176">Som representerar **en-till-många** relationer.</span><span class="sxs-lookup"><span data-stu-id="27949-176">Representing **one-to-many** relationships.</span></span>
* <span data-ttu-id="27949-177">Som representerar **många-till-många** relationer.</span><span class="sxs-lookup"><span data-stu-id="27949-177">Representing **many-to-many** relationships.</span></span>
* <span data-ttu-id="27949-178">Relaterade data **ändras ofta**.</span><span class="sxs-lookup"><span data-stu-id="27949-178">Related data **changes frequently**.</span></span>
* <span data-ttu-id="27949-179">Refererade data kan gå **unbounded**.</span><span class="sxs-lookup"><span data-stu-id="27949-179">Referenced data could be **unbounded**.</span></span>

> [!NOTE]
> <span data-ttu-id="27949-180">Vanligtvis normaliserar ger bättre **skriva** prestanda.</span><span class="sxs-lookup"><span data-stu-id="27949-180">Typically normalizing provides better **write** performance.</span></span>
> 
> 

### <a name="where-do-i-put-the-relationship"></a><span data-ttu-id="27949-181">Där placera relationen?</span><span class="sxs-lookup"><span data-stu-id="27949-181">Where do I put the relationship?</span></span>
<span data-ttu-id="27949-182">Tillväxten av relationen hjälper dig att avgöra i vilken dokumentet för att lagra referensen.</span><span class="sxs-lookup"><span data-stu-id="27949-182">The growth of the relationship will help determine in which document to store the reference.</span></span>

<span data-ttu-id="27949-183">Om vi tittar på JSON nedan som modeller utgivare och böcker.</span><span class="sxs-lookup"><span data-stu-id="27949-183">If we look at the JSON below that models publishers and books.</span></span>

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over the world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive in to Azure Cosmos DB" }

<span data-ttu-id="27949-184">Om antalet böcker per utgivaren är liten med begränsad tillväxt, kan det vara användbart att lagra book referens i publisher-dokument.</span><span class="sxs-lookup"><span data-stu-id="27949-184">If the number of the books per publisher is small with limited growth, then storing the book reference inside the publisher document may be useful.</span></span> <span data-ttu-id="27949-185">Men om antal böcker per utgivaren är unbounded skulle sedan denna datamodell leda till föränderliga, växande matriser, som i ovanstående exempel publisher dokumentet.</span><span class="sxs-lookup"><span data-stu-id="27949-185">However, if the number of books per publisher is unbounded, then this data model would lead to mutable, growing arrays, as in the example publisher document above.</span></span> 

<span data-ttu-id="27949-186">Växla runt lite skulle resultera i en modell som representerar fortfarande samma data, men nu undviker dessa stora föränderliga samlingar.</span><span class="sxs-lookup"><span data-stu-id="27949-186">Switching things around a bit would result in a model that still represents the same data but now avoids these large mutable collections.</span></span>

    Publisher document: 
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents: 
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over the world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive in to Azure Cosmos DB", "pub-id": "mspress"}

<span data-ttu-id="27949-187">I exemplet ovan har vi bort samlingen unbounded på utgivaren dokumentet.</span><span class="sxs-lookup"><span data-stu-id="27949-187">In the above example, we have dropped the unbounded collection on the publisher document.</span></span> <span data-ttu-id="27949-188">I stället vi bara har en en referens till en utgivare på varje bokdokument.</span><span class="sxs-lookup"><span data-stu-id="27949-188">Instead we just have a a reference to the publisher on each book document.</span></span>

### <a name="how-do-i-model-manymany-relationships"></a><span data-ttu-id="27949-189">Hur jag för att modellera många: många-relationer?</span><span class="sxs-lookup"><span data-stu-id="27949-189">How do I model many:many relationships?</span></span>
<span data-ttu-id="27949-190">I en relationsdatabas *många: många* relationer ofta modelleras med anslutning till tabeller som bara ansluta poster från andra tabeller tillsammans.</span><span class="sxs-lookup"><span data-stu-id="27949-190">In a relational database *many:many* relationships are often modeled with join tables, which just join records from other tables together.</span></span> 

![Koppla tabeller](./media/documentdb-modeling-data/join-table.png)

<span data-ttu-id="27949-192">Du kanske tro att replikera samma sak med hjälp av dokument och skapa en datamodell som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="27949-192">You might be tempted to replicate the same thing using documents and produce a data model that looks similar to the following.</span></span>

    Author documents: 
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive in to Azure Cosmos DB" }

    Joining documents: 
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

<span data-ttu-id="27949-193">Detta kan fungera.</span><span class="sxs-lookup"><span data-stu-id="27949-193">This would work.</span></span> <span data-ttu-id="27949-194">Dock kräver inläsning av antingen en författare med deras böcker eller läsa in en bok med dess upphovsman alltid minst två ytterligare frågor mot databasen.</span><span class="sxs-lookup"><span data-stu-id="27949-194">However, loading either an author with their books, or loading a book with its author, would always require at least two additional queries against the database.</span></span> <span data-ttu-id="27949-195">En fråga till anslutande dokumentet och sedan en annan fråga för att hämta det faktiska dokumentet är ansluten.</span><span class="sxs-lookup"><span data-stu-id="27949-195">One query to the joining document and then another query to fetch the actual document being joined.</span></span> 

<span data-ttu-id="27949-196">Om allt anslutning till tabellen gör fäster ihop två typer av data, varför inte släppa den helt?</span><span class="sxs-lookup"><span data-stu-id="27949-196">If all this join table is doing is gluing together two pieces of data, then why not drop it completely?</span></span>
<span data-ttu-id="27949-197">Tänk på följande.</span><span class="sxs-lookup"><span data-stu-id="27949-197">Consider the following.</span></span>

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in to Azure Cosmos DB", "authors": ["a2"]}

<span data-ttu-id="27949-198">Nu om jag har en författare jag vet vilka böcker som de har skrivit omedelbart och om jag hade ett book dokument läsas in I motsats vet ID innehålla.</span><span class="sxs-lookup"><span data-stu-id="27949-198">Now, if I had an author, I immediately know which books they have written, and conversely if I had a book document loaded I would know the ids of the author(s).</span></span> <span data-ttu-id="27949-199">Detta sparar den mellanliggande frågan mot anslutning till tabellen minskar antalet server förfrågningar programmet måste göra.</span><span class="sxs-lookup"><span data-stu-id="27949-199">This saves that intermediary query against the join table reducing the number of server round trips your application has to make.</span></span> 

## <span data-ttu-id="27949-200"><a id="WrapUp"></a>Hybrid datamodeller</span><span class="sxs-lookup"><span data-stu-id="27949-200"><a id="WrapUp"></a>Hybrid data models</span></span>
<span data-ttu-id="27949-201">Vi nu har tittat bädda in (eller denormalizing) och refererande (eller normaliserar) data har sina upsides och kompromisser har vi har sett.</span><span class="sxs-lookup"><span data-stu-id="27949-201">We've now looked embedding (or denormalizing) and referencing (or normalizing) data, each have their upsides and each have compromises as we have seen.</span></span> 

<span data-ttu-id="27949-202">Det behöver inte alltid vara antingen eller inte scared blanda saker lite.</span><span class="sxs-lookup"><span data-stu-id="27949-202">It doesn't always have to be either or, don't be scared to mix things up a little.</span></span> 

<span data-ttu-id="27949-203">Baserat på ditt programs specifika användningsmönster och arbetsbelastningar som det kan finnas fall där blanda inbäddad refererade klokt och kan leda till enklare programlogik med färre server förfrågningar men har ändå en bra nivå av prestanda.</span><span class="sxs-lookup"><span data-stu-id="27949-203">Based on your application's specific usage patterns and workloads there may be cases where mixing embedded and referenced data makes sense and could lead to simpler application logic with fewer server round trips while still maintaining a good level of performance.</span></span>

<span data-ttu-id="27949-204">Överväg följande JSON.</span><span class="sxs-lookup"><span data-stu-id="27949-204">Consider the following JSON.</span></span> 

    Author documents: 
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",        
        "countOfBooks": 3,
         "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "http://....png"}
            {"profile": "http://....png"}
            {"large": "http://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "http://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "http://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
        ]
    }

<span data-ttu-id="27949-205">Vi har här (mest) följt inbäddad modell, där data från andra enheter som är inbäddade i det översta dokumentet, men andra data refereras.</span><span class="sxs-lookup"><span data-stu-id="27949-205">Here we've (mostly) followed the embedded model, where data from other entities are embedded in the top-level document, but other data is referenced.</span></span> 

<span data-ttu-id="27949-206">Om du tittar på book dokumentet ser vi några intressanta fält när vi tittar på matrisen för författare.</span><span class="sxs-lookup"><span data-stu-id="27949-206">If you look at the book document, we can see a few interesting fields when we look at the array of authors.</span></span> <span data-ttu-id="27949-207">Det finns en *id* fält som är det fält som vi använder för att referera tillbaka till ett författare dokument, standard Övning i en normaliserade modell, men sedan vi har också *namn* och *thumbnailUrl*.</span><span class="sxs-lookup"><span data-stu-id="27949-207">There is an *id* field which is the field we use to refer back to an author document, standard practice in a normalized model, but then we also have *name* and *thumbnailUrl*.</span></span> <span data-ttu-id="27949-208">Vi kunde har precis har fastnat med *id* och lämnas programmet för att hämta ytterligare information den behövs från respektive författare dokumentet med hjälp av länken ””, men eftersom vårt program visar författarens namn och en miniatyrbild med alla böcker visas kan vi spara onödig kommunikation till servern per bok i en lista av denormalizing **vissa** data från författare.</span><span class="sxs-lookup"><span data-stu-id="27949-208">We could've just stuck with *id* and left the application to get any additional information it needed from the respective author document using the "link", but because our application displays the author's name and a thumbnail picture with every book displayed we can save a round trip to the server per book in a list by denormalizing **some** data from the author.</span></span>

<span data-ttu-id="27949-209">Kontrollera, om författarens namn ändras eller de ville uppdatera sina foto vi skulle behöva gå en uppdatering alla böcker de publicerade någonsin men detta är en godkänd beslut för vårt program baserat på antagandet att författare inte ändrar deras namn så ofta.</span><span class="sxs-lookup"><span data-stu-id="27949-209">Sure, if the author's name changed or they wanted to update their photo we'd have to go an update every book they ever published but for our application, based on the assumption that authors don't change their names very often, this is an acceptable design decision.</span></span>  

<span data-ttu-id="27949-210">Det finns i exemplet **före beräknade mängder** värden för att spara dyra bearbetning på en Läsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="27949-210">In the example there are **pre-calculated aggregates** values to save expensive processing on a read operation.</span></span> <span data-ttu-id="27949-211">I det här exemplet är några av data som är inbäddade i dokumentet författare data som beräknas vid körning.</span><span class="sxs-lookup"><span data-stu-id="27949-211">In the example, some of the data embedded in the author document is data that is calculated at run-time.</span></span> <span data-ttu-id="27949-212">Varje gång en ny bok publiceras, skapas en bokdokument **och** countOfBooks fältet är inställt på ett beräknat värde baserat på antalet book dokument som finns för en viss författare.</span><span class="sxs-lookup"><span data-stu-id="27949-212">Every time a new book is published, a book document is created **and** the countOfBooks field is set to a calculated value based on the number of book documents that exist for a particular author.</span></span> <span data-ttu-id="27949-213">Denna optimering är bra i skrivskyddade tunga system där vi har råd att utföra beräkningar på skrivningar för att optimera läsningar.</span><span class="sxs-lookup"><span data-stu-id="27949-213">This optimization would be good in read heavy systems where we can afford to do computations on writes in order to optimize reads.</span></span>

<span data-ttu-id="27949-214">Möjlighet att ha en modell med före beräknade fält är möjligt eftersom Azure Cosmos DB stöder **flera dokument transaktioner**.</span><span class="sxs-lookup"><span data-stu-id="27949-214">The ability to have a model with pre-calculated fields is made possible because Azure Cosmos DB supports **multi-document transactions**.</span></span> <span data-ttu-id="27949-215">Många NoSQL-lagringsplatser kan inte göra transaktioner mellan dokument och därför gör designbeslut, till exempel ”inkludera alltid allt”, på grund av den här begränsningen.</span><span class="sxs-lookup"><span data-stu-id="27949-215">Many NoSQL stores cannot do transactions across documents and therefore advocate design decisions, such as "always embed everything", due to this limitation.</span></span> <span data-ttu-id="27949-216">Du kan använda serversidan utlösare eller lagrade procedurer som Infoga böcker och uppdatera författarna alla inom en ACID-transaktion med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="27949-216">With Azure Cosmos DB, you can use server-side triggers, or stored procedures, that insert books and update authors all within an ACID transaction.</span></span> <span data-ttu-id="27949-217">Nu du inte **har** att bädda in allt i ett dokument bara för att se till att dina data förblir konsekventa.</span><span class="sxs-lookup"><span data-stu-id="27949-217">Now you don't **have** to embed everything in to one document just to be sure that your data remains consistent.</span></span>

## <span data-ttu-id="27949-218"><a name="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27949-218"><a name="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="27949-219">De största takeaways från den här artikeln är att förstå att datamodeller i en schemafria värld är precis lika viktig som någonsin.</span><span class="sxs-lookup"><span data-stu-id="27949-219">The biggest takeaways from this article is to understand that data modeling in a schema-free world is just as important as ever.</span></span> 

<span data-ttu-id="27949-220">Precis som det är inte ett enskilt sätt att representera en bit data på en skärm, är det inte ett enskilt sätt att modellera dina data.</span><span class="sxs-lookup"><span data-stu-id="27949-220">Just as there is no single way to represent a piece of data on a screen, there is no single way to model your data.</span></span> <span data-ttu-id="27949-221">Du behöver förstå ditt program och hur produceras, använda och bearbeta data.</span><span class="sxs-lookup"><span data-stu-id="27949-221">You need to understand your application and how it will produce, consume, and process the data.</span></span> <span data-ttu-id="27949-222">Genom att använda några av de riktlinjer som visas här kan du ange om hur du skapar en modell som åtgärdar omedelbara behov av ditt program.</span><span class="sxs-lookup"><span data-stu-id="27949-222">Then, by applying some of the guidelines presented here you can set about creating a model that addresses the immediate needs of your application.</span></span> <span data-ttu-id="27949-223">När dina program behöver ändra kan du utnyttja flexibiliteten hos en schemafria databas att omfatta som ändras och utvecklas datamodellen enkelt.</span><span class="sxs-lookup"><span data-stu-id="27949-223">When your applications need to change, you can leverage the flexibility of a schema-free database to embrace that change and evolve your data model easily.</span></span> 

<span data-ttu-id="27949-224">Om du vill veta mer om Azure Cosmos DB kan referera till tjänstens [dokumentationen](https://azure.microsoft.com/documentation/services/cosmos-db/) sidan.</span><span class="sxs-lookup"><span data-stu-id="27949-224">To learn more about Azure Cosmos DB, refer to the service's [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span> 

<span data-ttu-id="27949-225">Att förstå hur till Fragmentera data över flera partitioner avser [partitionering Data i Azure Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="27949-225">To understand how to shard your data across multiple partitions, refer to [Partitioning Data in Azure Cosmos DB](documentdb-partition-data.md).</span></span> 
