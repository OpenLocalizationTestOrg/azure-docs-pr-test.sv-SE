---
title: aaaDeploy Azure-mall med SAS-token och PowerShell | Microsoft Docs
description: "Använda Azure Resource Manager och Azure PowerShell toodeploy resurser tooAzure från en mall som är skyddat med SAS-token."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: b95e096591d6213f8ef79235c8cd85705c4b79ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a>Distribuera privata Resource Manager-mall med SAS-token och Azure PowerShell

När mallen finns i ett lagringskonto, kan du begränsa åtkomst toohello mallen och ange en signatur (SAS) delad åtkomst-token under distributionen. Det här avsnittet beskrivs hur toouse Azure PowerShell med Resource Manager mallar tooprovide en SAS-token under distributionen. 

## <a name="add-private-template-toostorage-account"></a>Lägga till privata mallen toostorage konto

Du kan lägga till mallar tooa storage-konto och länka toothem under distributionen med en SAS-token.

> [!IMPORTANT]
> Genom att följa hello stegen nedan är hello blob som innehåller hello mall tillgänglig tooonly hello Kontoägare. När du skapar en SAS-token för hello blob är hello blob tillgänglig tooanyone med den URI. Om en annan användare spärras hello URI, är den användaren kan tooaccess hello-mallen. Med hjälp av en SAS-token är ett bra sätt för att begränsa åtkomst tooyour mallar bör, men du inte känsliga data som lösenord direkt i hello mallen.
> 
> 

hello följande exempel ställer in en behållare för privat lagring-konto och överför en mall:
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a>Ange SAS-token under distributionen
toodeploy en privat mall i ett lagringskonto genererar en SAS-token och inkludera den i hello URI för hello mall. Ange hello utgången tid tooallow tillräckligt med tid toocomplete hello-distribution.
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get hello URI with hello SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

Ett exempel på hur du använder en SAS-token med länkade mallar finns [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).


## <a name="next-steps"></a>Nästa steg
* En introduktion toodeploying mallar, se [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md).
* En fullständig exempelskript som distribuerar en mall finns [skript för distribuera Resource Manager-mallar](resource-manager-samples-powershell-deploy.md)
* toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

