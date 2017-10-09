---
title: "aaaAutomating programdistribution med virtuella Datortillägg | Microsoft Docs"
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 9fc8b1ba-60f5-410b-8190-9f1ff885e50e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 38a02a4271d6b9ba02a473a51794a7bd90ca3a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a>Programdistribution med Azure Resource Manager-mallar för virtuella Linux-datorer

När alla krav för Azure infrastrukturella har identifierats och översättas till en Distributionsmall, måste hello faktiska programdistribution toobe åtgärdas. Programdistribution här refererar tooinstalling hello faktiska binärfiler till Azure-resurser. Hello musik Store exempel, .net kärn, NGINX och handledarens måste toobe installeras och konfigureras på varje virtuell dator. hello musik Store binärfiler måste toobe installeras på hello virtuell dator och hello musik Store-databasen som skapats i förväg.

Det här dokumentet beskriver hur virtuella datortillägg kan automatisera program distribution och konfiguration av tooAzure virtuella datorer. Alla beroenden och unika konfigurationer är markerade. Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat. hello fullständig mallen hittar du här – [musik Store distributionen på Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Konfigurationsskript
Tillägg för virtuell dator är särskilda program som kör mot virtuella datorer tooprovide configuration automation. Tillägg är tillgängliga för många specifika ändamål, till exempel antivirusprogram, loggningskonfiguration och Docker-konfiguration. Tillägget för anpassat skript kan vara används toorun skript mot en virtuell dator. Med hello musik Store exempel fungerar toohello anpassat skript tillägget tooconfigure hello Ubuntu virtuella datorer och installera hello musik Store-programmet. 

Granska hello-skript som körs före med information om hur virtuella datortillägg deklareras i en Azure Resource Manager-mall. Det här skriptet konfigurerar hello Ubuntu virtuella toohost hello musik Store-programmet. När du kör hello skriptet installerar alla nödvändiga program, installera hello musik store-programmet från källkontrollen och Förbered hello-databas. 

toolearn mer information om värd för en .net Core program på Linux, se [publicera tooa Linux produktionsmiljö](https://docs.asp.net/en/latest/publishing/linuxproduction.html).

> Det här exemplet är i exempelsyfte.
> 
> 

```bash
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>Tillägget för VM-skript
VM-tillägg kan köras mot en virtuell dator vid byggning genom att inkludera hello tillägget resurs i hello Azure Resource Manager-mall. hello tillägg kan läggas till med hello Visual Studio Lägg till resurs guide eller genom att infoga giltig JSON i hello mallen. hello skripttillägg resursen är kapslad i hello resurs för virtuell dator. Detta kan ses i följande exempel hello.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [VM skripttillägg](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359). 

Observera i hello nedan JSON som hello skriptet lagras i GitHub. Det här skriptet kan också lagras i Azure Blob storage. Azure Resource Manager-mallar kan också hello skript för körning av strängen tooconstructed så att mallen parametrar värden kan användas som parametrar för körning av skript. I det här fallet data är tillgängliga när du distribuerar hello mallar och dessa värden kan sedan användas när skriptet hello.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Mer information om hur du använder tillägget för anpassat skript för hello finns [anpassade skriptfilnamnstillägg med Resource Manager-mallar](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="next-step"></a>Nästa steg
<hr>

[Utforska mer Azure Resource Manager-mallar](https://github.com/Azure/azure-quickstart-templates)

