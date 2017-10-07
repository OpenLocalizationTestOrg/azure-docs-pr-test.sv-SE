---
title: aaaCreate text analytics modeller i Azure Machine Learning Studio | Microsoft Docs
description: "Hur toocreate textanalys modeller i Azure Machine Learning Studio med moduler för text förbearbetning, N g eller hash-funktionen"
services: machine-learning
documentationcenter: 
author: rastala
manager: jhubbard
editor: 
ms.assetid: 08cd6723-3ae6-4e99-a924-e650942e461b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: roastala
ms.openlocfilehash: e3799f37ba54bb2ec8815ecf5ed34e145ffb20e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Skapa textanalysmodeller i Azure Machine Learning Studio
Du kan använda Azure Machine Learning toobuild och operationalisera text analytics modeller. Med hjälp av dessa modeller kan du till exempel lösa problem med dokumentet klassificering eller sentiment analys.

I en text analytics experiment skulle du normalt:

1. Rensa och Förbearbeta text dataset
2. Extrahera numeriska funktionen angreppsmetoderna från förbearbetade text
3. Klassificerings- eller regressionsmodell träningsmodell
4. Poängsätta och validera hello modellen
5. Distribuera hello modellen tooproduction

I kursen får du lära dig stegen som vi går igenom en sentiment analysmodell använder Amazon Book granskningar dataset (finns i det här dokumentet, research ”biografier, Bollywood, BOM rutor och blandningsföretag: domän anpassning för Sentiment klassificering” av John Blitzer Markera Dredze och Fernando Pereira; Koppling av beräkningar Linguistics (ACL) 2007.) Den här datauppsättningen består av Granska resultat (1 till 2 eller 4-5) och en valfri form text. hello målet är toopredict hello, granska poäng: låg (1 - 2) eller hög (4-5).

Du kan hitta experiment som beskrivs i den här självstudiekursen i Cortana Intelligence Gallery:

