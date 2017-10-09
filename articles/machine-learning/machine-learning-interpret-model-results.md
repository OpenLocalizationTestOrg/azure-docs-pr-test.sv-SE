---
title: aaaInterpret modellen resulterar i Machine Learning | Microsoft Docs
description: "Hur toochoose hello optimala parameterinställning för en algoritm med och visualisera poängsätta modell utdata."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6230e5ab-a5c0-4c21-a061-47675ba3342c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 52161b1aa5ff3e7a63fc4b1bfb7c5e354eabcc50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="interpret-model-results-in-azure-machine-learning"></a>Tolka modellen resultaten i Azure Machine Learning
Det här avsnittet beskrivs hur toovisualize och tolka förutsägelse resulterar i Azure Machine Learning Studio. När du har en modell tränas och göra förutsägelser ovanpå det (”bedömas hello modellen”), måste toounderstand och tolka hello förutsägelse resultat.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Det finns fyra huvudsakliga typer av machine learning-modeller i Azure Machine Learning:

* Klassificering
* Klustring
* Regression
* Rekommenderare system

hello-moduler som används för förutsägelse ovanpå dessa modeller är:

* [Bedömningsmodell] [ score-model] -modulen för klassificering och regression
* [Tilldela tooClusters] [ assign-to-clusters] -modulen för kluster
* [Poängsätta Matchbox rekommenderare] [ score-matchbox-recommender] för rekommendation system

Det här dokumentet förklarar hur toointerpret förutsägelse resultat för var och en av dessa moduler. En översikt av dessa moduler finns [hur toochoose parametrar toooptimize algoritmerna i Azure Machine Learning](machine-learning-algorithm-parameters-optimize.md).

Det här avsnittet tar förutsägelse tolkning men inte modellen utvärdering. Mer information om hur tooevaluate din modell finns [hur tooevaluate modell prestanda i Azure Machine Learning](machine-learning-evaluate-model-performance.md).

Om du är ny tooAzure Machine Learning och behöver hjälp att skapa ett enkelt experiment tooget igång finns [skapa ett enkelt experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md) i Azure Machine Learning Studio.

## <a name="classification"></a>Klassificering
Det finns två underkategorier klassificering problem:

* Problem med bara två klasser (tvåklass eller binär klassificering)
* Problem med fler än två klasser (flera klassen klassificering)

Azure Machine Learning har olika moduler toodeal med de olika typerna av klassificering, men hello metoder för att tolka resultaten förutsägelse är liknande.

### <a name="two-class-classification"></a>Tvåklass klassificering
**Exempel experiment**

