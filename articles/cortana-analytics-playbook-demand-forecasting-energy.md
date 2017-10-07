---
title: "aaaCortana Intelligence lösning mallen Playbook för begäran Prognosticering energi | Microsoft Docs"
description: "En Lösningsmall med Microsoft Cortana Intelligence som hjälper till att begäran för ett företag för energiförbrukning för en prognos."
services: cortana-analytics
documentationcenter: 
author: ilanr9
manager: ilanr9
editor: yijichen
ms.assetid: 8855dbb9-8543-45b9-b4c6-aa743a04d547
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2016
ms.author: ilanr9;yijichen;garye
ms.openlocfilehash: 32fc6ece7e24ced34282baf107548039694a38b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana Intelligence lösning mallen Playbook för begäran Prognosticering energi
## <a name="executive-summary"></a>Sammanfattning
I hello senaste åren har Sakernas Internet (IoT), alternativa energi och stordata ha en sammanfogad toocreate stora affärsmöjligheter i hello verktyget och energi domän. Vid hello har samtidigt, hello verktyget och hello hela energisektorn sett förbrukning förenkla med konsumenter krävande bättre sätt toocontrol användningen av energi. Därför hello verktyget och smarta rutnätet företag är i behov av bra tooinnovate och förnya sig själva. Dessutom många kraft och utility rutnät blir inaktuella och dyra toomaintain och hantera. Hello team har arbetat på ett antal Användarsegmentet inom hello energi domän under hello senaste året. Vi har påträffat ofta i vilka hello verktyg eller ISV: er (oberoende programvaruleverantörer) har söker i prognoser för framtida energiförbrukning under dessa åtaganden. Dessa prognoser spelar en viktig roll i verksamheten aktuella och framtida och har blivit hello grunden för olika användningsfall. Dessa inkluderar kortsiktiga och långsiktiga power belastningen prognos, handel, belastningsutjämning, rutnätet optimering osv. Stordata och avancerade analyser AA-metoder, till exempel Machine Learning (ML) är hello viktiga aktiverarna för produktion korrekt och tillförlitlig prognoser.  

I den här playbook vi samlat hello företag och analytiska riktlinjer som behövs för en lyckad utveckling och distribution av energiförbrukning prognos lösning. Dessa föreslagna riktlinjer hjälper verktyg, dataanalytiker och data engineers vid upprättandet av fullständigt operationalized molnbaserade, begäran prognoser lösningar. För företag som precis har börjat sina stordata och avancerade analyser resa representera sådan lösning hello inledande startvärdet i sina långsiktig strategi för smart rutnätet.

> [!TIP]
> toodownload ett diagram som ger en översikt över arkitekturen för den här mallen finns [Cortana Intelligence Lösningsmall arkitektur för begäran Prognosticering energi](cortana-analytics-architecture-demand-forecasting-energy.md).  
> 
> 

## <a name="overview"></a>Översikt
Det här dokumentet beskriver hello företag, data och tekniska aspekter av med hjälp av Cortana Intelligence och i särskilda Azure Machine Learning (AML) för hello implementering och distribution av energi prognoser lösningar. hello dokumentet består av tre delar:  

1. Förståelse för verksamheten  
2. Förstå data  
3. Teknisk implementering

Hej **företag att förstå** del hello business aspekt en behov toounderstand och Överväg att tidigare toomaking en investering beslut. Den förklarar hur tooqualify hello affärsproblem på hand tooensure att förutsägelseanalyser och maskininlärning är verkligen effektiva och tillämpliga. ytterligare hello dokumentet förklarar hello grunderna i machine learning och hur den används tooaddress energi prognoser problem. Det visar hello förutsättningar och hello kriterier för användningsfall. Vissa exempel använda fall och företag fallet scenarier ingår också.

Data är hello huvudsakliga beståndsdel för någon lösning för maskininlärning. Hej **Data förstå** en del av det här dokumentet beskriver vissa viktiga aspekter av hello data. Det visar hello typ av data som behövs för energi prognoser data kvalitetskrav och vilka datakällor som vanligtvis finns. Vi förklarar också hur hello rådata är används tooprepare datafunktioner som faktiskt enhet hello modeling del.

hello tredje delen av hello dokument beskriver hello **teknisk implementering** aspekt av en lösning. Funktionen tekniker och modellering på hello kärnan i hello av vetenskapliga data och är därför som diskuteras i viss detalj. Den omfattar hello begreppet webbtjänster som är ett viktigt fordon för distribution av förutsägelseanalyslösningar. Vi beskriver också en typisk arkitektur operationalized slutpunkt till slutpunkt-lösning.

Hello-dokumentet innehåller dessutom referensmaterialet som du kan använda toogain ytterligare förståelse av hello domän- och teknikbehov.

