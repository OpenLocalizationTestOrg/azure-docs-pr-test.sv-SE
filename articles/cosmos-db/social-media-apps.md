---
title: "Azure DB Cosmos-designmönstret: sociala medier appar | Microsoft Docs"
description: "Läs mer om ett designmönster för sociala nätverk genom att utnyttja hello lagring flexibiliteten hos Azure Cosmos DB och andra Azure-tjänster."
keywords: Sociala medier appar
services: cosmos-db
author: ealsur
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 2dbf83a7-512a-4993-bf1b-ea7d72e095d9
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: mimig
ms.openlocfilehash: 47a22f2c5762d62b176921c8052e7bd75d8cf6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="going-social-with-azure-cosmos-db"></a>Gå sociala med Azure Cosmos DB
Bor i ett massivt sammankopplade society innebär att på någon punkt i livslängd du blir en del av en **sociala nätverk**. Vi använder sociala nätverk tookeep kontakten med vänner, kolleger, familj och ibland tooshare Vår passion med personer med gemensamma intressen.

Vi kanske har funderat över hur dessa nätverk lagra och kopplas samman våra data som tekniker och utvecklare eller kan ha även har gett toocreate eller systemarkitekt nya sociala nätverk för en specifik nischer marknadsföra yourselves. Det är då stor hello-frågan: hur lagras dessa data?

Anta att vi skapar ett nytt och blank sociala nätverk, där användarna kan skicka artiklar med relaterade mediet, till exempel bilder, videofilmer och musik. Användare kan kommentera inlägg och ge punkter för klassificering. Det ska vara en feed tjänster som användarna ser och kan toointeract med på Landningssida för hello webbplats. Detta inte låter mycket komplexa (överst), men för hello dig ut av enkelhet gör stoppa det (vi gick detaljerad information om anpassade feeds påverkas av relationer, men den överskrider hello syftet med den här artikeln).

Så, hur vi sparar du den och var?

Många av du kanske har erfarenhet på SQL-databaser eller minst ha begreppet [relationella modellering av data](https://en.wikipedia.org/wiki/Relational_model) och du kanske tro toostart rita ut så här:

![Diagram som illustrerar en relativ relationella modell](./media/social-media-apps/social-media-apps-sql.png) 

En perfekt normaliserade och snyggt datastruktur... som inte kan skalas. 

Hämta mig fel, jag har arbetat med SQL-databaser mitt liv, de är bra, men som var mönster, praxis och programvara plattform är inte perfekt för varje scenario.

Varför visas inte SQL hello bästa valet i det här scenariot? Nu ska vi titta på hello struktur för en enda post, om jag ville tooshow i en webbplats eller ett program måste toodo en fråga med... tabell 8 kopplingar (!) bara tooshow en enda post, nu bilden som en dataström med inlägg som dynamiskt läsa in och visas på hello-skärmen och du kan se där vi.

Vi kan självklart använder en humongous SQL-instans med tillräckligt med power toosolve tusentalsavgränsare frågor med dessa många kopplingar tooserve innehållet, men verkligen, varför skulle vi när en enklare lösning finns?

## <a name="hello-nosql-road"></a>Hej NoSQL väg
Den här artikeln hjälper dig i modeling din sociala plattform data med Azures NoSQL-databas [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) på ett kostnadseffektivt sätt samtidigt utnyttja andra Azure DB som Cosmos-funktioner som hello [Gremlin Graph API ](../cosmos-db/graph-introduction.md). Med hjälp av en [NoSQL](https://en.wikipedia.org/wiki/NoSQL) metod att lagra data i JSON-format och tillämpa [denormalization](https://en.wikipedia.org/wiki/Denormalization), våra tidigare komplicerad post kan omvandlas till en enda [dokument](https://en.wikipedia.org/wiki/Document-oriented_database):


    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"hello first video"},
            {"url":"http://mysecondvideo.mp4", "title":"hello second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"hello first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"hello second audio"}
        ]
    }

