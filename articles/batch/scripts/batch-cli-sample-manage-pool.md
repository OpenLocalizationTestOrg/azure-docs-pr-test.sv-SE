---
title: aaaAzure CLI skriptexempel - hantera pooler i Batch | Microsoft Docs
description: Azure CLI skriptexempel - hantera pooler i Batch
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
ms.openlocfilehash: 6c9ca9515565aff42752231a080943be8e4c810b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a>Hantera Azure Batch-pooler med Azure CLI

Dessa skript visar några av hello verktygen i hello Azure CLI toocreate och hantera pooler för beräkningsnoder i hello Azure Batch-tjänsten.

> [!NOTE]
> hello kommandona i det här exemplet skapas virtuella Azure-datorer. Virtuella datorer som körs kommer påförs kostnader tooyour konto. toominimize dessa debiteringar ta bort hello virtuella datorer när du är klar körs hello-exemplet. Se [Rensa pooler](#clean-up-pools).

Batch-pooler kan konfigureras på två sätt, antingen med Cloud Services-konfiguration (Windows) eller en virtuell dator-konfiguration (Windows och Linux). hello exempelskript nedan visar hur toocreate pooler med båda konfigurationerna.

## <a name="prerequisites"></a>Krav

- Installera hello Azure CLI med hjälp av anvisningarna i hello hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.
- Skapa ett Batch-konto om du inte redan har ett. Se [skapa ett batchkonto med hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.
- Konfigurera ett program toorun från en start-aktivitet om du inte gjort det ännu. Se [att lägga till program tooAzure Batch med Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) för ett exempelskript som skapar ett program och överför en programmet paketet tooAzure.

## <a name="pool-with-cloud-service-configuration-sample-script"></a>Pool med cloud service configuration exempelskript

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a>Pool med exempelskript konfigurationen för virtuell dator

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a>Rensa pooler

När du har kört hello ovan exempelskript kör du följande kommando toodelete hello pooler hello.
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hello och manipulera Batch pooler.
Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.

| Kommando | Anteckningar |
|---|---|
| [logga in AZ batch-konto](https://docs.microsoft.com/cli/azure/batch/account#login) | Autentisera mot en Batch-kontot.  |
| [Sammanfattning av AZ batch-program](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | Lista över hello tillgängliga program i hello Batch-kontot.  |
| [Skapa AZ batch-pool](https://docs.microsoft.com/cli/azure/batch/pool#create) | Skapa en pool för virtuella datorer.  |
| [AZ batch-pool uppsättningen](https://docs.microsoft.com/cli/azure/batch/pool#set) | Uppdatera egenskaperna för en pool.  |
| [listan för AZ batch pool nod-agent-SKU: er](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | Lista över tillgängliga noden agent SKU: er och bild information.  |
| [Ändra storlek AZ batch-pool](https://docs.microsoft.com/cli/azure/batch/pool#resize) | Ändra storlek på hello antalet virtuella datorer som körs i hello angetts poolen.  |
| [Visa för AZ batch-pool](https://docs.microsoft.com/cli/azure/batch/pool#show) | Visar hello egenskaperna för en pool.  |
| [ta bort AZ batch-pool](https://docs.microsoft.com/cli/azure/batch/pool#delete) | Ta bort hello angivna poolen.  |
| [Aktivera AZ batch-pool Autoskala](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | Aktivera automatisk skalning på poolen och tillämpa en formel.  |
| [inaktivera AZ batch-pool Autoskala](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | Inaktivera automatisk skalning på poolen.  |
| [AZ batch nodlistan](https://docs.microsoft.com/cli/azure/batch/node#list) | Lista över alla hello datornod i hello angivna poolen.  |
| [omstart av AZ batch nod](https://docs.microsoft.com/cli/azure/batch/node#reboot) | Starta om hello angivna compute-nod.  |
| [ta bort AZ batch-nod](https://docs.microsoft.com/cli/azure/batch/node#delete) | Ta bort hello visas noder från hello angetts poolen.  |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Batch CLI skriptexempel finns i hello [Azure Batch CLI dokumentationen](../batch-cli-samples.md).

