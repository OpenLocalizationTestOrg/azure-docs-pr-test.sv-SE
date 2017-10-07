---
title: "aaaCreate en Funktionsapp och distribuera Funktionskoden från Visual Studio Team Services | Microsoft Docs"
description: "Skapa en Funktionsapp och distribuera Funktionskoden från Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 774bee73025cc9ac46f8b2a6c10edbfa3c2d353b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-app-service"></a>Skapa en App Service

I det här scenariot får du lära dig hur toocreate en funktion appen med hello [förbrukning plan](../functions-scale.md#consumption-plan) med dess relaterade resurser och kontinuerligt distribuerar Funktionskoden från en Visual Studio Team Services VSTS ()-databas. I det här exemplet behöver du:

* En VSTS databas med funktioner kod som du har administrativa behörigheter för.
* En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

Det här exemplet skapar en app i Azure-funktion och distribuerar Funktionskoden från Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram, documentdb och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ storage-konto](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Skapar en App Service-plan. |
| [Skapa AZ functionapp](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [AZ apptjänst Webbkonfiguration källkontroll](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Associerar en funktionsapp med en Git eller ett. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure Functions CLI skriptexempel finns i hello [Azure Functions dokumentationen](../functions-cli-samples.md).
