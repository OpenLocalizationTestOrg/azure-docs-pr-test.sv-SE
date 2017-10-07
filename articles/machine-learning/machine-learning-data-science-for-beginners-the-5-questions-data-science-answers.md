---
title: "aaaThe 5 datavetenskap frågor - datavetenskap för nybörjare - Azure Machine Learning | Microsoft Docs"
description: "Datavetenskap Lär för är nybörjare grundläggande koncept i 5 kort video, från och med hello 5 frågor datavetenskap svar. Från Azure Machine Learning."
keywords: "Gör datavetenskap, datavetenskap nybörjare, datavetenskap för nybörjare, datavetenskap grunderna, datavetenskap frågor, datavetenskap video, datavetenskap introduktion"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: a01f93ee-01eb-4afe-abbd-cfa035c119b0
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: 6de0ca22556771a3044520d9ccc6771c3de78771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-for-beginners-video-1-hello-5-questions-data-science-answers"></a>Datavetenskap för nybörjare video 1: hello 5 frågor datavetenskap svar
Få en snabb introduktion toodata vetenskap från *datavetenskap för nybörjare* i fem kort videor från en övre data forskare. Dessa videorna finns grundläggande men användbart om du är intresserad av att göra datavetenskap eller du arbetar med data forskare.

Den här första videon är om hello typer av frågor som kan svara på datavetenskap. tooget hello mesta av hello-serien, titta på alla. [Gå toohello lista över videor](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/8/player]
>
>

## <a name="other-videos-in-this-series"></a>Andra videor i den här serien
*Datavetenskap för nybörjare* är en snabb introduktion toodata vetenskap tar cirka 25 minuter totalt. Checka ut alla fem videor:

* Video 1: hello 5 frågor datavetenskap svar
* Video 2: [är data som är redo för datavetenskap?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min. 56 sek)*
* Video 3: [Ställ en fråga som du kan svara med data](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sek)*
* Video 4: [förutsäga ett svar med en enkel modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min. 42 sek)*
* Video 5: [kopiera andras arbete toodo datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sek)*

## <a name="transcript-hello-5-questions-data-science-answers"></a>Betyg: hello 5 frågor datavetenskap svar
Hej! Välkommen toohello videoserie *datavetenskap för nybörjare*.

Datavetenskap kan det kännas avskräckande, så jag Lär hello grunderna här utan formler eller datorn programming språket.

I den här första videon förklarar vi om ”hello 5 frågor datavetenskap svar”.

Datavetenskap använder siffror och namn (även kallat kategorier eller etiketter) toopredict svar tooquestions.

Det kan Överraska dig, men *finns bara fem frågor som datavetenskap svar*:

* Är den här A eller B?
* Är detta underligt?
* Hur mycket – eller – hur många?
* Hur detta organiseras?
* Vad ska jag göra sedan?

Var och en av de här frågorna har besvarats av en separat familj av machine learning-metoder, kallas algoritmer.

Det är bra toothink om en algoritm som ett recept och data som hello beståndsdelar. En algoritm som visar hur toocombine och blanda hello data i ordning tooget ett svar. Datorer som fungerar som en mixer. De kan utföra de flesta av hello tunga arbetet med hello algoritmen för dig och de gör det ganska snabbt.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Fråga 1: Är den här A eller B? använder klassificering algoritmer
Vi börjar med hello fråga: är den här A eller B?

![Algoritmer för klassificering: är den här A eller B?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/classification-algorithms.png)

Den här algoritmfamiljen kallas tvåklass klassificering.

Det är användbart för alla frågor som har två möjliga svar.

Exempel:

* Den här däck misslyckas i hello nästa 1 000 miles: Ja eller Nej?
* Detta leder i flera kunder: en $5 kuponger eller en 25% rabatt?

