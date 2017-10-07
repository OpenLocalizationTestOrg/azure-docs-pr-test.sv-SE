---
title: aaaFeature teknikerna i datavetenskap | Microsoft Docs
description: "Beskriver hello tillämpningen av funktionen tekniker och innehåller exempel på sin roll i hello förbättring av data för machine learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 3fde69e8-5e7b-49ad-b3fb-ab8ef6503a4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: zhangya;bradsev
ms.openlocfilehash: af40ea9cc9395bc87fe695eeaef26aa71e0ec9e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="feature-engineering-in-data-science"></a>Egenskapsval i datavetenskap
Det här avsnittet beskriver hello tillämpningen av funktionen tekniker och innehåller exempel på sin roll i hello förbättring av data för machine learning. hello exempel används tooillustrate som den här processen hämtas från Azure Machine Learning Studio. 

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Detta **menyn** länkar tootopics som beskriver hur toocreate funktioner för data i olika miljöer. Den här uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Funktionen engineering försök tooincrease hello förutsägande kraften hos learning algoritmer genom att skapa funktioner från rådata som underlättar hello learning processen. Hej tekniker och funktioner är en del av hello TDSP beskrivs i hello [vad är hello Team datavetenskap Process livscykel?](data-science-process-overview.md) Funktionen tekniker och markeringen är delar av hello **utveckla funktioner** steg i hello TDSP. 

* **egenskapsval**: den här processen försöker toocreate ytterligare relevanta funktioner från hello befintliga raw-funktioner i hello data och tooincrease hello förutsägande kraften hos hello Inlärningsalgoritmen.
* **funktionen val**: den här processen väljer hello viktiga delmängd av den ursprungliga datafunktioner i ett försök tooreduce hello dimensionalitet hello utbildning problemet.

Normalt **egenskapsval** är tillämpade första toogenerate ytterligare funktioner och hello **funktion markeringen** steget är utförs tooeliminate irrelevanta, redundant eller hög korrelerade funktioner.

Hej träningsdata som används vid maskininlärning kan ofta förbättras av extrahering av funktioner från hello rådata som samlas in. Ett exempel på en bakåtkompilerade funktion i hello kontexten för att lära dig hur tooclassify hello avbildningar av handskriven tecken är skapandet av lite densitet kartan konstrueras från hello rådata bitars distribution data. Den här kartan kan hjälpa dig att hitta hello kanter hello tecken effektivare än att bara använda hello rådata distribution direkt.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="creating-features-from-your-data---feature-engineering"></a>Skapa funktioner från dina Data - Egenskapsval
Hej träningsdata består av en matris som består av exempel (poster eller observationer som lagras i rader) som har en uppsättning funktioner (variabler eller fält som lagras i kolumner). hello-funktioner som anges i hello experimentets utformning är förväntade toocharacterize hello mönster i hello data. Även om många av hello rådata fält direkt kan ingå i hello valda funktionen som angivits tootrain en modell, är det ofta hello fallet (bakåtkompilerade) tilläggsfunktioner behöver toobe konstrueras utifrån hello funktioner i hello rådata toogenerate en förbättrad utbildning dataset.

Vilken typ av funktioner som ska skapas tooenhance hello dataset när en modell? Bakåtkompilerade funktioner som förbättrar hello utbildning ger information som särskiljer hello mönster i hello data bättre. Vi räknar hello nya funktioner tooprovide ytterligare information som inte är tydligt avbildade eller enkelt tydligt i hello ursprungliga eller en befintlig funktionsuppsättningen. Men den här processen är något som en bild. Ljud-och produktiva kräver ofta vissa expertis.

När du startar med Azure Machine Learning är det enklaste toograsp som den här processen concretely med exempel finns i hello Studio. Här visas två exempel:

