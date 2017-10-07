---
title: "aaaMulti Geo hjälpdokumentationen | Microsoft Docs"
description: "Lär dig hur toocreate en arbetsyta och publicera en webbtjänst i en Azure-region skiljer sig från hello södra centrala USA (SCUS) Azure-region."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: 77b055950ebfe329131b40e5f0a2f6be1e33c51e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multi-geo-help-documentation"></a>Hjälpdokumentation för flera geografiska platser
Den här artikeln beskriver hur du kan skapa en arbetsyta och publicera en webbtjänst i olika Azure-regioner.  Hej [Azure produkter efter Region sidan](https://azure.microsoft.com/en-us/regions/services/) visar en lista över regioner där Azure Machine Learning är tillgängligt.

## <a name="create-a-workspace"></a>Skapa en arbetsyta
1. Logga in toohello klassiska Azure-portalen.
2. Klicka på **+ ny** > **DATATJÄNSTER** > **MASKININLÄRNING** > **SNABBREGISTRERING**.  Under **plats** välja en annan region, till exempel **Sydostasien**.
   ![Flera Geo hjälpa bild 1][1]
3. Välj hello arbetsyta och klicka sedan på **inloggning tooML Studio**.
   ![Flera Geo hjälpa bild 2][2]
4. Nu har du en arbetsyta i en annan region som du kan använda precis som andra arbetsytan. tooswitch bland dina arbetsytor utseende toohello övre högra hörnet på skärmen. Klicka på hello listrutan, Välj hello region och välj sedan hello arbetsytan. Allt är lokala toohello arbetsytan region.  Exempelvis blir alla webbtjänster som skapas från en arbetsyta i hello finns i samma region hello arbetsyta.
   ![Flera Geo hjälpa bild 3][3]

## <a name="open-an-experiment-from-gallery"></a>Öppna ett experiment från galleriet
Om du öppnar ett experiment från galleriet måste välja du även vilken region som du vill använda toocopy hello experimentet.

![Flera Geo hjälpa bild 4][4a]

## <a name="web-service-management"></a>Webbtjänsthantering
tooprogrammatically hantera webbtjänster, t.ex för omtränings, Använd hello regionspecifika adress:

* https://asiasoutheast.Management.azureml.NET
* https://europewest.Management.azureml.NET

### <a name="things-toonote"></a>Saker toonote
1. Du kan bara kopiera experiment mellan arbetsytor som tillhör toohello samma region det här sättet. Om du behöver toocopy experiment över arbetsytor i olika regioner, kan du använda hello [PowerShell](http://aka.ms/amlps) kommandot [ *kopiera AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish som. En annan lösning är toopublish hello experiment i galleriet i olistade läge och sedan öppna den i hello arbetsyta från hello annan region.
2. hello region selector visar endast arbetsytor för en region i taget.  
3. En kostnadsfri arbetsyta eller gästbehörighet (anonym) arbetsytan ska skapas och hanteras i södra centrala USA  
4. Webbtjänster som distribueras från en arbetsyta i Sydostasien ska också finnas i Sydostasien.  

## <a name="more-information"></a>Mer information
Ställ en fråga på hello [Azure Machine Learning-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
