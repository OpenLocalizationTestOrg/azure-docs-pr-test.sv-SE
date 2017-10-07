---
title: "aaaUsing linjär Regression i Machine Learning | Microsoft Docs"
description: "En jämförelse av linjär regression modeller i Excel och i Azure Machine Learning Studio"
metakeywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 417ae6ab-de4f-4bdd-957a-d96133234656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: kbaroni;garye
ms.openlocfilehash: 8716040ad296053a72fb06c7c9660a186123ac15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-linear-regression-in-azure-machine-learning"></a>Använda linjär regression i Azure Machine Learning
> *Kate Baroni* och *Ben Boatman* är enterprise lösningsarkitekter i Microsofts Data insikter Center utmärkt. I den här artikeln beskriver de sina erfarenheter migrera en befintlig regression analys suite tooa molnbaserad lösning med hjälp av Azure Machine Learning. 
> 
> 

&nbsp; 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="goal"></a>Målet
Projektet igång med två mål i åtanke: 

1. Använd förutsägelseanalyser tooimprove hello riktighet företagets månatliga intäkter projektioner 
2. Använda Azure Machine Learning tooconfirm, optimera, öka hastighet och skala för våra resultat. 

Vår organisation genomgår en månatlig intäkter prognoser process som många företag. Vår små affärsanalytiker-teamet har gett med Azure Machine Learning toosupport hello processen och förbättra prognosens noggrannhet. hello-teamet har använt flera månader samla in data från flera källor och kör hello dataattribut via statistiska analyser identifierar nyckelattribut relevanta tooservices försäljningsprognoser. hello nästa steg var toobegin prototyper statistiska regression modeller på hello data i Excel. Inom några veckor hade vi en Excel-regressionsmodell som outperforming hello aktuella fältet och ekonomi prognoser processer. Detta blev hello baslinjen förutsägelse resultat. 

Vi sedan tog hello nästa steg toomoving våra förutsägelseanalyser över tooAzure Machine Learning toofind ut hur Maskininlärning kan förbättra förutsägbar prestanda.

## <a name="achieving-predictive-performance-parity"></a>Uppnå förutsägbar prestanda paritet
Vår högsta prioritet var tooachieve paritet mellan regression Machine Learning och Excel. Angivna hello samma data och Hej samma dela för träning och testning data, vill vi tooachieve förutsägbar prestanda paritet mellan Excel och Maskininlärning. Först kunde inte. hello Excel modell gick bättre än förväntat hello Machine Learning-modell. fel inträffade hello är på grund av tooa bristande förståelse för grundläggande hello-verktyget i Machine Learning. Vi fick en bättre förståelse för hello grundläggande inställning för våra datauppsättningar efter en synkronisering med hello Produktteamet för Maskininlärning och uppnås paritet mellan hello två modeller. 

### <a name="create-regression-model-in-excel"></a>Skapa regressionsmodell i Excel
Vår Excel Regression används hello standard linjär regressionsmodell i hello analysverktygens i Excel. 

Det beräknade *medelvärde absolut % fel* och den som hello prestandamått för hello modellen. Det tog tre månader tooarrive på en fungerande modell i Excel. Vi har utvecklat mycket hello learning till hello Machine Learning Studio-experiment som slutligen var bra i Förstå kraven.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Skapa jämförbara experiment i Azure Machine Learning
Vi har följt de här stegen toocreate vårt experiment i Machine Learning Studio: 

1. Överförda hello dataset som en CSV-fil tooMachine Learning Studio (liten fil)
2. Skapa ett nytt experiment och användas hello [Välj kolumner i datauppsättning] [ select-columns] modulen tooselect hello samma datafunktioner som används i Excel 
3. Använda hello [dela Data] [ split] modul (med *relativa uttryck* läge) toodivide hello data till hello samma utbildning datauppsättningar som har gjorts i Excel 
4. Redan experimenterat lite med hello [linjär Regression] [ linear-regression] modul (standardalternativen endast), dokumenterade och jämförs hello resultat tooour Excel regressionsmodell

