---
title: "Azure Machine Learning-webbtjänster: Distribution och användning | Microsoft Docs"
description: "Resurser för att distribuera och använda webbtjänster."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: 47635376-d1f4-4ea4-a6af-bd1f99f69a69
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 539c2abb053a0f981be0374defe45cf4d96b740b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure Machine Learning-webbtjänster: Distribution och användning
Du kan använda Azure Machine Learning toodeploy maskininlärning arbetsflöden och modeller som webbtjänster. Dessa webbtjänster kan sedan använda toocall hello machine learning-modeller från program över hello Internet toodo förutsägelser i realtid eller i batchläge. Eftersom hello webbtjänster RESTful kan anropa du dem från olika programmeringsspråk språk och plattformar, till exempel .NET och Java, och program som Excel.

hello nästa avsnitt innehåller länkar toowalkthroughs, kod och dokumentation toohelp komma igång.

## <a name="deploy-a-web-service"></a>Distribuera en webbtjänst
### <a name="with-azure-machine-learning-studio"></a>Med Azure Machine Learning Studio
Machine Learning Studio och hello Microsoft Azure Machine Learning-webbtjänster portalen kan du distribuera och hantera en webbtjänst utan att skriva kod.

hello följande länkar ger allmän Information om hur toodeploy en ny webbtjänst:

* För en översikt över hur toodeploy en ny webbtjänst som baseras på Azure Resource Manager, se [distribuera en ny webbtjänst](machine-learning-webservice-deploy-a-web-service.md).
* En genomgång om hur toodeploy en webbtjänst finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).
* En fullständig genomgång om hur toocreate och distribuera en webbtjänst kan se [genomgången steg 1: skapa en arbetsyta i Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md).
* Vissa exempel som distribuerar en webbtjänst finns:

  * [Genomgång steg 5: Distribuera hello Azure Machine Learning-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)
  * [Hur toodeploy en web service toomultiple regioner](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Med resursprovidern för web services API: er (Azure Resource Manager API: er)
hello Azure Machine Learning resource provider för webbtjänster kan distribution och hantering av webbtjänster med hjälp av REST API-anrop. Mer information finns i [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) referens.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a>Med PowerShell-cmdlets
Azure Machine Learning resource provider för webbtjänster kan distribution och hantering av webbtjänster med PowerShell-cmdlets.

toouse hello cmdlets måste du först logga in tooyour Azure-konto från inom hello PowerShell-miljö med hjälp av hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet. Om du inte känner till hur toocall PowerShell-kommandon som baseras på Resource Manager, se [med hjälp av Azure PowerShell med Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).

tooexport din förutsägande experimentera använder [den här exempelkoden](https://github.com/ritwik20/AzureML-WebServices). När du har skapat hello .exe-fil från hello kod skriver du:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Hello-program körs skapas en web service JSON-mall. toouse hello mallen toodeploy en webbtjänst, måste du lägga till hello följande information:

* Lagringskontonamn och nyckel

    Du kan hämta hello lagringskontonamn och nyckel från antingen hello [Azure-portalen](https://portal.azure.com/) eller hello [klassiska Azure-portalen](http://manage.windowsazure.com/).
* Åtagande plan-ID

    Du kan hämta hello plan ID från hello [Azure Machine Learning-webbtjänster](https://services.azureml.net) portal genom att logga in och klicka på ett namn för energischema.

Lägg till dem toohello JSON-mall som underordnade till hello *egenskaper* nod på hello samma nivå som hello *MachineLearningWorkspace* nod.

Här är ett exempel:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Se följande artiklar hello och exempelkod för ytterligare information:

* [Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference på MSDN
* Exempel [genomgången](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) på GitHub

## <a name="consume-hello-web-services"></a>Använda hello-webbtjänster
### <a name="from-hello-azure-machine-learning-web-services-ui-testing"></a>Från hello Azure Machine Learning Web Services-Användargränssnittet (testar)
Du kan testa webbtjänsten från hello Azure Machine Learning-webbtjänster portalen. Detta inkluderar testning hello svar på begäranden tjänsten (RR) och Batch Execution service (BES)-gränssnitt.

* [Distribuera en ny webbtjänst](machine-learning-webservice-deploy-a-web-service.md)
* [Distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md)
* [Genomgång steg 5: Distribuera hello Azure Machine Learning-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Från Excel
Du kan hämta en Excel-mall som förbrukar hello-webbtjänsten:

* [Använda en Azure Machine Learning-webbtjänst från Excel](machine-learning-consuming-from-excel.md)
* [Excel-tillägg för Azure Machine Learning-webbtjänster](machine-learning-excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>En REST-baserad klient
Azure Machine Learning-webbtjänster är RESTful-API: er. Du kan använda dessa API: er från olika plattformar, till exempel .NET, Python, R, Java, etc. hello **förbruka** för webbtjänsten på hello [Microsoft Azure Machine Learning-webbtjänster portal](https://services.azureml.net) har exempel kod som kan hjälpa dig att komma igång. Mer information finns i [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).
