---
title: "Träna om en ny Azure Machine Learning-webbtjänst med PowerShell | Microsoft Docs"
description: "Lär dig mer om att genom programmering träna om en modell och uppdatera webbtjänsten för att använda den nya tränade modellen i Azure Machine Learning med Machine Learning Management PowerShell-cmdlets."
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
ms.openlocfilehash: 804dd59e62f38ee1878045d93211ee18e0d5bfce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-the-machine-learning-management-powershell-cmdlets"></a><span data-ttu-id="d3764-103">Träna om en ny Resource Manager-baserat webbtjänst med hjälp av Machine Learning Management PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="d3764-103">Retrain a New Resource Manager based web service using the Machine Learning Management PowerShell cmdlets</span></span>
<span data-ttu-id="d3764-104">När du träna om en ny webbtjänst uppdatera förutsägande web service definition för att referera till den nya tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="d3764-104">When you retrain a New web service, you update the predictive web service definition to reference the new trained model.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="d3764-105">Krav</span><span class="sxs-lookup"><span data-stu-id="d3764-105">Prerequisites</span></span>
<span data-ttu-id="d3764-106">Du måste skapa ett experiment utbildning och prediktivt experiment som visas i [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="d3764-106">You must set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="d3764-107">Prediktivt experiment måste distribueras som en Azure Resource Manager (nya) baserad machine learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d3764-107">The predictive experiment must be deployed as an Azure Resource Manager (New) based machine learning web service.</span></span> <span data-ttu-id="d3764-108">Om du vill distribuera en ny webbtjänst måste du ha tillräckliga behörigheter i prenumerationen som du distribuerar webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d3764-108">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="d3764-109">Mer information finns i [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="d3764-109">For more information, see [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="d3764-110">Ytterligare information om distribuera webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="d3764-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="d3764-111">Den här processen kräver att du har installerat Azure Machine Learning-Cmdlets.</span><span class="sxs-lookup"><span data-stu-id="d3764-111">This process requires that you have installed the Azure Machine Learning Cmdlets.</span></span> <span data-ttu-id="d3764-112">Information om installation av Machine Learning-cmdlets finns i [Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference på MSDN.</span><span class="sxs-lookup"><span data-stu-id="d3764-112">For information installing the Machine Learning cmdlets, see the [Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN.</span></span>

<span data-ttu-id="d3764-113">Kopiera följande information från omtränings utdata:</span><span class="sxs-lookup"><span data-stu-id="d3764-113">Copied the following information from the retraining output:</span></span>

* <span data-ttu-id="d3764-114">BaseLocation</span><span class="sxs-lookup"><span data-stu-id="d3764-114">BaseLocation</span></span>
* <span data-ttu-id="d3764-115">RelativeLocation</span><span class="sxs-lookup"><span data-stu-id="d3764-115">RelativeLocation</span></span>

<span data-ttu-id="d3764-116">Du vidta åtgärder är:</span><span class="sxs-lookup"><span data-stu-id="d3764-116">The steps you take are:</span></span>

1. <span data-ttu-id="d3764-117">Logga in på ditt konto i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d3764-117">Sign in to your Azure Resource Manager account.</span></span>
2. <span data-ttu-id="d3764-118">Hämta web service definition</span><span class="sxs-lookup"><span data-stu-id="d3764-118">Get the web service definition</span></span>
3. <span data-ttu-id="d3764-119">Exportera Web Service Definition som JSON</span><span class="sxs-lookup"><span data-stu-id="d3764-119">Export the Web Service Definition as JSON</span></span>
4. <span data-ttu-id="d3764-120">Uppdatera referens till ilearner-blob i JSON.</span><span class="sxs-lookup"><span data-stu-id="d3764-120">Update the reference to the ilearner blob in the JSON.</span></span>
5. <span data-ttu-id="d3764-121">Importera JSON till en Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="d3764-121">Import the JSON into a Web Service Definition</span></span>
6. <span data-ttu-id="d3764-122">Uppdatera webbtjänsten med nya Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="d3764-122">Update the web service with new Web Service Definition</span></span>

## <a name="sign-in-to-your-azure-resource-manager-account"></a><span data-ttu-id="d3764-123">Logga in på Azure Resource Manager-konto</span><span class="sxs-lookup"><span data-stu-id="d3764-123">Sign in to your Azure Resource Manager account</span></span>
<span data-ttu-id="d3764-124">Du måste först logga in på ditt Azure-konto från i PowerShell-miljö med den [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d3764-124">You must first sign in to your Azure account from within the PowerShell environment using the [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-the-web-service-definition"></a><span data-ttu-id="d3764-125">Hämta Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="d3764-125">Get the Web Service Definition</span></span>
<span data-ttu-id="d3764-126">Hämta sedan webbtjänsten genom att anropa den [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d3764-126">Next, get the Web Service by calling the [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="d3764-127">Web Service Definition är en intern representation av den tränade modellen för webbtjänsten och kan inte ändras direkt.</span><span class="sxs-lookup"><span data-stu-id="d3764-127">The Web Service Definition is an internal representation of the trained model of the web service and is not directly modifiable.</span></span> <span data-ttu-id="d3764-128">Kontrollera att du hämtar Web Service Definition för experimentet förutsägbara och inte experimentet utbildning.</span><span class="sxs-lookup"><span data-stu-id="d3764-128">Make sure that you are retrieving the Web Service Definition for your Predictive experiment and not your training experiment.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="d3764-129">För att fastställa resursgruppens namn för en befintlig webbtjänst, kör du cmdleten Get-AzureRmMlWebService utan några parametrar för att visa webbtjänsterna i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d3764-129">To determine the resource group name of an existing web service, run the Get-AzureRmMlWebService cmdlet without any parameters to display the web services in your subscription.</span></span> <span data-ttu-id="d3764-130">Leta upp webbtjänsten och titta på dess web service-ID.</span><span class="sxs-lookup"><span data-stu-id="d3764-130">Locate the web service, and then look at its web service ID.</span></span> <span data-ttu-id="d3764-131">Namnet på resursgruppen är det fjärde elementet med ID precis efter den *resursgrupper* element.</span><span class="sxs-lookup"><span data-stu-id="d3764-131">The name of the resource group is the fourth element in the ID, just after the *resourceGroups* element.</span></span> <span data-ttu-id="d3764-132">I följande exempel är resursgruppens namn standard-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="d3764-132">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="d3764-133">Du kan också ta reda på resursgruppens namn för en befintlig webbtjänst genom loggar du in på Microsoft Azure Machine Learning-webbtjänster portal.</span><span class="sxs-lookup"><span data-stu-id="d3764-133">Alternatively, to determine the resource group name of an existing web service, log on to the Microsoft Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="d3764-134">Välj webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d3764-134">Select the web service.</span></span> <span data-ttu-id="d3764-135">Resursgruppens namn är den femte del av URL för webbtjänsten precis efter den *resursgrupper* element.</span><span class="sxs-lookup"><span data-stu-id="d3764-135">The resource group name is the fifth element of the URL of the web service, just after the *resourceGroups* element.</span></span> <span data-ttu-id="d3764-136">I följande exempel är resursgruppens namn standard-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="d3764-136">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a><span data-ttu-id="d3764-137">Exportera Web Service Definition som JSON</span><span class="sxs-lookup"><span data-stu-id="d3764-137">Export the Web Service Definition as JSON</span></span>
<span data-ttu-id="d3764-138">Ändra definitionen för den tränade modellen att använda den nya utbildade modellen måste du först använda den [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) för att exportera den till en fil i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="d3764-138">To modify the definition to the trained model to use the newly Trained Model, you must first use the [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet to export it to a JSON format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a><span data-ttu-id="d3764-139">Uppdatera referens till ilearner-blob i JSON.</span><span class="sxs-lookup"><span data-stu-id="d3764-139">Update the reference to the ilearner blob in the JSON.</span></span>
<span data-ttu-id="d3764-140">Leta upp [tränad modell] i tillgångarna, uppdatera den *uri* värde i den *locationInfo* nod med ilearner-blob-URI.</span><span class="sxs-lookup"><span data-stu-id="d3764-140">In the assets, locate the [trained model], update the *uri* value in the *locationInfo* node with the URI of the ilearner blob.</span></span> <span data-ttu-id="d3764-141">URI: N skapas genom att kombinera den *BaseLocation* och *RelativeLocation* från utdata från BES omtränings anrop.</span><span class="sxs-lookup"><span data-stu-id="d3764-141">The URI is generated by combining the *BaseLocation* and the *RelativeLocation* from the output of the BES retraining call.</span></span> <span data-ttu-id="d3764-142">Sökvägen för att referera till den nya tränade modellen uppdateras.</span><span class="sxs-lookup"><span data-stu-id="d3764-142">This updates the path to reference the new trained model.</span></span>

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

## <a name="import-the-json-into-a-web-service-definition"></a><span data-ttu-id="d3764-143">Importera JSON till en Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="d3764-143">Import the JSON into a Web Service Definition</span></span>
<span data-ttu-id="d3764-144">Du måste använda den [importera AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) för att konvertera den ändrade JSON-filen till en Web Service Definition som du kan använda för att uppdatera Web Service Definition.</span><span class="sxs-lookup"><span data-stu-id="d3764-144">You must use the [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet to convert the modified JSON file back into a Web Service Definition that you can use to update the Web Service Definition.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a><span data-ttu-id="d3764-145">Uppdatera webbtjänsten med nya Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="d3764-145">Update the web service with new Web Service Definition</span></span>
<span data-ttu-id="d3764-146">Slutligen kan du använda [uppdatering AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) att uppdatera Web Service Definition.</span><span class="sxs-lookup"><span data-stu-id="d3764-146">Finally, you use [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet to update the Web Service Definition.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a><span data-ttu-id="d3764-147">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="d3764-147">Summary</span></span>
<span data-ttu-id="d3764-148">Med Machine Learning PowerShell management-cmdlets kan uppdatera du den tränade modellen av en förutsägbar webbtjänsten för att aktivera scenarier som:</span><span class="sxs-lookup"><span data-stu-id="d3764-148">Using the Machine Learning PowerShell management cmdlets, you can update the trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="d3764-149">Periodiska modell via programmering med nya data.</span><span class="sxs-lookup"><span data-stu-id="d3764-149">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="d3764-150">Distribution av en modell för kunder med målet att låta dem träna om modellen med sina egna data.</span><span class="sxs-lookup"><span data-stu-id="d3764-150">Distribution of a model to customers with the goal of letting them retrain the model using their own data.</span></span>

