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
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a><span data-ttu-id="b89e3-103">Distribuera ett program till ett Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="b89e3-103">Deploy an application to a Service Fabric cluster</span></span>

<span data-ttu-id="b89e3-104">Det här exempelskriptet kopierar ett programpaket till ett kluster avbildningsarkivet registrerar programtypen i klustret och skapar en instans av programmet från programtypen.</span><span class="sxs-lookup"><span data-stu-id="b89e3-104">This sample script copies an application package to a cluster image store, registers the application type in the cluster, and creates an application instance from the application type.</span></span> <span data-ttu-id="b89e3-105">Alla standardtjänster skapas också just nu.</span><span class="sxs-lookup"><span data-stu-id="b89e3-105">Any default services are also created at this time.</span></span>

<span data-ttu-id="b89e3-106">Om det behövs installerar den [Service Fabric CLI](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b89e3-106">If needed, install the [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="b89e3-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="b89e3-107">Sample script</span></span>

<span data-ttu-id="b89e3-108">[!code-sh[huvudsakliga](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "distribuerar ett program till ett kluster")]</span><span class="sxs-lookup"><span data-stu-id="b89e3-108">[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="b89e3-109">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="b89e3-109">Clean up deployment</span></span>

<span data-ttu-id="b89e3-110">När du är klar i [ta bort](cli-remove-application.md) skript som kan användas för att ta bort programmet.</span><span class="sxs-lookup"><span data-stu-id="b89e3-110">When done, the [remove](cli-remove-application.md) script can be used to remove the application.</span></span> <span data-ttu-id="b89e3-111">Ta bort skriptet tar bort programinstansen Avregistrerar programtypen och tar bort programpaketet från avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="b89e3-111">The remove script deletes the application instance, unregisters the application type, and deletes the application package from the image store.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b89e3-112">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b89e3-112">Next steps</span></span>

<span data-ttu-id="b89e3-113">Mer information finns i [Service Fabric CLI dokumentationen](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b89e3-113">For more information, see the [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="b89e3-114">Ytterligare Service Fabric CLI prover för Azure Service Fabric kan hittas i den [Service Fabric CLI exempel](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b89e3-114">Additional Service Fabric CLI samples for Azure Service Fabric can be found in the [Service Fabric CLI samples](../samples-cli.md).</span></span>
