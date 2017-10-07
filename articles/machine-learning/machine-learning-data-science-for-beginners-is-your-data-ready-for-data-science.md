---
title: "aaaIs dina data är klara för datavetenskap? -Utvärdering av Azure Machine Learning | Microsoft Docs"
description: "Lär dig hello 4 kriterier för data toobe redo för datavetenskap. Datavetenskap för nybörjare video 2 har konkreta exempel toohelp med grundläggande data utvärderingsversionen."
keywords: "relevanta data utvärdera data, förbereder data, Datakriterier, data som är redo"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: d502062c-da70-4b21-9054-0bfd9902612e
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: ef6bb680ace771537157dbdd50a4ccce0a3eb7ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="is-your-data-ready-for-data-science"></a>Är dina data klara för datavetenskap?
## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Datavetenskap för nybörjare serien
Lär dig hur tooevaluate dina data toomake att den uppfyller grundläggande kriterier toobe redo för datavetenskap.

tooget hello mesta av hello-serien, titta på alla. [Gå toohello lista över videor](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/9/player]
>
>

## <a name="other-videos-in-this-series"></a>Andra videor i den här serien
*Datavetenskap för nybörjare* är en snabb introduktion toodata vetenskap i fem kort video.

* Video 1: [hello 5 frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min. 14 sek)*
* Video 2: Är redo för datavetenskap dina data?
* Video 3: [Ställ en fråga som du kan svara med data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sek)*
* Video 4: [förutsäga ett svar med en enkel modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min. 42 sek)*
* Video 5: [kopiera andras arbete toodo datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sek)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Betyg: Är redo för datavetenskap dina data?
Välkommen för ”är dina data redo för datavetenskap”? Hej andra video i hello serien *datavetenskap för nybörjare*.  

Innan datavetenskap kan ge hello av svar som du vill kan du toogive den vissa hög kvalitet raw material toowork med. Precis som gör en pizza hello hello bättre hello-komponenter du börjar med, bättre hello färdiga produkten. 

## <a name="criteria-for-data"></a>Kriterier för data
Så hello gäller datavetenskap finns det vissa komponenter att vi behöver toopull tillsammans.

Vi behöver data som är:

* Relevanta
* Ansluten
* Korrekt
* Tillräckligt med toowork med

## <a name="is-your-data-relevant"></a>Gäller dina data?
Hello första beståndsdel - måste data som är relevanta.

![Relevanta data kontra irrelevanta data - utvärdera data](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/relevant-and-irrelevant-data.png)

Titta på tabellen hello hello vänster. Vi uppfyllda sju personer utanför Boston staplar, mätt sina blod alkohol nivå, hello Red Sox batting medelvärdet för de senaste spelet och hello pris av i hello närmsta bekvämlighet store.

Det här är alla perfekt legitima data. Endast fel är att den inte är relevanta. Det finns ingen uppenbara relation mellan dessa siffror. Om jag gav du hello aktuella pris mjölk och hello Red Sox batting medelvärde, det finns inget sätt som du kan gissa blod alkohol innehållet.

Titta på hello tabellen nu på hello rätt. Den här gången vi mätt varje person brödtext masslagring och inventerade hello antalet drycker som de har haft.  hello talen i varje rad är nu andra relevanta tooeach. Om jag gav min brödtext massa och hello antal Margaritas som jag har haft, kan du en uppskattning av min blod alkohol innehåll.

## <a name="do-you-have-connected-data"></a>Du har anslutit data?
hello nästa komponent är anslutna data.

![Ansluten data kontra frånkopplade data - Datakriterier data är klar](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/connected-vs-disconnected-data.png)

Här är några relevanta data på hello kvalitet hamburgers: grill temperatur, patty vikt och betyg i hello lokala mat magazine. Men Observera hello luckor i hello tabellen hello vänster.

De flesta datauppsättningar saknas vissa värden. Det är den vanliga toohave hål så här och det finns sätt toowork runtom. Men om det finns för mycket saknas, dina data börjar toolook som ost och Schweiz.

Om du tittar på hello tabell hello vänster finns så mycket data som saknas, är det svårt toocome in med någon typ av relation mellan grill temperatur- och patty vikt. Detta är ett exempel på frånkopplade data.

hello tabellen på hello höger, men är full och slutföra – ett exempel på anslutna data.

## <a name="is-your-data-accurate"></a>Stämmer din data?
hello är nästa beståndsdel vi behöver korrekta. Här är fyra mål som vi vill att toohit med pilarna.

![Korrekta data kontra felaktiga data - data villkor](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/inaccurate-vs-accurate-data.png)

Titta på hello mål i hello övre högra hörnet. Vi har en nära gruppering höger runt hello piltavla. Naturligtvis är korrekt. Oväntat, på hello språk datavetenskap, våra prestanda hello mål höger under det anses också vara korrekt.

Om du toomap ut hello mittpunkt pilarna, skulle du se att det är mycket nära toohello piltavla. hello pilarna sprids ut alla runt hello mål, så att de ska anses vara inexakt, men de är uppbyggd kring hello piltavla så att de ska anses vara korrekt.

Nu titta på hello övre vänstra mål. Vår pilarna nådde här mycket nära varandra en nära gruppering. De är exakt, men de är felaktigt eftersom hello center går ut hello piltavla. Och naturligtvis hello pilarna i hello nedre vänstra mål är både stämmer och inexakt. Den här archer måste flera praxis.

## <a name="do-you-have-enough-data-toowork-with"></a>Har du tillräckligt med data toowork med?
Slutligen beståndsdel #4 – vi behöver toohave tillräckligt med data.

![Har du tillräckligt med data för analys? Utvärdering av](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/barely-enough-data.png)

Tänk på att varje datapunkt i tabellen som en pensellinje i en återgivningen. Om du har några av dem, hello återgivningen kan vara ganska fuzzy - det är svårt tootell vad det är.

Om du lägger till vissa mer penseldrag startar din återgivnings tooget lite skarpare.

När du har knappt tillräckligt med linjer visas bara tillräckligt med toomake bred beslut. Är någonstans I kanske vill toovisit? Det ser ut klara, som ser ut som ren vattenstämplar – Ja, som är där vi på semester.

När du lägger till mer data hello bild blir tydligare och du kan göra mer detaljerad beslut. Nu kan jag se hello tre hotell på hello vänstra bank. Du vet jag gillar hello arkitektur funktioner i hello något i hello förgrunden. Jag kommer det, stanna kvar på hello tredje våning.

Med data som är relevanta, ansluten, korrekt, och tillräckligt, har vi alla hello komponenter toodo måste vissa datavetenskap med hög kvalitet.

Vara säker på att toocheck ut hello andra fyra videor i *datavetenskap för nybörjare* från Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Nästa steg
* [Försök med ett första datavetenskap experiment i Machine Learning Studio](machine-learning-create-experiment.md)
* [Få en introduktion tooMachine utbildning i Microsoft Azure](machine-learning-what-is-machine-learning.md)