### <a name="review-initial-results"></a>Granska resultatet från första
Först hello Excel modellen tydligt gick bättre än förväntat hello Machine Learning Studio modellen: 

|  | Excel | Studio |
| --- |:---:|:---:|
| Prestanda | | |
| <ul style="list-style-type: none;"><li>Justeras R ruta</li></ul> |0.96 |Saknas |
| <ul style="list-style-type: none;"><li>Värde för <br />Bestämning</li></ul> |Saknas |0.78<br />(låg noggrannhet) |
| Medelabsolutfel |$9. 5M |$ 19.4 M |
| Medelabsolutfel (%) |6.03% |12.2% |

När vi körde våra processen och resultat hello utvecklare och datavetare på hello Machine Learning-teamet angetts de snabbt några användbara tips. 

* När du använder hello [linjär Regression] [ linear-regression] modul i Machine Learning Studio finns två metoder:
  * Online toningen härstammar: Kanske passar bättre för större skala problem
  * Vanlig minsta kvadrat: Detta är hello metod de flesta tänka på när de hör linjär regression. Vanlig minsta kvadrat kan vara mer föredra för små datauppsättningar.
* Överväg att modifiera hello L2 Regularization vikt parametern tooimprove prestanda. Det är som standard too0.001, men för våra liten datamängd vi ställer du in den too0.005 tooimprove prestanda. 

### <a name="mystery-solved"></a>Okända löst!
När vi tillämpas hello rekommendationer vi uppnås hello samma baslinje-prestanda i Machine Learning Studio som Excel: 

|  | Excel | Studio (första) | Studio med minsta kvadrat |
| --- |:---:|:---:|:---:|
| Märkt värde |Verkliga (numeriskt) |samma |samma |
| Deltagaren |Excel -> Data Analysis -> Regression |Linjär Regression. |Linjär Regression |
| Deltagaren alternativ |Saknas |Som standard |vanlig minsta kvadrat<br />L2 = 0,005 |
| Datauppsättning |26 rader, 3 funktioner, 1 etikett. Alla numeriska. |samma |samma |
| Dela: Train |Excel tränats på hello först 18 rader, testas på hello senast 8 rader. |samma |samma |
| Dela: Test |Excel-regressionsformeln tillämpas toohello senaste 8 rader |samma |samma |
| **Prestanda** | | | |
| Justeras R ruta |0.96 |Saknas | |
| Bestämningskoefficienten |Saknas |0.78 |0.952049 |
| Medelabsolutfel |$9. 5M |$ 19.4 M |$9. 5M |
| Medelabsolutfel (%) |<span style="background-color: 00FF00;"> 6.03%</span> |12.2% |<span style="background-color: 00FF00;"> 6.03%</span> |

Dessutom hello Excel koefficienter jämfört med bra toohello funktionen vikter i hello Azure tränade modellen:

|  | Excel koefficienter | Azure-funktionen vikterna |
| --- |:---:|:---:|
| Skärningspunkt/Bias |19470209.88 |19328500 |
| Funktion A |0.832653063 |0.834156 |
| Funktion B |11071967.08 |11007300 |
| Funktionen C |25383318.09 |25140800 |

## <a name="next-steps"></a>Nästa steg
Vi vill tooconsume hello Machine Learning-webbtjänsten i Excel. Vår affärsanalytiker förlitar sig på Excel och det behövs ett sätt toocall hello Machine Learning webbtjänsten med en rad med Excel-data och låta den returnera hello förutsade värdet tooExcel. 

Vi ville också toooptimize vår modell med hello alternativ och algoritmer som är tillgängliga i Machine Learning Studio.

### <a name="integration-with-excel"></a>Integrering med Excel
Vår lösning var toooperationalize våra Machine Learning regression modellen genom att skapa en webbtjänst från hello tränad modell. Hello-webbtjänsten har skapats och vi kan kalla den direkt från Excel tooreturn beräknade intäktsvärdet inom några minuter. 

