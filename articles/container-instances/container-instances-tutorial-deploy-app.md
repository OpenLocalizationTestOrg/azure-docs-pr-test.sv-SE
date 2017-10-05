---
title: "Azure Behållarinstanser tutorial – distribuera appen | Microsoft Docs"
description: "Azure Behållarinstanser tutorial – distribuera appen"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 54151a5c1850ab7120fe666a46dc5dc99c0f5157
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Distribuera en behållare till Behållarinstanser som Azure

Det här är sist av tre delar självstudiekursen. I föregående avsnitt [en behållare avbildningen skapades](container-instances-tutorial-prepare-app.md) och [pushas till ett Azure Container registret](container-instances-tutorial-prepare-acr.md). Det här avsnittet Slutför kursen genom att distribuera behållaren till Azure-Behållarinstanser. Slutfört stegen innefattar:

> [!div class="checklist"]
> * Ange en behållare grupp med en Azure Resource Manager-mall
> * Distribuera behållargruppen med hjälp av Azure CLI
> * Visa behållaren loggar

## <a name="deploy-the-container-using-the-azure-cli"></a>Distribuera behållare med hjälp av Azure CLI

Azure CLI gör det möjligt för distribution av en behållare till Azure-Behållarinstanser i ett enda kommando. Eftersom bilden behållaren finns i registret för privat Azure-behållare, måste du inkludera de autentiseringsuppgifter som krävs för att komma åt den. Om det behövs kan du fråga dem enligt nedan.

Behållaren registret inloggningsserver (uppdatera ditt namn i registret):

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

Behållaren registret lösenord:

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

För att distribuera avbildningen behållare från behållaren registret med en resursbegäran av 1 processorkärna och 1GB minne, kör du följande kommando:

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

Inom några sekunder får du ett första svar från Azure Resource Manager. Om du vill visa status för distributionen, använder du:

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

Vi kan fortsätta att köra det här kommandot förrän tillståndet ändras från *väntande* till *kör*. Vi kan sedan fortsätta.

## <a name="view-the-application-and-container-logs"></a>Kontrollera händelseloggarna för programmet och en behållare

När distributionen lyckas, öppna webbläsaren till IP-adressen som visas i utdata från kommandot:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world-app i webbläsaren][aci-app-browser]

Du kan också visa loggutdata på behållaren:

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

Resultat:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a>Nästa steg

I kursen får slutfört du processen för att distribuera din behållare till Behållarinstanser som Azure. Följande steg har slutförts:

> [!div class="checklist"]
> * Distribuera behållare från Azure-behållare registret med hjälp av Azure CLI
> * Visar programmet i webbläsaren
> * Visa loggar för behållaren

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
