---
title: "Ta bort för Azure Service Fabric CLI skriptexempel"
description: "Ta bort ett program från ett Azure Service Fabric-kluster med hjälp av Azure Service Fabric-CLI"
services: service-fabric
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: adegeo
ms.custom: mvc
ms.openlocfilehash: d86f195d2c37a71e476c5ba4eec040dd46931d23
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Ta bort ett program från ett Service Fabric-kluster

Det här exempelskriptet tar bort en instans som körs Service Fabric-program, Avregistrerar en programtypen och versionen från klustret.  Tar bort programinstansen också att tas bort alla körs tjänsten instanser som är associerade med programmet. Därefter tas programmets filer bort från avbildningsarkivet. 

Om det behövs installerar den [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Exempelskript

[!code-sh[huvudsakliga](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "ta bort ett program från ett kluster")]

## <a name="next-steps"></a>Nästa steg

Mer information finns i [Service Fabric CLI dokumentationen](../service-fabric-cli.md).

Ytterligare Service Fabric CLI prover för Azure Service Fabric kan hittas i den [Service Fabric CLI exempel](../samples-cli.md).
