---
title: "aaaAutomating programdistribution med virtuella Datortillägg | Microsoft Docs"
description: "Virtuella Azure-datorn DotNet grundläggande självstudierna"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 79c91304-6c1b-4354-a185-fecc129b139d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d52537fbd4e935f19d3864def11484f519f8598
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-deployment-with-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="0cbe8-103">Programdistribution med Azure Resource Manager-mallar för virtuella Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="0cbe8-103">Application deployment with Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="0cbe8-104">När alla krav för Azure infrastrukturella har identifierats och översättas till en Distributionsmall, måste hello faktiska programdistribution toobe åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-104">Once all Azure infrastructural requirements have been identified and translated into a deployment template, hello actual application deployment needs toobe addressed.</span></span> <span data-ttu-id="0cbe8-105">Programdistribution här refererar tooinstalling hello faktiska binärfiler till Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-105">Application deployment here is referring tooinstalling hello actual application binaries onto Azure resources.</span></span> <span data-ttu-id="0cbe8-106">Hello musik Store exempel, .net Core och IIS behöver toobe installeras och konfigureras på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-106">For hello Music Store sample, .Net Core and IIS need toobe installed and configured on each virtual machine.</span></span> <span data-ttu-id="0cbe8-107">hello musik Store binärfiler måste toobe installeras på hello virtuell dator och hello musik Store-databasen som skapats i förväg.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-107">hello Music Store binaries need toobe installed onto hello virtual machine, and hello Music Store database pre-created.</span></span>

<span data-ttu-id="0cbe8-108">Det här dokumentet beskriver hur virtuella datortillägg kan automatisera program distribution och konfiguration av tooAzure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-108">This document details how Virtual Machine extensions can automate application deployment and configuration tooAzure virtual machines.</span></span> <span data-ttu-id="0cbe8-109">Alla beroenden och unika konfigurationer är markerade.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="0cbe8-110">Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-110">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="0cbe8-111">hello fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="0cbe8-111">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="configuration-script"></a><span data-ttu-id="0cbe8-112">Konfigurationsskript</span><span class="sxs-lookup"><span data-stu-id="0cbe8-112">Configuration script</span></span>
<span data-ttu-id="0cbe8-113">Tillägg för virtuell dator är särskilda program som kör mot virtuella datorer tooprovide configuration automation.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-113">Virtual Machine extensions are specialized programs that execute against virtual machines tooprovide configuration automation.</span></span> <span data-ttu-id="0cbe8-114">Tillägg är tillgängliga för många specifika ändamål, till exempel antivirusprogram, loggningskonfiguration och Docker-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-114">Extensions are available for many specific purposes such as anti-virus, logging configuration, and Docker configuration.</span></span> <span data-ttu-id="0cbe8-115">hello tillägget för anpassat skript kan vara används toorun skript mot en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-115">hello Custom Script extension can be used toorun any script against a virtual machine.</span></span> <span data-ttu-id="0cbe8-116">Med hello musik Store exempel fungerar toohello anpassat skript tillägget tooconfigure hello virtuella Windows-datorer och installera hello musik Store-programmet.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-116">With hello Music Store sample, it is up toohello custom script extension tooconfigure hello Windows virtual machines and install hello Music Store application.</span></span>

<span data-ttu-id="0cbe8-117">Granska hello-skript som körs före med information om hur virtuella datortillägg deklareras i en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-117">Before detailing how virtual machine extensions are declared in an Azure Resource Manager template, examine hello script that is run.</span></span> <span data-ttu-id="0cbe8-118">Det här skriptet konfigurerar hello Windows virtuella toohost hello musik Store-programmet.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-118">This script configures hello Windows virtual machine toohost hello Music Store application.</span></span> <span data-ttu-id="0cbe8-119">När du kör hello skriptet installerar alla nödvändiga program, installera hello musik store-programmet från källkontrollen och Förbered hello-databas.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-119">When run, hello script installs all needed software, install hello Music store application from source control, and prepare hello database.</span></span> 

> <span data-ttu-id="0cbe8-120">Det här exemplet är i exempelsyfte.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-120">This sample is for demonstration purposes.</span></span>

