---
title: "Steg 6: Komma åt hello på Machine Learning-webbtjänst | Microsoft Docs"
description: "Steg 6 i hello utveckla en förutsägelselösning genomgång: komma åt en aktiva Azure Machine Learning-webbtjänst."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 211de0294092c6a6b5e6eb608d5d3b88107674c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-6-access-hello-azure-machine-learning-web-service"></a>Genomgång steg 6: Komma åt hello Azure Machine Learning-webbtjänst

Detta är hello sista steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Skapa en Machine Learning-arbetsyta](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Överför befintliga data](machine-learning-walkthrough-2-upload-data.md)
3. [Skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Träna och utvärdera hello modeller](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Distribuera hello-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)
6. **Få åtkomst till hello-webbtjänsten**

- - -
I hello föregående steg i den här genomgången distribuerade vi en webbtjänst som använder vår kredit risk förutsägelse modell. Nu användare kan toosend data tooit och få resultat. 

hello Web service är en Azure-webbtjänst som kan ta emot och returnera data med hjälp av REST API: er i ett av två sätt:  

* **Frågor och svar** – hello användare skickar en eller fler rader med kredit data toohello tjänsten med hjälp av en HTTP-protokollet och hello tjänsten svarar med en eller flera uppsättningar av resultaten.
* **Batch-körningen** – hello användaren lagrar en eller fler rader med kredit data i ett Azure blob- och skickar sedan hello blob plats toohello-tjänsten. hello service poäng alla hello rader med data i Hej inkommande blob, lagrar hello resulterar i en annan blob och returnerar hello URL för behållaren.  

Hej snabbaste och enklaste sättet tooaccess en klassisk webbtjänst är via hello [Azure ML-svar på begäranden Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) eller [Azure ML Batch Execution Service Web Appmallen](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Mallarna web app kan skapa ett anpassat webbprogram som känner din webbtjänsten indata och vad returneras. Allt du behöver toodo är att ge åtkomst tooyour webbtjänsten och data och hello mallen hello rest.

Mer information om hur du använder hello app mallar finns [använda en Azure Machine Learning webbtjänst med en mall för app](machine-learning-consume-web-service-with-web-app-template.md).

Du kan också skapa ett anpassat program tooaccess hello webbtjänsten med starter koden du i R, C# och Python programmeringsspråk.

Du hittar information i [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

