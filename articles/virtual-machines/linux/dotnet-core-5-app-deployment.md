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
# <a name="application-deployment-with-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="5fdf4-103">Programdistribution med Azure Resource Manager-mallar för virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="5fdf4-103">Application deployment with Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="5fdf4-104">När alla krav för Azure infrastrukturella har identifierats och översättas till en Distributionsmall, måste hello faktiska programdistribution toobe åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-104">Once all Azure infrastructural requirements have been identified and translated into a deployment template, hello actual application deployment needs toobe addressed.</span></span> <span data-ttu-id="5fdf4-105">Programdistribution här refererar tooinstalling hello faktiska binärfiler till Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-105">Application deployment here is referring tooinstalling hello actual application binaries onto Azure resources.</span></span> <span data-ttu-id="5fdf4-106">Hello musik Store exempel, .net kärn, NGINX och handledarens måste toobe installeras och konfigureras på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-106">For hello Music Store sample, .Net Core, NGINX, and Supervisor need toobe installed and configured on each virtual machine.</span></span> <span data-ttu-id="5fdf4-107">hello musik Store binärfiler måste toobe installeras på hello virtuell dator och hello musik Store-databasen som skapats i förväg.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-107">hello Music Store binaries need toobe installed onto hello virtual machine, and hello Music Store database pre-created.</span></span>

<span data-ttu-id="5fdf4-108">Det här dokumentet beskriver hur virtuella datortillägg kan automatisera program distribution och konfiguration av tooAzure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-108">This document details how Virtual Machine extensions can automate application deployment and configuration tooAzure virtual machines.</span></span> <span data-ttu-id="5fdf4-109">Alla beroenden och unika konfigurationer är markerade.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="5fdf4-110">Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-110">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="5fdf4-111">hello fullständig mallen hittar du här – [musik Store distributionen på Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="5fdf4-111">hello complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

## <a name="configuration-script"></a><span data-ttu-id="5fdf4-112">Konfigurationsskript</span><span class="sxs-lookup"><span data-stu-id="5fdf4-112">Configuration script</span></span>
<span data-ttu-id="5fdf4-113">Tillägg för virtuell dator är särskilda program som kör mot virtuella datorer tooprovide configuration automation.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-113">Virtual Machine extensions are specialized programs that execute against virtual machines tooprovide configuration automation.</span></span> <span data-ttu-id="5fdf4-114">Tillägg är tillgängliga för många specifika ändamål, till exempel antivirusprogram, loggningskonfiguration och Docker-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-114">Extensions are available for many specific purposes such as anti-virus, logging configuration, and Docker configuration.</span></span> <span data-ttu-id="5fdf4-115">Tillägget för anpassat skript kan vara används toorun skript mot en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-115">A custom script extension can be used toorun any script against a virtual machine.</span></span> <span data-ttu-id="5fdf4-116">Med hello musik Store exempel fungerar toohello anpassat skript tillägget tooconfigure hello Ubuntu virtuella datorer och installera hello musik Store-programmet.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-116">With hello Music Store sample, it is up toohello custom script extension tooconfigure hello Ubuntu virtual machines and install hello Music Store application.</span></span> 

<span data-ttu-id="5fdf4-117">Granska hello-skript som körs före med information om hur virtuella datortillägg deklareras i en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-117">Before detailing how virtual machine extensions are declared in an Azure Resource Manager template, examine hello script that is run.</span></span> <span data-ttu-id="5fdf4-118">Det här skriptet konfigurerar hello Ubuntu virtuella toohost hello musik Store-programmet.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-118">This script configures hello Ubuntu virtual machine toohost hello Music Store application.</span></span> <span data-ttu-id="5fdf4-119">När du kör hello skriptet installerar alla nödvändiga program, installera hello musik store-programmet från källkontrollen och Förbered hello-databas.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-119">When run, hello script installs all needed software, install hello Music store application from source control, and prepare hello database.</span></span> 

<span data-ttu-id="5fdf4-120">toolearn mer information om värd för en .net Core program på Linux, se [publicera tooa Linux produktionsmiljö](https://docs.asp.net/en/latest/publishing/linuxproduction.html).</span><span class="sxs-lookup"><span data-stu-id="5fdf4-120">toolearn more about hosting a .Net Core application on Linux, see [Publish tooa Linux production environment](https://docs.asp.net/en/latest/publishing/linuxproduction.html).</span></span>

> <span data-ttu-id="5fdf4-121">Det här exemplet är i exempelsyfte.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-121">This sample is for demonstration purposes.</span></span>
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

## <a name="vm-script-extension"></a><span data-ttu-id="5fdf4-122">Tillägget för VM-skript</span><span class="sxs-lookup"><span data-stu-id="5fdf4-122">VM Script Extension</span></span>
<span data-ttu-id="5fdf4-123">VM-tillägg kan köras mot en virtuell dator vid byggning genom att inkludera hello tillägget resurs i hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-123">VM Extensions can be run against a virtual machine at build time by including hello extension resource in hello Azure Resource Manager template.</span></span> <span data-ttu-id="5fdf4-124">hello tillägg kan läggas till med hello Visual Studio Lägg till resurs guide eller genom att infoga giltig JSON i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-124">hello extension can be added with hello Visual Studio Add Resource wizard, or by inserting valid JSON into hello template.</span></span> <span data-ttu-id="5fdf4-125">hello skripttillägg resursen är kapslad i hello resurs för virtuell dator. Detta kan ses i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-125">hello Script Extension resource is nested inside hello Virtual Machine resource; this can be seen in hello following example.</span></span>

<span data-ttu-id="5fdf4-126">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [VM skripttillägg](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).</span><span class="sxs-lookup"><span data-stu-id="5fdf4-126">Follow this link toosee hello JSON sample within hello Resource Manager template – [VM Script Extension](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359).</span></span> 

<span data-ttu-id="5fdf4-127">Observera i hello nedan JSON som hello skriptet lagras i GitHub.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-127">Notice in hello below JSON that hello script is stored in GitHub.</span></span> <span data-ttu-id="5fdf4-128">Det här skriptet kan också lagras i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-128">This script could also be stored in Azure Blob storage.</span></span> <span data-ttu-id="5fdf4-129">Azure Resource Manager-mallar kan också hello skript för körning av strängen tooconstructed så att mallen parametrar värden kan användas som parametrar för körning av skript.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-129">Also, Azure Resource Manager templates allow hello script execution string tooconstructed such that template parameters values can be used as parameters for script execution.</span></span> <span data-ttu-id="5fdf4-130">I det här fallet data är tillgängliga när du distribuerar hello mallar och dessa värden kan sedan användas när skriptet hello.</span><span class="sxs-lookup"><span data-stu-id="5fdf4-130">In this case data is provided when deploying hello templates, and these values can then be used when executing hello script.</span></span>

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

<span data-ttu-id="5fdf4-131">Mer information om hur du använder tillägget för anpassat skript för hello finns [anpassade skriptfilnamnstillägg med Resource Manager-mallar](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5fdf4-131">For more information on using hello custom script extension, see [Custom script extensions with Resource Manager templates](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="next-step"></a><span data-ttu-id="5fdf4-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5fdf4-132">Next Step</span></span>
<hr>

[<span data-ttu-id="5fdf4-133">Utforska mer Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="5fdf4-133">Explore More Azure Resource Manager Templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

