---
title: "aaaExcel-tillägg för webbtjänster för Machine Learning | Microsoft Docs"
description: "Hur toouse Azure Machine Learning Web services direkt i Excel utan att skriva någon kod."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/14/2017
ms.author: tedway;garye
ms.openlocfilehash: c52f40d33c9907f284e4750afe47181dc3365fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel-tillägg för Azure Machine Learning-webbtjänster
Excel gör det enkelt toocall webbtjänster direkt utan hello måste toowrite någon kod.

## <a name="steps-toouse-an-existing-web-service-in-hello-workbook"></a>Steg tooUse en befintlig webbtjänst i hello arbetsboken

1. Öppna hello [Excel exempelfilen](http://aka.ms/amlexcel-sample-2), som innehåller hello Excel-tillägget och data om passagerare på hello Titanic.
2. Välj hello-webbtjänsten genom att klicka på den-”Titanic efterlevande ge prognoser (Excel-tillägg prov) [poäng]” i det här exemplet.
   
    ![Välj-webbtjänst][01]
3. Detta tar toohello **Predict** avsnitt.  Den här arbetsboken innehåller redan exempeldata, men en tom arbetsbok du kan markera en cell i Excel och klickar på **använda exempeldata**.
4. Välj hello data med rubriker och klickar på ikonen för hello indata intervall.  Kontrollera hello ”Mina data har rubriker” kryssrutan är markerad.
5. Under **utdata**, ange antalet celler för hello där du vill hello utdata toobe, till exempel ”H1” här.
6. Klicka på **förutsäga**.
   
    ![Förutsäga avsnitt][02]

Distribuera en webbtjänst eller Använd en befintlig webbtjänst. Mer information om hur du distribuerar en webbtjänst finns [genomgången steg 5: distribuera hello Azure Machine Learning-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md).

Hämta hello API-nyckel för webbtjänsten. Om du utför beror den här åtgärden på om du har publicerat en klassiska Machine Learning-webbtjänst för en ny Machine Learning-webbtjänst.

**Använda en klassisk-webbtjänst** 

1. Klicka på hello i Machine Learning Studio **WEB SERVICES** avsnittet hello vänster och väljer sedan hello-webbtjänsten.
   
    ![Studio väljer du en webbtjänst][04]
2. Kopiera hello API-nyckel för hello-webbtjänsten.
   
    ![Studio API-nyckel][05]
3. På hello **INSTRUMENTPANELEN** för hello webbtjänsten klickar du på hello **frågor och svar** länk.
4. Leta efter hello **Begärd URI** avsnitt.  Kopiera och spara hello-URL.

> [!NOTE]
> Det är nu möjligt toosign till hello [Azure Machine Learning-webbtjänster](https://services.azureml.net) portal tooobtain hello API-nyckel för en klassiska Machine Learning-webbtjänst.
> 
> 

**Använd en ny webbtjänst**

1. I hello [Azure Machine Learning-webbtjänster](https://services.azureml.net) klickar du på **Web Services**, Välj din webbtjänst. 
2. Klicka på **använda**.
3. Leta efter hello **grundläggande förbrukning info** avsnitt. Kopiera och spara hello **primärnyckel** och hello **begäran och svar** URL.

## <a name="steps-tooadd-a-new-web-service"></a>Steg tooAdd en ny webbtjänst

1. Distribuera en webbtjänst eller Använd en befintlig webbtjänst. Mer information om hur du distribuerar en webbtjänst finns [genomgången steg 5: distribuera hello Azure Machine Learning-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md).
2. Klicka på **använda**.
3. Leta efter hello **grundläggande förbrukning info** avsnitt. Kopiera och spara hello **primärnyckel** och hello **begäran och svar** URL.
4. I Excel, går toohello **Web Services** avsnittet (om du är i hello **Predict** klickar du på Bakåt pilen hello toogo toohello lista över webbtjänster).
   
    ![Gå tooWeb service markering][03]
5. Klicka på **Lägg till webbtjänst**.
6. Klistra in hello URL i hello Excel-tillägget textruta med etiketten **URL**.
7. Klistra in hello API eller den primära nyckeln i hello textrutan **API-nyckeln**.
8. Klicka på **Lägg till**.
   
    ![URL: en och API-nyckeln för det klassiska.][06]
9. webbtjänst för toouse hello, följer du föregående anvisningar hello, ”steg tooUse en befintlig web Service”.

## <a name="sharing-your-workbook"></a>Dela din arbetsbok
Om du sparar arbetsboken sparas också hello API eller den primära nyckeln för hello webbtjänster som du har lagt till. Det innebär att du ska dela hello arbetsboken med personer som du litar på.

Frågor i hello följande kommentar avsnittet eller på vår [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