[Förutsäga Book granskningar](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Förutsäga Book granskningar - Prediktivt Experiment](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Steg 1: Rensa och Förbearbeta text dataset
Vi börjar hello experiment genom att dela hello Granska resultat i kategoriska låg och hög buckets tooformulate hello problem som tvåklass klassificering. Vi använder [redigera Metadata](https://msdn.microsoft.com/library/azure/dn905986.aspx) och [Kategoriska Gruppvärden](https://msdn.microsoft.com/library/azure/dn906014.aspx) moduler.

![Skapa etikett](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Sedan rensning hello text med [Förbearbeta Text](https://msdn.microsoft.com/library/azure/mt762915.aspx) modul. hello Rensa minskar hello brus i hello dataset, hjälpa dig hitta hello de viktigaste funktionerna och förbättra hello riktighet hello sista modellen. Vi ta bort stoppord - vanliga ord som ”” eller ”a” - och siffror, specialtecken, duplicerade tecken, e-postadresser och URL: er. Vi också konvertera hello text toolowercase lemmatize hello ord och identifiera meningen gränser som sedan visas som ”|||” symbol i förbearbetade text.

![Förbearbeta Text](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Om du vill att toouse en egen lista över stoppord? Du kan överföra den i som valfria indata. Du kan också använda anpassade C#-syntax reguljära tooreplace delsträngar och ta bort ord som en del av tal: substantiv, verb eller adjektiv.

När hello förbearbetning är klar kan vi dela hello data i tåg och testa uppsättningar.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Steg 2: Extrahera numeriska funktionen angreppsmetoderna från förbearbetade text
toobuild en modell för textdata du vanligtvis har tooconvert fritt format text i numeriska funktionen angreppsmetoderna. I det här exemplet använder vi [extrahera N Gram funktioner från Text](https://msdn.microsoft.com/library/azure/mt762916.aspx) modulen tootransform hello data toosuch textformat. Den här modulen tar en kolumn med avgränsade med blanksteg ord och beräknar en ordlista med ord eller N-g ord som förekommer i dina dataset. Sedan Räknar hur många gånger varje word eller N gram visas i varje post och skapar angreppsmetoderna funktionen från dem räknar. I den här självstudiekursen ange vi N gram storlek too2 så våra funktionen angreppsmetoderna omfattar kombinationer av två efterföljande ord och enstaka ord.

![Extrahera N g](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Det använda TF * IDF (termen frekvens inverterade dokumentet frekvens) viktning tooN gram räknar. Den här metoden lägger till vikten av ord som ofta visas i en enskild post men är ovanliga över hello hela datamängden. Andra alternativ inkluderar binary TF, och ett diagram över väger.

Textfunktioner har ofta högt dimensionalitet. Till exempel om din Kristi har 100 000 unika ord, skulle funktionen-utrymme ha 100 000 dimensioner eller mer om N gram används. hello extrahera N Gram funktioner modulen ger en uppsättning alternativ tooreduce hello dimensionalitet. Du kan välja tooexclude ord som är kort eller lång, eller för ovanliga eller för många toohave betydande värde. I den här självstudiekursen exkludera vi N gram som visas i färre än 5 poster eller mer än 80% av poster.

Du kan också använda funktionen val tooselect endast de funktioner som är mest hello korrelerade med mål-förutsägelse. Vi använder chi2 funktion markeringen tooselect 1000 funktioner. Du kan visa hello ordförråd för markerade ord eller N gram genom att klicka på hello rätt utdata från extrahera N-g-modulen.

Du kan använda funktionen Hashing modulen som en alternativ metod toousing extrahera N Gram funktioner. Observera dock att [funktions-hashning](https://msdn.microsoft.com/library/azure/dn906018.aspx) har inte inbyggda val av funktioner eller TF * IDF väger.

## <a name="step-3-train-classification-or-regression-model"></a>Steg 3: Klassificerings- eller regressionsmodell träningsmodell
Hello text har nu omvandlade toonumeric funktionen kolumner. hello datauppsättningen fortfarande innehåller strängen kolumner från föregående steg så att vi använda Välj kolumner i datauppsättning tooexclude dem.

Vi använder sedan [Two-Class Logistic Regression](https://msdn.microsoft.com/library/azure/dn905994.aspx) toopredict våra mål: granska för hög eller låg poäng. Hello text analytics problem har nu tagits omvandlas till en vanlig klassificeringsproblem. Du kan använda hello verktyg som finns tillgängliga i Azure Machine Learning tooimprove hello modellen. Du kan experimentera med olika klassificerare toofind ut hur korrekta resultat de ge eller använda hyperparameter justera tooimprove hello noggrannhet.

![Tåg och poäng](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-hello-model"></a>Steg 4: Poängsätta och validera hello modellen
Hur vill du validera hello tränade modellen? Vi poängsätta mot hello testdata och utvärdera hello noggrannhet. Dock hello modellen lärt dig hello transformering N g och deras vikt från hello utbildning dataset. Därför ska användas som ordförråd och dessa vikter när extraherar funktioner från testdata, som motsats toocreating hello ordförråd på nytt. Därför vi lägga till funktioner extrahera N Gram modulen toohello bedömningen grenen av hello experiment ansluta hello utdata ordförråd från utbildning gren och ange hello ordförråd läget endast tooread. Vi också inaktivera hello filtrering på N-g av frekvensen genom att ange hello minsta too1 instans och maximala too100% och inaktivera hello val av funktioner.

När hello textkolumn i testet har omvandlas toonumeric funktionen kolumner, exkludera vi hello sträng kolumner från föregående steg som i utbildning grenen. Sedan använder vi poängsätta modell modulen toomake förutsägelser och utvärdera modell modulen tooevaluate hello noggrannhet.

## <a name="step-5-deploy-hello-model-tooproduction"></a>Steg 5: Distribuera hello modellen tooproduction
hello modellen är nästan klara toobe distribueras tooproduction. När du distribuerade som webbtjänsten den tar Fritextfält sträng som inmatning och returnera en förutsägelse ”hög” eller ”låg”. Den använder hello lärt dig N gram ordförråd tootransform hello text toofeatures och tränats logistic regression modellen toomake en förutsägelse från dessa funktioner. 

tooset in hello prediktivt experiment vi först spara hello N gram ordförråd som dataset och hello tränats logistic regressionsmodell från hello utbildning grenen av hello experiment. Sedan kan spara vi hello experiment med ”Spara som” toocreate ett experiment diagram för prediktivt experiment. Vi kan ta bort hello dela Data modulen och hello utbildning gren från hello experiment. Vi ansluter sedan hello sparat tidigare N gram ordförråd och modellen tooExtract N Gram funktioner och Poängmodell moduler respektive. Vi har också ta bort hello utvärdera modell.

Vi Infoga Välj kolumner i datauppsättning modulen innan Förbearbeta modulen tooremove hello etikett textkolumn och avmarkerar alternativet ”Lägg till poäng kolumnen toodataset” i modulen poängsätta. På så sätt kan hello webbtjänsten inte begär hello etikett den försöker toopredict och inte echo hello inkommande funktioner i svaret.

![Prediktivt Experiment](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Nu har vi ett experiment som kan publiceras som en webbtjänst och anropas med begäran och svar eller batch execution API: er.

## <a name="next-steps"></a>Nästa steg
Lär dig mer om text analytics moduler från [MSDN-dokumentationen](https://msdn.microsoft.com/library/azure/dn905886.aspx).

