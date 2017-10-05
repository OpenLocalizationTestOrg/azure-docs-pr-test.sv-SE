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
# <a name="remove-an-application-from-a-service-fabric-cluster"></a><span data-ttu-id="473a3-103">Ta bort ett program från ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="473a3-103">Remove an application from a Service Fabric cluster</span></span>

<span data-ttu-id="473a3-104">Det här exempelskriptet tar bort en instans som körs Service Fabric-program, Avregistrerar en programtypen och versionen från klustret.</span><span class="sxs-lookup"><span data-stu-id="473a3-104">This sample script deletes a running Service Fabric application instance, unregisters an application type and version from the cluster.</span></span>  <span data-ttu-id="473a3-105">Tar bort programinstansen också att tas bort alla körs tjänsten instanser som är associerade med programmet.</span><span class="sxs-lookup"><span data-stu-id="473a3-105">Deleting the application instance also deletes all the running service instances associated with that application.</span></span> <span data-ttu-id="473a3-106">Därefter tas programmets filer bort från avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="473a3-106">Next, the application files are deleted from the image store.</span></span> 

<span data-ttu-id="473a3-107">Om det behövs installerar den [Service Fabric CLI](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="473a3-107">If needed, install the [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="473a3-108">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="473a3-108">Sample script</span></span>

<span data-ttu-id="473a3-109">[!code-sh[huvudsakliga](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "ta bort ett program från ett kluster")]</span><span class="sxs-lookup"><span data-stu-id="473a3-109">[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]</span></span>

## <a name="next-steps"></a><span data-ttu-id="473a3-110">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="473a3-110">Next steps</span></span>

<span data-ttu-id="473a3-111">Mer information finns i [Service Fabric CLI dokumentationen](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="473a3-111">For more information, see the [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="473a3-112">Ytterligare Service Fabric CLI prover för Azure Service Fabric kan hittas i den [Service Fabric CLI exempel](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="473a3-112">Additional Service Fabric CLI samples for Azure Service Fabric can be found in the [Service Fabric CLI samples](../samples-cli.md).</span></span>
