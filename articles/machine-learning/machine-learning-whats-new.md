---
title: "aaaWhat är ny i Azure Machine Learning | Microsoft Docs"
description: "Nya funktioner som är tillgängliga i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 6532aabcff023d6c81f79f21d501335c234c55fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-azure-machine-learning"></a>Vad är nytt i Azure Machine Learning?

### <a name="hello-march-2017-release-of-microsoft-azure-machine-learning-updates-provides-hello-following-feature"></a>hello mars 2017 versionen av Microsoft Azure Machine Learning uppdateringar innehåller hello följande funktion:



* Dedikerade kapacitet för Azure Machine Learning BES-jobb

    Machine Learning Batch-Pool bearbetning använder hello [Azure Batch](../batch/batch-technical-overview.md) tooprovide kundhanterad skala för hello Azure Machine Learning Batch Execution Service-tjänsten. Poolen batchbearbetning kan toocreate Azure Batch-pooler som du kan skicka batchjobb och ha dem köra förutsägbart.

    Mer information finns i [Azure Batch-tjänsten för Machine Learning jobb](machine-learning-dedicated-capacity-for-bes-jobs.md).


### <a name="hello-august-2016-release-of-microsoft-azure-machine-learning-updates-provide-hello-following-features"></a>hello augusti 2016-versionen av Microsoft Azure Machine Learning uppdateringar innehåller hello följande funktioner:
* Klassiska webbtjänster kan hanteras i hello nya [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/) portal som tillhandahåller en plats toomanage alla aspekter av webbtjänsten.    
  * Som innehåller webbtjänsten [användningsstatistik](machine-learning-manage-new-webservice.md).
  * Förenklar testning av Azure Machine Learning fjärr-begäran-anrop med exempeldata.
  * Ger en ny Batch Execution Service testsida exempel data och jobbet skicka historik.
  * Tillhandahåller enklare hantering av slutpunkten.

### <a name="hello-july-2016-release-of-microsoft-azure-machine-learning-updates-provide-hello-following-features"></a>hello juli 2016-versionen av Microsoft Azure Machine Learning uppdateringar innehåller hello följande funktioner:
* Webbtjänster som nu hanteras som Azure-resurser som hanteras via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) gränssnitt, vilket ger hello följande förbättringar:
  * Det finns nya [REST API: er](https://msdn.microsoft.com/library/azure/Dn950030.aspx) toodeploy och hantera Resource Manager baserat webbtjänsterna.
  * Det finns en ny [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/) portal som tillhandahåller en plats toomanage alla aspekter av webbtjänsten.
* Innehåller en ny prenumeration-baserade, flera regioner web service distributionsmodell med Resource Manager baserade API: er som utnyttjar hello Resource Manager Resource Provider för webbtjänster.
* Inför nya [prissättningar](https://azure.microsoft.com/pricing/details/machine-learning/) och planera hanteringsmöjligheter med hello nya Resource Manager RP för fakturering.
  * Du kan nu [distribuera din web service toomultiple regioner](machine-learning-how-to-deploy-to-multiple-regions.md) utan att behöva toocreate en prenumeration i varje region.
* Innehåller webbtjänsten [användningsstatistik](machine-learning-manage-new-webservice.md).
* Förenklar testning av Azure Machine Learning fjärr-begäran-anrop med exempeldata.
* Ger en ny Batch Execution Service testsida exempel data och jobbet skicka historik.

Dessutom hello Machine Learning Studio har uppdaterats tooallow du toodeploy toohello ny webbtjänst modell eller fortsätta toodeploy toohello klassiska Web service modellen. 

