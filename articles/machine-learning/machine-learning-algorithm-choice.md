---
title: "aaaHow toochoose maskininlärningsalgoritmer | Microsoft Docs"
description: "Hur toochoose Azure Machine Learning-algoritmer för övervakad och oövervakad inlärning i kluster, klassificerings- eller regressionsmodell experimenten."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a3b23d7f-f083-49c4-b6b1-3911cd69f1b4
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/25/2017
ms.author: garye
ms.openlocfilehash: 367b2278acc2435f27f9d24ead8199db58aca283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-algorithms-for-microsoft-azure-machine-learning"></a>Hur toochoose algoritmer för Microsoft Azure Machine Learning
hello besvara toohello frågan ”vad machine learning algoritmen ska jag använda”? är alltid ”det beror”. Det beror på hello storlek, kvaliteten och uppbyggnad hello data. Det beror på vad du vill toodo med hello svar. Det beror på hur hello matematiska algoritm som hello översattes till instruktioner för hello datorn du använder. Och det beror på hur lång tid som du har. Även hello mest inträffade datavetare går inte att avgöra vilken algoritm utför bäst innan du försöker dem.

## <a name="hello-machine-learning-algorithm-cheat-sheet"></a>hello Machine Learning algoritmen Cheat blad
Hej **Microsoft Azure Machine Learning algoritmen Cheat blad** val hello höger datorn Inlärningsalgoritmen för din förutsägelseanalyslösningar från hello Microsoft Azure Machine Learning-biblioteket med algoritmer.
Den här artikeln guidar dig igenom hur toouse den.

> [!NOTE]
> toodownload hello fusklapp och följ längs med den här artikeln går för[maskin learning algoritmen fusklapp för Microsoft Azure Machine Learning Studio](machine-learning-algorithm-cheat-sheet.md).
> 
> 

Den här fusklapp har en särskild målgrupp i åtanke: en början data forskare med undergraduate nivå machine learning som försöker toochoose en algoritm toostart med i Azure Machine Learning Studio. Det innebär att den gör vissa generaliseringar och oversimplifications, men den pekar du i en säker riktning. Det innebär också att det finns många av algoritmer som inte visas här. När Azure Machine Learning växer tooencompass en fullständig uppsättning tillgängliga metoder kan vi lägger till dem.

De här rekommendationerna är kompilerade feedback och tips från många dataanalytiker och machine learning experter. Vi har inte accepterar allt, men jag försökte tooharmonize våra åsikter i en grov konsensus. De flesta av hello instruktioner avvikelser som börjar med ”det beror...”

### <a name="how-toouse-hello-cheat-sheet"></a>Hur toouse hello cheat blad
Läsa hello sökväg och algoritmen etiketter på hello diagram som ”för  *&lt;sökväg etikett&gt;*, använda  *&lt;algoritmen&gt;*”. Till exempel ”för *hastighet*, använda *två klassen logistic regression*”. Ibland mer än en gren gäller.
Ibland är ingen av dem en perfekt passning. De är avsedda toobe regeln för USB rekommendationer, så oroa dig inte om det är exakt.
Flera datavetare jag pratade med säger att bara till sätt att hitta hello mycket bästa algoritmen är tootry hello alla.