Ett exempel på en två-klass klassificeringsproblem är hello klassificering av iris blommor. hello uppgift är tooclassify iris blommor baserat på deras funktioner. hello Iris datauppsättning i Azure Machine Learning är en delmängd av hello populära [Iris datauppsättning](http://en.wikipedia.org/wiki/Iris_flower_data_set) som innehåller instanser av endast två blomma art (klasser 0 och 1). Det finns fyra funktioner för varje blomma (sepal längd, sepal bredd, bladets längd och bladets bredd).

![Skärmbild av iris experiment](./media/machine-learning-interpret-model-results/1.png)

Bild 1. Iris tvåklass klassificering problemet experiment

Ett experiment har genomfört toosolve problemet, som visas i bild 1. En två-klass ökat beslut trädet modell har tränats och bedömas. Nu kan du visualisera hello förutsägelse resultat från hello [Poängmodell] [ score-model] modulen genom att klicka på hello utdataporten för hello [Poängmodell] [ score-model]modul och klicka sedan på **visualisera**.

![Modulen poängsätta modell](./media/machine-learning-interpret-model-results/1_1.png)

Detta öppnar hello bedömningen resultat som visas i bild 2.

![Resultat av iris tvåklass klassificering experiment](./media/machine-learning-interpret-model-results/2.png)

Figur 2. Visualisera poängsätta modell resultatet i två klassen klassificering

**Tolkning av resultat**

Det finns sex kolumner i hello resultattabellen. hello vänstra fyra kolumner är hello fyra funktioner. hello höger två kolumner, poängsatta etiketter och bedömas sannolikhet finns hello förutsägelse resultat. hello bedömas sannolikhet kolumnen visar hello sannolikheten att en blomma tillhör toohello positivt class (klassen 1). Till exempel hello första numret i hello kolumn (0.028571) betyder att det finns 0.028571 sannolikheten att hello första blomma tillhör tooClass 1. Hej poängsatta etiketter kolumnen visar hello förutsade klass för varje blomma. Detta baseras på hello bedömas sannolikhet kolumnen. Om hello bedömas sannolikheten för en blomma är större än 0,5 prognoser den som klass 1. Det är också förutsade som klass 0.

**Tjänstepublicering för webbprogram**

När hello förutsägelse resultat har förstått och bedöms ljud, kan hello experiment publiceras som en webbtjänst så att du kan distribuera i olika program och anropa tooobtain klassen förutsägelser på alla nya iris blommor. toolearn hur toochange en utbildning experimentera till bedömningsprofil experiment och publicera den som en webbtjänst finns i [publicerar hello Azure Machine Learning-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md). Den här proceduren innehåller ett bedömningsprofil experiment som visas i bild 3.

![Skärmbild av bedömningen experiment](./media/machine-learning-interpret-model-results/3.png)

Bild 3. Bedömningsprofil hello iris tvåklass klassificering problemet experiment

Nu behöver du tooset hello indata och utdata för hello-webbtjänsten. hello-indata är hello högra indataporten av [Poängmodell][score-model], vilket är hello Iris blomma funktioner indata. hello hello utdata beror på om du är intresserad av i hello förutsade klass (poängsatta etikett), hello bedömas sannolikhet eller båda. I det här exemplet förutsätts att du är intresserad av båda. tooselect hello önskad utdatakolumner, Använd en [Välj kolumner i datauppsättning] [ select-columns] modul. Klicka på [Välj kolumner i datauppsättning][select-columns], klickar du på **starta kolumnväljaren**, och välj **poängsatta etiketter** och **Scored Troliga**. När du har angett hello utdataporten för [Välj kolumner i datauppsättning] [ select-columns] och kör det du vara redo toopublish hello bedömningsprofil experiment som en webbtjänst genom att klicka på **Publicera webbplats TJÄNSTEN**. Det ser ut som bild 4 hello slutliga experimentet.

![hello iris tvåklass klassificering experiment](./media/machine-learning-interpret-model-results/4.png)

Bild 4. Sista bedömningsprofil experiment för en iris tvåklass klassificeringsproblem

När du kör hello webbtjänst och ange några värden för funktionen på en instans av test returnerar hello resultatet två tal. hello är andra hello bedömas sannolikheten hello första siffran är hello bedömas etikett. Den här blomma prognoser som klass 1 med 0.9655 sannolikhet.

![Testa tolkar poängsätta modell](./media/machine-learning-interpret-model-results/4_1.png)

![Testresultat för bedömningsprofil](./media/machine-learning-interpret-model-results/5.png)

Bild 5. Web service resultatet av iris tvåklass klassificering

### <a name="multi-class-classification"></a>Flera klassen klassificering
**Exempel experiment**

I det här experimentet utför du en bokstav recognition uppgift är ett exempel på multiklass-baserad klassificering. hello klassificerare försöker toopredict en viss enhetsbeteckning (klassen) baserat på vissa handskriven attributvärden som extraheras från hello handskriven bilder.

![Bokstav recognition exempel](./media/machine-learning-interpret-model-results/5_1.png)

I hello träningsdata finns 16 funktioner som extraheras från handskriven bokstav bilder. hello 26 bokstäver utgör våra 26 klasser. Bild 6 visar ett experiment som kommer att träna en multiklass-baserad klassificering modellen för bokstav recognition och förutsäga hello samma funktion som anges på en datauppsättning för testet.

![Bokstav recognition multiklass-baserad klassificering experiment](./media/machine-learning-interpret-model-results/6.png)

Bild 6. Bokstav recognition multiklass-baserad klassificering problemet experiment

Visualisera hello resultat från hello [Poängmodell] [ score-model] modulen genom att klicka på hello utdataporten för [Poängmodell] [ score-model] modulen och sedan Klicka på **visualisera**, bör du se innehåll som visas på bild 7.

![Modellen poängresultat](./media/machine-learning-interpret-model-results/7.png)

Bild 7. Visualisera poängresultat modell i en klass med flera klassificering

**Tolkning av resultat**

hello vänstra 16 kolumner representerar hello funktionen värden av hello test. hello kolumner med namn som bedömas sannolikhet för klassen ”XX” är precis som hello bedömas sannolikhet kolumn i hello tvåklass fall. De visar hello sannolikheten att hello motsvarande post hamnar i en viss klass. Till exempel för hello första posten finns 0.003571 sannolikheten för att det är ett ”A” 0.000451 sannolikheten för att det är en ”B” och så vidare. hello sista kolumnen (Scored etiketter) är hello samma som poängsatta etiketter i hello tvåklass fall. Hello-klassen med hello största bedömas sannolikheten som hello förväntade klassen hello motsvarande transaktion väljs. Till exempel är hello första posten, hello bedömas etiketten ”F” eftersom den har hello största sannolikhet toobe ett ”F” (0.916995).

**Tjänstepublicering för webbprogram**

Du kan också få hello bedömas etikett för varje post och hello sannolikheten för hello bedömas etikett. hello grundläggande logiken är toofind hello största sannolikhet bland alla hello bedömas sannolikhet. toodo detta, behöver du toouse hello [köra R-skriptet] [ execute-r-script] modul. hello R-koden visas i figur 8 och hello resultatet av hello experiment visas i bild 9.

![R-kodexempel](./media/machine-learning-interpret-model-results/8.png)

Figur 8. R-koden för att extrahera poängsatta etiketter och hello associerade troliga hello etiketter

![Experiment resultat](./media/machine-learning-interpret-model-results/9.png)

Bild 9. Sista bedömningsprofil experiment hello bokstav recognition multiklass-baserad klassificering problemet

När du har publicerat och kör hello webbtjänst och ange värden för vissa inkommande funktionen returnerade hello resultatet ser ut som bild 10. Den här handskriven bokstav, med dess extraherade 16 funktioner är förväntade toobe ”D” med 0.9715 sannolikhet.

![Testa tolkar score-modul](./media/machine-learning-interpret-model-results/9_1.png)

![Testresultat](./media/machine-learning-interpret-model-results/10.png)

Bild 10. Web service resultatet av multiklass-baserad klassificering

## <a name="regression"></a>Regression
Regressionsproblem skiljer sig från klassificering problem. I en klassificering problemet försöker du toopredict diskreta klasser, till exempel vilken klass en iris blomma tillhör. Men som du ser i hello följande exempel på ett problem med regression försöker toopredict en kontinuerlig variabel, exempelvis hello priset på en bil.

**Exempel experiment**

Använda bil pris förutsägelse som ditt exempel för regression. Du försöker toopredict hello priset på en bil baserat på dess funktioner, inklusive se, bränsletyp, brödtexten och hjul för enheten. hello experiment visas i figur 11.

![Bil pris regression experiment](./media/machine-learning-interpret-model-results/11.png)

Figur 11. Bil pris regression problemet experiment

Visualisera hello [Poängmodell] [ score-model] modulen hello resultatet ser ut som figur 12.

![Bedömningsprofil resultat för bil pris förutsägelse problem](./media/machine-learning-interpret-model-results/12.png)

Figur 12. Bedömningsprofil resultat för hello bil pris förutsägelse problem

**Tolkning av resultat**

Poängsatta etiketter är hello resultatkolumnen i resultatet för den här bedömningsprofil. hello nummer är hello förväntade priset för varje bil.

**Tjänstepublicering för webbprogram**

Du kan publicera hello regression experiment till en webbtjänst och kalla den för en bil pris prognos i hello samma sätt som i hello tvåklass klassificering användningsfall.

![Bedömningen experiment för bil pris regression problem](./media/machine-learning-interpret-model-results/13.png)

Figur 13. Bedömningen experiment på en bil pris regression problem

Kör hello webbtjänst hello returnerade resultatet ser ut som figur 14. hello förväntade priset för den här bil är $15,085.52.

![Testa tolkar poängsättningsmodul](./media/machine-learning-interpret-model-results/13_1.png)

![Bedömningen modulen resultat](./media/machine-learning-interpret-model-results/14.png)

Figur 14. Web service-resultatet av en bil pris regression problem

## <a name="clustering"></a>Klustring
**Exempel experiment**

Nu ska vi använda hello Iris datauppsättning igen toobuild klustring experiment. Här kan du filtrera ut hello klassen etiketter i hello datamängden så att den bara innehåller funktioner och kan användas i kluster. I den här iris användningsfall genom att ange hello antalet kluster toobe två under hello utbildning processen, vilket innebär att du vill gruppera hello blommor i två klasser. hello experiment visas i figur 15.

![Iris klustring problemet experiment](./media/machine-learning-interpret-model-results/15.png)

Figur 15. Iris klustring problemet experiment

Kluster till skillnad från klassificering hello datauppsättning för träning saknar grunden sanningen etiketter ensamt. Klustring grupper hello utbildning datauppsättning instanser i olika kluster. Under hello utbildning hello hello modelletiketter poster genom att lära dig hello skillnaderna mellan deras funktioner. Efter det hello tränad modell kan användas för toofurther klassificera framtida transaktioner. Det finns två delar av hello resultatet vi är intresserade i ett kluster problem. hello första delen etiketter hello datauppsättning för träning och hello klassificerar andra en ny datamängd med hello tränad modell.

hello första delen av hello resultatet kan vara visualiseras genom att klicka på vänster utdataporten för hello [Clustering Träningsmodell] [ train-clustering-model] och sedan klicka på **visualisera**. hello visualiseringen illustreras i bild 16.

![Klustring resultat](./media/machine-learning-interpret-model-results/16.png)

Bild 16. Visualisera resultat för datauppsättning för träning hello-kluster

hello resultatet av hello andra delen, klustring nya poster med hello utbildade klustringsmodell illustreras i bild 17.

![Visualisera clustering resultat](./media/machine-learning-interpret-model-results/17.png)

Figur 17. Visualisera klustring resultat på en ny datauppsättning

**Tolkning av resultat**

Även om hello resultaten av hello två delar härrör från olika experiment faser, de letar hello samma och tolkas i hello samma sätt. hello är första fyra kolumner funktioner. hello sista kolumnen tilldelningar, beror hello förutsägelse. hello poster som tilldelats hello samma nummer beräknas toobe i hello samma klustret, det vill säga de delar likheter på något sätt (experimentet använder hello standardmått Euclidean avstånd). Eftersom du har angett hello antalet kluster toobe 2 är hello poster i tilldelningar märkta 0 eller 1.

**Tjänstepublicering för webbprogram**

Du kan publicera hello clustering experiment till en webbtjänst och kalla den för klustring förutsägelser hello samma sätt som i hello tvåklass klassificering användningsfall.

![Bedömningen experiment för iris klustring problem](./media/machine-learning-interpret-model-results/18.png)

Bild 18. Bedömningen experiment för ett kluster iris-problem

När du har kört hello webbtjänsten returnerade hello resultatet ser ut som bild 19. Den här blomma är förväntade toobe i klustret 0.

![Testa tolka poängsättningsmodul](./media/machine-learning-interpret-model-results/18_1.png)

![Bedömningen modulen resultat](./media/machine-learning-interpret-model-results/19.png)

Bild 19. Web service resultatet av iris tvåklass klassificering

## <a name="recommender-system"></a>Rekommenderare system
**Exempel experiment**

För rekommenderare system, du kan använda hello restaurang rekommendation problemet som ett exempel: du kan rekommendera hotell för kunder baserat på deras klassificering historik. hello indata består av tre delar:

* Restaurang klassificeringar från kunder
* Funktionen kundinformation
* Restaurang funktionsdata

Det finns flera saker som vi kan göra med hello [Train Matchbox rekommenderare] [ train-matchbox-recommender] modul i Azure Machine Learning:

* Förutsäga klassificering för en viss användare och objekt
* Rekommendera objekt tooa angivna användare
* Hitta användare relaterade tooa angivna användare
* Sök efter objekt relaterade tooa beroende objekt

Du kan välja vad du vill toodo genom att välja hello fyra alternativen i hello **rekommenderare förutsägelse typ** menyn. Här kan du gå igenom alla fyra scenarier.

![Matchbox rekommenderare](./media/machine-learning-interpret-model-results/19_1.png)

En typisk Azure Machine Learning-experiment för ett system för rekommenderare ser ut som bild 20. Information om hur toouse dessa moduler för rekommenderare system, se [Train matchbox rekommenderare] [ train-matchbox-recommender] och [poäng matchbox rekommenderare] [ score-matchbox-recommender].

![Rekommenderare system experiment](./media/machine-learning-interpret-model-results/20.png)

Figur 20. Rekommenderare system experiment

**Tolkning av resultat**

**Förutsäga klassificering för en viss användare och objekt**

Genom att välja **klassificering förutsägelse** under **rekommenderare förutsägelse typ**, du ber hello rekommenderare system toopredict hello klassificeringen för en viss användare och objekt. Hej visualisering av hello [poäng Matchbox rekommenderare] [ score-matchbox-recommender] utdata ser ut som bild 21.

![Poängsätta resultatet av hello rekommenderare system--klassificeringen förutsägelse](./media/machine-learning-interpret-model-results/21.png)

Figur 21. Visualisera hello poäng resultatet av hello rekommenderare system--klassificering förutsägelse

hello först två kolumner är hello användaren objektet par som tillhandahålls av hello indata. hello tredje kolumnen är hello förväntade klassificering av en användare för ett visst objekt. Hello första raden förutsade kunden U1048 är till exempel toorate restaurang 135026 som 2.

**Rekommendera objekt tooa angivna användare**

Genom att välja **objektet rekommendation** under **rekommenderare förutsägelse typ**, begär du hello rekommenderare system toorecommend objekt tooa ges användaren. hello sista parametern toochoose i det här scenariot är *rekommenderas val av återställningsobjekt*. Hej alternativet **från klassificerade av objekt (för utvärdering av modellen)** är främst avsedd för utvärdering av modellen under hello utbildning processen. För det här steget förutsägelse vi väljer **från alla objekt**. Hej visualisering av hello [poäng Matchbox rekommenderare] [ score-matchbox-recommender] utdata ser ut som figur 22.

![Poäng resultatet av rekommenderare system--objektet rekommendation](./media/machine-learning-interpret-model-results/22.png)

Figur 22. Visualisera poäng resultatet av hello rekommenderare system--objektet rekommendation

Hej första hello sex kolumner representerar hello angivna användar-ID: N toorecommend objekt, som tillhandahålls av hello indata. hello representerar andra fem kolumner hello objekt rekommenderas toohello användare i fallande ordning relevanta. Till exempel hello första raden bör hello mest restaurang för kund U1048 134986, följt av 135018, 134975, 135021 och 132862.

**Hitta användare relaterade tooa angivna användare**

Genom att välja **relaterade användare** under **rekommenderare förutsägelse typ**, begär du hello rekommenderare system toofind relaterade tooa angivna användare. Relaterade användare kan hello-användare som har liknande inställningar. hello sista parametern toochoose i det här scenariot är *relaterade användarens val*. Hej alternativet **från användare att klassificerade objekt (för utvärdering av modellen)** är främst avsedd för utvärdering av modellen under hello utbildning processen. Välj **från alla användare** för det här steget för förutsägelse. Hej visualisering av hello [poäng Matchbox rekommenderare] [ score-matchbox-recommender] utdata ser ut som bild 23.

![Poäng resultatet av rekommenderare system--relaterade användare](./media/machine-learning-interpret-model-results/23.png)

Figur 23. Visualisera poängresultat hello rekommenderare system--relaterade användare

Hej först av hello sex kolumner visar hello angivna användar-ID: N behövs toofind relaterade användare, som tillhandahålls av indata. hello andra fem kolumner store hello förväntade relaterade användare för hello användare i fallande ordning relevanta. Hello första raden är till exempel hello relevant kunden för kunden U1048 U1051, följt av U1066, U1044, U1017 och U1072.

**Sök efter objekt relaterade tooa beroende objekt**

Genom att välja **relaterade objekt** under **rekommenderare förutsägelse typ**, du ber hello rekommenderare system toofind relaterade objekt tooa beroende objekt. Relaterade objekt är hello objekt troligen toobe tyckte av hello samma användare. hello sista parametern toochoose i det här scenariot är *relaterade val av återställningsobjekt*. Hej alternativet **från klassificerade av objekt (för utvärdering av modellen)** är främst avsedd för utvärdering av modellen under hello utbildning processen. Vi väljer **från alla objekt** för det här steget för förutsägelse. Hej visualisering av hello [poäng Matchbox rekommenderare] [ score-matchbox-recommender] utdata ser ut som bild 24.

![Poäng resultatet av rekommenderare system, relaterade objekt](./media/machine-learning-interpret-model-results/24.png)

Figur 24. Visualisera poängresultat hello rekommenderare system--relaterade objekt

Hej första hello sex kolumner representerar hello angivna objekt-ID: N som behövs toofind relaterade objekt, som tillhandahålls av hello indata. hello andra fem kolumner store hello förväntade relaterade objekt för hello objektet i fallande ordning enligt betydelse. Hello första raden är till exempel hello relevant artikel för objektet 135026 135074, följt av 135035, 132875, 135055 och 134992.

**Tjänstepublicering för webbprogram**

hello-processen för att publicera dessa experiment som web services tooget förutsägelser liknar för varje hello fyra scenarier. Här vidtar vi hello andra scenariot (rekommenderar objekt tooa ges användaren) som ett exempel. Du kan följa hello samma procedur med hello andra tre.

Sparar hello tränats rekommenderare system som en tränad modell och filtrering hello indata tooa enanvändarläge ID-kolumnen som begärts, du kan koppla samman hello experiment som bild 25 och publicera den som en webbtjänst.

![Bedömningen experiment hello restaurang rekommendation problemet](./media/machine-learning-interpret-model-results/25.png)

Bild 25. Bedömningen experiment hello restaurang rekommendation problemet

Kör hello webbtjänst hello returnerade resultatet ser ut som bild 26. hello fem rekommenderade hotell för användaren U1048 är 134986, 135018, 134975, 135021 och 132862.

![Exempel på rekommenderare systemtjänst](./media/machine-learning-interpret-model-results/25_1.png)

![Exempel på resultat experiment](./media/machine-learning-interpret-model-results/26.png)

Bild 26. Web service resultatet av restaurang rekommendation problem

<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
