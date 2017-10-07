---
title: "aaaRetrain en ny Azure Machine Learning-webbtjänst med PowerShell | Microsoft Docs"
description: "Lär dig hur tooprogrammatically träna om en modell och uppdatera hello web service toouse hello nyligen tränad modell i Azure Machine Learning med Machine Learning Management PowerShell-cmdlets för hello."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 5b77fa82cfe17f0b4e90007ef81c506ab712475b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-hello-machine-learning-management-powershell-cmdlets"></a>Träna om en ny Resource Manager-baserat webbtjänst med hello Machine Learning Management PowerShell-cmdlets
När du träna om en ny webbtjänst uppdatera hello förutsägande web service definition tooreference hello ny tränad modell.  

## <a name="prerequisites"></a>Krav
Du måste skapa ett experiment utbildning och prediktivt experiment som visas i [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md). 

> [!IMPORTANT]
> Hej prediktivt experiment måste distribueras som en Azure Resource Manager (nya) baserad machine learning-webbtjänst. toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten. Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md). 

Ytterligare information om distribuera webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).

Den här processen kräver att du har installerat hello Azure Machine Learning-Cmdlets. Information som installerar hello Machine Learning-cmdlets finns hello [Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference på MSDN.

Kopierade hello följande information från hello omtränings utdata:

* BaseLocation
* RelativeLocation

hello du vidta åtgärder är:

1. Logga in tooyour Azure Resource Manager-konto.
2. Hämta hello web service definition
3. Exportera hello Web Service Definition som JSON
4. Uppdatera hello toohello ilearner referensblobben i hello JSON.
5. Importera hello JSON till en Web Service Definition
6. Uppdatera hello-webbtjänsten med nya Web Service Definition

## <a name="sign-in-tooyour-azure-resource-manager-account"></a>Logga in tooyour Azure Resource Manager-konto
Du måste först loggar in tooyour Azure-konto från inom hello PowerShell-miljö med hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.

## <a name="get-hello-web-service-definition"></a>Hämta hello Web Service Definition
Därefter hämta hello webbtjänsten genom att anropa hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet. hello Web Service Definition är en intern representation av hello tränad modell för hello webbtjänsten och kan inte ändras direkt. Kontrollera att du hämtar hello Web Service Definition för experimentet förutsägbara och inte experimentet utbildning.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

toodetermine hello resursgruppens namn för en befintlig webbtjänst kör hello Get-AzureRmMlWebService cmdlet utan några parametrar toodisplay hello web-tjänster i din prenumeration. Leta upp hello webbtjänsten och titta på dess web service-ID. hello hello resursgruppen heter hello fjärde elementet i hello ID precis efter hello *resursgrupper* element. I följande exempel hello, används hello resursgruppens namn standard-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Du kan också toodetermine hello resursgruppens namn för en befintlig webbtjänst inloggning toohello portal för Microsoft Azure Machine Learning-webbtjänster. Välj hello-webbtjänst. hello resursgruppens namn används hello femte element av hello URL för webbtjänsten för hello, bara efter hello *resursgrupper* element. I följande exempel hello, används hello resursgruppens namn standard-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-as-json"></a>Exportera hello Web Service Definition som JSON
toomodify hello definition toohello tränas modellen toouse Hej nyligen Trained Model, måste du först använda hello [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport den tooa JSON-fil.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob-in-hello-json"></a>Uppdatera hello toohello ilearner referensblobben i hello JSON.
Leta upp hello [tränad modell], uppdatering hello i hello tillgångar *uri* värdet i hello *locationInfo* nod med hello hello ilearner blob-URI. hello URI genereras genom att kombinera hello *BaseLocation* och hello *RelativeLocation* från hello utdata från hello BES omtränings-anrop. Detta uppdaterar hello sökvägen tooreference hello ny tränad modell.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-hello-json-into-a-web-service-definition"></a>Importera hello JSON till en Web Service Definition
Du måste använda hello [importera AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello ändrade JSON-fil i en Web Service Definition som du kan använda tooupdate hello Web Service Definition.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service-with-new-web-service-definition"></a>Uppdatera hello-webbtjänsten med nya Web Service Definition
Slutligen kan du använda [uppdatering AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello Web Service Definition.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Sammanfattning
Du kan uppdatera hello tränad modell för en förutsägbar webbtjänst att aktivera scenarier som använder hello Machine Learning PowerShell cmdlet: ar:

* Periodiska modell via programmering med nya data.
* Distribution av en modell toocustomers med hello målet för att låta dem träna om hello modellen med sina egna data.

