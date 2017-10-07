---
title: "aaaAzure Behållarinstanser - flera behållargruppen | Azure-dokument"
description: "Azure Behållarinstanser - grupp med flera behållare"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 976f578cd2a9bf7f05ab97f24662139bb72062ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-group"></a>Distribuera en behållare grupp

Stöd för Azure Behållarinstanser hello distribution av flera behållare till en enda värd med en *behållargruppen*. Detta är användbart när du skapar ett program sidovagn för loggning, övervakning eller någon annan konfiguration där en tjänst behöver en andra anslutna process. 

Det här dokumentet går igenom kör en enkel flera behållare sidovagn konfiguration med en Azure Resource Manager-mall.

## <a name="configure-hello-template"></a>Konfigurera hello mall

Skapa en fil med namnet `azuredeploy.json` och kopiera hello följande json till den. 

I det här exemplet definieras en behållare grupp med två behållare och en offentlig IP-adress. hello första behållare för hello grupp körs ett Internetriktade Internetprogram. hello andra behållare, hello sidovagn gör ett HTTP-begäran toohello huvudsakliga webbprogram via hello gruppen lokala nätverk. 

Det här exemplet sidovagn kan vara expanderade tootrigger en avisering om det tog emot en HTTP-svarskoden än 200 OK. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",    
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
    "resources": [
      {
        "name": "myContainerGroup",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2017-08-01-preview",
        "location": "[resourceGroup().location]",
        "properties": {
          "containers": [
            {
              "name": "[variables('container1name')]",
              "properties": {
                "image": "[variables('container1image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                },
                "ports": [
                  {
                    "port": 80
                  }
                ]
              }
            },
            {
              "name": "[variables('container2name')]",
              "properties": {
                "image": "[variables('container2image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                }
              }
            }
          ],
          "osType": "Linux",
          "ipAddress": {
            "type": "Public",
            "ports": [
              {
                "protocol": "tcp",
                "port": "80"
              }
            ]
          }
        }
      }
    ],
    "outputs": {
      "containerIPv4Address": {
        "type": "string",
        "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
      }
    }
  }
```

toouse en bild registret privata behållare lägga till ett objekt toohello json-dokument med hello följande format.

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-hello-template"></a>Distribuera hello mall

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Distribuera hello mallen med hello [az distribution skapa](/cli/azure/group/deployment#create) kommando.

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

Inom några sekunder får du ett första svar från Azure. 

## <a name="view-deployment-state"></a>Visa status för distributionen

tooview hello tillstånd hello distribution, Använd hello `az container show` kommando. Detta returnerar hello etablerats offentliga IP-adressen via vilken hello program kan nås.

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

Resultat:

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Visa loggfiler   

Visa hello loggutdata för en behållare med hello `az container logs` kommando. Hej `--container-name` argumentet anger hello behållare från vilken toopull loggar. I det här exemplet har hello första behållare angetts. 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

Resultat:

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

toosee hello loggar för hello sida bil behållare, kör hello samma kommando anger hello andra behållarnamn.

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

Resultat:

```bash
Every 3.0s: curl -I http://localhost                                                                                                                       Mon Jul 17 11:27:36 2017

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 1663
Content-Type: text/html; charset=utf-8
Last-Modified: Sun, 16 Jul 2017 02:08:22 GMT
Date: Mon, 17 Jul 2017 18:27:36 GMT
```

Som du ser att hello sidovagn regelbundet en HTTP-begäran toohello huvudsakliga webbprogram via hello gruppen lokala nätverket tooensure körs.

## <a name="next-steps"></a>Nästa steg

Det här dokumentet beskrivs hello steg som krävs för att distribuera flera behållare instans av Azure-behållaren. En utgången tooend Azure Behållarinstanser upplevelse, finns i hello Azure Behållarinstanser kursen.

> [!div class="nextstepaction"]
> [Azure Behållarinstanser kursen]:./container-instances-tutorial-prepare-app.md
