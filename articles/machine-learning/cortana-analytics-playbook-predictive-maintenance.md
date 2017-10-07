---
title: "aaaPredictive Underhåll investerar med Azure - Cortana Intelligence lösningsmall | Microsoft Docs"
description: "En Lösningsmall med Microsoft Cortana Intelligence för förebyggande underhåll i aerospace, verktyg och transport."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2e8b66db-91eb-432b-b305-6abccca25620
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: da863a89d2409a8b56b23066f15b4da13e1cd3d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Cortana Intelligence lösning mallen Playbook för förebyggande underhåll i aerospace och andra företag
## <a name="executive-summary"></a>Sammanfattning
Förutsägande Underhåll är en av hello mest begärd program i förutsägelseanalyser unarguable fördelar, inklusive enorm mängden besparingar. Den här playbook syftar till att tillhandahålla en referens för förutsägande Underhåll lösningar med hello betoning på viktiga användningsområden.
Det förberedda toogive hello reader förståelsen av hello vanligaste affärsscenarier av förutsägande underhåll, utmaningar över kvalificerande affärsproblem för sådana lösningar, nödvändiga data toosolve dessa affärsproblem förutsägelsemodellering tekniker toobuild lösningar med exempel lösningsarkitekturer sådana data och bästa praxis.
Här beskrivs också hello närmare information modellen utvecklings- och utvärdering av hello förutsägelsemodeller utvecklats, till exempel funktionen tekniker,. I princip samlar denna playbook hello företag och analytiska riktlinjer som behövs för en lyckad utveckling och distribution av förutsägande Underhåll lösningar. Dessa riktlinjer är förberedd toohelp hello målgruppen skapa en första lösning med hjälp av Cortana Intelligence Suite och specifikt Azure Machine Learning som en startpunkt pekar i sin långsiktig strategi för förutsägande underhåll. hello dokumentation om Cortana Intelligence Suite och Azure Machine Learning finns i [Cortana Analytics](http://www.microsoft.com/server-cloud/cortana-analytics-suite/overview.aspx) och [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) sidor.

> [!TIP]
> En teknisk guide tooimplementing lösning mallen, finns [tekniska guide toohello Cortana Intelligence Lösningsmall för förebyggande underhåll](cortana-analytics-technical-guide-predictive-maintenance.md).
> toodownload ett diagram som ger en översikt över arkitekturen för den här mallen finns [arkitektur för hello Cortana Intelligence Lösningsmall för förebyggande underhåll](cortana-analytics-architecture-predictive-maintenance.md).
> 
> 

## <a name="playbook-overview-and-target-audience"></a>Målgruppen för Playbook översikt och mål
Den här playbook är ordnad toobenefit både teknisk och icke-teknisk målgrupp med olika bakgrund och intressen i förutsägande Underhåll utrymme. Hej playbook täcker båda högnivåaspekterna av hello olika typer av förutsägande Underhåll lösningar och information om hur tooimplement dem. hello innehåll balanseras för att tillgodose båda toohello målgrupp som bara är intresserad av att förstå lösning utrymme och hello typ av program som de som är ute efter tooimplement lösningarna och därför är intresserad av teknisk information.

Merparten av hello innehållet i den här playbook ta inte tidigare datavetenskap vetskap eller kunskaper. Men kräver vissa delar av hello playbook något förtrogenhet med datavetenskap begrepp toobe följa implementeringsdetaljer. Inledande nivå datavetenskap kunskaper är krävs toofully omfattas av hello material i dessa avsnitt.

hello första hälften av hello playbook innehåller en introduktion toopredictive Underhåll program, hur tooqualify förutsägande underhållslösningen, en samling av vanliga Använd fall hello uppgifter om problem i verksamheten hello data runt dessa användning fall och hello företagets fördelar med att implementera lösningarna förutsägande underhåll. Dessa avsnitt behöver inte ha någon teknisk kunskap i hello förutsägelseanalyser domän.

I hello andra halvan av hello playbook upp vi hello typer av tekniker för förutsägelsemodellering för förutsägande Underhåll program och hur du implementerar dessa modeller med exemplen från hello användningsområden som beskrivs i hello första hälften av hello playbook. Detta illustreras med att gå igenom stegen för dataförbearbetning av som data etiketter och funktionen teknik, modell markeringen, utbildning/testning och prestanda utvärdering bästa praxis. Dessa avsnitt är lämpliga för avancerade användare.

## <a name="predictive-maintenance-in-iot"></a>Förutsägande Underhåll i IoT
hello effekten av utrustning oplanerade driftstopp kan vara mycket destruktiva för företag. Det är kritiska tookeep fältet utrustning som körs i ordning toomaximize användning och prestanda och minimerar kostsamma, oplanerade driftstopp. Väntar på hello fel toooccur är helt enkelt inte prisvärt i dagens företag operations scen. tooremain konkurrenskraftiga, företag leta efter nya sätt toomaximize tillgången prestanda genom att göra användningen av hello data som samlas in från olika kanaler. Ett bra sätt tooanalyze sådan information är tooutilize förutsägande analytiska metoder som använder historiska mönster toopredict framtida resultat. En av hello populäraste för dessa lösningar kallas förutsägande underhåll som vanligtvis definieras som men inte begränsat toopredicting risken för fel i en tillgång i hello nära framtiden så att hello tillgångar kan vara övervakade tooproactively identifiera fel och vidta åtgärder innan hello fel uppstår. Dessa lösningar identifiera felet mönster toodetermine tillgångar på hello största risken för fel. Den här tidig identifiering av problem kan distribuera begränsad Underhåll resurser på ett mer kostnadseffektivt sätt och förbättra kvaliteten och ange kedjan processer.

Med hello ökning av hello Sakernas Internet (IoT) program, förutsägande Underhåll har får ökande åtgärder i hello branschen hello datainsamling och bearbetning av tekniker har mognadslagrats tillräckligt med toogenerate överföra, lagra och analysera alla typer av data i batchar eller i realtid. Dessa tekniker aktivera enkel utveckling och distribution av lösningar för slutpunkt till slutpunkt med avancerade Analyslösningar med förutsägande Underhåll lösningar tillhandahåller tvekan hello största fördelen.

Affärsproblem hello förutsägande Underhåll domän mellan hög operativa risker på grund av fel toounexpected och begränsad inblick i hello orsaken till problem i komplexa företagsmiljöer. hello merparten av problemen kan vara kategoriserade toofall under hello följande verksamhetsrelaterade frågor:

* Vad är hello sannolikheten att utrustning misslyckas i hello nära framtiden?
* Vad är hello återstående livslängd hello utrustning?
* Vad är hello orsaker till fel och vilka underhållsåtgärder ska utföras toofix dessa problem?

Genom att använda förutsägande Underhåll tooanswer dessa frågor, företag kan du:

* Minska operativa risker och ökar avkastningen på tillgångar genom att upptäcka fel innan de inträffade
* Minska onödiga tidsbaserade underhållsåtgärder och kontrollera kostnaden för underhåll
* Förbättra övergripande bild av varumärke, eliminera felaktiga reklam och resulterande förlorade försäljning från kunden partikelhållfasthet.
* Lägre kostnader för inventering genom att minska inventering nivåer genom att förutsäga beställningspunkten
* Identifiera mönster anslutna toovarious underhållsproblem

Förutsägande Underhåll lösningar kan ge företag med KPI: er som hälsa poäng toomonitor realtid tillgången villkor, en uppskattning av hello återstående livslängd av tillgångar, rekommendation för förebyggande underhållsaktiviteter och Beräknat datum för ersättning av delar.

## <a name="qualification-criteria-for-predictive-maintenance"></a>Kriterier för förebyggande underhåll
Det är viktigt tooemphasize att inte alla användningsfall eller affärsproblem effektivt kan lösas genom förutsägande underhåll. Viktiga kriterier inkluderar om hello problemet är förutsägbar karaktär som en tydlig sökväg för åtgärden finns i ordning tooprevent fel när de identifieras i förväg och viktigast av allt data med tillräckligt kvalitet toosupport hello användningsfall är tillgänglig. Här kan fokusera vi på hello kraven för att skapa en lyckad förutsägande Underhåll lösning.

När du skapar förutsägelsemodeller använder vi historiska data tootrain hello modell som kan identifiera dolda mönster och ytterligare identifiera dessa mönster i hello framtida data. Dessa modeller tränas med exempel som beskrivs av deras egenskaper och hello målet för förutsägelse. hello tränad modell är förväntade toomake förutsägelser på hello målet genom att bara titta på hello funktioner i hello nya exempel. Det är viktigt att hello modellen avbilda hello förhållandet mellan funktioner och hello målet för förutsägelse. Tootrain en effektiv maskininlärning modellen, måste träningsdata som innehåller funktioner som faktiskt har förutsägande ström mot hello målet för förutsägelse betydelse hello data bör vara relevanta toohello förutsägelse målet tooexpect korrekt förutsägelser.

Om hello mål är toopredict fel i train hjul, bör hello utbildning data innehåller hjul-relaterade funktioner (t.ex. telemetri som avspeglar hello hälsostatus hjul, hello hittills utfört, car belastning, etc.). Men om hello mål är toopredict train motorn fel, behöver vi förmodligen en ny uppsättning utbildningsdata som har motorn-relaterade funktioner. Innan du skapar förutsägelsemodeller vi räknar hello expert toounderstand hello data relevans affärsbehov och ange hello kunskap som är nödvändiga tooselect relevanta delmängder av data för hello analys.

Det finns tre grundläggande datakällor vi letar efter när kvalificera ett företag problemet toobe lämpar sig för en lösning med förutsägande Underhåll:

1. Fel historik: I förutsägande Underhåll program felhändelser normalt sällsynta. När du skapar förutsägelsemodeller som förutsäga fel, måste dock hello algoritmen toolearn hello normal drift mönster samt hello fel mönster via hello utbildning process. Därför är det viktigt att hello träningsdata innehåller tillräckligt många exemplen i båda kategorier i ordning toolearn dessa två olika mönster. Därför kräver att data har tillräckligt många misslyckade händelser. Felhändelser finns i Underhåll och delar ersättning historik eller avvikelser i hello utbildning data kan också användas som kodfel som identifieras av hello domän experter.
2. Underhåll och reparera historik: En grundläggande datakälla för förutsägande Underhåll lösningar är Hej detaljerad Underhållshistorik över hello tillgång som innehåller information om hello komponenter ersättas, förebyggande underhåll aktiverar utförs, etc. Det är mycket viktigt toocapture händelserna som dessa påverkar hello försämring mönster och frånvaro av denna information gör vilseledande resultat.
3. Datorn villkor: I ordning toopredict hur många dagar (timmar, miles, transaktioner osv.) en dator varar innan det misslyckas antar vi hello datorns hälsostatus försämras med tiden under dess drift. Vi förvänta dig därför hello toocontain tid olika funktioner som samlar in det här mönstret ålder och eventuella avvikelser som leder toodegradation. Hej telemetridata från olika sensorer representerar ett bra exempel i IoT-appar. I ordning toopredict om en dator ska toofail inom ett tidsintervall bör helst hello datainsamling förnedrande trend under denna tidsperiod innan hello faktiska felhändelse.

Vi behöver dessutom data som är direkt relaterade toohello driftsförhållanden hello mål tillgång för förutsägelse. hello beslutet mål baserat på tillgänglighet av både företag behov och data. Ta hello träna hjul felförutsägelse exempelvis vi kan förutsäga ”om hello hjul är pågående toohave fel” eller ”hello hela tåget kommer har ett fel”. hello först en riktar sig till en mer specifik komponent medan den andra som riktar sig mot fel på hello tåget. hello är andra en mer allmän fråga som kräver mycket mer spridda dataelement än hello förstnämnda, vilket gör det svårare toobuild en modell. Däremot kanske försök toopredict hjul fel genom att titta hello övergripande train villkorsdata inte möjligt eftersom den inte innehåller information på komponentnivå hello. I allmänhet är det mer sensible toopredict specifika felhändelser än mer allmän som.

En vanlig fråga som vanligtvis tillfrågas om fel historikdata är ”hur många misslyckade händelser är obligatoriska tootrain en modell och hur många betraktas som” det finns tillräckligt med ”? Det finns inga Rensa fråga toothat som många förutsägelseanalyser, är det vanligtvis hello kvaliteten hello data som bestämmer vad som är acceptabel. Om hello dataset inte innehåller funktioner som är relevanta toofailure förutsägelse sedan kanske även om det finns många felhändelser bygger en bra modell inte möjligt. Dock är hello tumregel att hello mer hello fel händelser hello bättre hello modellen finns och en grov uppskattning av hur många fel exempel krävs är en mycket kontext och data beroende mått. Det här problemet beskrivs i hello avsnittet för att hantera imbalanced datauppsättningar där vi föreslår metoder toocope med hello problem med att inte ha tillräckligt med fel.

## <a name="sample-use-cases"></a>Exempel användningsområden
Det här avsnittet fokuserar på en samling med förutsägande Underhåll användningsområden från flera branscher som Aerospace, verktyg och transport. Varje avsnitt flyttar till hello användningsfall samlas in från dessa områden och beskriver affärsproblem, hello data omgivande hello affärsproblem och hello fördelarna med en lösning för förutsägande underhåll.

### <a name="aerospace"></a>Aerospace
#### <a name="use-case-1-flight-delay-and-cancellations"></a>Användningsfall 1: Svarta fördröjning och avbryta
##### <a name="business-problem-and-data-sources"></a>*Företag problem och datakällor*
En av hello större affärsproblem att flygbolagen yta är hello betydande kostnader som är associerade med flygplan förskjuts på grund av problem med toomechanical. Om hello mekaniskt fel inte kan repareras avbrytas även flygplan. Detta är mycket kostsam fördröjningar skapa problem i schemaläggning och åtgärder, orsaker felaktiga rykte och kunden klagomål tillsammans med andra problem. Flygbolag är särskilt intresserade förutsäga sådana mekaniskt fel i förväg så att de kan minska svarta fördröjningar eller avbryta. hello målet hello förutsägande Underhåll lösning i dessa fall är toopredict hello sannolikheten att ett flygplan fördröjd eller avbrutits, baserat på relevanta datakällor, till exempel information om Underhåll historik och svarta flöde. hello två viktiga datakällor för den här användningsfall är hello svarta ben och sidan loggar. Svarta ben data innehåller information om hello flödet flyginformation, till exempel hello datum och tid för utgångspunkten och ankomst, utgångspunkten och ankomst flygplatser och så vidare. Sidan loggdata innehåller ett antal fel och underhåll koder som registreras av hello underhållspersonal.

##### <a name="business-value-of-hello-predictive-model"></a>*Värdet av hello förutsägelsemodell*
Använder hello tillgängliga historiska data, har en förutsägelsemodell skapats med en flera klassificeringsalgoritm toopredict hello typ av mekaniskt problemet, vilket resulterar i en fördröjning eller annullering av en svarta inom hello nästkommande 24 timmar. Genom att göra den här förutsägelse nödvändiga underhållsåtgärder kan tas toomitigate hello risk medan ett flygplan genomgår underhåll och därmed förhindra eventuella fördröjningar eller avbryta. Med hjälp av Azure Machine Learning-webbtjänsten, kan hello förutsägelsemodeller sömlöst och enkelt integreras i flygbolagen befintliga operativsystem. 

#### <a name="use-case-2-aircraft-component-failure"></a>Användningsfall 2: Flygplan komponentfel
##### <a name="business-problem-and-data-sources"></a>*Företag problem och datakällor*
Flygplan motorer är mycket känslig och dyra delar av utrustning och motorn del ersättningar är bland hello mest vanliga underhållsuppgifter i hello flygbolag bransch. Underhåll lösningar för flygbolag kräver noggrann hantering av lager komponenttillgänglighet, leverans och planera. Som kan toogather intelligence på komponenten tillförlitlighet leder toosubstantial minskning på investeringar. hello större datakällan för den här användningsfall är telemetridata som samlats in från ett antal sensorer i hello flygplan med information på hello villkor av hello flygplan. Underhåll poster har också använda tooidentify när fel uppstod och ersättningar har gjorts.

##### <a name="business-value-of-hello-predictive-model"></a>Värdet av hello förutsägelsemodell
En modell för flera klassen klassificering skapades som beräknar sannolikheten att ett fel på grund av tooa vissa komponent i hello nästa månad. Genom att använda dessa lösningar kan flygbolagen minska kostnaderna för reparation av komponenten, förbättra lager komponenttillgänglighet, sänka inventering av relaterade resurser och förbättra Underhåll planering.

### <a name="utilities"></a>Hjälpmedel
#### <a name="use-case-1-atm-cash-dispense-failure"></a>Användningsfall 1: Avstå ATM kontanter fel
##### <a name="business-problem-and-data-sources"></a>*Företag problem och datakällor*
Chefer i tillgångsinformation beräkningsintensiva branscher ofta tillstånd att primära operativa risker tootheir företag är oväntade fel av deras tillgångar. Exempelvis är fel på maskiner som bl.a i bankwebbplatser branschen ett mycket vanligt problem som uppstår ofta. Sådana problem Se förutsägande Underhåll lösningar användbar för operatörer av maskinerna. I den här användningsfall är förutsägelse problemet toocalculate hello sannolikheten att en ATM kontanter återkallelse transaktion hämtar avbryts på grund av tooa i hello kontanter utgivaren, till exempel pappersstopp eller en del. Viktiga datakällor för det här fallet är sensoravläsningar som samlar in mätningar medan anteckningar pengar fördelas och underhåll poster insamlade över tid. Sensordata med sensoravläsningar per varje transaktionen har slutförts och sensoravläsningar per varje anteckning vara. hello sensor avläsningar tillhandahålls mätningar som glapp mellan anteckningar, tjocklek Observera ankomst avståndet osv. Underhåll data med felkoder och reparationsinformation. Dessa har använt tooidentify fel fall.

##### <a name="business-value-of-hello-predictive-model"></a>*Värdet av hello förutsägelsemodell*
Två förutsägelsemodeller har inbyggda toopredict fel i hello kontanter återkallelse transaktioner och fel i hello enskilda anteckningar vara under en transaktion. Genom kan toopredict transaktion fel i förväg, bl.a kan vara underhålls proaktivt tooprevent fel uppstår. Dessutom med Obs felförutsägelse om en transaktion sannolikt toofail innan den är klar på grund av tooa Obs överflödiga fel, det kan vara bästa toostop hello processen och varna hello kunden för ofullständiga transaktion i stället för väntar hello Underhåll tooarrive när hello fel som kan ge toolarger kunden klagomål.

#### <a name="use-case-2-wind-turbine-failures"></a>Användningsfall 2: Vind turbinen fel
##### <a name="business-problem-and-data-sources"></a>*Företag problem och datakällor*
Med hello rera av medvetenheten, vind turbiner har blivit hello större källor av energiförbrukning och de kostnad vanligtvis miljontals kronor. En av hello viktiga komponenter av vind turbiner är hello generator motor som är försedd med många sensorer som hjälper toomonitor turbinen tillstånd och status. Hej sensoravläsningar innehåller värdefull information som kan vara används toobuild en förutsägelsemodell toopredict kritiska nyckel (nyckeltal), till exempel tiden toofailure för komponenter i hello vind turbinen. Data för den här användningsfall kommer från flera vind turbiner som finns i tre olika platser. Mått från Stäng tooa hundra sensorer från varje turbinen registrerades var 10: e sekund för ett år. Dessa värden inkluderar mått som temperatur, generator hastighet, turbinen power och generator upplösning.

##### <a name="business-value-of-hello-predictive-model"></a>*Värdet av hello förutsägelsemodell*
Förutsägelsemodeller har skapats tooestimate återstående livslängd för generatorer och temperatursensorer. Genom att förutsäga hello sannolikheten för fel, Underhåll tekniker kan fokusera på misstänkt turbiner som är sannolikt toofail snart toocomplement tidsbaserade Underhåll systemen.
Dessutom kan sätta förutsägelsemodeller insight toohello andelen bidrag för olika faktorer toohello sannolikheten för fel som hjälper företag toohave en bättre förståelse för hello rot orsaka problem.

#### <a name="use-case-3-circuit-breaker-failures"></a>Användningsfall 3: strömbrytare fel
##### <a name="business-problem-and-data-sources"></a>*Företag problem och datakällor*
El- och åtgärder som innefattar produktion, distribution eller försäljning av el kräver mycket underhåll för att säkerställa power rader fungerar på alla gånger tooguarantee leverans av energi toohouseholds. Fel i dessa åtgärder är viktig när nästan alla enheter sker med power problem i hello områden där de förekommer. Utlösts är kritiska för dessa åtgärder som de är en typ av utrustning som Klipp ut elektrisk ström vid problem och kort kretsar tooprevent eventuella skador toopower rader inträffar. Problem i verksamheten för den här användningsfall är toopredict strömbrytare fel Underhåll loggar, kommandohistoriken och tekniska specifikationer.

Tre viktiga datakällor för det här fallet är underhåll av loggar med korrigerande, förebyggande och systematiskt åtgärder, användningsdata som innehåller kommandon för automatisk och manuell skicka toocircuit utlösts som öppna och Stäng åtgärder och technical specifikationen data om egenskaperna för varje strömbrytare, till exempel år som gjorts, plats, modell, osv.

##### <a name="business-value-of-hello-predictive-model"></a>*Värdet av hello förutsägelsemodell*
Förutsägande Underhåll lösningar att minska kostnaderna för reparation och öka hello livscykeln för utrustning som utlösts. Dessa modeller också förbättra hello kvaliteten på hello power nätverk eftersom modeller ger varningar i förväg som minskar oväntade fel som leda toofewer avbrott toohello service.

#### <a name="use-case-4-elevator-door-failures"></a>Användningsfall 4: Snabba dörren fel
##### <a name="business-problem-and-data-sources"></a>*Företag problem och datakällor*
De flesta stora snabba företag har vanligtvis miljontals hissar kör hello världen. toogain en konkurrensfördel de fokuserar på tillförlitlighet som är vad gäller de flesta tootheir kunder. Ritning på hello potential hello Internet of Things genom att ansluta sina hissar toohello moln och samla in data från snabba sensorer och system, är de kan tootransform data till värdefull affärsinformation vilket frågeprestanda avsevärt förbättrar aktiviteter med erbjuder förutsägbara och föregripande underhåll som inte är något som är tillgängliga toohello konkurrenter ännu. hello affärsbehoven för det här fallet är tooprovide ett kunskapsbas förutsägande program som beräknar hello möjliga orsaker till dörren fel. hello krävs data för den här implementeringen består av tre delar som är snabba statiska funktioner (t.ex. identifierare, minimera frekvens för underhåll, skapande av typen, etc.), information om användning (t.ex. antalet dörren cykler, genomsnittlig dörren Stäng tid, etc.) och fel historik (d.v.s. historiska fel poster och deras orsaker).

En multiklass-baserad logistic regressionsmodell har skapats med Azure Machine Learning toosolve hello förutsägelse problem, med hello integrerad statiska funktioner och användningsdata som funktioner och hello orsakerna till historiska fel poster som klass etiketter. Den här förutsägelsemodell används av en app på en mobil enhet som används av fältet tekniker toohelp standardnätverksprotokoll fungerar. När en tekniker som finns på webbplatsen toorepair en snabba, kan han eller hon referera toothis app för rekommenderade orsaker och bästa kurser Underhåll åtgärder toofix hello snabba dörrar så snabbt som möjligt.

### <a name="transportation-and-logistics"></a>Transport och logistik
#### <a name="use-case-1-brake-disc-failures"></a>Användningsfall 1: Bromsar skiva fel
##### <a name="business-problem-and-data-sources"></a>*Företag problem och datakällor*
Vanliga underhållsprinciper för fordon omfattar korrigerande och förebyggande underhåll. Åtgärda Underhåll innebär att hello vehicle har reparerats när ett fel har uppstått som kan orsaka en allvarlig besvär toohello drivrutin på grund av ett oväntat fel och hello tid som gått förlorat på ett besök toomechanic. De flesta fordon är också ämne tooa förebyggande underhåll principen, som kräver att utföra vissa kontroller på ett schema som inte tar hänsyn till kontot hello skick faktiskt hello bil undersystem. Ingen av dessa metoder är att helt eliminera problem. hello specifika användningsfall här är bromsar skiva felförutsägelse baserat på data som samlas in via sensorer som installerats i hello däck system på en bil som håller reda på historiska intresseväckande mönster och andra villkor som hello bil exponeras för. hello viktigaste datakällan för det här fallet är hello sensordata som till exempel mäter acceleration, bromsar mönster, körning avstånd, hastighet, osv. Den här informationen, tillsammans med andra statisk information, till exempel bil funktioner hjälp bygga en bra uppsättning predictors som kan användas i en förutsägelsemodell. En annan viktig information är hello fel data som härledas från en del ordning databasen (används tookeep hello extra ordning datum och kvantitet som bilar servas i hello hos återförsäljarna).

##### <a name="business-value-of-hello-predictive-model"></a>*Värdet av hello förutsägelsemodell*
hello affärsvärde för en förutsägbar tillvägagångssättet är stor. Ett system med förutsägande Underhåll kan schemalägga en besök toohello återförsäljare baserat på en förutsägelsemodell. hello modell kan baseras på sensoriska information som som representerar hello aktuella tillstånd hello car och hello körning historik. Den här metoden kan minimera hello risken för oväntade fel som kan även uppstå innan hello nästa regelbundet underhåll.
Det kan också minska hello onödiga förebyggande underhåll. Drivrutinen kan proaktivt informeras om att en ändring av delar kan vara nödvändigt i några veckor och ange hello återförsäljare med denna information. hello återförsäljare kunde sedan förbereda ett enskilda Underhåll paket för hello drivrutinen i förväg.

#### <a name="use-case-2-subway-train-door-failures"></a>Användningsfall 2: Språng train dörren fel
##### <a name="business-problem-and-data-sources"></a>*Företag problem och datakällor*
En av hello större grund av fördröjningar och problem på språng operations är dörren fel train bilar. Förutsäga om en bil tåget kan ha ett dörren fel eller som kan tooforecast hello antal dagar till hello nästa door fel är mycket viktigt förutseende. Den ger hello möjlighet toooptimize train dörren underhåll och minska hello train stillestånd.

#### <a name="data-sources"></a>Datakällor
Tre datakällorna i den här användningsfall är 

* **Träna händelsedata**, vilket är hello tidigare poster train-händelser 
* **Underhåll data** , till exempel underhållstyper, arbete ordning typer och prioritetskoder  
* **registrerar fel**.

##### <a name="business-value-of-hello-predictive-model"></a>*Värdet av hello förutsägelsemodell*
Två modeller har skapats toopredict nästa dag fel sannolikhet med hjälp av binär klassificering och dagar till felet med hjälp av regression. Liknar hello tidigare fall hello modeller skapa fantastiska möjligheter tooimprove Tjänstkvalitet och öka nöjda komplement hello regelbundet underhåll systemen.

## <a name="data-preparation"></a>Förberedelse av data
### <a name="data-sources"></a>Datakällor
hello gemensamma uppgifter för problem med förutsägande Underhåll kan sammanfattas på följande sätt:

* Fel historik: hello fel historiken för en dator eller en komponent i hello-datorn.
* Underhållshistorik: hello reparera historiken för en dator, t.ex. felkoder, tidigare underhållsaktiviteter eller komponenten ersättningar.
* Villkor och användning: hello driftsförhållanden för en dator som t.ex. data som samlas in från sensorer.
* Datorn funktioner: hello funktioner för en dator, t.ex. motorn storlek, märke och modell, plats.
* Operatorn funktioner: hello funktioner hello-operator, t.ex. kön, erfarenheter.

Det är möjligt och vanligtvis hello fall att fel Historik finns i Underhållshistorik som i hello form av särskilda felkoder eller datum för ledig delar. I sådana fall kan fel extraheras från hello Underhåll data. Olika domäner kan dessutom ha en mängd andra datakällor som påverkar fel mönster som inte listas här omfattande. Dessa bör identifieras med hjälp av hello domän experter när du skapar förutsägelsemodeller.

Det är några exempel på ovan dataelement från användningsområden:

Fel historik: bekämpa fördröjning datum, flygplan komponenten fel datum och typer, ATM kontanter återkallelse transaktion fel, train/snabba dörren fel, bromsar disk ersättning datum, vind turbinen fel datum och strömbrytare kommandot fel.

Underhållshistorik: svarta felloggar, ATM för fel transaktionsloggar träna Underhåll poster, inklusive underhållstyp, kort beskrivning etc. och strömbrytare Underhåll poster.

Villkor och användning: svarta dirigerar och tider, sensordata som samlas in från flygplan motorer, sensoravläsningar från ATM-transaktioner, träna händelser data, sensoravläsningar från vind turbiner, hissar och anslutna bilar.

Datorn funktioner: strömbrytare tekniska specifikationer som spänning nivåer, geolokalisering eller bil funktioner, till exempel märke, modell, motorn storleken däck typer produktion och osv.

Ovanstående datakällor hello hello två huvudsakliga datatyper vi observerar i förutsägande underhållsdomän är temporal data och statiska data.
Fel-historik, datorn villkor reparera historik användningshistorik nästan alltid komma med tidsstämplar som visar hello tid för samlingen på varje datablock. Funktioner på datorer och operatorn funktioner är i allmänhet statiska eftersom de vanligtvis beskriver hello tekniska specifikationer av datorer eller aktörens egenskaper. Det är möjligt för dessa funktioner toochange med tiden och om så de ska behandlas som datakällor för tidsstämpel.

### <a name="merging-data-sources"></a>Sammanslagning av datakällor
Innan du hämtar till en typ av funktionen tekniker eller märkning processen måste toofirst förbereda våra data i hello formuläret som krävs för toocreate funktioner från. hello slutmålet är en post för varje gång för varje tillgång med dess funktioner och etiketter toobe matas in hello maskininlärningsalgoritmen toogenerate. I ordning tooprepare som rensar slutliga datauppsättning, ska vissa före bearbetning vidta åtgärder. Första steget är toodivide hello varaktighet för datainsamlingen i tidsenheter där varje post tillhör tooa tidsenhet för en tillgång. Insamling av data kan också delas in i andra enheter, till exempel åtgärder, men för enkelhetens skull vi använda tidsenheter hello resten av hello förklaringar.

hello måttenheten för gången kan vara i sekunder, minuter, timmar, dagar, månader, cykler, miles eller transaktioner beroende på hello effektivitet förberedelse av data och hello ändringar förekommer i hello villkor för hello tillgång från en gång enheten toohello andra eller andra faktorer specifika toohello domän. Hello tidsenhet har med andra ord inte toobe hello samma som hello frekvensen för datainsamling som i många fall inte kan visa skillnader från en enhet toohello andra. Till exempel om temperatur värden som samlades var 10: e sekund, välja en tidsenhet 10 sekunder för hello hela analys blåses upp hello antal exempel utan att ange ytterligare information. Bättre strategi skulle vara toouse medelvärde över en timme som exempel.

Exempel allmänna data scheman för hello möjliga datakällor förklaras i hello tidigare avsnittet är:

Underhåll poster: dessa är hello poster med underhållsåtgärder som utförs. hello rådata Underhåll data kommer vanligtvis med ett Tillgångsnummer och tidsstämpel med information om vilka underhållsaktiviteter har utförts vid den tiden. Vid sådan rådata måste underhållsaktiviteter toobe översättas till kategoriska kolumner med varje kategori motsvarande tooa Underhåll åtgärdstyp. hello grundläggande dataschemat för Underhåll poster skulle innehåller kolumner för tillgångsinformation ID, tid och underhåll åtgärd.

Fel poster: dessa är hello-poster som tillhör toohello mål för förutsägelse som vi kallar fel eller orsaken till felet. Dessa kan vara specifika felkoder eller händelser för fel som definieras av specifika villkor. I vissa fall kan innehåller data flera felkoder vilket motsvarar toofailures av intresse. Fel kan inte målet för förutsägelse så att andra fel är oftast används tooconstruct funktioner som kan ha att göra med fel. hello grundläggande dataschemat för fel poster omfattar Tillgångsnummer, tid och problem eller fel orsak kolumner om orsak finns.

Datorn villkor: dessa är helst realtidsövervakning data om hello driftsförhållanden hello data. Exempelvis för dörren fel dörren öppna och stänga tider är bra indikatorer om hello dörrar aktuella tillstånd. hello grundläggande dataschemat för datorn villkor skulle innehåller kolumner för tillgångsinformation ID, tid och villkoret värde.

Dator-och operator: datorn och operatorn data kan kopplas till ett schema tooidentify som var drivs av vilken operator tillsammans med egenskaperna tillgången och operatorn. Till exempel ägs en bil vanligtvis av en drivrutin med attribut, till exempel ålder, körning upplevelse osv. Om dessa data ändras med tiden kan dessa data bör också innehålla en time-kolumn och ska behandlas som tid olika data för generering av funktionen. hello grundläggande dataschemat för datorn villkor omfattar Tillgångsnummer, tillgångsinformation funktioner, operator-ID och operatorn funktioner.

hello slutliga tabell innan etiketter och funktionen generation kan genereras av vänster koppla datorn villkor tabell med fel poster för tillgångsinformation ID och tidsfälten. Den här tabellen kan sedan kopplas till Underhåll poster på Tillgångsnummer och tid fält och slutligen till datorn och operatorn funktioner på tillgången-ID. hello första vänster koppling lämnar null-värden för fel när datorn är normal drift, dessa imputeras genom en indikatorvärdet för normal drift. Fel kolumnen är används toocreate etiketter för hello förutsägelsemodell.

### <a name="feature-engineering"></a>Funktionstekniker
hello första steget i modellering är funktionen tekniker. hello uppfattning om funktionen generation är tooconceptually beskrivs och abstrakt hälsostatus för en dator vid en given tidpunkt med hjälp av historiska data som samlats in toothat punkt i tiden. I nästa avsnitt om hello ger vi en översikt över hello typ av tekniker som kan användas för förutsägande underhåll och hur hello etiketter är klar för varje teknik. exakt Hello-tekniken som ska användas beror på hello och affärsprogram problem. Hello funktionen engineering metoder som beskrivs nedan kan dock användas som baslinjen för att skapa funktioner. Nedan diskuterar vi fördröjning funktioner som ska konstrueras från datakällor som medföljer tidsstämplar och statiska funktioner som har skapats från statisk datakällor och ge exempel från hello användningsfall.

#### <a name="lag-features"></a>Lag funktioner
Som nämnts tidigare i förutsägande Underhåll finns vanligtvis historiska data med tidsstämplar som visar hello tid för samlingen på varje datablock. Det finns många sätt för att skapa funktioner från hello data som medföljer tidsstämplad data. I det här avsnittet diskuterar vi några av dessa metoder som används för förebyggande underhåll. Vi är inte begränsade av dessa metoder som enbart. Eftersom funktionen tekniker anses toobe en hello mest kreativa områden för förutsägelsemodellering, kan det finnas andra sätt toocreate-funktioner. Här kan ge vi några allmänna metoder.

##### <a name="rolling-aggregates"></a>*Rullande mängder*
För varje post för en tillgång Välj vi ett rullande fönster storlek ”W” vilket är hello antalet enheter som vi vill gärna toocompute historiska mängder för. Vi för att beräkna rullande sammanställd funktioner med hello W punkter före hello datumet för den posten. Vissa exempel rullande mängder kan löpande antal, medel, standardavvikelser, baserat på standardavvikelser avvikare, CUSUMS åtgärder, lägsta och högsta värden för fönster. En annan intressant tekniken är toocapture trend ändringar, toppar och ändras med hjälp av algoritmer som identifierar avvikelser i data med hjälp av algoritmer för avvikelseidentifiering identifiering.

Exempel, finns i bild 1 där vi representerar sensorn värden som registrerats för en tillgång för varje tidsenhet med hello blå linjer och markera hello rullande genomsnittlig funktionen beräkning för W = 3 för hello poster vid t<sub>1</sub> och t<sub>2 </sub> som anges med orange och grön grupperingar respektive.

![Bild 1. Rullande sammanställd funktioner](media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png)

Bild 1. Rullande sammanställd funktioner

Exempel för flygplan komponentfel var sensor värdena från föregående vecka, senaste tre dagarna och sista dagen används toocreate rullande innebär, standardavvikelse och summan funktioner. För ATM-fel, både rådata sensor värden och återställa innebär median, intervall, standardavvikelser användes på samma sätt kan antalet avvikare utöver tre standardavvikelser övre och nedre CUMSUM funktioner.

Antal felkoder från föregående vecka var svarta fördröjning förutsägelse används toocreate funktioner. Antal hello händelser på hello sista dagen räknar av händelser över hello föregående två veckor för tåget dörren fel och variansen för antal händelser för hello tidigare 15 dagar har använt toocreate fördröjning funktioner. Samma räknat användes för underhåll-relaterade händelser.

Dessutom är genom att välja ett W som mycket stora (t.ex. år), det är möjligt toolook på hello hela historiken för en tillgång till exempel räknar alla Underhåll registrerar fel osv. fram till hello tiden hello posten. Den här metoden används för att kunna räkna strömbrytare fel för hello sista tre år. För tåget fel, har också alla underhållshändelser räknas toocreate en funktionen toocapture hello långsiktiga Underhåll effekter.

##### <a name="tumbling-aggregates"></a>*Rullande mängder*
För varje etikett post för en tillgång, vi väljer ett fönster med storleken ”W -<sub>k</sub>” där n är hello tal eller windows storlek ”W” som vi vill toocreate lag-funktioner. ”k” kan registreras som en stort antal toocapture långsiktiga försämring mönster eller en liten nummer toocapture kortsiktig effekter. Vi använder sedan k rullande windows W -<sub>k</sub> , W -<sub>(k-1)</sub>,..., W -<sub>2</sub> , W -<sub>1</sub> toocreate sammanställd funktioner för hello perioder innan hello posten datum och tid (se bild 2). Dessa också rullande windows på hello poster nivå för en enhet som inte har avbildats på bild 2 men hello idé är hello samma som i bild 1 där t<sub>2</sub> också används toodemonstrate hello inför effekt.

![Figur 2. Rullande sammanställd funktioner](media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png)

Figur 2. Rullande sammanställd funktioner

Exempel för vind turbiner, B = 1 och k = 3 månaderna har använt toocreate fördröjning funktioner för varje hello senaste 3 månaderna med övre och nedre avvikare.

#### <a name="static-features"></a>Statiska funktioner
Dessa är tekniska specifikationer av hello utrustning tillverkningsdatum, modellnummer, plats, t.ex. Fördröjning funktioner är främst numeriska karaktär, blir statiska funktioner vanligen kategoriska variabler i hello modeller. Som ett exempel strömbrytare egenskaper, till exempel spänning, aktuella och power specifikationer tillsammans med transformatorn typer användes strömkällor etc.. Hej typ av däck hjul sådana som om de är legering eller stål användes med hello statiska funktioner för bromsar skiva fel.

Andra viktiga steg som hanterar saknade värden och normalisering ska utföras under genereringen av funktionen. Det finns flera metoder för saknade värdet uppräkning och databasnormalisering som inte beskrivs här. Det är dock bra tootry olika metoder toosee om en prestandaökning förutsägelse är möjligt.

hello sista funktionstabellen när funktionen engineering stegen som beskrivs i hello tidigare avsnitt bör likna hello efter exempel dataschemat när enheten är en dag:

| Tillgångsnummer | Tid | Funktionen kolumner | Etikett |
| --- | --- | --- | --- |
| 1 |Dag 1 | | |
| 1 |Dag 2 | | |
| ... |... | | |
| 2 |Dag 1 | | |
| 2 |Dag 2 | | |
| ... |... | | |

## <a name="modeling-techniques"></a>Tekniker för förutsägelsemodellering
Förutsägande Underhåll är en mycket omfattande domän ofta använda verksamhetsrelaterade frågor som kan vara närmat sig från många olika vinklar av hello förutsägande modeling perspektiv. I nästa avsnitt hello får huvudsakliga metoder att använda toomodel olika frågor som kan lösas med förutsägande Underhåll lösningar. Även om det finns likheter, har varje modell egna sätt att konstruera etiketter som beskrivs i detalj. Du kan se toohello förutsägande Underhåll mall som ingår i hello exempelexperiment i Azure Machine Learning som en tillhörande resurs. hello länkar toohello online material för den här mallen finns i avsnittet för hello-resurser. Du kan se hur vissa hello funktionen engineering tekniker som beskrivs ovan och hello modeling teknik som beskrivs i nästa avsnitt av hello tillämpas toopredict flygplan motorn fel med hjälp av Azure Machine Learning.

### <a name="binary-classification-for-predictive-maintenance"></a>Binär klassificering för förebyggande underhåll
Binär klassificering för förebyggande underhåll är används toopredict hello sannolikheten att utrustning misslyckas i en framtida tidsperiod. hello tidsperiod bestäms av och baserat på regler och till hands hello-data. Några vanliga tidsperioder är minsta lead tid som krävs för toopurchase delar tooreplace sannolikt toodamage komponenter eller tid krävs toodeploy Underhåll resurser tooperform Underhåll rutiner toofix hello problem som är sannolikt toooccur inom som tidsperiod. Vi kallar framtida horizon perioden ”X”.

I ordning toouse binär klassificering behöver vi tooidentify två typer av exemplen som vi kallar positiva och negativa. Varje exempel är en post som tillhör tooa tidsenhet för en tillgång begreppsmässigt beskriva och abstrahera dess driftsförhållanden in toothat tidsenhet via funktionen tekniker med historisk och andra datakällor som beskrivs ovan. Hello gäller binär klassificering för förebyggande underhåll är positivt typen anger fel (1-etiketten) och negativa typen anger normal drift (etikett = 0) där etiketter är av typen kategoriska. hello målet är toofind en modell som identifierar varje ny exempel som sannolikt toofail eller fungera normalt inom hello bredvid X tidsenheter.

#### <a name="label-construction"></a>Etikett konstruktion
Fråga i ordning toocreate en förutsägelsemodell tooanswer hello ”vad är hello sannolikheten att hello tillgången misslyckas i hello bredvid X tidsenheter”?, etikettering görs genom att ta X poster tidigare toohello fel på en tillgång och ge dem etiketter som ”om toofail” (etikett = 1) När alla poster som ”normala” etiketter (etikett = 0). I den här metoden är etiketter kategoriska variabler (se bild 3).

![Bild 3. Etiketter för binär klassificering](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png)

Bild 3. Etiketter för binär klassificering

För svarta fördröjningar och avbryta hämtas X som en dag toopredict fördröjningar i hello nästkommande 24 timmar. Alla flygplan inom 24 timmar innan fel har betecknats som 1s. För ATM kontanter överflödiga fel, två binär klassificering modeller har skapats toopredict hello fel sannolikheten för en transaktion i hello 10 minuter och även toopredict hello sannolikheten för fel i hello nästa 100 anteckningar vara. Alla transaktioner som har inträffat i hello sista 10 minuter för hello felet är märkta som 1 för hello första modellen. Och alla anteckningar vara inom hello senaste 100 anteckningar för ett fel har betecknats som 1 för andra hello-modell. För strömbrytare fel är hello uppgift toopredict hello sannolikheten att hello nästa strömbrytare kommando misslyckas då X väljs toobe en framtida kommando. För tåget dörren fel byggdes hello binär klassificering modellen toopredict fel i hello kommande sju dagar. För vind turbinen fel valdes X som tre månader.

Vind turbinen och train dörren fall används också för regression analysis toopredict återstående livslängd med hello samma data, men genom att använda en annan märkning strategi som beskrivs i nästa avsnitt om hello.

### <a name="regression-for-predictive-maintenance"></a>Regression för förebyggande underhåll
Regression modeller i förutsägande Underhåll är används toocompute återstående livslängd (RUL) för en tillgång som har definierats som hello tidsperiod som hello tillgången fungerar innan nästa hello-fel inträffar. Samma som binär klassificering, varje exempel är en post som tillhör tooa tidsenhet ”Y” för en tillgång. I hello regression är dock hello målet toofind en modell som beräknar hello återstående livslängd varje ny exempel som en kontinuerlig tal som är hello tid kvar innan hello misslyckades. Vi kallar denna tidsperiod vissa multipel av Y. Varje exempel har även en återstående livslängd som kan beräknas genom att mäta hello mängden återstående tid för det exemplet innan nästa hello-fel.

#### <a name="label-construction"></a>Etikett konstruktion
Angivna hello frågan ”vad är hello återstående livslängd hello utrustning?
”, märker för hello regressionsmodell kan konstrueras genom att använda varje post tidigare toohello fel och ge dem etiketter genom att beräkna hur många enheter tid som återstår innan nästa hello-fel. I den här metoden är etiketter kontinuerlig variabler (se figur 4).

![Bild 4. Etiketter för regression](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png)

Bild 4. Etiketter för regression

Skiljer sig binär klassificering för regression, tillgångar utan några fel i hello data kan inte användas för modellering som etiketter görs i referenspunkten tooa fel och beräkningen är inte möjligt utan att känna till hur länge hello tillgången fortfarande varit i livet innan ett fel uppstod. Det här problemet är bäst med ett annat statistiska teknik som kallas överlevnad analys.
Vi inte toodiscuss överlevnad analys i den här playbook på grund av hello potentiella problem som kan uppstå när du använder hello tekniken toopredictive Underhåll användningsfall som avser tiden varierande data med återkommande intervall.

### <a name="multi-class-classification-for-predictive-maintenance"></a>Flera klassen klassificering för förebyggande underhåll
Flera klassen klassificering för förebyggande underhåll kan användas för att förutsäga två framtida resultat. hello först är en tooassign en tillgång tooone av hello flera möjliga perioder med tiden toogive en mängd tid toofailure för varje resurs. hello andra är tooidentify hello risken för fel i en framtida period förfallodatum tooone av hello flera rotorsaker. Som gör det möjligt för underhållspersonal som är utrustade med denna kunskap kan hantera hello problem i förväg. En annan metod för flera klassen modellering fokuserar på att fastställa hello mest sannolika orsaken till ett givet ett fel. Detta gör att rekommendationer toobe för hello översta Underhåll åtgärder toobe vidtagits i ordning toofix ett fel.
Genom att låta en rankningslista över rot orsakar och tillhörande repareringsåtgärder  
tekniker kan vara mer effektivt att deras första reparationsåtgärder efter fel.

#### <a name="label-construction"></a>Etikett konstruktion
Angivna hello två frågor som ”vad är hello sannolikheten att en tillgång misslyckas i hello nästa” aZ ”tidsenheter där” a ”är hello antalet perioder” och ”vad är hello sannolikheten att hello tillgången misslyckas i hello bredvid X tidsenheter förfaller tooproblem” P<sub>jag </sub>”där” i ”hello antalet möjliga underliggande orsaker, etiketter är görs på följande sätt för dessa tootechniques hello.

För hello första fråga etiketter görs genom att använda aZ poster före hello felet av en tillgång och ge dem etiketter med buckets tid (3Z, 2Z Z) som etiketter när etiketter alla poster som ”normala” (etikett = 0). I den här metoden är etikett kategoriska variabeln (se bild 5).

![Bild 5. Etiketter för multiklass-baserad klassificering för fel tid förutsägelse](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png)

Bild 5. Etiketter för multiklass-baserad klassificering för fel tid förutsägelse

För andra fråga hello etiketter görs genom att ta X poster före hello felet av en tillgång och etiketter som ”om toofail på grund av problem P<sub>jag</sub>” (etikett = P<sub>jag</sub>) när etiketter alla poster som ” Normal ”(etikett = 0). I den här metoden är etiketter kategoriska variabler (se bild 6).

![Bild 6. Etiketter för multiklass-baserad klassificering för rot-orsaken förutsägelse](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png)

Bild 6. Etiketter för multiklass-baserad klassificering för rot-orsaken förutsägelse

hello modellen tilldelar sannolikheten för fel på grund av tooeach P<sub>jag</sub> samt hello sannolikheten för fel. Dessa sannolikhet kan beställas av omfattning tooallow förutsägelser av hello problem som är mest sannolika toooccur i hello framtida. Användningsfall för flygplan komponenten fel struktur som multiklass-baserad klassificeringsproblem. Detta gör att hello förutsägelser av hello troliga misslyckades på grund av tootwo olika trycket ventilkomponenter inträffar inom hello nästa månad.

För att rekommendera underhållsåtgärder efter fel, kräver etikettering inte en framtida horizon toobe plockats. Detta beror på att hello modellen inte är att förutsäga fel i hello framtida men det är bara att förutsäga hello mest sannolika orsaken när hello fel redan har skett. Snabba dörren fel som hör till hello tredje fall är målet för hello toopredict felorsaken med hello angivna historiska data om driftsförhållanden. Den här modellen används sedan toopredict hello troligen rotorsaker när ett fel har uppstått. En viktig fördel med den här modellen är van vid tekniker tooeasily diagnostisera och lösa problem som annars skulle behöva års erfarenhet underlättar.

## <a name="training-validation-and-testing-methods-in-predictive-maintenance"></a>Utbildning, verifiering och tester metoder i förutsägande Underhåll
I förutsägande underhåll, liknande tooany andra Lösningsutrymme som innehåller tidsstämplad data, hello vanliga träning och testning rutinunderhåll behov tootake konto hello tid olika aspekter toobetter generalisera på överblivna framtida data.

### <a name="cross-validation"></a>Mellan verifiering
Många maskininlärningsalgoritmer beror på ett antal justeringsmodeller som kan ändra modellen prestanda avsevärt. hello optimala värdena för dessa justeringsmodeller inte beräknas automatiskt när hello modell, men anges av data forskare. Det finns flera sätt att hitta bra värdena för justeringsmodeller. hello vanligaste en är ”k vikning korsvalidering” som delar upp hello exempel slumpmässigt i vikningar som ”k”. För varje uppsättning justeringsmodeller värden är Inlärningsalgoritmen kör k gånger. Vid varje iteration hello exemplen i hello aktuella vikning används som en verifiering, hello resten av hello exempel används som en träningsmängden. hello algoritmen tränar över utbildning exempel och hello prestandamått beräknas över verifieringen exempel. Hello slutet av denna loop för varje uppsättning hyperparameter värden, vi beräkna medelvärdet av hello k prestanda mått och välj hyperparameter värden som har hello bästa genomsnittliga prestanda.

Som tidigare nämnts förutsägande Underhåll problem registreras data som en tidsserie händelser som kommer från flera datakällor. Dessa poster sorteras enligt toohello tiden för etikettering en post eller ett exempel. Därför om vi dela hello datauppsättningen slumpmässigt till utbildning och validering uppsättning är vissa hello utbildning exempel senare än några exempel validering. Detta resulterar i att uppskatta framtida prestanda för hyperparameter värden baserat på hello data som anlänt innan modellen har tränats. Dessa beräkningar kanske alltför optimistisk, särskilt om tidsserier inte är stilla och ändra deras beteende över tid. Valda hyperparameter värden kan därför inte är optimalt.

Ett bättre sätt att hitta bra värdena för justeringsmodeller är toosplit hello exempel i utbildning och validering i tid beroende sätt, så att alla validering exemplen är senare än alla utbildning exempel. Sedan för varje uppsättning värden för justeringsmodeller vi träna algoritmen hello över träningsmängden, åtgärd modellens prestanda över hello samma verifiering och väljer hyperparameter värden som visar hello bästa prestanda. När time series-data inte är stilla och utvecklas över tid, hello hyperparameter värdena som väljs av tåget/validering dela lead tooa bättre framtida ”modellens prestanda än med hello värdena som väljs slumpmässigt av korsvalidering.

hello sista modellen genereras av utbildning en inlärningsalgoritm över hela data med hjälp av hello bästa hyperparameter värden som hittas med hjälp av utbildning/validering delade eller korsvalidering.

### <a name="testing-for-model-performance"></a>Testa prestanda för modellen
När du har skapat en modell måste vi tooestimate framtida prestanda på nya data. hello enklaste uppskattning kan vara hello prestanda för hello modellen över hello utbildningsdata. Men denna uppskattning är alltför optimistisk eftersom modellen är skräddarsydda toohello data används tooestimate prestanda. En bättre uppfattning kan vara prestanda för mått hyperparameter värden som beräknats över hello validering mängd eller ett mått för genomsnittliga prestanda som beräknats från korsvalidering. Men för hello samma orsaker som tidigare beskrivits, dessa beräkningar är fortfarande alltför optimistisk. Vi behöver mer realistisk metoder för att mäta prestanda i modellen.

Ett sätt är toosplit hello data slumpmässigt till uppsättningar för träning, validering och test. hello är utbildning och validering används tooselect värdena för justeringsmodeller och train hello-modell med dem. hello prestandan för modellen mäts över hello test mängd.

Ett annat sätt som är relevanta toopredictive underhåll, är toosplit exempel i utbildning, validering och testa uppsättningar i tid beroende sätt, så att alla testar exempel är senare än alla exempel för träning och validering. När hello dela modellen generation och prestanda måttenhetsvärde är klar hello samma som tidigare beskrivits.

När-tidsserier är stilla och enkelt toopredict generera båda metoderna liknande uppskattningar av framtida prestanda. Men när tidsserie ej stationära och/eller hårda toopredict, hello andra sättet genererar mer realistisk uppskattningar av framtida prestanda än hello första.

### <a name="time-dependent-split"></a>Tid beroende delning
Som bästa praxis, i det här avsnittet tar vi en närmare titt på hur du implementerar tid beroende dela. Vi beskriver en dubbelriktad delning tid beroende mellan träning och testning mängder, men exakt hello samma logik tillämpas för tid beroende dela för uppsättningar för träning och validering.

Anta att vi har en dataström med tidsstämplad händelser, t.ex mått från olika sensorer. Funktioner i utbildning och testa exempel samt deras etiketter definieras över tidsramar som innehåller flera händelser.
Till exempel för binär klassificering, enligt beskrivningen i funktionen Engineering och modellering tekniker avsnitt funktioner skapas baserat på hello tidigare händelser och etiketter skapas baserat på framtida händelser inom ”X” tidsenheter i hello framtida. Därför kommer senare hello etiketter tidsramen för ett exempel sedan hello tidsramen för dess funktioner. Tid beroende delning väljer vi en punkt i tiden då vi träna en modell med ögonen öppna för justeringsmodeller med hjälp av historiska data in toothat punkt. tooprevent sprids framtida etiketter som ligger utanför hello utbildning avklippt i träningsdata, vi välja hello senaste tidsram toolabel utbildning exempel toobe X-enheter innan hello utbildning slutdatum. På bild 7 representerar varje fylld cirkel en rad i hello slutliga funktionen datauppsättning för vilka hello funktioner och etiketter beräknas enligt toothe metod som beskrivs ovan. Hello bild visar ges som hello-poster som ska träda i träning och testning anger när du implementerar tid beroende delning för X = 2 och W = 3:

![Bild 7. Tid beroende dela för binär klassificering](media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png)

Bild 7. Tid beroende dela för binär klassificering

hello representerar grön rutor hello poster toohello tidsenheter som kan användas för utbildning. Enligt beskrivningen tidigare, varje utbildning exemplet i hello slutliga tabellen feature genereras genom att titta på föregående 3 perioderna för funktionen generation och 2 framtida perioder för etikettering innan utbildning dag avklippt. Vi använda inte exempel i hello utbildning när någon del av hello 2 framtida perioder för det exemplet ligger utanför hello utbildning avklippt eftersom antar vi att vi inte har insyn utöver utbildning avklippt. På grund av toothat begränsning representerar svart exemplen hello poster av hello sista etiketten datamängd som inte ska användas i hello datauppsättning för träning. Dessa poster används inte vid testning av data, antingen eftersom de är innan hello utbildning avklippt och deras märkning tidsramar beror delvis på hello utbildning tidsram som inte ska vara hello fallet som vi vill gärna toocompletely separata etiketter tidsramar för träning och testning tooprevent etikett information sprids.

Den här tekniken möjliggör överlapp av hello data som används för generering av funktionen mellan träning och testning exempel som är nära toothe utbildning avklippt. Beroende på datatillgänglighet, kan en även allvarligare uppdelning åstadkommas genom att inte använda någon av hello exemplen i uppsättningen test som ligger inom W tidsenheter av hello utbildning avklippt.

Från vårt arbete påträffades att regression modeller som används för att förutsäga återstående livslängd mer störs av hello läckageproblem och med en slumpmässig delning leder tooextreme overfitting. Regression problem ska hello dela på samma sätt, så att poster som hör till tillgångar med fel innan utbildning avklippta ska användas för träningsmängden och resurser som har fel när hello avklippt ska användas för att testa set.

Som en allmän metod är en annan viktig bästa praxis för att dela data för träning och testning toouse en delning av Tillgångsnummer så att ingen av hello tillgångar som användes i utbildning används för att testa eftersom idé tester är säker på att toomake som när en ny tillgång används  toomake förutsägelser på, hello modellen tillhandahåller realistiska resultat.

### <a name="handling-imbalanced-data"></a>Hantering av imbalanced data
Klassificering problem om det finns fler exempel på en klass än i hello sägs andra data är hello toobe imbalanced. Vi rekommenderar vi vill gärna toohave tillräckligt med representanter för varje klass i hello utbildning data toobe kan toodifferentiate mellan olika klasser. Om en klass är mindre än 10% av hello data kan anta vi att hello data är imbalanced och vi kallar hello underrepresented dataset minoritet klass. Mycket, ofta hitta vi imbalanced datauppsättningar om en klass är kraftigt underrepresented jämfört med tooothers till exempel genom att endast som utgör 0,001% hello datapunkter. Klassen obalans är ett problem i många domäner inklusive att upptäcka bedrägerier, intrång och förutsägande Underhåll där fel har vanligtvis sällsynta förekomster hello tillgångar som utgör hello minoritet klassen exempel hello livstid.

Om klassen obalans manipuleras prestanda för de flesta standard algoritmer för maskininlärning som de syftar toominimize hello övergripande Felfrekvens. För en datauppsättning med 99% negativt klassen exempel och 1% positivt klassen exempel kan vi få 99% noggrannhet genom att helt enkelt etiketter alla instanser som negativt. Men det felaktigt klassificerar alla positivt exempel hello algoritmen är inte en användbar även om hello noggrannhet mått är mycket hög. Vanliga utvärdering mätvärden, till exempel övergripande noggrannhet på frekvens för fel är därför inte tillräckliga vid imbalanced learning. Andra mätvärden, till exempel precision, återkallning, F1 resultat och kostnaden justerade ROC kurvor som används för utvärderingar vid imbalanced datauppsättningar som beskrivs i hello utvärdering mått avsnitt.

Det finns dock några metoder som hjälper avhjälpa klassen obalans problem. hello två huvudsakliga som samlar in tekniker och cost känsliga learning.

#### <a name="sampling-methods"></a>Provtagning
hello användning av provtagning metoder i imbalanced learning består av ändring av hello dataset av vissa autentiseringsmekanismer i ordning tooprovide belastningsutjämnade dataset. Även om det finns en mängd olika provtagningar, de flesta direkt vidarebefordra som är slumpmässig oversampling och under provtagning.

Bara angivna, slumpmässiga oversampling väljer slumpmässigt från minoritet klass, replikeras de här exemplen och lägga till dem tootraining datauppsättning. Detta ökar hello antal totala exemplen i minoritet klass och slutligen balansera hello antal exempel på olika klasser. En risk för oversampling är att flera instanser av vissa exempel kan orsaka hello klassificerare toobecome för specifika inledande toooverfitting.
Detta skulle leda till hög utbildning noggrannhet men ha mycket dåliga prestanda på den överblivna testa data. Däremot väljer slumpmässiga under provtagning slumpmässigt från majoritet klass och ta bort dessa exempel från datauppsättning för träning. Ta bort exempel från majoritet klass kanske dock hello klassificerare toomiss viktiga begrepp gällande toohello majoritet klass. Hybrid provtagning där minoritet klassen översamplas och majoritet klassen samplas under på hello samtidigt är ett annat användbart sätt. Det finns många andra mer avancerade provtagningar är tillgängliga och effektiva provtagningsmetoderna för klassen obalans är ett populärt research område emot konstant uppmärksamhet och bidrag från många kanaler. Användning av olika tekniker för att bestämma hello mest effektiva de som är vanligtvis vänstra toohello data forskare tooresearch och experimentera och kan variera mycket beroende på hello dataegenskaper. Dessutom är det viktigt toomake till att provtagningsmetoder är tillämpade endast toohello utbildning angetts men inte hello test uppsättningen.

#### <a name="cost-sensitive-learning"></a>Kostnad känsliga learning
I förutsägande Underhåll fel som utgör hello minoritet klass av större intresse än vanliga exempel och därmed hello fokus ligger på prestanda hello algoritmen på fel är vanligtvis hello fokus. Detta kallas ofta olika förlust eller asymmetriska kostnaderna för misclassifying element i olika klasser där felaktigt förutsäga ett positivt som negativt kan kosta mer än vice versa. hello önskade klassificerare ska kunna kan toogive hög förutsägelsefunktionen via hello minoritet klass, utan att kraftigt hello noggrannhet för hello majoritet klass.

Det finns flera sätt som du kan göra detta. Genom att tilldela en hög kostnad felklassificering av hello minoritet kan klass och försök toominimize förlorar den sammanlagda kostnaden, hello problemet med olika effektivt behandlas. Vissa maskininlärningsalgoritmer använder den här idén natur, till exempel SVMs (Support Vector datorer) där kostnaden för positiva och negativa exempel kan införlivas under utbildning. På samma sätt används metoderna och vanligtvis visa goda prestanda vid imbalanced data som ökar beslut trädet algoritmer.

## <a name="evaluation-metrics"></a>Utvärdering av mått
Som tidigare nämnts gör klass obalans sämre prestanda som algoritmer tenderar tooclassify majoritet klassen exempel bättre i kostnaden för minoritet klassen fall som hello totala felklassificering fel har förbättrats avsevärt när majoritet klass är märkt med korrekt. Detta orsakar låg återkallning priser och blir ett större problem när hello kostnaden för falsklarm toohello företag är mycket hög. Precisionen är hello populäraste mått som används för att beskriva en klassificerare prestanda. Men som beskrivs ovan noggrannhet är ineffektiv och inte hello verkliga prestanda för en klassificerare funktioner som den är mycket känslig toodata distributioner. Andra utvärdering mått är i stället använda tooassess imbalanced learning problem. I sådana fall ska precision, återkallning och F1 resultat hello inledande mått toolook på när du utvärderar prestanda för förutsägande Underhåll modellen. Återkalla priser anger hur många av hello fel i hello test set korrekt har identifierats av hello modell för förutsägande underhåll. Högre återkallning priser betyder hello modellen är att fånga hello true fel. Precision mått avser hello mängden falsklarm där lägre precision priser motsvarar högre falsklarm. F1 poäng anser både precision och återkalla priser med bästa värdet 1 och sämsta är 0.

Dessutom för binär klassificering, decile tabeller och lift är diagram mycket informativa när du utvärderar prestanda. De fokus ligger endast på klassen positivt (fel) och ger en mer komplex bild av algoritmen prestanda än vad som visas genom att titta på bara en fast operativa punkt på hello ROC (mottagare operativsystem egenskap) kurvan.
Decile tabeller erhålls genom att beställa hello test exempel enligt deras förväntade troliga fel beräknas av hello modell innan tröskelvärde toodecide på hello slutliga etiketten. hello beställda exempel grupperas i deciles (d.v.s. hello 10% prov med största sannolikhet och 20%, 30% och så vidare). En kan beräkna hur hello algoritmen prestanda ändras vid varje decile av computing hello förhållandet mellan true positivt hastighet för varje decile och dess slumpmässiga baslinjen (d.v.s. 0.1, 0,2..). Lift diagram är används tooplot decile värden genom att markera decile SANT positiva satsen jämfört med slumpmässiga true positiva satsen par för alla deciles. Vanligtvis är hello första deciles hello fokus hello resultat eftersom här största vinster visas. Första deciles kan också ses som representativa för ”oskyddad” när den används för förebyggande underhåll.

## <a name="sample-solution-architecture"></a>Exempel lösningsarkitektur
När du distribuerar en lösning med förutsägande Underhåll kan vi är intresserad av en tooend lösning som innehåller en kontinuerlig cykel för datapåfyllning, lagring av data för modellen utbildning, funktionen generering, förutsägelse och visualisering av hello resultat tillsammans med en aviseringen genererar mekanism, till exempel en tillgång övervakning instrumentpanel. Vi vill en pipeline för data som ger framtida insikter toohello användare i ett kontinuerligt automatiserat sätt. En exempel förutsägande Underhåll arkitektur för sådana en IoT data pipeline illustreras i bild 8 nedan. I arkitekturen realtid telemetri samlas in till en Händelsehubb som lagrar strömmande data. Dessa data inhämtas av stream analytics för realtidsbearbetning av data, till exempel generering av funktionen. hello funktioner används sedan för webbtjänsten för toocall hello förutsägelsemodell och resultatet visas på hello instrumentpanelen. AT hello samma tid, infogade data lagras i en historisk databas och samman med externa datakällor som lokala data baserar toocreate utbildning exempel för modellering också.
Samma datalager kan användas för batch poängsättning av hello exempel och lagring av hello resultat som kan vara används tooprovide förutsägande rapporter på instrumentpanelen för hello igen.

![Figur 8. Exempel lösningsarkitektur för förebyggande underhåll](media/cortana-analytics-playbook-predictive-maintenance/example-solution-architecture-for-predictive-maintenance.png)

Figur 8. Exempel lösningsarkitektur för förebyggande underhåll

Mer information om varje hello komponenter hello-arkitekturen finns för[Azure](https://azure.microsoft.com/) dokumentation.

