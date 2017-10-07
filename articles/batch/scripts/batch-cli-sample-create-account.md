---
title: aaaAzure CLI skriptexempel - skapa ett Batch-konto | Microsoft Docs
description: "Azure CLI-skript exempel – skapa ett Batch-konto"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: 62b640eebbafdd1081822a638fd0720121ef067a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-cli"></a>Skapa ett batchkonto med hello Azure CLI

Det här skriptet skapar ett Azure Batch-konto och visar hur olika egenskaper för hello-konto kan efterfrågas och uppdateras.

## <a name="prerequisites"></a>Krav

Installera hello Azure CLI med hjälp av anvisningarna i hello hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.

## <a name="batch-account-sample-script"></a>Exempelskript för batch-konto

När du skapar en Batch-kontot tilldelas som standard dess datornoderna internt av hello Batch-tjänsten. Allokerade datornoderna blir ämne tooa separat kärnkvot och hello kan autentiseras antingen via autentiseringsuppgifterna för delad nyckel eller ett Azure Active Dirctory-token.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a>Batch-kontot med användaren exempelskript för prenumeration

Du kan också välja toohave Batch skapa dess compute-noder i din egen Azure-prenumeration.
Konton som allokerar beräkna noder till din prenumeration måste autentiseras via Azure Active Directory-token och hello datornoderna allokerade räknas som en prenumerationens kvoter. toocreate ett konto i det här läget, en måste ange en Key Vault-referens när du skapar hello-konto.

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a>Rensa distribution

När du har kört antingen hello ovan exempelskript kör hello efter kommandot tooremove resursgruppen och alla relaterade resurser (inklusive Batch-konton, Azure Storage-konton och Azure nyckelvalv).

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, Batch-kontot och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ batch-kontot](https://docs.microsoft.com/cli/azure/batch/account#create) | Skapar hello Batch-kontot.  |
| [AZ batch inställt](https://docs.microsoft.com/cli/azure/batch/account#set) | Uppdaterar egenskaperna för hello Batch-kontot.  |
| [Visa för AZ batch-konto](https://docs.microsoft.com/cli/azure/batch/account#show) | Hämtar information om hello angetts Batch-kontot.  |
| [AZ batch-kontot nycklar lista](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | Hämtar hello snabbtangenter för hello angetts Batch-kontot.  |
| [logga in AZ batch-konto](https://docs.microsoft.com/cli/azure/batch/account#login) | Autentiseras mot hello angetts Batch-kontot för ytterligare CLI interaktion.  |
| [Skapa AZ storage-konto](https://docs.microsoft.com/cli/azure/storage/account#create) | Skapar ett lagringskonto. |
| [Skapa AZ keyvault](https://docs.microsoft.com/cli/azure/keyvault#create) | Skapar ett nyckelvalv. |
| [Ange en AZ keyvault-princip](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | Uppdatera hello säkerhetsprincipen för hello angivna nyckelvalvet. |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/group#delete) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Batch CLI skriptexempel finns i hello [Azure Batch CLI dokumentationen](../batch-cli-samples.md).
