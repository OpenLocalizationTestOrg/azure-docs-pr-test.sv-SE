---
title: aaaConfigure hello uppgradering av ett Service Fabric-program | Microsoft Docs
description: "Lär dig hur tooconfigure hello inställningar för att uppgradera ett Service Fabric-program med hjälp av Microsoft Visual Studio."
services: service-fabric
documentationcenter: na
author: mikkelhegn
manager: mfussell
editor: tglee
ms.assetid: 1757ba85-0b7b-4f16-8a23-2ddaa61c86c6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/29/2017
ms.author: mikkelhegn
ms.openlocfilehash: 8ca50aa9d911f3c98f017490c8fe29011e8d80cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-upgrade-of-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="c9138-103">Konfigurera hello uppgradering av ett Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9138-103">Configure hello upgrade of a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="c9138-104">Visual Studio tools för Azure Service Fabric stödja uppgradera publicerar toolocal eller fjärr-kluster.</span><span class="sxs-lookup"><span data-stu-id="c9138-104">Visual Studio tools for Azure Service Fabric provide upgrade support for publishing toolocal or remote clusters.</span></span> <span data-ttu-id="c9138-105">Det finns tre scenarier där du vill tooupgrade dina program tooa nyare version i stället för att ersätta hello program under testning och felsökning:</span><span class="sxs-lookup"><span data-stu-id="c9138-105">There are three scenarios in which you want tooupgrade your application tooa newer version instead of replacing hello application during testing and debugging:</span></span>

* <span data-ttu-id="c9138-106">Programdata går inte förlorade under hello uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="c9138-106">Application data won't be lost during hello upgrade.</span></span>
* <span data-ttu-id="c9138-107">Tillgänglighet förblir hög så att det inte går eventuella avbrott i tjänsten under hello uppgraderingen, om det finns tillräckligt med instanser av tjänsten sprids uppgraderingsdomäner.</span><span class="sxs-lookup"><span data-stu-id="c9138-107">Availability remains high so there won't be any service interruption during hello upgrade, if there are enough service instances spread across upgrade domains.</span></span>
* <span data-ttu-id="c9138-108">Du kan köra testerna mot ett program medan den uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="c9138-108">Tests can be run against an application while it's being upgraded.</span></span>