Den här frågan kan också vara omformulerad tooinclude mer än två alternativ: är den här A eller B- eller C- eller D, etc.?  Detta kallas för multiklass-baserad klassificering och det är användbart när du har flera – eller flera tusen – möjliga svar. Multiklass-baserad klassificering väljer hello troligen en.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Fråga 2: Är detta underligt? använder algoritmer för identifiering av avvikelseidentifiering
hello nästa fråga datavetenskap kan besvara är: är den här konstigt ut? Den här frågan besvaras av en familj av algoritmer som kallas avvikelseidentifiering.

![Algoritmer för identifiering av avvikelseidentifiering: är den här konstigt ut?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/anomaly-detection-algorithms.png)

Om du har ett kreditkort, har du redan utnyttjat avvikelseidentifiering. Företaget kreditkort analyserar dina köp-mönster, så att de kan meddela dig toopossible bedrägeri. Avgifter som är ”underligt” kan vara ett inköp på ett arkiv där du normalt inte handla eller köpa ett ovanligt dyra objekt.

Den här frågan kan vara användbart i många olika sätt. Exempel:

* Om du har en bil med trycket mätare kanske du vill tooknow: är den här trycket mätaren läsning normal?
* Om du övervaka Hej internet, som du vill ha tooknow: är det här meddelandet från hello vanliga internet?

Avvikelseidentifiering flaggor oväntat eller ovanliga händelser eller beteenden. Den ger ledtrådar där toolook för problem.

## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Fråga 3: Hur mycket? eller hur många? använder regression algoritmer
Maskininlärning kan också förutsäga hello svaret tooHow mycket? eller hur många? hello algoritmen familj som svar på den här frågan kallas regression.

![Regression algoritmer: hur mycket? eller hur många?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/regression-algorithms.png)

Regression algoritmer göra numeriska förutsägelser som:

* Vad ska hello temperatur vara nästa tisdag?  
* Vad är min fjärde kvartal försäljning?

De hjälper besvara varje fråga som ber om ett tal.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Fråga 4: Hur detta organiseras? använder kluster algoritmer
Hello sista två frågor är nu lite mer avancerade.

Ibland du vill toounderstand hello struktur för en datauppsättning – hur organiseras detta? Du har inte exempel som du redan känner till resultat för den här frågan.

Det finns många sätt tootease ut hello strukturen för data. En metod som kluster. Den separerar data till fysiska ”samlar ihop”, för enklare tolkning. Med klustring finns det inget en rätt svar.

![Klustring algoritmer: hur organiseras detta?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/clustering-algorithms.png)

Vanliga exempel klustring frågor är:

* Vilka användare som hello samma typer av filmer?
* Vilka skrivarmodeller misslyckas hello samma sätt?

Förstå hur data ska ordnas du bättre förstå - och förutsäga - beteenden och händelser.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Fråga 5: Vad ska jag göra nu? använder förstärkning learning algoritmer
hello senaste fråga – vad ska jag göra nu? – använder en familj av algoritmer som kallas förstärkning learning.

Förstärkning learning har inspirerat av hur hello hjärna för mus och människor svara toopunishment och fördelar. Dessa algoritmer Läs från resultat och besluta om hello nästa åtgärd.

Normalt passar förstärkning learning bra för automatiserade system som har toomake mycket små beslut utan mänsklig vägledning.

![Förstärkning Learning algoritmer: Vad ska jag göra sedan?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/reinforcement-learning-algorithms.png)

Svarar på frågor är alltid om vilka åtgärder som bör vidtas - vanligtvis av en dator eller en robot. Exempel är:

* Om jag har ett system för ett hus temperatur: justera hello temperatur eller lämna den där det är?  
* Om jag en egen intresseväckande bil: vid ett gult ljus bromsa eller påskynda?  
* För en robot vakuum: Behåll vacuuming eller gå tillbaka toohello debitering station?

Förstärkning learning algoritmer samla in data som de go learning från pröva.

Så som är den - kan hello 5 frågor datavetenskap svara.

## <a name="next-steps"></a>Nästa steg
* [Försök med ett första datavetenskap experiment i Machine Learning Studio](machine-learning-create-experiment.md)
* [Få en introduktion tooMachine utbildning i Microsoft Azure](machine-learning-what-is-machine-learning.md)
