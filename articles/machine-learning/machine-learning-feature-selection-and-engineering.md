---
title: aaaFeature tekniker och markering i Azure Machine Learning | Microsoft Docs
description: "Beskriver hello tillämpningen av val av funktioner och tekniker för funktionen och innehåller exempel på deras roll i hello data förbättrad process för maskininlärning."
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
redirect_document_id: True
ms.openlocfilehash: e3e59329bf46f334396f5975b4e656137362d7ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Funktioner och egenskapsval i Azure Machine Learning
Det här avsnittet beskriver hello tillämpningen av funktionen tekniker och val av funktioner i hello data förbättrad process för maskininlärning. Det visar de här processerna omfattar med exempel som tillhandahålls av Azure Machine Learning Studio.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Hej träningsdata som används vid maskininlärning kan ofta förbättras hello markering eller extrahering av funktioner från hello rådata som samlas in. Ett exempel på en bakåtkompilerade funktion i hello kontexten för att lära dig hur tooclassify hello avbildningar av handskriven tecken är en bit densitet karta konstrueras från hello rådata bitars distribution data. Den här kartan kan lättare att hitta hello kanter hello tecken effektivare än hello raw-distribution.

Funktioner för bakåtkompilerade och valda öka hello effektiv hello utbildning process som försöker tooextract hello nyckelinformation i hello data. De även förbättra hello kraften i indata dessa modeller tooclassify hello korrekt och toopredict resultat för intresse mer robustly. Funktionen tekniker och val kan också kombinera toomake hello learning mer beräkningsmässigt tractable. Den gör detta genom att öka och minska hello antal funktioner behövs toocalibrate eller tåg en modell. Matematiskt tala hello funktioner valda tootrain hello modellen är en minimal uppsättning oberoende variabler som förklarar hello mönster i hello data och förutsäga resultat har.

hello tekniker och funktioner är en del av en större process som vanligtvis består av fyra steg:

* Datainsamling
* Förbättring av data
* Modellen konstruktion
* Efterbearbetning

Teknik- och utgör hello data förbättring steg i machine learning. Tre aspekter av den här processen kan särskiljas för våra ändamål:

* **Före databearbetning**: den här processen försöker tooensure som hello insamlade data är ren och konsekvent. Den innehåller uppgifter som att integrera flera datauppsättningar, hantering av data som saknas, hantering inkonsekventa data och konvertering av datatyper.
* **Egenskapsval**: den här processen försöker toocreate ytterligare relevanta funktioner från hello befintliga raw-funktioner i hello data och tooincrease förutsägande power toohello learning algoritm.
* **Funktionen val**: den här processen väljer hello viktiga delmängd av ursprunglig data funktioner tooreduce hello dimensionalitet hello utbildning problemet.

Det här avsnittet beskrivs endast hello funktionen tekniker och funktioner markeringen aspekter av hello förbättring av data. Mer information om hello före bearbetning steget finns [förbearbetning av data i Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).

## <a name="creating-features-from-your-data--feature-engineering"></a>Skapa funktioner från dina data--egenskapsval
Hej träningsdata består av en matris som består av exempel (poster eller observationer som lagras i rader) som har en uppsättning funktioner (variabler eller fält som lagras i kolumner). hello-funktioner som anges i hello experimentets utformning är förväntade toocharacterize hello mönster i hello data. Även om många av hello rådata fält direkt kan ingå i hello valda funktionen som angivits tootrain en modell, måste ytterligare bakåtkompilerade funktioner ofta toobe konstrueras utifrån hello funktioner i hello rådata toogenerate en förbättrad utbildning datamängd.

Vilken typ av funktioner som ska skapas tooenhance hello datauppsättning när en modell? Bakåtkompilerade funktioner som förbättrar hello utbildning ger information som särskiljer hello mönster i hello data bättre. Du förväntar dig hello nya funktioner tooprovide ytterligare information som inte är tydligt avbildade eller enkelt tydligt i hello ursprungliga eller befintliga funktionen, men den här processen är något som en bild. Ljud-och produktiva kräver ofta vissa expertis.

När du börjar med Azure Machine Learning är det enklaste toograsp processen concretely med exempel finns i Machine Learning Studio. Här visas två exempel:

* Ett exempel på regression ([förutsägelse hello antalet cykel hyra](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)) i ett övervakat experiment där hello målvärden känt
* Ett text-utvinningsmodellen klassificering exempel använder [hash-funktionen][feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Exempel 1: Lägga till temporala funktioner för en regressionsmodell
toodemonstrate hur tooengineer funktioner för en regression-aktivitet ska vi använda hello experiment ”begäran prognoser för cyklar” i Azure Machine Learning Studio. hello syftar experimentet toopredict hello begäran för hello cyklar, det vill säga hello antalet cykel hyra inom en viss månad, dag eller timme. hello datauppsättning **cykel uthyrning UCI datauppsättning** används som hello inkommande rådata.

Den här datauppsättningen baseras på verkliga data från hello kapital Bikeshare företag som upprätthåller en cykel uthyrning nätverk i Washington DC i hello USA. hello datauppsättning representerar hello antalet cykel hyra inom en viss timme på dagen, från 2011 too2012 och innehåller 17379 rader och kolumner som 17. hello rådata funktionsuppsättningen innehåller väder (temperatur, fuktighet, vindhastighet) och hello typ av hello dag (helgdag eller veckodag). hello fältet toopredict är **cnt**, en uppräkning som representerar hello cykel hyra inom en viss timme och som sträcker sig från 1 too977.

tooconstruct effektiva funktioner i hello träningsdata, fyra regression modeller har skapats med hjälp av hello samma algoritm men med fyra olika träningsdata anger. hello fyra datauppsättningar representerar hello samma inkommande rådata, men med ett ökande antal funktioner. Dessa funktioner grupperas i fyra kategorier:

1. A = väder helgdag + veckodag + helgens funktioner för hello förväntade dag
2. B = antalet cyklar som fanns i varje hello hyrs tidigare 12 timmar
3. C = antalet cyklar har hyrs i varje hello tidigare 12 dagar vid hello samma timme
4. D = antal cyklar har hyrs i varje hello tidigare 12 veckor vid hello samma timme och hello samma dag

Förutom funktionen uppsättning A som redan finns i hello ursprungliga rådata skapas hello andra tre olika funktioner via hello funktionen teknisk process. Funktionsuppsättning B insamlingar hello senaste begäran för hello cyklar. Funktionen Ange C insamlingar hello begäran för cyklar i en viss timme. D insamlingar begäran för cyklar funktionsuppsättning på särskilda timme och viss veckodag hello. Var och en av hello fyra utbildning datauppsättningar innehåller funktionsuppsättningar A, A + B, A + B + C och A + B + C + D respektive.

Dessa fyra utbildning datauppsättningar bildas i hello Azure Machine Learning-experiment via fyra grenar från hello förbearbetade inkommande data. Förutom hello längst till vänster gren, var och en av dessa filialer innehåller en [köra R-skriptet] [ execute-r-script] modulen där en uppsättning härledda funktioner (funktion anger B och C D) respektive skapas och läggas till toohello importeras datauppsättning. hello följande bild visar hello R-skript används toocreate funktionsuppsättningen B i hello andra vänstra grenen.

![Skapa en funktionsuppsättning](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

hello sammanfattas följande tabell hello jämförelse av hello prestandaresultat hello fyra modeller. hello bäst resultat visas funktionerna A + B + C. Observera att hello Felfrekvens minskar när ytterligare funktioner ingår i hello övningsdata. Detta verifierar våra antagande att hello funktionsuppsättningar B och C ger ytterligare relevant information för hello regression aktivitet. Lägga till hello D funktionsuppsättningen verkar inte tooprovide eventuell ytterligare minskning hello Felfrekvens.

![Jämför prestandaresultat](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Exempel 2: Skapa funktioner i texten datautvinning
Funktionen tekniker används ofta i uppgifter relaterade tootext mining, till exempel dokumentet klassificering och sentiment analys. Till exempel när du vill tooclassify dokument i flera kategorier och är en typisk antagandet att hello ord eller fraser som ingår i ett dokumentkategori mindre troligt toooccur i en annan dokumenttyp. Hello frekvensen av hello ord eller en fras distribution är med andra ord kan toocharacterize olika kategorierna. I texten utvinningsmodellen program är hello-funktionen tekniska processen nödvändiga toocreate hello funktioner som rör ord eller en fras frekvenser eftersom enskilda delar textinnehållet fungera normalt som hello indata.

tooachieve detta uppgift, en teknik som kallas *funktions-hashning* är tillämpade tooefficiently Stäng godtycklig textfunktioner i index. I stället för att associera varje text-funktionen (ord eller fraser) tooa visst index, den här metoden fungerar genom att använda en hash-funktionen toohello funktioner och genom att använda sina hash-värden som index direkt.

I Azure Machine Learning finns en [funktions-hashning] [ feature-hashing] modul som skapar funktionerna ord eller en fras. hello visar följande bild ett exempel på hur du använder den här modulen. hello inkommande data innehåller två kolumner: hello book klassificering från 1 too5 och hello faktiska granskning innehåll. hello målet med detta [funktions-hashning] [ feature-hashing] modulen är tooretrieve nya funktioner som visar hello förekomsten ofta hello motsvarande ord eller fraser inom hello Särskild granskning. toouse denna modul måste toocomplete hello följande steg:

1. Välj hello-kolumn som innehåller hello-indata (**Col2** i det här exemplet).
2. Ange *Hashing bitsize* too8, vilket innebär att 2 ^ 8 = 256 funktioner har skapats. hello eller en fras i hello text är sedan hashas too256 index. Hej parametern *Hashing bitsize* mellan 1 too31. Om hello-parametern anges tooa större tal, hello ord eller fraser som är mindre troligt toobe hash-kodas till hello samma index.
3. Ange hello parametern *N gram* too2. Detta hämtar hello förekomsten ofta unigrams (en funktion för varje ord) och bigrams (en funktion för varje par av intilliggande ord) från hello-indata. Hej parametern *N gram* mellan 0 too10 som visar hello maximalt antal sekventiella ord toobe ingår i en funktion.  

![Funktionen hash-modul](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

hello följande bild visar hur dessa nya funktioner som ser ut.

![Funktionen hash-exempel](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Filtrering funktioner från dina data--val av funktioner
*Funktionen val* är en process som ofta tillämpas toohello konstruktion av utbildning datauppsättningar för förutsägelsemodellering åtgärder som klassificerings- eller regressionsmodell uppgifter. hello målet är tooselect en delmängd av hello funktioner från hello ursprungliga datauppsättningen som minskar dess storlek genom att använda en minimal uppsättning funktioner toorepresent hello maximal mängd varians i hello data. Den här delmängd av funktionerna innehåller hello endast funktioner toobe ingår tootrain hello modellen. Val av funktioner har två huvudsakliga syften:

* Val av funktioner ökar ofta klassificering noggrannhet genom att ta bort irrelevanta, redundanta eller hög korrelerade funktioner.
* Funktionen val minskar hello antal funktioner, vilket gör hello modellen utbildning processen mer effektiv. Detta är särskilt viktigt för inlärning är dyr tootrain som support vector datorer.

Även om Funktionsurval syftar tooreduce hello antal funktioner i hello datauppsättning används tootrain hello modellen, är det inte vanligtvis kallas tooby hello termen *dimensionalitet minskning.* Funktionen val metoder extrahera en delmängd av ursprungliga funktioner i hello data utan att ändra dem.  Metoder för dimensionalitet minskning av bakåtkompilerade funktioner som kan omvandla hello ursprungliga funktioner och därmed ändra dem. Exempel på dimensionalitet minskning metoder är huvudnamn komponenten analys, kanoniska korrelation analys och enkel värdet uppdelning.

En allmänt tillämpade kategori av funktionen markeringsmetoder i en övervakad kontext är val av filter-baserad funktion. Genom att utvärdera hello korrelation mellan varje funktion och hello målattribut gäller metoderna statistiskt mått-tooassign en poäng tooeach funktion. hello funktioner sedan rangordnas hello poäng, som du kan använda tooset hello tröskelvärdet för att hålla eller ta bort en specifik funktion. Exempel på hello statistiskt mått som används i dessa metoder är kvadratvärdet korrelation, ömsesidig information och hello chi2-testet.

Azure Machine Learning Studio innehåller moduler för val av funktioner. I följande bild hello visas dessa moduler innehåller [Filter-baserade Funktionsurval] [ filter-based-feature-selection] och [Fisher linjär Discriminant analys] [ fisher-linear-discriminant-analysis].

![Funktionen val exempel](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)

Till exempel använda hello [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modulen med hello datautvinning textexempel beskrivs tidigare. Anta att du vill toobuild en regressionsmodell när en uppsättning 256 funktioner har skapats via hello [funktions-hashning] [ feature-hashing] modulen och hello svar variabeln är **Kol1**och representerar ett book granska klassificering, allt från 1 too5. Ange **funktion bedömningsprofil metoden** för**kvadratvärdet korrelation**, **målkolumnen** för**Kol1**, och **antalet önskade funktioner** för**50**. hello modulen [Filter-baserade Funktionsurval] [ filter-based-feature-selection] producerar en datauppsättning som innehåller 50 funktioner tillsammans med hello målattribut **Kol1**. hello följande bild visar hello flödet av experimentet och hello indataparametrar.

![Funktionen val exempel](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

hello visar följande bild hello resulterande datauppsättningar. Varje funktion beräknas baserat på hello kvadratvärdet korrelation mellan sig själv och hello målattribut **Kol1**. hello funktioner med högsta poäng behålls.

![Filter-baserad funktion markeringen datauppsättningar](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Följande bild visar hello hello motsvarande resultat av hello valda funktionerna.

![Valda funktionen resultat](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Genom att använda detta [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modulen, 50 av 256 funktioner är markerade eftersom de har de flesta funktioner samverkar med hello målvariabel hello **Kol1** baserat på hello bedömningen metoden **kvadratvärdet korrelation**.

## <a name="conclusion"></a>Slutsats
Funktionen tekniker och Funktionsurval är ofta utförs tooprepare hello utbildningsdata i två steg när du skapar en machine learning-modell. Normalt sett funktionen tekniker är tillämpade första toogenerate ytterligare funktioner och sedan hello funktionen markeringssteget är utförs tooeliminate irrelevanta, redundanta eller hög korrelerade funktioner.

Det är inte alltid nödvändigtvis tooperform teknik eller funktion val av funktioner. Om det behövs beror på hello data du har eller samla in hello-algoritmen som du väljer och hello syftet med hello experiment.

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