Det är viktigt toonote att vi inte avser toocover i det här dokumentet hello djupare datavetenskap process, dess matematiska och tekniska aspekter. Dessa uppgifter finns i [Azure ML-dokumentationen](http://azure.microsoft.com/services/machine-learning/) och [bloggar](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Målgrupp
hello målgrupp för det här dokumentet är både företag och tekniska personal som vill toogain kunskap och förståelse av Machine Learning-baserade lösningar och hur de används specifikt inom hello energi prognoser domän.

Datavetare kan också dra nytta av läsning av det här dokumentet toogain en bättre förståelse för hello hög nivå processen att enheter hello distribution av en energi prognoser lösning. Det kan också vara används tooestablish utgångspunkt bra startpunkt mer detaljerad och avancerade material i den här kontexten.

### <a name="industry-trends"></a>Världen
I hello senaste åren, IoT, alternativa energi och stordata ha en sammanfogad toocreate stora affärsmöjligheter hello verktyget och energi. Vid hello har samtidigt, hello verktyget och hello hela energi sektorer sett förbrukning förenkla med konsumenter krävande bättre sätt toocontrol användningen av energi.

Många verktyg och smarta elbolag har pioneering hello [smart rutnätet](https://en.wikipedia.org/wiki/Smart_grid) genom att distribuera ett antal Använd fall som gör att användning av hello data som genereras av hello rutnätet. Många användningsområden omfångsfasen handlar om hello sina egenskaper elektricitet produktion: inte ackumulerade eller lagrade tagits ur bruk som inventering. Därför måste tillverkas är användas. Verktyg som vill toobecome effektivare måste tooforecast strömförbrukningen bara eftersom som ger dem större möjlighet för**balansera tillgång och efterfrågan**, vilket gör energi svinn **minska växthusgaser gas utsläpp**, och kontrollera kostnaden.

När man talar kostnader, finns det en annan viktig aspekt som är pris. Nya möjligheter tootrade power mellan verktyg har hämtat i en bra måste för**göra prognoser för framtida behov och framtida pris elektricitet**. Detta kan hjälpa företag att fastställa sina produktionsvolymer.

När vi använder hello ordet ”smart' refererar vi faktiskt tooa rutnät som kan lära sig och sedan göra förutsägelser. Den kan förutsäga när ändringar i förbrukning samt **förutse tillfällig överbelastning situationer och justeras automatiskt för den**. Genom kontroll distans (med hello hjälp av dessa smarta mätare), kan lokaliserade överlagring situationer hanteras. **Genom att förutsäga först och sedan fungerar**, hello rutnätet gör själva smartare över tid.

Hello resten av det här dokumentet fokuseras på en specifik familj av användningsområden som täcker Skapa prognoser för framtida, kortsiktiga och långsiktiga energiförbrukning. Vi har arbetat inom följande områden för några månader och har fått vissa kunskap och kunskaper som möjliggör tooproduce branschen klass resultat. Andra användningsfall omfattas samt i hello dokument i hello nära framtid.

## <a name="business-understanding"></a>Förståelse för verksamheten
### <a name="business-goals"></a>Affärsmål
Hej **energi Demo** målet är toodemonstrate en typisk förutsägelseanalyser och maskininlärning lösning som kan distribueras i en mycket kort tidsperiod. Vår fokus är särskilt när du aktiverar energi begäran prognosen lösningar så att dess affärsvärde kan snabbt insåg och utnyttjas på. hello informationen i den här playbook hjälper hello kunden uppnå hello följande mål:

* Kort tid toovalue av machine learning baserad lösning
* Möjlighet tooexpand en pilot Använd case tooother använda fall eller tooa bredare scope baserat på deras affärsbehov
* Snabbt få Cortana Intelligence Suite produktinformation

Med dessa mål i åtanke syftar denna playbook till att leverera hello företag och teknisk kunskap som hjälper att uppnå dessa mål.

### <a name="power-load-and-demand-forecasting"></a>Power belastning och begäran prognoser
Inom hello energi sektor, kan det finnas många sätt tillgång och efterfrågan prognoser kan hjälpa dig med allvarliga problem. I själva verket kan begäran prognoser ses hello grunden för många core användningsområden i hello bransch. I allmänhet Vi anser att två typer av energi begäran prognoser: kortsiktigt och långsiktigt. Var och en kan ett annat syfte och använda en annan metod. hello största skillnaden mellan hello två är hello prognoser horizon, vilket innebär att hello tidsintervall i hello framtida vi skulle prognos.

#### <a name="short-term-load-forecasting"></a>Kort sikt belastningen prognoser
Inom hello kontext av energiförbrukning definieras kort sikt ladda prognoser (STLF) som hello aggregerade belastning som prognostiserat i hello nära framtiden på olika delar av hello rutnätet (eller hello rutnät som helhet). I den här kontexten är kortvarigt definierade toobe tidsrymd inom hello 1 timme too24 timmar. I vissa fall kan också en horizon 48 timmar. Därför är STLF väldigt vanligt i drift fall av hello rutnät. Här följer några exempel på STLF drivs användningsområden:

* Tillgång och efterfrågan belastningsutjämning
* Stöd för energisparfunktioner handel
* Marknaden gör (inställningen power pris)
* Rutnätet operativa optimering
* [Svaret på begäran](https://en.wikipedia.org/wiki/Demand_response)
* Högsta begäran prognoser
* Hantering av efterfrågan på klientsidan
* Belastningsutjämning och överlagra förebyggande
* Långsiktigt belastningen prognoser
* Identifiering av fel och avvikelseidentifiering
* Utjämning av belastning/reducering 

STLF modellen främst baseras på hello nära tidigare (sista dag eller vecka) uppgifter och Använd prognostiserat temperatur som en viktig ge prognoser. Hämta korrekt temperatur prognos hello nästa timma och uppåt too24 timmar blir mindre en utmaning nu dagar. Dessa modeller är mindre känsliga tooseasonal mönster eller trender för långsiktig användning.

SLTF lösningar är också troligt toogenerate stor volym med förutsägelse anrop (tjänstbegäranden) eftersom de är anropas timme och i vissa fall även med högre frekvens. Det är också mycket vanligt toosee implantation där varje enskild understation eller transformatorn representeras som en fristående modell och är därför hello volymen av begäranden för förutsägelse ännu större.

#### <a name="long-term-load-forecasting"></a>Långsiktigt belastningen prognoser
hello-målet på lång sikt belastningen prognoser (LTLF) är tooforecast power begäran med en tidsrymd som sträcker sig från 1 vecka toomultiple månader (och i vissa fall för ett antal år). Det här intervallet för horizon gäller främst för planering och investering användningsfall.

För långsiktig scenarier är det viktigt toohave hög kvalitet data som beskriver ett intervall av flera år (lägsta 3 år). Dessa modeller vanligtvis extraheras säsongsvärdet mönster från hello historiska data och använda externa predicators sådana som väder och klimatförändringar mönster.

Det är viktigt tooclarify som hello längre hello prognoser horizon är hello mindre exakt hello prognos kanske. Det är därför viktigt tooproduce vissa förtroende intervall tillsammans med faktiska hello prognos som skulle låta människor toofactor hello möjliga variationen i sina planeringsprocessen.

Eftersom hello förbrukningen för LTLF främst planering, kan vi räknar med mycket lägre förutsägelse volymer (som jämfört med tooSTLF). Vi skulle normalt finns dessa förutsägelser bädda in i visualiseringsverktyg som Excel eller PowerBI och skötas manuellt av användaren hello.

### <a name="short-term-vs-long-term-prediction"></a>Kort sikt vs. Långsiktigt förutsägelse
hello följande tabell jämförs STLF och LTLF i avseende toohello viktigaste attribut:

| Attribut | Kort sikt belastning vid en prognos | Långsiktigt belastningen prognos |
| --- | --- | --- |
| Prognos Horizon |1 timme too48 timmar |Från 1 too6 månader eller mer |
| Data granularitet |Varje timme |Varje timme eller varje dag |
| Vanliga användningsområden |<ul><li>/ Efterfrågan belastningsutjämning</li><li>Välj timme prognoser</li><li>Svaret på begäran</li></ul> |<ul><li>Långsiktigt planering</li><li>Rutnätet tillgångar planering</li><li>Resursplanering</li></ul> |
| Vanliga predictors |<ul><li>Dag eller vecka</li><li>Timme på dagen</li><li>Varje timme temperatur</li></ul> |<ul><li>Månad</li><li>Dagen i månaden</li><li>Långsiktigt temperatur- och klimatförändringar</li></ul> |
| Historiska dataintervall |Två toothree års data |Fem too10 års data |
| Vanliga noggrannhet |MAPE * 5% eller lägre |MAPE * 25% eller lägre |
| Prognosen frekvens |Varje timme eller var 24: e timme |Genereras när varje månad, kvartal eller år |

\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – innebär genomsnittlig procent fel

Kan ses från den här tabellen, är det mycket viktigt toodistinguish mellan hello kort och hello långsiktiga prognoser scenarier då dessa representerar olika affärsbehov och kan ha olika distribution och användningsmönster.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Exempel användningsfall 1: eSmart system – överlagring optimering
En viktig roll i en [smart rutnätet](https://en.wikipedia.org/wiki/Smart_grid) är toodynamically och ständigt optimera och justera för hello ändra förbrukning mönster. Strömförbrukningen kan påverkas av kortsiktig ändringar som beror huvudsakligen på temperatur variationer (*t.ex.*, mer kraft används för luften villkors- eller uppvärmning). AT hello samma tid, power förbrukning påverkas även av långsiktiga trender. Dessa kan innehålla säsongsvärdet effekter, helgdagar, långsiktig förbrukning tillväxt och även ekonomiska faktorer, till exempel konsumenten index, olja pris och BNP.

I det här fallet används [eSmart](http://www.esmartsystems.com/) vill toodeploy en molnbaserad lösning som gör att förutsäga hello benägenheten av en överlagring situation på alla angivna understation hello rutnätet. I synnerhet ville eSmart tooidentify omformarstationer som är sannolikt toooverload inom hello nästa timma, så en omedelbar åtgärd kan vidtas tooavoid eller lösa denna situation.

En korrekt och snabb göra förutsägelse kräver implementering av tre förutsägelsemodeller:

* Långt termen modellen som aktiverar prognoser energiförbrukningen på varje understation under hello nästa några veckor eller månader
* Kort sikt modellen som gör att förutsägelser av överlagring situationen på en viss understation under hello nästa timma
* Temperatur-modell som ger prognoser för framtida temperatur över flera scenarier

hello syftar hello långsiktiga modellen toorank hello omformarstationer av deras benägenheten toooverload (anges sin kapacitet för överföring av power) under hello nästa vecka eller månad. Detta gör att hello skapandet av en kort lista med omformarstationer som skulle användas som indata för hello kortsiktig förutsägelse. Som temperaturen är ett viktigt ge prognoser för långsiktig hello-modellen, finns en måste tooconstantly skapa flera scenariot temperatur införs och feed dem som indata till toohello långsiktiga modell. hello kortvarigt prognos anropas sedan toopredict vilka understation är sannolikt toooverload över hello nästa timma.

hello kort och lång sikt modeller distribueras separat per varje understation. Hello praktiska körning av dessa modeller kräver därför omfattande orchestration. varje timme på dagen hello dedikerade toogain högre förutsägelsefunktionen hello kort sikt en mer detaljerad modell. Dessa modeller utförs varje timme och slutför körs inom några minuter tooallow tillräcklig tid toorespond och vidta förebyggande åtgärder om det behövs. Den här samlingen av modeller hålls uppdaterad av periodiska via programmering med hjälp av hello senaste data.

Mer information om den här användningsfall hittar [här](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Använd Case kriterier – krav
hello huvudsakliga styrkan hos Cortana Intelligence är kraftfull möjlighet toodeploy och skala machine learning central lösningar. Det är utformad toosupport tusentalsavgränsare i förutsägelser som körs samtidigt. Kan anpassas automatiskt toomeet ett ändra förbrukning mönster. Det är därför en lösning fokus på beräkningar prestanda. Till exempel är ett verktyg för företag intresserad av att producera korrekta energiförbrukning för hello nästa timma och för varje timme på dagen hello. Hej på andra sidan, vi vill mindre besvaras hello frågan om varför hello-begäran är förväntade toobe (hello modellen själva ta hand om).

Det är därför viktigt toorealize att inte alla användningsfall och företagsproblem kan lösas effektivt använder maskininlärning.

Cortana Intelligence och maskininlärning kan vara mycket effektivt lösa affärsproblem angivna när hello följande villkor är uppfyllda:

* hello affärsproblem i hand är **förutsägande** till sin natur. En förutsägbar användning case exempel är ett verktyg för företag som vill toopredict power belastningen på en viss understation under hello nästa timma. Hej på andra sidan, analysera och rangordning drivrutiner historiska behovet att **beskrivande** natur och därför mindre tillämplig.
* Det finns en Rensa **sökvägen för åtgärden** toobe tas en gång hello förutsägelse är tillgänglig. Till exempel kan att förutsäga en överlagring på en understation under hello nästa timma utlösa en proaktiv åtgärd för att minska belastning som är associerad med den understation och vilket potentiellt gör en överlagring.
* hello användningsfall representerar en **vanliga typ av problem** så att när löst kan det få hello sätt toosolving andra liknande användningsfall.
* hello-kund kan ange **kvantitativa och kvalitativa mål** toodemonstrate en för lyckad lösningsimplementering. Till exempel ett bra kvantitativt mål för energi begäran prognos skulle vara hello krävs noggrannhet tröskelvärdet (*t.ex.*, in too5% fel tillåts) eller när förutsäga understation överlagring sedan hello precision (antal true positiva identifieringar) och återkalla (omfattningen av SANT positiva identifieringar) ska vara överskrider ett visst tröskelvärde. Dessa mål måste härledas från hello kundens affärsmål.
* Det finns en Rensa **integrering scenariot** med hello företagets business arbetsflöde. Hello understation belastningen prognos kan till exempel integreras i hello rutnätet kontrollen tooallow överlagring förebyggande aktiviteter.
* hello kunden har redo toouse **data med tillräckligt hög kvalitet** toosupport hello användningsfall (se mer i hello nästa avsnitt, **Data Quality**, för den här playbook).
* Hej kunden omfattar central data molnarkitektur eller **molnbaserade maskininlärning**, inklusive Azure ML och andra komponenter i Cortana Intelligence Suite.
* hello kunden är villigt tooestablish **dataflöde en end tooend** att verksamhet hello överföringen av data i hello moln kontinuerligt och är villigt för**operationalisera** hello lösning.
* hello kunden är redo för**dedikerar resurser** som kommer att aktiverats och blivit engagerade vid första pilotprojekt hello-implementeringen så att kunskap och ägare av hello-lösning kan överföras toohello kunden vid lyckades slutförande.
* hello kunden resursen ska vara en **skicklig data professional**, helst en data-forskare.

Kriteriet av användningsfall baserat på hello villkoren ovan kan avsevärt förbättra hello slutförandefrekvenser för användningsfall och upprätta en bra beachhead för hello implementering för framtida användning.

### <a name="cloud-based-solutions"></a>Molnbaserade lösningar
Cortana Intelligence Suite i Azure är en integrerad miljö som finns i hello molnet. hello distribution av en lösning för avancerade analyser i en molnmiljö innehåller betydande fördelar för företag och på hello samtidigt kan innebära stor förändring för företag att fortfarande använda lokala IT-lösningar. Inom hello energisektorn finns en tydlig trend över stegvis migrering av åtgärder i hello moln. Denna trend går hand i hand tillsammans med hello utvecklingen av hello smart rutnät som beskrivs ovan, i **världen**. Den här playbook fokuserar på en molnbaserad lösning i hello energi domän, är det viktigt tooexplain hello fördelar och andra överväganden för att distribuera en molnbaserad lösning.

Kanske är hello största fördelen med en molnbaserad lösning hello kostnaden. Som en lösning använder molnet distribuerade komponenter, det finns ingen direktkostnader eller kostnad för sålda varor (kostnader för sålda varor) Komponentkostnader som är kopplade till den. Det innebär att det finns inget behov av tooinvest av maskinvara, programvara och IT-underhåll och det finns därför en betydande minskning av företag risk.

En annan viktig fördel är hello betalning per användning kostnaden strukturen för molnbaserade lösningar. Molnbaserad servrar för datoranvändning eller lagring kan distribueras och skalas på en just-efter behov. Detta representerar hello kostnaden effektivitet nytta av en molnbaserad lösning.

Slutligen är det inget behov av att investera i IT-underhåll eller för framtida infrastrukturutveckling, eftersom detta är en del av hello molnbaserat erbjudande. toothat utsträckning, Cortana Intelligence Suite innehåller hello bäst i klassen tjänster och dess översikt över håller utvecklas. Nya funktioner, komponenter och funktioner introduceras hela tiden och utvecklas.

För ett företag som precis har startats dess övergång i hello moln rekommenderar hög vi tootake en stegvis metod genom att implementera ett moln migrering översikt över. Vi tror att hello användningsområden som beskrivs i den här playbook representerar en utmärkt möjlighet för testkörning förutsägelseanalyslösningar i molnet hello för verktyg och företag i hello energi domän.

#### <a name="business-case-justification-considerations"></a>Business-Case motivering överväganden
I många fall kan hello kund vara intresserad av att göra en motivering för ett givet användningsfall där en molnbaserad lösning och Machine Learning är viktiga komponenter. Till skillnad från en lokal lösning i hello fallet med en molnbaserad lösning hello summa kostnad komponenten är minimal och de flesta hello kostnadselement är associerad med den faktiska användningen. När det gäller toodeploying en energi prognoser lösning på Cortana Intelligence Suite kan flera tjänster integreras med en enda gemensam kostnadsstruktur. Exempelvis databaser (*t.ex.*, SQL Azure) kan vara används toostore hello rådata och sedan för hello faktiska införs Azure ML är används toohost hello prognoser tjänster. I det här exemplet kan hello kostnad struktur omfatta lagring och komponenter.

På hello däremot bör ett ha en god förståelse av hello affärsvärde drift ett energiförbrukning prognoser (kort eller lång sikt). I själva verket är det viktigt toorealize hello affärsvärde för varje prognosen åtgärd. Till exempel korrekt prognoser power belastning för hello nästkommande 24 timmar kan förhindra overproduction eller kan förhindra överlagringar på hello rutnätet och detta kan vara möjligt vad gäller finansiella besparingar dagligen.

En grundläggande formel för beräkning av hello finansiella fördelen begäran prognos lösning är: ![grundläggande formel för beräkning av hello finansiella fördelen begäran prognos lösning](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Eftersom Cortana Intelligence Suite ger en betalning per användning prismodellen så behövs det ingen medför en fast kostnad komponenten toothis formel. Den här formeln kan beräknas för varje dag, varje månad eller årlig.

Aktuella Cortana Intelligence Suite och Azure ML prissättning finns [här](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Lösning utvecklingsprocessen
Hej utvecklingscykeln av en energiförbrukning prognoser lösningen innebär vanligtvis 4 faser, som vi göra utnyttjar molnbaserad teknik och tjänster inom hello Cortana Intelligence Suite.

Detta illustreras i följande diagram hello:

![Smart rutnätet cykel](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

hello följande stycke beskrivs processen steg 4:

1. **Datainsamling** – alla avancerade analyser baserade lösningen är beroende av data (se **Data förstå**). Mer specifikt när det gäller toopredictive analyser och prognoser vi förlitar sig på pågående, dynamiska flödet av data. Hello gäller energi begäran prognoser, dessa data kan hämtas direkt från smart mätare eller aggregeras redan på en lokal databas. Vi använder sig också av andra externa datakällor, till exempel väder och temperatur. Den här pågående flödet av data måste styrd, schemalagd och lagras. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (ADM) är vår huvudsakliga bestämmer hög grad för att utföra den här uppgiften.
2. **Modeling** – för korrekt och tillförlitlig energi prognoser en utveckla (tåg) och upprätthålla en bra modell som möjliggör användning av hello historiska data och extrakt hello beskrivande och förutsägbara mönster i hello data. hello område för Machine Learning (ML) har växt snabbt med mer avancerade algoritmer utvecklas regelbundet. Azure ML Studio ger en bättre användarupplevelse som hjälper till att använda hello mest avancerade algoritmer ML inom en fullständig arbetsflöde. Det här arbetsflödet illustreras i en intuitiv flödesdiagram och innehåller hello förberedelse av data, funktionen extrahering, modellering och utvärdering av modellen. hello användare kan dra på hundratals olika modeller som ingår i den här miljön. Hello slutet av den här fasen har en data-forskare en aktiv-modell som har utvärderats och är klar för distribution.
   
   följande diagram hello är en illustration av ett vanligt arbetsflöde:
   
   ![Modellera arbetsflödet](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)
3. **Distribution** – med en aktiv-modell, hello nästa steg är distribution. Här omvandlas hello modellen till en webbtjänst som Exponerar en RESTful-API som kan anropas samtidigt över hello Internet från olika klienter för användning. Azure ML ger ett enkelt sätt att distribuera en modell direkt från hello Azure ML Studio med ett enda klick på en knapp. hello hela distributionsprocessen sker under huven hello. Den här lösningen kan anpassas automatiskt toomeet hello krävs förbrukning.
4. **Förbrukning** – i det här steget ska vi faktiskt göra användning av hello prognoser modellen tooproduce förutsägelser. hello förbrukningen kan köras från en användarprogram (*t.ex.*, instrumentpanel) eller direkt från ett system som fungerar som/efterfrågan NLB system eller en lösning för optimering av rutnätet. Flera användningsområden kan köras från en modell.

## <a name="data-understanding"></a>Förstå data
Efter täcker hello business överväganden (se **företag att förstå**) en efterfrågan på energi prognoser lösning, vi är nu redo toodiscuss hello datadel. Alla förutsägelseanalys är beroende av tillförlitliga data. För energi prognoser för begäran, vi förlitar sig på av historiska förbrukningsdata med olika detaljnivåer. Den historiska data används som hello raw material. Den kommer att göras en noggrann analys i vilken hello data forskare identifierar predictors (även hänvisade tooas funktioner) som kan placeras i en modell som slutligen skapar prognoser hello krävs.

I hello resten av det här avsnittet, kommer vi beskriver hello olika åtgärder och överväganden för att förstå hello data och hur toobring den tooa användbart format.

### <a name="hello-model-development-cycle"></a>hello modellen utvecklingscykeln
Produktion av bra prognoser modeller kräver vissa förberedelser för noggrann planering och. Bryta ned hello modellera processen i flera steg och fokuserar på ett steg i taget kan kraftigt förbättra hello resultatet av hello hela processen.

hello följande diagram illustrerar hur hello modellera processen kan delas upp i flera steg:

![Modellen utvecklingscykeln](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Som kan ses hello cykeln består av sex steg:

* Problemet formulering
* Datapåfyllning och datagranskning
* Förberedelse av data och funktionen konstruktion
* Modellering
* Modellen utvärdering
* Utveckling

I hello resten av det här avsnittet beskriver vi hello enskilda steg och tooconsider objekt i varje steg.

### <a name="problem-formulation"></a>Problemet formulering
Vi kan överväga hello problemet formulering hello viktigaste steg ett behov tootake tidigare tooimplementing alla förutsägelseanalys. Vi skulle här transformeringen hello affärsproblem och dela upp den toospecific element som kan lösas med hjälp av data och modellering tekniker. Detta är ett bra tooformulate hello problem som en uppsättning frågor som vi vill gärna tooanswer. Här följer några möjliga frågor som kan användas inom hello omfång energi begäran prognoser:

* Vad är hello förväntade belastningen på en enskild understation i hello nästa timmar eller dagar?
* Vid vilken tid på dagen hello min rutnät får belastning begäran?
* Hur troligt är min rutnätet toosustain hello förväntad belastning?
* Hur mycket energi bör hello power station generera under varje timme på dagen hello?

Utformningen av dessa frågor kan vi toofocus på komma hello rätt data och implementera en lösning som är fullständigt justerad med hello affärsproblem till hands. Dessutom kan vi ange vissa viktiga mått som ger oss tooevaluate hello prestanda för hello modellen. Till exempel hur exakt bör hello prognos vara och vad är hello antal fel som är godkända av hello företag?

### <a name="data-sources"></a>Datakällor
hello moderna smart rutnätet samlar in data från olika delar och komponenter i hello rutnätet. Dessa data representerar olika aspekter av hello igen och hello användning av hello power rutnätet. Vi är att begränsa hello diskussion om datakällor som visar hello faktiska begäran förbrukning inom hello omfattning hello energiförbrukning prognos. En viktig källa för energiförbrukning är smart mätare. Verktyg runt hello världen distribuerar snabbt smart mätare för sina kunder. Smart mätare registrera hello faktiska energiförbrukningen och ständigt vidarebefordra data tillbaka toohello för företaget. Data samlas in och skickas tillbaka på ett fast intervall mellan timmen too1 5 minuter. Mer avancerade smart mätare programmerad via fjärranslutning toocontrol och balans hello faktisk förbrukning inom ett hushåll. Smart mätaren data är relativt tillförlitlig och innehåller en tidsstämpel. Det gör en viktig beståndsdel för begäran vid en prognos. Mätaren data kan sammanställas (samlade in) på olika nivåer inom hello rutnätet topologi: transformatorn, understation, region, *etc*. Vi kan sedan välja hello krävs aggregering nivå toobuild en prognosmodell för den. Till exempel om hello för företag skulle som tooforecast framtida belastning på var och en av dess rutnätet omformarstationer kan sedan alla mätare data sammanställs för varje enskild understation och används som indata för hello prognoser modellen. Vi refererar toosmart mätare som en intern datakälla.

En tillförlitlig energi begäran prognos förlitar sig även på andra externa datakällor. En viktig faktor som påverkar strömförbrukningen är hello väder- eller mer exakt hello temperatur. Historiska data visar stark korrelation mellan utanför temperatur- och strömförbrukning. Under varm sommar dagar användare se sina luftkonditionering och under hello vinter slå på systemet. En pålitlig källa för historiska temperaturer på hello rutnätet plats är därför nyckel. Dessutom vi också förlitar sig på rättvisande prognos av temperatur som en ge prognoser energiförbrukningen.

Andra externa datakällor kan också skapa energi begäran prognosmodeller. Dessa kan innehålla långsiktiga klimatförändringar ändringar, ekonomiska index (*t.ex.*, BNP), med mera. I det här dokumentet innehåller vi inte dessa andra datakällor.

### <a name="data-structure"></a>Datastruktur
När du identifierar hello krävs datakällor, skulle vi som tooensure att rådata som har samlats in innehåller hello rätta datafunktioner. toobuild en tillförlitlig begäran prognosmodell måste vi skulle tooensure som hello data som samlas in innehåller dataelement som kan hjälpa dig att förutsäga framtida hello-begäran. Här följer några grundläggande kraven hello datastruktur (schema) för hello rådata.

hello rådata består av rader och kolumner. Varje mått representeras som en enda rad med data. Varje rad med data innehåller flera kolumner (även hänvisade tooas funktioner eller fält).

1. **Tidsstämpeln** – hello tidsstämpelsfält representerar hello faktiska tid när hello mätning registrerades. Det måste uppfylla något av hello vanliga datum/tid-format. Både datum och tid delar ska tas med. I de flesta fall det behövs ingen för hello tid toobe registreras hello andra nivå. Det är viktigt toospecify hello tidszon som hello data registreras.
2. **Läsa ID** -fältet identifierar hello mätaren eller hello mätning enhet. Det är en kategoriska variabel och kan vara en kombination av siffror och tecken.
3. **Värdet för förbrukning** – detta är hello faktisk förbrukning vid ett angivet datum/tid. hello förbrukningen kan mätas i kWh (kilowatt-hour) eller andra önskade enheter. Det är viktigt toonote som hello måttenhet måste vara konsekvent över alla mått i hello data. I vissa fall kan förbrukning gav över 3 power faser. I så fall skulle måste toocollect alla hello oberoende förbrukning faser.
4. **Temperatur** – hello temperatur vanligtvis samlas in från en oberoende källa. Det bör dock vara kompatibel med hello förbrukningsdata. Den ska innehålla en tidsstämpel som beskrivs ovan, så att programmet toobe synkroniseras med hello faktisk förbrukningsdata. hello temperatur värdet kan anges i grader eller Fahrenheit men ska vara konsekvent över alla mått.
5. **Plats –** hello Platsfältet associeras vanligtvis med hello plats där hello temperatur data har samlats in. Det kan representeras som en zip-nummer eller latituden/longitud (lat/långt)-format.

hello följande tabeller visar exempel på ett bra förbrukning och temperatur dataformat:

| **Datum** | **Tid** | **Mätaren ID** | **Fas 1** | **Fas 2** | **Fas 3** |
| --- | --- | --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |ABC1234 |7.0 |2.1 |5.3 |
| 7/1/2015 |10:00:01 |ABC1234 |7.1 |2.2 |4.3 |
| 7/1/2015 |10:00:02 |ABC1234 |6.0 |2.1 |4.0 |

| **Datum** | **Tid** | **Plats** | **Temperatur** |
| --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |11242 |24.4 |
| 7/1/2015 |10:00:01 |11242 |24.4 |
| 7/1/2015 |10:00:02 |11242 |24.5 |

Som visas ovan, innehåller det här exemplet 3 olika värden för användning som är associerade med 3 power faser. Observera också att hello datum och tid fälten avgränsas, men de kan också slås samman till en kolumn. I det här fallet representeras hello plats kolumn i en formatet 5-siffriga postnummer och hello temperatur i grader Celsius format.

### <a name="data-format"></a>Dataformatet
Cortana Intelligence Suite kan stödja hello vanligaste dataformat som CSV, TVS, JSON, *etc*. Det är viktigt att hello dataformat hålls konsekvent för hello hela livscykeln för hello-projekt.

### <a name="data-ingestion"></a>Datainhämtning
Eftersom energi begäran prognos är kontinuerligt och ofta förutsade, måste vi Kontrollera att hello rådata flödar med hjälp av en heltäckande och tillförlitlig införandet av data. hello införandet processen måste garantera att hello rådata är tillgänglig för hello prognoser processen hello som krävs för närvarande. Innebär att hello data införandet frekvensen måste vara större än hello prognoser frekvens.

Exempel: om våra begäran prognoser lösning skulle Generera en ny prognos på 8:00:00 dagligen och vi behöver tooensure alla hello-data som har samlats in under hello senast 24 timmar har fullständigt inhämtas till den tidpunkten och har tooeven inkluderar hello senaste timmen av data.

I ordning tooaccomplish, Cortana Intelligence Suite ger olika sätt toosupport en tillförlitlig införandet av data. Detta diskuteras ytterligare i hello **distribution** i det här dokumentet.

### <a name="data-quality"></a>Data Quality
hello raw-datakälla som krävs för att utföra prognoser tillförlitliga och korrekta begäran måste uppfylla vissa grundläggande Datakriterier. Avancerade statistiska metoder kan vara används toocompensate för vissa möjliga data quality problemet, men vi behöver fortfarande tooensure att vi passerande vissa grundläggande data quality tröskelvärde när du vill föra in nya data. Här följer några överväganden om rådata kvalitet:

* **Värde saknas** – refererar detta toohello situation när specifikt mått inte samlades in. hello här grundläggande krav är att hello saknas värdet hastighet inte får vara större än 10% för en viss tidsperiod. I fall att ett värde saknas det bör anges med ett fördefinierat (till exempel: '9999') och inte '0' som kan vara ett giltigt mått.
* **Mätningsnoggrannhet** – hello faktiska värdet för förbrukning eller temperatur redovisas korrekt. Felaktiga mätningar genererar felaktiga prognoser. Normalt vara hello mätning fel lägre än 1% relativa toohello true-värde.
* **Tid för mätning** – det krävs hello faktiska tidsstämpeln hello uppgifter som samlas in kommer inte avvika med mer än 10 sekunder relativa toohello SANT tiden för hello faktiska mått.
* **Synkronisering** – när flera datakällor som används (*t.ex.*, förbrukning och temperatur) vi måste se till att det inte finns några tidssynkronisering problem mellan dem. Det betyder att hello tid skillnaden mellan tidsstämpel hello samlas in från alla två oberoende datakällor inte får överstiga mer än 10 sekunder.
* **Latens** – som beskrivs ovan, i **Datapåfyllning**, vi är beroende av en tillförlitlig trafikflöde och införandet av data. toocontrol att vi måste se till att vi styra hello datafördröjning. Det har angetts som hello tidsskillnaden mellan hello som hello verkliga mätningen togs och hello tid då det har lästs in i hello Cortana Intelligence Suite lagring och är redo för användning. För kortsiktigt får prognosmodellen hello totala svarstiden inte vara större än 30 minuter. För lång sikt får prognosmodellen hello totala svarstiden inte vara större än 1 dag.

### <a name="data-preparation-and-feature-engineering"></a>Förberedelse av data och funktionen konstruktion
När hello rådata har tagits inhämtas (se **Datapåfyllning**) och har på ett säkert sätt lagras, är det redo toobe bearbetas. hello förberedelsefasen data i grunden tar hello rådata och konvertera (omvandla, är Omforma) till ett formulär för hello modeling fasen. Som kan innehålla enkla åtgärder som att använda hello rådata kolumn som är med dess faktiska mätvärdet, standardiserade värden, mer komplexa åtgärder som [tid eftersläpande](https://en.wikipedia.org/wiki/Lag_operator), med mera. hello nyskapad datakolumner är refererad tooas datafunktioner hello processen för att generera dessa är refererad tooas funktionen tekniker. Hello slutet av den här processen har vi en ny datauppsättning som erhållits från hello rådata och kan användas för förutsägelsemodellering. Dessutom hello data förberedelsefasen måste tootake vård saknade värden (se **Data Quality**) och kompensera för dem. I vissa fall måste vi också toonormalize hello data tooensure alla värden som representeras i hello samma skala.

I det här avsnittet vi listor över hello vanliga funktioner som ingår i hello energi prognosmodeller begäran.

**Tid som drivs funktioner:** funktionerna härleds från hello datum/timestamp data. Dessa extraheras och konverteras till kategoriska funktioner som:

* Tid på dagen – detta är hello timme av hello dag tar värden från 0 too23
* Dag i veckan – detta representerar hello hello veckodag och tar värden från 1 (söndag) too7 (lördag)
* Dag i månaden – detta representerar hello faktiska datum och kan ta värden från 1 too31
* Månaden på året – detta representerar hello månad och tar värden från 1 (januari) too12 (December)
* Lördag – detta är en funktion för binärvärde som tar hello värdet 0 för veckodagar eller 1 för lördag
* Helgdag - detta är ett binärt värde funktion som tar hello värdet 0 för en vanlig dag eller 1 för en helgdag
* Fourier villkor – hello Fourier är vikter som härleds från Hej, tidsstämpel och är används toocapture hello säsongsvärdet (cycles) i hello data. Eftersom vi kan ha flera säsonger på våra data kan vi behöver flera Fourier termer. Begäran värden kan till exempel ha årlig, varje vecka och dag säsonger/cykler vilket leder till 3 Fourier villkoren.

**Oberoende mätning funktioner:** hello oberoende innefattar alla hello dataelement som vi vill att toouse som predictors i vår modell. Här utesluts hello beroende funktionen som vi måste toopredict.

* Funktionen lag – dessa är flyttat tid värdena för hello faktiska begäran. Fördröjning 1-funktioner ska innehålla hello begäran värdet i hello föregående Timma (förutsatt att per timme) relativa toohello aktuell tidsstämpel. På liknande sätt kan vi kan lägga till fördröjning 2, lag 3, *etc*. hello faktiska kombination av fördröjning funktioner som används fastställs under hello modeling fas av utvärderingen av hello modellen resultat.
* Långsiktigt trender – den här funktionen representerar hello linjär ökning efterfrågan mellan år.

**Funktionen för beroende:** hello beroende funktionen är hello datakolumnen som vi vill gärna vår modell toopredict. Med [övervakad maskininlärning](https://en.wikipedia.org/wiki/Supervised_learning), vi måste först träna hello-modell med hello beroende funktioner (som också är refererad tooas etiketter). Detta tillåter hello modellen toolearn hello mönster i hello data som är associerade med beroende hello-funktionen. Vi vill förmodligen toopredict hello faktiska begäran och därför kan vi använda den som hello beroende funktion energi efterfrågan vid en prognos.

**Hantering av värden som saknas:** under förberedelsefasen för hello data, behöver vi toodetermine hello bästa strategi toohandle värden som saknas. Detta är mest med hjälp av hello olika statistiska [uppräkning datametoder](https://en.wikipedia.org/wiki/Imputation_\(statistics\)). Hello gäller energiförbrukning prognoser, vi vanligtvis sedan imputera saknade värden med hjälp av glidande medelvärde från föregående tillgängliga datapunkter.

**Databasnormalisering:** databasnormalisering är en annan typ av transformation som används toobring alla numeriska data, till exempel begäran prognos i liknande omfattning. Detta ger vanligtvis bättre hello modellen noggrannhet och precision. Vi vanligtvis gör du genom att dividera hello faktiskt värde med hello dataområde hello.
Hello originalvärdet skalar till ett mindre intervall, vanligtvis mellan 1 och 1.

## <a name="modeling"></a>Modellering
hello modeling fas är där hello hello data till en modell konverteringen. I hello är kärnan i den här processen det avancerade algoritmer som skanna hello historiska data (utbildningsdata), extrahera mönster och skapa en modell. Att modellen kan vara senare används toopredict på nya data som inte har använt toobuild hello modellen.

När vi har en fungerande tillförlitliga krävs modellen vi kan sedan använda datorn tooscore nya data är strukturerade tooinclude hello funktioner (X). Hej riskpoängprocessen gör användning av beständiga hello-modellen (objekt från hello utbildning fasen) och förutsäga hello målvariabel som markerats med Ŷ.

### <a name="demand-forecasting-modeling-techniques"></a>Begäran prognoser Modeling tekniker
Begäran prognoser vi göra hello gäller användning av historiska data som är sorterade efter tid. Det används vanligtvis toodata som innehåller hello time-dimension som [time series](https://en.wikipedia.org/wiki/Time_series). hello mål i tid serien modellering toofind tid relaterade trender säsongsvärdet automatiskt korrelation (korrelation över tid) och formulera de till en modell.

Under de senaste åren har avancerade algoritmer utvecklade tooaccommodate tid serien prognoser och tooimprove prognoser noggrannhet. Kort diskuterar vi några av dem här.

> [!NOTE]
> Det här avsnittet är inte avsedda toobe som används som en machine learning och skapa prognoser översikt utan istället som en kort undersökning av modeling tekniker som ofta används för begäran Prognosticering. Mer information och utbildningsmaterial om tid serien prognoser rekommenderar vi starkt hello online book [Prognosticering: principer och praxis](https://www.otexts.org/book/fpp).
> 
> 

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (glidande medelvärde)**](https://www.otexts.org/fpp/6/2)
Glidande medelvärde är en hello första analytiska tekniker som har använts för tid serien Prognosticering och det är fortfarande ett av de vanligaste hello tekniker per dag. Det är också hello grunden för mer avancerade prognoser tekniker. Vi prognoser hello nästa datapunkt som medelvärdet över senaste punkterna hello K, där K anger hello ordningen för glidande medelvärde hello med glidande medelvärde.

hello glidande medelvärde tekniken har hello effekten av Utjämning hello prognos och därför inte kan hantera även stora volatil i hello data.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (exponentiell utjämning)**](https://www.otexts.org/fpp/7/5)
Exponentiell utjämning ETS är en familj av olika metoder som använder viktat medelvärde för senaste datapunkter i ordning toopredict hello nästa datapunkt. hello idé är tooassign högre vikterna toomore senaste värdena och minska gradvis denna vikt för äldre uppmätta värden. Det finns ett antal olika metoder med den här serien, dem bland hantering av säsongsvärdet i hello data som [Holt somrar vad gäller såväl när metoden](https://www.otexts.org/fpp/7/5).

Vissa av dessa metoder även ta hänsyn till hello säsongsvärdet hello data.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatisk Regression integrerat glidande medelvärde)**](https://www.otexts.org/fpp/8)
Automatisk Regression integrerad glidande medelvärde (ARIMA) är en annan familj av metoder som används för tidsserier prognoser. Det kombinerar praktiskt taget automatiskt regression metoder med glidande medelvärde. Automatisk regression metoder använda regression modeller genom att göra tidigare time series-värden i ordning toocompute hello nästa datum punkt. ARIMA metoder kan du också använda differentierande metoder som omfattar beräkna hello skillnaden mellan datapunkter och använda dem i stället för hello ursprungliga uppmätta värdet. Slutligen ARIMA också använder hello glidande medelvärde tekniker som beskrivs ovan. hello kombinationen av alla dessa metoder på olika sätt är vilka konstruktioner hello familj av ARIMA-metoder.

ETS och ARIMA används brett idag för energi begäran prognoser och många andra prognosmodellen problem. I många fall kombineras dessa tillsammans toodeliver mycket korrekta resultat.

**Allmän multipel Regression** Regression modeller kunde hello viktigaste sätt att modellera inom hello domänen för machine learning och statistik. Hello gäller tidsserier vi använder regression toopredict hello framtida värden (*t.ex.*, på begäran). I regression vi tar en linjär kombination av hello predictors och Läs hello vikter (även hänvisade tooas koefficienter) för de predictors under hello utbildning processen. hello målet är tooproduce regressionslinjen som kommer prognos våra förutsägelsevärdet. Regression metoder är lämplig när hello målvariabel är numeriska och därför passar också tid serien prognoser. Det finns en mängd olika metoder för regression inklusive väldigt enkelt regression modeller som [linjär Regression](https://en.wikipedia.org/wiki/Linear_regression) och mer avancerade de som till exempel beslutsträd, [slumpmässiga skogar](https://en.wikipedia.org/wiki/Random_forest), [Neurala nätverk](https://en.wikipedia.org/wiki/Artificial_neural_network), och förstärkta beslutsträd.

Konstruera energiförbrukning prognoser som regression problem ger oss mycket flexibilitet för att välja våra datafunktioner som kan kombineras hello faktiska begäran tidsdata serien och externa faktorer, till exempel temperatur. Mer information om hello valda funktioner beskrivs i funktionen Engineering hello (se **förberedelse av Data och funktionen Engineering**) avsnitt i den här playbook.

Från våra erfarenhet av implementering och distribution av energi begäran prognoser pilot vi har hittat som hello avancerade regression modeller som är tillgängliga i Azure ML tenderar tooproduce hello bäst resultat och vi använder dem.

## <a name="model-evaluation"></a>Modellen utvärdering
Utvärdering av modellen har en viktig roll i hello **modellen utvecklingscykeln**. I det här steget ser vi till att verifierar hello modellen och dess prestanda med verkliga data. Under hello modeling steg använder vi en del av hello tillgängliga data för hello modell. Under utvärderingsfasen för hello vidtar vi hello resten av hello tootest hello datamodellen. Praktiskt taget innebär det att vi mata hello modellen nya data som har varit omstruktureras och innehåller samma funktioner som hello utbildning dataset hello. Dock under hello verifieringsprocessen vi använder hello modellen toopredict hello målvariabel i stället ange hello tillgängliga målvariabel. Det används ofta toothis process som poängsättning av modellen. Vi använder sedan hello true målvärden och jämför dem med hello förutsade viktiga. hello avsikten är toomeasure och minimera hello förutsägelse fel, vilket innebär att hello skillnaden mellan hello förutsägelser och hello true-värde. Kvantifiera hello fel mått är nyckeln eftersom vi skulle som toofine finjustera hello modellen och validera om hello fel faktiskt minskar. Justera hello modell kan göras genom att ändra Modellparametrar som styr hello learning process eller genom att lägga till eller ta bort funktioner (som anges tooas [parametrar Svep](https://channel9.msdn.com/Blogs/Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Praktiskt taget som innebär att vi kan måste tooiterate mellan hello funktionen konstruktion, modellering, och modellen utvärdering faser flera gånger tills det är kan tooreduce hello Felnivån toohello krävs.

Det är viktigt tooemphasis hello förutsägelse fel kommer aldrig att noll eftersom det aldrig är en modell som perfekt förutsäga var resultatet. Det finns dock omfattning fel som kan accepteras av hello företag. Vi vill gärna tooensure att vår modell förutsägelse fel vid hello nivå eller bättre än hello business tolerans nivå under hello verifieringsprocessen. Det är därför viktigt tooset hello andelen hello tillåtna fel hello början av hello växla under hello **problemet formulering** fasen.

### <a name="typical-evaluation-techniques"></a>Utvärdering av vanliga tekniker
Det finns olika sätt i vilka förutsägelse fel kan mätas och möjligt. I det här avsnittet fokuseras hello diskussion utvärdering tekniker relevanta tootime serie och som är specifika för energiförbrukning prognos.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE står för innebär absoluta procentandel fel. Med MAPE vi beräknar hello skillnaden mellan varje prognostiserat punkt och hello faktiska värdet för den punkten. Vi sedan kvantifiera hello fel per punkt genom en beräkning hello andelen hello skillnaden relativa toohello faktiskt värde. I det sista steget genomsnittlig vi dessa värden. hello matematisk formel används för MAPE är hello följande:

![MAPE formeln](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*där A<sub>t</sub> är hello faktiskt värde, F<sub>t</sub> är hello förutsägelsevärdet och n är hello prognos horizon.*

## <a name="deployment"></a>Distribution
När vi har lagt hello modeling fasen och verifiera hello modellen prestanda är vi klara toogo till hello distributionsfasen. I den här kontexten distribution innebär att aktivera hello kunden tooconsume hello modellen genom att köra faktiska förutsägelser på den i stor skala. hello begreppet distribution är nyckeln i Azure ML eftersom vårt huvudmål är tooconstantly anropa förutsägelser som skillnad från toojust hämta hello insikter från hello data. Hej distributionsfasen ingår hello där vi aktivera hello modellen toobe används i stor skala.

Inom hello kontext för energiförbrukning prognos, är vårt mål tooinvoke kontinuerliga och periodiska prognoser samtidigt säkerställa att nya data är tillgängliga för hello modellen och att hello prognostiserat data skickas tillbaka toohello konsumerande klienten.

### <a name="web-services-deployment"></a>Distribution av Web Services
hello huvudsakliga distribuerbara byggblock i Azure ML är hello-webbtjänst. Detta är hello mest effektiva sättet tooenable förbrukning av en förutsägelsemodell i hello molnet. hello webbtjänsten kapslar hello modellen och kapslar in den i en [RESTful](http://www.restapitutorial.com/) API (Application Programming Interface). hello API kan användas som en del av alla klientkod enligt beskrivningen i hello diagrammet nedan.

![Vi Tjänstdistribution och användning](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Som kan ses hello webbtjänsten distribueras i hello Cortana Intelligence Suite molnet och kan anropas via dess exponerade REST API-slutpunkten. Olika typer av klienter i olika domäner kan anropa hello tjänsten via hello Web API samtidigt. hello-webbtjänsten kan även skala för att stödja tusentals samtidiga anrop.

### <a name="a-typical-solution-architecture"></a>En typisk lösningsarkitektur
När du distribuerar en energiförbrukning prognoser lösningen är vi intresserade av att distribuera en tooend lösning som är mer omfattande än hello förutsägelse webbtjänsten och underlättar hello hela dataflöde. För närvarande hello vi anropa en ny prognos, behöver vi toomake till den hello modellen matas med funktioner för aktuella data. Som innebär att hello nyligen insamlade rådata ständigt inhämtas, bearbetas och omvandlas till hello nödvändiga funktioner på vilken hello modell har skapats. AT hello samma tid och vi vill toomake hello prognostiserat data är tillgängliga för hello avslutas konsumerande klienter. Ett exempel data flödet cykel (eller data pipeline) visas i hello diagrammet nedan:

![Energiförbrukning prognos slutet tooEnd dataflöde](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Dessa är hello steg som äger rum som en del av hello energi begäran prognosen cykel:

1. Miljontals distribuerade data mätare som ofta genererar strömförbrukningsdata i realtid.
2. Den här informationen samlas in och överförs till en databas i molnet (*t.ex.*, Azure Blob).
3. Innan den kan bearbetas är hello rådata aggregerade tooa understation eller regionsnivån som definieras av hello företag.
4. hello funktionen bearbetning (se **förberedelse av Data och bearbetning av funktionen**) sedan sker och ger hello som krävs för datamodell utbildning eller bedömningen – hello funktionen uppsättning data lagras i en databas (*t.ex.*, SQL Azure).
5. hello återkommande utbildning tjänsten anropas toore train hello prognoser modellen – som uppdaterad version av modellen hello sparas så att den kan användas av hello poängsättning av webbtjänsten.
6. hello bedömningen webbtjänst anropas enligt ett schema som passar hello krävs prognosen frekvens.
7. hello prognostiserat data lagras i en databas som kan användas av hello slutet förbrukning av klienten.
8. hello förbrukning klienten hämtar hello prognoser, gäller den tillbaka till hello rutnätet och förbrukar i enlighet med hello krävs användningsfall.

Det är viktigt toonote som den här hela livscykel helt automatiserad och körs enligt ett schema. hello hela orchestration av data cykeln kan göras med hjälp av verktyg som [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-tooend-deployment-architecture"></a>Avsluta tooEnd arkitektur för distribution
Distribuera en energi begäran prognosen lösning på Cortana Intelligence i ordning toopractically, behöver vi tooensure att hello nödvändiga komponenter har upprättats och korrekt konfigurerad.

hello illustrerar följande diagram en typisk Cortana Intelligence-baserad arkitektur som implementerar och samordnar hello flödet datacykel som beskrivs ovan:

![Avsluta tooEnd arkitektur för distribution](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Mer information om varje hello komponenter och hello hela arkitektur finns toohello energi Lösningsmall.

