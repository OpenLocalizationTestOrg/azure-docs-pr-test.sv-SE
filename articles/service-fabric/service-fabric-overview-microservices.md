---
title: aaaIntroduction toomicroservices i Azure | Microsoft Docs
description: "En översikt över Varför skapa molnprogram med en mikrotjänster är viktiga för moderna programutveckling och hur Azure Service Fabric ger en plattform tooachieve detta."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: fae2be85-0ab4-4cd3-9d1f-e0d95fe1959b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell
ms.openlocfilehash: b11920b9105e7575390e8fcf0d1ef6ab3c632978
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="why-a-microservices-approach-toobuilding-applications"></a>Varför en mikrotjänster hanterar toobuilding program?
Som programutvecklare finns det inget i hur vi tror att om ett program i komponentdelar factoring. Det är hello centrala paradigmet Objektorientering, programvara abstraktioner och Komponentindelning. Den här factorization tenderar idag tootake hello form av klasser och gränssnitt mellan delade bibliotek och tekniklager. En riskbedömning utförs vanligtvis med en backend-store, affärslogik på mellannivå och en frontend användargränssnittet (UI). Vad *har* ändras över hello senare år är att vi, som utvecklare skapar distribuerade program som är för hello molnet och styrs av hello företag.

hello föränderliga behov är:

* En tjänst som har skapats och fungerar på skalan tooreach kunder i nya geografiska regioner (till exempel).
* Snabbare leverans av funktioner och möjligheter toobe kan toorespond toocustomer krav på ett sätt som är flexibel.
* Förbättrad användning tooreduce resurskostnader.

Dessa affärsbehov påverkar *hur* vi skapa program.

Mer information om metoden hello Azure toomicroservices [Mikrotjänster: ett program revolution drivs av hello molnet](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Monolitisk kontra mikrotjänster Designmetoden
Alla program utvecklas över tid. Lyckade program utvecklas genom att vara användbara toopeople. Misslyckade program utvecklas inte och så småningom är föråldrade. hello fråga blir: hur mycket vet du om dina krav idag, och vad de i hello framtida? Anta exempelvis att du skapar en reporting program för en avdelning. Du är säker på att programmet hello förblir inom företaget hello omfång och att hello rapporter är tillfällig. Ditt val av metoden skiljer sig från, säg, skapa en tjänst som ger video innehåll tootens miljoner kunder. 

Ibland kan komma åt något ut hello dörren som konceptbevis är hello intresseväckande faktor när du vet att programmet hello gjorts med senare. Det finns lite plats i utveckling över något som aldrig används. Dess hello vanliga engineering kompromiss. På hello däremot när företag talar om att skapa för hello moln, hello förväntan är tillväxt och användning. hello problemet är att tillväxt och skala oförutsägbart. Vi vill gärna toobe kan tooprototype snabbt vid också känna till att det finns på en sökväg toodeal med framtida lyckades. Det här är hello lean Start metod: skapa, mått, Läs och iterera.

Under hello klient – server era avsett vi toofocus om hur du skapar nivåindelat program med hjälp av specifika tekniker i varje nivå. hello termen *monolitisk* program är här för dessa metoder. hello gränssnitt avsett toobe mellan hello nivåer och en mer tätt kopplade design användes mellan komponenter inom varje nivå. Utvecklare som utformats och beräknade klasser som har kompilerats till bibliotek och länkade till några körbara filer och DLL-filer. 

Det finns fördelar toosuch monolitisk Designmetoden. Det är ofta enklare toodesign och har snabbare anrop mellan komponenter, eftersom dessa anrop ofta över kommunikation mellan processer (IPC). Alla tester också en produkt, som toobe flera personer-resurs effektivt. hello Nackdelen är att det finns ett nära kopplingen mellan nivåindelade lager och du kan skala enskilda komponenter. Om du behöver tooperform korrigeringar eller uppgraderingar, har du toowait för andra toofinish sina testning. Det är svårare toobe flexibel.

Mikrotjänster åtgärda dessa downsides och mer överensstämmer nära med hello föregående affärskrav, men de har också både fördelar och skulder. hello fördelarna med mikrotjänster är att var och en vanligtvis kapslar enklare business-funktioner, vilket du skala upp eller ned, testa, distribuera och hantera oberoende av varandra. En viktig fördel av en mikrotjänster metod är att team drivs mer affärsscenarier än av teknik som hello riskbedömning uppmuntrar. I praktiken mindre team utveckla en mikrotjänster baserat på ett scenario med kund och använda några tekniker som de själva väljer. 

Hello organisation behöver alltså inte toostandardize teknisk toomaintain mikrotjänster program. Enskilda team att egna tjänster kan göra vad klokt för dem baserat på teamet kunskaper eller vad är mest lämpliga toosolve hello problem. I praktiken kan lagra en uppsättning rekommenderade tekniker, till exempel en viss NoSQL eller web application framework, är att föredra.

hello Nackdelen med mikrotjänster finns i Hantera hello ökat antal olika enheter och mer komplexa distributioner och versioner som behandlar. Nätverkstrafiken mellan hello mikrotjänster ökar samt hello motsvarande nätverksfördröjningar. Många chatty, detaljerade tjänster är ett recept för utmaning prestanda. Utan verktyg toohelp visa dessa beroenden, är det svårt för ”finns” hello hela systemet. 

Standarder gör hello mikrotjänster metod fungerar genom att godkänna på hur toocommunicate som feltoleranta av endast hello saker du behöver från en tjänst i stället för fast kontrakt. Det är viktigt toodefine avtalen direkt i hello utforma eftersom services uppdatera oberoende av varandra. En annan beskrivning myntades för hur man designar med en mikrotjänster är ”detaljerade tjänstorienterad arkitektur (SOA)”.

***I sin enklaste handlar hello mikrotjänster Designmetoden om en frikopplad federation för tjänster med oberoende ändringar tooeach och överenskomna standarder för kommunikation.***

Som mer molnappar produceras personer upptäcker att den här uppdelning av hello övergripande appen till oberoende, scenariot fokuserar services är en bättre långsiktig strategi.

## <a name="comparison-between-application-development-approaches"></a>Jämförelse mellan programutveckling närmar sig
![Programutveckling för Service Fabric-plattformen][Image1]

1) En monolitisk app dividerat normalt med funktionella lager, till exempel webb-, business- och innehåller domän-specifika funktioner.

