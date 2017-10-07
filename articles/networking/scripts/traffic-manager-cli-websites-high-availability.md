---
title: "aaaAzure skriptexempel CLI - vägtrafik för hög tillgänglighet för program | Microsoft Docs"
description: "Skriptexempel Azure CLI - vägtrafik för hög tillgänglighet för program"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: tysonn
tags: azure-infrastructure
ms.assetid: 
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: 2142c8bbec1dffc2f12b5500df142a429393a145
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Vidarebefordra trafik för hög tillgänglighet för program

Det här skriptet skapar en resursgrupp, två apptjänstplaner, två webbappar, en traffic manager-profil och två traffic manager-slutpunkter. Traffic Manager dirigerar trafik toohello programmet i en region som hello primär region och toohello sekundär region när programmet hello i hello primära regionen är inte tillgänglig. Innan du kör skriptet hello måste du ändra hello MyWebApp, MyWebAppL1 och MyWebAppL2 värdena toounique värdena i Azure. Du kan komma åt hello app i hello primär region med hello URL mywebapp.trafficmanager.net när du har kört hello skript.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a>Rensa distribution 

Efter hello skriptexempel har körts kan hello Följ kommandot vara används tooremove hello resursgrupp, App Service-appen och alla relaterade resurser.

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate en resursgrupp, webbprogram, trafikhanterarprofilen hello och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ programtjänstplan](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Skapar en App Service-plan. Detta påminner om en servergrupp för din Azure webbapp. |
| [Skapa AZ apptjänst web](https://docs.microsoft.com/cli/azure/appservice/web#create) | Skapar en Azure webbapp i hello App Service-plan. |
| [Skapa AZ network traffic manager-profilen](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | Skapar en Azure Traffic Manager-profil. |
| [Skapa AZ network traffic manager-slutpunkt](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | Lägger till en slutpunkt tooan Azure Traffic Manager-profilen. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare App Service CLI skriptexempel finns i hello [Azure nätverk dokumentationen](../cli-samples.md).
