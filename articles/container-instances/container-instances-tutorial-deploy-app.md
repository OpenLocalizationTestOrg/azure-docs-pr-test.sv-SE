---
title: "aaaAzure Behållarinstanser tutorial – distribuera appen | Microsoft Docs"
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
ms.openlocfilehash: b9fb098d9491e1073f0be4b14a0b9b1a18f16095
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-tooazure-container-instances"></a>Distribuera en behållare tooAzure Behållarinstanser

Detta är hello senaste i tre delar självstudiekursen. I föregående avsnitt [en behållare avbildningen skapades](container-instances-tutorial-prepare-app.md) och [pushas tooan Azure Container registret](container-instances-tutorial-prepare-acr.md). Det här avsnittet är klar hello kursen genom att distribuera hello behållaren tooAzure Behållarinstanser. Slutfört stegen innefattar:

> [!div class="checklist"]
> * Ange en behållare grupp med en Azure Resource Manager-mall
> * Distribuera hello behållargruppen med hello Azure CLI
> * Visa behållaren loggar

## <a name="deploy-hello-container-using-hello-azure-cli"></a>Distribuera hello behållare med hello Azure CLI

hello Azure CLI gör det möjligt för distribution av en behållare tooAzure Behållarinstanser i ett enda kommando. Eftersom hello behållaren bilden finns i Hej registret för privat Azure-behållare, måste du inkludera hello autentiseringsuppgifter krävs tooaccess den. Om det behövs kan du fråga dem enligt nedan.

Behållaren registret inloggningsserver (uppdatera ditt namn i registret):

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

Behållaren registret lösenord:

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

toodeploy behållaren avbildningen från hello behållaren registret med en resurs begär 1 processorkärna och 1GB minne, kör följande kommando hello:

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

Inom några sekunder får du ett första svar från Azure Resource Manager. tooview hello tillstånd för hello distribution, Använd:

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

Vi kan fortsätta att köra det här kommandot förrän hello tillstånd ändras från *väntande* för*kör*. Vi kan sedan fortsätta.

## <a name="view-hello-application-and-container-logs"></a>Visa hello program- och loggfiler

När hello distribution lyckas, öppna din webbläsare toohello IP-adress som visas i hello utdata från hello följande kommando:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world-app i hello webbläsare][aci-app-browser]

Du kan också visa hello loggutdata för hello behållare:

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

I kursen får slutfört du hello processen för att distribuera din behållare tooAzure Behållarinstanser. hello följande steg har slutförts:

> [!div class="checklist"]
> * Distribuera hello behållare från hello Azure Container registret med hjälp av hello Azure CLI
> * Visar hello program i hello webbläsare
> * Visa hello behållaren loggar

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
