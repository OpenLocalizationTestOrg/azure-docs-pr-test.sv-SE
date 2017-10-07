---
title: aaaEvaluate modellen prestanda i Machine Learning | Microsoft Docs
description: "Förklarar hur tooevaluate modell prestanda i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 5dc5348a-4488-4536-99eb-ff105be9b160
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: bradsev;garye
ms.openlocfilehash: 03477368758dbb13aa6f54c5d27fb215615d1f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooevaluate-model-performance-in-azure-machine-learning"></a>Hur tooevaluate modell prestanda i Azure Machine Learning
Den här artikeln visar hur tooevaluate hello prestanda för en modell i Azure Machine Learning Studio och ger en kort förklaring av hello tillgängliga mått för den här uppgiften. Tre vanliga scenarier för övervakad inlärning visas: 

* Regression
* binär klassificering 
* multiklass-baserad klassificering

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Utvärdera hello prestanda för en modell är en av hello core etapper i hello datavetenskap processen. Anger hur lyckade hello bedömningen (förutsägelser) i en dataset har av en tränad modell. 

Azure Machine Learning stöder modellen utvärdering via två av dess huvudsakliga moduler för maskininlärning: [utvärdera modell] [ evaluate-model] och [kontrolleras mot modellen] [ cross-validate-model]. Dessa moduler kan du toosee hur modellen presterar i ett antal mätvärden som är vanliga i machine learning och statistik.

## <a name="evaluation-vs-cross-validation"></a>Utvärderingen vs. Mellan verifiering
Utvärdering och mellan validering är standard sätt toomeasure hello prestandan för din modell. De båda generera utvärdering mått som du kan kontrollera eller jämföra med de andra modeller.

[Utvärdera modellen] [ evaluate-model] förväntar sig en poängsatta dataset som indata (eller 2 i fall som toocompare hello prestanda för 2 olika modeller). Det innebär att du behöver tootrain din modell med hello [Träningsmodell] [ train-model] modulen och göra förutsägelser för vissa datauppsättningen med hello [Poängmodell] [ score-model] modulen innan du kan utvärdera hello resultat. hello utvärdering baseras på hello bedömas etiketter/troliga tillsammans med hello true etiketterna, som är resultatet av hello [Poängmodell] [ score-model] modul.

Du kan också använda mellan validering tooperform ett antal train poäng utvärdera åtgärder (10 gånger) automatiskt på olika delar av hello indata. hello indata har delats 10, där en är reserverad för testning och hello andra 9 för utbildning. Den här processen upprepas 10 gånger och medelvärdet av hello utvärdering mått. På så sätt kan bestämma hur väl en modell skulle generalisera toonew datauppsättningar. Hej [kontrolleras mot modellen] [ cross-validate-model] modul hämtar i ett omdöme modellen och vissa etikett dataset- och utgångar hello utvärderingsresultaten av hello 10 gånger, dessutom toohello var i genomsnitt resultat.

I följande avsnitt hello, vi bygga enkla regression och klassificering modeller och utvärdera deras prestanda med hjälp av både hello [utvärdera modell] [ evaluate-model] och hello [kontrolleras Modellen] [ cross-validate-model] moduler.

## <a name="evaluating-a-regression-model"></a>Utvärdera en regressionsmodell
Förutsätter vi vill toopredict en bil priset genom att använda vissa funktioner, till exempel dimensioner, hästkrafter, motorn specifikationer och så vidare. Det här är en typisk regression problem, där hello målvariabel (*pris*) är en kontinuerlig numeriskt värde. Vi kan passa in en enkel linjär regressionsmodell att värdena för en viss bil angivna hello-funktionen kan förutsäga hello priset på den bilen. Du kan använda den här regressionsmodell tooscore hello samma dataset som vi tränats på. När vi har hello förväntade priser för alla hello bilar, kan vi utvärdera hello prestanda för hello modellen genom att titta på hur mycket hello förutsägelser avvika från hello faktiska priser i genomsnitt. tooillustrate kan vi använda hello *Automobile price data (Raw) dataset* tillgängliga i hello **sparade datauppsättningar** avsnitt i Azure Machine Learning Studio.

