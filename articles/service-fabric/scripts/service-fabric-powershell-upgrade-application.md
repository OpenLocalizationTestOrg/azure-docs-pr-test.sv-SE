---
title: Azure PowerShell skriptexempel - uppgradering ett Service Fabric-program | Microsoft Docs
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
ms.openlocfilehash: 454849f82ddb23ddb9d71459f86e3cf5a1589254
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upgrade-a-service-fabric-application"></a>Uppgradera ett Service Fabric-program

Det här exempelskriptet uppgraderar en instans som körs Service Fabric-program till version 1.3.0. Skriptet kopierar nytt programpaket till klustret image store, registrerar programtypen, startar en övervakade uppgradering och kontrollerar uppgraderingsstatus kontinuerligt tills uppgraderingen har slutförts eller återställs. Anpassa parametrarna efter behov. 

Om det behövs installerar du Service Fabric PowerShell-modulen med den [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Exempelskript

[!code-powershell[huvudsakliga](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "uppgradera ett program")]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Hämtar alla program i Service Fabric-kluster eller ett visst program.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Hämtar status för en uppgradering av Service Fabric-programmet. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Hämtar Service Fabric-programtyper som registrerats för Service Fabric-klustret. |
| [Avregistrera ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Avregistrerar en Service Fabric-programtyp.  |
| [Kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopierar ett Service Fabric-programpaket till image store.  |
| [Registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Registrerar en Service Fabric-programtyp. |
| [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Uppgraderar ett Service Fabric-program till den angivna typ versionen. |


## <a name="next-steps"></a>Nästa steg

Mer information om Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).

Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i den [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).
