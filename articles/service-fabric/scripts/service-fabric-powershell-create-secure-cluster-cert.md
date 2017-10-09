---
title: aaaAzure PowerShell skriptexempel - skapa ett Service Fabric-kluster | Microsoft Docs
description: "Azure PowerShell-skript exempel – skapa ett Service Fabric-kluster."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 12fdc201bd51688cb850cd456b1e00442b79c22d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster"></a>Skapa ett Service Fabric-kluster

Det här exempelskriptet skapar Service Fabric-kluster ett kluster med fem noder skyddas med ett X.509-certifikat.  hello-kommando skapar ett självsignerat certifikat och överförs tooa nya nyckelvalvet. hello certifikat är också kopierade tooa lokal katalog.  Ange hello *-OS* parametern toochoose hello version av Windows eller Linux som körs på hello klusternoder.  Anpassa hello parametrarna efter behov.

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview) och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure. 

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgrupp, kluster och alla relaterade resurser.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Skapar ett nytt Service Fabric-kluster. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare Azure Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).
