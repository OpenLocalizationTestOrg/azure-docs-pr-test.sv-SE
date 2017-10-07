---
title: aaaMachine learning algoritmen fusklapp | Microsoft Docs
description: "Utskrivbara machine learning-algoritmen fusklapp hjälper dig att välja rätt hello-algoritm för din förutsägelsemodell i Azure Machine Learning Studio."
keywords: "algoritmen fusklapp, fusklapp, maskininlärningsalgoritmen"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e1dc31ec-1acb-463f-ba77-de565d4ddf4d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: garye
ms.openlocfilehash: 2bedd77bfc65128a90c6128743016415dd8609d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-algorithm-cheat-sheet-for-microsoft-azure-machine-learning-studio"></a>Facit för Machine Learning-algoritm i Microsoft Azure Machine Learning Studio
Hej **Microsoft Azure Machine Learning algoritmen Cheat blad** hjälper dig att välja rätt hello-algoritmen för en modell för förutsägelseanalys.

[Azure Machine Learning Studio](https://studio.azureml.net/) har stora biblioteket med algoritmer från hello ***regression***, ***klassificering***, ***clustering***, och ***avvikelseidentifiering*** familjer. Varje är utformad tooaddress en annan typ av machine learning problem.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>Hämta: Algoritmen fusklapp för Maskininlärning
**Hämta hello fusklapp här: [Machine Learning algoritmen Cheat blad (11 x 17 tum)](http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)**

![Datorn Inlärningsalgoritmen fusklapp: Lär dig hur toochoose en Machine Learning-algoritm.][cheat-sheet]

[cheat-sheet]: ./media/machine-learning-algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

Hämta och skriva ut hello Machine Learning algoritmen Cheat blad i tabloid storlek tookeep den praktiska och få hjälp med att välja en algoritm.

> [!NOTE]
> Se artikeln hello [hur toochoose algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) för en detaljerad guide toousing denna fusklapp.
> 
> 

## <a name="more-help-with-algorithms"></a>Mer hjälp med algoritmer
* Hjälp med att använda den här fusklapp för att välja rätt hello-algoritmen, plus en ingående beskrivning av hello olika typer av maskininlärningsalgoritmer och hur de används finns [hur toochoose algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).
* En nedladdningsbar infographic som beskriver algoritmer och exempel finns [nedladdningsbara Infographic: maskin lär du dig grunderna med algoritmen exempel](machine-learning-basics-infographic-with-algorithm-examples.md).
* En lista efter kategori av alla hello tillgängliga maskininlärningsalgoritmer i Machine Learning Studio finns [initiera modell] [ initialize-model] i modulen hjälpen och hello Machine Learning Studio-algoritmen.
* En fullständig alfabetisk lista över algoritmer och moduler i Machine Learning Studio finns [A-Ö-listan över Machine Learning Studio moduler] [ a-z-list] i Machine Learning Studio algoritmen och hjälpa till att modulen.
* toodownload och skriva ut ett diagram som ger en översikt över hello funktioner i Machine Learning Studio finns [Översiktsdiagram över funktioner i Azure Machine Learning Studio i](machine-learning-studio-overview-diagram.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="notes-and-terminology-definitions-for-hello-machine-learning-algorithm-cheat-sheet"></a>Anteckningar och termer för hello maskininlärningsalgoritmen cheat blad

* hello förslag som erbjuds i den här algoritmen fusklapp är ungefärliga regler för USB. Vissa kan böjas och vissa kan vara flagrantly bröt mot. Det här är avsedda toosuggest startpunkt. Inte tyvärr toorun en jämförelse konkurrensen mellan flera algoritmer på dina data. Det är bara ingen ersättning för förstå hello principer med varje algoritm och förstå hello system som genererats av dina data.

* Varje maskininlärningsalgoritmen har sin egen formatmall eller *induktiv bias*. Flera algoritmer kan vara lämpligt för ett specifikt problem och en algoritm kan vara en passar bättre än andra. Men det är inte alltid möjligt tooknow i förväg som är bäst för hello. I sådana fall visas flera algoritmer tillsammans i hello fusklapp. En lämplig strategi skulle tootry en algoritm och om hello resultatet inte är tillräckliga, försök hello andra. Här är ett exempel från hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) av ett experiment som används för flera algoritmer mot hello samma data och jämför hello resultat: [jämför flera klassen klassificerare: enhetsbokstaven igenkänning ](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

* Det finns tre huvudsakliga kategorier av machine learning: **övervakad inlärning**, **oövervakad inlärning**, och **förstärkning learning**.

  * I **övervakad inlärning**, varje datapunkt etikett eller kopplas till en kategori eller ett värde av intresse.  Ett exempel på en kategoriska etikett tilldelar en avbildning som en katt eller en hund.  Hello försäljningspriset som är associerade med en bil som används är ett exempel på en etikett för värdet. hello målet med övervakad inlärning är toostudy många heter exempel sådana och toobe kan toomake förutsägelser om framtida datapunkter. Till exempel identifiera nya foton med rätt djuret eller tilldela korrekt försäljning priser tooother används bilar hello. Detta är ett populärt och praktiskt typ av maskininlärning. Alla hello moduler i Azure Machine Learning är övervakad inlärning algoritmer utom för [K-Means Clustering][k-means-clustering].

  * I **oövervakad inlärning**, datapunkterna har inga etiketter. I stället hello målet med en algoritm för oövervakad inlärning är tooorganize hello data på vissa sätt toodescribe dess struktur. Detta kan innebära att gruppera den i kluster som K-means eller hitta olika sätt att granska komplexa data så att den visas enklare.

  * I **förstärkning learning**, hello algoritmen hämtar toochoose en åtgärd i svaret tooeach datapunkt. Det är en vanlig metod i robotics där hello uppsättning sensoravläsningar vid en punkt i tiden är en datapunkt och hello algoritmen måste välja hello robot nästa åtgärd. Det är också en fysisk plats för Sakernas Internet-program. Hej Inlärningsalgoritmen får också en ersättning signal som har en kort stund senare, som anger hur bra hello beslut. Utifrån detta ändrar hello-algoritmen sin strategi i ordning tooachieve hello högsta ersättning. Det finns inga förstärkning learning i Azure ML-algoritmen moduler.

* **Bayesian metoder** Se hello antagandet om statistiskt oberoende datapunkter. Detta innebär att hello unmodeled variationer i en datapunkt är uncorrelated med andra, det vill säga den kan inte förutsägas. Till exempel om hello data spelas in hello antal minuter tills hello nästa språng train anländer isär två mätningarna för en dag är statistiskt oberoende. Dock isär två mätningarna för en minut är inte statistiskt oberoende - hello-värdet för en hög förutsägande för hello värdet hello andra.

* **Ökat beslut trädet regression** drar nytta av funktionen överlappar varandra eller interaktion mellan funktioner. Att innebär att i alla angivna data pekar, hello värdet för en funktion är något förutsägande för hello-värdet i en annan. Till exempel i den dagliga hög/låg temperatur data kan veta hello låg temperatur hello dag du toomake en rimlig gissning på hello hög. hello uppgifterna i hello två funktioner är något redundant.

* Klassificera data i mer än två kategorier kan göras genom att använda en kombination flera klassen klassificerare eller genom att kombinera en uppsättning tvåklass klassificerare i en **ensemble**. Det finns en separat tvåklass klassificerare för varje klass i hello ensemble metoden – var och en avgränsar hello data i två kategorier: ”den här klassen” och ”inte den här klassen”. Sedan rösta dessa klassificerare på hello rätt tilldelningen av hello datapunkt. Detta är hello operativa princip bakom [en eller alla Multiclass][one-vs-all-multiclass].

* Flera metoder, inklusive logistic regression och hello Bayes point-dator, förutsätter **linjär klassgränser**. Det vill säga de förutsätter att hello gränserna mellan klasser är ungefär raka linjer (eller hyperplanes i hello mer allmän fall). Det här är ofta en egenskap av hello data som du inte känner till när du har gjort tooseparate den, men det är något som vanligtvis kan hämtas av visualisera i förväg. Om hello klassgränser ser mycket oregelbundna kan behålla med beslutsträd, beslut jungles, support vector datorer eller neurala nätverk.

* Neurala nätverk kan användas med kategoriska variabler genom att skapa en **dummy variabeln** för varje kategori, ange värdet too1 i fall där hello kategori gäller, 0 där den inte.


<!-- This is how you can embed a link in an image in HTML. Don't know how toodo this in markdown.
<a href="http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet.pdf">
<img src="C:\Users\garye\azure-docs-pr\articles\media\machine-learning-algorithm-cheat-sheet\cheat-sheet-small.png">
</a>
-->

<!-- Module References -->
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx
[initialize-model]: https://msdn.microsoft.com/library/azure/0c67013c-bfbc-428b-87f3-f552d8dd41f6/
[k-means-clustering]: https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/
[one-vs-all-multiclass]: https://msdn.microsoft.com/library/azure/7191efae-b4b1-4d03-a6f8-7205f87be664/