Och kan hämtas med en enda fråga, och inga kopplingar. Detta är mycket mer lätt och budget-wise, den kräver färre resurser tooachieve ett bättre resultat.

Azure Cosmos-DB ser till att alla hello egenskaper som indexeras med dess automatisk indexering, som även kan vara [anpassade](indexing-policies.md). Hej schemafria metod kan vi lagra dokument med olika och dynamiska strukturer, kanske imorgon vi vill inlägg toohave kommer att hantera en lista över kategorier eller hash-taggar som är kopplade till dem, Cosmos DB hello nya dokument med hello läggs attribut med utan extra arbetet som krävs av oss.

Kommentarer om en post kan behandlas som bara andra inlägg med en överordnad-egenskap (Detta förenklar våra objektmappningen). 

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

Och all kommunikation kan lagras på ett separat objekt som räknare:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Att skapa flöden är bara en fråga om hur du skapar dokument som kan innehålla en lista över post-ID: n med en viss relevans ordning:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Vi kan ha en ”senaste” dataström med listan sorteras efter skapandedatum, en ”senaste” dataström med de skickar med mer om i hello senaste 24 timmarna, vi kan även genomföra en anpassad dataström för varje användare baserat på logik som blandare och intressen och det kan fortfarande vara en lista över  Bokför. Det beror på hur toobuild dessa visas, men hello läsning prestanda förblir röra sig obehindrat. När vi skaffar en av dessa listor kan vi utfärda en enskild fråga tooCosmos DB med hello [i operatorn](documentdb-sql-query.md#WhereClause) tooobtain sidor i inlägg i taget.

hello feed dataströmmar kan byggas med [Azure App Services](https://azure.microsoft.com/services/app-service/) background processer: [Webjobs](../app-service-web/web-sites-create-web-jobs.md). När en post har skapats behandling i bakgrunden kan aktiveras med hjälp av [Azure Storage](https://azure.microsoft.com/services/storage/) [köer](../storage/queues/storage-dotnet-how-to-use-queues.md) och Webjobs aktiveras med hjälp av hello [Azure Webjobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md), implementering hello efter spridning i strömmar som baseras på våra egna anpassade logik. 

Punkter och om över ett inlägg kan bearbetas i en uppskjuten sätt med hjälp av den här samma teknik toocreate en överensstämmelse miljö.

Blandare är svårare. Cosmos DB har en storleksgräns för maximal dokumentet och läsning/skrivning stora dokument kan påverka hello skalbarhet. Så får du tycker om lagra blandare som ett dokument med den här strukturen:

    {
        "id":"234d-sd23-rrf2-552d",
        "followersOf": "dse4-qwe2-ert4-aad2",
        "followers":[
            "ewr5-232d-tyrg-iuo2",
            "qejh-2345-sdf1-ytg5",
            //...
            "uie0-4tyg-3456-rwjh"
        ]
    }

Detta kan fungera för en användare med några tusentalsavgränsare styrstavsföljare, men om vissa ryktbarhet ansluter till vår rangordningar, den här metoden leder tooa stora storlek och kan slutligen träffar hello storlek cap.

toosolve kan vi kan använda en blandad metod. Som en del av hello statistik dokument kan vi lagra hello antal blandare:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

Och hello faktiska diagram över blandare kan lagras i Azure Cosmos DB [Gremlin Graph API](../cosmos-db/graph-introduction.md), toocreate [brytpunkter](http://mathworld.wolfram.com/GraphVertex.html) för varje användare och [kanter](http://mathworld.wolfram.com/GraphEdge.html) som underhåller hello ” Ett sätt B ”relationer. hello Graph API vi ska du inte bara hämta hello blandare med en viss användare men skapa mer komplexa frågor tooeven i vanliga föreslår personer. Om vi lägger till toohello diagram hello innehåll kategorier som personer som eller få, kan vi börja vävning upplevelser som innehåller smart innehållsidentifiering föreslå innehåll än de som vi följer som eller söka efter personer som vi kanske har mycket gemensamt.

hello statistik dokument kan fortfarande vara används toocreate kort i hello Användargränssnittet eller snabb profil förhandsgranskningar.

## <a name="hello-ladder-pattern-and-data-duplication"></a>Hej ”stege” mönstret och data duplicering
Som du såg i hello JSON-dokumentet som refererar till en post, finns det flera förekomster av en användare. Och du skulle ha gissa höger, det innebär att hello information som representerar en användare får den här denormalization kan finnas i mer än en plats.

I ordning tooallow för snabbare frågor, uppstår i samband med dataduplicering. hello problem med den här sidoeffekt är att om av en åtgärd som en användare dataändringar vi behöver toofind alla hello aktiviteter han någonsin kunde och uppdatera dem alla. Inte ljudet praktisk, höger?

Vi kan gå toosolve den genom att identifiera hello nyckelattribut för en användare som beskrivs i vårt program för varje aktivitet. Om vi visuellt visa en post i vårt program och visar bara hello creator namn och bild, Varför spara alla hello användardata i hello ”Skapad” attributet? Om för varje kommentar visar vi bara hello användares bild, behöver vi inte verkligen hello resten av sin information. Det är där något jag anropa hello ”stege mönstret” kommer i play.

Låt oss ta användarinformation som exempel:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

Genom att titta på den här informationen kan vi snabbt identifiera som är viktig information och som därmed inte kan skapa ”stege”:

![Diagram över ett stege mönster](./media/social-media-apps/social-media-apps-ladder.png)

hello minsta steg kallas en UserChunk, hello minimal mängd information som identifierar en användare och den används för dataduplicering. Genom att minska hello storleken på hello dupliceras tooonly hello informationen ”visas” minska vi hello risken för massiv uppdateringar.

hello mellersta steget kallas hello användaren, är det fullständiga hello-data som ska användas på de flesta prestanda beroende frågor på Cosmos DB hello mest ofta och viktiga. Den omfattar hello information som representeras av en UserChunk.

hello största är hello utökad användare. Den innehåller alla hello kritiska användarinformation samt andra data som verkligen inte kräver toobe Läs snabbt eller dess användning är eventuell (till exempel hello inloggningen). Dessa data kan lagras utanför Cosmos-DB i Azure SQL Database eller Azure Storage-tabeller.

Varför skulle vi dela hello användaren och även spara informationen på olika platser? Eftersom från en prestanda synvinkel hello större hello dokument hello costlier hello-frågor. Skydda dokument tunn med hello rätt information toodo alla dina prestanda-beroende frågar för det sociala nätverket och store hello andra extra information för eventuell scenarier som, fullständig profil redigeringar, inloggningar, även datautvinning för användningsanalys och Big Data initiativ. Vi verkligen bryr dig inte om hello datainsamling för datautvinning är långsammare eftersom den körs på Azure SQL Database, vi har gäller dock att våra användare har en snabb och smidig upplevelse. En användare som lagras på Cosmos DB skulle se ut så här:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"@john"
    }

Och en Post skulle se ut:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

Och när du redigerar uppstår där hello attribut av hello segment påverkas, är det enkelt toofind hello påverkas dokument med hjälp av frågor som pekar toohello indexerade attribut (Välj * FROM anslår p var p.createdBy.id == ”edited_user_id”) och sedan uppdaterar hello segment.

## <a name="hello-search-box"></a>hello sökrutan
Användare skapar som tur är mycket innehåll. Och vi ska kunna tooprovide hello möjlighet toosearch och söka efter innehåll som inte är direkt i deras innehåll dataströmmar kanske eftersom vi inte följer hello skapare, eller kanske vi försöker bara toofind gamla inlägget vi gjorde 6 månader sedan.

Tack och lov, och eftersom vi använder Azure Cosmos DB vi enkelt implementera motorn hjälp [Azure Search](https://azure.microsoft.com/services/search/) på några minuter och utan att skriva en enda rad kod (andra än självklart hello Sök processen och användargränssnitt).

Varför är det så enkelt?

Azure Search implementerar de anropa [indexerare](https://msdn.microsoft.com/library/azure/dn946891.aspx)bakgrund bearbetar den hooken i data-databaser och automagically lägga till, uppdatera eller ta bort dina objekt i hello index. De stöder en [Azure SQL Database indexerare](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure BLOB indexerare](../search/search-howto-indexing-azure-blob-storage.md) och tack och lov, [Azure Cosmos DB indexerare](../search/search-howto-index-documentdb.md). hello övergången av information från Cosmos DB tooAzure sökning är enkel som både store-information i JSON-format måste vi bara för[skapa indexet](../search/search-create-index-portal.md) och mappa vilka attribut från våra dokument vi vill indexerade och det i några minuter (beroende på hello storleken på våra data), alla våra innehållet blir tillgängligt toobe eftersöks av hello bästa sökning som en tjänst-lösning i cloud-infrastruktur. 

Mer information om Azure Search kan du besöka hello [Hitchhiker's Guide tooSearch](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="hello-underlying-knowledge"></a>hello underliggande knowledge
När du sparar det här innehållet som växer och växer varje dag, kan vi sitter vi nu tänker: Vad kan jag göra med den här dataströmmen av information från Mina användare?

hello svaret är enkla: placera den toowork och lära dig från den.

Men det kan vi lära dig? Några enkelt exempel är [sentiment analysis](https://en.wikipedia.org/wiki/Sentiment_analysis), innehåll rekommendationer baserat på användarens inställningar eller även en automatiserad innehåll kontrollant som garanterar att alla hello innehåll som publicerats av vårt sociala nätverk är säker för hello familj.

Nu när jag fick du ansluta tänker du förmodligen behöver du vissa PhD i matematiska vetenskap tooextract dessa mönster och information från enkla databaser och filer, men du kommer att fel.

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), en del av hello [Cortana Intelligence Suite](https://www.microsoft.com/en/server-cloud/cortana-analytics-suite/overview.aspx), är hello en helt hanterad molntjänst som låter dig skapa arbetsflöden med hjälp av algoritmer i ett enkelt dra och släpp-gränssnitt, code egna algoritmer i [R](https://en.wikipedia.org/wiki/R_\(programming_language\)) eller använda en del av hello som redan har byggts och klara toouse API: er som: [textanalys](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [innehåll kontrollant](https://www.microsoft.com/moderator) eller [rekommendationer](https://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

tooachieve någon av dessa Machine Learning-scenarier kan vi använda [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) tooingest hello information från olika källor och använda [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) tooprocess hello information och generera utdata som kan bearbetas av Azure Machine Learning.

Ett annat tillgängliga alternativ är toouse [Microsoft kognitiva Services](https://www.microsoft.com/cognitive-services) tooanalyze våra användare innehåll, inte bara kan vi förståelse bättre (via analysera de skriver med [Text Analytics API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), men Vi kan även identifiera oönskade eller mogen innehåll och fungerar därför med [datorn Vision API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Kognitiva tjänster omfattar en mängd out box-lösningar som inte kräver någon form av Machine Learning knowledge toouse.

## <a name="a-planet-scale-social-experience"></a>En planeten skala-upplevelsen
Det finns en sista, men inte minst viktiga avsnittet jag måste lösa: **skalbarhet**. När du utformar en arkitektur som det är viktigt att varje komponent kan skala på egen hand, antingen eftersom vi behöver tooprocess mer data eller eftersom vi vill toohave större geografiska täckningen (eller båda!). Tack och lov, uppnå en komplicerad uppgift är en **NYCKELFÄRDIGT upplevelse** med Cosmos DB.

Har stöd för cosmos DB [dynamisk partitionering](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) out-of-the-box genom att automatiskt skapa partitioner baserat på en viss **partitionsnyckel** (definieras som en hello attribut i dokument). Definiera hello rätt partitionsnyckel som måste göras vid designtillfället och att ha i åtanke hello [metodtips](../cosmos-db/partition-data.md#designing-for-partitioning) tillgängliga; hello gäller en upplevelsen, din strategi för partitionering måste vara lika justerade hello sätt du fråga (läsningar samma partition inom hello är önskvärt) och skriva (undvika ”aktiva punkter” genom att sprida skrivningar på flera partitioner). Vissa alternativ är: partitioner baserat på en temporal nyckel (dag/månad/vecka), med innehåll kategori av geografisk region, av användaren. Det alla beror på hur du ska fråga hello data och visa den i din-upplevelsen. 

En intressant punkt värt att nämna att Cosmos DB kör dina frågor (inklusive [mängder](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) för alla partitioner transparent, behöver du inte tooadd alla logik som datamängden växer.

Med tiden kan du slutligen växer i trafik och förbrukning av nätverksresurser (mätt i [RUs](request-units.md), eller begära enheter) kommer att öka. Du kan läsa och skriva oftare när din userbase växer och de kommer att börja skapa och läsa mer innehåll. Hej möjligheten för **skala din genomströmning** är viktigt. Öka våra RUs är mycket enkelt, vi kan göra det med några få klick på hello Azure-portalen eller genom att [utfärda kommandon via hello API](https://docs.microsoft.com/rest/api/documentdb/replace-an-offer).

![Skala upp och definiera en partitionsnyckel](./media/social-media-apps/social-media-apps-scaling.png)

Vad händer om saker får bättre och användare från en annan region, land eller kontinent, Lägg märke till din plattform och börja använda det en bra oväntat!

Men vänta... du inser snart sina erfarenheter din plattform inte är den optimala; de är så långt från din operativa region hello svarstiden är förskräckliga ut, och du givetvis inte vill att de tooquit. Om bara det uppstod ett enkelt sätt att **utöka räckhåll globala**... men det finns!

Cosmos DB kan du [replikeras dina data globalt](../cosmos-db/tutorial-global-distribution-documentdb.md) och transparent med ett par klick och automatiskt välja bland tillgängliga regioner, hello från din [klientkod](../cosmos-db/tutorial-global-distribution-documentdb.md). Det innebär också att du har [flera redundans regioner](regional-failover.md). 

När dina data replikeras globalt måste toomake till att klienterna kan dra nytta av den. Om du använder en klientdel web eller öppnar API: er från mobila klienter, kan du distribuera [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) och klona din Azure App Service på alla hello önskad regioner, som använder en [prestanda configuration](../app-service-web/web-sites-traffic-manager.md)toosupport din utökade global täckning. När klienterna har åtkomst till din klientdel eller API: er, kommer de att vara routade toohello närmaste Apptjänst, som i sin tur ansluts toohello lokala Cosmos-DB-repliken.

![Lägga till global täckning tooyour sociala plattform](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Slutsats
Den här artikeln försöker tooshed vissa lysa i hello alternativ för att skapa sociala nätverk helt på Azure med prisvärda tjänster och tillhandahålla goda resultat genom att uppmuntra hello användning av en flera lager lagring lösningen och data distributionsplats som kallas ”stege”.

![Diagram över interaktion mellan Azure-tjänster för sociala nätverk](./media/social-media-apps/social-media-apps-azure-solution.png)

hello sanningen är att det finns ingen silver punkt för den här typen av scenarier, den har hello samverkan som skapats av hello kombination av bra tjänster som kan vi toobuild bra upplevelser: hello hastighet och fria Azure Cosmos DB tooprovide ett bra sociala program hello intelligence bakom en lösning för förstklassigt sökning som Azure Search, hello flexibilitet i Azure App Service toohost inte även språkoberoende program men kraftfull bakgrundsprocesser och hello utbyggbara Azure Storage- och Azure SQL Database för lagra stora mängder data och hello analytiska kraften i Azure Machine Learning toocreate leverera kunskap och intelligence som kan ge feedback tooour processer och hjälp oss att hello rätt innehåll toohello rätt användare.

## <a name="next-steps"></a>Nästa steg
toolearn mer om användningsfall för Cosmos DB finns [vanliga Cosmos-DB användningsfall](use-cases.md).
