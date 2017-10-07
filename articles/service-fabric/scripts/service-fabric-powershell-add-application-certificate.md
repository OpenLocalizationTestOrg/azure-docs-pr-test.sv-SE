---
title: "aaaAzure PowerShell skriptexempel - lägga till programmet cert tooa klustret | Microsoft Docs"
description: "Azure PowerShell-skript exempel – Lägg till ett program certifikat tooa Service Fabric-kluster."
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
ms.openlocfilehash: aba62598e2e4775012f89b5070bef5e61aec64f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-an-application-certificate-tooa-service-fabric-cluster"></a>Lägg till ett program certifikat tooa Service Fabric-kluster

Det här exempelskriptet skapar ett självsignerat certifikat i hello angivna Azure key vault och installerar den tooall noder i hello Service Fabric-klustret. hello certifikat hämtas även tooa lokal mapp. hello namnet på hello hämtat certifikat är hello samma som hello namnet på hello certifikat i hello nyckelvalvet. Anpassa hello parametrarna efter behov.

Om det behövs installerar du Azure PowerShell med hjälp av hello-instruktion finns i hello hello [Azure PowerShell guiden](/powershell/azure/overview) och kör sedan `Login-AzureRmAccount` toocreate en anslutning till Azure. 

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate tooa cluster")]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon: varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Lägg till AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Lägga till en ny virtuell dator skala program certifikat toohello som utgör hello klustret.  |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare Azure Powershell-exempel för Azure Service Fabric kan hittas i hello [Azure PowerShell-exempel](../service-fabric-powershell-samples.md).
