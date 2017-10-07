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
# <a name="retrain-a-new-resource-manager-based-web-service-using-hello-machine-learning-management-powershell-cmdlets"></a><span data-ttu-id="49e99-103">Träna om en ny Resource Manager-baserat webbtjänst med hello Machine Learning Management PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="49e99-103">Retrain a New Resource Manager based web service using hello Machine Learning Management PowerShell cmdlets</span></span>
<span data-ttu-id="49e99-104">När du träna om en ny webbtjänst uppdatera hello förutsägande web service definition tooreference hello ny tränad modell.</span><span class="sxs-lookup"><span data-stu-id="49e99-104">When you retrain a New web service, you update hello predictive web service definition tooreference hello new trained model.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="49e99-105">Krav</span><span class="sxs-lookup"><span data-stu-id="49e99-105">Prerequisites</span></span>
<span data-ttu-id="49e99-106">Du måste skapa ett experiment utbildning och prediktivt experiment som visas i [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="49e99-106">You must set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="49e99-107">Hej prediktivt experiment måste distribueras som en Azure Resource Manager (nya) baserad machine learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="49e99-107">hello predictive experiment must be deployed as an Azure Resource Manager (New) based machine learning web service.</span></span> <span data-ttu-id="49e99-108">toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="49e99-108">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="49e99-109">Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="49e99-109">For more information, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="49e99-110">Ytterligare information om distribuera webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="49e99-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="49e99-111">Den här processen kräver att du har installerat hello Azure Machine Learning-Cmdlets.</span><span class="sxs-lookup"><span data-stu-id="49e99-111">This process requires that you have installed hello Azure Machine Learning Cmdlets.</span></span> <span data-ttu-id="49e99-112">Information som installerar hello Machine Learning-cmdlets finns hello [Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference på MSDN.</span><span class="sxs-lookup"><span data-stu-id="49e99-112">For information installing hello Machine Learning cmdlets, see hello [Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN.</span></span>

<span data-ttu-id="49e99-113">Kopierade hello följande information från hello omtränings utdata:</span><span class="sxs-lookup"><span data-stu-id="49e99-113">Copied hello following information from hello retraining output:</span></span>

* <span data-ttu-id="49e99-114">BaseLocation</span><span class="sxs-lookup"><span data-stu-id="49e99-114">BaseLocation</span></span>
* <span data-ttu-id="49e99-115">RelativeLocation</span><span class="sxs-lookup"><span data-stu-id="49e99-115">RelativeLocation</span></span>

<span data-ttu-id="49e99-116">hello du vidta åtgärder är:</span><span class="sxs-lookup"><span data-stu-id="49e99-116">hello steps you take are:</span></span>

1. <span data-ttu-id="49e99-117">Logga in tooyour Azure Resource Manager-konto.</span><span class="sxs-lookup"><span data-stu-id="49e99-117">Sign in tooyour Azure Resource Manager account.</span></span>
2. <span data-ttu-id="49e99-118">Hämta hello web service definition</span><span class="sxs-lookup"><span data-stu-id="49e99-118">Get hello web service definition</span></span>
3. <span data-ttu-id="49e99-119">Exportera hello Web Service Definition som JSON</span><span class="sxs-lookup"><span data-stu-id="49e99-119">Export hello Web Service Definition as JSON</span></span>
4. <span data-ttu-id="49e99-120">Uppdatera hello toohello ilearner referensblobben i hello JSON.</span><span class="sxs-lookup"><span data-stu-id="49e99-120">Update hello reference toohello ilearner blob in hello JSON.</span></span>
5. <span data-ttu-id="49e99-121">Importera hello JSON till en Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="49e99-121">Import hello JSON into a Web Service Definition</span></span>
6. <span data-ttu-id="49e99-122">Uppdatera hello-webbtjänsten med nya Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="49e99-122">Update hello web service with new Web Service Definition</span></span>

## <a name="sign-in-tooyour-azure-resource-manager-account"></a><span data-ttu-id="49e99-123">Logga in tooyour Azure Resource Manager-konto</span><span class="sxs-lookup"><span data-stu-id="49e99-123">Sign in tooyour Azure Resource Manager account</span></span>
<span data-ttu-id="49e99-124">Du måste först loggar in tooyour Azure-konto från inom hello PowerShell-miljö med hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="49e99-124">You must first sign in tooyour Azure account from within hello PowerShell environment using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-hello-web-service-definition"></a><span data-ttu-id="49e99-125">Hämta hello Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="49e99-125">Get hello Web Service Definition</span></span>
<span data-ttu-id="49e99-126">Därefter hämta hello webbtjänsten genom att anropa hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="49e99-126">Next, get hello Web Service by calling hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="49e99-127">hello Web Service Definition är en intern representation av hello tränad modell för hello webbtjänsten och kan inte ändras direkt.</span><span class="sxs-lookup"><span data-stu-id="49e99-127">hello Web Service Definition is an internal representation of hello trained model of hello web service and is not directly modifiable.</span></span> <span data-ttu-id="49e99-128">Kontrollera att du hämtar hello Web Service Definition för experimentet förutsägbara och inte experimentet utbildning.</span><span class="sxs-lookup"><span data-stu-id="49e99-128">Make sure that you are retrieving hello Web Service Definition for your Predictive experiment and not your training experiment.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="49e99-129">toodetermine hello resursgruppens namn för en befintlig webbtjänst kör hello Get-AzureRmMlWebService cmdlet utan några parametrar toodisplay hello web-tjänster i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="49e99-129">toodetermine hello resource group name of an existing web service, run hello Get-AzureRmMlWebService cmdlet without any parameters toodisplay hello web services in your subscription.</span></span> <span data-ttu-id="49e99-130">Leta upp hello webbtjänsten och titta på dess web service-ID.</span><span class="sxs-lookup"><span data-stu-id="49e99-130">Locate hello web service, and then look at its web service ID.</span></span> <span data-ttu-id="49e99-131">hello hello resursgruppen heter hello fjärde elementet i hello ID precis efter hello *resursgrupper* element.</span><span class="sxs-lookup"><span data-stu-id="49e99-131">hello name of hello resource group is hello fourth element in hello ID, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="49e99-132">I följande exempel hello, används hello resursgruppens namn standard-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="49e99-132">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="49e99-133">Du kan också toodetermine hello resursgruppens namn för en befintlig webbtjänst inloggning toohello portal för Microsoft Azure Machine Learning-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="49e99-133">Alternatively, toodetermine hello resource group name of an existing web service, log on toohello Microsoft Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="49e99-134">Välj hello-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="49e99-134">Select hello web service.</span></span> <span data-ttu-id="49e99-135">hello resursgruppens namn används hello femte element av hello URL för webbtjänsten för hello, bara efter hello *resursgrupper* element.</span><span class="sxs-lookup"><span data-stu-id="49e99-135">hello resource group name is hello fifth element of hello URL of hello web service, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="49e99-136">I följande exempel hello, används hello resursgruppens namn standard-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="49e99-136">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-as-json"></a><span data-ttu-id="49e99-137">Exportera hello Web Service Definition som JSON</span><span class="sxs-lookup"><span data-stu-id="49e99-137">Export hello Web Service Definition as JSON</span></span>
<span data-ttu-id="49e99-138">toomodify hello definition toohello tränas modellen toouse Hej nyligen Trained Model, måste du först använda hello [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport den tooa JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="49e99-138">toomodify hello definition toohello trained model toouse hello newly Trained Model, you must first use hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport it tooa JSON format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob-in-hello-json"></a><span data-ttu-id="49e99-139">Uppdatera hello toohello ilearner referensblobben i hello JSON.</span><span class="sxs-lookup"><span data-stu-id="49e99-139">Update hello reference toohello ilearner blob in hello JSON.</span></span>
<span data-ttu-id="49e99-140">Leta upp hello [tränad modell], uppdatering hello i hello tillgångar *uri* värdet i hello *locationInfo* nod med hello hello ilearner blob-URI.</span><span class="sxs-lookup"><span data-stu-id="49e99-140">In hello assets, locate hello [trained model], update hello *uri* value in hello *locationInfo* node with hello URI of hello ilearner blob.</span></span> <span data-ttu-id="49e99-141">hello URI genereras genom att kombinera hello *BaseLocation* och hello *RelativeLocation* från hello utdata från hello BES omtränings-anrop.</span><span class="sxs-lookup"><span data-stu-id="49e99-141">hello URI is generated by combining hello *BaseLocation* and hello *RelativeLocation* from hello output of hello BES retraining call.</span></span> <span data-ttu-id="49e99-142">Detta uppdaterar hello sökvägen tooreference hello ny tränad modell.</span><span class="sxs-lookup"><span data-stu-id="49e99-142">This updates hello path tooreference hello new trained model.</span></span>

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

## <a name="import-hello-json-into-a-web-service-definition"></a><span data-ttu-id="49e99-143">Importera hello JSON till en Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="49e99-143">Import hello JSON into a Web Service Definition</span></span>
<span data-ttu-id="49e99-144">Du måste använda hello [importera AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello ändrade JSON-fil i en Web Service Definition som du kan använda tooupdate hello Web Service Definition.</span><span class="sxs-lookup"><span data-stu-id="49e99-144">You must use hello [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello modified JSON file back into a Web Service Definition that you can use tooupdate hello Web Service Definition.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service-with-new-web-service-definition"></a><span data-ttu-id="49e99-145">Uppdatera hello-webbtjänsten med nya Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="49e99-145">Update hello web service with new Web Service Definition</span></span>
<span data-ttu-id="49e99-146">Slutligen kan du använda [uppdatering AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello Web Service Definition.</span><span class="sxs-lookup"><span data-stu-id="49e99-146">Finally, you use [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello Web Service Definition.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a><span data-ttu-id="49e99-147">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="49e99-147">Summary</span></span>
<span data-ttu-id="49e99-148">Du kan uppdatera hello tränad modell för en förutsägbar webbtjänst att aktivera scenarier som använder hello Machine Learning PowerShell cmdlet: ar:</span><span class="sxs-lookup"><span data-stu-id="49e99-148">Using hello Machine Learning PowerShell management cmdlets, you can update hello trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="49e99-149">Periodiska modell via programmering med nya data.</span><span class="sxs-lookup"><span data-stu-id="49e99-149">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="49e99-150">Distribution av en modell toocustomers med hello målet för att låta dem träna om hello modellen med sina egna data.</span><span class="sxs-lookup"><span data-stu-id="49e99-150">Distribution of a model toocustomers with hello goal of letting them retrain hello model using their own data.</span></span>

