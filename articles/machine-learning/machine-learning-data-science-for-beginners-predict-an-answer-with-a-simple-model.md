---
title: aaaPredict ett svar med en enkel regressionsmodell - Azure Machine Learning | Microsoft Docs
description: "Hur modellen toocreate en enkel regression toopredict ett pris i datavetenskap för nybörjare video 4. Innehåller en linjär regression med måldata."
keywords: "Skapa en modell, enkla modellen, pris förutsägelse, enkla regressionsmodell"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a28f1fab-e2d8-4663-aa7d-ca3530c8b525
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: d4270c2237c33b7e898b78a08b292bc9d62e49ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="predict-an-answer-with-a-simple-model"></a>Förutsäga ett svar med en enkel modell
## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Datavetenskap för nybörjare serien
Lär dig hur toocreate en enkel regression modellen toopredict hello priset på en rombformade i datavetenskap för nybörjare video 4. Vi ska rita en regressionsmodell med måldata.

tooget hello mesta av hello-serien, titta på alla. [Gå toohello lista över videor](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>Andra videor i den här serien
*Datavetenskap för nybörjare* är en snabb introduktion toodata vetenskap i fem kort video.

* Video 1: [hello 5 frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min. 14 sek)*
* Video 2: [är data som är redo för datavetenskap?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min. 56 sek)*
* Video 3: [Ställ en fråga som du kan svara med data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sek)*
* Video 4: Förutsäga ett svar med en enkel modell
* Video 5: [kopiera andras arbete toodo datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sek)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Betyg: Förutsäga ett svar med en enkel modell
Välkommen toohello fjärde video i hello ”datavetenskap för nybörjare” serien. I det här objektet ska vi skapa en enkel modell och göra en förutsägelse.

En *modellen* är en förenklad artikel om våra data. Jag lära dig vad det innebär.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Samla relevant, korrekt, ansluten, tillräckligt med data
Jag vill säga tooshop för en rombformade. Jag har en ring som hörde toomy mor med en inställning för en 1.35 cirkumflex rombformade och jag vill tooget en uppfattning om hur mycket kostar. Jag blir anteckningar, penna till hello smycken arkivet och jag skriva ned hello priset på alla hello stjärnor i hello fallet och hur mycket de väg i carats. Från och med hello första rombformade - 1.01 carats och $7,366.

Nu när jag gå igenom och göra detta för alla hello andra stjärnor i hello store.

![Datakolumner rombformade](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Observera att vår lista har två kolumner. Varje kolumn har ett annat attribut - vikt i carats och pris- och varje rad är en enskild datapunkt som representerar en enskild rombformade.

Vi har skapat en liten faktiskt data här - ange en tabell. Observera att det uppfyller våra kriterier för kvalitet:

* hello data är **relevanta** -vikt är definitivt relaterade tooprice
* Den har **korrekt** -vi dubbelkollat hello priser som vi anteckna
* Den har **anslutna** -det finns inga blanksteg i något av dessa kolumner
* Och som vi ser den har **tillräckligt med** data tooanswer våra fråga

## <a name="ask-a-sharp-question"></a>Ställ en fråga som skarpa
Nu ska vi utgöra våra fråga i skarpa sätt: ”hur mycket kommer det kosta toobuy en 1.35 cirkumflex rombformade”?

Vår lista har en 1.35 cirkumflex rombformade, så måste vi toouse hello resten av våra data tooget en toohello fråga.

## <a name="plot-hello-existing-data"></a>Rita hello befintliga data
hello första vi ska göra är rita en vågrät nummer linje som kallas en axel toochart hello vikter. hello är hello vikterna 0 too2 så att vi ska rita som täcker intervall och placera tick för varje halva cirkumflex.

Nästa vi rita en lodrät axel toorecord hello pris och ansluta den toohello vikt vågrät axel. Detta kan vara i enheter om dollar. Nu har vi en uppsättning koordinaten axlar.

![Vikt- och axlar](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Vi kommer tootake informationen nu är och konverterar den till en *punktdiagram ritytans*. Det här är ett bra sätt toovisualize numeriska datamängder.

Vi eyeball en lodrät linje vid 1.01 carats för hello första datapunkt. Vi eyeball sedan en vågrät linje vid $7,366. Om de uppfyller Rita vi en punkt. Detta representerar vår första rombformade.

Nu vi gå igenom varje rombformade i listan och hello samma sak. När vi via det här är vad vi får: flera punkter, en för varje rombformade.

![Punktdiagram ritytans](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-hello-model-through-hello-data-points"></a>Rita hello modellen via hello datapunkter
Nu om du tittar på hello punkter och squint hello samling ser ut som en fat fuzzy linje. Vi kan ta våra markör och rita rakt igenom den.

Genom att dra en linje vi har skapat en *modellen*. Tänk på detta som tar verkliga hälsningsmeddelande och göra en simplistic tecknad version av den. Nu hello tecknad är fel - hello raden Gå inte igenom alla hello datapunkter. Men det är en användbar förenkling av distribution.

![Regressionslinjen](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

hello faktum att hello prickar inte går exakt genom hello raden är OK. Datavetare förklara detta genom att säga att det finns hello modell - hello rad - och sedan varje punkt har vissa *brus* eller *varians* som är kopplade till den. Det finns hello underliggande perfekt relationen och det finns mer, verkliga hälsningsmeddelande som lägger till brus och osäkerhet.

Eftersom vi försöker tooanswer hello fråga *hur mycket?* detta kallas en *regression*. Och eftersom vi använder rakt, är det en *linjär regression*.

## <a name="use-hello-model-toofind-hello-answer"></a>Hello modellen toofind hello svara
Nu när vi har en modell och vi fråga den våra: hur mycket kostar en 1.35 cirkumflex rombformade?

tooanswer våra fråga vi ögonglobens 1.35 carats och rita en lodrät linje. Om den korsar hello modellen rad eyeball vi en vågrät linje toohello dollar axel. Den träffar höger på 10 000. BOM! Hello svaret är: en 1.35 cirkumflex rombformade kostnader cirka 10 000 $.

![Hitta hello svar på hello modellen](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Skapa en konfidensintervallet
Det är fysiska toowonder exakt hur den här förutsägelse är. Det är användbart tooknow om hello 1.35 cirkumflex rombformade ska vara mycket nära för 10 000 kr eller mycket högre eller lägre. toofigure detta, vi Rita kuvertet runt hello regressionslinjen som innehåller de flesta av hello punkter. Kuvertet kallas vår *konfidensintervallet*: vi ganska säker på att priserna ligger inom det här kuvertet eftersom i hello senaste de flesta av dem har. Vi kan hämta två mer horisontella raderna från där hello 1.35 cirkumflex rad korsar hello ända hello kuvertets.

![Konfidensintervallet](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Nu kan vi säga något om våra konfidensintervallet: vi kan säga säkert sätt att hello priset på en 1.35 cirkumflex rombformade är ungefär $ 10 000 - men det kan vara så lågt som 8 000 och det kan vara så högt som $ 12 000.

## <a name="were-done-with-no-math-or-computers"></a>Vi är klara, utan matematiska eller datorer
Vi gjorde vad datavetare hämta betald toodo och vi gjorde det genom att rita:

* Vi frågade en fråga för att vi kan besvara med data
* Vi har skapat en *modellen* med *linjär regression*
* Vi har gjort en *prediction*, med en *konfidensintervallet*

Och vi använde matematiska eller datorer toodo den.

Nu om vi hade mer information som...

* hello klipp ut av hello rombformade
* färgvariationer (hur nära hello symbol är toobeing vit)
* hello antalet inkluderingar i hello rombformade

.. så vi fick fler kolumner. I så fall blir matematiska användbara. Om du har fler än två kolumner är hårda toodraw punkter i dokumentet. hello matematiska kan du anpassa den raden eller plan tooyour data mycket snyggt.

Även om i stället för bara ett fåtal stjärnor, vi hade två tusen eller två miljoner och sedan göra mycket snabbare som fungerar med en dator.

Idag har hittills diskuterats hur toodo linjär regression, och få en förutsägelse med hjälp av data.

Vara säker på att toocheck ut hello andra videor i ”datavetenskap för nybörjare” från Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Nästa steg
* [Försök med ett första datavetenskap experiment i Machine Learning Studio](machine-learning-create-experiment.md)
* [Få en introduktion tooMachine utbildning i Microsoft Azure](machine-learning-what-is-machine-learning.md)
