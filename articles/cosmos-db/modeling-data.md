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
# <a name="modeling-document-data-for-nosql-databases"></a>Modeling dokumentdata för NoSQL-databaser
Medan schemafria databaser som Azure Cosmos DB underlättar super tooembrace ändringar tooyour datamodellen fortfarande bör du ägna några tid att tänka igenom dina data. 

Hur data kommer toobe lagras? Hur ska programmet kommer tooretrieve och fråga data? Är programmet läsa tjockt, eller Skriv tunga? 

När du har läst den här artikeln kommer du att kunna tooanswer hello följande frågor:

* Hur ska tycker om ett dokument i en dokumentdatabasen?
* Vad är datamodellering och varför ska jag hand? 
* Hur är modellering data i en dokumentet databasen olika tooa relationsdatabas?
* Hur jag express data relationer i en icke-relationell databas?
* När bädda data och när länkar toodata?

## <a name="embedding-data"></a>Bädda in data
När du startar modellerar data i ett dokumentarkiv, till exempel Azure Cosmos DB försök tootreat entiteter som **fristående dokument** representeras i JSON.

Innan vi fördjupa dig för mycket mer Låt oss ta ett par steg tillbaka och ta en titt på hur vi kan modellen något i en relationsdatabas, ett ämne som många av oss redan är bekant med. hello följande exempel visas hur en person kan lagras i en relationsdatabas. 

![Relationsdatabas modellen](./media/documentdb-modeling-data/relational-data-model.png)

När du arbetar med relationsdatabaser, vi har blivit vilka undervisning förekommer för år toonormalize normalisera, normalisera.

Normaliserar data vanligtvis omfattar tar en entitet, till exempel en person, och dela upp frågan i toodiscrete delar av data. En person kan ha flera poster för kontakt-information samt flera poster i hello exemplet ovan. Vi även ta ytterligare ett steg och redovisar kontaktuppgifter genom att extrahera ytterligare vanliga fält som typen. Samma för adress, varje post här har en typ som *Start* eller *företag* 

hello guidar lokala när normalisera data är för**lagra inte redundanta data** på varje registrera och i stället referera toodata. I det här exemplet tooread en person med sina kontaktuppgifter och adresser, behöver du toouse kopplingar tooeffectively sammanställda data vid körning.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Uppdatering av en person med kontaktinformation och adresser kräver skrivåtgärder över många enskilda tabeller. 

Nu ska vi ta en titt på hur vi vill modellera hello samma data som en fristående enhet i en databas för dokumentet.

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

Med hello metoden ovan har vi nu **Avnormaliserade** hello användarpost där vi **inbäddade** alla hello uppgifterna toothis person som deras kontaktuppgifter och adresser i tooa enkel JSON-dokumentet.
Dessutom eftersom vi inte begränsad fast tooa schema har vi hello flexibilitet toodo saker som att ha kontaktuppgifterna med olika former helt. 

Hämta en fullständig personpost från hello databas är nu en enda Läsåtgärd mot en enda samling och för ett enskilt dokument. Uppdatera en personpost med kontaktuppgifter och adresser, är också en enda skrivåtgärd mot ett enskilt dokument.

Av denormalizing data behöva programmet tooissue färre frågor och uppdateringar toocomplete vanliga åtgärder. 

### <a name="when-tooembed"></a>När tooembed
I allmänhet använder inbäddade data modeller när:

* Det finns **innehåller** relationer mellan entiteter.
* Det finns **en till några** relationer mellan entiteter.
* Det finns inbäddade data som **ändras sällan**.
* Det är inbäddat data inte växa **utan gräns**.
* Det finns inbäddade data som är **integrerad** toodata i ett dokument.

> [!NOTE]
> Vanligtvis Avnormaliserade data modeller får du bättre **läsa** prestanda.
> 
> 

### <a name="when-not-tooembed"></a>När inte tooembed
Även om hello tumregel i en databas för dokumentet är toodenormalize allt och bädda in alla data i tooa enskilt dokument, kan detta leda toosome situationer som bör undvikas.

Ta den här JSON-fragment.

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

Det kan vara vad en post entitet med inbäddade kommentarer skulle se ut som om vi modellera en typisk blogg eller CMS, system. hello problem med det här exemplet är det hello kommentarer matris **unbounded**, vilket innebär att det finns inget (gräns) toohello antal kommentarer som en enda post kan ha. Detta blir ett problem som hello storlek på hello dokument kan växa avsevärt.

Eftersom hello datastorleken hello dokument växer hello möjlighet tootransmit hello data över hello överföring samt läsning och uppdatering hello dokument i skala, påverkas.

I det här fallet är det bättre tooconsider hello efter modell.

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

