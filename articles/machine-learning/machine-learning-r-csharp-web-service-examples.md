---
title: "aaa(deprecated) Machine learning-webbtjänster exempel som skapats med R - Azure | Microsoft Docs"
description: "(föråldrad) Hitta en bra uppsättning web services exempel skapats med R-koden och Machine Learning och publicerats toohello Azure Marketplace."
keywords: csharp r-koden, web services-exempel
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 97d66cb7-6a84-4ae9-8095-0b5f5ba82d7f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: jaymathe
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 20b074d38e65aed907d40549bb61f124cb5dfe1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-web-services-examples-using-r-code-on-azure-machine-learning-and-published-toomicrosoft-azure-marketplace"></a>(föråldrad) Exempel med hjälp av R-koden på Azure Machine Learning och publicerade tooMicrosoft Azure Marketplace-webbtjänster

> [!NOTE]
> hello Microsoft DataMarket dras och dessa API: er är inaktuella. 
> 
> Du hittar många användbara exempel experiment och API: er i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Mer information om hello galleriet finns [resursen och identifiera resurser i hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

I den här artikeln är exempel web services har skapats med Azure Machine Learning och publiceras toohello Azure Marketplace. Varje web service innehåller ett övergripande dokument anslutits bädda in provdatauppsättningar för att testa hello tjänster och förklarar hur hello användare kan skapa en liknande tjänst sig själva. 

I Azure Machine Learning Studio användarna skriva R-koden och med några få klick kan du publicera den som en webbtjänst för program och enheter tooconsume runt hello world. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Leder enkla räknare som tillhandahåller en anpassad text utvinningsmodellen sentiment analys ge prognoser för statistiska funktioner toocreating både nya och erfarna R-användare kan dra nytta av hello enkel som kan användare av Azure Machine Learning operationalisera R kod. När dessa webbtjänster kan användas av användare potentiellt via en mobil app eller en webbplats hello syftet med dessa webbtjänster som exempel är tooshow hur Maskininlärning kan operationalisera R skript för analys och innehålla används toocreate webbtjänster överst i R-koden.

Varje exempel innehåller ett C#-exempel för användning av web service.

![Diagram över R-koden i Azure Machine Learning: R lösningar för egna användning eller publicerade toohello Azure Marketplace.][1]

Överväg att hello följande scenarier.

## <a name="scenario-1-generic-model"></a>Scenario 1: Allmän modell
En användare arbetar med en allmän modell som kan vara tillämpade tooa den nya användarens data, till exempel en grundläggande prognoser tid seriedata eller ett specialbyggt R-metod med avancerade analyser. Den här användaren publicerar hello modellen som en webbtjänst för andra tooconsume med sina data.

* [Binär klassificerare](machine-learning-r-csharp-binary-classifier.md)
* [Klustermodell](machine-learning-r-csharp-cluster-model.md)
* [Multivarierad linjär regression](machine-learning-r-csharp-multivariate-linear-regression.md)
* [Prognostisering – Exponentiell utjämning](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [ETS + STL prognoser](machine-learning-r-csharp-retail-demand-forecasting.md)
* [Prognosticering - Autoregressive integrerad glidande medelvärde (ARIMA)](machine-learning-r-csharp-arima.md)
* [Analys av fortsatt giltighet](machine-learning-r-csharp-survival-analysis.md)

## <a name="scenario-2-trained-model--specific-data"></a>Scenario 2: Tränad modell – specifika data
En användare har data som innehåller användbar förutsägelser via R-koden som ett stort urval av person enkäter klustrade via en k-means algoritmen toopredict hello användares personlighetstyp eller hälsa undersökning data som kan använda toopredict en person risk för lungcancer med ett överlevnad analys R-paket. hello användaren publicerar hello data via en webbtjänst som beräknar resultatet av en ny användare.

## <a name="scenario-3-trained-model--generic-data"></a>Scenario 3: Tränad modell – allmänna data
En användare har allmänna data (till exempel en text Kristi) som gör att en web service toobe inbyggda och allmänna används över flera olika typer av användningsområden och scenarier.

* [Lexikonbaserad attitydanalys](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

## <a name="scenario-4-advanced-calculator"></a>Scenario 4: Avancerade Kalkylatorn
En användare ger avancerade beräkningar eller simulering som inte kräver någon tränad modell eller montering av en modell toohello användarens data.

* [Skillnad i proportionstest](machine-learning-r-csharp-difference-in-two-proportions.md)
* [Normalt fördelningspaket](machine-learning-r-csharp-normal-distribution.md)
* [Binomialt fördelningspaket](machine-learning-r-csharp-binomial-distribution.md)

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
Vanliga frågor om förbrukningen av hello webbtjänst eller publishing toohello Marketplace finns [här](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png