```PowerShell
<#
    .SYNOPSIS
        Downloads and configures .Net Core Music Store application sample across IIS and Azure SQL DB.
#>

Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# Firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# Folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# Install iis
Install-WindowsFeature web-server -IncludeManagementTools

# Install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# Download music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music

# Azure SQL connection sting in environment variable
[Environment]::SetEnvironmentVariable("Data:DefaultConnection:ConnectionString", "Server=$sqlserver;Database=MusicStore;Integrated Security=False;User Id=$user;Password=$password;MultipleActiveResultSets=True;Connect Timeout=30",[EnvironmentVariableTarget]::Machine)

# Pre-create database
$env:Data:DefaultConnection:ConnectionString = "Server=$sqlserver;Database=MusicStore;Integrated Security=False;User Id=$user;Password=$password;MultipleActiveResultSets=True;Connect Timeout=30"
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# Configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a><span data-ttu-id="0cbe8-121">Tillägget för VM-skript</span><span class="sxs-lookup"><span data-stu-id="0cbe8-121">VM Script Extension</span></span>
<span data-ttu-id="0cbe8-122">VM-tillägg kan köras mot en virtuell dator vid byggning genom att inkludera hello tillägget resurs i hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-122">VM Extensions can be run against a virtual machine at build time by including hello extension resource in hello Azure Resource Manager template.</span></span> <span data-ttu-id="0cbe8-123">hello tillägg kan läggas till med hello Visual Studio Lägg till resurs guide eller genom att infoga giltig JSON i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-123">hello extension can be added with hello Visual Studio Add Resource wizard, or by inserting valid JSON into hello template.</span></span> <span data-ttu-id="0cbe8-124">hello skripttillägg resursen är kapslad i hello resurs för virtuell dator. Detta kan ses i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-124">hello Script Extension resource is nested inside hello Virtual Machine resource; this can be seen in hello following example.</span></span>

<span data-ttu-id="0cbe8-125">Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [VM skripttillägg](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339).</span><span class="sxs-lookup"><span data-stu-id="0cbe8-125">Follow this link toosee hello JSON sample within hello Resource Manager template – [VM Script Extension](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339).</span></span> 

<span data-ttu-id="0cbe8-126">Observera i hello nedan JSON som hello skriptet lagras i GitHub.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-126">Notice in hello below JSON that hello script is stored in GitHub.</span></span> <span data-ttu-id="0cbe8-127">Det här skriptet kan också lagras i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-127">This script could also be stored in Azure Blob storage.</span></span> <span data-ttu-id="0cbe8-128">Azure Resource Manager-mallar kan också hello skript körning sträng toobe konstruerat så att mallen parametrar värden kan användas som parametrar för körning av skript.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-128">Also, Azure Resource Manager templates allow hello script execution string toobe constructed such that template parameters values can be used as parameters for script execution.</span></span> <span data-ttu-id="0cbe8-129">I det här fallet data är tillgängliga när du distribuerar hello mallar och dessa värden kan sedan användas när skriptet hello.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-129">In this case data is provided when deploying hello templates, and these values can then be used when executing hello script.</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

<span data-ttu-id="0cbe8-130">Som nämnts ovan är det också möjligt toostore dina anpassade skript i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-130">As mentioned above, it is also possible toostore your custom scripts in Azure Blob storage.</span></span> <span data-ttu-id="0cbe8-131">Det finns två alternativ för att lagra hello skriptresurser i blob storage; antingen se hello offentliga behållare eller skript och följ hello samma närmar sig som ovan, eller också kan lagras i privata blob storage som kräver tooprovide hello storageAccountName och storageAccountKey toohello CustomScriptExtension resursdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-131">There are two options for storing hello script resources in blob storage; either make hello container/script public and follow hello same approach as above, or it can also be kept in private blob storage which requires you tooprovide hello storageAccountName and storageAccountKey toohello CustomScriptExtension resource definition.</span></span>

<span data-ttu-id="0cbe8-132">I hello exemplet nedan har vi gått ett steg ytterligare.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-132">In hello example below we have gone a step further.</span></span> <span data-ttu-id="0cbe8-133">Det är möjligt tooprovide hello lagringskontonamn och nyckel som en parameter eller variabel under distributionen, mallar för Resource Manager tillhandahåller hello `listKeys` funktion som kan få hello lagringskonto nyckeln programmässigt och infoga den i toohello mallen du vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-133">While it is possible tooprovide hello storage account name and key as a parameter or variable during deployment, Resource Manager templates provide hello `listKeys` function that can obtain hello storage account key programmatically and insert it in toohello template for you at deployment time.</span></span>

<span data-ttu-id="0cbe8-134">I hello exempel CustomScriptExtension resursdefinitionen nedan våra anpassat skript redan har laddats upp tooan Azure storage-konto med namnet `mystorageaccount9999` som finns i en annan resursgrupp med namnet `mysa999rgname`.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-134">In hello example CustomScriptExtension resource definition below, our custom script has already been uploaded tooan Azure storage account called `mystorageaccount9999` which exists in another Resource Group called `mysa999rgname`.</span></span> <span data-ttu-id="0cbe8-135">När vi distribuera en mall som innehåller den här resursen hello `listKeys` funktionen hämtar programmässigt hello lagringskontonyckel för hello lagringskontot `mystorageaccount9999` i hello resursgruppen `mysa999rgname` och infogas i toohello mall för oss.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-135">When we deploy a template containing this resource, hello `listKeys` function programmatically obtains hello storage account key for hello storage account `mystorageaccount9999` in hello Resource Group `mysa999rgname` and inserts it in toohello template for us.</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://mystorageaccount9999.blob.core.windows.net/container/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]",
      "storageAccountName": "mystorageaccount9999",
      "storageAccountKey": "[listKeys(resourceId('mysa999rgname','Microsoft.Storage/storageAccounts', mystorageaccount9999), '2015-06-15').key1]"
    }
  }
}
```

<span data-ttu-id="0cbe8-136">hello största fördelen med den här metoden är att inte kräver att du toochange din mall eller distributionsparametrarna i hello-händelse hello lagring konto viktiga ändra.</span><span class="sxs-lookup"><span data-stu-id="0cbe8-136">hello main benefit of this approach is that it does not require you toochange your template or deployment parameters in hello event of hello storage account key changing.</span></span>

<span data-ttu-id="0cbe8-137">Mer information om hur du använder tillägget för anpassat skript för hello finns [anpassade skriptfilnamnstillägg med Resource Manager-mallar](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0cbe8-137">For more information on using hello custom script extension, see [Custom script extensions with Resource Manager templates](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-step"></a><span data-ttu-id="0cbe8-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0cbe8-138">Next Step</span></span>
<hr>

[<span data-ttu-id="0cbe8-139">Utforska mer Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="0cbe8-139">Explore More Azure Resource Manager Templates</span></span>](https://github.com/Azure/azure-quickstart-templates)

