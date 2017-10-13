---
title: "Vad är Azure Machine Learning Studio? | Microsoft Docs"
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
ms.openlocfilehash: 923bf1163e4d27e8c453fc2fcd58ebb80222bd6a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-machine-learning-studio"></a>Vad är Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio är ett drag-och-släpp-verktyg där flera användare kan samarbeta för att bygga, testa och distribuera prediktiva analyslösningar utifrån dina data. Tjänsten Machine Learning Studio publicerar modeller som webbtjänster som enkelt kan användas av anpassade appar eller BI-verktyg som Excel.

Machine Learning Studio är platsen där datavetenskap, prediktiva analyser, molnresurser och dina data sammanstrålar.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Den interaktiva arbetsytan i tjänsten Machine Learning Studio
För att skapa en prediktiv analysmodell använder du normalt data från eller flera källor, omvandlar och analyserar dessa data genom att utföra en serie datamodifieringar och statistiska funktioner och sedan genererar du en uppsättning resultat. Att utveckla en modell som den här är en process som är baserad på upprepningar (iteration). När du ändrar olika funktionerna och deras parametrar kommer resultaten att konvergera tills du är nöjd och säker på att du har en tränad, effektiv modell.

**Azure Machine Learning Studio** ger dig en interaktiv, visuell arbetsyta där du kan bygga, testa och utföra iterationer på en prediktiv analysmodell. Du drar och släpper ***datauppsättningar*** och ***analysmoduler*** till en interaktiv ***arbetsyta***, kopplar samman dem för att skapa ett experiment och sedan kör du dessa i Machine Learning Studio. Om du vill köra en iteration med modelldesignen, redigerar du experimentet, sparar en kopia om du så önskar och sedan kör du försöket igen. När du är klar kan du konvertera ditt ***träningsexperiment*** till ett ***prediktivt experiment***, och sedan ***publicera*** det som en webbtjänst så att din modell kan användas av andra.

Ingen programmering krävs. Det enda du behöver göra är att koppla samman datauppsättningar och moduler visuellt för att skapa en prediktiv analysmodell.

> [!TIP]
> Se [Översiktsdiagram över funktionerna i Azure Machine Learning Studio](studio-overview-diagram.md) om du vill ladda ned och skriva ut ett diagram med en översikt över funktionerna i Machine Learning Studio.
> 
> 

