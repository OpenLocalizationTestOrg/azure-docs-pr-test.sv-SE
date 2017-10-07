---
title: "aaaCreate din första Azure Behållarinstanser behållare | Azure-dokument"
description: "Distribuera och kom igång med Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b57c52714933bd3b28c44d33f9af7cd1f23fb951
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Skapa din första behållare i Azure Container Instances

Azure Behållarinstanser som gör det enkelt toocreate och hantera behållare i Azure. I den här snabbstartsguide du skapa en behållare i Azure och exponera den toohello internet med en offentlig IP-adress. Den här åtgärden utförs med ett enda kommando. Inom några sekunder visas det här i webbläsaren:

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.12 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure Container Instances är Azure-resurser och måste vara placerade i en Azure-resursgrupp, en logisk samling dit Azure-resurser distribueras och hanteras.

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Skapa en behållare

Du kan skapa en behållare genom att ange ett namn, en Docker-avbildning och en Azure-resursgrupp. Du kan eventuellt exponera hello behållaren toohello internet med en offentlig IP-adress. I så fall använder vi en behållare som är värd för en väldigt enkel webbapp skriven i [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

Du bör få ett svar tooyour begäran inom några sekunder. Inledningsvis hello behållare kommer att ha en **skapa** tillstånd, men det bör starta inom några sekunder. Du kan kontrollera statusen för hello med hello `show` kommando:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

Längst ned hello hello utdata visas etableringsstatusen hello-behållaren och dess IP-adress:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

När behållaren hello flyttar toohello **lyckades** tillstånd, kan du nå den i hello webbläsare med hello IP-adressen. 

![App som distribuerats via Azure Container Instances visas i webbläsare][aci-app-browser]

## <a name="pull-hello-container-logs"></a>Hämta hello behållaren loggar

Du kan dra hello loggar för hello-behållaren som du skapat med hello `logs` kommando:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Resultat:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-hello-container"></a>Ta bort hello behållaren

När du är klar med hello behållare, du kan ta bort den med hjälp av hello `delete` kommando:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

Alla hello code för hello-behållare som används i den här snabbstartsguide [på GitHub][app-github-repo], tillsammans med dess Dockerfile. Om du vill tootry skapar själv och distribuera den tooAzure Behållarinstanser som använder hello Azure Container registret fortsätta toohello Azure Behållarinstanser kursen.

> [!div class="nextstepaction"]
> [Azure Container Instances-självstudier](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png