2) Du skalar en monolitisk app genom att klona på flera servrar för virtuella datorer/behållare.

3) Ett program för mikrotjänster separerar funktioner i separata mindre tjänster.

4) Hej mikrotjänster metoden skalar upp genom att distribuera varje tjänst oberoende av varandra, skapar instanser av dessa tjänster via servrar för virtuella datorer/behållare.

Designa med en mikrotjänster metod är inte en panacea för alla projekt, men den överensstämmer närmare med hello affärsmål som beskrivs ovan. Från och med en monolitisk metod kan användas om du vet att du har möjlighet hello toorework hello kod senare i en mikrotjänster design. Vanligare, börja med ett monolitisk program och dela långsamt upp stegvis, från och med hello huvudområden som behöver toobe mer skalbart eller flexibel.

toosummarize, hello mikrotjänster metod är toocompose ditt program för många små tjänster. hello-tjänsterna köras i behållare som distribueras på ett kluster på datorer. Utveckla en tjänst som fokuserar på ett scenario med mindre grupper och testa oberoende versionen, distribuera och anpassa varje tjänst så att hela hello-program kan utvecklas.

## <a name="what-is-a-microservice"></a>Vad är en mikrotjänster?
Det finns olika definitioner av mikrotjänster. Om du söker hello Internet, hittar du många användbara resurser som tillhandahåller egna åsikter och definitioner. Men är de flesta av hello följande egenskaper för mikrotjänster ofta överens:

* Kapslar in ett kunden eller scenario. Vad är hello problem du lösa?
* Utvecklats av en liten teknikteamet.
* Skrivs i någon programmeringsspråket och använda alla framework.
* Består av koden och (valfritt) tillstånd som är oberoende av varandra version distribueras och skalas.
* Samverka med andra mikrotjänster via väldefinierade gränssnitt och protokoll.
* Unikt namn (URL) har använt tooresolve deras plats.
* Förbli konsekventa och finns tillgänglig i hello förekomst av fel.