## <a name="parameters-needed-tooupgrade"></a><span data-ttu-id="c9138-109">Parametrar krävs tooupgrade</span><span class="sxs-lookup"><span data-stu-id="c9138-109">Parameters needed tooupgrade</span></span>
<span data-ttu-id="c9138-110">Du kan välja mellan två typer av distribution: regelbundna eller uppgradering.</span><span class="sxs-lookup"><span data-stu-id="c9138-110">You can choose from two types of deployment: regular or upgrade.</span></span> <span data-ttu-id="c9138-111">En vanlig distribution raderas alla tidigare distributionsinformation och data på hello kluster, medan en uppgradering distribution bevarar den.</span><span class="sxs-lookup"><span data-stu-id="c9138-111">A regular deployment erases any previous deployment information and data on hello cluster, while an upgrade deployment preserves it.</span></span> <span data-ttu-id="c9138-112">När du uppgraderar ett Service Fabric-program i Visual Studio, behöver du uppgradera parametrar för tooprovide program och kontrollera hälsoprinciper.</span><span class="sxs-lookup"><span data-stu-id="c9138-112">When you upgrade a Service Fabric application in Visual Studio, you need tooprovide application upgrade parameters and health check policies.</span></span> <span data-ttu-id="c9138-113">Uppgradering av programmet parametrar för att styra hello uppgradering vid kontroll av hälsoprinciper fastställa om hello uppgraderingen lyckades.</span><span class="sxs-lookup"><span data-stu-id="c9138-113">Application upgrade parameters help control hello upgrade, while health check policies determine whether hello upgrade was successful.</span></span> <span data-ttu-id="c9138-114">Se [uppgradering av Service Fabric-programmet: Uppgraderingsparametrar](service-fabric-application-upgrade-parameters.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c9138-114">See [Service Fabric application upgrade: upgrade parameters](service-fabric-application-upgrade-parameters.md) for more details.</span></span>

<span data-ttu-id="c9138-115">Det finns tre uppgradera lägen: *övervakade*, *UnmonitoredAuto*, och *UnmonitoredManual*.</span><span class="sxs-lookup"><span data-stu-id="c9138-115">There are three upgrade modes: *Monitored*, *UnmonitoredAuto*, and *UnmonitoredManual*.</span></span>

* <span data-ttu-id="c9138-116">En uppgradering av övervakade automatiserar hello uppgradering och programmet hälsokontroll.</span><span class="sxs-lookup"><span data-stu-id="c9138-116">A Monitored upgrade automates hello upgrade and application health check.</span></span>
* <span data-ttu-id="c9138-117">Uppgradering UnmonitoredAuto automatiserar hello uppgraderingen, men hoppar över hello hälsokontrollen för programmet.</span><span class="sxs-lookup"><span data-stu-id="c9138-117">An UnmonitoredAuto upgrade automates hello upgrade, but skips hello application health check.</span></span>
* <span data-ttu-id="c9138-118">När du gör en uppgradering UnmonitoredManual toomanually måste uppgradera varje domän.</span><span class="sxs-lookup"><span data-stu-id="c9138-118">When you do an UnmonitoredManual upgrade, you need toomanually upgrade each upgrade domain.</span></span>

<span data-ttu-id="c9138-119">Varje Uppgraderingsläge kräver olika uppsättningar med parametrar.</span><span class="sxs-lookup"><span data-stu-id="c9138-119">Each upgrade mode requires different sets of parameters.</span></span> <span data-ttu-id="c9138-120">Se [uppgradera applikationsparametrarna](service-fabric-application-upgrade-parameters.md) toolearn mer om hello tillgängliga alternativ för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="c9138-120">See [Application upgrade parameters](service-fabric-application-upgrade-parameters.md) toolearn more about hello available upgrade options.</span></span>

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a><span data-ttu-id="c9138-121">Uppgradera ett Service Fabric-program i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9138-121">Upgrade a Service Fabric application in Visual Studio</span></span>
<span data-ttu-id="c9138-122">Om du använder hello Visual Studio Service Fabric verktyg tooupgrade ett Service Fabric-program, kan du ange en publicera processen toobe en uppgradering i stället för en vanlig distribution genom att kontrollera hello **uppgradera hello program** Kontrollera ruta.</span><span class="sxs-lookup"><span data-stu-id="c9138-122">If you’re using hello Visual Studio Service Fabric tools tooupgrade a Service Fabric application, you can specify a publish process toobe an upgrade rather than a regular deployment by checking hello **Upgrade hello application** check box.</span></span>

### <a name="tooconfigure-hello-upgrade-parameters"></a><span data-ttu-id="c9138-123">uppgradera tooconfigure hello-parametrar</span><span class="sxs-lookup"><span data-stu-id="c9138-123">tooconfigure hello upgrade parameters</span></span>
1. <span data-ttu-id="c9138-124">Klicka på hello **inställningar** knappen Nästa toohello kryssruta.</span><span class="sxs-lookup"><span data-stu-id="c9138-124">Click hello **Settings** button next toohello check box.</span></span> <span data-ttu-id="c9138-125">Hej **redigera uppgradera parametrar** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="c9138-125">hello **Edit Upgrade Parameters** dialog box appears.</span></span> <span data-ttu-id="c9138-126">Hej **redigera uppgradera parametrar** dialogrutan stöder hello övervakade och UnmonitoredAuto UnmonitoredManual uppgradera lägen.</span><span class="sxs-lookup"><span data-stu-id="c9138-126">hello **Edit Upgrade Parameters** dialog box supports hello Monitored, UnmonitoredAuto, and UnmonitoredManual upgrade modes.</span></span>
2. <span data-ttu-id="c9138-127">Välj hello Uppgraderingsläge som du vill använda toouse och fyller sedan hello parametern rutnätet.</span><span class="sxs-lookup"><span data-stu-id="c9138-127">Select hello upgrade mode that you want toouse and then fill out hello parameter grid.</span></span>

    <span data-ttu-id="c9138-128">Varje parameter har standardvärden.</span><span class="sxs-lookup"><span data-stu-id="c9138-128">Each parameter has default values.</span></span> <span data-ttu-id="c9138-129">Hej valfri parameter *DefaultServiceTypeHealthPolicy* tar en hash-tabell inmatning.</span><span class="sxs-lookup"><span data-stu-id="c9138-129">hello optional parameter *DefaultServiceTypeHealthPolicy* takes a hash table input.</span></span> <span data-ttu-id="c9138-130">Här är ett exempel på hello hash-tabell Indataformatet för *DefaultServiceTypeHealthPolicy*:</span><span class="sxs-lookup"><span data-stu-id="c9138-130">Here’s an example of hello hash table input format for *DefaultServiceTypeHealthPolicy*:</span></span>

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    <span data-ttu-id="c9138-131">*ServiceTypeHealthPolicyMap* är en annan valfri parameter som använder en hash-tabell inmatning hello följande format:</span><span class="sxs-lookup"><span data-stu-id="c9138-131">*ServiceTypeHealthPolicyMap* is another optional parameter that takes a hash table input in hello following format:</span></span>

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    <span data-ttu-id="c9138-132">Här är en verklig exempel:</span><span class="sxs-lookup"><span data-stu-id="c9138-132">Here's a real-life example:</span></span>

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. <span data-ttu-id="c9138-133">Om du väljer UnmonitoredManual Uppgraderingsläge måste du manuellt starta toocontinue en PowerShell-konsolen och slutför hello uppgraderingsprocessen.</span><span class="sxs-lookup"><span data-stu-id="c9138-133">If you select UnmonitoredManual upgrade mode, you must manually start a PowerShell console toocontinue and finish hello upgrade process.</span></span> <span data-ttu-id="c9138-134">Se för[uppgradering av Service Fabric-programmet: avancerade alternativ](service-fabric-application-upgrade-advanced.md) toolearn hur manuell uppgradering fungerar.</span><span class="sxs-lookup"><span data-stu-id="c9138-134">Refer too[Service Fabric application upgrade: advanced topics](service-fabric-application-upgrade-advanced.md) toolearn how manual upgrade works.</span></span>

## <a name="upgrade-an-application-by-using-powershell"></a><span data-ttu-id="c9138-135">Uppgradera ett program med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="c9138-135">Upgrade an application by using PowerShell</span></span>
<span data-ttu-id="c9138-136">Du kan använda PowerShell-cmdlets tooupgrade ett Service Fabric-program.</span><span class="sxs-lookup"><span data-stu-id="c9138-136">You can use PowerShell cmdlets tooupgrade a Service Fabric application.</span></span> <span data-ttu-id="c9138-137">Se [Service Fabric uppgradera självstudien](service-fabric-application-upgrade-tutorial.md) och [Start ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="c9138-137">See [Service Fabric application upgrade tutorial](service-fabric-application-upgrade-tutorial.md) and [Start-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) for detailed information.</span></span>

## <a name="specify-a-health-check-policy-in-hello-application-manifest-file"></a><span data-ttu-id="c9138-138">Ange en hälsoprincip för kontrollen i hello programmanifestfilen</span><span class="sxs-lookup"><span data-stu-id="c9138-138">Specify a health check policy in hello application manifest file</span></span>
<span data-ttu-id="c9138-139">Varje tjänst i ett Service Fabric-program kan ha sin egen parametrar för hälsotillstånd som åsidosätter hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="c9138-139">Every service in a Service Fabric application can have its own health policy parameters that override hello default values.</span></span> <span data-ttu-id="c9138-140">Du kan ange dessa parametervärden i hello programmanifestfilen.</span><span class="sxs-lookup"><span data-stu-id="c9138-140">You can provide these parameter values in hello application manifest file.</span></span>

<span data-ttu-id="c9138-141">hello följande exempel visas hur tooapply unikt hälsotillstånd Kontrollera princip för varje tjänst i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="c9138-141">hello following example shows how tooapply a unique health check policy for each service in hello application manifest.</span></span>

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a><span data-ttu-id="c9138-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9138-142">Next steps</span></span>
<span data-ttu-id="c9138-143">Mer information om hur du distribuerar ett program finns [distribuera ett befintligt program i Azure Service Fabric](service-fabric-deploy-existing-app.md).</span><span class="sxs-lookup"><span data-stu-id="c9138-143">For more information about deploying an application, see [Deploy an existing application in Azure Service Fabric](service-fabric-deploy-existing-app.md).</span></span>