Den här modellen har hello tre senaste kommentarer inbäddade på hello efter sig själv, vilket är en matris med en fast gräns detta tid. hello andra kommentarer grupperas i toobatches 100 kommentarer och lagras i separata dokument. hello storleken på hello batch valdes som 100 eftersom appen fiktiva tillåter hello 100 användarkommentarer för tooload i taget.  

Ett annat fall där inbäddning data inte är en bra idé är när hello inbäddade data används ofta i dokument och kommer att ändras ofta. 

Ta den här JSON-fragment.

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

Detta kan vara en person portfölj. Vi har valt tooembed hello lager information i tooeach portfölj dokumentet. I en miljö där relaterade data ändras ofta som en börs handel program, kommer bädda in data som ändras ofta toomean att du kontinuerligt uppdaterar varje portfölj dokumentet varje gång en börs säljs.

Börs *zaza* får säljas många hundratals gånger i en enda dag och tusentals användare kan ha *zaza* på sina portfölj. Med en datamodell som hello ovan skulle vi ha tooupdate tusentals portfölj dokument många gånger varje dag inledande tooa system som inte kan skalas mycket bra. 

## <a id="Refer"></a>Refererar till data
Därför bädda in data fungerar bra i många fall, men det är tydligt att det finns scenarier när denormalizing data medför flera problem än lönar. Så vad vi gör nu? 

Relationsdatabaser är inte hello enda plats där du kan skapa relationer mellan entiteter. I en databas i dokumentet kan du ha information i ett dokument som verkligen relaterar toodata i andra dokument. Nu, jag inte arbeta för ett även en minut att vi skapa system som är bättre passar tooa relationsdatabaser i Azure Cosmos DB eller andra dokumentdatabasen, men enkla relationer är skadad och kan vara användbar. 

Vi valde toouse hello exempel på en portfölj från tidigare men den här gången vi refererar toohello lager artikeln på hello portfölj i stället för att bädda in den i hello JSON nedan. Det här sättet är när hello lager objekt ändras ofta under hela hello dag hello endast dokument som behöver uppdateras toobe hello lager dokument. 

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


En omedelbar Nackdelen med toothis metod är dock om ditt program är nödvändiga tooshow information om varje lager som hålls kvar när du visar en persons portfölj; i det här fallet behöver du toomake flera resor toohello tooload hello databasinformation för varje lager dokument. Här har vi gjort beslut tooimprove hello effektivitet för skrivåtgärder som inträffa ofta under hello dagen, men som i sin tur komprometteras på hello läsåtgärder som eventuellt få mindre påverkan på hello prestandan för viss systemet.

> [!NOTE]
> Normaliserade datamodeller **kan kräva mer sändningar** toohello server.
> 
> 

### <a name="what-about-foreign-keys"></a>Nyheter om externa nycklar?
Eftersom det inte finns något begrepp om en begränsning, foreign key- eller på annat sätt, relationer mellan dokument som du har i dokument är effektivt ”svaga länkar” och kommer inte att verifieras av själva hello-databasen. Om du vill tooensure som hello data som ett dokument refererar tooactually finns, måste du toodo detta i ditt program eller via hello serversidan utlösare eller lagrade procedurer på Azure Cosmos DB.

### <a name="when-tooreference"></a>När tooreference
I allmänhet använder normaliserade data modeller när:

* Som representerar **en-till-många** relationer.
* Som representerar **många-till-många** relationer.
* Relaterade data **ändras ofta**.
* Refererade data kan gå **unbounded**.

> [!NOTE]
> Vanligtvis normaliserar ger bättre **skriva** prestanda.
> 
> 

### <a name="where-do-i-put-hello-relationship"></a>Där placera hello relationen?
hello tillväxt hello relationen hjälper dig att avgöra i vilken dokumentet toostore hello referens.

Om vi tittar på hello JSON som modeller utgivare och böcker nedan.

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

Om hello antalet hello böcker per utgivaren är liten med begränsad tillväxt, kan det vara användbart att lagra hello book referens i hello publisher-dokument. Men om hello antal böcker per utgivaren är unbounded skulle sedan denna datamodell leda toomutable, växande matriser som hello exempel publisher dokument ovan. 

Växla runt lite skulle resultera i en modell som fortfarande representerar hello samma data men nu undviker dessa stora föränderliga samlingar.

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

I ovanstående exempel hello, vi utelämnade hello unbounded mängden hello publisher-dokument. I stället vi bara har en referens toohello utgivare på varje bokdokument.

### <a name="how-do-i-model-manymany-relationships"></a>Hur jag för att modellera många: många-relationer?
I en relationsdatabas *många: många* relationer ofta modelleras med anslutning till tabeller som bara ansluta poster från andra tabeller tillsammans. 

![Koppla tabeller](./media/documentdb-modeling-data/join-table.png)

