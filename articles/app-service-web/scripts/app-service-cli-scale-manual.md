---
title: "aaaAzure CLI skriptexempel - skala ett webbprogram manuellt med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Skriptexempel Azure CLI - skala ett webbprogram manuellt med hjälp av Azure CLI 2.0"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 64464c8a44522fdc2c8f3d0192388302a1d12667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-manually"></a>Skala ett webbprogram manuellt

I det här scenariot får du lära dig toocreate en resursgrupp, apptjänst planerings- och webb-app. Du kommer sedan att skala hello App Service-Plan från en enda instans toomultiple instanser.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, webbprogram och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ programtjänstplan](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Skapar en App Service-plan. Detta påminner om en servergrupp för din Azure webbapp. |
| [Skapa AZ webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Skapar ett Azure-webbapp. |
| [AZ apptjänst plan uppdatering](https://docs.microsoft.com/cli/azure/appservice/plan#update) | Uppdaterar egenskaperna för hello App Service-plan. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).