Du kan sammanfatta dessa egenskaper till:

***Mikrotjänster program består av små, oberoende version och skalbar kunden fokuserar tjänster som kommunicerar med varandra via standardprotokoll med väldefinierade gränssnitt.***

Vi omfattas hello först två punkter i hello föregående avsnitt och nu när vi expanderar på och förtydliga hello andra.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Skrivs i någon programmeringsspråket och använda alla framework
Som utvecklare ska vi ledigt toochoose ett språk eller ramverk som vi vill, beroende på dina kunskaper eller hello behov hello-tjänsten. I vissa tjänster kanske du värdet hello prestandafördelarna med C++ framför allt annat. I andra tjänster kanske hello enkel hanterad utveckling i C# eller Java viktigaste. I vissa fall kan du behöva toouse en viss partner-biblioteket data lagringsteknik eller sätt att exponera hello tjänsten tooclients.

När du har valt en teknik kommer toohello operativa eller livscykelhantering och skalning av hello-tjänsten.

### <a name="allows-code-and-state-toobe-independently-versioned-deployed-and-scaled"></a>Tillåter kod och tillstånd toobe oberoende version distribueras och skalas
Men du väljer toowrite mikrotjänster, hello kod och eventuellt hello tillstånd bör oberoende distribuera, uppgradera och skala. Detta är faktiskt en hello svårare problem toosolve, eftersom det handlar om tooyour valet av tekniker. För att skala är det svårt att förstå hur toopartition (eller Fragmentera) både hello kod och tillstånd. När hello kod och tillståndet använder olika tekniker som är vanliga i dag, måste hello distribution skript för din mikrotjänster toobe kan toocope med skalning båda. Detta är även om rörligheten och flexibiliteten, så du kan uppgradera vissa hello mikrotjänster utan tooupgrade alla samtidigt.

Returnerar toohello monolitisk jämfört med mikrotjänster metod för ett ögonblick hello följande diagram visar hello skillnader i hello metoden toostoring tillstånd.

#### <a name="state-storage-between-application-styles"></a>Tillstånd lagring mellan program format
![Service Fabric-plattformen lagring av användartillstånd][Image2]

***hello monolitisk tillvägagångssättet hello vänster har en databas och för specifika tekniker.***

***Hej mikrotjänster tillvägagångssättet på hello rätt har ett diagram över sammankopplade mikrotjänster där tillståndet är vanligtvis begränsade toohello mikrotjänster och olika tekniker används.***

I en monolitisk metod vanligtvis använder hello program en enskild databas. hello fördelen är att det är en enda plats, vilket gör det enkelt toodeploy. Varje komponent kan ha en enskild tabell toostore dess tillstånd. Grupper måste toostrictly separat tillstånd, vilket är en utmaning. Gör en koppling mellan tabeller oundvikligen finns temptations tooadd en ny kolumn tooan befintliga kundtabell och skapa beroenden på hello lagringsskikt. När det händer kan du skala enskilda komponenter. 

Hej mikrotjänster inriktning varje tjänst hanterar och lagrar sin egen tillstånd. Varje tjänst ansvarar för att skala både koden och tillstånd tillsammans toomeet hello behoven hos hello-tjänsten. En Nackdelen är att om det finns en behovet toocreate vyer eller frågor, av programdata, behöver du tooquery i olika tillstånd butiker. Detta är normalt lösta genom att ha en separat mikrotjänster som skapar en vy över en mängd mikrotjänster. Om du behöver tooperform flera improviserat frågor på hello data bör varje mikrotjänster skriva dess data tooa datalagring datatjänst för analys av offline.

