---
title: "Snabbstart – Skapa din första Azure Container Instances-behållare | Azure Docs"
description: "Distribuera och kom igång med Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4c7f48c993d66dd79538fd73ccaed1355c2e8cdd
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/24/2018
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Skapa din första behållare i Azure Container Instances
Azure Container Instances gör det enkelt att skapa och hantera Docker-behållare i Azure, utan att behöva etablera virtuella datorer eller gå upp till en högre tjänstnivå. I den här snabbstarten skapar du en behållare i Azure och gör den tillgänglig på Internet med en offentlig IP-adress. Den här åtgärden utförs med ett enda kommando. Inom några sekunder visas det här i webbläsaren:

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt][azure-account] konto innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Du kan använda Azure Cloud Shell eller en lokal installation av Azure CLI för att genomföra den här snabbstarten. Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0.21 eller senare under den här snabbstarten. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0][azure-cli-install].

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure Container Instances är Azure-resurser och måste vara placerade i en Azure-resursgrupp, en logisk samling dit Azure-resurser distribueras och hanteras.

Skapa en resursgrupp med kommandot [az group create][az-group-create].

I följande exempel skapas en resursgrupp med namnet *myResourceGroup* på platsen *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Skapa en behållare

Du kan skapa en behållare genom att ange ett namn, en dockeravbildning och en Azure-resursgrupp med kommandot [az container create][az-container-create]. Du kan också göra behållaren tillgänglig på Internet med en offentlig IP-adress. I den här snabbstarten distribuerar du en behållare som är värd för en liten webbapp som skrivits i [Node.js][node-js].

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --ip-address public --ports 80
```

Om några sekunder bör du få ett svar på din begäran. Först har behållaren statusen **Creating** (skapas) men den bör starta inom några sekunder. Du kan kontrollera statusen med kommandot [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

Längst ned i resultatet visas behållarens etableringsstatus och IP-adress:

```json
...
"ipAddress": {
      "ip": "13.88.176.27",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "location:": "eastus",
    "name": "mycontainer",
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

När behållaren övergår i status **Succeeded** (lyckades) kan du nå den via webbläsaren med den angivna IP-adressen.

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

## <a name="pull-the-container-logs"></a>Hämta behållarloggarna

Du kan hämta loggar för den behållare du skapade med hjälp av kommandot [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Resultat:

```bash
listening on port 80
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://52.224.178.107/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
```

## <a name="delete-the-container"></a>Ta bort behållaren

När du är klar med behållaren kan du ta bort den med kommandot [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Kontrollera att behållaren har tagits bort genom att köra kommandot [az container list](/cli/azure/container#az_container_list):

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

Behållaren **MinBehållare** ska inte visas i kommandots utdata. Om du inte har några andra behållare i resursgruppen visas inga utdata.

## <a name="next-steps"></a>Nästa steg

All kod för behållaren som används i den här snabbstartsguiden finns [på GitHub][app-github-repo] tillsammans med dess Dockerfile. Om du vill försöka skapa den på egen hand och distribuera den till Azure Container Instances via Azure Container Registry, går du vidare till självstudierna för Azure Container Instances.

> [!div class="nextstepaction"]
> [Azure Container Instances-självstudier](./container-instances-tutorial-prepare-app.md)

Om du vill testa alternativ för att köra behållare i ett orkestreringssystem på Azure kan du gå vidare till snabbstarterna om [Service Fabric][service-fabric] eller [Azure Container Service (AKS)][container-service].

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
