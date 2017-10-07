---
title: "aaaModeling dokumentdata för en NoSQL-databas | Microsoft Docs"
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
ms.openlocfilehash: 2e388c833f204287896dfa8e6f79c88073731b6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="modeling-document-data-for-nosql-databases"></a><span data-ttu-id="9bf90-104">Modeling dokumentdata för NoSQL-databaser</span><span class="sxs-lookup"><span data-stu-id="9bf90-104">Modeling document data for NoSQL databases</span></span>
<span data-ttu-id="9bf90-105">Medan schemafria databaser som Azure Cosmos DB underlättar super tooembrace ändringar tooyour datamodellen fortfarande bör du ägna några tid att tänka igenom dina data.</span><span class="sxs-lookup"><span data-stu-id="9bf90-105">While schema-free databases, like Azure Cosmos DB, make it super easy tooembrace changes tooyour data model you should still spend some time thinking about your data.</span></span> 

<span data-ttu-id="9bf90-106">Hur data kommer toobe lagras?</span><span class="sxs-lookup"><span data-stu-id="9bf90-106">How is data going toobe stored?</span></span> <span data-ttu-id="9bf90-107">Hur ska programmet kommer tooretrieve och fråga data?</span><span class="sxs-lookup"><span data-stu-id="9bf90-107">How is your application going tooretrieve and query data?</span></span> <span data-ttu-id="9bf90-108">Är programmet läsa tjockt, eller Skriv tunga?</span><span class="sxs-lookup"><span data-stu-id="9bf90-108">Is your application read heavy, or write heavy?</span></span> 

<span data-ttu-id="9bf90-109">När du har läst den här artikeln kommer du att kunna tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="9bf90-109">After reading this article, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="9bf90-110">Hur ska tycker om ett dokument i en dokumentdatabasen?</span><span class="sxs-lookup"><span data-stu-id="9bf90-110">How should I think about a document in a document database?</span></span>
* <span data-ttu-id="9bf90-111">Vad är datamodellering och varför ska jag hand?</span><span class="sxs-lookup"><span data-stu-id="9bf90-111">What is data modeling and why should I care?</span></span> 
* <span data-ttu-id="9bf90-112">Hur är modellering data i en dokumentet databasen olika tooa relationsdatabas?</span><span class="sxs-lookup"><span data-stu-id="9bf90-112">How is modeling data in a document database different tooa relational database?</span></span>
* <span data-ttu-id="9bf90-113">Hur jag express data relationer i en icke-relationell databas?</span><span class="sxs-lookup"><span data-stu-id="9bf90-113">How do I express data relationships in a non-relational database?</span></span>
* <span data-ttu-id="9bf90-114">När bädda data och när länkar toodata?</span><span class="sxs-lookup"><span data-stu-id="9bf90-114">When do I embed data and when do I link toodata?</span></span>

## <a name="embedding-data"></a><span data-ttu-id="9bf90-115">Bädda in data</span><span class="sxs-lookup"><span data-stu-id="9bf90-115">Embedding data</span></span>
<span data-ttu-id="9bf90-116">När du startar modellerar data i ett dokumentarkiv, till exempel Azure Cosmos DB försök tootreat entiteter som **fristående dokument** representeras i JSON.</span><span class="sxs-lookup"><span data-stu-id="9bf90-116">When you start modeling data in a document store, such as Azure Cosmos DB, try tootreat your entities as **self-contained documents** represented in JSON.</span></span>

<span data-ttu-id="9bf90-117">Innan vi fördjupa dig för mycket mer Låt oss ta ett par steg tillbaka och ta en titt på hur vi kan modellen något i en relationsdatabas, ett ämne som många av oss redan är bekant med.</span><span class="sxs-lookup"><span data-stu-id="9bf90-117">Before we dive in too much further, let us take a few steps back and have a look at how we might model something in a relational database, a subject many of us are already familiar with.</span></span> <span data-ttu-id="9bf90-118">hello följande exempel visas hur en person kan lagras i en relationsdatabas.</span><span class="sxs-lookup"><span data-stu-id="9bf90-118">hello following example shows how a person might be stored in a relational database.</span></span> 

![Relationsdatabas modellen](./media/documentdb-modeling-data/relational-data-model.png)

<span data-ttu-id="9bf90-120">När du arbetar med relationsdatabaser, vi har blivit vilka undervisning förekommer för år toonormalize normalisera, normalisera.</span><span class="sxs-lookup"><span data-stu-id="9bf90-120">When working with relational databases, we've been taught for years toonormalize, normalize, normalize.</span></span>

