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
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a><span data-ttu-id="37bfd-103">Distribuera ett program tooa Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="37bfd-103">Deploy an application tooa Service Fabric cluster</span></span>

<span data-ttu-id="37bfd-104">Det här exempelskriptet kopierar ett paket tooa klustret avbildningen programarkiv registrerar hello programtyp i hello kluster och skapar en instans av programmet från hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="37bfd-104">This sample script copies an application package tooa cluster image store, registers hello application type in hello cluster, and creates an application instance from hello application type.</span></span> <span data-ttu-id="37bfd-105">Alla standardtjänster skapas också just nu.</span><span class="sxs-lookup"><span data-stu-id="37bfd-105">Any default services are also created at this time.</span></span>

<span data-ttu-id="37bfd-106">Om det behövs installerar du hello [Service Fabric CLI](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="37bfd-106">If needed, install hello [Service Fabric CLI](../service-fabric-cli.md).</span></span>

## <a name="sample-script"></a><span data-ttu-id="37bfd-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="37bfd-107">Sample script</span></span>

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="37bfd-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="37bfd-108">Clean up deployment</span></span>

<span data-ttu-id="37bfd-109">När du är klar hello [ta bort](cli-remove-application.md) skript kan vara används tooremove hello program.</span><span class="sxs-lookup"><span data-stu-id="37bfd-109">When done, hello [remove](cli-remove-application.md) script can be used tooremove hello application.</span></span> <span data-ttu-id="37bfd-110">hello ta bort skriptet tar bort hello programinstansen Avregistrerar hello programtyp och tar bort hello programpaketet från avbildningsarkivet.</span><span class="sxs-lookup"><span data-stu-id="37bfd-110">hello remove script deletes hello application instance, unregisters hello application type, and deletes hello application package from the image store.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37bfd-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37bfd-111">Next steps</span></span>

<span data-ttu-id="37bfd-112">Mer information finns i hello [Service Fabric CLI dokumentationen](../service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="37bfd-112">For more information, see hello [Service Fabric CLI documentation](../service-fabric-cli.md).</span></span>

<span data-ttu-id="37bfd-113">Ytterligare Service Fabric CLI prover för Azure Service Fabric kan hittas i hello [Service Fabric CLI exempel](../samples-cli.md).</span><span class="sxs-lookup"><span data-stu-id="37bfd-113">Additional Service Fabric CLI samples for Azure Service Fabric can be found in hello [Service Fabric CLI samples](../samples-cli.md).</span></span>
