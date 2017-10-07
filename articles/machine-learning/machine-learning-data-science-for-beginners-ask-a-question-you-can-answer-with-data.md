---
title: "en fråga data kan besvara - aaaAsk datavetenskap problem - Azure Machine Learning | Microsoft Docs"
description: "Lär dig hur tooformulate skarpa datavetenskap fråga i datavetenskap för nybörjare video 3. Innehåller en jämförelse av klassificering och regression frågor."
keywords: "datavetenskap problem, datavetenskap frågor Formulera frågor, regression frågor, klassificering, frågor, skarpa fråga"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: 5b9501e3-9964-417a-8ffc-8913103da77b
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.openlocfilehash: 00c328f51e6d9ff6654b5966eb97d6762582f7e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ask-a-question-you-can-answer-with-data"></a>Ställ en fråga som du kan svara på med data
## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Datavetenskap för nybörjare serien
Lär dig hur tooformulate datavetenskap problem i en fråga i datavetenskap för nybörjare video 3. Den här videon innehåller en jämförelse av frågor för klassificering och regression algoritmer.

tooget hello mesta av hello-serien, titta på alla. [Gå toohello lista över videor](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>Andra videor i den här serien
*Datavetenskap för nybörjare* är en snabb introduktion toodata vetenskap i fem kort video.

* Video 1: [hello 5 frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min. 14 sek)*
* Video 2: [är data som är redo för datavetenskap?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min. 56 sek)*
* Video 3: Ställ en fråga som du kan svara med data
* Video 4: [förutsäga ett svar med en enkel modell](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min. 42 sek)*
* Video 5: [kopiera andras arbete toodo datavetenskap](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sek)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Betyg: Ställ en fråga som du kan svara med data
Välkommen toohello tredje video i hello serien ”datavetenskap för nybörjare”.  

I det här objektet får du några tips för att utforma en fråga som du kan svara med data.

Du kan få ut mer av den här videon om du tittar på hello två tidigare videor i den här serien: ”hello 5 frågor datavetenskap kan besvara” och ”är dina data är klar för datavetenskap”?

## <a name="ask-a-sharp-question"></a>Ställ en fråga som skarpa
Hittills har diskuterats hur datavetenskap är hello process med hjälp av namn (kallas även för kategorier eller etiketter) och siffror toopredict en tooa fråga. Men den kan inte vara valfri fråga. den har toobe en *skarpa fråga.*

En vaga fråga har inte toobe besvaras med ett namn eller en siffra. Måste vara en skarpa fråga.

Anta att du hittat ett magiskt ljus med en genie som sanningsenligt ska besvara varje fråga du har. Men det är en mischievous genie och han prova toomake sitt svar som vag och förvirrande eftersom han kan få direkt med. Du vill toopin honom ned med en fråga lufttätt så att han kan hjälpa men Berätta vad du önskade tooknow.

Om du tooask en vaga fråga som ”vad som händer toohappen med min börs”?, hello genie kan besvara, ”Hej pris ändras”. Utgivarens svaret är, men det är inte mycket användbara.

Men om du hade tooask en skarpa fråga som ”vad kan min börs försäljningspriset nästa vecka”?, hello genie kan inte hjälpa men ger dig en specifik besvara och förutsäga ett försäljningspriset.

## <a name="examples-of-your-answer-target-data"></a>Exempel på ditt svar: målprogram
När du formulerar din fråga Kontrollera toosee om du har exempel hello svar i dina data.

Om vår frågan ”vad min börs försäljningspriset blir nästa vecka”? sedan har vi toomake att våra data innehåller hello kurshistorik.

Om vår frågan är ”vilka bil i min flottan är pågående toofail först”? sedan har vi toomake att våra data innehåller information om föregående fel.

![Målprogram - exempel på ditt svar. Formulera en fråga om vetenskap.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

De här exemplen svar kallas för ett mål. Ett mål är vad vi försöker toopredict om framtida datapunkter, oavsett om det är en kategori eller en siffra.

Om du inte har några måldata, behöver du tooget vissa. Du kommer inte att kunna tooanswer din fråga utan den.

## <a name="reformulate-your-question"></a>Reformulate din fråga
Ibland kan du formulera om din fråga tooget mer användbar svar.

hello frågan ”är den här data punkt A eller B”? beräknar hello kategorinamnet (eller eller etikett) av något. tooanswer, vi använder en *klassificeringsalgoritm*.

Hej frågan ”hur mycket”? eller ”hur många”? beräknar ett belopp. tooanswer den vi använder en *regressionsalgoritm*.

toosee hur vi kan omvandla dessa kan vi titta på hello fråga ”vilka nyheter artikeln är hello mest intressanta toothis reader”? Uppmanas du att ange en förutsägelse av ett val från många möjligheter - med andra ord ”är den här A eller B eller C eller D”? - och använder en klassificeringsalgoritm för.

Men den här frågan kan vara enklare tooanswer om formulera om den som ”hur intressanta är varje artikel på den här listan toothis läsare”? Nu kan du ge varje artikel en numerisk poäng och är det enkelt tooidentify hello flest poäng artikel. Detta är att skriva om av hello klassificering fråga till en regression fråga eller hur mycket?

![Reformulate din fråga. Klassificering fråga kontra regression fråga.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

Hur du ställa en fråga är en ledtråd toowhich algoritm kan du få ett svar.

Du hittar att vissa typer av algoritmer - som hello som i exemplet nyheter artikeln - relaterade. Du kan reformulate din fråga toouse hello-algoritm som ger dig hello mest användbara svar.

Men de viktigaste fråga att skarpa - hello-fråga som du kan svar med data. Och kontrollera att du har hello rätt data tooanswer den.

Hittills har diskuterats vissa grundläggande principer för ställa en fråga som du kan svara med data.

Vara säker på att toocheck ut hello andra videor i ”datavetenskap för nybörjare” från Microsoft Azure Machine Learning.

## <a name="next-steps"></a>Nästa steg
* [Försök med ett första datavetenskap experiment i Machine Learning Studio](machine-learning-create-experiment.md)
* [Få en introduktion tooMachine utbildning i Microsoft Azure](machine-learning-what-is-machine-learning.md)