<span data-ttu-id="9bf90-121">Normaliserar data vanligtvis omfattar tar en entitet, till exempel en person, och dela upp frågan i toodiscrete delar av data.</span><span class="sxs-lookup"><span data-stu-id="9bf90-121">Normalizing your data typically involves taking an entity, such as a person, and breaking it down in toodiscrete pieces of data.</span></span> <span data-ttu-id="9bf90-122">En person kan ha flera poster för kontakt-information samt flera poster i hello exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="9bf90-122">In hello example above, a person can have multiple contact detail records as well as multiple address records.</span></span> <span data-ttu-id="9bf90-123">Vi även ta ytterligare ett steg och redovisar kontaktuppgifter genom att extrahera ytterligare vanliga fält som typen.</span><span class="sxs-lookup"><span data-stu-id="9bf90-123">We even go one step further and break down contact details by further extracting common fields like a type.</span></span> <span data-ttu-id="9bf90-124">Samma för adress, varje post här har en typ som *Start* eller *företag*</span><span class="sxs-lookup"><span data-stu-id="9bf90-124">Same for address, each record here has a type like *Home* or *Business*</span></span> 

<span data-ttu-id="9bf90-125">hello guidar lokala när normalisera data är för**lagra inte redundanta data** på varje registrera och i stället referera toodata.</span><span class="sxs-lookup"><span data-stu-id="9bf90-125">hello guiding premise when normalizing data is too**avoid storing redundant data** on each record and rather refer toodata.</span></span> <span data-ttu-id="9bf90-126">I det här exemplet tooread en person med sina kontaktuppgifter och adresser, behöver du toouse kopplingar tooeffectively sammanställda data vid körning.</span><span class="sxs-lookup"><span data-stu-id="9bf90-126">In this example, tooread a person, with all their contact details and addresses, you need toouse JOINS tooeffectively aggregate your data at run time.</span></span>

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

<span data-ttu-id="9bf90-127">Uppdatering av en person med kontaktinformation och adresser kräver skrivåtgärder över många enskilda tabeller.</span><span class="sxs-lookup"><span data-stu-id="9bf90-127">Updating a single person with their contact details and addresses requires write operations across many individual tables.</span></span> 

<span data-ttu-id="9bf90-128">Nu ska vi ta en titt på hur vi vill modellera hello samma data som en fristående enhet i en databas för dokumentet.</span><span class="sxs-lookup"><span data-stu-id="9bf90-128">Now let's take a look at how we would model hello same data as a self-contained entity in a document database.</span></span>

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

<span data-ttu-id="9bf90-129">Med hello metoden ovan har vi nu **Avnormaliserade** hello användarpost där vi **inbäddade** alla hello uppgifterna toothis person som deras kontaktuppgifter och adresser i tooa enkel JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="9bf90-129">Using hello approach above we have now **denormalized** hello person record where we **embedded** all hello information relating toothis person, such as their contact details and addresses, in tooa single JSON document.</span></span>
<span data-ttu-id="9bf90-130">Dessutom eftersom vi inte begränsad fast tooa schema har vi hello flexibilitet toodo saker som att ha kontaktuppgifterna med olika former helt.</span><span class="sxs-lookup"><span data-stu-id="9bf90-130">In addition, because we're not confined tooa fixed schema we have hello flexibility toodo things like having contact details of different shapes entirely.</span></span> 

<span data-ttu-id="9bf90-131">Hämta en fullständig personpost från hello databas är nu en enda Läsåtgärd mot en enda samling och för ett enskilt dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-131">Retrieving a complete person record from hello database is now a single read operation against a single collection and for a single document.</span></span> <span data-ttu-id="9bf90-132">Uppdatera en personpost med kontaktuppgifter och adresser, är också en enda skrivåtgärd mot ett enskilt dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-132">Updating a person record, with their contact details and addresses, is also a single write operation against a single document.</span></span>

<span data-ttu-id="9bf90-133">Av denormalizing data behöva programmet tooissue färre frågor och uppdateringar toocomplete vanliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9bf90-133">By denormalizing data, your application may need tooissue fewer queries and updates toocomplete common operations.</span></span> 

### <a name="when-tooembed"></a><span data-ttu-id="9bf90-134">När tooembed</span><span class="sxs-lookup"><span data-stu-id="9bf90-134">When tooembed</span></span>
<span data-ttu-id="9bf90-135">I allmänhet använder inbäddade data modeller när:</span><span class="sxs-lookup"><span data-stu-id="9bf90-135">In general, use embedded data models when:</span></span>