Hej *Web Services Dashboard* avsnittet innehåller en nedladdningsbar Excel-arbetsbok. hello arbetsboken innehåller före formaterad hello web API och schema tjänstinformation inbäddade. När du klickar på *hämta Excel-arbetsboken*, hello arbetsboken öppnas och du kan spara den tooyour lokal dator. 

![][1]

Kopiera fördefinierade parametrarna till hello blå parametern avsnitt med hello-arbetsbok är öppen, enligt nedan. När hello parametrar har angetts Excel visar toohello Machine Learning-webbtjänst och hello förväntade poängsatta etiketter visas under hello grön förutsagda värden. hello arbetsboken fortsätter toocreate förutsägelser för parametrar utifrån din tränad modell för alla rad-element som anges under parametrar. Mer information om hur toouse denna funktion finns [förbrukar en Azure Machine Learning-webbtjänst från Excel](machine-learning-consuming-from-excel.md). 

![][2]

### <a name="optimization-and-further-experiments"></a>Optimering och ytterligare experiment
Nu när vi hade en baslinje med vår modell i Excel flyttat vi ahead toooptimize våra linjär Regression Maskininlärningsmodell. Vi använde hello modulen [Filter-baserade Funktionsurval] [ filter-based-feature-selection] tooimprove på vår urvalet av inledande dataelement och hjälp oss att uppnå en prestandaförbättring 4.6% medelvärdet av absoluta fel. Vi använder den här funktionen som kan spara oss veckor i söka igenom data attribut toofind hello rätt uppsättning funktioner toouse för modellering för framtida projekt. 

Nästa vi planerar att tooinclude andra algoritmer som [Bayesian] [ bayesian-linear-regression] eller [Tvåklassförhöjt beslutsträd] [ boosted-decision-tree-regression] i vårt experiment toocompare prestanda. 

Om du vill tooexperiment med regression är en bra dataset tootry hello energi effektivitet Regression exempeluppsättningen som har många numeriska attribut. hello dataset tillhandahålls som en del av hello provdatauppsättningar i Machine Learning Studio. Du kan använda olika learning moduler toopredict uppvärmning belastning eller kylning belastning. hello diagrammet nedan är en prestanda jämförelse av olika regression lär sig mot hello energieffektivitet dataset förutsäga för hello mål variabeln kylning belastningen: 

| Modellen | Medelabsolutfel | Roten medelvärdet kvadrat fel | Relativa absoluta fel | Relativ kvadrat fel | Bestämningskoefficienten |
| --- | --- | --- | --- | --- | --- |
| Tvåklassförhöjda beslutsträdet |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Linjär Regression (toning härstammar) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Neural Network Regression |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Linjär Regression (vanlig minsta kvadrat) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Viktiga Takeaways
Vi lärt dig mycket av körs Excel regression och Azure Machine Learning-experimenten parallellt. Skapa hello baslinjen modell i Excel och jämföra dem med hjälp av Machine Learning toomodels [linjär Regression] [ linear-regression] hjälpte oss Läs Azure Machine Learning och vi identifierade affärsmöjligheter tooimprove data prestanda för urvalet och modell. 

Det är lämpligt toouse påträffades också [Filter-baserade Funktionsurval] [ filter-based-feature-selection] tooaccelerate framtida förutsägelse projekt. Genom att använda funktionen val tooyour data kan skapa du en förbättrad modell i Machine Learning med bättre prestanda. 

hello möjlighet tootransfer hello förutsägande analytiska prognoser från Machine Learning tooExcel hela organismen ger mycket större i hello möjlighet toosuccessfully ger resultat tooa bred business målgrupp. 

## <a name="resources"></a>Resurser
Här följer några resurser för att hjälpa dig att arbeta med regression: 

* Regression i Excel. Om du aldrig har försökt regression i Excel, den här kursen är det enkelt: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regression vs prognoser. Tyler Chessman skrev en bloggartikel som förklarar hur toodo time series Prognosticering i Excel, som innehåller en bra nybörjare beskrivning av linjär regression. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-Forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts) 
* Vanlig minst linjär Regression: Fel, problem och fallgropar. En introduktion och en beskrivning av Regression: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

