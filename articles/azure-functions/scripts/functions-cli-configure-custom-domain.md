---
title: "aaaAzure CLI skriptexempel - mappa en anpassad domän tooa funktionsapp | Microsoft Docs"
description: "Azure CLI skriptexempel - karta en anpassad domän tooa funktionsapp i Azure."
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c7cb0a3e132b491250623b945aecf6aea4f57c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="map-a-custom-domain-tooa-function-app"></a>Mappa en anpassad domän tooa funktionsapp

Det här exempelskriptet skapar en funktionsapp med relaterade resurser och sedan mappa `www.<yourdomain>` tooit. toomap tooa anpassad domän, appen funktionen måste skapas i en apptjänstplan och inte i en plan för användning. Azure Functions stöder bara mappa en anpassad domän med en A-post.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain tooa function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ storage-konto](https://docs.microsoft.com/cli/azure/storage/account#create) | Skapar ett lagringskonto som krävs av hello funktionsapp. |
| [Skapa AZ programtjänstplan](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Skapar en anpassad domän för toomap en Apptjänst-planen som krävs. |
| [Skapa AZ functionapp]() | Skapar en funktionsapp. |
| [Lägg till AZ apptjänst web config värdnamn](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Avbildar en anpassad domän tooa funktionsapp. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare funktioner CLI skriptexempel finns i hello [Azure Functions dokumentationen]().
