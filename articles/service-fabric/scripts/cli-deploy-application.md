---
title: Azure Service Fabric CLI skript distribuerar exempel
description: "Distribuera ett program till ett Azure Service Fabric-kluster med hjälp av Azure Service Fabric-CLI"
services: service-fabric
documentationcenter: 
author: Thraka
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
ms.openlocfilehash: c77ecfccdf7d3e34b0b3133e9c63810a04fb1132
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Distribuera ett program till ett Service Fabric-kluster

Det här exempelskriptet kopierar ett programpaket till ett kluster avbildningsarkivet registrerar programtypen i klustret och skapar en instans av programmet från programtypen. Alla standardtjänster skapas också just nu.

Om det behövs installerar den [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Exempelskript

[!code-sh[huvudsakliga](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "distribuerar ett program till ett kluster")]

## <a name="clean-up-deployment"></a>Rensa distribution

När du är klar i [ta bort](cli-remove-application.md) skript som kan användas för att ta bort programmet. Ta bort skriptet tar bort programinstansen Avregistrerar programtypen och tar bort programpaketet från avbildningsarkivet.

## <a name="next-steps"></a>Nästa steg

Mer information finns i [Service Fabric CLI dokumentationen](../service-fabric-cli.md).

Ytterligare Service Fabric CLI prover för Azure Service Fabric kan hittas i den [Service Fabric CLI exempel](../samples-cli.md).
