---
title: "Azure Behållarinstanser tutorial – distribuera appen"
description: "Självstudiekurs för Azure Behållarinstanser del 3 av 3 – distribuera program"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 471caa1b24dc7017c70782c072b2068f9635244b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Distribuera en behållare till Behållarinstanser som Azure

Det här är sista kurs i en serie i tre delar. Tidigare i serien [en behållare avbildningen skapades](container-instances-tutorial-prepare-app.md) och [pushas till ett Azure Container registret](container-instances-tutorial-prepare-acr.md). Den här artikeln Slutför kursen serien genom att distribuera behållaren till Behållarinstanser som Azure.

I den här kursen har du:

> [!div class="checklist"]
> * Distribuera behållare från Azure-behållare registret med hjälp av Azure CLI
> * Visa program i webbläsaren
> * Visa loggar för behållaren

## <a name="before-you-begin"></a>Innan du börjar

Den här kursen kräver att du använder Azure CLI version 2.0.23 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera, se [installera Azure CLI 2.0][azure-cli-install].

Den här kursen behöver en Docker-utvecklingsmiljö installeras lokalt. Docker innehåller paket som enkelt kan konfigurera Docker på någon [Mac][docker-mac], [Windows][docker-windows], eller [Linux] [ docker-linux] system.

Azure Cloud Shell inkluderar inte Docker-komponenter som krävs för att slutföra varje steg den här kursen. På den lokala datorn för att slutföra den här guiden måste du installera Azure CLI och Docker-utvecklingsmiljö.

## <a name="deploy-the-container-using-the-azure-cli"></a>Distribuera behållare med hjälp av Azure CLI

Azure CLI gör det möjligt för distribution av en behållare till Azure-Behållarinstanser i ett enda kommando. Eftersom bilden behållaren finns i registret för privat Azure-behållare, måste du inkludera de autentiseringsuppgifter som krävs för att komma åt den. Hämta autentiseringsuppgifter med hjälp av följande Azure CLI-kommandona.

Behållaren registret inloggningsserver (uppdatera ditt namn i registret):

```azurecli
az acr show --name <acrName> --query loginServer
```

Behållaren registret lösenord:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Kör följande kommando för att distribuera avbildningen behållare från behållaren registret med en resursbegäran av 1 processorkärna och 1 GB minne. Ersätt `<acrLoginServer>` och `<acrPassword>` med värden som du fick från föregående två kommandon.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public --ports 80
```

Inom några sekunder bör du få ett inledande svar från Azure Resource Manager. Du kan visa statusen för distributionen [az behållaren visa][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Upprepa den [az behållaren visa] [ az-container-show] kommandot förrän tillståndet ändras från *väntande* till *kör*, vilka som ska ha under en minut. När behållaren är *kör*, Fortsätt till nästa steg.

## <a name="view-the-application-and-container-logs"></a>Kontrollera händelseloggarna för programmet och en behållare

När distributionen lyckas, visa behållarens offentliga IP-adressen med den [az behållaren visa] [ az-container-show] kommando:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.ip
```

Exempel på utdata:`"13.88.176.27"`

Navigera till den offentliga IP-adressen i webbläsaren om du vill se programmet som körs.

![Hello world-app i webbläsaren][aci-app-browser]

Du kan också visa loggutdata på behållaren:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Resultat:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Rensa resurser

Om du behöver inte längre någon av de resurser som du skapade i den här självstudiekursen serien, kan du köra den [ta bort grupp az] [ az-group-delete] för att ta bort resursgruppen och alla resurser som den innehåller. Det här kommandot tar bort registernyckeln behållare som du skapade samt körs behållaren och alla relaterade resurser.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

I kursen får slutfört du processen för att distribuera din behållare till Behållarinstanser som Azure. Följande steg har slutförts:

> [!div class="checklist"]
> * Distribuera behållare från Azure-behållare registret med hjälp av Azure CLI
> * Programmet visas i webbläsaren
> * Visa loggar för behållaren

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md