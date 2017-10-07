---
title: "aaaTroubleshoot omtränings en Azure Machine Learning klassiska webbtjänsten | Microsoft Docs"
description: "Identifiera och lösa vanliga problem uppstod när du omtränings hello modellen för en Azure Machine Learning-webbtjänst."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 2b6a78eaba161877106dccdc23437b5e454fca7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-retraining-of-an-azure-machine-learning-classic-web-service"></a>Felsöka hello omtränings av en Azure Machine Learning klassiska-webbtjänst
## <a name="retraining-overview"></a>Omtränings översikt
När du distribuerar en prediktivt experiment som en bedömningsprofil webbtjänst är en statisk modell. När nya data blir tillgängliga eller när hello konsumenter av hello API har sina egna data, måste hello modellen toobe retrained. 

En fullständig genomgång av hello omtränings av en klassiska webbtjänst finns [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Omtränings process
När du behöver tooretrain hello webbtjänsten måste du lägga till vissa ytterligare delar:

* En webbtjänst som distribueras från hello Träningsexperiment. hello experiment måste ha en **Web Service utdata** modulen ansluten toohello utdata från hello **Träningsmodell** modul.  
  
    ![Koppla hello web service utdata toohello train-modell.][image1]
* En ny slutpunkt läggs tooyour poängsättning av webbtjänsten.  Du kan lägga till hello endpoint programmässigt med hello exempelkod som refereras i hello träna om Machine Learning-modeller via programmering avsnittet eller via hello klassiska Azure-portalen.

Du kan sedan använda hello C# exempelkod från hello utbildning Web tjänstens API hjälp sidan tooretrain modell. När du har utvärderat hello resultat och är nöjd med dem kan uppdatera du hello tränade modellen bedömningen webbtjänsten med hello ny slutpunkt som du har lagt till.

Med alla hello delar på plats är hello viktiga steg du måste vidta tooretrain hello modellen följande:

1. Anropa hello utbildning webbtjänst: hello anropet är toohello Batch Execution Service BES-, inte hello Begär svar Service (RR). Du kan använda hello C# exempelkoden i hello API hjälp sidan toomake hello anropet. 
2. Hitta hello värden för hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken*: dessa värden returneras i hello utdata från din anropet toohello utbildning Tjänsten. 
   ![Visar hello utdata från hello omtränings exemplet och hello BaseLocation, RelativeLocation och SasBlobToken värden.][image6]
3. Uppdateringen hello läggs endpoint från hello bedömningen webbtjänst med hello nya tränats modellen: med hello exempelkod anges i hello träna om Machine Learning modeller via programmering, uppdatera hello ny slutpunkt du har lagt till toohello bedömningen modellen med hello nyligen tränade modellen från hello utbildning webbtjänsten.

## <a name="common-obstacles"></a>Vanliga hinder
### <a name="check-toosee-if-you-have-hello-correct-patch-url"></a>Kontrollera toosee om du har hello korrigera korrigering URL
hello korrigering URL som du använder måste vara hello något som är associerade med hello nya bedömningsslutpunkten du har lagt till toohello poängsättning av webbtjänsten. Det finns ett antal sätt tooobtain hello korrigering URL:

**Alternativ 1: genom att programmera**

tooget hello korrigera korrigering URL:

1. Kör hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) exempelkod.
2. Hitta hello från hello-utdata för AddEndpoint *HelpLocation* värde och kopiera hello-URL.
   
   ![HelpLocation i hello utdata hello addEndpoint exemplet.][image2]
3. Klistra in hello URL i en webbläsare toonavigate tooa sida som innehåller hjälplänkar för hello webbtjänsten.
4. Klicka på hello **uppdatering resurs** hjälpsidan för länken tooopen hello korrigering.

**Alternativ 2: Använd hello klassiska Azure-portalen**

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Öppna hello Machine Learning-fliken. ![Datorn lutande fliken.][image4]
3. Klicka på arbetsytans namn, sedan **Web Services**.
4. Klicka på hello bedömningen webbtjänst som du arbetar med. (Om du inte ändrar hello standardnamnet hello-webbtjänsten, det går ut om [bedömningen Exp.].)
5. Klicka på **lägga till slutpunkten**.
6. När hello slutpunkt har lagts till, klickar du på namnet på slutpunkten hello. Klicka på **uppdatering resurs** tooopen hello hjälpsidan för uppdatering.

> [!NOTE]
> Om du har lagt till hello endpoint toohello utbildning webbtjänsten i stället för hello förutsägande webbtjänsten, du får följande fel när du klickar på hello hello **uppdatering resurs** länk: tyvärr, men den här funktionen stöds inte eller tillgängligt i den här kontexten. Den här webbtjänsten har inga resurser för uppdateras. Vi ber om ursäkt för besväret hello och arbetar med att förbättra det här arbetsflödet.
> 
> 

![Ny slutpunkt instrumentpanel.][image3]

hello korrigering hjälpsidan innehåller hello korrigering URL måste du använda och exempelkod som du kan använda toocall den.

![Patch-URL.][image5]

### <a name="check-toosee-that-you-are-updating-hello-correct-scoring-endpoint"></a>Kontrollera att du uppdaterar hello korrekt bedömningsprofil slutpunkt toosee
* Inte korrigering hello utbildning webbtjänst: hello korrigering åtgärden måste utföras på hello poängsättning av webbtjänsten.
* Inte korrigering hello standardslutpunkten webbtjänsten: hello korrigering åtgärden måste utföras på hello nya bedömningen webbtjänstslutpunkt som du har lagt till.

Du kan kontrollera vilka hello webbtjänstslutpunkt är som besökande hello klassiska Azure-portalen. 

> [!NOTE]
> Var noga med att du lägger till hello endpoint toohello förutsägande webbtjänst, inte hello utbildning webbtjänsten. Om du har distribuerat en utbildnings- och en förutsägbar webbtjänst korrekt, bör du se två separata webbtjänster som anges. hello förutsägande webbtjänsten ska avslutas med ”[förutsägande exp.]”.
> 
> 

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Öppna hello Machine Learning-fliken. ![Machine learning-arbetsytan Användargränssnittet.][image4]
3. Välj din arbetsyta.
4. Klicka på **webbtjänster**.
5. Välj förutsägbara webbtjänsten.
6. Kontrollera att din nya slutpunkt har lagts till toohello webbtjänsten.

### <a name="check-hello-workspace-that-your-web-service-is-in-tooensure-it-is-in-hello-correct-region"></a>Kontrollera hello-arbetsyta som webbtjänsten har tooensure i hello rätt region
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj Machine Learning hello-menyn.
   ![Machine learning region Användargränssnittet.][image4]
3. Kontrollera hello platsen för din arbetsyta.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png
