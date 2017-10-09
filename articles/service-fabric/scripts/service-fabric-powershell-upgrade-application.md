---
title: aaaAzure PowerShell skriptexempel - uppgraderar ett Service Fabric-program | Microsoft Docs
description: Azure PowerShell skriptexempel - uppgradering ett Service Fabric-program.
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 4f4777607bd6b35a76029e09ddb441006565d4cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-service-fabric-application"></a>Uppgradera ett Service Fabric-program

Det här exempelskriptet uppgraderar en aktiv Service Fabric application instans tooversion 1.3.0. kopierar hello nya program paketet toohello klustret avbildningsarkivet hello skript registrerar hello programtyp, startar en övervakade uppgradering och kontrollerar hello uppgraderingsstatus kontinuerligt tills hello uppgraderingen har slutförts eller återställs. Anpassa hello parametrarna efter behov. 

Om det behövs installerar du hello Service Fabric PowerShell-modulen med hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Hämtar alla hello program i hello Service Fabric-kluster eller ett visst program.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Hämtar hello status för en uppgradering av Service Fabric-programmet. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Hämtar hello Service Fabric application typer har registrerats på hello Service Fabric-klustret. |
| [Avregistrera ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Avregistrerar en Service Fabric-programtyp.  |
| [Kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopierar en Service Fabric application package toohello avbildning arkivet.  |
| [Registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Registrerar en Service Fabric-programtyp. |
| [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Uppgraderar en version Service Fabric-program för toohello angivet program. |


## <a name="next-steps"></a>Nästa steg

Mer information om hello Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).

Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).
