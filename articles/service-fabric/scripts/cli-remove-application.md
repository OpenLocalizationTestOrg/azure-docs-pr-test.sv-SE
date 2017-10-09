---
title: aaaAzure Service Fabric CLI ta bort skriptexempel
description: "Ta bort ett program från en Azure Service Fabric-kluster med hjälp av hello Azure Service Fabric CLI"
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
ms.openlocfilehash: 3ccefd4a04c5b7af71a2f959e11da6e402f25881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Ta bort ett program från ett Service Fabric-kluster

Det här exempelskriptet tar bort en instans som körs Service Fabric-program, Avregistrerar en programtypen och versionen från hello kluster.  Ta bort hello programinstansen tas även bort alla hello kör instanser av tjänsten som är associerade med programmet. Därefter tas hello-filer bort från avbildningsarkivet hello. 

Om det behövs installerar du hello [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Exempelskript

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Nästa steg

Mer information finns i hello [Service Fabric CLI dokumentationen](../service-fabric-cli.md).

Ytterligare Service Fabric CLI prover för Azure Service Fabric kan hittas i hello [Service Fabric CLI exempel](../samples-cli.md).
