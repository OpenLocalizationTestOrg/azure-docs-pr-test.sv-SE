---
title: Funktionen tekniker och markering i Azure Machine Learning | Microsoft Docs
description: "Beskriver för val av funktioner och tekniker för funktionen och ger exempel på deras roll i data förbättrad process för maskininlärning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 9ceb524d-842e-4f77-9eae-a18e599442d6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2017
ms.author: zhangya;bradsev
ROBOTS: NOINDEX
redirect_url: machine-learning-data-science-create-features
redirect_document_id: TRUE
ms.openlocfilehash: 51a5d8fed492cb9301e048c2b6a721e4573a47d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Funktioner och egenskapsval i Azure Machine Learning
Det här avsnittet beskriver funktionen tekniker och val av funktioner i data förbättrad process för maskininlärning. Det visar de här processerna omfattar med exempel som tillhandahålls av Azure Machine Learning Studio.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Utbildning data som används i machine learning kan ofta förbättras genom markering eller extrahering av funktioner från rådata som samlas in. Ett exempel på en bakåtkompilerade funktion i samband med att lära dig hur du klassificerar avbildningar av handskriven tecken är en bit densitet karta konstrueras från rådata bitars distribution data. Den här kartan kan lättare att hitta kanterna på tecknen effektivare än raw distribution.

Bakåtkompilerade och valda funktioner ökar effektiviteten för utbildning-processen, vilket görs ett försök att extrahera den viktiga informationen i data. De kan också förbättra kraften hos dessa modeller att klassificera indata korrekt och att förutsäga mer robustly resultat av intresse. Funktionen tekniker och val kan också kombinera om du vill göra learning mer beräkningsmässigt tractable. Den gör detta genom att öka och minska antalet funktioner som behövs för att kalibrera eller tränar en modell. Funktioner som markerats för att träna modellen är matematiskt sett en minimal uppsättning oberoende variabler som beskriver mönster i data och förutsäga resultat har.

Tekniker och funktioner är en del av en större process som vanligtvis består av fyra steg:

* Datainsamling
* Förbättring av data
* Modellen konstruktion
* Efterbearbetning

Teknik- och utgör steget data förbättring av maskininlärning. Tre aspekter av den här processen kan särskiljas för våra ändamål:

* **Före databearbetning**: den här processen försöker att säkerställa att insamlade data är ren och konsekvent. Den innehåller uppgifter som att integrera flera datauppsättningar, hantering av data som saknas, hantering inkonsekventa data och konvertering av datatyper.
* **Egenskapsval**: den här processen försöker att skapa ytterligare relevanta funktioner från befintliga raw-funktioner i data och för att öka förutsägande strömmen till Inlärningsalgoritmen.
* **Funktionen val**: den här processen väljer viktiga delmängden av ursprungliga datafunktioner att minska dimensionaliteten för utbildning-problem.

Det här avsnittet beskrivs endast funktionen tekniker och funktionen markeringen aspekter av förbättring av data. Läs mer i steget data före bearbetning [förbearbetning av data i Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).

## <a name="creating-features-from-your-data--feature-engineering"></a>Skapa funktioner från dina data--egenskapsval
Utbildning data består av en matris som består av exempel (poster eller observationer som lagras i rader) som har en uppsättning funktioner (variabler eller fält som lagras i kolumner). Funktioner som anges i experimentets utformning förväntas karaktäriserar mönster i data. Även om många av rådata fälten kan inkluderas direkt i den valda funktionen som angivits för tränar en modell, behöver bakåtkompilerade tilläggsfunktioner ofta konstrueras från funktionerna i rådata för att generera en förbättrad utbildning datamängd.

Vilken typ av funktioner som ska skapas för att förbättra datauppsättningen när en modell? Bakåtkompilerade funktioner som förbättrar utbildning ger information som särskiljer bättre mönster i data. Du förväntar dig att de nya funktionerna som innehåller ytterligare information som inte samlas in tydligt eller enkelt tydligt i funktionsuppsättningen ursprungliga eller en befintlig, men den här processen är något som en bild. Ljud-och produktiva kräver ofta vissa expertis.

När du startar med Azure Machine Learning är det enklast att förstå själva processen concretely med hjälp av exemplen i Machine Learning Studio. Här visas två exempel:

* Ett exempel på regression ([förutsägelse av antalet cykel hyra](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) i ett övervakat experiment där målvärden känt
* Ett text-utvinningsmodellen klassificering exempel använder [hash-funktionen][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Exempel 1: Lägga till temporala funktioner för en regressionsmodell
För att demonstrera hur du utveckla funktioner för en regression aktivitet, skriver du den experiment ”begäran Skapa prognoser för cyklar” i Azure Machine Learning Studio. Syftet med experimentet är att förutsäga behovet av cyklar, det vill säga antalet cykel hyra inom en viss månad, dag eller timme. Datauppsättningen **cykel uthyrning UCI datauppsättning** används som indata rådata.

Den här datauppsättningen baseras på verkliga data från det kapital Bikeshare företag som upprätthåller en cykel uthyrning nätverk i Washington i USA DC. Datamängden som representerar antalet cykel hyra inom en viss timme på dagen, från 2011 till 2012 och innehåller 17379 rader och kolumner som 17. Funktionsuppsättningen rådata innehåller väder (temperatur, fuktighet, vindhastighet) och vilken typ av dagen (helgdag eller veckodag). Fältet för att förutsäga är **cnt**, en uppräkning som representerar cykel hyra inom en viss timme och som sträcker sig från 1 till 977.

För att skapa effektiva funktioner i utbildning data skapas fyra regression modeller med hjälp av samma algoritm, men med fyra olika utbildning datauppsättningar. De fyra datauppsättningarna representerar samma inkommande rådata, men med ett ökande antal funktioner. Dessa funktioner grupperas i fyra kategorier:

1. A = väder helgdag + veckodag + helgens funktioner för förväntade dagen
2. B = antalet cyklar har hyrs i var och en av de föregående 12 timmarna
3. C = antalet cyklar har hyrs i var och en av de senaste 12 dagarna på samma timme
4. D = antal cyklar har hyrs i varje föregående 12 veckor på samma timme och samma dag

Förutom funktionen uppsättning A som redan finns i den ursprungliga rådata, skapas de andra tre olika funktioner via funktionen teknisk process. Funktionsuppsättning B registrerar den senaste begäran för cyklarna. Funktionen Ange C insamlingar behovet av cyklar i en viss timme. D insamlingar begäran för cyklar funktionsuppsättning på särskilda timme och viss dag i veckan. Var och en av de fyra utbildning datauppsättningarna innehåller funktionsuppsättningar A, A + B, A + B + C och A + B + C + D respektive.

Dessa fyra utbildning datauppsättningar bildas i Azure Machine Learning-experiment via fyra grenar från förbearbetade inkommande datauppsättningen. Förutom grenen längst till vänster, var och en av dessa filialer innehåller en [köra R-skriptet] [ execute-r-script] modulen där en uppsättning härledda funktioner (funktion anger B och C D) respektive skapas och läggs till i importerade datauppsättning. Följande bild visar R-skriptet som används för att skapa funktionsuppsättningen B i andra vänstra grenen.

![Skapa en funktionsuppsättning](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

I följande tabell sammanfattas jämförelse av de fyra modellerna prestandaresultat. Bästa resultat visas funktionerna A + B + C. Observera att Felfrekvensen minskar när ytterligare funktioner ingår i utbildning-data. Detta verifierar våra antagandet att funktionsuppsättningar B och C ger ytterligare relevant information för aktiviteten regression. Lägga till funktionsuppsättningen D verkar inte ange några ytterligare minskning i frekvens för fel.

![Jämför prestandaresultat](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Exempel 2: Skapa funktioner i texten datautvinning
Funktionen tekniker används ofta i uppgifter som rör text utvinningsmodellen, till exempel dokumentet klassificering och sentiment analys. Till exempel när du vill klassificera dokument i flera kategorier är vanliga antaganden att ord eller fraser som ingår i en dokumentkategori mindre troligt att ske i en annan dokumenttyp. Frekvensen för distribution word eller en fras kan med andra ord att beskriva olika kategorierna. I texten utvinningsmodellen program behövs funktionen tekniska processen för att skapa funktioner som rör ord eller en fras frekvenser eftersom enskilda delar av texten innehållet fungerar normalt som indata.

För att uppnå den här uppgiften, en teknik som kallas *hash-funktionen* används för att aktivera effektivt godtycklig textfunktioner i index. I stället för att associera varje text-funktion (ord eller fraser) till ett visst index, den här metoden fungerar genom att använda funktionerna för hash-funktion och genom att använda sina hash-värden som index direkt.

I Azure Machine Learning finns en [funktions-hashning] [ feature-hashing] modul som skapar funktionerna ord eller en fras. Följande bild visar ett exempel på hur du använder den här modulen. Uppsättningen inkommande data innehåller två kolumner: book klassificeringen i intervallet 1 till 5 och den faktiska granska innehållet. Syftet med detta [funktions-hashning] [ feature-hashing] modulen är att hämta nya funktioner som visar frekvensen förekomsten av motsvarande ord eller fraser inom särskild granskning. Om du vill använda den här modulen måste du utföra följande steg:

1. Markera den kolumn som innehåller indatatexten (**Col2** i det här exemplet).
2. Ange *Hashing bitsize* -8, vilket innebär att 2 ^ 8 = 256 funktioner har skapats. Ett ord eller en fras i texten sedan hash-kodas till 256 index. Parametern *Hashing bitsize* sträcker sig från 1 till 31. Om parametern har angetts till ett större antal är ord eller fraser mindre troligt att kodas till samma index.
3. Ange parametern *N gram* till 2. Detta hämtar frekvensen förekomsten av unigrams (en funktion för varje ord) och bigrams (en funktion för varje par av intilliggande ord) från indatatexten. Parametern *N gram* ligga mellan 0 och 10, som anger det maximala antalet sekventiella ord som ska ingå i en funktion.  

![Funktionen hash-modul](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Följande bild visar hur dessa nya funktioner som ser ut.

![Funktionen hash-exempel](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Filtrering funktioner från dina data--val av funktioner
*Funktionen val* är en process som används ofta för konstruktion av utbildning datauppsättningar för förutsägelsemodellering åtgärder som klassificerings- eller regressionsmodell uppgifter. Målet är att välja en delmängd av funktionerna i den ursprungliga datauppsättningen som minskar dess storlek genom att använda en minimal uppsättning funktioner som representerar längsta variationen i data. Den här del funktioner i innehåller bara funktionerna ska tas med för att träna modellen. Val av funktioner har två huvudsakliga syften:

* Val av funktioner ökar ofta klassificering noggrannhet genom att ta bort irrelevanta, redundanta eller hög korrelerade funktioner.
* Val av funktioner minskar antalet funktioner, vilket gör att modellen utbildning blir mer effektivt. Detta är särskilt viktigt för inlärning är billigare att träna som support vector datorer.

Även om Funktionsurval syftar till att minska antalet funktioner i den datamängd som används för att träna modellen, det är vanligtvis hänvisar inte till termen *dimensionalitet minskning.* Funktionen val metoder extrahera en delmängd av ursprungliga funktioner i data utan att ändra dem.  Metoder för dimensionalitet minskning av bakåtkompilerade funktioner som kan omvandla de ursprungliga funktionerna och därmed ändra dem. Exempel på dimensionalitet minskning metoder är huvudnamn komponenten analys, kanoniska korrelation analys och enkel värdet uppdelning.

En allmänt tillämpade kategori av funktionen markeringsmetoder i en övervakad kontext är val av filter-baserad funktion. Metoderna gäller ett statistiskt mått om du vill tilldela en poäng varje funktion som utvärderar sambandet mellan varje funktion och Målattributet. Funktionerna för rangordnas sedan poäng, där du kan ange ett tröskelvärde för att hålla eller ta bort en specifik funktion. Exempel på statistiska måtten används i dessa metoder är kvadratvärdet korrelation, ömsesidig information och chi2-testet.

Azure Machine Learning Studio innehåller moduler för val av funktioner. I följande bild visas dessa moduler inkluderar [Filter-baserade Funktionsurval] [ filter-based-feature-selection] och [Fisher linjär Discriminant analys][fisher-linear-discriminant-analysis].

![Funktionen val exempel](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)

Till exempel använda den [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modulen med utvinningsmodellen textexempel beskrivs tidigare. Anta att du vill skapa en regressionsmodell när en uppsättning 256 funktioner har skapats via den [funktions-hashning] [ feature-hashing] modulen, och att variabeln svar är **Kol1** och representerar en bok granska klassificering mellan 1 och 5. Ange **funktion bedömningsprofil metoden** till **kvadratvärdet korrelation**, **målkolumnen** till **Kol1**, och **antalet önskade funktioner** till **50**. Modulen [Filter-baserade Funktionsurval] [ filter-based-feature-selection] producerar en datauppsättning som innehåller 50 funktioner tillsammans med Målattributet **Kol1**. Följande bild visar flödet av experimentet och indataparametrarna.

![Funktionen val exempel](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Följande bild visar de resulterande mängderna data. Varje funktion beräknas baserat på kvadratvärdet sambandet mellan sig själv och Målattributet **Kol1**. Funktioner med högsta poäng behålls.

![Filter-baserad funktion markeringen datauppsättningar](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Följande bild visar motsvarande resultat för de valda funktionerna.

![Valda funktionen resultat](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Genom att använda detta [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modulen, 50 av 256 funktioner är markerade eftersom de har de funktioner som samverkar med en målvariabel **Kol1**baserat på bedömningsprofil metoden **kvadratvärdet korrelation**.

## <a name="conclusion"></a>Slutsats
Funktionen tekniker och Funktionsurval är två steg utförs ofta för att förbereda utbildning data när du skapar en machine learning-modell. Normalt sett funktionen tekniker används först för att generera ytterligare funktioner och sedan funktionen markeringen steg utförs för att ta bort irrelevanta, redundant eller hög korrelerade funktioner.

Det är inte alltid nödvändigtvis att utföra Funktionsurval teknik eller funktion. Om det behövs beror på data som har eller samla in och den algoritm som du väljer mål för experimentet.

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
