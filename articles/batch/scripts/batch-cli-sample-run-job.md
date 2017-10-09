---
title: "aaaAzure CLI skriptexempel - köra ett jobb med Batch | Microsoft Docs"
description: "Azure CLI-skriptexempel - köra ett jobb med Batch"
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
ms.openlocfilehash: f73a7cb341e550fd1c92f92201e20b27fa20d238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a>Jobb som körs på Azure Batch med Azure CLI

Det här skriptet skapar ett batchjobb och lägger till en serie uppgifter toohello jobb. Den visar även hur toomonitor ett jobb och dess uppgifter. Slutligen visar hur tooquery hello Batch-tjänsten för information om hello jobbets uppgifter effektivt.

## <a name="prerequisites"></a>Krav

- Installera hello Azure CLI med hjälp av anvisningarna i hello hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.
- Skapa ett Batch-konto om du inte redan har ett. Se [skapa ett batchkonto med hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.
- Konfigurera ett program toorun från en start-aktivitet om du inte gjort det ännu. Se [att lägga till program tooAzure Batch med Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) för ett exempelskript som skapar ett program och överför en programmet paketet tooAzure.
- Konfigurera en pool som hello jobbet ska köras. Se [hantera Azure Batch-pooler med Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) för ett exempelskript som skapar en pool med en tjänstkonfiguration moln eller en konfiguration av virtuell dator.

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a>Rensa jobb

När du har kört hello ovan exempelskript kör följande kommando tooremove hello jobbet och alla aktiviteter. Observera att hello poolen måste toobe bort separat. Se [hantera Azure Batch-pooler med Azure CLI](./batch-cli-sample-manage-pool.md) för mer information om hur du skapar och tar bort pooler.

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hello batchjobb och uppgifter. Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.

| Kommando | Anteckningar |
|---|---|
| [logga in AZ batch-konto](https://docs.microsoft.com/cli/azure/batch/account#login) | Autentisera mot en Batch-kontot.  |
| [Skapa AZ batchjobb](https://docs.microsoft.com/cli/azure/batch/job#create) | Skapar ett batchjobb.  |
| [AZ batch-jobbet uppsättningen](https://docs.microsoft.com/cli/azure/batch/job#set) | Uppdaterar egenskaperna för ett batchjobb.  |
| [AZ batch-jobbet visar](https://docs.microsoft.com/cli/azure/batch/job#show) | Hämtar information om batchjobb angivna.  |
| [Skapa AZ batchaktiviteten](https://docs.microsoft.com/cli/azure/batch/task#create) | Lägger till en aktivitet toohello angetts Batch-jobbet.  |
| [AZ batch uppgiften visa](https://docs.microsoft.com/cli/azure/batch/task#show) | Hämtar hello information om en aktivitet från hello angetts Batch-jobbet.  |
| [uppgiftslista för AZ batch](https://docs.microsoft.com/cli/azure/batch/task#list) | Visar hello uppgifter som är kopplad till hello angivna jobbet.  |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Batch CLI skriptexempel finns i hello [Azure Batch CLI dokumentationen](../batch-cli-samples.md).