* <span data-ttu-id="9bf90-136">Det finns **innehåller** relationer mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="9bf90-136">There are **contains** relationships between entities.</span></span>
* <span data-ttu-id="9bf90-137">Det finns **en till några** relationer mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="9bf90-137">There are **one-to-few** relationships between entities.</span></span>
* <span data-ttu-id="9bf90-138">Det finns inbäddade data som **ändras sällan**.</span><span class="sxs-lookup"><span data-stu-id="9bf90-138">There is embedded data that **changes infrequently**.</span></span>
* <span data-ttu-id="9bf90-139">Det är inbäddat data inte växa **utan gräns**.</span><span class="sxs-lookup"><span data-stu-id="9bf90-139">There is embedded data won't grow **without bound**.</span></span>
* <span data-ttu-id="9bf90-140">Det finns inbäddade data som är **integrerad** toodata i ett dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-140">There is embedded data that is **integral** toodata in a document.</span></span>

> [!NOTE]
> <span data-ttu-id="9bf90-141">Vanligtvis Avnormaliserade data modeller får du bättre **läsa** prestanda.</span><span class="sxs-lookup"><span data-stu-id="9bf90-141">Typically denormalized data models provide better **read** performance.</span></span>
> 
> 

### <a name="when-not-tooembed"></a><span data-ttu-id="9bf90-142">När inte tooembed</span><span class="sxs-lookup"><span data-stu-id="9bf90-142">When not tooembed</span></span>
<span data-ttu-id="9bf90-143">Även om hello tumregel i en databas för dokumentet är toodenormalize allt och bädda in alla data i tooa enskilt dokument, kan detta leda toosome situationer som bör undvikas.</span><span class="sxs-lookup"><span data-stu-id="9bf90-143">While hello rule of thumb in a document database is toodenormalize everything and embed all data in tooa single document, this can lead toosome situations that should be avoided.</span></span>