Här är ett exempel från hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) av ett experiment som används för flera algoritmer mot hello samma data och jämför hello resultat: [jämför flera klassen klassificerare: enhetsbokstaven igenkänning ](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

> [!TIP]
> toodownload och skriva ut ett diagram som ger en översikt över hello funktioner i Machine Learning Studio finns [Översiktsdiagram över funktioner i Azure Machine Learning Studio i](machine-learning-studio-overview-diagram.md).
> 
> 

## <a name="flavors-of-machine-learning"></a>Varianter av machine learning
### <a name="supervised"></a>Övervakad
Övervakad inlärning algoritmer göra förutsägelser baserat på en uppsättning exempel. Historiska lager priser kan exempelvis vara används toohazard gissningar i framtida priser. Varje exempel som används för träning är märkt med hello värdet av intresse – i det här fallet hello lager pris. En övervakad inlärningsalgoritm söker efter mönster i dessa värdeetiketter. Det kan använda all information som kan vara relevanta – hello dagen i veckan hello, hello säsongen, hello företagets ekonomiska data, hello typ av bransch, hello förekomst av störande geopolitiska händelser, och varje algoritm ser ut för olika typer av mönster. När hello algoritm har hittat hello bästa mönstret den kan den använder att mönster toomake förutsägelser för namnlösa tester data – framtidens priser.

Övervakad inlärning är ett populärt och praktiskt typ av maskininlärning. Med ett undantag är alla hello moduler i Azure Machine Learning övervakad inlärning algoritmer. Det finns flera specifika typer av övervakad inlärning som representeras i Azure Machine Learning: klassificering, regression och avvikelseidentifiering identifiering.

* **Klassificering**. När hello data som ska använda toopredict en kategori, kallas även övervakad inlärning klassificering. Detta är hello fallet när du tilldelar en avbildning som en bild av en katt eller en hund. När det finns två alternativ, kallas **tvåklass** eller **binomial klassificering**. När det finns flera kategorier som när förutsäga hello vinnaren av hello NCAA mars Madness turnering, problemet kallas **flera klassen klassificering**.
* **Regression**. När ett värde är att förutsade, precis som med lagrets priser, kallas övervakad inlärning regression.
* **Avvikelseidentifiering**. Ibland är hello målet tooidentify datapunkter som är helt enkelt ovanliga. Att upptäcka bedrägerier, till exempel alla sällsynt kreditkort utgiftsgränsen mönstren är tveksam. hello möjliga varianter är så många och hello utbildning exempel så några att det är inte möjligt toolearn vilken bedrägliga aktivitet ser ut. Den metod som tar avvikelseidentifiering är toosimply veta vilken normal aktivitet ser ut som (med en tidigare icke-olagliga transaktioner) och identifiera något som skiljer sig avsevärt.

### <a name="unsupervised"></a>Oövervakad
I oövervakade learning har datapunkterna inga etiketter. I stället inlärning hello målet för en oövervakad algoritmen är att ordna hello data på vissa sätt toodescribe dess struktur. Detta kan innebära att gruppera i kluster eller för att hitta olika sätt att granska komplexa data så att den visas enklare och mer organiserad.

### <a name="reinforcement-learning"></a>Förstärkning learning
I förstärkning learning hämtar hello algoritmen toochoose en åtgärd i svaret tooeach datapunkt. Hej Inlärningsalgoritmen får också en ersättning signal som har en kort stund senare, som anger hur bra hello beslut.
Utifrån detta ändrar hello-algoritmen sin strategi i ordning tooachieve hello högsta ersättning. Det finns inga förstärkning learning algoritmen moduler i Azure Machine Learning. Förstärkning learning är vanligt i robotics där hello uppsättning sensoravläsningar vid en punkt i tiden är en datapunkt och hello algoritmen måste välja hello robot nästa åtgärd. Det är också en fysisk plats för sakernas Internet program.

## <a name="considerations-when-choosing-an-algorithm"></a>Att tänka på när du väljer en algoritm
### <a name="accuracy"></a>Noggrannhet
Hämta hello mest exakta svaret möjliga är inte alltid nödvändigt.
Ibland är en uppskattning lämplig, beroende på vad du vill använda den för. Om så är fallet hello kan du kanske kan toocut din bearbetningstid kraftigt av skulle fastna för fler ungefärliga metoder. En annan fördel med flera ungefärliga metoder är att de naturligt tenderar att undvika [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Utbildning
Hej antal minuter eller timmar nödvändiga tootrain en modell varierar mycket mellan algoritmer. Utbildning tid är ofta beroende av noggrannhet – en vanligtvis medföljer hello andra. Dessutom kan är vissa algoritmer mer känslig toohello antalet datapunkter än andra.
När tiden begränsas hinner hello valet av algoritmen, särskilt när hello datauppsättning är stor.

### <a name="linearity"></a>Linearitet
Många maskininlärningsalgoritmer Se användning av linearitet. Linjär klassificering algoritmer förutsätter att klasser kan avgränsas med rakt (eller dess högre dimension analoga). Dessa inkluderar logistic regression och stöd för vector datorer (som implementeras i Azure Machine Learning).
Linjär regression algoritmer förutsätter att datatrender följer rakt. Dessa förutsättningar är inte bra för vissa problem, men på andra de avslutar noggrannhet.

![Icke-linjära klassen gräns][1]

***Icke-linjära klassen gräns*** *-förlita dig på en linjär klassificeringsalgoritm skulle resultera i sämre Precision*

![Data med en icke-linjär trend][2]

***Data med en icke-linjär trend*** *-metoden linjär regression skulle generera mycket större fel än nödvändigt*

Trots sina farorna är linjär algoritmer mycket populär första raden för angrepp. De tenderar toobe algoritmiskt snabbt och enkelt att träna.

### <a name="number-of-parameters"></a>Antalet parametrar
Parametrarna är hello rattar en data-forskare hämtar tooturn när du ställer in en algoritm. De är siffror som påverkar hello algoritmen beteende, till exempel feltolerans eller antal iterationer eller alternativ mellan varianter av hur hello algoritmen fungerar. hello utbildningstid och korrektheten i hello algoritmen kan ibland vara ganska känsliga toogetting bara hello rätt inställningar. Vanligtvis krävs algoritmer med många parametrar hello mest utvärderingsversion och fel toofind en bra kombination.

Du kan också finns en [parametern omfattande](machine-learning-algorithm-parameters-optimize.md) modulen block i Azure Machine Learning som försöker automatiskt alla parameterkombinationer på oavsett granularitet som du väljer. Även om detta är ett bra sätt toomake att omfattas av hello parametern utrymme, hello tid som krävs för tootrain en modell ökar exponentiellt med hello antal parametrar.

hello är upp och att med många parametrar vanligtvis anger att en algoritm har större flexibilitet. Det kan ofta uppnå mycket bra precision. Du kan hitta hello rätt kombination av parameterinställningar för har angetts.

### <a name="number-of-features"></a>Antal funktioner
För vissa typer av data kan hello antal funktioner vara mycket stora jämfört med toohello antal datapunkter. Detta är ofta hello fallet med genetics eller textdata. hello stort antal funktioner kan bog ned vissa algoritmer för maskininlärning gör utbildning tid unfeasibly lång. Support Vector datorer är särskilt väl lämpade toothis fallet (se nedan).

### <a name="special-cases"></a>Särskilda fall
Vissa algoritmer för maskininlärning göra viss antaganden om hello struktur hello data eller hello önskat resultat. Om du hittar en som passar dina behov, får den du bättre resultat, mer korrekta förutsägelser eller snabbare utbildning.

| **Algoritmen** | **Noggrannhet** | **Utbildning** | **Linearitet** | **Parametrar** | **Anteckningar** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Tvåklass klassificering** | | | | | |
| [logistic regression](https://msdn.microsoft.com/library/azure/dn905994.aspx) | |● |● |5 | |
| [beslut skog](https://msdn.microsoft.com/library/azure/dn906008.aspx) |● |○ | |6 | |
| [beslut Djungel](https://msdn.microsoft.com/library/azure/dn905976.aspx) |● |○ | |6 |Låg minneskrav |
| [tvåklassförhöjda beslutsträdet](https://msdn.microsoft.com/library/azure/dn906025.aspx) |● |○ | |6 |Stora minneskrav |
| [neurala nätverket](https://msdn.microsoft.com/library/azure/dn905947.aspx) |● | | |9 |[Ytterligare anpassning är möjligt](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [Genomsnittlig perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) |○ |○ |● |4 | |
| [support vector machine](https://msdn.microsoft.com/library/azure/dn905835.aspx) | |○ |● |5 |Bra för stora funktionsuppsättningar |
| [lokalt djup support vector machine](https://msdn.microsoft.com/library/azure/dn913070.aspx) |○ | | |8 |Bra för stora funktionsuppsättningar |
| ['Bayes point-dator](https://msdn.microsoft.com/library/azure/dn905930.aspx) | |○ |● |3 | |
| **Flera klassen klassificering** | | | | | |
| [logistic regression](https://msdn.microsoft.com/library/azure/dn905853.aspx) | |● |● |5 | |
| [beslut skog](https://msdn.microsoft.com/library/azure/dn906015.aspx) |● |○ | |6 | |
| [beslut Djungel](https://msdn.microsoft.com/library/azure/dn905963.aspx) |● |○ | |6 |Låg minneskrav |
| [neurala nätverket](https://msdn.microsoft.com/library/azure/dn906030.aspx) |● | | |9 |[Ytterligare anpassning är möjligt](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [ett-v-all](https://msdn.microsoft.com/library/azure/dn905887.aspx) |- |- |- |- |Se egenskaperna för vald hello tvåklass metod |
| **Regression** | | | | | |
| [linjär](https://msdn.microsoft.com/library/azure/dn905978.aspx) | |● |● |4 | |
| [Linjär Bayesian](https://msdn.microsoft.com/library/azure/dn906022.aspx) | |○ |● |2 | |
| [beslut skog](https://msdn.microsoft.com/library/azure/dn905862.aspx) |● |○ | |6 | |
| [tvåklassförhöjda beslutsträdet](https://msdn.microsoft.com/library/azure/dn905801.aspx) |● |○ | |5 |Stora minneskrav |
| [snabb skog quantile](https://msdn.microsoft.com/library/azure/dn913093.aspx) |● |○ | |9 |Distributioner i stället för punkt förutsägelser |
| [neurala nätverket](https://msdn.microsoft.com/library/azure/dn905924.aspx) |● | | |9 |[Ytterligare anpassning är möjligt](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [Poisson](https://msdn.microsoft.com/library/azure/dn905988.aspx) | | |● |5 |Tekniskt sett log-linjär. För att förutsäga antalet |
| [ordningstalet](https://msdn.microsoft.com/library/azure/dn906029.aspx) | | | |0 |För att förutsäga rang ordning |
| **Avvikelseidentifiering** | | | | | |
| [support vector machine](https://msdn.microsoft.com/library/azure/dn913103.aspx) |○ |○ | |2 |Särskilt bra för stora funktionsuppsättningar |
| [PCA-baserad avvikelseidentifiering](https://msdn.microsoft.com/library/azure/dn913102.aspx) | |○ |● |3 | |
| [K-means](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/) | |○ |● |4 |En algoritm för kluster |

**Algoritmen egenskaper:**

**●** -visar hög precision och snabb utbildning gånger hello användning av linearitet

**○** -visar bra Precision och måttlig utbildning gånger

## <a name="algorithm-notes"></a>Algoritmen anteckningar
### <a name="linear-regression"></a>Linjär regression
Som nämnts tidigare [linjär regression](https://msdn.microsoft.com/library/azure/dn905978.aspx) passar en rad (eller plan eller hyperplane) toohello datauppsättning. Det är en bestämmer hög grad, snabbt och enkelt, men det kan vara alltför simplistic för vissa problem.
Här en [linjär regression kursen](machine-learning-linear-regression-in-azure.md).

![Data med en linjär trend][3]

***Data med en linjär trend***

### <a name="logistic-regression"></a>Logistic regression
Även om den innehåller förvirrande 'regression' hello namn, logistic regression är faktiskt ett kraftfullt verktyg för [tvåklass](https://msdn.microsoft.com/library/azure/dn905994.aspx) och [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) klassificering. Det går snabbt och enkelt. Hej faktum att den använder en '-formad kurva i stället för rakt är det en naturlig anpassning för att dela data i grupper. Logistic regression ger linjär klassgränser, så se när du använder den, till en linjär uppskattning är något som du kan live med.

![Logistic regression tootwo klassen data med just en funktion][4]

***En logistic regression tootwo klassen data med ett enda funktionen*** *-klassen gräns är hello punkt vid vilken hello logistic kurvan är precis så nära tooboth klasser*

### <a name="trees-forests-and-jungles"></a>Träd och skogar jungles
Decision skogar ([regression](https://msdn.microsoft.com/library/azure/dn905862.aspx), [tvåklass](https://msdn.microsoft.com/library/azure/dn906008.aspx), och [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), beslut jungles ([tvåklass](https://msdn.microsoft.com/library/azure/dn905976.aspx) och [ multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)), och förstärkta beslutsträd ([regression](https://msdn.microsoft.com/library/azure/dn905801.aspx) och [tvåklass](https://msdn.microsoft.com/library/azure/dn906025.aspx)) baseras på beslutsträd, en grundläggande koncept för maskininlärning. Det finns många varianter av beslutsträd, men alla gör samma sak, dela in hello funktionen utrymme i regioner med mestadels hello samma etikett. Dessa kan vara områden av konsekvent kategori eller konstant värde, beroende på om du gör klassificerings- eller regressionsmodell.

![Beslutsträdet delar upp en funktion utrymme][5]

***Ett beslutsträd delar upp kan funktionen i områden av ungefär uniform värden***

Eftersom funktionen kan kan delas in i godtyckligt små områden, är det enkelt tooimagine att dela den buffertstorleken tillräckligt med toohave en datapunkt per region. Detta är ett ytterst exempel på overfitting. I ordning tooavoid detta en stor mängd träd är konstruerade med matematiska vidtas att hello träd inte korrelerade. hello genomsnittet av den här ”beslut skogen” är ett träd som undviker overfitting. Beslut skogar kan använda mycket minne. Beslutsdjungler är en variant som förbrukar mindre minne på hello bekostnad av en längre utbildningstid.

Förstärkta beslutsträd undvika overfitting genom att begränsa hur många gånger som de kan du dela upp och hur många datapunkter är tillåtna i varje region. Algoritmen skapar en sekvens med träd, där varje lär sig att kompensera för hello-fel som har lämnat hello trädet innan. hello resultatet är en mycket noggrann deltagaren som tenderar toouse mycket minne. Hello fullständiga tekniska beskrivning, kolla [Friedmans ursprungliga dokumentet](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Snabb skog quantile regression](https://msdn.microsoft.com/library/azure/dn913093.aspx) är en variation av beslutsträd för hello specialfall där du vill veta inte bara hello vanliga () medianvärdet för hello data i en region men motsvarande distribution i hello form av quantiles.

### <a name="neural-networks-and-perceptrons"></a>Neurala nätverk och perceptrons
Neurala nätverk är hjärna-inspirerat learning algoritmer som täcker [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), [tvåklass](https://msdn.microsoft.com/library/azure/dn905947.aspx), och [regression](https://msdn.microsoft.com/library/azure/dn905924.aspx) problem. De kommer i en rad, men hello neurala nätverk i Azure Machine Learning är alla hello form av riktat acykliskt diagram. Det innebär att inkommande funktioner skickas framåt (aldrig bakåt) via en sekvens av lager innan aktiveras i utdata. I varje lager viktas indata i olika kombinationer summeras och skickas till hello nästa lager. Den här kombinationen av enkla beräkningar resulterar i möjlighet toolearn sofistikerade klassen gränser och data trender till synes av Magiskt tal. Många lager nätverk för den här typen utföra hello ”djup learning” som bränslen så mycket rapportering av teknisk och vetenskaplig fiktion.

Den här högpresterande finns inte gratis men. Neurala nätverk kan ta en lång tid tootrain, särskilt för stora datamängder med många funktioner. De kan också ha fler parametrar än de flesta algoritmer, vilket innebär att parametern omfattande expanderar hello utbildningstid en hel del.
Och för de overachievers som önskar för[ange sina egna nätverksstrukturen](http://go.microsoft.com/fwlink/?LinkId=402867), du kan använda inexhaustible.

![Gränser som upptäckts av neurala nätverk][6]
***hello gränser som upptäckts av neurala nätverk kan vara komplicerat och oregelbundna***

Hej [tvåklass var i genomsnitt perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) neurala nätverk svaret tooskyrocketing utbildning gånger. Den använder en nätverksinfrastruktur som ger linjär klassgränser. Det är nästan primitiva dagens standarder, men den har en lång historik över robustly fungerar och är tillräckligt små toolearn snabbt.

### <a name="svms"></a>SVMs
Support vector datorer (SVMs) hitta hello gräns som avgränsar klasser av som bred en marginal som möjligt. När hello två klasser inte kan vara klart åtskilda, hitta hello algoritmer hello bästa gräns som de kan. Som skrivits i Azure Machine Learning hello [tvåklass SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) gör detta med en rak linje. (I SVM tala används en linjär kernel.) Eftersom det gör det här linjär uppskattning är kan toorun ganska snabbt. Där det sin verkliga styrka är med funktionen intensiva data, t.ex. text eller genom. I dessa fall är SVMs kan tooseparate klasser snabbare och med mindre overfitting än de flesta andra algoritmer dessutom toorequiring bara en liten mängd minne.

![Support vector machine klassen gräns][7]

***En typisk support vector machine klass gräns maximerar hello marginal avgränsa två klasser***

En annan produkt av Microsoft Research hello [tvåklass lokalt djup SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) är en icke-linjär variant av SVM som behåller de flesta av hello hastighet och minne effektivitet hello linjär version. Det är perfekt för fall där hello linjär metod inte får tillräckligt aktuell svar. hello utvecklare sparas det snabbt genom att bryta ned hello problem i flera mindre linjär SVM problem. Läs hello [fullständig beskrivning](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) hello information om hur de hämtas av den här lura.

Använder ett smarta tillägg av linjära SVMs hello [en klass SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) ritar en gräns som nära visar hello hela datauppsättningen. Det är användbart för identifiering av avvikelse. Alla nya datapunkter som långt hamnar utanför gränsen är tillräckligt ovanliga toobe anmärkningsvärda.

### <a name="bayesian-methods"></a>Bayesian metoder
Bayesian metoder har en hög önskvärt kvalitet: de undvika overfitting. De kan göra detta genom att göra några antaganden i förväg om hello sannolikt distribution hello svar. En annan byproduct med den här metoden är att de har mycket några parametrar. Azure Machine Learning har båda Bayesian algoritmer för båda klassificering ([Two-class Bayes point-dator](https://msdn.microsoft.com/library/azure/dn905930.aspx)) och regression ([Bayesian linjär regression](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Observera att dessa förutsätter att hello data kan dela eller ryms inom rakt.

På en historisk anteckning utvecklades Bayes' point datorer Microsoft Research. De har vissa undantagsfall snygg teoretisk arbete bakomliggande. hello berörda student är riktat toohello [ursprungliga artikeln i JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) och en [insiktsfulla blogg av Chris Bishop](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Särskilda algoritmer
Om du har en särskild målet kanske tur. Hello Azure Machine Learning samlingen består av algoritmer som specialiserade på:

- rangordnas förutsägelse ([ordningstal regression](https://msdn.microsoft.com/library/azure/dn906029.aspx)),
- Räkna förutsägelse ([Poisson regression](https://msdn.microsoft.com/library/azure/dn905988.aspx)),
- avvikelseidentifiering (en baserat på [huvudkomponenter analysis](https://msdn.microsoft.com/library/azure/dn913102.aspx) och baserat på ett [support vector machine](https://msdn.microsoft.com/library/azure/dn913103.aspx)s)
- kluster ([K-means](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/))

![PCA-baserad avvikelseidentifiering][8]

***PCA-baserad avvikelseidentifiering*** *-hello majoriteten av hello data hamnar i en stereotypical distributionsplats; punkter kraftigt avviker från att distributionsplatsen är tveksam*

![Datauppsättning grupperas med K-means][9]

***En datauppsättning är grupperade i fem kluster med hjälp av K-means***

Det finns också en ensemble [en v alla multiklass-baserad klassificerare](https://msdn.microsoft.com/library/azure/dn905887.aspx), vilka problem radbrytningar hello N-klass klassificering N-1 tvåklass klassificering problem. hello noggrannhet, utbildning och linearitet egenskaper bestäms av hello tvåklass klassificerare används.

![Två-klass, klassificerare kombineras tooform en klassificerare tre-klass][10]

***Ett par med två-klass, klassificerare kombinera tooform en klassificerare tre-klass***

Azure Machine Learning har även åtkomst tooa kraftfulla maskininlärning framework under hello rubrik [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
VW lekfull kategorisering här, eftersom den kan lära dig klassificerings- och regression problem och kan även lära sig från delvis omärkta data. Du kan konfigurera den toouse någon av ett antal learning, förlust funktioner och optimering algoritmer. Den utformades från hello bakgrund in toobe effektivt parallellt och mycket snabbt. Den hanterar funktionen löjligt stora mängder med mycket tydligt ansträngning.
Igång och lett av Microsoft Researchs egna John Langford är VW en formel en post i ett fält av börs bil algoritmer. Inte alla problem som passar VW men om den det kan vara värt att din stund tooclimb inlärningskurvan på dess gränssnitt. Det är också tillgängliga som [fristående öppen källkod](https://github.com/JohnLangford/vowpal_wabbit) på flera språk.

## <a name="more-help-with-algorithms"></a>Mer hjälp med algoritmer
* En nedladdningsbar infographic som beskriver algoritmer och exempel finns [nedladdningsbara Infographic: maskin lär du dig grunderna med algoritmen exempel](machine-learning-basics-infographic-with-algorithm-examples.md).
* En lista efter kategori av alla hello maskininlärningsalgoritmer tillgängliga i Azure Machine Learning Studio finns [initiera modell] [ initialize-model] i modulen hjälpen och hello Machine Learning Studio-algoritmen.
* En fullständig alfabetisk lista över algoritmer och moduler i Azure Machine Learning Studio finns [A-Ö-listan över Machine Learning Studio moduler] [ a-z-list] i Machine Learning Studio algoritmen och hjälpa till att modulen.
* toodownload och skriva ut ett diagram som ger en översikt över hello funktioner i Azure Machine Learning Studio finns [Översiktsdiagram över funktioner i Azure Machine Learning Studio i](machine-learning-studio-overview-diagram.md).


<!-- Reference links -->
[initialize-model]: https://msdn.microsoft.com/library/azure/dn905812.aspx
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx

<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