* Ett exempel på regression [förutsägelse hello antalet cykel hyra](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4) i ett övervakat experiment där hello målvärden känt
* En text utvinningsmodellen klassificering exempel med [hash-funktionen](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

## <a name="example-1-adding-temporal-features-for-regression-model"></a>Exempel 1: Lägga till Temporala funktioner för regressionsmodell
Nu ska vi använda hello experiment ”begäran Skapa prognoser för cyklar” i Azure Machine Learning Studio toodemonstrate hur tooengineer funktioner för en regression aktivitet. hello syftar experimentet toopredict hello begäran för hello cyklar, det vill säga hello antalet cykel hyra inom en specifik månad/dag/timme. Hej dataset ”cykel uthyrning UCI dataset” används som hello inkommande rådata. Den här datauppsättningen baseras på verkliga data från hello kapital Bikeshare företag som upprätthåller en cykel uthyrning nätverk i Washington DC i hello USA. hello datauppsättning representerar hello antalet cykel hyra inom en viss timme per dag i hello år 2011 och år 2012 och innehåller 17379 rader och kolumner som 17. hello rådata funktionsuppsättningen innehåller väder (temperatur/fuktighet/vindhastighet) och hello typ av hello dag (helgdag/veckodag). hello fältet toopredict är ”cnt”, en uppräkning som representerar hello cykel hyra inom en viss timme och vilket intervall mellan 1 too977.

Med hello målet för att konstruera effektiva funktioner i hello träningsdata, fyra regression modeller är byggda med hello samma algoritm men med fyra olika utbildning datauppsättningar. hello fyra datauppsättningar representerar hello samma inkommande rådata, men med ett ökande antal funktioner. Dessa funktioner grupperas i fyra kategorier:

1. A = väder helgdag + veckodag + helgens funktioner för hello förväntade dag
2. B = antalet cyklar som fanns i varje hello hyrs tidigare 12 timmar
3. C = antalet cyklar har hyrs i varje hello tidigare 12 dagar vid hello samma timme
4. D = antal cyklar har hyrs i varje hello tidigare 12 veckor vid hello samma timme och hello samma dag

Förutom funktionen uppsättning A som redan finns i hello ursprungliga rådata skapas hello andra tre olika funktioner via hello funktionen teknisk process. Funktionsuppsättning B insamlingar mycket färsk begäran för hello cyklar. Funktionen Ange C insamlingar hello begäran för cyklar i en viss timme. D insamlingar begäran för cyklar funktionsuppsättning på särskilda timme och viss veckodag hello. hello fyra utbildning datauppsättningar varje innehåller funktionen uppsättning A, A + B, A + B + C och A + B + C + D.

Dessa fyra utbildning datauppsättningar bildas i hello Azure Machine Learning-experiment, via fyra grenar från hello förbearbetade inkommande dataset. Förutom hello vänster de flesta gren, var och en av dessa filialer innehåller en [köra R-skriptet](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) modul, som är en uppsättning härledda funktioner (funktionsuppsättning B och C D) respektive konstruerade och tillagda toohello importeras dataset. hello följande bild visar hello R-skript används toocreate funktionsuppsättningen B i hello andra vänstra grenen.

![skapa funktioner](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

hello jämförelse av hello prestandaresultat hello fyra modeller sammanfattas i följande tabell hello. hello bäst resultat visas funktionerna A + B + C. Observera att hello fel hastighet minskas när ytterligare funktioner ingår i hello övningsdata. Verifierar våra antagandet att hello funktionsuppsättningen B C ger ytterligare relevant information för hello regression aktivitet. Men att lägga till hello D funktionen verkar inte tooprovide eventuell ytterligare minskning hello Felfrekvens.

![jämförelse av resultat](./media/machine-learning-data-science-create-features/result1.png)

## <a name="example2"></a>Exempel 2: Skapa funktioner i texten datautvinning
Funktionen tekniker används ofta i uppgifter relaterade tootext mining, till exempel dokumentet klassificering och sentiment analys. Till exempel när vi vill tooclassify dokument i flera kategorier är vanliga antaganden att hello word/fraser som ingår i en doc kategori mindre troligt toooccur i en annan doc-kategori. Hello replikeringsfrekvensen hello ord/fraser distribution kan med andra ord kan toocharacterize olika kategorierna. I texten utvinningsmodellen program, eftersom enskilda delar av text-innehållet är vanligtvis hello indata, hello funktionen tekniska processen är nödvändiga toocreate hello funktioner som rör /-fras frekvenser.

tooachieve detta uppgift, en teknik som kallas **funktions-hashning** är tillämpade tooefficiently Stäng godtycklig textfunktioner i index. I stället för att associera varje text-funktionen (ord/fraser) tooa visst index, den här metoden fungerar genom att använda en hash-funktionen toohello funktioner och deras hashvärden som index direkt.

I Azure Machine Learning finns en [funktions-hashning](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modul som skapar dessa /-fras funktioner lätt. Följande bild visar ett exempel på hur du använder den här modulen. hello inkommande datauppsättningen innehåller två kolumner: hello book klassificering från 1 too5 och hello faktiska granska innehållet. hello målet med detta [funktions-hashning](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modulen är tooretrieve flera nya funktioner som visar hello förekomsten ofta hello motsvarande ord / fraser som används inom hello Särskild granskning. toouse denna modul måste toocomplete hello följande steg:

* Välj först hello-kolumn som innehåller hello-indata (”Col2” i det här exemplet).
* Ange andra hello ”Hashing bitsize” too8, vilket innebär att 2 ^ 8 = 256 funktioner kommer att skapas. hello word/fas i alla hello text blir hashformaterats too256 index. hello-parametern ”Hashing bitsize” intervall från 1 too31. Hej ord och fraser används är mindre troligt toobe hash-kodas till hello samma index om den toobe ett större antal.
* Ange det tredje hello parametern ”N gram” too2. Detta får hello förekomsten ofta unigrams (en funktion för varje ord) och bigrams (en funktion för varje par av intilliggande ord) från hello-indata. hello parametern ”N gram” intervall från 0 too10 som visar hello maximalt antal sekventiella ord toobe ingår i en funktion.  

![”Funktion Hashing” modul](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

hello följande bild visar vad hello de här nya funktionen ser ut.

![”Funktion Hashing” exempel](./media/machine-learning-data-science-create-features/feature-Hashing2.png)

## <a name="conclusion"></a>Slutsats
Funktioner för bakåtkompilerade och valda öka hello effektiv hello utbildning process som försöker tooextract hello nyckelinformation i hello data. De även förbättra hello kraften i indata dessa modeller tooclassify hello korrekt och toopredict resultat för intresse mer robustly. Funktionen tekniker och val kan också kombinera toomake hello learning mer beräkningsmässigt tractable. Den gör detta genom att öka och minska hello antal funktioner behövs toocalibrate eller tåg en modell. Matematiskt tala hello funktioner valda tootrain hello modellen är en minimal uppsättning oberoende variabler som förklarar hello mönster i hello data och förutsäga resultat har.

Observera att det inte alltid är tooperform teknik eller funktion val av funktioner. Om det behövs eller inte beror på hello data som vi har eller samla in hello algoritmen vi väljer och hello syftet med hello experiment.