<span data-ttu-id="9bf90-144">Ta den här JSON-fragment.</span><span class="sxs-lookup"><span data-stu-id="9bf90-144">Take this JSON snippet.</span></span>

    {
        "id": "1",
        "name": "What's new in hello coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from hello interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

<span data-ttu-id="9bf90-145">Det kan vara vad en post entitet med inbäddade kommentarer skulle se ut som om vi modellera en typisk blogg eller CMS, system.</span><span class="sxs-lookup"><span data-stu-id="9bf90-145">This might be what a post entity with embedded comments would look like if we were modeling a typical blog, or CMS, system.</span></span> <span data-ttu-id="9bf90-146">hello problem med det här exemplet är det hello kommentarer matris **unbounded**, vilket innebär att det finns inget (gräns) toohello antal kommentarer som en enda post kan ha.</span><span class="sxs-lookup"><span data-stu-id="9bf90-146">hello problem with this example is that hello comments array is **unbounded**, meaning that there is no (practical) limit toohello number of comments any single post can have.</span></span> <span data-ttu-id="9bf90-147">Detta blir ett problem som hello storlek på hello dokument kan växa avsevärt.</span><span class="sxs-lookup"><span data-stu-id="9bf90-147">This will become a problem as hello size of hello document could grow significantly.</span></span>

<span data-ttu-id="9bf90-148">Eftersom hello datastorleken hello dokument växer hello möjlighet tootransmit hello data över hello överföring samt läsning och uppdatering hello dokument i skala, påverkas.</span><span class="sxs-lookup"><span data-stu-id="9bf90-148">As hello size of hello document grows hello ability tootransmit hello data over hello wire as well as reading and updating hello document, at scale, will be impacted.</span></span>

<span data-ttu-id="9bf90-149">I det här fallet är det bättre tooconsider hello efter modell.</span><span class="sxs-lookup"><span data-stu-id="9bf90-149">In this case it would be better tooconsider hello following model.</span></span>

    Post document:
    {
        "id": "1",
        "name": "What's new in hello coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from hello interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from hello field"},
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

<span data-ttu-id="9bf90-150">Den här modellen har hello tre senaste kommentarer inbäddade på hello efter sig själv, vilket är en matris med en fast gräns detta tid.</span><span class="sxs-lookup"><span data-stu-id="9bf90-150">This model has hello three most recent comments embedded on hello post itself, which is an array with a fixed bound this time.</span></span> <span data-ttu-id="9bf90-151">hello andra kommentarer grupperas i toobatches 100 kommentarer och lagras i separata dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-151">hello other comments are grouped in toobatches of 100 comments and stored in separate documents.</span></span> <span data-ttu-id="9bf90-152">hello storleken på hello batch valdes som 100 eftersom appen fiktiva tillåter hello 100 användarkommentarer för tooload i taget.</span><span class="sxs-lookup"><span data-stu-id="9bf90-152">hello size of hello batch was chosen as 100 because our fictitious application allows hello user tooload 100 comments at a time.</span></span>  

<span data-ttu-id="9bf90-153">Ett annat fall där inbäddning data inte är en bra idé är när hello inbäddade data används ofta i dokument och kommer att ändras ofta.</span><span class="sxs-lookup"><span data-stu-id="9bf90-153">Another case where embedding data is not a good idea is when hello embedded data is used often across documents and will change frequently.</span></span> 

<span data-ttu-id="9bf90-154">Ta den här JSON-fragment.</span><span class="sxs-lookup"><span data-stu-id="9bf90-154">Take this JSON snippet.</span></span>

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

<span data-ttu-id="9bf90-155">Detta kan vara en person portfölj.</span><span class="sxs-lookup"><span data-stu-id="9bf90-155">This could represent a person's stock portfolio.</span></span> <span data-ttu-id="9bf90-156">Vi har valt tooembed hello lager information i tooeach portfölj dokumentet.</span><span class="sxs-lookup"><span data-stu-id="9bf90-156">We have chosen tooembed hello stock information in tooeach portfolio document.</span></span> <span data-ttu-id="9bf90-157">I en miljö där relaterade data ändras ofta som en börs handel program, kommer bädda in data som ändras ofta toomean att du kontinuerligt uppdaterar varje portfölj dokumentet varje gång en börs säljs.</span><span class="sxs-lookup"><span data-stu-id="9bf90-157">In an environment where related data is changing frequently, like a stock trading application, embedding data that changes frequently is going toomean that you are constantly updating each portfolio document every time a stock is traded.</span></span>

<span data-ttu-id="9bf90-158">Börs *zaza* får säljas många hundratals gånger i en enda dag och tusentals användare kan ha *zaza* på sina portfölj.</span><span class="sxs-lookup"><span data-stu-id="9bf90-158">Stock *zaza* may be traded many hundreds of times in a single day and thousands of users could have *zaza* on their portfolio.</span></span> <span data-ttu-id="9bf90-159">Med en datamodell som hello ovan skulle vi ha tooupdate tusentals portfölj dokument många gånger varje dag inledande tooa system som inte kan skalas mycket bra.</span><span class="sxs-lookup"><span data-stu-id="9bf90-159">With a data model like hello above we would have tooupdate many thousands of portfolio documents many times every day leading tooa system that won't scale very well.</span></span> 

## <span data-ttu-id="9bf90-160"><a id="Refer"></a>Refererar till data</span><span class="sxs-lookup"><span data-stu-id="9bf90-160"><a id="Refer"></a>Referencing data</span></span>
<span data-ttu-id="9bf90-161">Därför bädda in data fungerar bra i många fall, men det är tydligt att det finns scenarier när denormalizing data medför flera problem än lönar.</span><span class="sxs-lookup"><span data-stu-id="9bf90-161">So, embedding data works nicely for many cases but it is clear that there are scenarios when denormalizing your data will cause more problems than it is worth.</span></span> <span data-ttu-id="9bf90-162">Så vad vi gör nu?</span><span class="sxs-lookup"><span data-stu-id="9bf90-162">So what do we do now?</span></span> 

<span data-ttu-id="9bf90-163">Relationsdatabaser är inte hello enda plats där du kan skapa relationer mellan entiteter.</span><span class="sxs-lookup"><span data-stu-id="9bf90-163">Relational databases are not hello only place where you can create relationships between entities.</span></span> <span data-ttu-id="9bf90-164">I en databas i dokumentet kan du ha information i ett dokument som verkligen relaterar toodata i andra dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-164">In a document database you can have information in one document that actually relates toodata in other documents.</span></span> <span data-ttu-id="9bf90-165">Nu, jag inte arbeta för ett även en minut att vi skapa system som är bättre passar tooa relationsdatabaser i Azure Cosmos DB eller andra dokumentdatabasen, men enkla relationer är skadad och kan vara användbar.</span><span class="sxs-lookup"><span data-stu-id="9bf90-165">Now, I am not advocating for even one minute that we build systems that would be better suited tooa relational database in Azure Cosmos DB, or any other document database, but simple relationships are fine and can be very useful.</span></span> 

<span data-ttu-id="9bf90-166">Vi valde toouse hello exempel på en portfölj från tidigare men den här gången vi refererar toohello lager artikeln på hello portfölj i stället för att bädda in den i hello JSON nedan.</span><span class="sxs-lookup"><span data-stu-id="9bf90-166">In hello JSON below we chose toouse hello example of a stock portfolio from earlier but this time we refer toohello stock item on hello portfolio instead of embedding it.</span></span> <span data-ttu-id="9bf90-167">Det här sättet är när hello lager objekt ändras ofta under hela hello dag hello endast dokument som behöver uppdateras toobe hello lager dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-167">This way, when hello stock item changes frequently throughout hello day hello only document that needs toobe updated is hello single stock document.</span></span> 

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


<span data-ttu-id="9bf90-168">En omedelbar Nackdelen med toothis metod är dock om ditt program är nödvändiga tooshow information om varje lager som hålls kvar när du visar en persons portfölj; i det här fallet behöver du toomake flera resor toohello tooload hello databasinformation för varje lager dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-168">An immediate downside toothis approach though is if your application is required tooshow information about each stock that is held when displaying a person's portfolio; in this case you would need toomake multiple trips toohello database tooload hello information for each stock document.</span></span> <span data-ttu-id="9bf90-169">Här har vi gjort beslut tooimprove hello effektivitet för skrivåtgärder som inträffa ofta under hello dagen, men som i sin tur komprometteras på hello läsåtgärder som eventuellt få mindre påverkan på hello prestandan för viss systemet.</span><span class="sxs-lookup"><span data-stu-id="9bf90-169">Here we've made a decision tooimprove hello efficiency of write operations, which happen frequently throughout hello day, but in turn compromised on hello read operations that potentially have less impact on hello performance of this particular system.</span></span>

> [!NOTE]
> <span data-ttu-id="9bf90-170">Normaliserade datamodeller **kan kräva mer sändningar** toohello server.</span><span class="sxs-lookup"><span data-stu-id="9bf90-170">Normalized data models **can require more round trips** toohello server.</span></span>
> 
> 

### <a name="what-about-foreign-keys"></a><span data-ttu-id="9bf90-171">Nyheter om externa nycklar?</span><span class="sxs-lookup"><span data-stu-id="9bf90-171">What about foreign keys?</span></span>
<span data-ttu-id="9bf90-172">Eftersom det inte finns något begrepp om en begränsning, foreign key- eller på annat sätt, relationer mellan dokument som du har i dokument är effektivt ”svaga länkar” och kommer inte att verifieras av själva hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="9bf90-172">Because there is currently no concept of a constraint, foreign-key or otherwise, any inter-document relationships that you have in documents are effectively "weak links" and will not be verified by hello database itself.</span></span> <span data-ttu-id="9bf90-173">Om du vill tooensure som hello data som ett dokument refererar tooactually finns, måste du toodo detta i ditt program eller via hello serversidan utlösare eller lagrade procedurer på Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bf90-173">If you want tooensure that hello data a document is referring tooactually exists, then you need toodo this in your application, or through hello use of server-side triggers or stored procedures on Azure Cosmos DB.</span></span>

### <a name="when-tooreference"></a><span data-ttu-id="9bf90-174">När tooreference</span><span class="sxs-lookup"><span data-stu-id="9bf90-174">When tooreference</span></span>
<span data-ttu-id="9bf90-175">I allmänhet använder normaliserade data modeller när:</span><span class="sxs-lookup"><span data-stu-id="9bf90-175">In general, use normalized data models when:</span></span>

* <span data-ttu-id="9bf90-176">Som representerar **en-till-många** relationer.</span><span class="sxs-lookup"><span data-stu-id="9bf90-176">Representing **one-to-many** relationships.</span></span>
* <span data-ttu-id="9bf90-177">Som representerar **många-till-många** relationer.</span><span class="sxs-lookup"><span data-stu-id="9bf90-177">Representing **many-to-many** relationships.</span></span>
* <span data-ttu-id="9bf90-178">Relaterade data **ändras ofta**.</span><span class="sxs-lookup"><span data-stu-id="9bf90-178">Related data **changes frequently**.</span></span>
* <span data-ttu-id="9bf90-179">Refererade data kan gå **unbounded**.</span><span class="sxs-lookup"><span data-stu-id="9bf90-179">Referenced data could be **unbounded**.</span></span>

> [!NOTE]
> <span data-ttu-id="9bf90-180">Vanligtvis normaliserar ger bättre **skriva** prestanda.</span><span class="sxs-lookup"><span data-stu-id="9bf90-180">Typically normalizing provides better **write** performance.</span></span>
> 
> 

### <a name="where-do-i-put-hello-relationship"></a><span data-ttu-id="9bf90-181">Där placera hello relationen?</span><span class="sxs-lookup"><span data-stu-id="9bf90-181">Where do I put hello relationship?</span></span>
<span data-ttu-id="9bf90-182">hello tillväxt hello relationen hjälper dig att avgöra i vilken dokumentet toostore hello referens.</span><span class="sxs-lookup"><span data-stu-id="9bf90-182">hello growth of hello relationship will help determine in which document toostore hello reference.</span></span>

<span data-ttu-id="9bf90-183">Om vi tittar på hello JSON som modeller utgivare och böcker nedan.</span><span class="sxs-lookup"><span data-stu-id="9bf90-183">If we look at hello JSON below that models publishers and books.</span></span>

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over hello world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive in tooAzure Cosmos DB" }

<span data-ttu-id="9bf90-184">Om hello antalet hello böcker per utgivaren är liten med begränsad tillväxt, kan det vara användbart att lagra hello book referens i hello publisher-dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-184">If hello number of hello books per publisher is small with limited growth, then storing hello book reference inside hello publisher document may be useful.</span></span> <span data-ttu-id="9bf90-185">Men om hello antal böcker per utgivaren är unbounded skulle sedan denna datamodell leda toomutable, växande matriser som hello exempel publisher dokument ovan.</span><span class="sxs-lookup"><span data-stu-id="9bf90-185">However, if hello number of books per publisher is unbounded, then this data model would lead toomutable, growing arrays, as in hello example publisher document above.</span></span> 

<span data-ttu-id="9bf90-186">Växla runt lite skulle resultera i en modell som fortfarande representerar hello samma data men nu undviker dessa stora föränderliga samlingar.</span><span class="sxs-lookup"><span data-stu-id="9bf90-186">Switching things around a bit would result in a model that still represents hello same data but now avoids these large mutable collections.</span></span>

    Publisher document: 
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents: 
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over hello world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive in tooAzure Cosmos DB", "pub-id": "mspress"}

<span data-ttu-id="9bf90-187">I ovanstående exempel hello, vi utelämnade hello unbounded mängden hello publisher-dokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-187">In hello above example, we have dropped hello unbounded collection on hello publisher document.</span></span> <span data-ttu-id="9bf90-188">I stället vi bara har en referens toohello utgivare på varje bokdokument.</span><span class="sxs-lookup"><span data-stu-id="9bf90-188">Instead we just have a a reference toohello publisher on each book document.</span></span>

### <a name="how-do-i-model-manymany-relationships"></a><span data-ttu-id="9bf90-189">Hur jag för att modellera många: många-relationer?</span><span class="sxs-lookup"><span data-stu-id="9bf90-189">How do I model many:many relationships?</span></span>
<span data-ttu-id="9bf90-190">I en relationsdatabas *många: många* relationer ofta modelleras med anslutning till tabeller som bara ansluta poster från andra tabeller tillsammans.</span><span class="sxs-lookup"><span data-stu-id="9bf90-190">In a relational database *many:many* relationships are often modeled with join tables, which just join records from other tables together.</span></span> 

![Koppla tabeller](./media/documentdb-modeling-data/join-table.png)

<span data-ttu-id="9bf90-192">Du kanske tro tooreplicate hello samma sak med hjälp av dokument och skapa en datamodell som ser ut ungefär toohello följande.</span><span class="sxs-lookup"><span data-stu-id="9bf90-192">You might be tempted tooreplicate hello same thing using documents and produce a data model that looks similar toohello following.</span></span>

    Author documents: 
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over hello world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive in tooAzure Cosmos DB" }

    Joining documents: 
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

<span data-ttu-id="9bf90-193">Detta kan fungera.</span><span class="sxs-lookup"><span data-stu-id="9bf90-193">This would work.</span></span> <span data-ttu-id="9bf90-194">Dock kräver inläsning av antingen en författare med deras böcker eller läsa in en bok med dess upphovsman alltid minst två ytterligare frågor mot hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="9bf90-194">However, loading either an author with their books, or loading a book with its author, would always require at least two additional queries against hello database.</span></span> <span data-ttu-id="9bf90-195">En fråga toohello koppla dokumentet och sedan en annan fråga toofetch hello faktiska som ansluten.</span><span class="sxs-lookup"><span data-stu-id="9bf90-195">One query toohello joining document and then another query toofetch hello actual document being joined.</span></span> 

<span data-ttu-id="9bf90-196">Om allt anslutning till tabellen gör fäster ihop två typer av data, varför inte släppa den helt?</span><span class="sxs-lookup"><span data-stu-id="9bf90-196">If all this join table is doing is gluing together two pieces of data, then why not drop it completely?</span></span>
<span data-ttu-id="9bf90-197">Tänk hello följande.</span><span class="sxs-lookup"><span data-stu-id="9bf90-197">Consider hello following.</span></span>

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in tooAzure Cosmos DB", "authors": ["a2"]}

<span data-ttu-id="9bf90-198">Nu om jag har en författare jag vet vilka böcker som de har skrivit omedelbart och om jag hade ett book dokument läsas in I motsats vet hello-ID för hello innehålla.</span><span class="sxs-lookup"><span data-stu-id="9bf90-198">Now, if I had an author, I immediately know which books they have written, and conversely if I had a book document loaded I would know hello ids of hello author(s).</span></span> <span data-ttu-id="9bf90-199">Detta sparar den mellanliggande frågan mot hello kopplingstabell minska hello antalet server sändningar tillämpningsprogrammet har toomake.</span><span class="sxs-lookup"><span data-stu-id="9bf90-199">This saves that intermediary query against hello join table reducing hello number of server round trips your application has toomake.</span></span> 

## <span data-ttu-id="9bf90-200"><a id="WrapUp"></a>Hybrid datamodeller</span><span class="sxs-lookup"><span data-stu-id="9bf90-200"><a id="WrapUp"></a>Hybrid data models</span></span>
<span data-ttu-id="9bf90-201">Vi nu har tittat bädda in (eller denormalizing) och refererande (eller normaliserar) data har sina upsides och kompromisser har vi har sett.</span><span class="sxs-lookup"><span data-stu-id="9bf90-201">We've now looked embedding (or denormalizing) and referencing (or normalizing) data, each have their upsides and each have compromises as we have seen.</span></span> 

<span data-ttu-id="9bf90-202">Det inte alltid har toobe antingen eller kan inte vara scared toomix saker in lite.</span><span class="sxs-lookup"><span data-stu-id="9bf90-202">It doesn't always have toobe either or, don't be scared toomix things up a little.</span></span> 

<span data-ttu-id="9bf90-203">Baserat på ditt programs specifika användningsmönster och arbetsbelastningar som det kan finnas fall där blanda inbäddad refererade klokt och gick lead toosimpler programlogik med färre server förfrågningar men har ändå en bra nivå av prestanda .</span><span class="sxs-lookup"><span data-stu-id="9bf90-203">Based on your application's specific usage patterns and workloads there may be cases where mixing embedded and referenced data makes sense and could lead toosimpler application logic with fewer server round trips while still maintaining a good level of performance.</span></span>

<span data-ttu-id="9bf90-204">Överväg följande JSON hello.</span><span class="sxs-lookup"><span data-stu-id="9bf90-204">Consider hello following JSON.</span></span> 

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

<span data-ttu-id="9bf90-205">Vi har här (mest) följt hello inbäddad modell, där data från andra enheter som är inbäddade i hello översta dokumentet, men andra data refereras.</span><span class="sxs-lookup"><span data-stu-id="9bf90-205">Here we've (mostly) followed hello embedded model, where data from other entities are embedded in hello top-level document, but other data is referenced.</span></span> 

<span data-ttu-id="9bf90-206">Om du tittar på hello book dokumentet ser vi några intressanta fält när vi tittar på hello matris med författare.</span><span class="sxs-lookup"><span data-stu-id="9bf90-206">If you look at hello book document, we can see a few interesting fields when we look at hello array of authors.</span></span> <span data-ttu-id="9bf90-207">Det finns en *id* fält som är hello fält som vi använder toorefer tillbaka tooan författare dokument, standard Övning i en normaliserade modell, men vi också innehålla *namn* och *thumbnailUrl*.</span><span class="sxs-lookup"><span data-stu-id="9bf90-207">There is an *id* field which is hello field we use toorefer back tooan author document, standard practice in a normalized model, but then we also have *name* and *thumbnailUrl*.</span></span> <span data-ttu-id="9bf90-208">Vi kunde har precis har fastnat med *id* och hello programmet tooget eventuell ytterligare information som den behövs från hello respektive författare dokument med hjälp av hello ”link”, men eftersom vårt program visar hello författarens namn och en miniatyrbild med alla böcker visas vi kan spara en onödig kommunikation toohello server per bok i en lista av denormalizing **vissa** data från hello författare.</span><span class="sxs-lookup"><span data-stu-id="9bf90-208">We could've just stuck with *id* and left hello application tooget any additional information it needed from hello respective author document using hello "link", but because our application displays hello author's name and a thumbnail picture with every book displayed we can save a round trip toohello server per book in a list by denormalizing **some** data from hello author.</span></span>

<span data-ttu-id="9bf90-209">Kontrollera, om hello författarens namn ändras eller de ville tooupdate sina foto vi skulle ha toogo en uppdatering var book de någonsin publicerade men för vårt program baserat på hello antagandet att författare inte ändrar deras namn så ofta är en godkänd design beslut.</span><span class="sxs-lookup"><span data-stu-id="9bf90-209">Sure, if hello author's name changed or they wanted tooupdate their photo we'd have toogo an update every book they ever published but for our application, based on hello assumption that authors don't change their names very often, this is an acceptable design decision.</span></span>  

<span data-ttu-id="9bf90-210">I exemplet hello finns **före beräknade mängder** värden toosave dyra bearbetning på en Läsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="9bf90-210">In hello example there are **pre-calculated aggregates** values toosave expensive processing on a read operation.</span></span> <span data-ttu-id="9bf90-211">I exemplet hello vissa hello data som är inbäddade i hello författare dokumentet är data som beräknas vid körning.</span><span class="sxs-lookup"><span data-stu-id="9bf90-211">In hello example, some of hello data embedded in hello author document is data that is calculated at run-time.</span></span> <span data-ttu-id="9bf90-212">Varje gång en ny bok publiceras, skapas en bokdokument **och** hello countOfBooks anges tooa beräknade värde baserat på hello antalet book dokument som finns för en viss författare.</span><span class="sxs-lookup"><span data-stu-id="9bf90-212">Every time a new book is published, a book document is created **and** hello countOfBooks field is set tooa calculated value based on hello number of book documents that exist for a particular author.</span></span> <span data-ttu-id="9bf90-213">Denna optimering är bra i skrivskyddade tunga system där vi har råd toodo beräkningar på skrivningar i ordning toooptimize läsningar.</span><span class="sxs-lookup"><span data-stu-id="9bf90-213">This optimization would be good in read heavy systems where we can afford toodo computations on writes in order toooptimize reads.</span></span>

<span data-ttu-id="9bf90-214">Hej möjlighet toohave en modell med före beräknade fält är möjligt eftersom Azure Cosmos DB stöder **flera dokument transaktioner**.</span><span class="sxs-lookup"><span data-stu-id="9bf90-214">hello ability toohave a model with pre-calculated fields is made possible because Azure Cosmos DB supports **multi-document transactions**.</span></span> <span data-ttu-id="9bf90-215">Många NoSQL-lagringsplatser kan inte göra transaktioner mellan dokument och därför gör designbeslut, till exempel ”inkludera alltid allt”, på grund av toothis begränsning.</span><span class="sxs-lookup"><span data-stu-id="9bf90-215">Many NoSQL stores cannot do transactions across documents and therefore advocate design decisions, such as "always embed everything", due toothis limitation.</span></span> <span data-ttu-id="9bf90-216">Du kan använda serversidan utlösare eller lagrade procedurer som Infoga böcker och uppdatera författarna alla inom en ACID-transaktion med Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9bf90-216">With Azure Cosmos DB, you can use server-side triggers, or stored procedures, that insert books and update authors all within an ACID transaction.</span></span> <span data-ttu-id="9bf90-217">Nu du inte **har** tooembed allt i tooone dokument bara toobe till att dina data förblir konsekventa.</span><span class="sxs-lookup"><span data-stu-id="9bf90-217">Now you don't **have** tooembed everything in tooone document just toobe sure that your data remains consistent.</span></span>

## <span data-ttu-id="9bf90-218"><a name="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9bf90-218"><a name="NextSteps"></a>Next steps</span></span>
<span data-ttu-id="9bf90-219">hello största takeaways från den här artikeln är toounderstand att datamodeller i en schemafria värld är precis lika viktig som någonsin.</span><span class="sxs-lookup"><span data-stu-id="9bf90-219">hello biggest takeaways from this article is toounderstand that data modeling in a schema-free world is just as important as ever.</span></span> 

<span data-ttu-id="9bf90-220">Precis som det finns ingen toorepresent för ett enskilt sätt en typ av data på en skärm, finns det inga enskilt sätt toomodel dina data.</span><span class="sxs-lookup"><span data-stu-id="9bf90-220">Just as there is no single way toorepresent a piece of data on a screen, there is no single way toomodel your data.</span></span> <span data-ttu-id="9bf90-221">Du behöver toounderstand ditt program och hur produceras, använda och bearbeta hello data.</span><span class="sxs-lookup"><span data-stu-id="9bf90-221">You need toounderstand your application and how it will produce, consume, and process hello data.</span></span> <span data-ttu-id="9bf90-222">Genom att använda några av hello kan riktlinjer som beskrivs här du ange om hur du skapar en modell som åtgärdar hello omedelbara behov av ditt program.</span><span class="sxs-lookup"><span data-stu-id="9bf90-222">Then, by applying some of hello guidelines presented here you can set about creating a model that addresses hello immediate needs of your application.</span></span> <span data-ttu-id="9bf90-223">När dina program behöver toochange, kan du utnyttja hello flexibiliteten hos en schemafria databasen tooembrace som ändras och utvecklas datamodellen enkelt.</span><span class="sxs-lookup"><span data-stu-id="9bf90-223">When your applications need toochange, you can leverage hello flexibility of a schema-free database tooembrace that change and evolve your data model easily.</span></span> 

<span data-ttu-id="9bf90-224">toolearn mer om Azure Cosmos DB finns toohello service [dokumentationen](https://azure.microsoft.com/documentation/services/cosmos-db/) sidan.</span><span class="sxs-lookup"><span data-stu-id="9bf90-224">toolearn more about Azure Cosmos DB, refer toohello service's [documentation](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span> 

<span data-ttu-id="9bf90-225">toounderstand hur tooshard data över flera partitioner finns för[partitionering Data i Azure Cosmos DB](documentdb-partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="9bf90-225">toounderstand how tooshard your data across multiple partitions, refer too[Partitioning Data in Azure Cosmos DB](documentdb-partition-data.md).</span></span> 
