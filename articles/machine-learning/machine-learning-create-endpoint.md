---
title: "aaaCreating slutpunkter för webbtjänster i Machine Learning | Microsoft Docs"
description: "Skapa slutpunkter för webbtjänster i Azure Machine Learning"
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 10a2bc586c6fe35e28d8bf0293854c578827c453
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-endpoints"></a>Skapa slutpunkter
> [!NOTE]
>  Det här avsnittet beskrivs tekniker tillämpliga tooa **klassiska** Machine Learning-webbtjänst.
> 
> 

När du skapar webbtjänster som du säljer vidarebefordra tooyour kunder måste tooprovide tränade modeller tooeach kunden är fortfarande länkade toohello experiment från vilka hello Web service skapades. Dessutom tillämpas uppdateringar toohello experiment ska selektivt tooan endpoint utan att skriva över hello anpassningar.

tooaccomplish, Azure Machine Learning kan du toocreate flera slutpunkter för en distribuerad webbtjänst. Varje slutpunkt i hello webbtjänsten är oberoende av varandra åtgärdas, begränsas och hanteras. Varje slutpunkt är en unik URL och auktorisering nyckel som du distribuerar tooyour kunder.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-tooa-web-service"></a>Att lägga till slutpunkter tooa Web service
Det finns tre sätt tooadd en slutpunkt tooa webbtjänsten.

* Programmässigt
* Hello Azure Machine Learning-webbtjänster-portalen
* Även om hello klassiska Azure-portalen

När hello slutpunkten har skapats kan du använda via synkron API: er, batch-API: er, och excel-kalkylblad. Dessutom tooadding slutpunkter via Användargränssnittet, du kan också använda hello Endpoint Management API: er tooprogrammatically lägga till slutpunkter.

> [!NOTE]
> Om du har lagt till ytterligare slutpunkter toohello webbtjänsten, kan du ta bort hello standardslutpunkten.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Lägga till en slutpunkt programmässigt
Du kan lägga till en slutpunkt tooyour webbtjänst genom programmering med hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) exempelkod.

## <a name="adding-an-endpoint-using-hello-azure-machine-learning-web-services-portal"></a>Lägga till en slutpunkt med hjälp av hello Azure Machine Learning-webbtjänster portal
1. Klicka på webbtjänster på hello navigeringen till vänster kolumn i Machine Learning Studio.
2. Hello längst ned på hello Web service instrumentpanelen, klickar du på **hantera slutpunkter**. hello Azure Machine Learning-webbtjänster portal öppnar toohello slutpunkter sida för hello webbtjänsten.
3. Klicka på **Ny**.
4. Skriv ett namn och beskrivning för hello ny slutpunkt. Slutpunkten namn måste vara 24 tecken eller mindre längd och måste bestå av gemena bokstäver eller siffror. Välj hello loggningsnivån och om exempeldata är aktiverad. Mer information om loggning finns [aktivera loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-hello-azure-classic-portal"></a>Lägga till en slutpunkt som använder hello klassiska Azure-portalen
1. Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com), klickar du på **Maskininlärning** i hello vänstra kolumnen. Klicka på hello-arbetsyta som innehåller hello-webbtjänst som du är intresserad av.
   
    ![Navigera tooworkspace](./media/machine-learning-create-endpoint/figure-1.png)
2. Klicka på **webbtjänster**.
   
    ![Navigera tooWeb tjänster](./media/machine-learning-create-endpoint/figure-2.png)
3. Klicka på hello-webbtjänst som du är intresserad av toosee hello listan över tillgängliga slutpunkter.
   
    ![Navigera tooendpoint](./media/machine-learning-create-endpoint/figure-3.png)
4. Hello längst hello-sidan, klickar du på **Lägg till slutpunkt**. Ange ett namn och beskrivning, se till att det finns inga slutpunkter med samma namn i den här webbtjänsten hello. Lämna hello begränsning nivå med sitt standardvärde om du inte har särskilda krav. toolearn mer information om hur du begränsar, se [skalning API-slutpunkter](machine-learning-scaling-webservice.md).
   
    ![Skapa slutpunkten](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Nästa steg
[Hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

