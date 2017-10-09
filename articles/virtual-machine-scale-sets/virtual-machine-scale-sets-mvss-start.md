---
title: aaaLearn om den virtuella datorn ange mallar | Microsoft Docs
description: "Läs toocreate en minsta lönsam skala ange mall för virtuella datorer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: b7a1cf6c03b22585e16db9c071d45795c8ae75df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>Lär dig mer om scale set-mallar för virtuella datorer
[Azure Resource Manager-mallar](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) är ett bra sätt toodeploy grupper av relaterade resurser. Den här självstudiekursen serien visar hur toocreate en minsta lönsam skala ange mall och toomodify den här mallen toosuit olika scenarier. Alla exempel kommer från den här [GitHub-lagringsplatsen](https://github.com/gatneil/mvss). 

Den här mallen är avsedda toobe enkel. Mer komplett exempel på skalan mallar finns i avsnittet hello [Azure Quickstart mallar GitHub-lagringsplatsen](https://github.com/Azure/azure-quickstart-templates) och Sök efter mappar som innehåller hello sträng `vmss`.

Om du redan är bekant med att skapa mallar du kan hoppa över toohello ”nästa steg” avsnittet toosee hur toomodify den här mallen.

## <a name="review-hello-template"></a>Granska hello mall

Använda GitHub tooreview våra minsta lönsam skaluppsättning mallen [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).

I den här självstudiekursen kommer vi undersöka hello diff (`git diff master minimum-viable-scale-set`) toocreate hello lägsta lönsam skala ange mall bit för bit.

## <a name="define-schema-and-contentversion"></a>Definiera $schema och contentVersion
Först måste vi definiera `$schema` och `contentVersion` i hello mallen. Hej `$schema` elementet definierar hello-versionen av hello mallen språket och används för syntaxmarkering för Visual Studio och liknande funktioner för verifiering. Hej `contentVersion` elementet används inte av Azure. I stället hjälper dig att hålla reda på hello versionen av principmallen.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a>Definiera parametrar
Nu ska vi definierar två parametrar `adminUsername` och `adminPassword`. Parametrarna är värden som du anger vid hello tiden för distributionen. Hej `adminUsername` parametern är helt enkelt en `string` typ, men eftersom `adminPassword` är en hemlighet vi ge den typen `securestring`. Dessa parametrar skickas senare i hello scale set-konfiguration.

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a>Definiera variabler
Resource Manager-mallar kan du definiera variabler toobe används senare i hello mallen. Vårt exempel används inte några variabler, så vi har tomt hello JSON-objekt.

```json
  "variables": {},
```

## <a name="define-resources"></a>Definiera resurser
Nästa är hello resurser avsnitt i hello mall. Här kan du definiera vad som faktiskt ska toodeploy. Till skillnad från `parameters` och `variables` (som är JSON-objekt), `resources` är en JSON-lista över JSON-objekt.

```json
   "resources": [
```

Alla resurser som kräver `type`, `name`, `apiVersion`, och `location` egenskaper. Det här exemplet första resursen har typen `Microsft.Network/virtualNetwork`och namnet `myVnet`, och apiVersion `2016-03-30`. (toofind hello senaste API-versionen för en resurstyp finns hello [Azure REST API-dokumentation](https://docs.microsoft.com/rest/api/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a>Ange plats
toospecify hello plats för hello virtuella nätverket, använder vi en [Resource Manager mallfunktionen](../azure-resource-manager/resource-group-template-functions.md). Den här funktionen måste stå inom citattecken och hakparenteser så här: `"[<template-function>]"`. I detta fall kan vi använda hello `resourceGroup` funktion. Det tar i inga argument och returnerar ett JSON-objekt med metadata om hello resursgruppen distributionen distribueras till. hello resursgruppen har angetts av hello användare vid hello tiden för distributionen. Vi sedan index i den här JSON-objekt med `.location` tooget hello plats från hello JSON-objekt.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Ange egenskaper för virtuellt nätverk
Varje Resource Manager-resurs har sin egen `properties` avsnittet för konfigurationer specifika toohello resurs. I det här fallet anger vi som hello det virtuella nätverket ska ha ett undernät med hello privat IP-adressintervall `10.0.0.0/16`. En skaluppsättning för är alltid finns i ett undernät. Det går inte att sträcka sig över undernät.

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a>Lägg till dependsOn-lista
Dessutom krävs toohello `type`, `name`, `apiVersion`, och `location` egenskaper för varje resurs kan ha en valfri `dependsOn` lista med strängar. Den här listan anger vilken andra resurser från den här distributionen måste avslutas innan du distribuerar den här resursen.

I det här fallet finns bara ett element i listan hello hello virtuella nätverket från hello föregående exempel. Vi kan ange detta beroende eftersom hello skaluppsättning behov hello nätverket tooexist innan du skapar virtuella datorer. Det här sättet hello skaluppsättning kan ge dessa privata IP-adresser för virtuella datorer från hello IP-adressintervall som tidigare angetts i hello-egenskaper. varje sträng i hello dependsOn listan hello format är `<type>/<name>`. Använd hello samma `type` och `name` använt tidigare i resursdefinitionen i hello virtuellt nätverk.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Ange egenskaper för skalan
Skaluppsättningar har många egenskaper för att anpassa hello virtuella datorer i hello skaluppsättning. En fullständig lista över de här egenskaperna finns hello [skaluppsättning REST API-dokumentation](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set). För den här självstudiekursen kommer vi ange några vanliga egenskaper.
### <a name="supply-vm-size-and-capacity"></a>Ange VM-storlek och kapacitet
Hej skaluppsättning behov tooknow vilka storleken på VM-toocreate (”sku namn”) och hur många sådana VMs toocreate (”artikelnummerkapaciteten”). toosee vilka VM-storlekar är tillgängliga, se hello [storlekar på VM-dokumentationen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Välj typ av uppdateringar
Hej skaluppsättning måste också tooknow hur toohandle uppdaterar på hello skaluppsättning. Det finns två alternativ `Manual` och `Automatic`. Mer information om hello skillnaderna mellan hello två dokumentationen hello på [hur tooupgrade en skaluppsättning](./virtual-machine-scale-sets-upgrade-scale-set.md).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>Välj operativsystem för VM
Hej skaluppsättning behov tooknow vilka operativsystem tooput på hello virtuella datorer. Här kan skapa vi hello virtuella datorer med ett fullständigt korrigeringsfil Ubuntu 16.04 LTS avbildning.

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a>Ange computerNamePrefix
Hej skaluppsättning distribuerar flera virtuella datorer. I stället för att ange namn på varje virtuell dator kan vi ange `computerNamePrefix`. hello skaluppsättning lägger till ett index toohello prefix för varje virtuell dator så att VM-namn har hello formuläret `<computerNamePrefix>_<auto-generated-index>`.

I följande fragment hello, använda vi hello parametrar från innan tooset Hej administratörsanvändarnamn och lösenord för alla virtuella datorer i hello skaluppsättning. Vi kan göra detta med hello `parameters` mallfunktionen. Den här funktionen använder en sträng som anger vilka parametern toorefer tooand matar ut hello värde för parametern.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>Ange konfiguration för VM-nätverk
Slutligen måste toospecify hello nätverkskonfigurationen för hello virtuella datorer i hello skaluppsättning. I det här fallet behöver vi bara toospecify hello-ID för hello undernät som skapats tidigare. Detta visar hello skaluppsättning tooput hello nätverksgränssnitt i det här undernätet.

Du kan hämta hello ID av hello virtuella nätverk som innehåller hello undernät med hello `resourceId` mallfunktionen. Den här funktionen använder hello typ och namn på en resurs och returnerar hello fullständigt kvalificerade identifieraren för den här resursen. Detta ID har hello formuläret:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Dock är hello identifierare för hello virtuellt nätverk inte tillräckligt. Du måste ange hello specifika undernät som hello skaluppsättning för virtuella datorer måste vara i. toodo, sammanfoga `/subnets/mySubnet` toohello ID för hello virtuellt nätverk. hello resultatet är hello fullständigt kvalificerade ID hello undernät. Gör den här sammanfogning med hello `concat` funktion, vilket tar i en serie med strängar och returnerar sina sammanfogning.

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