Versionshantering är särskilda toohello distribueras version av en mikrotjänster så att flera olika versioner distribuera och köra sida vid sida. Versionshantering adresser hello scenarier där en nyare version av en mikrotjänster misslyckas under uppgraderingen och måste tooroll tillbaka tooan tidigare version. hello utför andra scenarier för versionshantering A/B-typ, testa, där olika användare ha olika versioner av hello-tjänsten. Exempelvis är det vanliga tooupgrade en mikrotjänster för en specifik uppsättning kunder tootest nya funktioner innan den distribueras mer brett. Efter Livscykelhantering för mikrotjänster ger detta nu oss toocommunication mellan dem.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Samverkar med andra mikrotjänster via väldefinierade gränssnitt och protokoll
Det här avsnittet behöver lite åtgärdas här, eftersom omfattande dokumentation om tjänstorienterad arkitektur som har publicerats via hello senaste tio åren beskriver kommunikationsmönster. Tjänstkommunikation använder vanligtvis en REST-metod med HTTP- och TCP-protokoll och XML- eller JSON som hello serialiseringsformat. Från ett gränssnitt perspektiv handlar om omfattar hello web Designmetoden. Men ingenting slutar du från att använda binärt protokoll eller egna dataformat. Att förbereda för personer toohave taget svårare med din mikrotjänster om dessa är öppet tillgängliga.

### <a name="has-a-unique-name-url-used-tooresolve-its-location"></a>Ett unikt namn (URL) har använt tooresolve sin plats
Kom ihåg hur vi hålla om att hello mikrotjänster metod är som hello web? Som hello web måste din mikrotjänster toobe adresserbara där den körs. Om du funderar på datorer och vilket som kör en viss mikrotjänster, går saker felaktiga snabbt. 

I hello samma sätt att DNS matchar en viss URL tooa viss dator, din mikrotjänster måste toohave ett unikt namn så att dess aktuella plats går att identifiera. Mikrotjänster behöver adresserbara namn som gör dem oberoende av hello-infrastruktur som de körs på. Detta innebär att det finns en interaktion mellan hur din tjänst har distribuerats och hur det upptäcks, eftersom det måste toobe ett register för tjänsten. Lika, när en dator misslyckas hello registry-tjänsten måste berättar där hello-tjänsten körs nu. 

Detta leder fram toohello nästa ämne: konsekvent och återhämtning.

### <a name="remains-consistent-and-available-in-hello-presence-of-failures"></a>Förblir konsekventa och tillgängliga i hello förekomst av fel
Oväntade fel som behandlar är en av hello svåraste problem toosolve, särskilt i ett distribuerat system. Mycket av hello-kod som vi skriver som utvecklare hanterar undantag och det är också där hello de flesta tid det tar för testning. hello problem är mer komplicerat än att skriva kod toohandle fel. Vad händer när hello datorn där hello mikrotjänster körs inte? Inte bara måste toodetect felet mikrotjänster (hårda fel på en egen), men du måste också något toorestart din mikrotjänster. 

En mikrotjänster måste toobe flexibel toofailures och starta om ofta på en annan dator tillgänglighet skäl. Det kommer också ned toohello tillstånd som sparats uppdrag hello mikrotjänster där hello mikrotjänster kan återställa det här tillståndet från, och om hello mikrotjänster kan toorestart har. Det måste alltså toobe återhämtning i hello beräkning (hello omstart) samt återhämtning i hello eller data (inga data går förlorade och hello data förblir konsekventa).

hello problem med återhämtning är beredda under andra scenarier, till exempel när fel inträffa under en uppgradering av programmet. Hej behöver mikrotjänster, arbetar med hello distributionssystem inte toorecover. Toothen måste också bestämma om det fortsätta toomove vidarebefordra toohello nyare version eller i stället återställa tooa föregående version toomaintain ett konsekvent tillstånd. Frågor som om tillräckligt många datorer är tillgängliga tookeep framåt och hur toorecover tidigare versioner av hello mikrotjänster måste toobe anses vara. Detta kräver hello mikrotjänster tooemit hälsa information toobe kan toomake besluten.

