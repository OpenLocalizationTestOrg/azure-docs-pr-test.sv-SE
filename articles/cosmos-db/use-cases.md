---
title: "aaaCommon använder fall och scenarier för Azure Cosmos DB | Microsoft Docs"
description: "Lär dig mer om hello upp fem användningsfall för Azure Cosmos DB: användaren genereras innehåll, händelseloggning, katalogdata, inställningar för användardata och Sakernas Internet (IoT)."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: eca68a58-1a8c-4851-8cf8-6e4d2b889905
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: mimig
ms.openlocfilehash: 115a418e7b18d5033c4d7e64591bf302aea0190b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="common-azure-cosmos-db-use-cases"></a>Vanliga användningsområden för Azure Cosmos DB
Den här artikeln innehåller en översikt över flera vanliga användningsområden för Azure Cosmos DB.  hello rekommendationerna i den här artikeln fungera som en startpunkt som du utvecklar programmet med Cosmos DB.   

När du har läst den här artikeln att du kunna tooanswer hello följande frågor: 

* Vad är hello vanliga användningsområden för Azure Cosmos DB?
* Vad är hello fördelarna med att använda Azure Cosmos DB för retail program?
* Vad är hello fördelarna med att använda Azure Cosmos DB som ett datalager för system Sakernas Internet (IoT)?
* Vad är hello fördelarna med att använda Azure Cosmos DB för webb- och mobilprogram?

## <a name="introduction"></a>Introduktion
[Azure Cosmos-DB](../cosmos-db/introduction.md) är Microsofts tjänst för globalt distribuerad databas. hello-tjänsten är utformad tooallow kunder tooelastically (och oberoende) skala dataflöden och lagringsutrymmen till alla geografiska regioner. Azure Cosmos-databasen är hello första globalt distribuerad databastjänsten i hello marknaden idag toooffer omfattande [serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/) omfattar dataflöde, svarstid, tillgänglighet och konsekvenskontroll. 

