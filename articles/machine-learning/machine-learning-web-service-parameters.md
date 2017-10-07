---
title: "aaaUse Webbtjänstparametrar som Azure Machine Learning | Microsoft Docs"
description: "Hur hello toouse Azure Machine Learning Webbtjänstparametrar toomodify beteendet för din modell när hello webbtjänsten används."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: raymondl;garye
ms.openlocfilehash: 214711eb819a6cea34db905abdf015da11e846d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a>Använda parametrar för Azure Machine Learning-webbtjänst
En Azure Machine Learning-webbtjänst skapas genom att publicera ett experiment som innehåller moduler med parametrar. I vissa fall kan kanske du vill toochange hello modulen beteende när hello-webbtjänsten körs. *Webbtjänstparametrar* Tillåt toodo den här uppgiften. 

Ett vanligt exempel att konfigurera hello [importera Data] [ reader] modulen så att användaren hello hello publicerade webbtjänsten kan ange en annan datakälla när hello webbtjänsten används. Eller konfigurera hello [exportera Data] [ writer] modulen så att du kan ange ett annat mål. Andra exempel är att ändra hello antalet bitar för hello [funktions-hashning] [ feature-hashing] modul eller hello antalet önskade funktioner för hello [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modul. 

Du kan ange Webbtjänstparametrar och koppla dem till en eller flera parametrar i experimentet och du kan ange om de är obligatoriska eller valfria. hello användaren av webbtjänsten för hello kan sedan ange värden för dessa parametrar när de anropa hello-webbtjänsten. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-tooset-and-use-web-service-parameters"></a>Hur tooset och använda Webbtjänstparametrar
Du kan definiera en Web Service-Parameter klickar hello ikonen nästa toohello parameter för en modul och väljer ”Ange som web service parameter”. Detta skapar en ny Web Service Parameter och kopplar den toothat modulen parametern. Sedan när hello webbtjänsten används hello användaren kan ange ett värde för hello Web Service-parametern och det är tillämpade toohello modulen parameter.

När du definierar en Web Service-Parameter, är det tillgängliga tooany andra modul-parametern i hello experiment. Om du definierar en Web Service-Parameter som är associerade med en parameter för en modul, kan du använda samma Web Service parametern för en annan modul som hello parametern förväntar hello samma typ av värde. Till exempel om hello Web Service-parametern är ett numeriskt värde, kan sedan den bara användas för parametrar som förväntar sig ett numeriskt värde. Hello användare anger ett värde för hello Web Service-Parameter, blir tillämpade tooall associerade parametrar.

Du kan bestämma om tooprovide standard värde för hello Web Service-parametern. Om du gör sedan hello är parametern valfri för hello användare av hello web service. Om du inte anger ett standardvärde kan sedan hello användaren är obligatoriska tooenter ett värde när hello webbtjänsten används.

hello API-dokumentationen för hello-webbtjänsten innehåller information för hello web Serviceanvändare på hur toospecify hello Web Service parametern programmässigt vid åtkomst till hello-webbtjänsten.

> [!NOTE]
> Hej API-dokumentationen för en klassiska webbtjänst tillhandahålls via hello **API hjälpsidan** länken i hello webbtjänsten **INSTRUMENTPANELEN** i Machine Learning Studio. Hej API-dokumentationen för en ny webbtjänst tillhandahålls via hello [Azure Machine Learning-webbtjänster](https://services.azureml.net/Quickstart) Företagsportalen på hello **förbruka** och **Swagger API** sidor för din webbtjänsten.
> 
> 

## <a name="example"></a>Exempel
Ett exempel antar vi att vi har ett experiment med en [exportera Data] [ writer] modul som skickar information tooAzure blob-lagring. Ska vi definiera Web Service Parameter med namnet ”Blob sökvägen” som gör att hello web service användaren toochange hello sökvägen toohello blob-lagring när hello tjänsten används.

1. Klicka på hello i Machine Learning Studio [exportera Data] [ writer] modulen tooselect den. Egenskaperna som visas i hello egenskaper fönstret toohello höger i hello experimentet.
2. Ange hello lagringstyp:
   
   * Under **ange datamål**, Välj ”Azure Blob Storage”.
   * Under **ange autentiseringstypen**, Välj ”konto”.
   * Ange hello kontoinformationen för hello Azure blob storage. 
     <p />
3.Klicka på hello ikonen toohello höger i hello **sökväg tooblob som inleds med behållaren parametern**. Det ser ut så här:
   
   ![Web Service-parametern ikonen][icon]
   
   Välj ”ange som web service parameter”.
   
   En post läggs till under **Webbtjänstparametrar** längst hello hello egenskapsrutan med hello namn ”sökväg tooblob som börjar med behållaren”. Detta är hello Web Service-Parameter som är nu som är associerade med den här [exportera Data] [ writer] modul-parametern.
4. toorename hello Web Service-parametern, hello klickar du på, ange ”Blob sökvägen” och tryck på hello **RETUR** nyckel. 
5. tooprovide ett standardvärde för hello Web Service-Parameter, klicka på hello ikonen toohello höger i hello namn, markera ”ge standardvärdet”, ange ett värde (till exempel ”container1/output1.csv”) och tryck på hello **RETUR** nyckel.
   
   ![Web Service-Parameter][parameter]
6. Klicka på **Run** (Kör). 
7. Klicka på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]** toodeploy hello-webbtjänsten.

> [!NOTE] 
> toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten. Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md). 

hello användaren av webbtjänsten hello kan nu ange ett nytt mål för hello [exportera Data] [ writer] modulen vid åtkomst till hello-webbtjänsten.

## <a name="more-information"></a>Mer information
En mer detaljerad exempel finns hello [Webbtjänstparametrar](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) post i hello [Machine Learning blogg](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Mer information om hur du använder en Machine Learning-webbtjänst finns [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).

<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