### <a name="creating-hello-experiment"></a>Skapa hello Experiment
Lägg till följande moduler tooyour arbetsytan i Azure Machine Learning Studio hello:

* Bil price data (Raw)
* [Linjär Regression][linear-regression]
* [Träningsmodell][train-model]
* [Poängsätta modell][score-model]
* [Utvärdera modellen][evaluate-model]

Ansluta hello portar som visas nedan i figur 1 och ange hello etikett kolumn hello [Träningsmodell] [ train-model] modul för*pris*.

![Utvärdera en regressionsmodell](media/machine-learning-evaluate-model-performance/1.png)

Bild 1. Utvärderar en regressionsmodell.

### <a name="inspecting-hello-evaluation-results"></a>Kontrollera hello utvärderingsresultat
Efter körs hello experiment kan du klicka på hello utdataporten för hello [utvärdera modell] [ evaluate-model] modulen och välj *visualisera* toosee hello resultatet. Hej utvärdering tillgängliga mått för regression modeller är: *medelvärdet av absoluta fel*, *rot innebär absoluta fel*, *relativa absoluta fel*,  *Relativa kvadrat fel*, och hello *Bestämningskoefficient*.

hello termen ”error” här representerar hello skillnaden mellan hello förutsägelsevärdet och hello true-värde. hello absolutvärdet eller hello rektangulär skillnaden är vanligtvis beräknade toocapture hello totala storleken på fel i alla instanser som hello skillnaden mellan hello förutsade och värdet true får vara negativt i vissa fall. hello fel mått mäter hello förutsägbar prestanda för en regressionsmodell vad gäller hello medelvärdet avvikelse för dess förutsägelser från hello true värden. Nedre felvärdena betyder hello modellen är exaktare i att göra förutsägelser. Ett mått för övergripande fel 0 innebär att hello modell passar perfekt hello data.

hello bestämningskoefficienten, som även kallas R kvadrat är också ett standardiserat sätt att mäta hur väl hello modell passar hello data. Det kan tolkas som hello andelen variation beskrivs av hello modellen. En högre andel är bättre i detta fall där 1 anger en perfekt passning.

![Linjär Regression utvärdering mått](media/machine-learning-evaluate-model-performance/2.png)

Figur 2. Linjär Regression utvärdering mått.

### <a name="using-cross-validation"></a>Med mellan verifiering
Som tidigare nämnts kan du utföra upprepade utbildning, poäng och utvärderingar automatiskt med hello [kontrolleras mot modellen] [ cross-validate-model] modul. Allt du behöver i det här fallet är en datamängd, ett omdöme modell och en [kontrolleras mot modellen] [ cross-validate-model] modul (se figuren nedan). Observera att du tooset hello etikettkolumnen för*pris* i hello [kontrolleras mot modellen] [ cross-validate-model] modulens egenskaper.

![Validera en regressionsmodell mellan](media/machine-learning-evaluate-model-performance/3.png)

Bild 3. Cross-validerar en regressionsmodell.

Efter körs hello experiment kan du granska resultatet hello genom att klicka på hello rätt utdataporten för hello [kontrolleras mot modellen] [ cross-validate-model] modul. Detta ger en detaljerad vy av hello mätvärden för varje iteration (vikt) och hello var i genomsnitt resultaten av hello mått (bild 4).

![Korsvalidering resultatet av en regressionsmodell](media/machine-learning-evaluate-model-performance/4.png)

Bild 4. Korsvalidering resultaten av en regressionsmodell.

