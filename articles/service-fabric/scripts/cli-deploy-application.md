---
title: aaaAzure Service Fabric CLI distribuera skriptexempel
description: "Distribuera ett program tooan Azure Service Fabric-kluster med hjälp av hello Azure Service Fabric CLI"
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
ms.openlocfilehash: aaec7042a4fd7ed32ad706cde70361f23d18fb48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a>Distribuera ett program tooa Service Fabric-kluster

Det här exempelskriptet kopierar ett paket tooa klustret avbildningen programarkiv registrerar hello programtyp i hello kluster och skapar en instans av programmet från hello programtyp. Alla standardtjänster skapas också just nu.

Om det behövs installerar du hello [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Exempelskript

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a>Rensa distribution

När du är klar hello [ta bort](cli-remove-application.md) skript kan vara används tooremove hello program. hello ta bort skriptet tar bort hello programinstansen Avregistrerar hello programtyp och tar bort hello programpaketet från avbildningsarkivet.

## <a name="next-steps"></a>Nästa steg

Mer information finns i hello [Service Fabric CLI dokumentationen](../service-fabric-cli.md).

Ytterligare Service Fabric CLI prover för Azure Service Fabric kan hittas i hello [Service Fabric CLI exempel](../samples-cli.md).
