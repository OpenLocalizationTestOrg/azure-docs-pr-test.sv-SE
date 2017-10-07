---
title: "aaaAzure CLI skriptexempel - skapa en webbapp med kontinuerlig distribution från GitHub | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en webbapp med kontinuerlig distribution från GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6adb06a35ceea8ea64723c9887c25c50f046e280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>Skapa en webbapp med kontinuerlig distribution från GitHub

Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan konfigurerar kontinuerlig distribution från en GitHub-databas. GitHub distribution utan kontinuerlig distribution finns [skapa en webbapp och distribuera kod från GitHub](app-service-cli-deploy-github.md). I det här exemplet behöver du:

* En GitHub-databas med programkod som du har administrativa behörigheter för.
* En [personlig åtkomst-Token (PATRIK)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) för ditt GitHub-konto.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ programtjänstplan](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Skapar en App Service-plan. |
| [Skapa AZ webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Skapar ett Azure-webbapp. |
| [AZ webapp distributionskonfiguration källa](https://docs.microsoft.com/cli/azure/webapp/deployment/source#config) | Associerar en Azure-app med en Git eller ett lagringsplats. |
| [AZ webapp Bläddra](https://docs.microsoft.com/cli/azure/webapp#browse) | Öppna en Azure-app i en webbläsare. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).
