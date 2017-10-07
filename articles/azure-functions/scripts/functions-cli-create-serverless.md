---
title: "aaaAzure CLI skriptexempel - skapa en Funktionsapp för serverlösa körning | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en Funktionsapp för serverlösa körning"
services: functions
documentationcenter: functions
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 04/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 7ec872b642e50827896a73a9e43bcc87233e15c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a>Skapa en Funktionsapp för serverlösa körning

Det här exempelskriptet skapar en Azure-Funktionsapp, vilket är en behållare för dina funktioner. Hej Funktionsapp skapas med hjälp av hello [förbrukning plan](../functions-scale.md#consumption-plan), vilket är idealiskt för händelsedriven serverlösa arbetsbelastningar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

Det här skriptet skapar en Azure-funktion-app med hello [förbrukning plan](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Varje kommando i hello tabellen länkar toocommand viss dokumentation. Det här skriptet använder hello följande kommandon:

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ storage-konto](/cli/azure/storage/account#create) | Skapar ett Azure Storage-konto. |
| [Skapa AZ functionapp](https://docs.microsoft.com/cli/azure/functionapp#create) | Skapar en Azure-funktion. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).
