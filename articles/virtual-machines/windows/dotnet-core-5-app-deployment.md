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
# <a name="application-deployment-with-azure-resource-manager-templates-for-windows-vms"></a>Programdistribution med Azure Resource Manager-mallar för virtuella Windows-datorer

När alla krav för Azure infrastrukturella har identifierats och översättas till en Distributionsmall, måste hello faktiska programdistribution toobe åtgärdas. Programdistribution här refererar tooinstalling hello faktiska binärfiler till Azure-resurser. Hello musik Store exempel, .net Core och IIS behöver toobe installeras och konfigureras på varje virtuell dator. hello musik Store binärfiler måste toobe installeras på hello virtuell dator och hello musik Store-databasen som skapats i förväg.

Det här dokumentet beskriver hur virtuella datortillägg kan automatisera program distribution och konfiguration av tooAzure virtuella datorer. Alla beroenden och unika konfigurationer är markerade. Distribuera en instans av hello lösning tooyour Azure-prenumeration och fungerar tillsammans med hello Azure Resource Manager-mall för hello bästa möjliga resultat. hello fullständig mallen hittar du här – [musik Store distributionen på Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="configuration-script"></a>Konfigurationsskript
Tillägg för virtuell dator är särskilda program som kör mot virtuella datorer tooprovide configuration automation. Tillägg är tillgängliga för många specifika ändamål, till exempel antivirusprogram, loggningskonfiguration och Docker-konfiguration. hello tillägget för anpassat skript kan vara används toorun skript mot en virtuell dator. Med hello musik Store exempel fungerar toohello anpassat skript tillägget tooconfigure hello virtuella Windows-datorer och installera hello musik Store-programmet.

Granska hello-skript som körs före med information om hur virtuella datortillägg deklareras i en Azure Resource Manager-mall. Det här skriptet konfigurerar hello Windows virtuella toohost hello musik Store-programmet. När du kör hello skriptet installerar alla nödvändiga program, installera hello musik store-programmet från källkontrollen och Förbered hello-databas. 

> Det här exemplet är i exempelsyfte.

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

## <a name="vm-script-extension"></a>Tillägget för VM-skript
VM-tillägg kan köras mot en virtuell dator vid byggning genom att inkludera hello tillägget resurs i hello Azure Resource Manager-mall. hello tillägg kan läggas till med hello Visual Studio Lägg till resurs guide eller genom att infoga giltig JSON i hello mallen. hello skripttillägg resursen är kapslad i hello resurs för virtuell dator. Detta kan ses i följande exempel hello.

Följ den här länken toosee hello JSON-exemplet i hello Resource Manager-mallen – [VM skripttillägg](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Observera i hello nedan JSON som hello skriptet lagras i GitHub. Det här skriptet kan också lagras i Azure Blob storage. Azure Resource Manager-mallar kan också hello skript körning sträng toobe konstruerat så att mallen parametrar värden kan användas som parametrar för körning av skript. I det här fallet data är tillgängliga när du distribuerar hello mallar och dessa värden kan sedan användas när skriptet hello.

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

Som nämnts ovan är det också möjligt toostore dina anpassade skript i Azure Blob storage. Det finns två alternativ för att lagra hello skriptresurser i blob storage; antingen se hello offentliga behållare eller skript och följ hello samma närmar sig som ovan, eller också kan lagras i privata blob storage som kräver tooprovide hello storageAccountName och storageAccountKey toohello CustomScriptExtension resursdefinitionen.

I hello exemplet nedan har vi gått ett steg ytterligare. Det är möjligt tooprovide hello lagringskontonamn och nyckel som en parameter eller variabel under distributionen, mallar för Resource Manager tillhandahåller hello `listKeys` funktion som kan få hello lagringskonto nyckeln programmässigt och infoga den i toohello mallen du vid tidpunkten för distribution.

I hello exempel CustomScriptExtension resursdefinitionen nedan våra anpassat skript redan har laddats upp tooan Azure storage-konto med namnet `mystorageaccount9999` som finns i en annan resursgrupp med namnet `mysa999rgname`. När vi distribuera en mall som innehåller den här resursen hello `listKeys` funktionen hämtar programmässigt hello lagringskontonyckel för hello lagringskontot `mystorageaccount9999` i hello resursgruppen `mysa999rgname` och infogas i toohello mall för oss.

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

hello största fördelen med den här metoden är att inte kräver att du toochange din mall eller distributionsparametrarna i hello-händelse hello lagring konto viktiga ändra.

Mer information om hur du använder tillägget för anpassat skript för hello finns [anpassade skriptfilnamnstillägg med Resource Manager-mallar](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-step"></a>Nästa steg
<hr>

[Utforska mer Azure Resource Manager-mallar](https://github.com/Azure/azure-quickstart-templates)

