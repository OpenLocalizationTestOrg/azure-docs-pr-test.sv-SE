---
title: aaaCreate en Azure-funktion som ansluter tooan Azure Storage | Microsoft Docs
description: Azure CLI-skript Sample - skapa en Azure-funktion som ansluter tooan Azure Storage
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: a51a2c17149478eb2d3d0d4034400ed00cd8416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a>Integrera Funktionsapp i Azure Storage-konto

Det här exempelskriptet skapar en Funktionsapp och Storage-konto.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

Det här exemplet skapar en app i Azure-funktion och lägger till hello lagring tooan appinställningen för anslutningssträngen.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Rensa distribution

Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, App Service-appen och alla relaterade resurser:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ inloggning](https://docs.microsoft.com/cli/azure/#login) | Inloggningen tooAzure. |
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapa en resursgrupp med platsen |
| [Skapa AZ storage-konto](https://docs.microsoft.com/cli/azure/storage/account) | skapar ett lagringskonto |
| [Skapa AZ functionapp](https://docs.microsoft.com/cli/azure/functionapp#create) | Skapa en ny funktionsapp |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/group#delete) | Rensa |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).