hello Azure Cosmos DB projektet igång i 2011 som ”projekt Florens” tooaddress developer-stötestenar som stora Internet-skala program inom Microsoft inför. Observera att dessa problem är inte unikt tooMicrosoft program, vi valt toomake Azure Cosmos DB allmänt tillgänglig tooexternal utvecklare i 2015 i formatet hello [Azure DocumentDB](https://azure.microsoft.com/blog/documentdb-moving-to-general-availability/). hello tjänsten används ubiquitously internt på Microsoft och är en av hello snabbast växande tjänster som används av Azure utvecklare externt. 

Azure Cosmos-DB är en global distribuerade och flera olika modeller databas som används i en mängd olika program och användningsfall. Det är ett bra alternativ för alla [serverlösa](http://azure.com/serverless) program som måste låg ordning millisekunder svarstider och måste tooscale snabbt och globalt. Den stöder flera datamodeller (nyckelvärde, dokument, diagram och kolumner) och många API: er för data åt inklusive [MongoDB](mongodb-introduction.md), [DocumentDB SQL](documentdb-introduction.md), [Gremlin](graph-introduction.md), och [Azure tabeller](table-introduction.md) internt och på extensible sätt. 

hello följande är några attribut i Azure Cosmos DB som gör det passar bra för program med höga prestanda med globala ambitionsnivå.

* Azure Cosmos-DB partitionerar internt data för hög tillgänglighet och skalbarhet. Azure Cosmos-DB erbjuder 99,99% garantier för tillgänglighet, dataflöde, låg latens och konsekvenskontroll.
* Azure Cosmos-DB har SSD-baserad lagring med låg latens ordning millisekunder svarstider.
* Azure Cosmos DBS stöd för konsekvensnivåer som eventuell och konsekvent prefix, session och begränsat föråldrad möjliggör fullständig flexibilitet och låg kostnad till prestanda förhållandet. Ingen databas service erbjuder så mycket flexibilitet som Azure Cosmos DB i nivåer konsekvenskontroll. 
* Azure Cosmos-DB har en flexibel data eget prisnivå modell som mätare lagring och genomströmning oberoende av varandra.
* Azure Cosmos DB reserverat dataflöde modell kan du toothink vad gäller antalet läsning/skrivning i stället för CPU/minne/IOPs för hello underliggande maskinvara.
* Azure Cosmos DB design kan du skala toomassive begäran volymer i hello ordning biljoner förfrågningar per dag.

Dessa attribut är användbara i webb-, mobil-, spel- och IoT-appar som måste låg svarstid och toohandle enorma mängder läsningar och skrivningar.

## <a name="iot-and-telematics"></a>IoT och telematik
IoT-användningsfall ofta delar vissa mönster i hur de infognings-, process, och lagra data.  Dessa system måste först tooingest belastning av data från enheten sensorer för olika språk. Sedan dessa system bearbeta och analysera strömmande data tooderive realtidsinsikter. hello data är sedan arkiveras toocold lagring för batch analytics. Microsoft Azure erbjuder omfattande tjänster som kan användas för IoT-användningsfall inklusive Azure Cosmos DB, Azure Event Hubs, Azure Stream Analytics, Azure Notification Hub, Azure Machine Learning, Azure HDInsight och PowerBI. 

![Azure IoT för Cosmos-DB-Referensarkitektur](./media/use-cases/iot.png)

Belastning av data kan vara inhämtas av Händelsehubbar i Azure som den erbjuder datapåfyllning i högt dataflöde med låg latens. Data som inhämtas som behöver bearbetas toobe realtid information kan vara går tooAzure Stream Analytics för realtidsanalys. Data kan läsas in i Azure Cosmos DB för ad hoc-frågor. När hello data läses in i Azure Cosmos DB, är hello data klar toobe efterfrågas. Dessutom kan kan nya data och ändringar tooexisting data läsas på Ändra feed. Ändra feeden är en beständig, Lägg till endast logg som lagrar ändras tooCosmos DB samlingar i följd. hello alla data eller bara ändringar toodata i Azure Cosmos DB kan användas som för referensdata som en del av realtidsanalys. Dessutom kan data ytterligare förfinad och bearbetas av anslutande Azure Cosmos DB data tooHDInsight för Pig, Hive eller karta/minska jobb.  Förfinad data läses sedan tillbaka tooAzure Cosmos DB för rapportering.   

En exempel IoT-lösning med hjälp av Azure Cosmos DB, EventHubs och Storm finns hello [hdinsight-storm-exempel lagringsplats på GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Läs mer på Azure-erbjudanden för IoT [skapa hello din Sakernas Internet](http://www.microsoft.com/server-cloud/internet-of-things.aspx). 

## <a name="retail-and-marketing"></a>Återförsäljarversion och marknadsföring
Azure Cosmos-DB används ofta i Microsofts egna e-handel plattformar, som kör hello Windows Store och XBox Live. Det används också i hello detaljhandel för lagring av data catalog och för händelsen som källa i ordning bearbetning pipelines.

Användningsscenarier för katalogen data omfattar lagring och hämtning av en uppsättning attribut för entiteter, till exempel personer, platser och produkter. Några exempel på katalogdata är användarkonton, produktkataloger, register för IoT-enhet och faktura för material som system. Attribut för den här informationen kan variera och kan förändras över tid toofit programkrav.

Överväg ett exempel på en produktkatalogen för en bil delar leverantör. Varje del kan ha sin egen attribut i tillägg toohello vanliga attribut som delar alla delar. Attribut för en viss del kan dessutom ändra hello efter år när en ny modell släpps. Azure Cosmos-DB stöder flexibel scheman och hierarkiska data och därmed den passar bra för att lagra produkten katalogdata.

![Azure DB Cosmos retail katalog Referensarkitektur](./media/use-cases/product-catalog.png)

Azure Cosmos-DB används ofta för händelsen källa toopower händelse drivs arkitekturer med dess [ändra feed](change-feed.md) funktioner. hello ändra feed innehåller underordnade mikrotjänster hello möjlighet tooreliably och inkrementellt Läs infogar och uppdateringar (till exempel ordning händelser) blir tooan Azure Cosmos DB. Den här funktionen kan vara balanserad tooprovide en beständig händelse lagra som förhandlare meddelande för ändring av tillstånd händelser och enheten bearbetning arbetsflöde mellan många mikrotjänster (som kan implementeras som [serverlösa Azure Functions](http://azure.com/serverless)).

![Azure Cosmos DB ordning pipeline Referensarkitektur](./media/use-cases/event-sourcing.png)

Data som lagras i Azure Cosmos DB kan dessutom integreras med HDInsight för analys av stordata med Apache Spark-jobb. Mer information om hello Spark-koppling för Azure Cosmos DB finns [kör ett Spark-jobb med Cosmos-databas och HDInsight](spark-connector.md).

## <a name="gaming"></a>Spel
hello databasnivå är en viktig komponent i spelappar. Moderna spel utföra grafiska bearbetning på mobile-konsolen klienter, men förlitar sig på hello molnet toodeliver anpassas och anpassat innehåll som i spelet statistik, sociala media integration och hög poäng ledartavlor. Spel ofta kräver enskild millisekunders latens för läser och skriver tooprovide en bra upplevelse i spelet. En spel databas måste toobe snabb och inte kan toohandle massiv toppar i begäran priser under nytt spel startar och funktionsuppdateringar.

Azure Cosmos-DB används av spel som [hello går döda: Nej Man mark](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) av [nästa spel](http://www.nextgames.com/), och [Halo 5: vårdnadshavare](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Azure Cosmos-DB innehåller hello följande fördelar toogame utvecklare:

* Azure Cosmos-DB kan prestanda toobe skalas uppåt eller nedåt Elastiskt. Detta gör att spel toohandle uppdatera profilen och statistik från dussintals toomillions av samtidiga spelare genom att göra en enda API-anrop.
* Azure DB Cosmos stöder millisekunder läser och skriver toohelp undvika eventuella beräkningstider under spelet.
* Azure Cosmos DB automatisk indexering tillåter filtrering mot flera olika egenskaper i realtid, till exempel spelare av deras interna player ID: N eller deras GameCenter, Facebook, Google-ID: N eller fråga baserat på player medlemskap i en guild. Detta är möjligt utan att skapa komplexa indexera eller infrastruktur för horisontell partitionering.
* Sociala funktioner inklusive i spelet chatt meddelanden player guild medlemskap, utmaningar slutförts, hög poäng ledartavlor och sociala diagram är enklare tooimplement med ett flexibelt schema.
* Azure Cosmos-databas som en hanterad plattform-som-en tjänst (PaaS) krävs minimal installation och hantering av arbete tooallow för snabb iteration och minska tiden toomarket.

![Referensarkitektur för Azure DB Cosmos-spel](./media/use-cases/gaming.png)

## <a name="web-and-mobile-applications"></a>Webb- och mobilprogram
Azure Cosmos-DB används ofta inom webb- och mobilprogram och passar bra för modellering kommunikation, integrera med tjänster från tredje part och för att skapa omfattande personlig upplevelse. Hej Cosmos DB SDK kan vara används build omfattande iOS och Android-program med hjälp av hello populära [Xamarin framework](mobile-apps-with-xamarin.md).  

### <a name="social-applications"></a>Sociala program
Ett vanligt användningsfall för Azure Cosmos DB är toostore och fråga användargenererat innehåll (UGC) för webb-, mobil och sociala media program. Några exempel på UGC är chatt sessioner, tweets, blogginlägg, klassificeringar och kommentarer. Hej UGC i sociala medieprogram är ofta en blandning av fri text, egenskaper, taggar och relationer som inte begränsas av hårda uppbyggnad. Innehåll, till exempel chatt, kommentarer och tjänster kan lagras i Cosmos DB utan transformationer eller komplexa objektet toorelational mappning lager.  Egenskaper för data kan läggas till eller ändras enkelt toomatch krav som utvecklare iterera över hello programkod därför främja snabb utveckling.  

Program som integreras med tredje parts sociala nätverk måste svara toochanging scheman från dessa nätverk. Data indexeras automatiskt som standard i Cosmos-databasen, som data är klar toobe efterfrågas när som helst. Programmen har därför hello flexibilitet tooretrieve projektioner enligt deras respektive behov.

Många av sociala hello-program körs på global nivå och kan den orsaka oförutsägbara användningsmönster. Flexibilitet vid skalning hello datalagret är viktigt eftersom hello programnivå skalas toomatch användarkrav.  Du kan skala ut genom att lägga till ytterligare datapartitioner under en Cosmos-DB-konto.  Dessutom kan du också skapa ytterligare Cosmos DB konton över flera regioner. Tjänsttillgänglighet Cosmos DB region, se [Azure-regioner](https://azure.microsoft.com/regions/#services).

![Azure DB Cosmos web app Referensarkitektur](./media/use-cases/apps-with-global-reach.png)

### <a name="personalization"></a>Personanpassning
Men i dag medföljer moderna program komplexa vyer och erfarenheter. Detta är vanligtvis dynamiska restaurang toouser inställningar eller stämningsläge- och anpassning behov. Därför kan program behöver toobe kan tooretrieve av personliga inställningar effektivt toorender UI-element och upplevelser snabbt. 

JSON, ett format som stöds av Cosmos DB är en effektiv format toorepresent UI layoutdata som det är inte bara lightweight, men även kan enkelt tolkas av JavaScript. Cosmos DB erbjuder justerbara konsekvensnivåer som tillåter snabb läsning med låg latens skrivningar. Därför kan lagra UI layoutdata inklusive personliga inställningar som JSON-dokument i Cosmos-databasen är ett effektivt sätt tooget data över hello överföring.

![Azure DB Cosmos web app Referensarkitektur](./media/use-cases/personalization.png)

## <a name="next-steps"></a>Nästa steg
tooget igång med Azure Cosmos DB Följ våra [snabbstarter](create-documentdb-dotnet.md), som hjälper dig att skapa ett konto och komma igång med Cosmos DB. 

Eller, om du vill läsa mer om kunder som använder Cosmos DB tooread hello följande artiklar för kunden är tillgängliga:

* [Jet.com](https://jet.com). E-handel utmanare ögon hello översta nivå, körs på hello Microsoft cloud, utnyttjar Cosmos DB på global nivå.
* [Asos.com](http://www.asos.com/). Asos.com är en brittiska Mode och bästa onlinebutik. I huvudsak avsedda för barn vuxna, säljer Asos över 850 märken som sin egen uppsättning klädesplagg och tillbehör.
* [Toyota](https://www.toyota.com/). Toyota meddelar Corporation är japanska bil tillverkare. Toyota utnyttjas Cosmos DB för en global IoT-app.
* [Citrix](https://customers.microsoft.com/story/citrix). Citrix utvecklar single-sign-on lösning med hjälp av Azure Service Fabric och Azure Cosmos DB
* [TEXA](https://customers.microsoft.com/story/texaspa) TEXAS revolutionerande IoT-lösningen för vehicle ägare kan du spara tid, pengar, gas – och troligtvis bor.
* [Domino's Pizza](https://www.dominos.com). Domino's Pizza Inc. är en American pizza restaurang kedja.
* [Johnson styr](http://www.johnsoncontrols.com). Johnson kontroller är en global diversifierat teknik och flera industriella ledande betjäna ett stort antal kunder i fler än 150 länder.
* [Microsoft Windows Universal Store, Azure IoT Hub, Xbox Live och andra tjänster på Internet-skala](https://azure.microsoft.com/blog/how-azure-documentdb-planet-scale-nosql-helps-run-microsoft-s-own-businesses/). Hur skapar Microsoft massivt skalbara tjänster med hjälp av Azure Cosmos DB.
* [Microsoft Data och analyser team](https://customers.microsoft.com/story/microsoftdataandanalytics). Microsofts team för Data och analyser uppnår planeten skala big-datainsamling med Azure Cosmos DB
* [Sulekha.com](https://customers.microsoft.com/story/sulekha-uses-azure-documentdb-to-connect-customers-and-businesses-across-india). Sulekha används Azure Cosmos DB tooconnect kunder och företag över Indien.
* [NewOrbit](https://customers.microsoft.com/story/neworbit-takes-flight-with-azure-documentdb). NewOrbit tar det rör sig med Azure Cosmos DB.
* [Affinio](https://customers.microsoft.com/doclink/affinio-switches-from-aws-to-azure-documentdb-to-harness-social-data-at-scale). Affinio växlar från AWS tooAzure Cosmos DB tooharness sociala data i större skala.
* [Nästa spel](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). hello Käppar Dead: Nej Man's mark spel soars för #1 stöds av Azure Cosmos DB.
* [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Hur Halo 5 implementerats sociala spel med hjälp av Azure Cosmos DB.
* [Cortana Analytics galleriet](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Gallery - en skalbar community-webbplats som bygger på Azure Cosmos DB.
* [Enkelt](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Inledande Integrator får flerspråkig företag globala insyn i minuter med flexibla Molntekniker.
* [Nyheter Republiken](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Lägger till toohello nyheter tooprovide affärsinformation med syftet för engagerade medborgare. 
* [SGS internationella](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). För konsekvent färg över hela världen hello aktivera stora märken tooSGS. Och SGS aktiverar tooAzure.
* [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Världsledande Telenor använder hello molnet toomove med hello hastigheten på en start. 
* [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). hello-lagret på hello framtida körs på snabba Sök och hello enkelt flödet av data.
* [Nucleo](https://customers.microsoft.com/story/azure-based-software-platform-breaks-down-barriers-bet). Azure-baserad programvaruplattform uppdelad barriärer mellan företag och kunder
* [Weka](https://customers.microsoft.com/story/weka-smart-fridge-improves-vaccine-management-so-more-people-can-be-protected-against-diseases). Weka smarta kombinerad kyl förbättrar vaccin hantering så att flera personer kan skyddas mot sjukdomar
* [Orange Tribes](https://customers.microsoft.com/story/theres-more-to-that-food-app-than-meets-the-eye-or-the-mouth). Det finns flera toothat mat app än uppfyller hello ögat eller hello munnen.
* [Verklig Madrid](https://customers.microsoft.com/story/real-madrid-brings-the-stadium-closer-to-450-million-f). Verklig Madrid ger hello stadium närmare too450 miljoner fläktar kring hello jordglob med hello Microsoft Cloud.
* [Tuku](https://customers.microsoft.com/story/tuku-makes-car-buying-fun-with-help-from-azure-services). TUKU gör bil köpa roliga med hjälp av Azure-tjänster