![Diagram för Azure ML Studio: Skapa experiment, läs in data från flera källor, skriv in bedömda data, skriv modeller.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Kom igång med Machine Learning Studio
När du startar [Machine Learning Studio](https://studio.azureml.net) kommer du att se **startsidan**. Härifrån kan du titta på dokumentation, videor, webbseminarier och hitta andra användbara resurser.

Klicka på menyn uppe till vänster ![Meny](./media/what-is-ml-studio/menu.png) och du ser flera alternativ.

### <a name="cortana-intelligence"></a>Cortana Intelligence
Om du klickar på **Cortana Intelligence** öppnas startsidan för [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite). Cortana Intelligence är en fullständigt hanterad programsvit för stordata och avancerad analys som hjälper dig att omvandla data till intelligenta åtgärder. På programsvitens startsida finns fullständig dokumentation, inklusive kundberättelser.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Det finns två alternativ här: **Start**, vilket är den sida där du startade, och **Studio**.

Klicka på **Studio**, så kommer du till **Azure Machine Learning Studio**. Först uppmanas du att logga in med ditt Microsoft-konto eller ditt arbets- eller skolkonto. När du har loggat in visas följande flikar till vänster:

* **PROJEKT** – Samlingar med experiment, datauppsättningar, anteckningsböcker och andra resurser som representerar ett enskilt projekt
* **EXPERIMENT** – Experiment som du har skapat och kört eller sparat som utkast
* **WEBBTJÄNSTER** – Webbtjänster som du har distribuerat från dina experiment
* **NOTEBOOKS** – Jupyter-anteckningsböcker som du har skapat
* **DATAUPPSÄTTNINGAR** – Datauppsättningar som du har överfört till Studio
* **TRÄNADE MODELLER** – Modeller som du har tränat i experiment och sparat i Studio
* **INSTÄLLNINGAR** – En uppsättning av inställningar som du kan använda för att konfigurera ditt konto och resurserna.

### <a name="gallery"></a>Galleri
Klicka på fliken **Galleri**, så kommer du till **[Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)**. Galleriet är en plats där grupper med dataanalytiker och utvecklare kan dela lösningar som skapats med hjälp av komponenter i Cortana Intelligence Suite.

Mer information om galleriet finns i [Dela och upptäck lösningar i Cortana Intelligence Gallery](gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Komponenter i ett experiment
Ett experiment består av datauppsättningar som förser de analytiska modulerna med data. Du kopplar sedan samman dessa moduler för att skapa en prediktiv analysmodell. Mer konkret uttryckt så har ett giltigt experiment följande egenskaper:

* Experimentet har minst en datauppsättning och en modul
* Datauppsättningar kan bara anslutas till moduler
* Moduler kan anslutas antingen till datauppsättningar eller till andra moduler
* Alla indataportar för moduler måste ha någon typ av koppling till dataflödet
* Alla nödvändiga parametrar måste anges för varje modul

Du kan skapa ett experiment från grunden eller så kan du använda ett befintligt exempelexperiment som mall. Mer information finns i avsnittet [Kopiera exempelexperiment för att skapa nya maskininlärningsexperiment](sample-experiments.md).

Om du vill se ett exempel på hur du kan skapa ett enkelt experiment kan du gå till [Skapa ett enkelt experiment i Azure Machine Learning Studio](create-experiment.md).

Om du vill få en fullständig genomgång av att skapa en prediktiv analyslösning kan du gå till [Utveckla en prediktiv lösning med Azure Machine Learning](walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Datauppsättningar
En datauppsättning är data som har överförts till Machine Learning Studio så att de kan användas i modelleringsprocessen. Ett antal provdatauppsättningar ingår i Machine Learning Studio för att du ska kunna experimentera med dem. Och du kan föra över fler datauppsättningar efter behov. Här följer några exempel på datauppsättningar som ingår:

* **MPG-data för olika bilar** – MPG-värden (Miles Per Gallon, USA:s motsvarighet till liter per 100 km) för bilar beräknat utifrån antalet cylindrar, hästkrafter o.s.v.
* **Bröstcancerdata** – Data för bröstcancerdiagnoser.
* **Data om skogsbränder** – Storleken på skogsbränder i nordöstra Portugal.

Medan du skapar ett experiment kan välja bland alternativ från listan över tillgängliga datauppsättningar på arbetsytans vänstra sida.

Om du vill se en lista över provdatauppsättningar som ingår i Machine Learning Studio, kan du gå till [Använda provdatauppsättningar i Azure Machine Learning Studio](use-sample-datasets.md).

### <a name="modules"></a>Moduler
En modul är en algoritm som du kan tillämpa på dina data. Machine Learning Studio har ett antal moduler, med allt från dataåtkomstfunktioner och träning till bedömning och processer för verifiering. Här följer några exempel på moduler som ingår:

* [Konvertera till ARFF][convert-to-arff] – Konverterar en .NET-serialiserad datauppsättning till formatet ARFF (Attribute-Relation File Format).
* [Beräkna elementär statistik][elementary-statistics] – Beräknar elementär statistik, som medelvärde, standardavvikelse osv.
* [Linjär regression][linear-regression] – Skapar en linjär regressionsmodell baserad på brantaste lutningsmetoden online.
* [Bedömningsmodell][score-model] – Bedömer en tränad klassificerings- eller regressionsmodell.

Medan du skapar ett experiment kan välja bland alternativ från listan över tillgängliga moduler på arbetsytans vänstra sida.  

En modul kan ha en uppsättning parametrar som du kan använda för att konfigurera modulens interna algoritmer. När du väljer en modul på arbetsytan modulens visas modulens parametrar i fönstret **Egenskaper** på arbetsytans högra sida. Du kan ändra parametrarna i det här fönstret för att finjustera din modell.

Om du vill få hjälp med att navigera genom det stora biblioteket med tillgängliga maskininlärningsalgoritmer kan du gå till [Så här väljer du algoritmer för Microsoft Azure Machine Learning](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Distribuera en prediktiv analysmodell som webbtjänst
När din prediktiva analysmodell är färdig kan du distribuera den som en webbtjänst direkt från Machine Learning Studio. Mer information om den här processen finns i [Distribuera en webbtjänst via Azure Machine Learning](publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
