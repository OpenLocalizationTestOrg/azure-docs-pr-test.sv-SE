---
title: Skapa en Azure-funktion som ansluter till en Azure Storage | Microsoft Docs
description: Azure CLI-skript Sample - skapa en Azure-funktion som ansluter till en Azure Storage
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
ms.openlocfilehash: 36dbc2c181c9991a27163e3194800f63c6c0e01e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-function-app-into-azure-storage-account"></a>Integrera Funktionsapp i Azure Storage-konto

Det här exempelskriptet skapar en Funktionsapp och Storage-konto.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

Det här exemplet skapar en app i Azure-funktion och lägger till anslutningssträngen för lagring till en appinställning.

[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "integrera funktionen appen till Azure Storage-konto")]


## <a name="clean-up-deployment"></a>Rensa distribution

När skriptexempel har körts följande kommando kan användas för att ta bort resursgruppen, App Service-appen och alla relaterade resurser:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ inloggning](https://docs.microsoft.com/cli/azure/#login) | Logga in på Azure. |
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapa en resursgrupp med platsen |
| [Skapa AZ storage-konto](https://docs.microsoft.com/cli/azure/storage/account) | skapar ett lagringskonto |
| [Skapa AZ functionapp](https://docs.microsoft.com/cli/azure/functionapp#create) | Skapa en ny funktionsapp |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/group#delete) | Rensa |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure Functions CLI skriptexempel finns i den [Azure Functions dokumentationen](../functions-cli-samples.md).