### <a name="reports-health-and-diagnostics"></a>Rapporter hälsa och diagnostik
Det kan verka uppenbart och förbises ofta, men en mikrotjänster måste rapportera dess hälsa och diagnostik. Annars finns lite insikt från ett operations perspektiv. Korrelerar diagnostiska händelser i en oberoende tjänster och hantera datorn jämföra toomake uppfattning om hello händelse ordning är svårt. I hello formaterar samma sätt som du interagerar med en mikrotjänster över överenskomna protokoll och data, det uppstår ett behov av i hur toolog hälsa och diagnostik händelser som slutligen hamna i en händelse lagra för frågor och visa. I en mikrotjänster metod är den nyckel som olika teamen komma överens om en enda loggningsformat. Det måste toobe en konsekvent metod tooviewing diagnostiska händelser i hello programmet i helhet.

Hälsotillstånd skiljer sig från diagnostik. Hälsa är om hello mikrotjänster rapportering dess aktuella tillstånd tootake lämpliga åtgärder. Ett bra exempel arbetar med uppgradering och distribution mekanismer toomaintain tillgänglighet. Även om en tjänst kan inte felfri på grund av tooa processen krasch eller datorn startas om, kanske hello service fortfarande fungerar. hello sista du behöver är toomake denna worse genom att utföra en uppgradering. hello bästa tillvägagångssättet är toodo en undersökning först eller väntar tills hello mikrotjänster toorecover. Hälsotillstånd händelser från en mikrotjänster hjälp oss att fatta välgrundade beslut och innebär att skapa självläkande tjänster.

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric som en mikrotjänster plattform
Azure Service Fabric kläckas från en övergång av Microsoft från leverera rutan produkter, som vanligtvis monolitisk i style toodelivering tjänster. hello upplevelse av att bygga och drift av stora tjänster som Azure SQL Database och Azure Cosmos DB Formats Service Fabric. hello plattform som utvecklats med tiden fler och fler tjänster antas den. Service Fabric hade allt toorun inte bara i Azure, utan också i fristående Windows Server-distributioner.

***hello syftet med Service Fabric är toosolve hello hårda problem med att skapa och köra en tjänst och använda infrastrukturresurser effektivt, så att team kan hitta lösningar med hjälp av en mikrotjänster-metod.***

Service Fabric innehåller tre områden toohelp som du skapar program som använder en mikrotjänster metod:

* En plattform som tillhandahåller system services toodeploy, uppgradera, identifiera, och starta om misslyckade tjänster, identifiera tjänster, vidarebefordra meddelanden, hantera tillstånd och övervaka hälsotillstånd. System i kraft som möjliggör många av hello egenskaperna för mikrotjänster som beskrevs tidigare.
* Möjlighet toodeploy program antingen som körs i behållare eller som bearbetas. Service Fabric är en behållare och processen orchestrator.
* Produktiv programmering API: er, toohelp som du skapar program som mikrotjänster: [ASP.NET Core och Reliable Actors Reliable Services](service-fabric-choose-framework.md). Du kan välja någon kod toobuild din mikrotjänster. Men dessa API: er Se hello jobbet enklare och de integreras med hello plattform på en djupare nivå. På så sätt kan till exempel kan du få information om hälsa och diagnostik eller du kan dra nytta av inbyggd hög tillgänglighet.

***Du kan använda alla tekniken Service Fabric är storleksoberoende på hur du skapar din tjänst. Det ger dock inbyggda programming API: er som gör det enklare toobuild mikrotjänster.***

### <a name="migrating-existing-applications-tooservice-fabric"></a>Migrera befintliga program tooService Fabric
En nyckel metoden tooService Fabric är tooreuse befintlig kod, som kan sedan moderniseras med nya mikrotjänster. Det finns fem faser tooapplication modernisering och du kan starta och stoppa på någon av hello steg. Dessa är;

1) Ta en traditionell monolitisk program
2) Lyfter och SKIFT, Använd behållare eller gäst körbara filer toohost befintlig kod i Service Fabric.
3) Modernisering - nya mikrotjänster läggs tillsammans med befintliga av koden. 
4) Förnya - Bryt hello monolitisk i mikrotjänster enbart baseras på behov.
5) Omvandlas till mikrotjänster - hello omvandling av befintliga monolitisk program eller skapa nya greenfield program.

