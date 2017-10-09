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
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="285d7-103">Ta bort ett program från ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="285d7-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="285d7-104">Det här exempelskriptet tar bort en instans som körs Service Fabric-program, Avregistrerar en programtypen och versionen från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="285d7-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from hello cluster.</span></span>  <span data-ttu-id="285d7-105">Ta bort hello programinstansen tas även bort alla hello kör instanser av tjänsten som är associerade med programmet.</span><span class="sxs-lookup"><span data-stu-id="285d7-105">Deleting hello application instance also deletes all hello running service instances associated with that application.</span></span> <span data-ttu-id="285d7-106">Därefter tas hello-filer bort från avbildningsarkivet hello.</span><span class="sxs-lookup"><span data-stu-id="285d7-106">Next, hello application files are deleted from hello image store.</span></span> 

<span data-ttu-id="285d7-107">Om det behövs installerar du hello [Service Fabric CLI](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="285d7-107">If needed, install hello [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="285d7-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="285d7-108">Sample script</span></span>

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a><span data-ttu-id="285d7-109">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="285d7-109">Next steps</span></span>

<span data-ttu-id="285d7-110">Mer information finns i hello [Service Fabric CLI dokumentationen](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="285d7-110">For more information, see hello [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="285d7-111">Ytterligare Service Fabric CLI prover för Azure Service Fabric kan hittas i hello [Service Fabric CLI exempel](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="285d7-111">Additional Service Fabric CLI samples for Azure Service Fabric can be found in hello [Service Fabric CLI samples](../samples-cli.md).</span></span>
