---
title: aaaA enkelt experiment i Machine Learning Studio | Microsoft Docs
description: "Den här självstudien om Machine Learning vägleder dig genom ett enkelt dataexperiment. Vi ska förutsäga hello priset på en bil med hjälp av en regressionsalgoritm."
keywords: "experiment, linjär regression,machine learning algoritmer, machine learning självstudier, teknik för förutsägbar modellering, dataexperiment"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: fb215851d380acf7d0f4934a426283369f9c4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Självstudie om Machine Learning: Skapa ditt första dataexperiment i Azure Machine Learning Studio

Om du aldrig har använt **Azure Machine Learning Studio** tidigare är den här självstudien något för dig.

I den här självstudiekursen kommer vi går igenom hur toouse Studio för hello första gången toocreate ett maskininlärningsexperiment. hello experiment testar en analytiska modell som beräknar hello priset på en bil utifrån olika variabler som märke och tekniska specifikationer.

> [!NOTE]
> Den här kursen visar hello grunderna för hur toodrag och släpp-moduler på experimentet, ansluta dem till varandra, köra hello experiment och titta på hello resultat. Vi kommer inte toodiscuss hello allmänt avsnitt av maskininlärning eller hur tooselect och använda hello 100 + inbyggda algoritmer och data manipulation moduler som ingår i Studio.
>
>Om du är ny toomachine learning hello videoserie [datavetenskap för nybörjare](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) kan vara en bra toostart. Den här videon serien är en bra överblick toomachine learning med vanliga ord och begrepp.
>
>Om du är bekant med machine learning, men du letar efter mer allmän information om Machine Learning Studio och hello maskininlärningsalgoritmer som den innehåller, följer nedan några bra resurser:
>
- [Vad är Machine Learning Studio?](machine-learning-what-is-ml-studio.md) – Detta är en översikt över Studio på hög nivå.
- [Maskin lär du dig grunderna med algoritmen exempel](machine-learning-basics-infographic-with-algorithm-examples.md) -den här infographic är användbart om du vill toolearn mer om hello olika typer av maskininlärningsalgoritmer som ingår i Machine Learning Studio.
- [Datorn Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) -den här handboken innehåller liknande information som hello infographic ovan, men ett interaktivt format.
- [Maskin learning algoritmen fusklapp](machine-learning-algorithm-cheat-sheet.md) och [hur toochoose algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) -den här nedladdningsbara affischer och tillhörande artikel diskutera hello Studio algoritmer i mer detalj.
- [Machine Learning Studio: Algoritmen och modulen hjälpa](https://msdn.microsoft.com/library/azure/dn905974.aspx) -detta är hello fullständiga referens för alla Studio-moduler, inklusive maskininlärningsalgoritmer

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Hur kan Machine Learning Studio hjälpa?

Machine Learning Studio gör det enkelt tooset upp ett experiment med dra och släpp-moduler förprogrammerade med tekniker för förutsägelsemodellering.

Med en interaktiv, visuell arbetsyta kan du dra och släppa ***datauppsättningar*** och analysera ***moduler*** till en interaktiv arbetsyta. Du ansluter dem tillsammans tooform en ***experimentera*** som du kör i Machine Learning Studio.
Du ***skapa en modell***, ***träna modellen hello***, och ***poängsätta och testa hello modellen***.

Du kan iterera med modelldesignen, redigera hello experiment och kör tills den ger du hello resultat som du letar efter. När din modell är klar kan du publicera den som en ***webbtjänst*** så andra kan skicka nya data till den och få förutsägelser i utbyte.

## <a name="open-machine-learning-studio"></a>Öppna Machine Learning Studio

tooget igång med Studio, gå för[https://studio.azureml.net](https://studio.azureml.net). Om du har loggat in på Machine Learning Studio förut klickar du på **Logga in**. Annars klickar du på **Registrera dig här** och väljer mellan den kostnadsfria versionen och betalversionen.

![Logga in tooMachine Learning Studio][sign-in-to-studio]
<br/>
***Logga in tooMachine Learning Studio***

## <a name="five-steps-toocreate-an-experiment"></a>Fem steg toocreate ett experiment

I den här självstudien om maskininlärning ska du följa fem grundläggande steg toobuild ett experiment i Machine Learning Studio toocreate, tåg och och betygsätta din modell:

- **Skapa en modell**
    - [Steg 1: Hämta data]
    - [Steg 2: Förbered hello data]
    - [Steg 3: Definiera funktioner]
- **Hej träningsmodell**
    - [Steg 4: Välja och tillämpa en inlärningsalgoritm]
- **Poängsätta och testa hello modellen**
    - [Steg 5: Förutsäga nya bilpriser]

[Steg 1: Hämta data]: #step-1-get-data
[Steg 2: Förbered hello data]: #step-2-prepare-the-data
[Steg 3: Definiera funktioner]: #step-3-define-features
[Steg 4: Välja och tillämpa en inlärningsalgoritm]: #step-4-choose-and-apply-a-learning-algorithm
[Steg 5: Förutsäga nya bilpriser]: #step-5-predict-new-automobile-prices

> [!TIP] 
> Du hittar en fungerande kopia av hello följande experiment i hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com). Gå för**[din första datavetenskap experimentera - förutsägelse av bilpriser](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**  och på **öppna i Studio** toodownload en kopia av hello experiment i Machine Learning Studio arbetsyta.


## <a name="step-1-get-data"></a>Steg 1: Hämta data

hello måste du först tooperform maskininlärning är data.
Du kan använda någon av flera exempeluppsättningar med data som ingår i Machine Learning Studio, eller så kan du importera data från flera källor. I det här exemplet använder vi hello exempeluppsättningen **Automobile price data (Raw)**, som ingår i din arbetsyta.
Den här datauppsättningen innehåller poster för ett antal olika bilar, inklusive uppgifter om modell, tekniska specifikationer och pris.

Här är hur tooget hello dataset i experimentet.

1. Skapa ett nytt experiment genom att klicka på **+ ny** längst hello hello Machine Learning Studio-fönstret, Välj **EXPERIMENTERA**, och välj sedan **tomt Experiment**.

2. hello experiment ges ett standardnamn som du kan se hello överst i hello arbetsytan. Välj den här texten och Byt namn på den toosomething beskrivande, till exempel **förutsägelse av bilpriser**. hello namnet behöver inte toobe unikt.

    ![Byt namn på hello experiment][rename-experiment]

2. toohello till vänster i hello experimentet finns en palett med datauppsättningar och moduler. Typen **bil** i hello sökrutan överst hello i den här paletten toofind hello datauppsättningen **Automobile price data (Raw)**. Dra den här datauppsättningen toohello för experimentet.

    ![Hitta hello bildata och drar den till hello experimentet][type-automobile]
    <br/>
    ***Hitta hello bildata och drar den till hello experimentet***

toosee vad den här informationen ser ut klickar du på hello utdataporten längst ned hello hello bildata och väljer sedan **visualisera**.

![Klicka på utdataporten hello och välj ”visualisera”][select-visualize]
<br/>
***Klicka på utdataporten hello och välj ”visualisera”***

> [!TIP]
> Datauppsättningar och moduler har indata och utdata-portar som representeras av cirklarna - portar hello överst utdata portar längst ned hello.
toocreate en flödet av data via experimentet, ansluter du en utdataporten för en modul tooan indataport på en annan.
När som helst klicka hello utdataporten för ett dataset eller modulen toosee vilka hello data ser ut då i hello dataflöde.

Varje instans av en bil visas som en rad i den här exempeluppsättningen och hello variabler som är associerade med varje bil visas som kolumner. Hello variabler för en specifik bil får vi tootry toopredict hello pris i kolumnen längst till höger (kolumn 26 med rubriken ”price”).

![Visa hello bil data i visualiseringsfönstret för hello data][visualize-auto-data]
<br/>
***Visa hello bil data i visualiseringsfönstret för hello data***

Stäng hello visualiseringsfönstret genom att klicka på hello ”**x**” i hello övre högra hörnet.

## <a name="step-2-prepare-hello-data"></a>Steg 2: Förbered hello data

En datauppsättning kräver vanligtvis viss bearbetning i förväg innan den kan analyseras. Du kan till exempel har märkt hello saknar värden finns i hello kolumnerna på olika rader. Dessa värden som saknas måste toobe rensad så hello modellen kan analysera hello data korrekt. I vårt fall ska vi ta bort alla rader som har värden som saknas. Dessutom hello **normalized-losses** kolumn har en stor del av värden som saknas, så vi ska utesluta kolumnen från hello modellen helt och hållet.

> [!TIP]
> Rengöringsband hello saknar värden från indata är en förutsättning för att använda de flesta av hello moduler.

Först lägger vi till en modul som tar bort hello **normalized-losses** kolumnen helt, och vi lägger till en annan modul som tar bort rader med data som saknas.

1. Typen **Markera kolumner** i hello sökrutan överst hello i hello modulen paletten toofind hello [Välj kolumner i datauppsättning] [ select-columns] modulen, drar den toohello experimentet . Den här modulen kan vi tooselect vilka kolumner med data som vi vill tooinclude eller utelämna i hello modellen.

2. Ansluta hello utdataporten för hello **Automobile price data (Raw)** dataset toohello inkommande port för hello [Välj kolumner i datauppsättning] [ select-columns] modul.

    ![Lägg till hello ”Välj kolumner i datauppsättning” modulen toohello för experimentet och koppla den][type-select-columns]
    <br/>
    ***Lägg till hello ”Välj kolumner i datauppsättning” modulen toohello för experimentet och koppla den***

3. Klicka på hello [Välj kolumner i datauppsättning] [ select-columns] och klicka på **starta kolumnväljaren** i hello **egenskaper** fönstret.

    - Klicka på vänster hello **med regler**
    - Under **Börjar med** klickar du på **Alla kolumner**. Detta uppmanar [Välj kolumner i datauppsättning] [ select-columns] toopass alla hello kolumner (utom de kolumner som vi om tooexclude).
    - Välj hello-listrutor **undanta** och **kolumnnamn**, och klicka sedan i textrutan för hello. En lista med kolumner visas. Välj **normalized-losses**, och det är extra toohello textruta.
    - Klicka på hello bockmarkeringen (OK) knappen tooclose hello kolumnväljaren (på hello längst ned till höger).

    ![Starta kolumnväljaren hello och utesluta hello ”normalized-losses” kolumn][launch-column-selector]
    <br/>
    ***Starta kolumnväljaren hello och utesluta hello ”normalized-losses” kolumn***

    Nu hello egenskapsrutan för **Välj kolumner i datauppsättning** anger att den släpps igenom alla kolumner från hello datamängd utom **normalized-losses**.

    ![hello egenskapsrutan visar hello ”normalized-losses” kolumnen utesluts][showing-excluded-column]
    <br/>
    ***hello egenskapsrutan visar hello ”normalized-losses” kolumnen utesluts***

    > [!TIP]
    Du kan lägga till en kommentar tooa modul genom att dubbelklicka på hello-modulen och skriva in text. Detta kan hjälpa dig en överblick över vilka hello-modulen gör i experimentet. I det här fallet dubbelklickar du på hello [Välj kolumner i datauppsättning] [ select-columns] modulen och Skriv hello kommentaren ”exkludera normalized förluster”.
    >
    >


    ![Dubbelklicka på modulen-tooadd en kommentar][add-comment]
    <br/>
    ***Dubbelklicka på modulen-tooadd en kommentar***

3. Dra hello [Rensa Data som saknas] [ clean-missing-data] modulen toohello experimentera arbetsytan och ansluta den toohello [Välj kolumner i datauppsättning] [ select-columns] modul. I hello **egenskaper** väljer **ta bort hela raden** under **Rensningsläge**. Detta uppmanar [Rensa Data som saknas] [ clean-missing-data] tooclean hello data genom att ta bort rader som har alla värden som saknas. Dubbelklicka hello modulen och Skriv hello kommentaren ”ta bort rader med värden som saknas”.

    ![Ange hello rengöringsband läge för ”ta bort hela raden” för modulen för hello ”Rensa Data som saknas”][set-remove-entire-row]
    <br/>
    ***Ange hello rengöringsband läge för ”ta bort hela raden” för modulen för hello ”Rensa Data som saknas”***

4. Kör hello experiment genom att klicka på **kör** på hello hello sidans nederkant.

    När hello experimentet har körts med alla hello-moduler en grön bock tooindicate som de har slutförts. Observera också hello **slutförts** status i hello övre högra hörnet.

![När den har körts, hello experimentet bör se ut ungefär så här][early-experiment-run]
<br/>
***När den har körts, hello experimentet bör se ut ungefär så här***

> [!TIP]
> Varför körde vi hello experiment nu? Genom att köra hello experiment hello kolumndefinitionerna för våra data skickas från hello datauppsättningen genom hello [Välj kolumner i datauppsättning] [ select-columns] modulen, och via hello [Rensa Data som saknas] [ clean-missing-data] modul. Detta innebär att alla moduler som vi ansluta för[Rensa Data som saknas] [ clean-missing-data] måste även ha samma information.

Allt vi har gjort i hello experiment in toothis punkt är ren hello data. Om du vill tooview hello rensade datauppsättningen klickar du på hello kvar utdataporten för hello [Rensa Data som saknas] [ clean-missing-data] modulen och välj **visualisera**. Observera att hello **normalized-losses** kolumn ingår inte längre och det inte finns några värden som saknas.

Nu när hello data är ren, vi redo toospecify vilka funktioner som vi ska toouse i hello förutsägelsemodell.

## <a name="step-3-define-features"></a>Steg 3: Definiera funktioner

Inom Machine Learning är *funktioner* enskilda mätbara egenskaper av något du är intresserad av. I vår datauppsättning representerar varje rad en bil och varje kolumn är en funktion i den bilen.

Hitta en bra uppsättning funktioner för att skapa en förutsägelsemodell kräver del experimenterande och kunskap om hello problem som du söker toosolve. Vissa funktioner är bättre för att förutsäga hello målet än andra. Dessutom kan vissa funktioner ha en stark korrelation med andra funktioner och går att ta bort. Till exempel är city-mpg och highway-mpg nära relaterade så att vi kan behålla en och ta bort hello andra utan att avsevärt påverka hello förutsägelse.

Vi ska skapa en modell som använder en delmängd av hello funktioner i vår datauppsättning. Du kan gå tillbaka senare och välja andra funktioner, kör hello experimentet igen och se om du får bättre resultat. Toostart, men vi försöker hello följande funktioner:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Dra en [Välj kolumner i datauppsättning] [ select-columns] modulen toohello experimentera arbetsytan. Ansluta hello kvar utdataporten för hello [Rensa Data som saknas] [ clean-missing-data] toohello modulindata av hello [Välj kolumner i datauppsättning] [ select-columns] modul.

    ![Ansluta hello ”Välj kolumner i datauppsättning” modulen toohello ”Rensa Data som saknas” modul][connect-clean-to-select]
    <br/>
    ***Ansluta hello ”Välj kolumner i datauppsättning” modulen toohello ”Rensa Data som saknas” modul***

2. Dubbelklicka hello modulen och Skriv ”Välj funktioner för förutsägelse”.

2. Klicka på **starta kolumnväljaren** i hello **egenskaper** fönstret.

3. Klicka på **Med regler**.

4. Under **Börjar med** klickar du på **Inga kolumner**. Välj hello filterraden **inkludera** och **kolumnnamn** och välj vår lista med kolumnnamn i hello textruta. Detta uppmanar hello modulen toonot släpp igenom alla kolumner (funktioner) förutom hello som vi anger.

5. Klicka hello bockmarkeringen (OK).

    ![Välj hello kolumner (funktioner) tooinclude i hello förutsägelse][select-columns-to-include]
    <br/>
    ***Välj hello kolumner (funktioner) tooinclude i hello förutsägelse***

Detta ger en filtrerad datamängd som innehåller endast hello-funktioner som vi vill toopass toohello learning algoritmen vi använder i hello nästa steg. Du kan komma tillbaka senare och försöka igen med andra funktioner.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Steg 4: Välja och tillämpa en inlärningsalgoritm

Nu när hello data är klar, består hur du skapar en förutsägelsemodell av träning och testning. Vi använder vår datamodell tootrain hello och sedan vi testa hello modellen toosee hur nära det är kan toopredict priser.
<!-- For now, don't worry about *why* we need tootrain and then test a model.-->

*Klassificering* och *regression* är två typer av övervakade Machine Learning-algoritmer. Klassificering förutsäger ett svar från en definierad uppsättning kategorier, till exempel en färg (röd, blå eller grön). Regression är används toopredict ett tal.

Eftersom vi vill toopredict pris, vilket är ett tal, använder vi en regressionsalgoritm. I det här exemplet kommer vi att använda en *linjär regressionsmodell*.

> [!TIP]
> Om du vill ha mer information om olika typer av maskininlärningsalgoritmer toolearn och när toouse dem, kan du visa hello första video i hello datavetenskap för nybörjare serien [hello fem frågor datavetenskap svar](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Du kan också titta på hello infographic [maskin lär du dig grunderna med algoritmen exempel](machine-learning-basics-infographic-with-algorithm-examples.md), eller kolla hello [maskin learning algoritmen fusklapp](machine-learning-algorithm-cheat-sheet.md).

Vi träna hello modellen genom att ge den en uppsättning data som innehåller hello pris. hello modellen söker hello data och leta efter visar sambandet mellan en bil funktioner och dess pris. Sedan vi testa hello modell - vi ger en uppsättning funktioner för bilar vi känner till och se hur nära hello modellen kommer toopredicting hello kända pris.

Vi använder våra data för både hello modell och testning genom att dela hello data i separata träning och testning datauppsättningar.

1. Markera och dra hello [dela Data] [ split] modulen toohello experimentera arbetsytan och ansluta den toohello senast [Välj kolumner i datauppsättning] [ select-columns] modul.

2. Klicka på hello [dela Data] [ split] modulen tooselect den. Hitta hello **andel av rader i hello första utdatauppsättningen** (i hello **egenskaper** fönstret toohello höger i hello arbetsytan) och ange den too0.75. På så sätt kan vi använda 75 procent av hello datamodellen tootrain hello, och lämna 25 procent för testning (senare, kan du experimentera med olika procenttal).

    ![Ange hello dela bråkdel av hello ”dela Data” modulen too0.75][set-split-data-percentage]
    <br/>
    ***Ange hello dela bråkdel av hello ”dela Data” modulen too0.75***

    > [!TIP]
    > Genom att ändra hello **slumptal** parameter, kan du generera olika slumpmässiga prov för träning och testning. Denna parameter styr hello seeding av hello startvärden nummer generator.

2. Kör hello experiment. När du kör hello experiment hello [Välj kolumner i datauppsättning] [ select-columns] och [dela Data] [ split] moduler skicka kolumnen definitioner toohello moduler som vi ska lägga till härnäst.  

3. tooselect hello learning-algoritmen, expandera hello **Machine Learning** kategori i hello modulen paletten toohello vänster av hello arbetsytan och expandera sedan **initiera modell**. Då visas flera kategorier av moduler som kan vara används tooinitialize maskininlärningsalgoritmer. Det här experimentet väljer hello [linjär Regression] [ linear-regression] modulen under hello **Regression** kategori, och dra den toohello experimentet.
(Du kan också hitta hello modulen genom att skriva ”linear regression” i sökrutan för hello palett.)

4. Leta upp och dra hello [Träningsmodell] [ train-model] modulen toohello experimentera arbetsytan. Ansluta hello utdata från hello [linjär Regression] [ linear-regression] modulen toohello kvar indata för hello [Träningsmodell] [ train-model] modulen, och ansluta hello utbildning utdata (vänster porten) för hello [dela Data] [ split] toohello rätt modulindata av hello [Träningsmodell] [ train-model] modul.

    ![Ansluta hello ”Träningsmodell” modulen tooboth hello ”Linear Regression” och ”delade Data” moduler][connect-train-model]
    <br/>
    ***Ansluta hello ”Träningsmodell” modulen tooboth hello ”Linear Regression” och ”delade Data” moduler***

5. Klicka på hello [Träningsmodell] [ train-model] modul, klicka på **starta kolumnväljaren** i hello **egenskaper** fönstret och välj sedan hello **pris** kolumn. Detta är hello-värde som vår modell är pågående toopredict.

    Du väljer hello **pris** kolumn i hello kolumnväljaren genom att flytta från hello **tillgängliga kolumner** listan toohello **markerade kolumner** lista.

    ![Markera hello priskolumnen för hello ”” träningsmodellmodulen][select-price-column]
    <br/>
    ***Markera hello priskolumnen för hello ”” träningsmodellmodulen***

6. Kör hello experiment.

Nu har vi en tränad regressionsmodell som kan använda tooscore nya bil data toomake pris förutsägelser.

![När du har kört, hello experimentet bör nu se ut ungefär så här][second-experiment-run]
<br/>
***När du har kört, hello experimentet bör nu se ut ungefär så här***

## <a name="step-5-predict-new-automobile-prices"></a>Steg 5: Förutsäga nya bilpriser

Nu när vi har tränat hello modellen med 75 procent av våra data kan vi använda den tooscore hello övriga 25 procenten av hello data toosee hur bra vår modell fungerar.

1. Leta upp och dra hello [Poängmodell] [ score-model] modulen toohello experimentera arbetsytan. Ansluta hello utdata från hello [Träningsmodell] [ train-model] modulen toohello kvar indataport av [Poängmodell][score-model]. Ansluta hello testutdata (den högra porten) för hello [dela Data] [ split] modulen toohello höger inkommande port [Poängmodell][score-model].

    ![Ansluta hello ”Poängmodell” modulen tooboth hello ”Träningsmodell” och ”delade Data” moduler][connect-score-model]
    <br/>
    ***Ansluta hello ”Poängmodell” modulen tooboth hello ”Träningsmodell” och ”delade Data” moduler***

2. Kör hello experimentet och visa hello utdata från hello [Poängmodell] [ score-model] modul (klickar du på hello utdataporten för [Poängmodell] [ score-model] och välj **Visualisera**). hello utdata visar hello förväntade värdena för pris och hello kända värdena från testdata hello.  

    ![Utdata från modulen för hello ”poängsätta modell”][score-model-output]
    <br/>
    ***Utdata från modulen för hello ”poängsätta modell”***

3. Slutligen testa vi hello kvaliteten på hello resultat. Markera och dra hello [utvärdera modell] [ evaluate-model] modulen toohello experimentera arbetsytan och ansluta hello utdata från hello [Poängmodell] [ score-model] modulen toohello kvar indata för [utvärdera modell][evaluate-model].

    > [!TIP]
    > Det finns två indataportar på hello [utvärdera modell] [ evaluate-model] modulen eftersom det kan vara används toocompare två modeller sida vid sida. Du kan senare lägga till en annan algoritm toohello experiment och använda [utvärdera modell] [ evaluate-model] toosee vilket ger bättre resultat.

4. Kör hello experiment.

tooview hello utdata från hello [utvärdera modell] [ evaluate-model] modul, klicka på utdataporten hello och välj sedan **visualisera**.

![Utvärderingsresultat av för hello experiment][evaluation-results]
<br/>
***Utvärderingsresultat av för hello experiment***

för vår modell visas följande statistik hello:

- **Medelabsolutfel** (MAE): hello medelvärdet av absoluta fel (ett *fel* är hello skillnaden mellan hello förutsade och hello faktiska värdet).
- **Medelkvadratfel** (RMSE): hello kvadratroten ur hello genomsnittet av kvadratfel i förutsägelser som görs på hello testdata.
- **Relativa absoluta fel**: hello medelvärdet av absoluta fel relativa toohello absoluta skillnaden mellan faktiska värden och hello medelvärdet av alla faktiska värden.
- **Relativa Kvadratfel**: hello genomsnittet av kvadratfel relativa toohello kvadrat skillnaden mellan faktiska värden för hello och hello medelvärdet av alla faktiska värden.
- **Bestämningskoefficienten**: kallas även hello **R-kvadratvärdet**, detta är ett statistiskt mått som anger hur väl en modell passar hello data.

Statistik, mindre är bättre för varje hello-fel. Ett mindre värde anger att hello förutsägelser bättre överensstämmer hello faktiska värden. För **Bestämningskoefficient**, hello närmare dess värde är tooone (1.0) hello bättre hello förutsägelser.

## <a name="final-experiment"></a>Slutligt experiment

hello slutliga experimentet bör se ut ungefär så här:

![hello slutliga experimentet][complete-linear-regression-experiment]
<br/>
***hello slutliga experimentet***

## <a name="next-steps"></a>Nästa steg

Nu när du har slutfört hello första självstudie om maskininlärning och har skapat ett experiment kan du fortsätta tooimprove hello modellen och sedan distribuera det som en förutsägbar webbtjänst.

- **Iterera tootry tooimprove hello modellen** – du kan till exempel ändra hello-funktioner som du använder i din förutsägelse. Du kan ändra hello egenskaper för hello [linjär Regression] [ linear-regression] algoritmen eller försök med en annan algoritm helt och hållet. Du kan även lägga till flera maskininlärningsexperiment algoritmer tooyour samtidigt och jämföra två av dem med hjälp av hello [utvärdera modell] [ evaluate-model] modul.
Ett exempel på hur toocompare flera modeller i en enda experiment, se [jämför Regressorer](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) i hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).

    > [!TIP]
    > toocopy eventuella iterationer av experimentet, Använd hello **Spara som** knappen på hello hello sidans nederkant. Du kan se alla hello iterationer av experimentet genom att klicka på **visa KÖRNINGSHISTORIK** på hello hello sidans nederkant. Mer information finns i [Hantera iterationer av experiment i Azure Machine Learning Studio][runhistory].

[runhistory]: machine-learning-manage-experiment-iterations.md

- **Distribuera hello modellen som en förutsägbar webbtjänst** – när du är nöjd med din modell, du kan distribuera den som en web service toobe används toopredict bilpriser med hjälp av nya data. Mer information finns i [Distribuera en Azure Machine Learning-webbtjänst][publish].

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Vill du toolearn mer? En mer omfattande och detaljerad genomgång av hello processen för att skapa, utbildning, poäng och distribuera en modell finns [utveckla en förutsägelselösning med hjälp av Azure Machine Learning][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/machine-learning-create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/machine-learning-create-experiment/rename-experiment.png
[visualize-auto-data]:./media/machine-learning-create-experiment/visualize-auto-data.png
[select-visualize]: ./media/machine-learning-create-experiment/select-visualize.png
[showing-excluded-column]:./media/machine-learning-create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/machine-learning-create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/machine-learning-create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/machine-learning-create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/machine-learning-create-experiment/second-experiment-run.png
[connect-score-model]:./media/machine-learning-create-experiment/connect-score-model.png
[evaluation-results]:./media/machine-learning-create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[connect-train-model]:./media/machine-learning-create-experiment/connect-train-model.png
[select-price-column]:./media/machine-learning-create-experiment/select-price-column.png

[score-model-output]:./media/machine-learning-create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