Du kanske tro tooreplicate hello samma sak med hjälp av dokument och skapa en datamodell som ser ut ungefär toohello följande.

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

Detta kan fungera. Dock kräver inläsning av antingen en författare med deras böcker eller läsa in en bok med dess upphovsman alltid minst två ytterligare frågor mot hello-databasen. En fråga toohello koppla dokumentet och sedan en annan fråga toofetch hello faktiska som ansluten. 

Om allt anslutning till tabellen gör fäster ihop två typer av data, varför inte släppa den helt?
Tänk hello följande.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in tooAzure Cosmos DB", "authors": ["a2"]}

Nu om jag har en författare jag vet vilka böcker som de har skrivit omedelbart och om jag hade ett book dokument läsas in I motsats vet hello-ID för hello innehålla. Detta sparar den mellanliggande frågan mot hello kopplingstabell minska hello antalet server sändningar tillämpningsprogrammet har toomake. 

## <a id="WrapUp"></a>Hybrid datamodeller
Vi nu har tittat bädda in (eller denormalizing) och refererande (eller normaliserar) data har sina upsides och kompromisser har vi har sett. 

Det inte alltid har toobe antingen eller kan inte vara scared toomix saker in lite. 

Baserat på ditt programs specifika användningsmönster och arbetsbelastningar som det kan finnas fall där blanda inbäddad refererade klokt och gick lead toosimpler programlogik med färre server förfrågningar men har ändå en bra nivå av prestanda .

Överväg följande JSON hello. 

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

Vi har här (mest) följt hello inbäddad modell, där data från andra enheter som är inbäddade i hello översta dokumentet, men andra data refereras. 

Om du tittar på hello book dokumentet ser vi några intressanta fält när vi tittar på hello matris med författare. Det finns en *id* fält som är hello fält som vi använder toorefer tillbaka tooan författare dokument, standard Övning i en normaliserade modell, men vi också innehålla *namn* och *thumbnailUrl*. Vi kunde har precis har fastnat med *id* och hello programmet tooget eventuell ytterligare information som den behövs från hello respektive författare dokument med hjälp av hello ”link”, men eftersom vårt program visar hello författarens namn och en miniatyrbild med alla böcker visas vi kan spara en onödig kommunikation toohello server per bok i en lista av denormalizing **vissa** data från hello författare.

Kontrollera, om hello författarens namn ändras eller de ville tooupdate sina foto vi skulle ha toogo en uppdatering var book de någonsin publicerade men för vårt program baserat på hello antagandet att författare inte ändrar deras namn så ofta är en godkänd design beslut.  

I exemplet hello finns **före beräknade mängder** värden toosave dyra bearbetning på en Läsåtgärd. I exemplet hello vissa hello data som är inbäddade i hello författare dokumentet är data som beräknas vid körning. Varje gång en ny bok publiceras, skapas en bokdokument **och** hello countOfBooks anges tooa beräknade värde baserat på hello antalet book dokument som finns för en viss författare. Denna optimering är bra i skrivskyddade tunga system där vi har råd toodo beräkningar på skrivningar i ordning toooptimize läsningar.

Hej möjlighet toohave en modell med före beräknade fält är möjligt eftersom Azure Cosmos DB stöder **flera dokument transaktioner**. Många NoSQL-lagringsplatser kan inte göra transaktioner mellan dokument och därför gör designbeslut, till exempel ”inkludera alltid allt”, på grund av toothis begränsning. Du kan använda serversidan utlösare eller lagrade procedurer som Infoga böcker och uppdatera författarna alla inom en ACID-transaktion med Azure Cosmos DB. Nu du inte **har** tooembed allt i tooone dokument bara toobe till att dina data förblir konsekventa.

## <a name="NextSteps"></a>Nästa steg
hello största takeaways från den här artikeln är toounderstand att datamodeller i en schemafria värld är precis lika viktig som någonsin. 

Precis som det finns ingen toorepresent för ett enskilt sätt en typ av data på en skärm, finns det inga enskilt sätt toomodel dina data. Du behöver toounderstand ditt program och hur produceras, använda och bearbeta hello data. Genom att använda några av hello kan riktlinjer som beskrivs här du ange om hur du skapar en modell som åtgärdar hello omedelbara behov av ditt program. När dina program behöver toochange, kan du utnyttja hello flexibiliteten hos en schemafria databasen tooembrace som ändras och utvecklas datamodellen enkelt. 

toolearn mer om Azure Cosmos DB finns toohello service [dokumentationen](https://azure.microsoft.com/documentation/services/cosmos-db/) sidan. 

toounderstand hur tooshard data över flera partitioner finns för[partitionering Data i Azure Cosmos DB](documentdb-partition-data.md). 