![Migrering tooMicroservices][Image3]

Det är viktigt tooemphasis igen, som du kan använda **starta och stoppa på någon av dessa steg**, du är inte tvingas toomoved toohello nästa steg. Låt oss nu titta på exempel för var och en av dessa steg.

**Lyfta och flytta** – många företag lyfta och rörliga befintliga monolitisk program i behållare toofor två grunder;

- Minskade kostnader antingen på grund av tooconsolidation och borttagning av befintlig maskinvara eller kör program på tätare. 
- Konsekvent distribution kontrakt mellan Utvecklingsavdelningen och åtgärder.

Kostnad minskning är att förstå och i Microsoft stort antal befintliga program som container bara toomillions dollar. Konsekvent distribution är svårare tooevaluate, men som lika viktiga. Det innebär att utvecklare kan fortfarande vara fria toochoose hello teknik som paket dem, men hello åtgärder endast accepterar ett enskilt sätt toodeploy och hantera dessa program. Det minskar hello åtgärder från att ha toodeal med hello komplexiteten i många olika tekniker eller tvinga utvecklare tooonly Välj vissa. I princip alla program är behållare i självständiga distributions-avbildningar.

Många organisationer stoppa här. De har redan hello fördelarna med behållare och Service Fabric ger hello fullständig hantering från distribution, uppgraderingar, versionshantering, återställningar, hälsa övervakning osv.

**Modernisering** -är hello tillägg av nya tjänster tillsammans med befintliga av koden. Om du ska toowrite ny kod, är det bästa toodecide tootake stegvis ned hello mikrotjänster sökväg. Detta kan lägger till en ny REST API-slutpunkt, eller nya affärslogik. På så sätt kan du börja på hello resa av skapa nya mikrotjänster och praxis som utvecklar och distribuerar dem.

**Förnya** -spara de ursprungliga ändrade affärsbehov hello början av den här artikeln som kör hello mikrotjänster metoden? I det här steget hello är beslut, är dessa inträffar toomy aktuella program och i så fall måste toostart dela hello monolitvolym eller införa. Här ett exempel är när en databas blir en flaskhals för bearbetning, eftersom den används som en arbetsflödeskö. Som hello antal arbetsflöde fungerar begäranden öka hello behov toobe distribuerade för skalan. Så att viss typ av hello-program som inte skalning, eller så måste tooupdate mer ofta dela detta ut i en mikrotjänster och förnya. 

**Omvandlas till mikrotjänster** -är där programmet är helt består av (eller indelas i) mikrotjänster. du har gjort hello mikrotjänster resa tooreach här. Du kan börja här, men toodo utan en mikrotjänster plattform toohelp du är en betydande investering. 

### <a name="are-microservices-right-for-my-application"></a>Är mikrotjänster för Mina program?
Kanske. Vi stötte var att många av dem insåg hello fördelarna med ett mikrotjänster-liknande sätt som fler och fler team i Microsoft började toobuild hello Molnets för affärsskäl. Bing, till exempel har utvecklat mikrotjänster i Sök efter år. Hej mikrotjänster metoden är ny för andra team. Team hitta att det fanns problem med hårda toosolve utanför de viktiga områdena av styrka. Det är därför Service Fabric erfarenheter drivande som hello teknik för att skapa tjänster.

hello syftet med Service Fabric är tooreduce hello svårigheter för att skapa program med en mikrotjänster, så att du inte har toogo via som många kostsamma redesigns. Börja litet, skala vid behov, inaktualisera tjänster, lägga till nya och utvecklas med kundens användning är hello-metod. Vi vet att det finns många problem ännu toobe löst toomake mikrotjänster mer användarvänligt för de flesta utvecklare. Behållare och hello aktören programmeringsmodell är exempel på små steg i den riktningen och vi vill att flera innovationer kommer framkommer toomake detta enklare.
 
<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Nästa steg
* [Översikt över Service Fabric-terminologi](service-fabric-technical-overview.md)
* [Mikrotjänster: Ett program-revolutionen har drivs av hello moln](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