## <a name="evaluating-a-binary-classification-model"></a>Utvärdering av en modell för binär klassificering
I ett scenario med binär klassificering hello målvariabel har bara två möjliga resultat, till exempel: {0, 1} eller {FALSKT, SANT}, {negativt, positivt}. Anta att du får en datauppsättning för vuxna anställda med några demografisk och anställningen variabler och att du uppmanas toopredict hello intäkter nivå, en binär variabel med hello värdena {”< = 50K” ”, > 50K”}. Med andra ord hello negativt klassen representerar hello anställda som göra mindre än eller lika med too50K per år och hello positivt klassen representerar alla anställda. Vi skulle som hello regression, tränar en modell, poängsätta vissa data och utvärdera hello resultat. hello största skillnaden här är hello val av mått som beräknar Azure Machine Learning och utdata. tooillustrate hello intäkter nivån förutsägelse scenariot kommer vi att använda hello [vuxna](http://archive.ics.uci.edu/ml/datasets/Adult) dataset toocreate en Azure Machine Learning experimentera och utvärdera hello prestanda för en två-klass logistic regressionsmodell, en binär som används ofta klassificerare.

### <a name="creating-hello-experiment"></a>Skapa hello Experiment
Lägg till följande moduler tooyour arbetsytan i Azure Machine Learning Studio hello:

* Vuxna inventering intäkter binär klassificering dataset
* [Two-Class Logistic Regression][two-class-logistic-regression]
* [Träningsmodell][train-model]
* [Poängsätta modell][score-model]
* [Utvärdera modellen][evaluate-model]

Ansluta hello portar som visas nedan i figur 5 och ange hello etikett kolumn hello [Träningsmodell] [ train-model] modul för*intäkter*.

![Utvärdering av en modell för binär klassificering](media/machine-learning-evaluate-model-performance/5.png)

Bild 5. Utvärderar en binär klassificering modell.

### <a name="inspecting-hello-evaluation-results"></a>Kontrollera hello utvärderingsresultat
Efter körs hello experiment kan du klicka på hello utdataporten för hello [utvärdera modell] [ evaluate-model] modulen och välj *visualisera* toosee hello utvärderingsresultaten (bild 7). Hej utvärdering tillgängliga mått för binär klassificering modeller är: *noggrannhet*, *Precision*, *återkalla*, *F1 poäng*, och *AUC*. Dessutom hello modulen matar ut en förvirring matris som visar hello antalet positiva identifieringar som SANT, FALSKT negativ, falska positiva identifieringar och SANT negativ samt *ROC*, *Precision/återkalla*, och *Lyfter* kurvor.

Precisionen är helt enkelt hello andelen korrekt klassificerad instanser. Det är vanligtvis hello första mått du tittar på när du utvärderar en klassificerare. När hello testdata är dock obalanserade (som de flesta av hello instanser tillhör tooone hello klasser) och du är intresserad av mer hello prestanda på någon av hello klasser, noggrannhet verkligen inte avbilda en klassificerare hello effektivitet. I hello intäkter nivån klassificering scenariot förutsätter att du testar på vissa data där 99% av hello instanser representerar personer som får mindre än eller lika med too50K per år. Det är möjligt tooachieve 0.99 noggrannhet genom att förutsäga hello klass ”< = 50K” för alla instanser. hello klassificerare visas i det här fallet toobe gör bra övergripande, men i själva verket misslyckas tooclassify någon hello high-income individer (hello 1%) korrekt.

Därför är det bra toocompute ytterligare mått som samlar in mer specifika aspekter av hello utvärdering. Innan du fortsätter hello detaljer om mått är det viktigt toounderstand hello förvirring matris för en binär klassificering utvärderingen. hello-klass som etiketter i hello träningsmängden kan ha endast 2 möjliga värden som vi vanligtvis finns tooas positivt eller negativt. hello positiva och negativa instanser som en klassificerare beräknar korrekt kallas true positiva identifieringar (TP) och true negativ (TN). På liknande sätt kallas hello felaktigt klassificerad instanser falska positiva identifieringar (RP) och FALSKT negativ (FN). hello förvirring matrisen är helt enkelt en tabell som visar hello antalet instanser som faller under följande 4 kategorier. Azure Machine Learning beslutar automatiskt som hello två klasser i hello dataset är positivt hello-klass. Om hello klassen etiketter är Boolean eller heltal och sedan hello tilldelas 'true' eller '1' namngivna instanser hello positivt klass. Om hello etiketter är strängar som hello fallet med hello intäkter dataset, hello etiketter sorteras alfabetiskt och hello första nivån väljs toobe hello negativt klassen medan hello andra nivån är positivt hello-klass.

![Binär klassificering förvirring matris](media/machine-learning-evaluate-model-performance/6a.png)

Bild 6. Binär klassificering förvirring matris.

Gå tillbaka toohello intäkter klassificeringsproblem skulle vi vill tooask flera utvärderingsfrågor som hjälper oss att förstå hello prestanda av hello-klassificerare som används. En mycket naturliga frågan är: ' utanför hello personer som hello modellen förväntade toobe med > 50 K (TP + RP), hur många har klassificerats på rätt sätt (TP) ”? Den här frågan kan lösas genom att titta på hello **Precision** i hello modellen är hello andelen positiva identifieringar som klassificerats korrekt: TP/(TP+FP). En annan vanlig fråga är ”utanför alla hello hög inkomst anställda med intäkter > 50 k (TP + FN), hur många hello klassificerare klassificera korrekt (TP)”. Detta är faktiskt hello **återkalla**, eller true positiva hello-satsen: TP/(TP+FN) av hello klassificerare. Du kan se att det finns en uppenbara kompromiss mellan precision och återkalla. Till exempel ges en relativt belastningsutjämnade dataset en klassificerare som beräknar främst positivt instanser skulle ha en hög återkallning, men en snarare Låg precision så många hello negativt instanser skulle klassificeras som resulterar i ett stort antal falska positiva identifieringar. toosee ritning av hur dessa två mått varierar, kan du klicka på hello ' PRECISION/ÅTERKALLA-kurvan i hello utvärdering resultatsidan utdata (övre vänstra del av bild 7).

![Utvärderingsresultat av binär klassificering](media/machine-learning-evaluate-model-performance/7.png)

Bild 7. Utvärderingsresultat av binär klassificering.

En annan relaterade mått som används ofta är hello **F1 poäng**, som tar både precision och återkalla i beräkningen. Det är hello harmoniska medelvärdet av de här 2 måtten och beräknas som sådana: F1 = 2 (precision x återkalla) / (precision + återkalla). hello F1 resultatet är en bra sätt toosummarize hello utvärdering i ett tal, men det är alltid en bra idé toolook på både precision och återkalla tillsammans toobetter förstå hur en klassificerare fungerar.

Dessutom kan en inspektera hello true positiva satsen kontra hello falska positiva satsen i hello **mottagare operativsystem egenskap (ROC)** kurva och hello motsvarande **område Under hello kurvan (AUC)** värde. hello närmare kurvan toohello övre vänstra hörnet är hello bättre hello klassificerares prestanda (som maximerar hello true positivt hastighet och minimerar hello falska positiva satsen). Kurvor som är nära toohello diagonal av Hej ritytans, resultatet från klassificerare som brukar toomake förutsägelser som stänger toorandom att gissa.

### <a name="using-cross-validation"></a>Med mellan verifiering
Som hello regression exempel vi utföra mellan validering toorepeatedly tåg, poängsätta och utvärdera olika delmängder av data för hello automatiskt. På liknande sätt kan vi använda hello [kontrolleras mot modellen] [ cross-validate-model] modul, ett omdöme logistic regressionsmodell och en datamängd. hello etikettkolumnen måste anges för*intäkter* i hello [kontrolleras mot modellen] [ cross-validate-model] modulens egenskaper. När du kör hello experiment och klicka på hello rätt utgående port för hello [kontrolleras mot modellen] [ cross-validate-model] modulen, kan vi se hello binär klassificering mått värden för varje vikning dessutom toohello medelvärdet och standardavvikelsen för var och en. 

![Validera en binär klassificering modell mellan](media/machine-learning-evaluate-model-performance/8.png)

Figur 8. Cross-validerar en binär klassificering modell.

![Korsvalideringsresultaten av en binär klassificerare](media/machine-learning-evaluate-model-performance/9.png)

Bild 9. Korsvalideringsresultaten av en binär klassificerare.

## <a name="evaluating-a-multiclass-classification-model"></a>Utvärdering av en modell för multiklass-baserad klassificering
I den här experiment kommer vi att använda hello populära [Iris](http://archive.ics.uci.edu/ml/datasets/Iris "Iris") datauppsättning som innehåller instanser av 3 olika typer (klasser) av hello iris anläggningen. Det finns 4 funktionen värden (sepal längd och bredd och bladets längd och bredd) för varje instans. Hello tidigare försök vi tränas och testade hello modeller med hello samma datauppsättningar. Här kan vi använder hello [dela Data] [ split] modulen toocreate 2 delmängder av data för hello, träna på hello först och poängsätta och utvärdera på hello andra. hello Iris dataset är allmänt tillgänglig på hello [UCI Machine Learning databasen](http://archive.ics.uci.edu/ml/index.html), och kan hämtas med hjälp av en [importera Data] [ import-data] modul.

### <a name="creating-hello-experiment"></a>Skapa hello Experiment
Lägg till följande moduler tooyour arbetsytan i Azure Machine Learning Studio hello:

* [Importera Data][import-data]
* [Multiclass beslut skog][multiclass-decision-forest]
* [Dela Data][split]
* [Träningsmodell][train-model]
* [Poängsätta modell][score-model]
* [Utvärdera modellen][evaluate-model]

Ansluta hello portar som visas nedan i figur 10.

Ange hello etikett kolumnindex av hello [Träningsmodell] [ train-model] modulen too5. hello dataset har ingen rubrikrad men vi vet hello klassen etiketter är i hello femte kolumnen.

Klicka på hello [importera Data] [ import-data] modulen och ange hello *datakällan* egenskapen för*Webbadress via HTTP*, och hello *URL*  toohttp://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data.

Ange hello bråkdel av instanser toobe används för hello [dela Data] [ split] modul (0,7 till exempel).

![Utvärdering av en multiklass-baserad klassificerare](media/machine-learning-evaluate-model-performance/10.png)

Bild 10. Utvärdering av en multiklass-baserad klassificerare

### <a name="inspecting-hello-evaluation-results"></a>Kontrollera hello utvärderingsresultat
Kör experimentet hello och klicka på hello utdataporten för [utvärdera modell][evaluate-model]. utvärderingsresultat av hello presenteras i hello form av en förvirring matris i det här fallet. hello matris visar hello faktiska kontra förväntade instanser för alla 3 klasser.

![Utvärderingsresultat av multiklass-baserad klassificering](media/machine-learning-evaluate-model-performance/11.png)

Figur 11. Utvärderingsresultat av multiklass-baserad klassificering.

### <a name="using-cross-validation"></a>Med mellan verifiering
Som tidigare nämnts kan du utföra upprepade utbildning, poäng och utvärderingar automatiskt med hello [kontrolleras mot modellen] [ cross-validate-model] modul. Du behöver en datamängd, ett omdöme modell och en [kontrolleras mot modellen] [ cross-validate-model] modul (se figuren nedan). Igen måste tooset hello etikett kolumn i hello [kontrolleras mot modellen] [ cross-validate-model] modul (kolumnindex 5 i det här fallet). När du kör hello experiment och klicka på hello höger utgående port för hello [kontrolleras mot modellen][cross-validate-model], du kan inspektera hello måttvärden för varje vika samt hello medelvärdet och standardavvikelsen. hello är visas här hello liknande toohello som beskrivs i hello binär klassificering fallet. Observera dock att i multiklass-baserad klassificering hello true positiva identifieringar/negativ och falska positiva identifieringar/negativ görs genom att räkna på grundval av per klass, eftersom det finns ingen övergripande positivt eller negativt klass. Till exempel när databehandling hello precision eller återkallar hello 'Iris setosa-klassen, förutsätts att detta är positivt hello-klassen och alla andra som negativt.

![Verifiera mellan en modell för multiklass-baserad klassificering](media/machine-learning-evaluate-model-performance/12.png)

Figur 12. Cross-validering av en modell för multiklass-baserad klassificering.

![Korsvalideringsresultaten av en modell för multiklass-baserad klassificering](media/machine-learning-evaluate-model-performance/13.png)

Figur 13. Korsvalideringsresultaten av en modell för multiklass-baserad klassificering.

<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/

