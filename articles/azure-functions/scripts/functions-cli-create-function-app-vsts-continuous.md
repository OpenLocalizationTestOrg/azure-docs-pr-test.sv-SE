---
title: "Skapa en Funktionsapp och distribuera Funktionskoden från Visual Studio Team Services | Microsoft Docs"
description: "Skapa en Funktionsapp och distribuera Funktionskoden från Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 2ef177b55ad7ffd351156821f429e6ff8fbeccc7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-app-service"></a>Skapa en App Service

I det här scenariot du lära dig hur du skapar en funktion app med hjälp av den [förbrukning plan](../functions-scale.md#consumption-plan) med dess relaterade resurser och kontinuerligt distribuerar Funktionskoden från en Visual Studio Team Services VSTS ()-databas. I det här exemplet behöver du:

* En VSTS databas med funktioner kod som du har administrativa behörigheter för.
* En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

Det här exemplet skapar en app i Azure-funktion och distribuerar Funktionskoden från Visual Studio Team Services.

[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon för att skapa en resursgrupp, webbprogram, documentdb och alla relaterade resurser. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ storage-konto](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Skapar en App Service-plan. |
| [Skapa AZ functionapp](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [AZ apptjänst Webbkonfiguration källkontroll](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Associerar en funktionsapp med en Git eller ett. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure Functions CLI skriptexempel finns i den [Azure Functions dokumentationen](../functions-cli-samples.md).
