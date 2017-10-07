---
title: "aaaWhat är Azure Machine Learning Studio? | Microsoft Docs"
description: "Översikt av Azure ML Studio, ett drag-och-släpp-verktyg för att snabbt skapa modeller från bibliotek med algoritmer och moduler som redan är helt färdiga att använda."
keywords: azure machine learning,azure ml, ml studio
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: a25f2d9e75d088a37930de98240c6e14c9567fa4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-machine-learning-studio"></a>Vad är Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio är ett samarbete, dra och släpp verktyg som du kan använda toobuild, testa och distribuera prediktiva Analyslösningar utifrån dina data. Tjänsten Machine Learning Studio publicerar modeller som webbtjänster som enkelt kan användas av anpassade appar eller BI-verktyg som Excel.

Machine Learning Studio är platsen där datavetenskap, prediktiva analyser, molnresurser och dina data sammanstrålar.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-machine-learning-studio-interactive-workspace"></a>Hej interaktiva arbetsytan i Machine Learning Studio
toodevelop en prediktiv analysmodell du vanligtvis använder data från en eller flera källor, transformera och analysera data via datamodifieringar och statistiska funktioner och generera en uppsättning resultat. Att utveckla en modell som den här är en process som är baserad på upprepningar (iteration). När du ändrar Hej olika funktioner och deras parametrar, resultaten att Konvergera tills du är nöjd du har en tränad, effektiv modell.

**Azure Machine Learning Studio** ger du en interaktiv, visuell arbetsyta tooeasily bygga, testa och iterera på en prediktiv analysmodell. Du dra och släpp ***datauppsättningar*** och analys ***moduler*** till en interaktiva arbetsytan och koppla dem tillsammans tooform en ***experimentera***, som du kör i Machine Learning Studio. tooiterate med modelldesignen du hello experiment, spara en kopia om du vill redigera och kör det igen. När du är klar kan du konvertera ditt ***träningsexperiment*** tooa ***prediktivt experiment***, och sedan publicera den som en ***webbtjänsten*** så att din modell kan användas av övriga.

Det finns ingen programmering krävs, precis visuellt ansluter datauppsättningar och moduler tooconstruct en prediktiv analysmodell.

> [!TIP]
> toodownload och skriva ut ett diagram som ger en översikt över hello funktioner i Machine Learning Studio finns [Översiktsdiagram över funktioner i Azure Machine Learning Studio i](machine-learning-studio-overview-diagram.md).
> 
> 

