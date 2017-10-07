---
title: aaaAzure PowerShell skriptexempel - distribuera programmet tooa kluster | Microsoft Docs
description: "Azure PowerShell-skript exempel – distribuera programmet tooa Service Fabric-klustret."
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
ms.openlocfilehash: b417c9908c72f016e930c43ff2d13e0cc5451f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a>Distribuera ett program tooa Service Fabric-kluster

Det här exempelskriptet kopierar ett paket tooa klustret avbildningen programarkiv registrerar hello programtyp i hello kluster och skapar en instans av programmet från hello programtyp.  Om alla standardtjänster definierades i hello programmanifestet av hello Målprogramstyp skapas tjänsterna just nu. Anpassa hello parametrarna efter behov. 

Om det behövs installerar du hello Service Fabric PowerShell-modulen med hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Efter hello skriptexempel har körts hello skriptet i [tar bort ett program](service-fabric-powershell-remove-application.md) kan använda tooremove hello programinstansen, avregistrering av programtyp hello och ta bort hello programpaketet från avbildningsarkivet hello.

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Kopiera ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopiera ett paket toohello klustret avbildningen programarkiv.  |
|[Registrera ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Registrerar ett programtypen och versionen på hello klustret. |
|[Ny ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Skapar ett program från en typ som registrerade programmet. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Service Fabric PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/service-fabric/?view=azureservicefabricps).

Ytterligare Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).
