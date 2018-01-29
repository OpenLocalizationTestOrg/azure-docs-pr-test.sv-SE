---
title: "Distribuera flera behållare grupper i Azure Container instanser"
description: "Lär dig hur du distribuerar en behållare grupp med flera behållare i Azure Container instanser."
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/10/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 41a47adb1f1da417038757934f0a6cf7e11555da
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/10/2018
---
# <a name="deploy-a-container-group"></a>Distribuera en behållare grupp

Azure Behållarinstanser stöder distribution av flera behållare till en enda värd med en [behållargruppen](container-instances-container-groups.md). Detta är användbart när du skapar ett program sidovagn för loggning, övervakning eller någon annan konfiguration där en tjänst behöver en andra anslutna process.

Det här dokumentet vägleder dig genom att köra en enkel flera behållare sidovagn konfiguration genom att distribuera en Azure Resource Manager-mall.

> [!NOTE]
> Flera behållare grupper är för närvarande begränsad till Linux-behållare. När vi arbetar för att göra alla funktioner till Windows-behållare, hittar du den aktuella plattformen skillnader i [kvoter och regional tillgänglighet för Azure-Behållarinstanser](container-instances-quotas.md).

## <a name="configure-the-template"></a>Konfigurera mallen

Skapa en fil med namnet `azuredeploy.json` och kopierar följande JSON till den.

En offentlig IP-adress, och två exponerade har definierats i det här exemplet är en behållare grupp med två behållare. Första behållaren i gruppen körs ett mot internet-program. Behållaren andra sidvagn, gör en HTTP-begäran till webbprogrammet huvudsakliga via gruppens lokala nätverk.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
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
      "apiVersion": "2017-10-01-preview",
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
                },
                {
                  "port": 8080
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
            },
            {
                "protocol": "tcp",
                "port": "8080"
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

Om du vill använda ett privat behållaren image register att lägga till ett objekt i JSON-dokumentet med följande format.

```json
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>Distribuera mallen

Skapa en resursgrupp med kommandot [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Distribuera mallen med den [az distribution skapa] [ az-group-deployment-create] kommando.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --name myContainerGroup --template-file azuredeploy.json
```

Du bör få ett inledande svar från Azure inom några sekunder.

## <a name="view-deployment-state"></a>Visa status för distributionen

Du kan visa statusen för distributionen av [az behållaren visa] [ az-container-show] kommando. Detta returnerar etablerade offentliga IP-adressen som programmet kan användas.

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Resultat:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                           IP:ports               CPU/Memory       OsType    Location
----------------  ---------------  -------------------  --------------------------------------------------------------  ---------------------  ---------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     westus
```

## <a name="view-logs"></a>Visa loggfiler

Visa loggutdata från en behållare med hjälp av den [az behållaren loggar] [ az-container-logs] kommando. Den `--container-name` argumentet anger behållaren som hämtar loggarna. Den första behållaren har angetts i det här exemplet.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Resultat:

```bash
listening on port 80
::1 - - [09/Jan/2018:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Om du vill se loggar för behållaren sida bilen köra samma kommando anger andra behållarens namn.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Resultat:

```bash
Every 3s: curl -I http://localhost                          2018-01-09 23:25:11

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Tue, 09 Jan 2018 23:25:11 GMT
Connection: keep-alive
```

Som du kan se göra sidovagn regelbundet en HTTP-begäran till huvudsakliga webb-applikationen via gruppens lokala nätverk så att den körs. Det här exemplet sidovagn kan expanderas för att utlösa en avisering om det tog emot en HTTP-svarskoden än 200 OK.

## <a name="next-steps"></a>Nästa steg

Den här artikeln beskrivs de steg som krävs för att distribuera en instans av flera behållare Azure-behållaren. En slutpunkt till slutpunkt Azure Behållarinstanser upplevelse finns i Azure Container instanser kursen.

> [!div class="nextstepaction"]
> [Azure Behållarinstanser självstudiekursen][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[az-group-deployment-create]: /cli/azure/group/deployment#az_group_deployment_create