![Diagram för Azure ML Studio: Skapa experiment, läs in data från flera källor, skriv in bedömda data, skriv modeller.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Kom igång med Machine Learning Studio
När du anger [Machine Learning Studio](https://studio.azureml.net) du se hello **Start** sidan. Härifrån kan du titta på dokumentation, videor, webbseminarier och hitta andra användbara resurser.

Klicka på hello övre vänstra menyn ![Meny](media/machine-learning-what-is-ml-studio/menu.png) och du ser flera alternativ.

### <a name="cortana-intelligence"></a>Cortana Intelligence
Klicka på **Cortana Intelligence** och vidarebefordras du toohello startsida hello [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite). Hej Cortana Intelligence Suite är en helt hanterad stordata och advanced analytics suite tootransform dina data i intelligent åtgärd. Se hello Suite startsidan för fullständig dokumentationen, inklusive kunden artiklar.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Det finns två alternativ, **Start**, hello sidan som du startade och **Studio**.

Klicka på **Studio** och vidarebefordras du toohello **Azure Machine Learning Studio**. Först blir du ombedd toosign in med ditt Microsoft-konto eller arbets-eller skolkonto. När inloggad, visas följande flikar hello vänster hello:

* **PROJEKT** – Samlingar med experiment, datauppsättningar, anteckningsböcker och andra resurser som representerar ett enskilt projekt
* **EXPERIMENT** – Experiment som du har skapat och kört eller sparat som utkast
* **WEBBTJÄNSTER** – Webbtjänster som du har distribuerat från dina experiment
* **NOTEBOOKS** – Jupyter-anteckningsböcker som du har skapat
* **DATAUPPSÄTTNINGAR** – Datauppsättningar som du har överfört till Studio
* **TRÄNADE MODELLER** – Modeller som du har tränat i experiment och sparat i Studio
* **INSTÄLLNINGAR för** – en samling inställningar som du kan använda tooconfigure ditt konto och resurserna.

### <a name="gallery"></a>Galleri
Klicka på **galleriet** och vidarebefordras du toohello  **[Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)**. hello galleriet är en plats där en grupp med dataanalytiker och utvecklare kan dela lösningar som skapats med hjälp av komponenter i hello Cortana Intelligence Suite.

Mer information om hello galleriet finns [dela och Upptäck lösningar i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Komponenter i ett experiment
Ett experiment består av datauppsättningar som förser data tooanalytical moduler som du kopplar sedan samman tooconstruct en prediktiv analysmodell. Mer konkret uttryckt så har ett giltigt experiment följande egenskaper:

* hello experimentet har minst en datauppsättning och en modul
* Datauppsättningar kan vara anslutet endast toomodules
* Moduler kan vara anslutna tooeither datauppsättningar eller andra moduler
* Alla indataportar för moduler måste ha vissa toohello anslutningsdata flöde
* Alla nödvändiga parametrar måste anges för varje modul

Du kan skapa ett experiment från grunden eller så kan du använda ett befintligt exempelexperiment som mall. Mer information finns i [kopiera exempel experimenten toocreate nya maskininlärningsexperiment](machine-learning-sample-experiments.md).

Om du vill se ett exempel på hur du kan skapa ett enkelt experiment kan du gå till [Skapa ett enkelt experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md).

Om du vill få en fullständig genomgång av att skapa en prediktiv analyslösning kan du gå till [Utveckla en prediktiv lösning med Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Datauppsättningar
En datauppsättning är data som har överförts tooMachine Learning Studio så att den kan användas i hello modellera processen. Ett antal provdatauppsättningar ingår i Machine Learning Studio för tooexperiment med och du kan föra över fler datauppsättningar efter behov. Här följer några exempel på datauppsättningar som ingår:

* **MPG-data för olika bilar** – MPG-värden (Miles Per Gallon, USA:s motsvarighet till liter per 100 km) för bilar beräknat utifrån antalet cylindrar, hästkrafter o.s.v.
* **Bröstcancerdata** – Data för bröstcancerdiagnoser.
* **Data om skogsbränder** – Storleken på skogsbränder i nordöstra Portugal.

När du skapar ett experiment du hello listan över tillgängliga datauppsättningar toohello vänsterkant hello arbetsytan.

En lista över provdatauppsättningar som ingår i Machine Learning Studio finns [använda hello provdatauppsättningar i Azure Machine Learning Studio](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Moduler
En modul är en algoritm som du kan tillämpa på dina data. Machine Learning Studio har ett antal moduler från data ingång funktioner tootraining bedömningen och validering processer. Här följer några exempel på moduler som ingår:

* [Konvertera tooARFF] [ convert-to-arff] -konverterar en .NET-serialiserade dataset tooAttribute-Relation File Format (ARFF).
* [Beräkna elementär statistik][elementary-statistics] – Beräknar elementär statistik, som medelvärde, standardavvikelse osv.
* [Linjär regression][linear-regression] – Skapar en linjär regressionsmodell baserad på brantaste lutningsmetoden online.
* [Bedömningsmodell][score-model] – Bedömer en tränad klassificerings- eller regressionsmodell.

När du skapar ett experiment du hello listan över tillgängliga moduler toohello vänsterkant hello arbetsytan.  

En modul kan ha en uppsättning parametrar som du kan använda tooconfigure hello modulens interna algoritmer. När du väljer en modul på arbetsytan hello hello modulens parametrar visas i hello **egenskaper** fönstret toohello höger i hello arbetsytan. Du kan ändra hello parametrar i den rutan tootune din modell.

Vissa hjälp navigera genom hello stora biblioteket med tillgängliga maskininlärningsalgoritmer finns [hur toochoose algoritmer för Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Distribuera en prediktiv analysmodell som webbtjänst
När din prediktiva analysmodell är färdig kan du distribuera den som en webbtjänst direkt från Machine Learning Studio. Mer information om den här processen finns i [Distribuera en webbtjänst via Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
