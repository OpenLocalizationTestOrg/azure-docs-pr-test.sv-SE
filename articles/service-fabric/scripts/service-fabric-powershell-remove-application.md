---
title: "aaaAzure skriptexempel PowerShell - ta bort programmet från ett kluster | Microsoft Docs"
description: "Azure PowerShell skriptexempel - ta bort ett program från ett Service Fabric-kluster."
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
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 3fe2082c2fbeffbff1622f206021d4d907197d19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Ta bort ett program från ett Service Fabric-kluster

Det här exempelskriptet tar bort en instans som körs Service Fabric-program, Avregistrerar en programtypen och versionen från hello klustret och tar bort hello programpaket från avbildningsarkivet för hello klustret.  Ta bort hello programinstansen tas även bort alla hello kör instanser av tjänsten som är associerade med programmet. Anpassa hello parametrarna efter behov. 

Om det behövs installerar du hello Service Fabric PowerShell-modulen med hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ta bort ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | Tar bort en instans som körs Service Fabric-program från hello kluster.  |
| [Avregistrera ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Avregistrerar en Service Fabric-programtypen och versionen från hello kluster. |
| [Ta bort ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Tar bort ett Service Fabric-programpaket från avbildningsarkivet hello.|

## <a name="next-steps"></a>Nästa steg

Mer information om hello Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).

Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).
