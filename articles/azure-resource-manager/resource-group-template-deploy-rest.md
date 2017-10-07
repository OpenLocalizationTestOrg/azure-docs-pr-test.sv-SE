---
title: aaaDeploy resurser med REST-API och mall | Microsoft Docs
description: "Använd Azure Resource Manager och hanteraren för filserverresurser REST API toodeploy en tooAzure resurser. hello resurser har definierats i en Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: tomfitz
ms.openlocfilehash: 347ad3bdb604429e7291297d448688204af69b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Distribuera resurser med Resource Manager-mallar och Resource Manager REST API
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST-API](resource-group-template-deploy-rest.md)
> 
> 

Den här artikeln förklarar hur toouse hello Resource Manager REST-API med Resource Manager mallar toodeploy tooAzure dina resurser.  

> [!TIP]
> För hjälp med felsökning av ett fel under distributionen, se:
> 
> * [Visa distributionsåtgärder](resource-manager-deployment-operations.md) toolearn om att få information som hjälper dig felsöka din fel
> * [Felsök vanliga fel när du distribuerar tooAzure resurser med Azure Resource Manager](resource-manager-common-deployment-errors.md) toolearn hur tooresolve vanliga distributionsfel
> 
> 

Mallen kan vara en lokal fil eller en extern fil som är tillgängliga via en URI. När mallen finns i ett lagringskonto, kan du begränsa åtkomst toohello mallen och ange en signatur (SAS) delad åtkomst-token under distributionen.

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-hello-rest-api"></a>Distribuera med hello REST API
1. Ange [gemensamma parametrar och rubriker](https://docs.microsoft.com/rest/api/index), inklusive autentiseringstoken.
2. Om du inte har en befintlig resursgrupp kan du skapa en resursgrupp. Ange ditt prenumerations-ID, hello namnet på hello ny resursgrupp och plats som du behöver för din lösning. Mer information finns i [skapa en resursgrupp](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. Verifiera distributionen innan du kör den genom att köra hello [validera en malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) igen. När du testar hello distribution, ange parametrar exakt samma sätt som när du kör hello-distribution (visas i nästa steg i hello).
4. Skapa en distribution. Ange ditt prenumerations-ID, hello namnet på hello resursgrupp, hello namnet på hello distribution och en länk tooyour mall. Information om hello mallfilen finns [parameterfilen](#parameter-file). Läs mer om hello REST API toocreate en resursgrupp, [skapar en för malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). Meddelande hello **läge** har angetts för**stegvis**. Ange toorun en fullständig distribution **läge** för**Slutför**. Var försiktig när du använder hello fullständig läget som av misstag kan du ta bort resurser som inte finns i mallen.
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0"
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0"
              }
            }
          }
   
      Även om du vill toolog svar innehåll, begär innehåll eller båda **debugSetting** i hello-begäran.
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      Du kan ställa in din lagring konto toouse signatur (SAS) för delade åtkomsttoken. Mer information finns i [delegera åtkomst med en signatur för delad åtkomst](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).
5. Hämta hello status för hello malldistribution. Mer information finns i [få information om en malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a>Parameterfilen

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Nästa steg
* toolearn om hantering av asynkrona REST-åtgärder, se [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).
* Ett exempel för att distribuera resurser via hello .NET-klientbibliotek finns [distribuera resurser med hjälp av .NET-bibliotek och en mall](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).
* Anvisningar om hur du distribuerar din lösning toodifferent miljöer finns [utvecklings- och testmiljöer i Microsoft Azure](solution-dev-test-environments.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

